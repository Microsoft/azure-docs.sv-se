---
title: Felsöka säkerhets-och åtkomst kontroll problem
description: Lär dig hur du felsöker säkerhets-och åtkomst kontroll problem i Azure Data Factory.
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 02/04/2021
ms.author: lle
ms.reviewer: craigg
ms.openlocfilehash: 37da2869f0cf315ac0a6851c9a77dcee4debf71a
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/05/2021
ms.locfileid: "99581694"
---
# <a name="troubleshoot-azure-data-factory-security-and-access-control-issues"></a>Felsöka problem med Azure Data Factory säkerhet och åtkomst kontroll

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Den här artikeln utforskar vanliga fel söknings metoder för säkerhet och åtkomst kontroll i Azure Data Factory.

## <a name="common-errors-and-messages"></a>Vanliga fel och meddelanden

### <a name="connectivity-issue-in-the-copy-activity-of-the-cloud-datastore"></a>Anslutnings problem i kopierings aktiviteten för moln data lagret

#### <a name="symptoms"></a>Symtom

Olika fel meddelanden kan returneras när anslutnings problem uppstår i käll-eller Sink-datalagret.

#### <a name="cause"></a>Orsak 

Problemet orsakas vanligt vis av något av följande faktorer:

* Proxyinställningarna i IR-noden (egen värd för integration Runtime) om du använder en IR-anslutning med egen värd.

* Brand Väggs inställningen i den egna IR-noden, om du använder en lokal IR-anslutning.

* Brand Väggs inställningen i moln data lagret.

#### <a name="resolution"></a>Lösning

* Kontrol lera följande saker för att säkerställa att det här är ett anslutnings problem:

   - Felet genereras från källan eller Sink-anslutningarna.
   - Problemet är i början av kopierings aktiviteten.
   - Felet är konsekvent för Azure IR eller den infraröda IR-filen med en nod, eftersom det kan vara ett slumpmässigt fel i en lokal IR-värd med flera noder om bara några av noderna har problem.

