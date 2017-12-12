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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: 16b60342106c69cb9959aad539b794cb8003acda
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox

In deze zelfstudie leert u hoe Salesforce Sandbox integreren met Azure Active Directory (Azure AD).

Salesforce Sandbox integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Salesforce-Sandbox heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce Sandbox (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce Sandbox, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce Sandbox eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce Sandbox uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Salesforce Sandbox uit de galerie toevoegen
Voor het configureren van de integratie van Salesforce Sandbox in Azure AD, moet u Salesforce Sandbox uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce Sandbox uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Salesforce Sandbox**, selecteer **Salesforce Sandbox** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SalesForce Sandbox in de lijst met resultaten](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Salesforce-Sandbox op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Salesforce Sandbox is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Salesforce Sandbox tot stand worden gebracht.

Wijs in het Salesforce-Sandbox de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Salesforce Sandbox](#create-a-salesforce-sandbox-test-user)**  - Salesforce Sandbox die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Salesforce-Sandbox-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce Sandbox** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Op de **Salesforce sandbox-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![SalesForce sandbox-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    a. In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    b. In de **id** textbox, typ de waarde met het volgende patroon volgen:`https://<instancename>--Sandbox.<entityid>.my.salesforce.com`
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met [Salesforce Client ondersteuningsteam](https://help.salesforce.com/support) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_400.png)

6. Op de **Salesforce Sandbox configuratie** sectie, klikt u op **Salesforce Sandbox configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png) 

7. Een nieuw tabblad openen in uw browser en meld u aan uw Salesforce-Sandbox-beheerdersaccount.

8. Klik op de **Setup** onder **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/configure1.png)

9. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster, klikt u op **identiteit** uitbreiden van de bijbehorende sectie. Klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Selecteer **SAML ingeschakeld**, en klik vervolgens op **opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-enable-saml.png)

11. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-admin-sso-new.png)

12. Voer de volgende stappen uit in de sectie SAML Single Sign-On-instellingen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-saml-config.png)

    a. In de **naam** textbox, typ de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD_Test*). 

    b. In de **verlener** veld, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal

    c. In de **entiteit-Id** textbox type `https://<instancename>--Sandbox.<entityid>.my.salesforce.com` als dit het eerste exemplaar van Salesforce Sandbox die u aan uw directory toevoegen wilt. Als u al hebt toegevoegd een exemplaar van Salesforce-Sandbox vervolgens voor de **entiteit-ID** typt u in de **aanmelding op URL**, die moet worden in deze indeling:`https://<instancename>--Sandbox.<entityid>.my.salesforce.com`  
 
    d. Voor het uploaden van de **Provider identiteitscertificaat**, klikt u op **bestand kiezen** om te bladeren en selecteer het certificaatbestand dat u hebt gedownload vanuit Azure-portal.  

    e. Als **SAML identiteitstype**, kies een van de volgende opties:
    
      * Selecteer **Assertion bevat de gebruikersnaam van de Salesforce**als van de gebruiker Salesforce Username wordt doorgegeven in SAML-verklaring

      * Selecteer **Assertion bevat de Federation-ID van het gebruikersobject**als Federation-ID van het gebruikersobject wordt doorgegeven in SAML-verklaring

      * Selecteer **Assertion bevat de ID van het gebruik van het gebruikersobject**als gebruikers-ID van het gebruikersobject wordt doorgegeven in SAML-verklaring
 
    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentifier van het onderwerp overzicht**.

    g. Als **Provider geïnitieerd aanvragen servicebinding**, selecteer **HTTP POST**. 

    h. In **identiteit Provider aanmeldings-URL** textbox, plak de waarde van **Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal. 

    ik. SFDC biedt geen ondersteuning voor SAML-afmelden.  Als een tijdelijke oplossing te plakken `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` in de **identiteit Provider afmelding URL** textbox.

    j. Klik op **Opslaan**.

### <a name="enable-your-domain"></a>Uw domein inschakelen
Deze sectie wordt ervan uitgegaan dat u al hebt gemaakt een domein.  Zie voor meer informatie [definiëren van uw domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Als u wilt maken voor uw domein, moet u de volgende stappen uitvoeren:**

1. Klik op het linkernavigatiedeelvenster in Salesforce **Bedrijfsinstellingen** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein**.
   
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-my-domain.png)
   
   >[!NOTE]
   >Zorg dat uw domein correct is geconfigureerd. 

2. In de **verificatieconfiguratie** sectie, klikt u op **bewerken**, naarmate **verificatieservice**, selecteert u de naam van de SAML Single Sign-On-instelling van de vorige sectie en ten slotte op **opslaan**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-sandbox-tutorial/sf-edit-auth-config.png)

Als u een domein geconfigureerd hebt, moeten uw gebruikers de domein-URL voor aanmelding bij de sandbox Salesforce gebruiken.  

Als u de waarde van de URL, klikt u op de SSO-profiel dat u hebt gemaakt in de vorige sectie.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-salesforce-sandbox-test-user"></a>Maak een testgebruiker Salesforce Sandbox

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce Sandbox. SalesForce Sandbox ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.
Er is geen actie-item voor u in deze sectie. Als een gebruiker in Salesforce Sandbox nog niet bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot de Salesforce-Sandbox.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan de Salesforce Sandbox.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Salesforce Sandbox**.

    ![De Salesforce-Sandbox-koppeling in de lijst met toepassingen](./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Salesforce Sandbox in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Salesforce-Sandbox-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-sandbox-tutorial/tutorial_general_203.png

