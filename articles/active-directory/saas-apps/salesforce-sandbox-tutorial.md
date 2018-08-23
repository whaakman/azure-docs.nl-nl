---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: 6feafba41cf65a752dd5bf0819b0b93bacff0aff
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42054644"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce-Sandbox

In deze zelfstudie leert u hoe u Salesforce Sandbox integreren met Azure Active Directory (Azure AD).

Sandboxes bieden u de mogelijkheid om te maken van meerdere kopieën van uw organisatie in afzonderlijke omgevingen voor verschillende doeleinden, zoals ontwikkeling, testen en training, zonder verlies van gegevens en toepassingen in uw Salesforce-productie de organisatie.
Zie voor meer informatie, [sandbox-overzicht](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Salesforce-Sandbox integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Salesforce-Sandbox heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce-Sandbox (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce Sandbox, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce-Sandbox eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce-Sandbox uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Salesforce-Sandbox uit de galerie toe te voegen

Voor het configureren van de integratie van Salesforce Sandbox in Azure AD, moet u Salesforce-Sandbox uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce-Sandbox uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Salesforce Sandbox**, selecteer **Salesforce Sandbox** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SalesForce-Sandbox in de lijst met resultaten](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Salesforce-Sandbox op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Salesforce Sandbox is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Salesforce Sandbox tot stand worden gebracht.

In Salesforce-Sandbox, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Salesforce Sandbox, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Salesforce Sandbox](#create-a-salesforce-sandbox-test-user)**  : als u wilt een equivalent van Britta Simon in Salesforce-Sandbox die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Salesforce-Sandbox-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce Sandbox** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Op de **Salesforce sandbox-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

   ![SalesForce sandbox-domein en URL's, eenmalige aanmelding informatie](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

   In de **antwoord-URL** tekstvak typt u uw organisatie-specifieke **antwoord-URL**.

   > [!NOTE]
   > Werk de antwoord-URL-waarde met de werkelijke antwoord-URL die verderop in deze zelfstudie wordt uitgelegd.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(RAW)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

6. Op de **Salesforce Sandbox configuratie** sectie, klikt u op **configureren Salesforce Sandbox** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

7. Open een nieuw tabblad in uw browser en meld u aan bij uw Salesforce-Sandbox administrator-account.

8. Klik op de **Setup** onder **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure1.png)

9. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster links, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure3.png)

11. Selecteer **SAML ingeschakeld**, en klik vervolgens op **opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Op de **instellingen voor eenmalige aanmelding** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-saml-config1.png)

    a. Selecteer **SAML ingeschakeld** selectievakje.

    b. In de **verlener** veld, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    c. Het uploaden van de **Provider identiteitscertificaat**, klikt u op **Bladeren** om te bladeren en selecteer het certificaatbestand dat u hebt gedownload vanuit Azure portal.

    d. In **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.

    e. Als **SAML identiteitstype**, kies een van de volgende opties:

      * Selecteer **verklaring bevat de gebruikersnaam van de Salesforce**als van de gebruiker Salesforce Username wordt doorgegeven in de SAML-verklaring

      * Selecteer **verklaring bevat de Federation-ID van het gebruikersobject**, als de Federation-ID van het gebruikersobject wordt doorgegeven in de SAML-verklaring
  
    f. Als **SAML identiteit locatie**, selecteer **identiteit is een kenmerkelement**.

    g. SFDC biedt geen ondersteuning voor SAML-afmelding.  Als tijdelijke oplossing, plak `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` in de **afmeldings-URL van aangepaste** tekstvak.

    h. Klik op **Opslaan**.

14. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **metagegevens downloaden** knop.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure4.png)

15. De metagegevens van de gedownloade openen in een ander browservenster en kopieer de **locatie** waarde en plak deze in de **antwoord-URL** tekstvak op de **Salesforce sandbox-domein en URL's**sectie in Azure portal.  

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure5.png)