* Om du använder en **IR med egen värd** kontrollerar du proxy-, brand Väggs-och nätverks inställningarna, eftersom det kan gå att ansluta till samma data lager om du använder en Azure IR. För att felsöka det här scenariot, se:

   * [IR-portar och brand väggar med egen värd](./create-self-hosted-integration-runtime.md#ports-and-firewalls)
   * [Azure Data Lake Storage koppling](./connector-azure-data-lake-store.md)
  
* Om du använder en **Azure IR** kan du försöka inaktivera brand Väggs inställningen för data lagret. Den här metoden kan lösa problemen i följande två situationer:
  
   * [Azure IR IP-adresser](./azure-integration-runtime-ip-addresses.md) finns inte med i listan över tillåtna.
   * Funktionen *Tillåt betrodda Microsoft-tjänster för att få åtkomst till det här lagrings kontot* är inaktive rad för [Azure Blob Storage](./connector-azure-blob-storage.md#supported-capabilities) och [Azure Data Lake Storage gen 2](./connector-azure-data-lake-storage.md#supported-capabilities).
   * Inställningen *Tillåt åtkomst till Azure-tjänster* är inte aktive rad för Azure Data Lake Storage gen1.

Om ingen av föregående metoder fungerar kontaktar du Microsoft om du behöver hjälp.


### <a name="invalid-or-empty-authentication-key-issue-after-public-network-access-is-disabled"></a>Ogiltigt eller tomt problem med autentiserings nyckel efter att offentlig nätverks åtkomst har inaktiverats

#### <a name="symptoms"></a>Symtom

När du har inaktiverat offentlig nätverks åtkomst för Data Factory, genererar den egen värdbaserade integrerings körningen följande fel: "nyckeln för autentisering är ogiltig eller tom."

#### <a name="cause"></a>Orsak

Problemet orsakas troligen av ett problem med en Domain Name System (DNS), eftersom inaktive ring av offentliga anslutningar och etablering av en privat slut punkt förhindrar åter anslutning.

Gör så här för att kontrol lera om Data Factory fullständiga domän namnet (FQDN) matchas med den offentliga IP-adressen:

1. Bekräfta att du har skapat den virtuella Azure-datorn (VM) i samma virtuella nätverk som den Data Factory privata slut punkten.

2. Kör PsPing och ping från den virtuella Azure-datorn till Data Factory FQDN:

   `psping.exe <dataFactoryName>.<region>.datafactory.azure.net:443`
   `ping <dataFactoryName>.<region>.datafactory.azure.net`

   > [!Note]
   > Du måste ange en port för PsPing-kommandot. Port 443 visas här men krävs inte.

3. Kontrol lera om båda kommandona matchar en Azure Data Factory offentliga IP-adress som baseras på en angiven region. IP-adressen ska ha följande format: `xxx.xxx.xxx.0`

#### <a name="resolution"></a>Lösning

Lös problemet genom att göra följande:

- Som alternativ vill vi föreslå att du manuellt lägger till en "Virtual Network länk" under Data Factory "privat länk till DNS-zon". Mer information finns i den [privata Azure-länken för Azure Data Factory](./data-factory-private-link.md#dns-changes-for-private-endpoints) artikeln. Instruktionen är att konfigurera den privata DNS-zonen eller en anpassad DNS-server för att matcha Data Factory FQDN till en privat IP-adress. 

- Men om du inte vill konfigurera den privata DNS-zonen eller den anpassade DNS-servern kan du prova följande tillfälliga lösning:

  1. Ändra värd filen i Windows och mappa den privata IP-adressen (den Azure Data Factory privata slut punkten) till Azure Data Factory FQDN.
  
     I den virtuella Azure-datorn går du till `C:\Windows\System32\drivers\etc` och öppnar *värd* filen i anteckningar. Lägg till raden som mappar den privata IP-adressen till FQDN i slutet av filen och spara ändringen.
     
     ![Skärm bild av den privata IP-adressen till värden.](media/self-hosted-integration-runtime-troubleshoot-guide/add-mapping-to-host.png)

  1. Kör samma kommandon på samma sätt som i föregående verifierings steg för att kontrol lera svaret, som ska innehålla den privata IP-adressen.

  1. Registrera den egna värdbaserade integrerings körningen igen så bör problemet lösas.

### <a name="unable-to-register-ir-authentication-key-on-self-hosted-vms-due-to-private-link"></a>Det går inte att registrera IR-autentisering på lokala datorer med egen värd på grund av en privat länk

#### <a name="symptoms"></a>Symtom

Det går inte att registrera IR-autentiseringsnyckel på den virtuella datorn med egen värd eftersom den privata länken är aktive rad. Du får följande fel meddelande:

"Det gick inte att hämta tjänstens token från ADF-tjänsten med nyckeln * * * * * * * * * * * * * * och tids kostnaden är: 0,1250079 sekunder: felkoden är: InvalidGatewayKey, activityId är: XXXXXXX och detaljerat fel meddelande är klientens IP-adress är inte giltig privat IP-orsak Data Factory kunde inte komma åt det offentliga nätverket och kan därför inte nå ut till molnet för att göra anslutningen korrekt."

#### <a name="cause"></a>Orsak

Problemet kan orsakas av den virtuella dator som du försöker installera IR med egen värd. För att ansluta till molnet, se till att åtkomst till offentligt nätverk är aktiverat.

#### <a name="resolution"></a>Lösning

**Lösning 1**
 
Lös problemet genom att göra följande:

1. Gå till sidan [fabriker – uppdatera](/rest/api/datafactory/Factories/Update) .

1. I det övre högra hörnet väljer du knappen **prova** .
1. Under **parametrar**, Fyll i den information som krävs. 
1. Under **brödtext** klistrar du in följande egenskap:

    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ```
1. Välj **Kör** för att köra funktionen. 

1. Under **parametrar**, Fyll i den information som krävs. 

1. Under **brödtext** klistrar du in följande egenskap:
    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ``` 

1. Välj **Kör** för att köra funktionen. 
1. Bekräfta att **svars koden: 200** visas. Den egenskap du klistrade in ska även visas i JSON-definitionen.

1. Lägg till IR-autentiseringsnyckel igen i integration Runtime.


**Lösning 2**

Lös problemet genom att gå till [Azures privata länk för Azure Data Factory](./data-factory-private-link.md).

Försök att aktivera offentlig nätverks åtkomst i användar gränssnittet, som visas på följande skärm bild:

![Skärm bild av "Enabled"-kontrollen för "Tillåt offentlig nätverks åtkomst" i fönstret nätverk.](media/self-hosted-integration-runtime-troubleshoot-guide/enable-public-network-access.png)

## <a name="next-steps"></a>Nästa steg

Om du behöver hjälp med fel sökning kan du prova följande resurser:

*  [Privat länk för Data Factory](data-factory-private-link.md)
*  [Data Factory blogg](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory funktions begär Anden](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-videor](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Microsoft Q&en sida](/answers/topics/azure-data-factory.html)
*  [Stack Overflow-forum för Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-information om Data Factory](https://twitter.com/hashtag/DataFactory)