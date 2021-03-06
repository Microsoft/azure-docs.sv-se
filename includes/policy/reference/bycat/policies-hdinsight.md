---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 4ec54a4d6488d29c61053f52b29cfd5d9ec2a4dd
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102611369"
---
|Name<br /><sub>(Azure Portal)</sub> |Beskrivning |Påverkan (ar) |Version<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure HDInsight-kluster ska matas in i ett virtuellt nätverk](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb0ab5b05-1c98-40f7-bb9e-dc568e41b501) |Genom att mata in Azure HDInsight-kluster i ett virtuellt nätverk låser du upp avancerade HDInsight-nätverks-och säkerhetsfunktioner och ger dig kontroll över din nätverks säkerhets konfiguration. |Granskning, inaktive rad, neka |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_VNETInjection_Audit.json) |
|[Azure HDInsight-kluster bör använda Kundhanterade nycklar för att kryptera data i vila](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F64d314f6-6062-4780-a861-c23e8951bee5) |Använd Kundhanterade nycklar för att hantera krypteringen i resten av dina Azure HDInsight-kluster. Som standard krypteras kund information med tjänst hanterade nycklar, men Kundhanterade nycklar krävs ofta för att uppfylla gällande regler för efterlevnad. Kundhanterade nycklar gör det möjligt att kryptera data med en Azure Key Vault-nyckel som skapats och ägs av dig. Du har fullständig kontroll och ansvar för nyckel livs cykeln, inklusive rotation och hantering. Läs mer på [https://aka.ms/hdi.cmk](https://aka.ms/hdi.cmk) . |Granska, neka, inaktive rad |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_CMK_Audit.json) |
|[Azure HDInsight-kluster bör använda kryptering på värden för att kryptera data i vila](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1fd32ebd-e4c3-4e13-a54a-d7422d4d95f6) |Att aktivera kryptering på värden hjälper till att skydda och skydda dina data så att de uppfyller organisationens säkerhets-och efterlevnads åtaganden. När du aktiverar kryptering på värden krypteras data som lagras på den virtuella dator värden i vila och flöden krypteras till lagrings tjänsten. |Granska, neka, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionAtHost_Audit.json) |
|[Azure HDInsight-kluster bör använda kryptering i överföring för att kryptera kommunikationen mellan Azure HDInsight-klusternoder](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd9da03a1-f3c3-412a-9709-947156872263) |Data kan manipuleras under överföringen mellan Azure HDInsight-klusternoder. Att aktivera kryptering i transit hanterar problem med missbruk och manipulering under överföringen. |Granska, neka, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionInTransit_Audit.json) |
