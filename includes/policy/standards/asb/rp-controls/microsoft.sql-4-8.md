---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: d8670127f871ded2cf01450609bf779deb3f2d9c
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103021625"
---
|Name<br /><sub>(Azure Portal)</sub> |Beskrivning |Påverkan (ar) |Version<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL-hanterade instanser bör använda Kundhanterade nycklar för att kryptera data i vila](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F048248b0-55cd-46da-b1ff-39efd52db260) |Genom att implementera transparent datakryptering (TDE) med din egen nyckel får du ökad insyn och kontroll över TDE-skyddet, ökad säkerhet med en HSM-baserad extern tjänst och befordran av separering av uppgifter. Den här rekommendationen gäller organisationer med ett relaterat krav på efterlevnad. |AuditIfNotExists, inaktiverat |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |
|[SQL-servrar bör använda Kundhanterade nycklar för att kryptera data i vila](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0d134df8-db83-46fb-ad72-fe0c9428c8dd) |Implementering av transparent datakryptering (TDE) med din egen nyckel ger ökad insyn och kontroll över TDE-skyddet, ökad säkerhet med en HSM-baserad extern tjänst och befordran av delning av uppgifter. Den här rekommendationen gäller organisationer med ett relaterat krav på efterlevnad. |AuditIfNotExists, inaktiverat |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |
|[transparent datakryptering på SQL-databaser ska aktive ras](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17k78e20-9358-41c9-923c-fb736d382a12) |Transparent data kryptering ska vara aktive rad för att skydda data i vila och uppfylla kraven för efterlevnad |AuditIfNotExists, inaktiverat |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlDBEncryption_Audit.json) |