16. Als u wilt configureren van de toepassing in **SP** gestart modus, zijn de vereisten voor die:

    a. U moet een geverifieerd domein hebben.

    b. U wilt configureren en inschakelen van uw Salesforce-Sandbox-domein, de stappen voor deze verderop in deze zelfstudie worden beschreven.

    c. In de Azure-portal op de **Salesforce sandbox-domein en URL's** sectie selectievakje **geavanceerde URL-instellingen weergeven** en voer de volgende stap:
  
    ![SalesForce sandbox-domein en URL's, eenmalige aanmelding informatie](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Deze waarde moet worden gekopieerd van de Salesforce-Sandbox-portal nadat u het domein hebt ingeschakeld.

17. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(RAW)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

19. Op de **Salesforce Sandbox configuratie** sectie, klikt u op **configureren Salesforce Sandbox** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

20. Open een nieuw tabblad in uw browser en meld u aan bij uw Salesforce-Sandbox administrator-account.

21. Klik op de **Setup** onder **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure1.png)

22. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster links, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

23. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure3.png)

24. Selecteer **SAML ingeschakeld**, en klik vervolgens op **opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

25. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

26. Als u een tweede exemplaar toevoegt, moet u om in te schakelen van een domein, zoals we hierboven vermeld (SP geïnitieerde aanvraag). In het gedeelte SAML Single Sign-On-instellingen op de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

    a. In de **naam** tekstvak typt u de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD_Test*).

    b. In de **verlener** veld, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **entiteit-ID** tekstvak gebruik `https://test.salesforce.com` waarde voor de eerste instantie en van de tweede instantie van de toepassing kunt u de tenant-specifieke id-waarde.

    d. Het uploaden van de **Provider identiteitscertificaat**, klikt u op **bestand kiezen** om te bladeren en selecteer het certificaatbestand dat u hebt gedownload vanuit Azure portal.

    e. Als **SAML identiteitstype**, kies een van de volgende opties:

      * Selecteer **verklaring bevat de gebruikersnaam van de Salesforce**als van de gebruiker Salesforce Username wordt doorgegeven in de SAML-verklaring

      * Selecteer **verklaring bevat de Federation-ID van het gebruikersobject**, als de Federation-ID van het gebruikersobject wordt doorgegeven in de SAML-verklaring

      * Selecteer **verklaring bevat de ID van het gebruik van het gebruikersobject**, als gebruikers-ID van het gebruikersobject wordt doorgegeven in de SAML-verklaring

    f. Als **SAML identiteit locatie**, selecteer **identiteit is in de NameIdentifier-element van het onderwerp overzicht**.

    g. Als **Service Provider gestart aanvragen Binding**, selecteer **HTTP POST**.

    h. In **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.

    i. SFDC biedt geen ondersteuning voor SAML-afmelding.  Als tijdelijke oplossing, plak `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` deze in de **afmeldings-URL van aangepaste** tekstvak.

    j. Klik op **Opslaan**.

27. Als u wilt inschakelen in uw Salesforce-Sandbox-domein, moet u de volgende stappen uitvoeren:

    > [!NOTE]
    > Voordat het inschakelen van het domein moet u hetzelfde maken op de Salesforce-Sandbox. Zie voor meer informatie, [definiëren van uw domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Zodra het domein is gemaakt, zorg ervoor dat deze correct geconfigureerd.

    * Klik in het linkernavigatiedeelvenster in Salesforce-Sandbox op **Bedrijfsinstellingen** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein**.

         ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

    * In de **verificatieconfiguratie** sectie, klikt u op **bewerken**.

        ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

    * In de **verificatieconfiguratie** sectie als **verificatieservice**, selecteert u de naam van de SAML Single Sign-On-instelling die u hebt ingesteld tijdens het configureren van eenmalige aanmelding in Sandbox met Salesforce en Klik op **opslaan**.

        ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/salesforce-sandbox-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-salesforce-sandbox-test-user"></a>Maak een testgebruiker Salesforce-Sandbox

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce Sandbox. SalesForce-Sandbox biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Sandbox met Salesforce bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te krijgen tot de Salesforce-Sandbox. SalesForce-Sandbox biedt ook ondersteuning voor automatisch inrichten van gebruikers, vindt u meer details [hier](salesforce-sandbox-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon verleent toegang tot Salesforce-Sandbox gebruiken Azure eenmalige aanmelding.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon naar Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Salesforce Sandbox**.

    ![De Salesforce-Sandbox-koppeling in de lijst met toepassingen](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Salesforce-Sandbox-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Salesforce-Sandbox-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](salesforce-sandbox-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png