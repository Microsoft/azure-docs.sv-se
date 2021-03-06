---
title: Övervaka programmets inloggnings hälsa för återhämtning i Azure Active Directory
description: Skapa frågor och aviseringar för att övervaka dina programs inloggnings tillstånd.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/10/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad99c8d319a22f8b5388838b9d537de2f610478a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101650999"
---
# <a name="monitoring-application-sign-in-health-for-resilience"></a>Övervaka programmets inloggnings hälsa för återhämtning

Om du vill öka infrastrukturens återhämtning ställer du in övervakning av programmets inloggnings hälsa för dina kritiska program så att du får en avisering om en inverkan på incidenten inträffar. För att hjälpa dig i den här ansträngningen kan du konfigurera aviseringar baserat på arbets boken för inloggnings hälsa. 

Med den här arbets boken kan administratörer övervaka autentiseringsbegäranden för program i din klient organisation. Den ger följande viktiga funktioner:

* Konfigurera arbets boken för att övervaka alla eller enskilda appar med nästan real tids data.

* Konfigurera aviseringar för att meddela dig när autentiserings mönster ändras så att du kan undersöka och vidta åtgärder.

* Jämför trender under en period, till exempel vecka över vecka, som är arbets bokens standardinställning.

> [!NOTE]
> Om du vill se alla tillgängliga arbets böcker och förutsättningarna för att använda dem går du [till använda Azure Monitor arbets böcker för rapporter](../reports-monitoring/howto-use-azure-monitor-workbooks.md).

Vid en påverkande händelse kan två saker hända:

* Antalet inloggnings program för ett program kan släppa precipitously eftersom användarna inte kan logga in.

* Antalet felaktiga inloggningar kan öka. 

Den här artikeln beskriver hur du konfigurerar arbets boken för inloggnings hälsa för att övervaka avbrott i användarnas inloggnings program.

## <a name="prerequisites"></a>Förutsättningar 

* En Azure AD-klientorganisation.

* En användare med rollen global administratör eller säkerhets administratör för Azure AD-klienten.

* En Log Analytics arbets yta i din Azure-prenumeration för att skicka loggar till Azure Monitor loggar. 

   * Lär dig hur du [skapar en arbets yta för Log Analytics](../../azure-monitor/logs/quick-create-workspace.md)

