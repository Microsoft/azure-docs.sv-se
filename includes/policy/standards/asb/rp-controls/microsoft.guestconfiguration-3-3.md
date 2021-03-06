---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c495e2f25f9e55fc3147ca4c8bcddf51288865ea
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103021598"
---
|Name<br /><sub>(Azure Portal)</sub> |Beskrivning |Påverkan (ar) |Version<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Granska Windows-datorer som saknar angivna medlemmar i gruppen Administratörer](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F30f71ea1-ac77-4f26-9fc5-2d926bbd4ba7) |Kräver att nödvändiga komponenter distribueras till princip tilldelnings omfånget. Mer information finns på [https://aka.ms/gcpol](https://aka.ms/gcpol) . Datorer är icke-kompatibla om den lokala gruppen administratörer inte innehåller en eller flera medlemmar som anges i princip parametern. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_AdministratorsGroupMembersToInclude_AINE.json) |
|[Granska Windows-datorer som har extra konton i gruppen Administratörer](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3d2a3320-2a72-4c67-ac5f-caa40fbee2b2) |Kräver att nödvändiga komponenter distribueras till princip tilldelnings omfånget. Mer information finns på [https://aka.ms/gcpol](https://aka.ms/gcpol) . Datorer är icke-kompatibla om den lokala gruppen administratörer innehåller medlemmar som inte finns med i princip parametern. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_AdministratorsGroupMembers_AINE.json) |
|[Granska Windows-datorer som har de angivna medlemmarna i gruppen Administratörer](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F69bf4abd-ca1e-4cf6-8b5a-762d42e61d4f) |Kräver att nödvändiga komponenter distribueras till princip tilldelnings omfånget. Mer information finns på [https://aka.ms/gcpol](https://aka.ms/gcpol) . Datorer är icke-kompatibla om den lokala gruppen administratörer innehåller en eller flera av de medlemmar som anges i princip parametern. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_AdministratorsGroupMembersToExclude_AINE.json) |
