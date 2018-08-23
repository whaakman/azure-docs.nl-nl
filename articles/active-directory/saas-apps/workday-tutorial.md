---
title: 'Zelfstudie: Azure Active Directory-integratie met Workday | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workday.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: jeedes
ms.openlocfilehash: 0d3eb8560d034dc6a50b39a504d0912ee6d49e65
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42060846"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Zelfstudie: Azure Active Directory-integratie met Workday

In deze zelfstudie leert u hoe u Workday integreren met Azure Active Directory (Azure AD).

Workday integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workday heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workday (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workday, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkdag eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Workday uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workday-from-the-gallery"></a>Workday uit de galerie toe te voegen

Voor het configureren van de integratie van Workday in Azure AD, moet u Workday uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workday uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Workday**, selecteer **Workday** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![WorkDay in de lijst met resultaten](./media/workday-tutorial/tutorial_workday_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Workday op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Workday is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Workday tot stand worden gebracht.

In Workday, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workday, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Workday](#create-a-workday-test-user)**  : als u wilt een equivalent van Britta Simon in Workday die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Workday-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Workday, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Workday** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/workday-tutorial/tutorial_workday_samlbase.png)

3. Op de **Workday domein en URL's** sectie, voert u de volgende stappen uit:

    ![WorkDay domein en URL's eenmalige aanmelding informatie](./media/workday-tutorial/tutorial_workday_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. In de **id** tekstvak, een URL typen: `http://www.workday.com`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap:

    ![WorkDay domein en URL's eenmalige aanmelding informatie](./media/workday-tutorial/tutorial_workday_url1.png)

    In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke aanmeldings-URL en de antwoord-URL. Uw antwoord-URL bijvoorbeeld een subdomein moet hebben: www, wd2, wd3, wd3 impl, wd5, wd5 impl).
    > Met behulp van bijvoorbeeld '*http://www.myworkday.com*"werkt, maar '*http://myworkday.com*" niet bestaat. Neem contact op met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om deze waarden te verkrijgen.

5. De SAML-asserties ondertekend verwacht WorkDay toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze configuratie.

    ![Eenmalige aanmelding configureren](./media/Workday-tutorial/tutorial_workday_attributes.png)

    > [!NOTE]
    > Hier hebben we de naam-ID met de UPN (user.userprincipalname) toegewezen als standaard. Moet u de naam-ID met de werkelijke gebruikers-ID in uw Workday-account (uw e-mail, UPN, enzovoort) worden toegewezen voor geslaagde werken van eenmalige aanmelding.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/workday-tutorial/tutorial_workday_certificate.png)

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/workday-tutorial/tutorial_general_400.png)

8. Op de **Workday configuratie** sectie, klikt u op **configureren Workday** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratie van WorkDay](./media/workday-tutorial/tutorial_workday_configure.png)

9. In een ander browservenster aanmelden bij uw bedrijf Workday site als beheerder.

10. In de **zoekvak** zoeken met de naam van de **Tenant-instellingen bewerken: beveiliging** bovenaan de linkerkant van de startpagina.

    ![Beveiliging voor de Tenant bewerken](./media/workday-tutorial/IC782925.png "beveiliging voor de Tenant bewerken")

11. In de **omleiding van URL's** sectie, voert u de volgende stappen uit:

    ![Omleidings-URL's](./media/workday-tutorial/IC7829581.png "Omleidings-URL's")

    a. Klik op **rij toevoegen**.

    b. In de **aanmeldings-URL omleiden** tekstvak en de **Omleidings-URL mobiele** tekstvak, type de **aanmeldings-URL** u hebt opgegeven op de **Workday domein en URL's** sectie van de Azure portal.

    c. In de Azure-portal op de **aanmelding configureren** venster, Kopieer de **afmelding URL**, en plak deze in de **Omleidings-URL voor afmelden** tekstvak.

    d. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam van de.  

    >[!NOTE]
    > De waarde van het kenmerk van de omgeving is gekoppeld aan de waarde van de tenant-URL:  
    >-Als de domeinnaam van de tenant-URL voor Workday wordt gestart met impl bijvoorbeeld: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), wordt de **omgeving** kenmerk moet worden ingesteld op implementatie.  
    >-Als de domeinnaam wordt gestart met iets anders, moet u contact opnemen met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om op te halen van de overeenkomende **omgeving** waarde.

12. In de **SAML Setup** sectie, voert u de volgende stappen uit:

    ![SAML-Setup](./media/workday-tutorial/IC782926.png "SAML-instellingen")

    a.  Selecteer **SAML-verificatie inschakelen**.

    b.  Klik op **rij toevoegen**.

