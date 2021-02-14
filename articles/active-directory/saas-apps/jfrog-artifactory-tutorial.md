---
title: 'Självstudie: Azure Active Directory integration med JFrog-artefakter | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory-och JFrog-artefakter.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.openlocfilehash: f0fafa5c0cc2e0b1bf0f4e11db3265824feb5296
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100374717"
---
# <a name="tutorial-integrate-jfrog-artifactory-with-azure-active-directory"></a>Självstudie: integrera JFrog-artefakter med Azure Active Directory

I den här självstudien får du lära dig hur du integrerar JFrog-artefakter med Azure Active Directory (Azure AD). När du integrerar JFrog-artefakter med Azure AD kan du:

* Kontroll i Azure AD som har åtkomst till JFrog-artefakter.
* Gör det möjligt för användarna att logga in automatiskt för att JFrog artefakter med sina Azure AD-konton.
* Hantera dina konton på en central plats – Azure Portal.

Mer information om SaaS app integration med Azure AD finns i [Vad är program åtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

För att komma igång behöver du följande objekt:

* En Azure AD-prenumeration. Om du inte har någon prenumeration kan du få ett [kostnads fritt konto](https://azure.microsoft.com/free/).
* JFrog-prenumeration med enkel inloggning (SSO) för artefakter.

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du Azure AD SSO i en test miljö.

* JFrog-artefakter stöder **SP-och IDP** -INITIERAd SSO
* JFrog-artefakter stöder **just-in-Time** User-etablering

## <a name="adding-jfrog-artifactory-from-the-gallery"></a>Lägga till JFrog artefakter från galleriet

Om du vill konfigurera integrationen av JFrog-artefakter i Azure AD måste du lägga till JFrog-artefakter från galleriet i listan över hanterade SaaS-appar.

1. Logga in på [Azure Portal](https://portal.azure.com) med antingen ett arbets-eller skol konto eller en personlig Microsoft-konto.
1. I det vänstra navigerings fönstret väljer du tjänsten **Azure Active Directory** .
1. Navigera till **företags program** och välj sedan **alla program**.
1. Välj **nytt program** om du vill lägga till ett nytt program.
1. I avsnittet **Lägg till från galleriet** , Skriv **JFrog artefakter** i sökrutan.
1. Välj **JFrog artefakter** från resultat panelen och Lägg sedan till appen. Vänta några sekunder medan appen läggs till i din klient organisation.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

Konfigurera och testa Azure AD SSO med JFrog-artefakter med hjälp av en test användare som heter **B. Simon**. För att SSO ska fungera måste du upprätta en länk relation mellan en Azure AD-användare och den relaterade användaren i JFrog-artefakter.

Om du vill konfigurera och testa Azure AD SSO med JFrog-artefakter slutför du följande Bygg stenar:

1. **[Konfigurera Azure AD SSO](#configure-azure-ad-sso)** – så att användarna kan använda den här funktionen.
2. **[Konfigurera JFrog-artefakt för enkel inloggning](#configure-jfrog-artifactory-sso)** – om du vill konfigurera enskilda Sign-On inställningar på program sidan.
3. **[Skapa en Azure AD-test](#create-an-azure-ad-test-user)** för att testa enkel inloggning med Azure AD med B. Simon.
4. **[Tilldela Azure AD-testuser](#assign-the-azure-ad-test-user)** -för att aktivera B. Simon för att använda enkel inloggning med Azure AD.
5. **[Skapa JFrog artefakt test User](#create-jfrog-artifactory-test-user)** – om du vill ha en motsvarighet till B. Simon i JFrog-artefakt som är länkad till Azure AD-representation av användare.
6. **[Testa SSO](#test-sso)** – för att kontrol lera om konfigurationen fungerar.

### <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD SSO

Följ de här stegen för att aktivera Azure AD SSO i Azure Portal.

1. I [Azure Portal](https://portal.azure.com/)på sidan för **JFrog artefakt** integrerings program kan du hitta avsnittet **Hantera** och välja **enkel inloggning**.
1. På sidan **Välj metod för enkel inloggning** väljer du **SAML**.
1. På sidan **Konfigurera en enskild Sign-On med SAML** klickar du på ikonen Redigera/penna för **grundläggande SAML-konfiguration** för att redigera inställningarna.

   ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

1. I avsnittet **grundläggande SAML-konfiguration** , om du vill konfigurera programmet i **IDP** initierat läge, anger du värdena för följande fält:

    a. I text rutan **identifierare** anger du en URL med hjälp av följande mönster: `<servername>.jfrog.io`

    b. I textrutan **Svars-URL** skriver du in en URL med följande mönster:
    
    - För artefakter 6. x: `https://<servername>.jfrog.io/artifactory/webapp/saml/loginResponse`
    - För artefakt 7. x: `https://<servername>.jfrog.io/<servername>/webapp/saml/loginResponse`

1. Klicka på **Ange ytterligare URL:er** och gör följande om du vill konfigurera appen i **SP**-initierat läge:

    I textrutan **Inloggnings-URL** skriver du in en URL med följande mönster:
    - För artefakter 6. x: `https://<servername>.jfrog.io/<servername>/webapp/`
    - För artefakt 7. x: `https://<servername>.jfrog.io/ui/login`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera värdena med den faktiska identifieraren, svars-URL och inloggnings-URL. Kontakta [JFrog](https://support.jfrog.com) för att hämta de här värdena. Du kan även se mönstren som visas i avsnittet **Grundläggande SAML-konfiguration** i Azure-portalen.

1. JFrog-artefakt programmet förväntar sig SAML-intyg i ett särskilt format, vilket kräver att du lägger till anpassade mappningar av attribut i konfigurationen för SAML-token. I följande skärmbild visas listan över standardattribut. Klicka på ikonen **Redigera** för att öppna dialog rutan användarattribut.

    ![Skärm bild som visar användarattribut med redigerings kontrollen som kallas.](common/edit-attribute.png)

1. Förutom ovanstående förväntar JFrog artefakter ett antal ytterligare attribut som ska skickas tillbaka i SAML-svaret. I avsnittet **användarattribut &-anspråk** i dialog rutan **grupp anspråk (förhands granskning)** utför du följande steg:

    a. Klicka på **pennan** bredvid **grupper som returneras i anspråk**.

    ![Skärm bild som visar användarattribut & anspråk med redigerings ikonen vald.](./media/jfrog-artifactory-tutorial/config04.png)

    ![Skärm bild som visar avsnittet grupp anspråk med alla grupper markerade.](./media/jfrog-artifactory-tutorial/config05.png)

    b. Välj **alla grupper** i alternativ listan.

    c. Klicka på **Spara**.

4. På sidan **Konfigurera enskilda Sign-On med SAML** , i avsnittet SAML- **signeringscertifikat** , letar du upp **certifikatet (base64)** och väljer **Hämta** för att ladda ned certifikatet och spara det på datorn.

    ![Länk för nedladdning av certifikatet](./media/jfrog-artifactory-tutorial/certificate-base.png)

6. Konfigurera artefakten (SAML service Providerns namn) med fältet "ID" (se steg 4). I avsnittet **Konfigurera JFrog artefakter** kopierar du lämpliga URL: er baserat på ditt krav.

   - För artefakter 6. x: `https://<servername>.jfrog.io/artifactory/webapp/saml/loginResponse` 
   - För artefakt 7. x: `https://<servername>.jfrog.io/<servername>/webapp/saml/loginResponse`

    ![Kopiera konfigurations-URL:er](common/copy-configuration-urls.png)

### <a name="configure-jfrog-artifactory-sso"></a>Konfigurera JFrog-artefakter för enkel inloggning

Allt du behöver för att konfigurera enkel inloggning på JFrog- **artefakten** kan konfigureras av artefakt administratören på SAML configugration-skärmen.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

I det här avsnittet ska du skapa en test användare i Azure Portal som kallas B. Simon.

1. I den vänstra rutan i Azure Portal väljer du **Azure Active Directory**, väljer **användare** och väljer sedan **alla användare**.
1. Välj **ny användare** överst på skärmen.
1. I **användar** egenskaperna följer du de här stegen:
   1. I **Namn**-fältet skriver du `B.Simon`.  
   1. I fältet **användar namn** anger du username@companydomain.extension . Till exempel `B.Simon@contoso.com`.
   1. Markera kryssrutan **Visa lösenord** och skriv sedan ned det värde som visas i rutan **Lösenord**.
   1. Klicka på **Skapa**.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändaren

I det här avsnittet ska du aktivera B. Simon för att använda enkel inloggning med Azure genom att bevilja åtkomst till JFrog-artefakter.

1. I Azure Portal väljer du **företags program** och väljer sedan **alla program**.
1. I listan program väljer du **JFrog artefakter**.
1. På sidan Översikt för appen letar du reda på avsnittet **Hantera** och väljer **användare och grupper**.

   ![Länken ”Användare och grupper”](common/users-groups-blade.png)

1. Välj **Lägg till användare** och välj sedan **användare och grupper** i dialog rutan **Lägg till tilldelning** .

    ![Länken Lägg till användare](common/add-assign-user.png)

1. I dialog rutan **användare och grupper** väljer du **B. Simon** från listan användare och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Om du förväntar dig ett roll värde i SAML Assertion, i dialog rutan **Välj roll** , väljer du lämplig roll för användaren i listan och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Klicka på knappen **tilldela** i dialog rutan **Lägg till tilldelning** .

### <a name="create-jfrog-artifactory-test-user"></a>Skapa JFrog artefakt test användare

I det här avsnittet skapas en användare som kallas B. Simon i JFrog-artefakt. JFrog-artefakter har stöd för just-in-Time-etablering, som är aktiverat som standard. Det finns inget åtgärdsobjekt för dig i det här avsnittet. Om en användare inte redan finns i JFrog-artefakten skapas en ny efter autentiseringen.

### <a name="test-sso"></a>Testa SSO 

I det här avsnittet testar du konfigurationen för enkel inloggning Azure AD med hjälp av åtkomstpanelen.

När du klickar på panelen JFrog artefakter på åtkomst panelen, bör du loggas in automatiskt till den JFrog-artefakt som du ställer in SSO för. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ytterligare resurser

- [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](./tutorial-list.md)

- [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Vad är villkorlig åtkomst i Azure Active Directory?](../conditional-access/overview.md)