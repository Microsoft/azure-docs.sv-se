---
title: SSL/TLS-anslutning – Azure Database for MariaDB
description: Information om hur du konfigurerar Azure Database for MariaDB och associerade program för att använda SSL-anslutningar korrekt
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: 6c4ee8c6d12a6350c319e40bbe30046af4932311
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550648"
---
# <a name="ssltls-connectivity-in-azure-database-for-mariadb"></a>SSL/TLS-anslutning i Azure Database for MariaDB
Azure Database for MariaDB stöder anslutning av databas servern till klient program med hjälp av Secure Sockets Layer (SSL). Framtvingande av SSL-anslutningar mellan databasservern och klientprogrammen hjälper till att skydda mot ”man in the middle”-attacker genom att kryptera dataströmmen mellan servern och programmet.

>[!NOTE]
> Baserat på feedback från kunder har vi utökat rot certifikatets utfasning för vår befintliga Baltimore rot certifikat utfärdare till 15 februari 2021 (02/15/2021).

> [!IMPORTANT] 
> SSL-rotcertifikat är inställt på att upphöra att gälla den 15 februari 2021 (02/15/2021). Uppdatera ditt program för att använda det [nya certifikatet](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem). Mer information finns i avsnittet om [planerade certifikat uppdateringar](concepts-certificate-rotation.md)

## <a name="default-settings"></a>Standardinställningar
Som standard ska databas tjänsten konfigureras för att kräva SSL-anslutningar vid anslutning till MariaDB.  Vi rekommenderar att du inte inaktiverar SSL-alternativet när det är möjligt.

När du konfigurerar en ny Azure Database for MariaDB-server via Azure Portal och CLI är tvingande av SSL-anslutningar aktiverat som standard.

I vissa fall kräver program en lokal certifikat fil som genereras från en certifikat utfärdare för betrodda certifikat utfärdare för att ansluta säkert. För närvarande kan kunder **bara använda** det fördefinierade certifikatet för att ansluta till en Azure Database for MariaDB-server som finns på https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem . 

På samma sätt pekar följande länkar till certifikaten för servrar i suveräna moln: [Azure Government](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem), [Azure Kina](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem)och [Azure Germany](https://www.d-trust.net/cgi-bin/D-TRUST_Root_Class_3_CA_2_2009.crt).

Anslutnings strängar för olika programmeringsspråk visas i Azure Portal. Dessa anslutnings strängar innehåller de SSL-parametrar som krävs för att ansluta till databasen. I Azure Portal väljer du servern. Under rubriken **Inställningar** väljer du **anslutnings strängarna**. SSL-parametern varierar beroende på anslutningen, till exempel "SSL = true" eller "sslmode = Kräv" eller "sslmode = required" och andra variationer.

Information om hur du aktiverar eller inaktiverar SSL-anslutning när du utvecklar program finns i [så här konfigurerar du SSL](howto-configure-ssl.md).

## <a name="tls-enforcement-in-azure-database-for-mariadb"></a>TLS-tvång i Azure Database for MariaDB

Azure Database for MariaDB stöder kryptering för klienter som ansluter till din databas server med hjälp av Transport Layer Security (TLS). TLS är ett bransch standard protokoll som garanterar säkra nätverks anslutningar mellan din databas server och klient program, så att du kan följa kraven för efterlevnad.

### <a name="tls-settings"></a>TLS-inställningar

Azure Database for MariaDB ger möjlighet att genomdriva TLS-versionen för klient anslutningarna. Om du vill framtvinga TLS-versionen använder du inställningen **minsta TLS-version** . Följande värden är tillåtna för den här alternativ inställningen:

|  Minsta TLS-inställning             | Klientens TLS-version stöds                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (standard) | Ingen TLS krävs                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 och högre         |
| TLS1_1                           | TLS 1,1, TLS 1,2 och högre              |
| TLS1_2                           | TLS version 1,2 och högre                  |


Om du till exempel anger värdet för lägsta TLS-inställnings version till TLS 1,0 betyder det att servern tillåter anslutningar från klienter som använder TLS 1,0, 1,1 och 1.2 +. Du kan också ställa in detta på 1,2 innebär att du bara tillåter anslutningar från klienter som använder TLS 1.2 + och alla anslutningar med TLS 1,0 och TLS 1,1 kommer att avvisas.

> [!Note] 
> Som standard tillämpar Azure Database for MariaDB inte en lägsta TLS-version (inställningen `TLSEnforcementDisabled` ).
>
> När du har tillverkat en lägsta TLS-version kan du inte senare inaktivera den lägsta version som tillämpas.

Information om hur du anger TLS-inställningen för Azure Database for MariaDB finns i [Konfigurera TLS-inställningen](howto-tls-configurations.md).

## <a name="cipher-support-by-azure-database-for-mariadb"></a>Chiffrering stöder Azure Database for MariaDB

Som en del av SSL/TLS-kommunikationen verifieras chiffersviter och endast stöd för chiffersviter är tillåtna för att kommunicera med databas servern. Verifieringen av chiffersviter styrs i [Gateway-lagret](concepts-connectivity-architecture.md#connectivity-architecture) och inte uttryckligen på själva noden. Om chiffersviter inte matchar någon av paketen i listan nedan, kommer inkommande klient anslutningar att avvisas.

### <a name="cipher-suite-supported"></a>Chiffrering stöds

*   TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
*   TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
*   TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
*   TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

## <a name="next-steps"></a>Nästa steg
- Läs mer om [regler för Server brand vägg](concepts-firewall-rules.md)
- Lär dig hur du [konfigurerar SSL](howto-configure-ssl.md)
- Lär dig hur du [konfigurerar TLS](howto-tls-configurations.md)
