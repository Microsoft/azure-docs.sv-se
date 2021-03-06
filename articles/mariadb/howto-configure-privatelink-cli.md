---
title: Privat länk – Azure CLI – Azure Database for MariaDB
description: Lär dig hur du konfigurerar privat länk för Azure Database for MariaDB från Azure CLI
author: mksuni
ms.author: sumuth
ms.service: mariadb
ms.topic: how-to
ms.date: 01/09/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 55f375c83affea8585ec7ebf881a80315ff7a38c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100361331"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mariadb-using-cli"></a>Skapa och hantera en privat länk för Azure Database for MariaDB med CLI

En privat slut punkt är det grundläggande Bygg blocket för privat länk i Azure. Den gör det möjligt för Azure-resurser, t. ex. Virtual Machines (VM), att kommunicera privat med privata länk resurser. I den här artikeln får du lära dig hur du använder Azure CLI för att skapa en virtuell dator i en Azure-Virtual Network och en Azure Database for MariaDB-server med en privat Azure-slutpunkt.

> [!NOTE]
> Funktionen privat länk är bara tillgänglig för Azure Database for MariaDB servrar i Generell användning eller Minnesoptimerade pris nivåer. Se till att databas servern är på någon av dessa pris nivåer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Förutsättningar

- Du behöver en [Azure Database for MariaDB-Server](quickstart-create-mariadb-server-database-using-azure-cli.md). 

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Den här artikeln kräver version 2.0.28 eller senare av Azure CLI. Om du använder Azure Cloud Shell är den senaste versionen redan installerad.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Innan du kan skapa en resurs måste du skapa en resurs grupp som är värd för Virtual Network. Skapa en resursgrupp med [az group create](/cli/azure/group). I det här exemplet skapas en resurs grupp med namnet *myResourceGroup* på *westeurope* -platsen:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Skapa en Virtual Network med [AZ Network VNet Create](/cli/azure/network/vnet). I det här exemplet skapas en standard Virtual Network med namnet *myVirtualNetwork* med ett undernät med namnet *undernät*:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Inaktivera privata slut punkts principer för undernät 
Azure distribuerar resurser till ett undernät i ett virtuellt nätverk, så du måste skapa eller uppdatera under nätet för att inaktivera [nätverks principer](../private-link/disable-private-endpoint-network-policy.md)för privata slut punkter. Uppdatera en under näts konfiguration med namnet *mitt undernät* med [AZ Network VNet Subnet Update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>Skapa den virtuella datorn 
Skapa en virtuell dator med AZ VM Create. När du uppmanas anger du ett lösen ord som ska användas som inloggnings uppgifter för den virtuella datorn. I det här exemplet skapas en virtuell dator med namnet *myVm*: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```
 Observera den offentliga IP-adressen för den virtuella datorn. Du kommer att använda den här adressen när du ansluter till den virtuella datorn från Internet i nästa steg.

## <a name="create-an-azure-database-for-mariadb-server"></a>Skapa en Azure Database for MariaDB-server 
Skapa en Azure Database for MariaDB med kommandot AZ MariaDB Server Create. Kom ihåg att namnet på din MariaDB-Server måste vara unikt i Azure, så Ersätt plats hållarens värde inom hakparenteser med ditt eget unika värde: 

```azurecli-interactive
# Create a server in the resource group 
az mariadb server create \
--name mydemoserver \
--resource-group myResourcegroup \
--location westeurope \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2
```

> [!NOTE]
> I vissa fall finns Azure Database for MariaDB och VNet-under nätet i olika prenumerationer. I dessa fall måste du se till att följande konfigurationer:
> - Se till att båda prenumerations resurs leverantören för **Microsoft. DBforMariaDB** är registrerad. Mer information hittar du i [Resource Manager-Registration][resource-manager-portal]

## <a name="create-the-private-endpoint"></a>Skapa den privata slut punkten 
Skapa en privat slut punkt för MariaDB-servern i Virtual Network: 

```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id $(az resource show -g myResourcegroup -n mydemoserver --resource-type "Microsoft.DBforMariaDB/servers" --query "id" -o tsv) \    
    --group-id mariadbServer \  
    --connection-name myConnection  
 ```


## <a name="configure-the-private-dns-zone"></a>Konfigurera Privat DNS zon 
Skapa en Privat DNS zon för MariDB-Server domän och skapa en kopplings länk med Virtual Network. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.mariadb.database.azure.com" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.mariadb.database.azure.com"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the Azure database for MariaDB name 
 
 
#Create DNS records 
az network private-dns record-set a create --name mydemoserver --zone-name privatelink.mariadb.database.azure.com --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name mydemoserver --zone-name privatelink.mariadb.database.azure.com --resource-group myResourceGroup -a <Private IP Address>
```

> [!NOTE] 
> FQDN i DNS-inställningen för kunden matchar inte den privata IP-adressen som kon figurer ATS. Du måste konfigurera en DNS-zon för den konfigurerade FQDN: en som visas [här](../dns/dns-operations-recordsets-portal.md).

## <a name="connect-to-a-vm-from-the-internet"></a>Ansluta till en virtuell dator från Internet

Anslut till VM- *myVm* från Internet på följande sätt:

1. I portalens sökfältet anger du *myVm*.

1. Välj knappen **Anslut**. När du har valt knappen **Anslut** öppnas **Anslut till den virtuella datorn**.

1. Välj **Hämta RDP-fil**. Azure skapar en Remote Desktop Protocol-fil (*. RDP*) och laddar ned den till datorn.

1. Öppna den *nedladdade RDP* -filen.

    1. Välj **Anslut** om du uppmanas att göra det.

    1. Ange det användar namn och lösen ord som du angav när du skapade den virtuella datorn.

        > [!NOTE]
        > Du kan behöva välja **fler alternativ**  >  **Använd ett annat konto** för att ange de autentiseringsuppgifter du angav när du skapade den virtuella datorn.

1. Välj **OK**.

1. Du kan få en certifikatvarning under inloggningen. Välj **Ja** eller **Fortsätt** om du får en certifikatvarning.

1. När virtuella datorns skrivbord visas kan du minimera det att gå tillbaka till din lokala dator.  

## <a name="access-the-mariadb-server-privately-from-the-vm"></a>Få åtkomst till MariaDB-servern privat från den virtuella datorn

1. Öppna PowerShell i fjärr skrivbordet för *myVM*.

2. Ange  `nslookup mydemoserver.privatelink.mariadb.database.azure.com`. 

    Du får ett meddelande som liknar detta:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemoserver.privatelink.mariadb.database.azure.com
    Address:  10.1.3.4
    ```

3. Testa anslutningen till den privata länken för MariaDB-servern med valfri tillgänglig klient. I exemplet nedan har jag använt [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) för att utföra åtgärden.

4. I **ny anslutning** anger eller väljer du den här informationen:

    | Inställning | Värde |
    | ------- | ----- |
    | Anslutningsnamn| Välj önskat anslutnings namn.|
    | Värdnamn | Välj *mydemoserver.privatelink.MariaDB.Database.Azure.com* |
    | Användarnamn | Ange användar namn som *username@servername* anges när MariaDB-servern skapas. |
    | Lösenord | Ange ett lösen ord som angavs när MariaDB-servern skapades. |
    ||

5. Välj **Testa anslutning** eller **OK**.

6. Du kan också Bläddra bland databaser från den vänstra menyn och skapa eller fråga efter information från MariaDB-databasen

8. Stäng fjärr skrivbords anslutningen till myVm.

## <a name="clean-up-resources"></a>Rensa resurser 
När de inte längre behövs kan du använda AZ Group Delete för att ta bort resurs gruppen och alla resurser den har: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Nästa steg
Läs mer om [vad som är en privat Azure-slutpunkt](../private-link/private-endpoint-overview.md)

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md
