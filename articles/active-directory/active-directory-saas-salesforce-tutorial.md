---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: ed127afbca5135ade21f6ac53d18d46e88939fd9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce

In deze zelfstudie leert u hoe Salesforce integreren met Azure Active Directory (Azure AD).

Salesforce integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Salesforce heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce uit de galerie toevoegen
Voor het configureren van de integratie van Salesforce in Azure AD, moet u Salesforce uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Salesforce**, selecteer **Salesforce** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SalesForce in de lijst met resultaten](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Salesforce op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Salesforce is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Salesforce tot stand worden gebracht.

Wijs in Salesforce, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Salesforce, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Salesforce](#create-a-salesforce-test-user)**  - Salesforce die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Salesforce-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Op de **Salesforce-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![SalesForce-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)
    
    a. In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:
    
    Account voor de onderneming:`https://<subdomain>.my.salesforce.com`

    Developer-account:`https://<subdomain>-dev-ed.my.salesforce.com`
    
    b. In de **id** textbox, typ de waarde met het volgende patroon volgen:
    
    Account voor de onderneming:`https://<subdomain>.my.salesforce.com`

    Developer-account:`https://<subdomain>-dev-ed.my.salesforce.com`
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met [Salesforce Client ondersteuningsteam](https://help.salesforce.com/support) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Op de **Salesforce configuratie** sectie, klikt u op **configureren Salesforce** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![SalesForce-configuratie](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 

7. Open een nieuw tabblad in de browser en meld u aan op uw Salesforce-beheerdersaccount.

8. Klik op de **Setup** onder **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/configure1.png)

9. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster, klikt u op **identiteit** uitbreiden van de bijbehorende sectie. Klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

10. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)
    
    > [!NOTE]
    > Als u nog geen Single Sign-On-instellingen voor uw Salesforce-account inschakelen, moet u mogelijk contact opnemen met [Salesforce Client ondersteuningsteam](https://help.salesforce.com/support). 

11. Selecteer **SAML ingeschakeld**, en klik vervolgens op **opslaan**.

      ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
12. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

13. Op de **SAML Single Sign-On instelling bewerken** maken de volgende configuraties:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. Voor de **naam** veld, typ een beschrijvende naam voor deze configuratie. Om een waarde voor **naam** automatisch invullen de **API-naam** textbox.

    b. In de **verlener** veld, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.

    c. In de **tekstvak voor de entiteit-Id**, typ de naam van uw Salesforce-domein met het volgende patroon volgen:
      
      * Account voor de onderneming:`https://<subdomain>.my.salesforce.com`
      * Developer-account:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Voor het uploaden van de **Provider identiteitscertificaat**, klikt u op **bestand kiezen** om te bladeren en selecteer het certificaatbestand dat u hebt gedownload vanuit Azure-portal.

    e. Als **SAML identiteitstype**, kies een van de volgende opties:
    
      * Selecteer **Assertion bevat de gebruikersnaam van de Salesforce**als van de gebruiker Salesforce Username wordt doorgegeven in SAML-verklaring

      * Selecteer **Assertion bevat de Federation-ID van het gebruikersobject**als Federation-ID van het gebruikersobject wordt doorgegeven in SAML-verklaring

      * Selecteer **Assertion bevat de ID van het gebruik van het gebruikersobject**als gebruikers-ID van het gebruikersobject wordt doorgegeven in SAML-verklaring

    f. Voor **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentifier van het onderwerp overzicht**.

    g. Voor **Provider geïnitieerd aanvragen servicebinding**, selecteer **HTTP-omleiding**.

    h. In **identiteit Provider aanmeldings-URL** textbox, plak de waarde van **Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal
    
    ik. Tot slot op **opslaan** instellingen toe te passen uw SAML eenmalige aanmelding.

14. Klik op het linkernavigatiedeelvenster in Salesforce **Bedrijfsinstellingen** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

15. Schuif omlaag naar de **verificatieconfiguratie** uit en klik op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

16. In de **verificatieconfiguratie** sectie, Controleer de **aanmeldingspagina** als **verificatie gevolgd** van uw SAML SSO-configuratie en klik vervolgens op  **Sla**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Als meer dan één authentication-service is ingeschakeld, worden gebruikers gevraagd om te selecteren welke verificatieservice die ze aanmelden willen tijdens de initialisatie van eenmalige aanmelding met uw Salesforce-omgeving. Als u niet dat dit wilt gebeurt, dan u moet **laat alle andere verificatieservices uitgeschakeld**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-salesforce-test-user"></a>Een testgebruiker Salesforce maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce. SalesForce ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.
Er is geen actie-item voor u in deze sectie. Als een gebruiker in Salesforce nog niet bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot Salesforce.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Salesforce.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Salesforce, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Salesforce**.

    ![De Salesforce-koppeling in de lijst met toepassingen](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Salesforce-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Salesforce-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

