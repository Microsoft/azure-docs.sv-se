---
title: Registrera SAP S/4HANA-källa och installations genomsökningar (för hands version) i Azure avdelningens kontroll
description: Den här artikeln beskriver hur du registrerar SAP S/4HANA-källa i Azure avdelningens kontroll och konfigurerar en genomsökning.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: overview
ms.date: 2/25/2021
ms.openlocfilehash: 6d31bd0911b5cf765215e6a482a39b2458c4ba0d
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101696166"
---
# <a name="register-and-scan-a-sap-s4hana-source-preview"></a>Registrera och skanna en SAP S/4HANA-källa (för hands version)

Den här artikeln beskriver hur du registrerar en SAP S/4HANA-källa i avdelningens kontroll och konfigurerar en genomsökning.

## <a name="supported-capabilities"></a>Funktioner som stöds

SAP S/4HANA-källan stöder **fullständig sökning** för att extrahera metadata från en SAP s/4HANA-instans och hämtar **härkomst** mellan data till gångar.

## <a name="prerequisites"></a>Förutsättningar

1.  Konfigurera den senaste [integrerings körningen med egen värd](https://www.microsoft.com/download/details.aspx?id=39717).
    Mer information finns i [skapa och konfigurera en integration runtime med egen värd](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).

2.  Kontrol lera att [JDK 11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) är installerat på den virtuella datorn där integration runtime med egen värd är installerat.

3.  Se till att \" Visual C++ redistributable 2012 uppdatering 4 \" är installerat på den lokala datorn för integration Runtime. Om du \' ännu inte har installerat det kan du ladda ned det [här](https://www.microsoft.com/download/details.aspx?id=30679).

4.  Hämta 64-bitars [SAP-anslutaren för Microsoft .NET 3,0](https://support.sap.com/en/product/connectors/msnet.html) från SAP \' s webbplats och installera den på den lokala integration runtime-datorn. Under installationen ser du till att du väljer alternativet **Installera sammansättningar till GAC** i fönstret **valfria installations steg** .

    :::image type="content" source="media/register-scan-saps4hana-source/requirement.png" alt-text="krav" border="true":::

5.  Anslutningen läser metadata från SAP med API: t Java Connector (JCo) 3,0. Se därför till att Java Connector är tillgängligt på den virtuella datorn där integration runtime med egen värd är installerat.
    Kontrol lera att du använder rätt JCo-distribution för din miljö. Se till exempel att sapjco3. jar-och sapjco3.dll-filerna är tillgängliga på en Microsoft Windows-dator.

    > [!Note] 
    >Driv rutinen bör vara tillgänglig för alla konton på den virtuella datorn. Installera det inte i ett användar konto.

6.  Distribuera ABAP-modulen för metadata extrahering på SAP-servern genom att följa anvisningarna i [distributions guiden för ABAP Functions](abap-functions-deployment-guide.md). Du behöver ett ABAP Developer-konto för att skapa modulen RFC-funktion på SAP-servern. Användar kontot måste ha tillräcklig behörighet för att ansluta till SAP-servern och köra följande moduler i RFC-funktionen:
    -   STFC_CONNECTION (kontrol lera anslutning)
    -   RFC_SYSTEM_INFO (kontrol lera system information)

## <a name="setting-up-authentication-for-a-scan"></a>Konfigurera autentisering för en sökning

Den enda autentisering som stöds för SAP S/4HANA-källan är **grundläggande autentisering**

## <a name="register-sap-s4hana-source"></a>Registrera SAP S/4HANA-källa

Gör så här för att registrera en ny SAP S/4HANA-källa i data katalogen:

1.  Navigera till ditt avdelningens kontroll-konto.
2.  Välj **källor** i det vänstra navigerings fältet.
3.  Välj **register**
4.  Välj **SAP S/4HANA** på register källor. Välj **Fortsätt**

    :::image type="content" source="media/register-scan-saps4hana-source/register-saps-4-hana.png" alt-text="registrera SAPS/4Hana-alternativ" border="true":::

Gör följande på skärmen **Registrera källor (SAP S/4HANA)** :

1.  Ange ett **namn** som data källan ska visas i katalogen.

2.  Ange namnet på **program servern** för att ansluta till SAP S/4HANA-källan. Det kan också vara en IP-adress till SAP Application Server-värden.

3.  Ange SAP- **system numret**. Detta är ett tvåsiffrigt heltal mellan 00 och 99.

4.  Välj en samling eller skapa en ny (valfritt)

5.  Slutför för att registrera data källan.

    :::image type="content" source="media/register-scan-saps4hana-source/register-saps-4-hana-2.png" alt-text="registrera SAP S/4HANA" border="true":::

## <a name="creating-and-running-a-scan"></a>Skapa och köra en sökning

Om du vill skapa och köra en ny genomsökning gör du följande:

1.  Klicka på integration runtime i hanterings centret. Kontrol lera att en lokal integration Runtime har kon figurer ATS. Om den inte har kon figurer ATS använder du stegen [här](https://docs.microsoft.com/azure/purview/manage-integration-runtimes) för att skapa en egen värd för integration runtime

2.  Navigera till **källor.**

3.  Välj den registrerade SAP S/4HANA-källan.

4.  Välj **+ Ny skanning**

5.  Ange informationen nedan:

    a.  **Namn**: namnet på genomsökningen

    b.  **Anslut via integration runtime**: Välj den konfigurerade integrerings körningen med egen värd.

    c.  **Autentiseringsuppgift**: Välj autentiseringsuppgifter för att ansluta till data källan. Se till att:

    -   Välj grundläggande autentisering när du skapar en autentiseringsuppgift.
    -   Ange ett användar-ID för att ansluta till SAP-servern i fältet användar namn indata.
    -   Lagra användar lösen ordet som används för att ansluta till SAP-servern i den hemliga nyckeln.

    d.  **Klient-ID:** Ange här klient-ID för SAP-systemet. Klienten identifieras med ett tresiffrig tal från 000 till 999.

    e.  **Sökväg till JCo-bibliotek**: Ange sökvägen till den mapp där JCo-biblioteken finns.

    f.  **Maximalt tillgängligt minne:** Högsta mängd minne (i GB) som är tillgängligt på kundens virtuella dator som ska användas genom genomsökning av processer. Detta beror på storleken på SAP S/4HANA-källan som ska genomsökas.

    :::image type="content" source="media/register-scan-saps4hana-source/scan-saps-4-hana.png" alt-text="Skanna SAP S/4HANA" border="true":::

6.  Klicka på **Fortsätt**.

7.  Välj din **genomsöknings utlösare**. Du kan ställa in ett schema eller köra genomsökningen en gång.

8.  Granska din genomsökning och klicka på **Spara och kör**.

## <a name="viewing-your-scans-and-scan-runs"></a>Visa genomsökningar och skannings körningar

1. Gå till hanterings centret. Välj **data källor** under avsnittet **källor och sökning** .

2. Välj önskad data källa. En lista över befintliga genomsökningar visas på den data källan.

3. Välj den genomsökning vars resultat du är intresse rad av.

4. På den här sidan visas alla tidigare skannings körningar tillsammans med mått och status för varje genomsöknings körning. Den visar även om din genomsökning har schemalagts eller manuell, hur många till gångar som har klassificeringar tillämpade, hur många totala till gångar som har identifierats, start-och slut tid för genomsökningen och den totala genomsökningens varaktighet.

## <a name="manage-your-scans"></a>Hantera dina sökningar

Gör följande för att hantera eller ta bort en genomsökning:

1. Gå till hanterings centret. Välj **data källor** under avsnittet **källor och sökning** och välj sedan på önskad data källa.

2. Välj den genomsökning som du vill hantera. Du kan redigera sökningen genom att välja **Redigera**.

3. Du kan ta bort din genomsökning genom att välja **ta bort**.

## <a name="next-steps"></a>Nästa steg

- [Bläddra i Azure avdelningens kontroll Data Catalog](how-to-browse-catalog.md)
- [Sök i Azure avdelningens kontroll-Data Catalog](how-to-search-catalog.md)
