---
title: Betala din faktura för Microsoft Azure
description: Lär dig hur du betalar en faktura i Microsoft Azure-portalen. Du måste vara ägare, deltagare eller fakturaansvarig för faktureringsprofilen för att betala i portalen.
keywords: billing, past due, balance, pay now,
author: banders
ms.reviewer: judupont
tags: billing, past due, pay now, bill, invoice, pay
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 01/13/2021
ms.author: banders
ms.openlocfilehash: ecc5c8ebef0d2add365d128e11caedaa173d9d63
ms.sourcegitcommit: ec39209c5cbef28ade0badfffe59665631611199
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103232151"
---
# <a name="how-to-pay-your-bill-for-microsoft-azure"></a>Så betalar du din faktura för Microsoft Azure

Den här artikeln gäller kunder med ett Microsoft-kundavtal.

[Kontrollera din åtkomst till ett Microsoft-kundavtal](#check-access-to-a-microsoft-customer-agreement).

Det finns två sätt att betala en faktura för Azure. Du kan betala med betalningsmetoden som är standard för din faktureringsprofil, eller göra en engångsbetalning som kallas **Betala nu**.

Om du har registrerat dig för Azure via en Microsoft-representant anges din standardbetalningsmetod alltid som *check eller banköverföring*.

Om du har Azure-krediter tillämpas de automatiskt på din faktura varje faktureringsperiod.

## <a name="pay-by-default-payment-method"></a>Betala med betalningsmetoden som är standard

Standardbetalningsmetoden för din faktureringsprofil kan antingen vara ett kredit- eller debetkort, check eller banköverföring.

### <a name="credit-or-debit-card"></a>Kredit- eller debetkort

Om standardbetalningsmetoden för din faktureringsprofil är ett kredit- eller debetkort debiteras detta kort automatiskt varje faktureringsperiod.

Om den automatiska kredit- eller debetkortbetalningen inte går igenom av någon anledning kan du göra en engångsbetalning med ett kredit- eller debetkort i Microsoft Azure-portalen med hjälp av funktionen **Betala nu**.

Läs artikeln [Så här betalar du med faktura](../manage/pay-by-invoice.md) om du vill lära dig hur du ändrar standardbetalningsmetoden för att betala med check eller via banköverföring.

### <a name="check-or-wire-transfer"></a>Check eller banköverföring

Om standardbetalningsmetoden för din faktureringsprofil är med check eller via banköverföring följer du betalningsanvisningarna i fakturafilen (PDF).

Om beloppet på fakturan är lägre än tröskelbeloppet för din valuta kan du göra en engångsbetalning i Microsoft Azure-portalen med ett kredit- eller debetkort via **Betala nu**. Om beloppet på fakturan överskrider tröskelvärdet kan du inte betala med ett kredit- eller debetkort. Du kan se tröskelbeloppet för din valuta i Microsoft Azure-portalen genom att välja **Betala nu**.

## <a name="pay-now-in-the-azure-portal"></a>Betala nu i Azure-portalen

För att kunna betala för fakturor på Azure-portalen måste du ha rätt [MCA-behörigheter](../manage/understand-mca-roles.md) eller vara faktureringskontoadministratör. Faktureringskontoadministratören är den användare som ursprungligen registrerade sig för MCA-kontot.

1. Logga in på [Azure-portalen](https://portal.azure.com).
1. Sök efter **Kostnadshantering och fakturering**.
1. Välj **Fakturor** under **Fakturering** i den vänstra menyn.
1. Om någon av dina fakturor förfaller eller har förfallit visas en blå **Betala nu**-länk för fakturan. Välj **Betala nu**.
1. I fönstret Betala nu klickar du på **Välj en betalningsmetod** för att välja ett befintligt kreditkort eller lägga till ett nytt.
1. När du har valt en betalningsmetod väljer du **Betala nu**.

Fakturans status visar *betald* inom 24 timmar.

## <a name="pay-now-for-customers-in-india"></a>Betala nu för kunder i Indien

Reserv banken i Indien utfärdade [nya förordningar](https://www.rbi.org.in/Scripts/NotificationUser.aspx?Id=12002&Mode=0) som börjar gälla den 1 april 2021. Efter det här datumet kan banker i Indien börja tacka automatiska återkommande betalningar, och betalningar måste göras manuellt i Azure Portal.

Om din bank avböjer en automatisk återkommande betalning meddelar vi dig via e-post och ger instruktioner om hur du går vidare.

Från och med 1 april 2021 kan du betala en utestående balans när som helst genom att följa dessa steg: 

1. Logga in på [Azure-portalen](https://portal.azure.com/) som kontoadministratör.
1. Sök efter **Kostnadshantering + fakturering**.
1. På sidan Översikt väljer du knappen **betala nu** . (Om du inte ser knappen **betala nu** har du inte ett utestående saldo.)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Kontrollera åtkomsten till ett Microsoft-kundavtal
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Nästa steg

- Information om hur du blir berättigad att betala med check/banköverföring finns i [Så betalar du med faktura](../manage/pay-by-invoice.md)
