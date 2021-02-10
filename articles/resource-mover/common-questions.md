---
title: Vanliga frågor om Azure Resource-arbetskraften?
description: Få svar på vanliga frågor om Azure Resource-arbetskraften
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 02/04/2021
ms.author: raynew
ms.openlocfilehash: a75cd3c5dbf205f49aa606bfe96623a61bce39db
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/09/2021
ms.locfileid: "100007065"
---
# <a name="common-questions"></a>Vanliga frågor

I den här artikeln besvaras vanliga frågor om [Azure Resource-arbetskraft](overview.md).


## <a name="moving-across-regions"></a>Flytta över regioner

### <a name="can-i-move-resources-across-any-regions"></a>Kan jag flytta resurser mellan olika regioner?

För närvarande kan du flytta resurser från vilken offentlig käll region som helst till en offentlig mål region, beroende på vilka [resurs typer som är tillgängliga i den regionen](https://azure.microsoft.com/global-infrastructure/services/). Det finns för närvarande inte stöd för att flytta resurser i Azure Government regioner.

### <a name="what-resources-can-i-move-across-regions-using-resource-mover"></a>Vilka resurser kan jag flytta mellan regioner med resurs förflyttning?

Med hjälp av resurs förflyttning kan du för närvarande flytta följande resurser över flera regioner:

- Virtuella Azure-datorer och tillhör ande diskar
- Nätverkskort
- Tillgänglighetsuppsättningar 
- Virtuella Azure-nätverk 
- Offentliga IP-adresser
- Nätverkssäkerhetsgrupper (NSG:er)
- Interna och offentliga belastningsutjämnare 
- Azure SQL-databaser och elastiska pooler

### <a name="can-i-move-disks-across-regions"></a>Kan jag flytta diskar i flera regioner?

Det går inte att välja diskar som resurser som flyttas mellan regioner. Diskar flyttas dock som en del av flyttningen av en virtuell dator.

### <a name="what-does-it-mean-to-move-a-resource-group"></a>Vad innebär det att flytta en resurs grupp?

När en resurs väljs för flytt läggs motsvarande resurs grupp automatiskt till för flytt. Detta krävs eftersom mål resursen måste placeras under en resurs grupp som den var i målet. Du kan välja att anpassa och ange en befintlig resurs grupp när den har lagts till för flytt. Observera att om du flyttar en resurs grupp innebär det **inte** att alla resurser i käll resurs gruppen kommer att flyttas.

### <a name="can-i-move-resources-across-subscriptions-when-i-move-them-across-regions"></a>Kan jag flytta resurser mellan prenumerationer när jag flyttar dem över flera regioner?

Du kan ändra prenumerationen när du har flyttat resurser till mål regionen. [Lär dig mer](../azure-resource-manager/management/move-resource-group-and-subscription.md) om att flytta resurser till en annan prenumeration. 

### <a name="does-azure-resource-move-service-store-customer-data"></a>Flyttar Azure-resursen kund information? 
Nej. Tjänsten för resurs flyttning lagrar inte kund information, den lagrar bara metadatainformation som underlättar spårningen och förloppet för de resurser som valts för flytt, av kunden.


### <a name="where-is-the-metadata-for-moving-across-regions-stored"></a>Var finns metadata för att flytta mellan regioner?

Den lagras i en [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) -databas och i [Azure Blob Storage](../storage/common/storage-service-encryption.md)i en Microsoft-prenumeration. För närvarande lagras metadata i USA, östra 2 och Europa, norra. Vi kommer att utöka denna täckning till andra regioner. Detta begränsar dig inte från att flytta resurser över några offentliga regioner.

### <a name="is-the-collected-metadata-encrypted"></a>Har insamlade metadata krypterats?

Ja, både under överföring och i vila.
- Under överföringen skickas metadata på ett säkert sätt till resurs förflyttnings tjänsten via Internet med hjälp av HTTPS.
- Metadata krypteras i lagring.

### <a name="how-is-managed-identity-used-in-resource-mover"></a>Hur används den hanterade identiteten i resurs förflyttningen?

[Hanterad identitet](../active-directory/managed-identities-azure-resources/overview.md) (tidigare kallad HANTERAD TJÄNSTIDENTITET (MSI)) tillhandahåller Azure-tjänster med en automatiskt hanterad identitet i Azure AD.
- Resurs förflyttningen använder hanterad identitet så att den kan komma åt Azure-prenumerationer för att flytta resurser mellan regioner.
- En flyttnings samling behöver en tilldelad identitet, med åtkomst till den prenumeration som innehåller de resurser som du flyttar.

- Om du flyttar resurser mellan regioner i portalen sker den här processen automatiskt.
- Om du flyttar resurser med hjälp av PowerShell kör du cmdletar för att tilldela en tilldelad identitet till samlingen och tilldelar sedan en roll med rätt prenumerations behörigheter till identitetens huvud namn. 

### <a name="what-managed-identity-permissions-does-resource-mover-need"></a>Vilka behörigheter för hanterad identitet behöver resurs förflyttningen? 

En Azure Resource Mover-hanterad identitet måste ha minst följande behörigheter: 

- Behörighet att skriva/skapa resurser i användar prenumeration, som är tillgängliga med *deltagar* rollen. 
- Behörighet att skapa rolltilldelningar. Är vanligt vis tillgängligt med administratörs rollerna *ägare* eller *användar åtkomst* , eller med en anpassad roll som har tilldelats *behörigheten Microsoft. auktorisering/roll tilldelningar/Skriv* . Den här behörigheten behövs inte om dataresursens hanterade identitet redan har beviljats åtkomst till Azure-datalagret. 
 
När du lägger till resurser i Resource Mover-hubben på portalen hanteras behörigheter automatiskt så länge användaren har de behörigheter som beskrivs ovan. Om du lägger till resurser med PowerShell tilldelar du behörigheter manuellt.

> [!IMPORTANT]
> Vi rekommenderar starkt att du inte ändrar eller tar bort roll tilldelningar för identitet. 

### <a name="what-should-i-do-if-i-dont-have-permissions-to-assign-role-identity"></a>Vad ska jag göra om jag inte har behörighet att tilldela roll identiteten?

**Möjlig orsak** | **Rekommendation**
--- | ---
Du är inte *deltagare* och *administratör för användar åtkomst* (eller *ägare*) när du lägger till en resurs för första gången. | Använd ett konto med behörighet som *deltagare* och *användar åtkomst administratör* (eller *ägare*) för prenumerationen.
Den hanterade identiteten för resurs förflyttad identitet saknar den roll som krävs. | Lägg till rollerna deltagare och användar åtkomst administratör.
Resurs förflyttnings hanterad identitet återställdes till *ingen*. | Återaktivera en systemtilldelad identitet i flytt samlingen > **identitet**. Du kan också lägga till resursen igen i **Lägg till resurser**, vilket gör samma sak.  
Prenumerationen har flyttats till en annan klient. | Inaktivera och aktivera sedan hanterad identitet för flytt samlingen.

### <a name="how-can-i-do-multiple-moves-together"></a>Hur kan jag göra flera flyttningar tillsammans?

Ändra kombinationerna av källa/mål efter behov med hjälp av alternativet ändra i portalen.

### <a name="what-happens-when-i-remove-a-resource-from-a-list-of-move-resources"></a>Vad händer när jag tar bort en resurs från en lista över flytt resurser?

Du kan ta bort resurser som du har lagt till i flytta listan. Beteende när du tar bort en resurs från listan beror på resursens tillstånd. [Läs mer](remove-move-resources.md#vm-resource-state-after-removing).



## <a name="next-steps"></a>Nästa steg

[Läs mer](about-move-process.md) om resurs förflyttnings komponenter och flyttnings processen.
