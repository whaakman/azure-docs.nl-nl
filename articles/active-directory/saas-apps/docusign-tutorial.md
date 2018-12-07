---
title: 'Zelfstudie: Azure Active Directory-integratie met DocuSign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: 6b2b5f72c9520498d4834bbbfaf6c56656a807e7
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015211"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Zelfstudie: Azure Active Directory-integratie met DocuSign

In deze zelfstudie leert u hoe DocuSign integreren met Azure Active Directory (Azure AD).

DocuSign integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot DocuSign heeft.
- U kunt uw gebruikers automatisch ophalen ondertekend in DocuSign (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met DocuSign, moet u de volgende items:

- Een Azure AD-abonnement
- Een DocuSign eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. DocuSign uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-docusign-from-the-gallery"></a>DocuSign uit de galerie toe te voegen

Voor het configureren van de integratie van DocuSign in Azure AD, moet u DocuSign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen DocuSign uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **DocuSign**, selecteer **DocuSign** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![DocuSign in de lijst met resultaten](./media/docusign-tutorial/tutorial_docusign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met DocuSign op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in DocuSign is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in DocuSign tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met DocuSign, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker DocuSign](#creating-a-docusign-test-user)**  : als u wilt een equivalent van Britta Simon in DocuSign die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing DocuSign.

**Voor het configureren van Azure AD eenmalige aanmelding met DocuSign, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **DocuSign** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![DocuSign-domein en URL's, eenmalige aanmelding informatie](./media/docusign-tutorial/tutorial_docusign_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en -id in die later wordt toegelicht **SAML 2.0-eindpunten weergeven** sectie in de zelfstudie.

5. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/docusign-tutorial/tutorial_docusign_certificate.png) 

6. Op de **DocuSign instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![DocuSign-configuratie](common/configuresection.png)

7. In een ander browservenster, meld u aan bij uw **DocuSign-beheerportal** als beheerder.

8. In de rechterbovenhoek klikt u rechts van de pagina op profiel **logo** en klik vervolgens op **gaat u naar de beheerder**.
  
    ![Eenmalige aanmelding configureren][51]

9. Klik op de pagina van uw domein-oplossingen op **domeinen**

    ![Eenmalige aanmelding configureren][50]

10. Onder de **domeinen** sectie, klikt u op **CLAIM domein**.

    ![Eenmalige aanmelding configureren][52]

11. Op de **claimen van een domein** dialoogvenster in de **domeinnaam** tekstvak typt u het domein van uw bedrijf, en klik vervolgens op **CLAIM**. Zorg ervoor dat u het domein controleren en de status actief is.

    ![Eenmalige aanmelding configureren][53]

12. Klik op de pagina van uw domein-oplossingen op **id-Providers**.
  
    ![Eenmalige aanmelding configureren][54]

13. Onder **id-Providers** sectie, klikt u op **id-PROVIDER toevoegen**. 

    ![Eenmalige aanmelding configureren][55]

14. Op de **instellingen van de identiteit** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren][56]

    a. In de **naam** tekstvak typt u een unieke naam voor uw configuratie. Gebruik geen spaties.

    b. In de **Identity Provider Issuer tekstvak**, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **afmeldings-URL van id-Provider** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    e. Selecteer **aanmelding AuthN aanvraag**.

    f. Als **AuthN verzenden aanvraag door**, selecteer **POST**.

    g. Als **afmeldingsaanvraag verzenden door**, selecteer **ophalen**.

    h. In de **toewijzing van aangepast kenmerk** sectie, klikt u op **nieuwe toewijzing toevoegen**.

    ![Eenmalige aanmelding configureren][62]

    i. Kies het veld dat u wilt toewijzen met Azure AD-Claim. In dit voorbeeld wordt de **emailaddress** claim is toegewezen door de waarde van **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Dit is de standaardnaam van de claim van Azure AD voor e-mailbericht claim en klik vervolgens op **opslaan**.

    ![Eenmalige aanmelding configureren][57]

    > [!NOTE]
    > Gebruik het juiste **gebruikers-id** om toe te wijzen van de gebruiker uit Azure AD DocuSign gebruiker toewijzen. Selecteer het juiste veld en voer de juiste waarde op basis van de instellingen van uw organisatie.

    j. In de **Identity Provider certificaten** sectie, klikt u op **certificaat toevoegen**, en upload het certificaat dat u hebt gedownload van Azure AD-portal en klikt u op **opslaan**.

    ![Eenmalige aanmelding configureren][58]

    k. In de **id-Providers** sectie, klikt u op **acties**, en klik vervolgens op **eindpunten**.

    ![Eenmalige aanmelding configureren][59]

    l. In de **SAML 2.0-eindpunten weergeven** sectie op **DocuSign-beheerportal**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren][60]

    * Kopiëren de **URL voor Service Provider-verlener**, en plak deze in de **id** -tekstvak in **DocuSign-domein en URL's** sectie in Azure portal.

    * Kopiëren de **aanmeldings-URL voor Service Provider**, en plak deze in de **aanmelding URL** -tekstvak in **DocuSign-domein en URL's** sectie in Azure portal.

    * Klik op **sluiten**

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-docusign-test-user"></a>Het maken van een testgebruiker DocuSign

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in DocuSign. DocuSign biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot DocuSign als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [DocuSign-ondersteuningsteam](https://support.docusign.com/).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan DocuSign.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **DocuSign**.

    ![Eenmalige aanmelding configureren](./media/docusign-tutorial/tutorial_docusign_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel DocuSign in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing DocuSign.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
