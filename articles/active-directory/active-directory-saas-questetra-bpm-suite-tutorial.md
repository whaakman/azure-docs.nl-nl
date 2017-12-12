---
title: 'Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Questetra BPM Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: e2515d571aa1615f14b597ef82b1087df3827f82
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite

In deze zelfstudie leert u hoe Questetra BPM Suite integreren met Azure Active Directory (Azure AD).

Questetra BPM Suite integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Questetra BPM Suite heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Questetra BPM Suite (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Questetra BPM Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een Questetra BPM Suite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Questetra BPM Suite uit de galerie toevoegen
2. Configureren en testen eenmalige aanmelding Azure AD

## <a name="add-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM Suite uit de galerie toevoegen
Voor het configureren van de integratie van Questetra BPM Suite in Azure AD, moet u Questetra BPM Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Questetra BPM Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Questetra BPM Suite**, selecteer **Questetra BPM Suite** van resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met Questetra BPM Suite op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Questetra BPM Suite is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Questetra BPM Suite tot stand worden gebracht.

Wijs in Questetra BPM Suite, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Questetra BPM Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Questetra BPM Suite](#create-a-questetra-bpm-suite-test-user)**  - Questetra BPM Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Questetra BPM Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Questetra BPM Suite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Op SAML gebaseerde aanmelding](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_samlbase.png)

3. Op de **Questetra BPM Suite domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Sectie Questetra BPM Suite domein en URL 's](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.questetra.net/saml/SSO/alias/bpm`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.questetra.net/`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. U kunt deze waarden ophalen uit **SP informatie** sectie op uw **Questetra BPM Suite** bedrijf-site, wordt verderop in de zelfstudie of neem contact op met besproken [clientondersteuning Questetra BPM Suite team](https://www.questetra.com/contact/). 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base 64)** en sla het certificaatbestand op uw computer.

    ![Certificaat voor ondertekening van SAML-sectie](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop Opslaan](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_400.png)

6. Op de **Questetra BPM Suite configuratie** sectie, klikt u op **configureren Questetra BPM Suite** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratiesectie Questetra BPM Suite](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_configure.png) 

7. In een ander browservenster, meld u aan bij uw **Questetra BPM Suite** bedrijf site als beheerder.

8. Klik in het menu bovenaan op **systeeminstellingen**. 
   
    ![Azure AD voor eenmalige aanmelding][10]

9. Openen van de **SingleSignOnSAML** pagina, klikt u op **eenmalige aanmelding (SAML)**. 
   
    ![Azure AD voor eenmalige aanmelding][11]

10. Op uw **Questetra BPM Suite** bedrijf site, in de **SP informatie** sectie, voert u de volgende stappen uit:

    a. Kopiëren de **ACS URL**, en plak deze in de **aanmelding op URL** textbox in de **Questetra BPM Suite domein en de URL's** sectie vanuit Azure-portal.
    
    b. Kopieer de **entiteit-ID**, en plak deze in de **id** textbox in de **Questetra BPM Suite domein en de URL's** sectie vanuit Azure-portal.

11. Op uw **Questetra BPM Suite** bedrijf site, voert u de volgende stappen uit: 
   
    ![Eenmalige aanmelding configureren][15]
   
    a. Selecteer **eenmalige aanmelding inschakelen**.
   
    b. In **entiteit-ID** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
    
    c. In **aanmelden pagina-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
    
    d. In **afmelden pagina-URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
    
    e. In de **NameID indeling** textbox type `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Open uw **Base 64-** in Kladblok een gecodeerd certificaat gedownload vanuit Azure-portal, de inhoud van het kopiëren naar het Klembord en plakt u deze in de **validatiecertificaat** textbox. 

    g. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-questetra-bpm-suite-test-user"></a>Maak een testgebruiker Questetra BPM Suite

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Questetra BPM Suite.

**Als u wilt een gebruiker Britta Simon aangeroepen in Questetra BPM Suite maakt, moet u de volgende stappen uitvoeren:**

1. Meld u op met uw bedrijf Questetra BPM Suite site als een beheerder.
2. Ga naar **systeeminstellingen > gebruikerslijst > nieuwe gebruiker**. 
3. In het dialoogvenster Nieuwe gebruiker de volgende stappen uitvoeren: 
   
    ![Testgebruiker maken][300] 
   
    a. In de **naam** textbox type **naam** van de gebruiker  **britta.simon@contoso.com** .
   
    b. In de **e** textbox type **e** van de gebruiker**britta.simon@contoso.com**
   
    c. In de **wachtwoord** textbox, typ een **wachtwoord** van de gebruiker.
    
    d. Klik op **nieuwe gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Questetra BPM Suite.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Questetra BPM Suite**.

    ![Questetra BPM Suite in lijst met apps](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Questetra BPM Suite in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Questetra BPM Suite.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png

[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_203.png
[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 

