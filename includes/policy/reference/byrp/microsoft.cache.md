---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/04/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 4ab99740697e7026f9c90178bb9b4912e8119674
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/04/2021
ms.locfileid: "99558882"
---
|Name<br /><sub>(Azure Portal)</sub> |Description |Påverkan (ar) |Version<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure cache för Redis bör finnas i ett virtuellt nätverk](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7d092e0a-7acd-40d2-a975-dca21cae48c4) |Azure Virtual Network (VNet)-distribution ger förbättrad säkerhet och isolering för Azure-cache för Redis, samt undernät, åtkomst kontroll principer och andra funktioner för att ytterligare begränsa åtkomst. När en Azure-cache för Redis-instans har kon figurer ATS med ett VNet, är den inte offentligt adresserad och kan endast nås från virtuella datorer och program i VNet. |Granska, neka, inaktive rad |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_CacheInVnet_Audit.json) |
|[Endast säkra anslutningar till Azure-cachen för Redis ska aktive ras](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |Granska aktivering av enbart anslutningar via SSL till Azure cache för Redis. Användningen av säkra anslutningar säkerställer autentiseringen mellan servern och tjänsten och skyddar data i överföring från nätverks lager attacker, till exempel man-in-the-, avlyssnings-och session-kapning |Granska, neka, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