13. In de **SAML-id-Providers** sectie, voert u de volgende stappen uit:

    ![SAML-id-Providers](./media/workday-tutorial/IC7829271.png "SAML-id-Providers")

    a. In de **identiteit providernaam** tekstvak typt u de naam van een provider (bijvoorbeeld: *SPInitiatedSSO*).

    b. In de Azure-portal op de **aanmelding configureren** venster, Kopieer de **SAML entiteit-ID** waarde en plak deze in de **verlener** tekstvak.

    ![SAML-id-Providers](./media/workday-tutorial/IC7829272.png "SAML-id-Providers")

    c. In de Azure-portal op de **aanmelding configureren** venster, Kopieer de **afmelding URL** waarde en plak deze in de **antwoord-URL voor afmelden** tekstvak.

    d. In de Azure-portal op de **aanmelding configureren** venster, Kopieer de **Single Sign-On Service URL voor SAML** waarde en plak deze in de **IdP SSO Service URL** tekstvak.

    e. In **gebruikt voor omgevingen** textbox, selecteert u de omgevingsnaam van de.

    f. Klik op **openbare sleutel identiteitscertificaat-Provider**, en klik vervolgens op **maken**.

    ![Maak](./media/workday-tutorial/IC782928.png "maken")

    g. Klik op **x509 maken openbare sleutel**.

    ![Maak](./media/workday-tutorial/IC782929.png "maken")

14. In de **weergave x509 openbare sleutel** sectie, voert u de volgende stappen uit:

    ![De openbare sleutel weergave x509](./media/workday-tutorial/IC782930.png "weergave x509 openbare sleutel")

    a. In de **naam** tekstvak, typ een naam voor uw certificaat (bijvoorbeeld: *PPE\_SP*).

    b. In de **geldig vanaf** tekstvak typt u de geldige van kenmerkwaarde van uw certificaat.

    c.  In de **geldig tot** tekstvak typt u de geldig is voor het kenmerkwaarde van uw certificaat.

    > [!NOTE]
    > U kunt de geldige ophalen uit de datum en de geldig tot nu toe uit het gedownloade certificaat door erop te dubbelklikken.  De datums worden vermeld in de **Details** tabblad.
    >
    >

    d.  Het base-64 gecodeerde certificaat openen in Kladblok en kopieer vervolgens de inhoud ervan.

    e.  In de **certificaat** tekstvak, plak de inhoud van het Klembord.

    f.  Klik op **OK**.

15. Voer de volgende stappen uit:

    ![Configuratie van eenmalige aanmelding](./media/workday-tutorial/WorkdaySSOConfiguratio.png "SSO-configuratie")

    a.  In de **Service Provider-ID** tekstvak, type **http://www.workday.com**.

    b. Selecteer **niet nebo Deflate voor Serviceprovider geïnitieerde verificatieaanvraag**.

    c. Als **handtekening-methode voor netwerkverificatie-aanvragen**, selecteer **SHA256**.

    ![Methode voor netwerkverificatie aanvraag handtekening](./media/workday-tutorial/WorkdaySSOConfiguration.png "verificatiemethode aanvraag handtekening") 

    d. Klik op **OK**.

    ![OK](./media/workday-tutorial/IC782933.png "OK")

    > [!NOTE]
    > Controleer of dat u eenmalige aanmelding correct ingesteld. Als u eenmalige aanmelding met een onjuiste instelling inschakelt, kunt u mogelijk niet de toepassing met uw referenties invoeren en vergrendeld. In dit geval Workday biedt een back-aanmelden-url waar gebruikers kunnen zich aanmelden met hun gebruikelijke gebruikersnaam en wachtwoord in de volgende indeling: [uw Workday URL]/login.flex?redirect=n

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/workday-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/workday-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/workday-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/workday-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-workday-test-user"></a>Maak een testgebruiker Workday

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Workday. Werken met [Workday Client ondersteuningsteam](https://www.workday.com/en-us/partners-services/services/support.html) om toe te voegen de gebruikers in de Workday-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot Workday.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Workday, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Workday**.

    ![De Workday-koppeling in de lijst met toepassingen](./media/workday-tutorial/tutorial_workday_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de Workday-tegel in het toegangsvenster klikt, u moet u automatisch aangemeld bij uw Workday-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


<!--Image references-->

[1]: ./media/workday-tutorial/tutorial_general_01.png
[2]: ./media/workday-tutorial/tutorial_general_02.png
[3]: ./media/workday-tutorial/tutorial_general_03.png
[4]: ./media/workday-tutorial/tutorial_general_04.png

[100]: ./media/workday-tutorial/tutorial_general_100.png

[200]: ./media/workday-tutorial/tutorial_general_200.png
[201]: ./media/workday-tutorial/tutorial_general_201.png
[202]: ./media/workday-tutorial/tutorial_general_202.png
[203]: ./media/workday-tutorial/tutorial_general_203.png
