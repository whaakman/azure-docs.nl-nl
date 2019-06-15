---
title: 'Zelfstudie: Azure Active Directory-integratie met iLMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iLMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b2e012a4ce8ac4a9a5afb895d545beb0a0b8946
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100618"
---
# <a name="tutorial-integrate-ilms-with-azure-active-directory"></a>Zelfstudie: ILMS integreren met Azure Active Directory

In deze zelfstudie leert u hoe u iLMS integreert met Azure Active Directory (Azure AD). Wanneer u iLMS met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot iLMS heeft.
* Kunnen uw gebruikers worden automatisch aangemeld iLMS met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* ingeschakeld abonnement iLMS eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. biedt ondersteuning voor iLMS **SP en IDP** gestart door SSO

## <a name="adding-ilms-from-the-gallery"></a>ILMS uit de galerie toe te voegen

Voor het configureren van de integratie van iLMS in Azure AD, moet u iLMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **iLMS** in het zoekvak in.
1. Selecteer **iLMS** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met iLMS met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iLMS vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met iLMS, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van eenmalige aanmelding iLMS](#configure-ilms-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker iLMS](#create-ilms-test-user)**  : als u wilt een equivalent van Britta Simon in iLMS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **iLMS** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina als u wilt configureren van de toepassing in **IDP** gestart modus, voer de waarden voor de volgende velden:

    a. In de **id** in het tekstvak, plak de **id** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS.

    b. In de **antwoord-URL** in het tekstvak, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in het volgende patroon van iLMS-beheerportal `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In de **aanmeldings-URL** in het tekstvak, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS als `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

1. Om in te schakelen JIT wordt ingericht, wordt uw toepassing iLMS de SAML-asserties ondertekend verwacht in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

    > [!NOTE]
    > U moet inschakelen **Un-recognized-gebruikersaccount maken** in iLMS om toe te wijzen deze kenmerken. Volg de instructies [hier](https://support.inspiredelearning.com/help/adding-updating-and-managing-users#just-in-time-provisioning-with-saml-single-signon) voor een beter beeld van de configuratie van de kenmerken.

1. Bovendien hierboven verwacht iLMS toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | --------|------------- |
    | deling | user.department |
    | regio | user.state |
    | Afdeling | user.jobtitle |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de **iLMS instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-ilms-sso"></a>ILMS eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw **iLMS-beheerportal** als beheerder.

2. Klik op **SSO:SAML** onder **instellingen** tabblad SAML-instellingen openen en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/1.png)

3. Vouw de **serviceprovider** sectie en kopieert u de **id** en **eindpunt (URL)** waarde.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/2.png) 

4. Onder **id-Provider** sectie, klikt u op **metagegevens importeren**.

5. Selecteer de **Federatiemetagegevens** bestand gedownload vanuit de Azure-portal van de **SAML-handtekeningcertificaat** sectie.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png)

6. Als u inschakelen wilt voor het maken van accounts iLMS voor ongedaan maken inrichting JIT-gebruikers herkennen, volgt u onderstaande stappen te volgen:

    a. Controleer **Account maken voor niet-herkende gebruiker**.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

    b. De kenmerken in Azure AD met de kenmerken in iLMS toewijzen. Geef de naam van de kenmerken of de standaardwaarde is opgegeven in de kolom kenmerk.

    c. Ga naar **bedrijfsregels** tabblad en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/5.png)

    d. Controleer **Un-recognized regio's maken, afdelingen en afdelingen** om regio's, afdelingen en afdelingen die nog niet bestaan op het moment van eenmalige aanmelding te maken.

    e. Controleer **Update gebruiker profiel tijdens aanmelding** om op te geven of profiel van de gebruiker is bijgewerkt met elke eenmalige aanmelding.

    f. Als de **lege waarden van de Update voor niet verplichte velden in het gebruikersprofiel** optie is ingeschakeld, optioneel profiel velden die leeg bij het aanmelden wordt zijn ook voor zorgen dat iLMS profiel van de gebruiker bevatten lege waarden voor deze velden.

    g. Controleer **fout meldingse-mail verzenden** en voer het e-mailadres van de gebruiker waar u de fout e-mailmelding ontvangen.

7. Klik op **Save** om de instellingen op te slaan.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `Britta Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iLMS.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **iLMS**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ilms-test-user"></a>ILMS testgebruiker maken

Toepassing ondersteunt Just in time gebruikersinrichting en na-verificatiegebruikers automatisch in de toepassing gemaakt worden. JIT werkt als u hebt geklikt op de **Un-recognized-gebruikersaccount maken** selectievakje tijdens de SAML-configuratie-instelling op iLMS-beheerportal.

Als u een gebruiker handmatig hebt gemaakt wilt, volgt u onderstaande stappen te volgen:

1. Meld u aan uw bedrijf iLMS site als een beheerder.

2. Klik op **gebruiker registreren** onder **gebruikers** tabblad openen **gebruiker registreren** pagina.

   ![Werknemer toevoegen](./media/ilms-tutorial/3.png)

3. Op de **gebruiker registreren** pagina, de volgende stappen uitvoeren.

    ![Werknemer toevoegen](./media/ilms-tutorial/create_testuser_add.png)

    a. In de **voornaam** tekstvak, de eerste naam zoals Julia.

    b. In de **achternaam** tekstvak typt u de achternaam, zoals Simon.

    c. In de **E-mail-ID** tekstvak, typ het e-mailadres van de gebruiker, zoals BrittaSimon@contoso.com.

    d. In de **regio** vervolgkeuzelijst, selecteer de waarde voor de regio.

    e. In de **deling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    f. In de **afdeling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    g. Klik op **Opslaan**.

    > [!NOTE]
    > U kunt registratie-e-mail naar gebruiker verzenden door te selecteren **registratie E-mail** selectievakje.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel iLMS in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de iLMS waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