* Azure AD-loggar integrerade med Azure Monitor loggar

   * Lär dig hur du [integrerar inloggnings loggar för Azure AD med Azure Monitor Stream.](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

 

## <a name="configure-the-app-sign-in-health-workbook"></a>Konfigurera arbets boken för appens inloggnings hälsa 

Om du vill komma åt arbets böcker öppnar du **Azure Portal**, väljer **Azure Active Directory** och väljer sedan **arbets böcker**.

Du ser arbets böcker under användning, villkorlig åtkomst och fel sökning. Arbets boken för appens inloggnings hälsa visas i användnings avsnittet.

När du använder en arbets bok kan den visas i avsnittet nyligen ändrade arbets böcker.

![Skärm bild som visar galleriet för arbets böcker i Azure Portal.](./media/monitor-sign-in-health-for-resilience/sign-in-health-workbook.png)


I arbets boken app Sign in Health kan du visualisera vad som händer med dina inloggningar. 

Som standard visar arbets boken två grafer. Dessa diagram Jämför vad som händer med din app (er) nu, jämfört med samma period för en vecka sedan. De blå linjerna är aktuella och de orange linjerna är föregående vecka.

![Skärm bild som visar inloggnings hälso diagram.](./media/monitor-sign-in-health-for-resilience/sign-in-health-graphs.png)

**Det första diagrammet är en timmes användning (antalet lyckade användare)**. Genom att jämföra det aktuella antalet lyckade användare till en typisk användnings period får du en utgångs punkt i användningen som kan kräva en undersökning. En minskning av lyckade användnings frekvenser kan hjälpa till att identifiera prestanda-och användnings problem som fel frekvensen inte kan göra. Om användarna till exempel inte kan komma åt ditt program för att försöka logga in, så skulle det inte finnas några problem, bara en direkt användning. En exempel fråga för dessa data finns i följande avsnitt.

Den andra grafen är en timmes problem frekvens. En ökning i fel frekvens kan tyda på ett problem med dina autentiseringsmekanismer. Felfrekvensen kan bara mätas om användarna kan försöka autentisera sig. Om användarna inte kan få åtkomst till att göra försöket visas inte felen.

Du kan konfigurera en avisering som meddelar en specifik grupp när användningen eller felfrekvensen överskrider ett angivet tröskelvärde. En exempel fråga för dessa data finns i följande avsnitt.

 ## <a name="configure-the-query-and-alerts"></a>Konfigurera frågan och aviseringar

Du skapar varnings regler i Azure Monitor och kan automatiskt köra sparade frågor eller anpassade loggs ökningar med jämna mellanrum.

Använd följande instruktioner för att skapa e-postaviseringar baserat på de frågor som återges i diagrammen. Exempel skripten nedan skickar ett e-postmeddelande när

* den framgångna användningen går från 90% från samma timme två dagar sedan, precis som i diagrammet per timme i föregående avsnitt. 

* felfrekvensen ökar med 90% från samma timme två dagar sedan, precis som i grafen med Tim avbrott i föregående avsnitt. 

 Utför följande steg för att konfigurera den underliggande frågan och ange aviseringar. Du använder exempel frågan som grund för din konfiguration. En förklaring av frågans struktur visas i slutet av det här avsnittet.

Mer information om hur du skapar, visar och hanterar logg aviseringar med Azure Monitor finns i [Hantera logg aviseringar](../../azure-monitor/alerts/alerts-log.md).

 
1. I arbets boken väljer du **Redigera** och väljer sedan **ikonen fråga** ovanför den högra sidan i grafen.   

   [![Skärm bild som visar redigera arbets bok.](./media/monitor-sign-in-health-for-resilience/edit-workbook.png)](./media/monitor-sign-in-health-for-resilience/edit-workbook.png)

   Fråge loggen öppnas.

  [![Skärm bild som visar loggen för frågor.](./media/monitor-sign-in-health-for-resilience/query-log.png)](/media/monitor-sign-in-health-for-resilience/query-log.png)
‎

2. Kopiera något av följande exempel skript för en ny Kusto-fråga.

**Kusto fråga för att ta bort användning**

```Kusto

let thisWeek = SigninLogs

| where TimeGenerated > ago(1h)

| project TimeGenerated, AppDisplayName, UserPrincipalName

//| where AppDisplayName contains "Office 365 Exchange Online"

| summarize users = dcount(UserPrincipalName) by bin(TimeGenerated, 1hr)

| sort by TimeGenerated desc

| serialize rn = row_number();

let lastWeek = SigninLogs

| where TimeGenerated between((ago(1h) - totimespan(2d))..(now() - totimespan(2d)))

| project TimeGenerated, AppDisplayName, UserPrincipalName

//| where AppDisplayName contains "Office 365 Exchange Online"

| summarize usersPriorWeek = dcount(UserPrincipalName) by bin(TimeGenerated, 1hr)

| sort by TimeGenerated desc

| serialize rn = row_number();

thisWeek

| join

(

 lastWeek

)

on rn

| project TimeGenerated, users, usersPriorWeek, difference = abs(users - usersPriorWeek), max = max_of(users, usersPriorWeek)

| where (difference * 2.0) / max > 0.9

```

 

**Kusto fråga för ökning i felgrad**


```kusto

let thisWeek = SigninLogs

| where TimeGenerated > ago(1 h)

| project TimeGenerated, UserPrincipalName, AppDisplayName, status = case(Status.errorCode == "0", "success", "failure")

| where AppDisplayName == **APP NAME**

| summarize success = countif(status == "success"), failure = countif(status == "failure") by bin(TimeGenerated, 1h)

| project TimeGenerated, failureRate = (failure * 1.0) / ((failure + success) * 1.0)

| sort by TimeGenerated desc

| serialize rn = row_number();

let lastWeek = SigninLogs

| where TimeGenerated between((ago(1 h) - totimespan(2d))..(ago(1h) - totimespan(2d)))

| project TimeGenerated, UserPrincipalName, AppDisplayName, status = case(Status.errorCode == "0", "success", "failure")

| where AppDisplayName == **APP NAME**

| summarize success = countif(status == "success"), failure = countif(status == "failure") by bin(TimeGenerated, 1h)

| project TimeGenerated, failureRatePriorWeek = (failure * 1.0) / ((failure + success) * 1.0)

| sort by TimeGenerated desc

| serialize rn = row_number();

thisWeek

| join (lastWeek) on rn

| project TimeGenerated, failureRate, failureRatePriorWeek

| where abs(failureRate – failureRatePriorWeek) > **THRESHOLD VALUE**

```

3. Klistra in frågan i fönstret och välj **Kör**. Se till att du ser det slutförda meddelandet som visas i bilden nedan och resultaten under meddelandet.

   [![Skärm bild som visar resultatet av kör frågan.](./media/monitor-sign-in-health-for-resilience/run-query.png)](./media/monitor-sign-in-health-for-resilience/run-query.png)

4. Markera frågan och välj + **ny varnings regel**. 
 
   [![Skärm bild som visar skärmen ny varnings regel.](./media/monitor-sign-in-health-for-resilience/new-alert-rule.png)](./media/monitor-sign-in-health-for-resilience/new-alert-rule.png)


5. Konfigurera aviserings villkor. I avsnittet villkor väljer du länken **när den genomsnittliga anpassade loggs ökningen är större än det definierade antalet**. I fönstret Konfigurera signal logik bläddrar du till aviserings logik

   [![Skärm bild som visar skärmen konfigurera aviseringar.](./media/monitor-sign-in-health-for-resilience/configure-alerts.png)](./media/monitor-sign-in-health-for-resilience/configure-alerts.png)
 
   * **Tröskelvärde**: 0. Det här värdet meddelar om eventuella resultat.

   * **Utvärderings period (i minuter)**: 60. Det här värdet ser ut ungefär en gång i timmen

   * **Frekvens (i minuter)**: 60. Det här värdet anger utvärderings perioden till en gång per timme för den föregående timmen.

   * Välj **Klar**.

6. Konfigurera de här inställningarna i avsnittet **åtgärder** :  

    [![Skärm bild som visar sidan Skapa aviserings regel.](./media/monitor-sign-in-health-for-resilience/create-alert-rule.png)](./media/monitor-sign-in-health-for-resilience/create-alert-rule.png)

   * Under **åtgärder** väljer du **Välj åtgärds grupp** och lägger till den grupp som du vill meddelas om aviseringar.

   * Under **Anpassa åtgärder** väljer du **e-postaviseringar**.

   * Lägg till en **ämnesrad**.

7. Konfigurera de här inställningarna under **aviserings regel information**:

   * Lägg till ett beskrivande namn och en beskrivning.

   * Välj den **resurs grupp** som aviseringen ska läggas till i.

   * Välj standard **allvarlighets grad** för aviseringen.

   * Välj **Aktivera aviserings regel vid skapande** om du vill att den ska vara direkt, annars väljer du **Ignorera aviseringar**.

8. Välj **Skapa varningsregel**.

9. Välj **Spara**, ange ett namn för frågan, **Spara som en fråga med en aviserings kategori**. Välj sedan **Spara** igen.  

   [![Skärm bild som visar knappen Spara fråga.](./media/monitor-sign-in-health-for-resilience/save-query.png)](./media/monitor-sign-in-health-for-resilience/save-query.png)



### <a name="refine-your-queries-and-alerts"></a>Förfina dina frågor och aviseringar
Ändra dina frågor och aviseringar för maximal effektivitet.

* Se till att testa dina aviseringar.

* Ändra känslighet och frekvens för aviseringar så att du får viktiga aviseringar. Administratörer kan bli desensitized till varningar om de får för många och saknar något viktigt. 

* Se till att e-postmeddelandet som visas i din administratörs e-postklienter läggs till i listan över tillåtna avsändare. Annars kan du missa aviseringar på grund av ett skräp post filter på din e-postklient. 

* Aviserings frågan i Azure Monitor får bara innehålla resultat från de senaste 48 timmarna. [Detta är en aktuell begränsning efter design](https://github.com/MicrosoftDocs/azure-docs/issues/22637).

## <a name="create-processes-to-manage-alerts"></a>Skapa processer för att hantera aviseringar

När du har konfigurerat frågan och aviseringarna skapar du affärs processer för att hantera aviseringarna.

* Vem ska övervaka arbets boken och när?
* Vem kommer att undersöka när en avisering genereras?

* Vilka är kommunikations behoven? Vem kommer att skapa kommunikationen och vem som ska få dem?

* Vilka affärs processer behöver utlösas om ett avbrott inträffar?

## <a name="next-steps"></a>Nästa steg

[Läs mer om arbets böcker](../reports-monitoring/howto-use-azure-monitor-workbooks.md)

 

 

