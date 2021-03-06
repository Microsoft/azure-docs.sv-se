---
title: Så här genererar och överför du HSM-skyddade nycklar för Azure Key Vault hanterad HSM-Azure Key Vault | Microsoft Docs
description: Använd den här artikeln för att hjälpa dig att planera för, generera och överföra dina egna HSM-skyddade nycklar som ska användas med hanterad HSM. Kallas även för att ta med din egen nyckel (BYOK).
services: key-vault
author: amitbapat
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/04/2021
ms.author: ambapat
ms.openlocfilehash: dd5b38a858ceba12f5d48f1782da5b85228c4b06
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102212118"
---
# <a name="import-hsm-protected-keys-to-managed-hsm-byok"></a>Importera HSM-skyddade nycklar till hanterad HSM (BYOK)

 Azure Key Vault Managed HSM stöder import av nycklar som genererats i din lokala maskin varu säkerhetsmodul (HSM). nycklarna lämnar aldrig HSM-skyddets gränser. Det här scenariot kallas ofta för att *ta med din egen nyckel* (BYOK). Hanterad HSM använder Marvell LiquidSecurity HSM-korten (FIPS 140-2 nivå 3 verifierade) för att skydda dina nycklar.

Använd informationen i den här artikeln när du planerar för, genererar och överför dina egna HSM-skyddade nycklar som ska användas med hanterad HSM.

