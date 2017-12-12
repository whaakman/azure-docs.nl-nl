---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Jira door resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO voor Jira door resolutie GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 3a3224acd2376efca19a29576980b6a3ca6a9e99
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Jira door resolutie GmbH

In deze zelfstudie leert u het integreren van SAML SSO voor Jira door resolutie GmbH met Azure Active Directory (Azure AD).

Integratie van SAML SSO voor Jira door resolutie GmbH met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAML SSO voor Jira door resolutie GmbH heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAML SSO voor Jira door resolutie GmbH (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML SSO voor Jira door resolutie GmbH, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAML SSO voor Jira door resolutie GmbH eenmalige aanmelding voor ingeschakelde abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAML SSO voor Jira door resolutie GmbH uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a>SAML SSO voor Jira door resolutie GmbH uit de galerie toevoegen
Voor het configureren van de integratie van SAML SSO voor Jira door resolutie GmbH in Azure AD, moet u SAML SSO voor Jira door resolutie GmbH uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAML SSO voor Jira resolutie GmbH uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **SAML SSO voor Jira door resolutie GmbH**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. Selecteer in het deelvenster resultaten **SAML SSO voor Jira door resolutie GmbH**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAML SSO voor Jira door resolutie die GmbH op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in SAML SSO voor Jira door resolutie GmbH aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SAML SSO voor Jira door de resolutie GmbH moet tot stand worden gebracht.

Wijs in SAML SSO voor Jira door resolutie GmbH, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAML SSO voor Jira door resolutie GmbH, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SAML SSO voor Jira door resolutie GmbH testgebruiker](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  - bevatten een equivalent van Britta Simon SAML SSO voor Jira door resolutie GmbH die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw SAML SSO voor Jira configureren door de resolutie GmbH toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAML SSO voor Jira door resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAML SSO voor Jira door resolutie GmbH** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. Op de **SAML SSO voor Jira door resolutie GmbH domein en URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/samlsso`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/samlsso`

4. Controleer **weergeven geavanceerde instellingen voor URL**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [SAML SSO voor Jira door resolutie GmbH Client ondersteuningsteam](https://www.resolution.de/go/support) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster geopend, moet u zich aanmelden bij uw **SAML SSO voor Jira door resolutie GmbH beheerportal** als beheerder.

8. Beweeg de muisaanwijzer op het tandwiel en klik op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. U omgeleid naar de pagina toegang als beheerder. Voer de **wachtwoord** en klik op **bevestigen** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. Klik onder sectie tabblad invoegtoepassingen op **vinden van nieuwe invoegtoepassingen**. Search **SAML eenmalige aanmelding op (SSO) voor JIRA** en klik op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. De installatie van de invoegtoepassing wordt gestart. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. Klik op **configureren** voor het configureren van de nieuwe invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. Op **SAML-configuratie voor invoegtoepassing van SingleSignOn** pagina, klikt u op **toevoegen van nieuwe IdP** knop voor het configureren van de instellingen van de id-Provider.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. Op **Kies uw SAML-identiteitsprovider** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon5a.png)
 
    a. Stel **Azure AD** als de IdP-type.
    
    b. Voeg **naam** van de id-Provider (bijvoorbeeld Azure AD).
    
    c. Voeg **beschrijving** van de id-Provider (bijvoorbeeld Azure AD).
    
    d. Klik op **Volgende**.
    
16. Op **identiteit providerconfiguratie** pagina, klikt u op **volgende** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon5b.png)

17. Op **SAML IdP-metagegevens importeren** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon5c.png)

    a. Klik op **bestand laden** knop en kies Metadata XML-bestand die u in stap 5 hebt gedownload.

    b. Klik op **importeren** knop.
    
    c. Wacht even tot importeren is gelukt.
    
    d. Klik op **volgende** knop.
    
18. Op **gebruikers-ID-kenmerk en transformatie** pagina, klikt u op **volgende** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon5d.png)
    
19. Op **gebruiker gemaakt en update** pagina, klikt u op **opslaan & volgende** instellingen op te slaan.   
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon6a.png)
    
20. Op **testen van uw instellingen** pagina, klikt u op **test overslaan & handmatig configureren** test van de gebruiker nu overslaan. Dit wordt uitgevoerd in de volgende sectie en sommige instellingen in Azure-portal vereist. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon6b.png)
    
21. In het dialoogvenster apprearing lezen **overslaan van de test betekent...** , klikt u op **OK**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/addon6c.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>Maken van een SAML SSO voor Jira door resolutie GmbH testgebruiker

Om Azure AD-gebruikers aan te melden bij SAML SSO voor Jira door resolutie GmbH, moeten ze in SAML SSO voor Jira worden ingericht met behulp van de resolutie GmbH.  
In SAML SSO voor Jira door resolutie GmbH is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw SAML SSO voor Jira door resolutie GmbH bedrijf site als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klik op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. U wordt omgeleid naar de pagina beheerderstoegang in te voeren **wachtwoord** en klik op **bevestigen** knop.

    ![Werknemer toevoegen](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. Onder **Gebruikersbeheer** tabblad gedeelte, klikt u op **gebruiker maken**.

    ![Werknemer toevoegen](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. Op de **'Een nieuwe gebruiker maken'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    a. In de **e-mailadres** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **volledige naam** textbox, volledige naam van de gebruiker zoals Britta Simon type.

    c. In de **gebruikersnaam** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** textbox, typt u het wachtwoord van gebruiker.

    e. Klik op **gebruiker maken**.   

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon SAML SSO voor Jira toegang verlenen door resolutie GmbH gebruikt Azure eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen SAML SSO voor Jira door resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAML SSO voor Jira door resolutie GmbH**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op het SAML SSO voor Jira door resolutie GmbH tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw SAML SSO voor Jira door resolutie GmbH toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

