---
title: Användare och grupper i princip för villkorlig åtkomst – Azure Active Directory
description: Som är användare och grupper i en princip för villkorlig åtkomst för Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/04/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f316b17096e1241fe23cbf2c965122fd8966522
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102630885"
---
# <a name="conditional-access-users-and-groups"></a>Villkorlig åtkomst: användare och grupper

En princip för villkorlig åtkomst måste innehålla en användar tilldelning som en av signalerna i besluts processen. Användare kan tas med eller undantas från principer för villkorlig åtkomst. Azure Active Directory utvärderar alla principer och säkerställer att alla krav uppfylls innan åtkomst beviljas till användaren. Förutom den här artikeln har vi en video om hur du [tar med eller undantar användare från principer för villkorlig åtkomst](https://www.youtube.com/watch?v=5DsW1hB3Jqs) som vägleder dig genom processen som beskrivs nedan. 

![Användare som en signal i besluten som fattas av villkorlig åtkomst](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Inkludera användare

Den här listan över användare inkluderar vanligt vis alla användare som en organisation är mål för i en princip för villkorlig åtkomst. 

Följande alternativ är tillgängliga för att ta med när du skapar en princip för villkorlig åtkomst.

- Inget
   - Inga användare har valts
- Alla användare
   - Alla användare som finns i katalogen, inklusive B2B-gäster.
- Välj användare och grupper
   - Alla gäst-och externa användare
      - Det här valet inkluderar alla B2B-gäster och externa användare, inklusive alla användare med `user type` attributet inställt på `guest` . Det här valet gäller även för alla externa användare som är inloggade från en annan organisation som en moln lösnings leverantör (CSP). 
   - Katalogroller
      - Administratörer kan välja särskilda inbyggda Azure AD-katalog roller som används för att fastställa princip tilldelningen. Organisationer kan till exempel skapa en mer restriktiv princip för användare som har tilldelats rollen som global administratör. Andra roll typer stöds inte, inklusive administrativa enhets omfångs katalog roller, anpassade roller.
   - Användare och grupper
      - Tillåter mål för specifika uppsättningar av användare. Organisationer kan till exempel välja en grupp som innehåller alla medlemmar i PERSONALAVDELNINGEN när en HR-app väljs som Cloud-App. En grupp kan vara vilken typ av grupp som helst i Azure AD, inklusive dynamiska eller tilldelade säkerhets-och distributions grupper. Principen kommer att tillämpas på kapslade användare och grupper.

> [!IMPORTANT]
> När du väljer vilka användare och grupper som ska ingå i en princip för villkorlig åtkomst finns det en gräns för hur många enskilda användare som kan läggas till direkt i en princip för villkorlig åtkomst. Om det finns stora mängder enskilda användare som behövs för att läggas till direkt i en princip för villkorlig åtkomst, rekommenderar vi att du placerar användarna i en grupp och tilldelar gruppen till principen för villkorlig åtkomst i stället.

> [!WARNING]
> Om användare eller grupper är medlem i över 2048 grupper kan deras åtkomst blockeras. Den här gränsen gäller både direkt och kapslad grupp medlemskap.

> [!WARNING]
> Principer för villkorlig åtkomst stöder inte användare som har tilldelats en katalog roll som är [begränsad till en administrativ enhet](../roles/admin-units-assign-roles.md) eller katalog roller som omfattas direkt till ett-objekt, t. ex. genom [anpassade roller](../roles/custom-create.md).

## <a name="exclude-users"></a>Exkludera användare

När organisationer både inkluderar och exkluderar en användare eller grupp, undantas användaren eller gruppen från principen, eftersom en undantags åtgärd åsidosätter en inkludera i principen. Undantag används ofta för nöd åtkomst eller Bryt glass konton. Mer information om konton för nöd åtkomst och varför de är viktiga finns i följande artiklar: 

* [Hantera konton för nödåtkomst i Azure AD](../roles/security-emergency-access.md)
* [Skapa en elastisk strategi för hantering av åtkomst kontroll med Azure Active Directory](../authentication/concept-resilient-controls.md)

Följande alternativ är tillgängliga för att undanta när du skapar en princip för villkorlig åtkomst.

- Alla gäst-och externa användare
   - Det här valet inkluderar alla B2B-gäster och externa användare, inklusive alla användare med `user type` attributet inställt på `guest` . Det här valet gäller även för alla externa användare som är inloggade från en annan organisation som en moln lösnings leverantör (CSP). 
- Katalogroller
   - Administratörer kan välja särskilda Azure AD-katalog roller som används för att fastställa tilldelningen. Organisationer kan till exempel skapa en mer restriktiv princip för användare som har tilldelats rollen som global administratör.
- Användare och grupper
   - Tillåter mål för specifika uppsättningar av användare. Organisationer kan till exempel välja en grupp som innehåller alla medlemmar i PERSONALAVDELNINGEN när en HR-app väljs som Cloud-App. En grupp kan vara vilken typ av grupp som helst i Azure AD, inklusive dynamiska eller tilldelade säkerhets-och distributions grupper.

### <a name="preventing-administrator-lockout"></a>Förhindra administratörs utelåsning

För att förhindra att en administratör låser sig själva från sin katalog när du skapar en princip som tillämpas på **alla användare** och **alla appar** visas följande varning.

> Lås inte dig själv! Vi rekommenderar att du använder en princip för en liten uppsättning användare först för att kontrol lera att den fungerar som förväntat. Vi rekommenderar också att du undantar minst en administratör från den här principen. Detta säkerställer att du fortfarande har åtkomst och kan uppdatera en princip om en ändring krävs. Granska berörda användare och appar.

Som standard tillhandahåller principen ett alternativ för att undanta den aktuella användaren från principen, men denna standard kan åsidosättas av administratören som visas i följande bild. 

![Varning, Lås inte ut!](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups-lockout-warning.png)

[Vad ska jag göra om du är utelåst från Azure Portal?](troubleshoot-conditional-access.md#what-to-do-if-you-are-locked-out-of-the-azure-portal)

## <a name="next-steps"></a>Nästa steg

- [Villkorlig åtkomst: molnappar eller åtgärder](concept-conditional-access-cloud-apps.md)

- [Vanliga principer för villkorlig åtkomst](concept-conditional-access-policy-common.md)