> [!NOTE]
> Den här funktionen är inte tillgänglig för Azure Kina 21Vianet. Den här import metoden är endast tillgänglig för [HSM: er som stöds](#supported-hsms). 

Mer information och en själv studie kurs om hur du kommer igång med hanterad HSM finns i [Vad är hanterad HSM?](overview.md).

## <a name="overview"></a>Översikt

Här är en översikt över processen. De åtgärder som ska utföras beskrivs senare i artikeln.

* I hanterad HSM genererar du en nyckel (kallas *nyckel utbytes nyckel* (KEK)). KEK måste vara en RSA-HSM-nyckel som bara har `import` nyckel åtgärden. 
* Hämta den offentliga nyckeln KEK som en. PEM-fil.
* Överför den offentliga KEK-nyckeln till en frånkopplad dator som är ansluten till en lokal HSM.
* I den frånkopplade datorn använder du verktyget BYOK som tillhandahålls av din HSM-leverantör för att skapa en BYOK-fil. 
* Mål nyckeln är krypterad med en KEK, som förblir krypterad tills den överförs till den hanterade HSM: en. Endast den krypterade versionen av nyckeln lämnar den lokala HSM.
* Det går inte att exportera en KEK som har genererats i en hanterad HSM. HSM: er tillämpar regeln att ingen tydlig version av en KEK finns utanför en hanterad HSM.
* KEK måste finnas i samma hanterade HSM där mål nyckeln ska importeras.
* När BYOK-filen överförs till hanterad HSM använder en hanterad HSM den privata KEK-nyckeln för att dekryptera mål nyckel materialet och importera det som en HSM-nyckel. Den här åtgärden sker helt i HSM. Mål nyckeln finns alltid kvar i HSM-skyddets gränser.


## <a name="prerequisites"></a>Förutsättningar

Om du vill använda Azure CLI-kommandona i den här artikeln måste du ha följande objekt:

* En prenumeration på Microsoft Azure. Om du inte har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).
* Azure CLI-versionen 2.12.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI]( /cli/azure/install-azure-cli).
* En hanterad HSM som [stöds av HSM: er-listan](#supported-hsms) i din prenumeration. Se [snabb start: etablera och aktivera en hanterad HSM med Azure CLI](quick-create-cli.md) för att etablera och aktivera en hanterad HSM.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Du kan logga in i Azure via CLI genom att skriva:

```azurecli
az login
```

Mer information om inloggningsalternativen via CLI finns i [Logga in med Azure CLI](/cli/azure/authenticate-azure-cli)

## <a name="supported-hsms"></a>HSM: er som stöds

|Leverantörs namn|Typ av leverantör|HSM-modeller som stöds|Mer information|
|---|---|---|---|
|Hjälp programmet nCipher|Tillverkare<br/>HSM som en tjänst|<ul><li>nshield maskinvarusäkerhetsmodul-serien HSM: er</li><li>nshield maskinvarusäkerhetsmodul som en tjänst</ul>|[Hjälp programmet nCipher nytt BYOK-verktyg och dokumentation](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|
|Thales|Tillverkare|<ul><li>Luna HSM 7-serien med inbyggd program vara version 7,3 eller senare</li></ul>| [Luna BYOK-verktyg och dokumentation](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|
|Fortanix|Tillverkare<br/>HSM som en tjänst|<ul><li>Self-Defending nyckel hanterings tjänst (SDKMS)</li><li>Equinix SmartKey</li></ul>|[Exportera SDKMS-nycklar till moln leverantörer för BYOK-Azure Key Vault](https://support.fortanix.com/hc/en-us/articles/360040071192-Exporting-SDKMS-keys-to-Cloud-Providers-for-BYOK-Azure-Key-Vault)|
|Marvell|Tillverkare|Alla LiquidSecurity-HSM: er med<ul><li>Version 2.0.4 eller senare av inbyggd program vara</li><li>Inbyggd program vara version 3,2 eller senare</li></ul>|[Marvell BYOK-verktyg och dokumentation](https://www.marvell.com/products/security-solutions/nitrox-hs-adapters/exporting-marvell-hsm-keys-to-cloud-azure-key-vault.html)|
|Cryptomathic|ISV (Enterprise Key Management System)|Flera HSM-varumärken och-modeller, inklusive<ul><li>Hjälp programmet nCipher</li><li>Thales</li><li>Utimaco</li></ul>Mer [information finns på Cryptomathic-webbplatsen](https://www.cryptomathic.com/azurebyok)|[Cryptomathic BYOK-verktyg och dokumentation](https://www.cryptomathic.com/azurebyok)|
|Securosys SA|Tillverkare, HSM som en tjänst|Primus HSM-familjen, Securosys-moln HSM|[Primus BYOK-verktyg och dokumentation](https://www.securosys.com/primus-azure-byok)|
|StorMagic|ISV (Enterprise Key Management System)|Flera HSM-varumärken och-modeller, inklusive<ul><li>Utimaco</li><li>Thales</li><li>Hjälp programmet nCipher</li></ul>Mer [information finns på StorMagic-webbplatsen](https://stormagic.com/doc/svkms/Content/Integrations/Azure_KeyVault_BYOK.htm)|[SvKMS och Azure Key Vault BYOK](https://stormagic.com/doc/svkms/Content/Integrations/Azure_KeyVault_BYOK.htm)|
|IBM|Tillverkare|IBM 476x, CryptoExpress|[IBM Enterprise Key Management Foundation](https://www.ibm.com/security/key-management/ekmf-bring-your-own-key-azure)|
|Utimaco|Tillverkare<br/>HSM som en tjänst|u. förtroende ankare, CryptoServer|[Utimaco BYOK-verktyg och integrerings guide](https://support.hsm.utimaco.com/support/downloads/byok)|
||||


## <a name="supported-key-types"></a>Nyckeltyper som stöds

|Nyckelnamn|Nyckeltyp|Nyckel storlek/kurva|Ursprung|Description|
|---|---|---|---|---|
|Nyckel utbytes nyckel (KEK)|RSA| 2 048-bitars<br />3 072-bitars<br />4 096-bitars|Managed HSM|Ett HSM-backat RSA-nyckelpar genererat i hanterad HSM|
|Mål nyckel|
||RSA|2 048-bitars<br />3 072-bitars<br />4 096-bitars|Vendor HSM|Nyckeln som ska överföras till hanterad HSM|
||EC|P-256<br />P-384<br />P-521|Vendor HSM|Nyckeln som ska överföras till hanterad HSM|
||Symmetrisk nyckel (okt-HSM)|128-bitars<br />192-bitars<br />256-bitars|Vendor HSM|Nyckeln som ska överföras till hanterad HSM|
||||
## <a name="generate-and-transfer-your-key-to-the-managed-hsm"></a>Generera och överför din nyckel till hanterad HSM

Så här genererar och överför du din nyckel till en hanterad HSM:

* [Steg 1: skapa en KEK](#step-1-generate-a-kek)
* [Steg 2: Ladda ned den offentliga nyckeln KEK](#step-2-download-the-kek-public-key)
* [Steg 3: generera och Förbered din nyckel för överföring](#step-3-generate-and-prepare-your-key-for-transfer)
* [Steg 4: överför din nyckel till hanterad HSM](#step-4-transfer-your-key-to-managed-hsm)

### <a name="step-1-generate-a-kek"></a>Steg 1: skapa en KEK

En KEK är en RSA-nyckel som genereras i en hanterad HSM. KEK används för att kryptera den nyckel som du vill importera ( *mål* nyckeln).

KEK måste vara:
- En RSA-HSM-nyckel (2 048-bitar; 3 072-bit; eller 4 096-bitars)
- Genereras i samma hanterade HSM där du tänker importera mål nyckeln
- Skapat med tillåtna nyckel åtgärder inställt på `import`

> [!NOTE]
> KEK måste ha ' import ' som den enda tillåtna nyckel åtgärden. import är ömsesidigt uteslutande med alla andra nyckel åtgärder.

Använd kommandot [AZ Key Vault Key Create](/cli/azure/keyvault/key#az-keyvault-key-create) för att skapa en KEK som har nyckel åtgärder inställt på `import` . Registrera nyckel identifieraren ( `kid` ) som returneras från följande kommando. (Du kommer att använda `kid` värdet i [steg 3](#step-3-generate-and-prepare-your-key-for-transfer).)

```azurecli-interactive
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --hsm-name ContosoKeyVaultHSM
```
---


### <a name="step-2-download-the-kek-public-key"></a>Steg 2: Ladda ned den offentliga nyckeln KEK

Använd [AZ Key Vault Key Download](/cli/azure/keyvault/key#az-keyvault-key-download) för att hämta den offentliga KEK-nyckeln till en. PEM-fil. Mål nyckeln som du importerar krypteras med hjälp av den offentliga nyckeln KEK.

```azurecli-interactive
az keyvault key download --name KEKforBYOK --hsm-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```
---

Överför filen KEKforBYOK. PublicKey. pem till din frånkopplade dator. Du kommer att behöva den här filen i nästa steg.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>Steg 3: generera och Förbered din nyckel för överföring

Se din HSM-leverantörs dokumentation för att ladda ned och installera BYOK-verktyget. Följ anvisningarna från din HSM-leverantör för att generera en mål nyckel och skapa sedan ett nyckel överförings paket (en BYOK-fil). Verktyget BYOK använder `kid` från [steg 1](#step-1-generate-a-kek) och filen KEKforBYOK. PublicKey. pem som du laddade ned i [steg 2](#step-2-download-the-kek-public-key) för att generera en krypterad mål nyckel i en BYOK-fil.

Överför BYOK-filen till den anslutna datorn.

> [!NOTE] 
> Det finns inte stöd för att importera RSA 1 024-bitars nycklar. Det finns för närvarande inte stöd för att importera en Elliptic Curve-nyckel (EC).
>
> **Känt problem**: det går bara att importera en RSA 4K-mål nyckel från Luna HSM: er med den inbyggda program varan 7.4.0 eller senare.

### <a name="step-4-transfer-your-key-to-managed-hsm"></a>Steg 4: överför din nyckel till hanterad HSM

Slutför nyckel importen genom att överföra nyckel överförings paketet (en BYOK-fil) från den frånkopplade datorn till den Internet-anslutna datorn. Använd kommandot [AZ Key Vault Key import](/cli/azure/keyvault/key#az-keyvault-key-import) för att överföra BYOK-filen till den hanterade HSM: en.

```azurecli-interactive
az keyvault key import --hsm-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Om överföringen lyckas, visar Azure CLI egenskaperna för den importerade nyckeln.

## <a name="next-steps"></a>Nästa steg

Du kan nu använda den här HSM-skyddade nyckeln i din hanterade HSM. Mer information finns i [detta pris-och funktions jämförelse](https://azure.microsoft.com/pricing/details/key-vault/).



