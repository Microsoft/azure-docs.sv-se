---
title: Felsök vanliga problem med att hantera onboarding i Azure
description: Vanliga fel vid autohantering av registrerings fel och fel sökning
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: alsin
ms.openlocfilehash: df5133ad4bb3155afdc9d43e595591d9cfda4ea0
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644450"
---
# <a name="troubleshoot-common-automanage-onboarding-errors"></a>Felsök vanliga fel vid autohantering av onboarding
Den automatiska hanteringen kan Miss lyckas med att publicera en dator på tjänsten. Det här dokumentet innehåller information om hur du felsöker distributions fel, delar några vanliga orsaker till att distributioner kan Miss lyckas och beskriver potentiella nästa steg i minskningen.

## <a name="troubleshooting-deployment-failures"></a>Felsöka distributions fel
När en dator registreras på autohantering leder det till att en Azure Resource Manager distribution skapas. Om onboarding Miss lyckas kan det vara bra att kontakta distributionen för mer information om varför den misslyckades. Det finns länkar till distributionerna i disfällbar information som visas i bilden nedan.

:::image type="content" source="media\automanage-common-errors\failure-flyout.png" alt-text="Utfällbar information om autostyrning av problem.":::

### <a name="check-the-deployments-for-the-resource-group-containing-the-failed-vm"></a>Kontrol lera distributionerna för resurs gruppen som innehåller den felande virtuella datorn
Fel utfällning innehåller en länk till distributionerna i den resurs grupp som innehåller den dator som inte kunde registreras och ett prefixvärde som du kan använda för att filtrera distributioner med. När du klickar på länken går du till bladet distributioner, där du sedan kan filtrera distributioner för att se Hantera distributioner automatiskt på datorn. Om du distribuerar över flera regioner måste du se till att du klickar på distributionen i rätt region.

### <a name="check-the-deployments-for-the-subscription-containing-the-failed-vm"></a>Kontrol lera distributionerna för prenumerationen som innehåller den felande virtuella datorn
Om du inte ser några fel i resurs grupps distributionen är nästa steg att titta på distributionerna i prenumerationen som innehåller den virtuella datorn som inte kunde registreras. Klicka på länken **distributioner för prenumeration** i felfällingen och filtrera distributioner med hjälp av filtret **automanage-DefaultResourceGroup** . Använd resurs grupps namnet från bladet om du vill filtrera distributioner. Distributions namnet kommer att suffixs med ett region namn. Om du distribuerar över flera regioner måste du se till att du klickar på distributionen i rätt region.

### <a name="check-deployments-in-a-subscription-linked-to-a-log-analytics-workspace"></a>Kontrol lera distributioner i en prenumeration som är länkad till en Log Analytics arbets yta
Om du inte ser några misslyckade distributioner i resurs gruppen eller prenumerationen som innehåller den virtuella datorn som misslyckades, och om den virtuella datorn har anslutits till en Log Analytics arbets yta i en annan prenumeration, går du till den prenumeration som är länkad till din Log Analytics arbets yta och kontrollerar om det finns misslyckade distributioner.

## <a name="common-deployment-errors"></a>Vanliga distributionsfel

Fel |  Åtgärd
:-----|:-------------|
Fel vid autohantering av konto för otillräcklig behörighet | Detta kan inträffa om du nyligen har flyttat en prenumeration som innehåller ett nytt konto för autohantering till en ny klient. Steg för att lösa detta finns [här](./repair-automanage-account.md).
Området för arbets ytan matchar inte region mappnings kraven | Det gick inte att publicera datorn utan den Log Analytics arbets ytan som datorn är länkad till är inte mappad till en Automation-region som stöds. Se till att din befintliga Log Analytics arbets yta och Automation-konto finns i en [region mappning som stöds](../automation/how-to/region-mappings.md).
"Tilldelningen misslyckades; Det finns ingen ytterligare information tillgänglig " | Öppna ett ärende med Microsoft Azure support.

## <a name="next-steps"></a>Nästa steg

* [Läs mer om Azure automanage](./automanage-virtual-machines.md)

> [!div class="nextstepaction"]
> [Aktivera automanage för virtuella datorer i Azure Portal](quick-create-virtual-machines-portal.md)