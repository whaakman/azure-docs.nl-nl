---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe Creative Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe Creative Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2018
ms.author: jeedes
ms.openlocfilehash: 506f52faf916aa0d5ca2e8587bdbcc16ab88e130
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054281"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Adobe Creative Cloud

In deze zelfstudie leert u over het integreren van Adobe Creative Cloud met Azure Active Directory (Azure AD).

Integratie van Adobe Creative Cloud met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Adobe Creative Cloud heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Adobe Creative Cloud (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Adobe Creative Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Adobe Creative Cloud eenmalige aanmelding ingeschakeld abonnement
- Een Adobe Creative Cloud Enterprise-versie vereist

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adobe Creative Cloud uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Adobe Creative Cloud uit de galerie toe te voegen

Voor het configureren van de integratie van Adobe Creative Cloud in Azure AD, moet u Adobe Creative Cloud uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Adobe Creative Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Adobe Creative Cloud**, selecteer **Adobe Creative Cloud** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Adobe Creative Cloud in de lijst met resultaten](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Adobe Creative Cloud op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Adobe Creative Cloud is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Adobe Creative Cloud tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Adobe Creative Cloud, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Adobe Creative Cloud](#create-an-adobe-creative-cloud-test-user)**  : als u wilt een equivalent van Britta Simon in Adobe Creative Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Adobe Creative Cloud.

**Voor het configureren van Azure AD eenmalige aanmelding met Adobe Creative Cloud, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Adobe Creative Cloud** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_samlbase.png)

3. Op de **Adobe Creative Cloud domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![Adobe Creative Cloud domein en URL's, eenmalige aanmelding informatie](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.okta.com/saml2/service-provider/<token>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [Adobe Creative Cloud Enterprise](https://www.adobe.com/au/creativecloud/business/teams/plans.html) om deze waarden te verkrijgen.

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Adobe Creative Cloud domein en URL's, eenmalige aanmelding informatie](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de waarde als: `https://adobe.com`

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_certificate.png)
     
6. Adobe Creative Cloud-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerk** tabblad van de toepassing. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/adobe-creative-cloud-tutorial/tutorial_attribute.png)

7. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| ----------------|
    | FirstName |User.givenName |
    | LastName |User.surname |
    | Email |User.mail |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/adobe-creative-cloud-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/adobe-creative-cloud-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.

    > [!NOTE]
    > Gebruikers moeten beschikken over een geldige licentie aan Office 365 ExO voor e-mailbericht claimwaarde te worden gevuld in het SAML-antwoord.

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobe-creative-cloud-tutorial/tutorial_general_400.png)

9. Op de **Adobe Creative Cloudconfiguratie** sectie, klikt u op **Adobe Creative Cloud configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie**.

    ![Configuratie van Adobe Creative Cloud](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_configure.png)

10. In een ander browservenster aanmelden bij [Adobe-beheerconsole](https://adminconsole.adobe.com) als beheerder.

11. Ga naar **instellingen** in de bovenste navigatiebalk van de balk en kies vervolgens **identiteit**. De lijst met domeinen wordt geopend. Klik op **configureren** koppeling op basis van uw domein. Voer de volgende stappen uit op **eenmalige aanmelding op configuratie vereist** sectie. Zie voor meer informatie, [instellen van een domein](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![Instellingen voor](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "instellingen")

    a. Klik op **Bladeren** voor het uploaden van het gedownloade certificaat uit Azure AD naar **IDP certificaat**.

    b. In de **IDP verlener** tekstvak plaatst de waarde van **SAML entiteit-Id** die u hebt gekopieerd uit **aanmelding configureren** sectie in Azure portal.

    c. In de **IDP aanmeldings-URL** tekstvak plaatst de waarde van **SAML SSO Service URL** die u hebt gekopieerd uit **aanmelding configureren** sectie in Azure portal.

    d. Selecteer **HTTP - omleiding** als **IDP Binding**.

    e. Selecteer **e-mailadres** als **aanmelding gebruikersinstelling**.

    f. Klik op **opslaan** knop.

12. Het dashboard wordt nu het XML-bestand aanwezig **"Metagegevens downloaden"** bestand. Het bevat van Adobe EntityDescriptor URL en AssertionConsumerService URL. Open het bestand en ze configureren in de Azure AD-toepassing.

    ![Single Sign-On aan App configureren](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Gebruik de waarde van de EntityDescriptor Adobe geleverd, kunt u voor **id** op de **App-instellingen configureren** dialoogvenster.

    b. Gebruik de waarde van de AssertionConsumerService Adobe geleverd, kunt u voor **antwoord-URL** op de **App-instellingen configureren** dialoogvenster.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/adobe-creative-cloud-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/adobe-creative-cloud-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/adobe-creative-cloud-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/adobe-creative-cloud-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-adobe-creative-cloud-test-user"></a>Maak een testgebruiker Adobe Creative Cloud

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Adobe Creative Cloud, moeten ze worden ingericht voor Adobe Creative Cloud. In het geval van Adobe Creative Cloud, met de inrichting is een handmatige taak.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Aanmelden bij [Adobe-beheerconsole](https://adminconsole.adobe.com) site als beheerder.

2. De gebruiker binnen de console van Adobe toevoegen als federatieve-ID en deze toewijzen aan een Product-profiel. Zie voor gedetailleerde informatie over het toevoegen van gebruikers [gebruikers toevoegen in Adobe-beheerconsole](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Op dit moment, typ uw e-mailadres/upn in de vorm van Adobe aanmelding, druk op tab en u moet worden gefedereerd terug naar Azure AD:
    * Webtoegang: www.adobe.com > aanmelden
    * In het hulpprogramma voor desktop-app > aanmelden
    * In de toepassing > help > aanmelden

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Adobe Creative Cloud.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon in Adobe Creative Cloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Adobe Creative Cloud**.

    ![De Adobe Creative Cloud-koppeling in de lijst met toepassingen](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Adobe Creative Cloud in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Adobe Creative Cloud.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Instellen van een domein (adobe.com)](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
* [Azure configureren voor gebruik met Adobe eenmalige aanmelding (adobe.com)](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)

<!--Image references-->

[1]: ./media/adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/adobe-creative-cloud-tutorial/tutorial_general_203.png
