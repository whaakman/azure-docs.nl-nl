---
title: 'Zelfstudie: Azure Active Directory-integratie met perceptie Verenigde Staten (niet-UltiPro) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en perceptie Verenigde Staten (niet-UltiPro).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: d94d233a12e51bf851a791fda481b91c513d64b7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Zelfstudie: Azure Active Directory-integratie met perceptie Verenigde Staten (niet-UltiPro)

In deze zelfstudie leert u hoe perceptie Verenigde Staten (niet-UltiPro) integreren met Azure Active Directory (Azure AD).

Perceptie Verenigde Staten (niet-UltiPro) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die naar perceptie Verenigde Staten (niet-UltiPro) toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemelde naar perceptie Verenigde Staten (niet-UltiPro) (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met perceptie Verenigde Staten (niet-UltiPro), moet u de volgende items:

- Een Azure AD-abonnement
- Een perceptie Verenigde Staten (niet-UltiPro) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Perceptie Verenigde Staten (niet-UltiPro) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Perceptie Verenigde Staten (niet-UltiPro) uit de galerie toevoegen
Voor het configureren van de integratie van perceptie Verenigde Staten (niet-UltiPro) met Azure AD, moet u toevoegen perceptie Verenigde Staten (niet-UltiPro) uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen perceptie Verenigde Staten (niet-UltiPro) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **perceptie Verenigde Staten (niet-UltiPro)**, selecteer **perceptie Verenigde Staten (niet-UltiPro)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Perceptie Verenigde Staten (niet-UltiPro) in de lijst met resultaten](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met perceptie Verenigde Staten (niet-UltiPro) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in perceptie Verenigde Staten (niet-UltiPro) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de perceptie Verenigde Staten (niet-UltiPro) tot stand worden gebracht.

In perceptie Verenigde Staten (niet-UltiPro), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met perceptie Verenigde Staten (niet-UltiPro), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker perceptie Verenigde Staten (niet-UltiPro)](#create-a-perception-united-states-non-ultipro-test-user)**  - hebben een equivalent van Britta Simon in perceptie Verenigde Staten (niet-UltiPro) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing perceptie Verenigde Staten (niet-UltiPro).

**Voor het configureren van Azure AD eenmalige aanmelding met perceptie Verenigde Staten (niet-UltiPro), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **perceptie Verenigde Staten (niet-UltiPro)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. Op de **perceptie Verenigde Staten (niet-UltiPro)-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Perceptie Verenigde Staten (niet-UltiPro)-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. In de **id** textbox, typ de URL:`https://perception.kanjoya.com/sp`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > De waarde is geen echte. U kunt de waarde wordt bijgewerkt met de werkelijke antwoord-URL, die verderop in de zelfstudie wordt beschreven.
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. Op de **perceptie Verenigde Staten (niet-UltiPro) configuratie** sectie, klikt u op **configureren perceptie Verenigde Staten (niet-UltiPro)** openen **eenmalige aanmelding configureren** venster . Kopieer de **SAML entiteit-ID** van de **Naslaggids punt.**

    a. De **perceptie Verenigde Staten (niet-UltiPro)** toepassing vereist de **SAML entiteit-ID** waarde, die u hebt gekopieerd, URI gecodeerd. Als u de waarde van de uri-gecodeerde, gebruikt u de volgende koppeling:**http://www.url-encode-decode.com/**.

    b. Nadat u de uri gecodeerde waarde combineren met de **antwoord-URL** zoals vermeld hieronder -

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Plak de bovenstaande waarde in de **antwoord-URL** textbox in **perceptie Verenigde Staten (niet-UltiPro)-domein en de URL's** sectie.

    ![Perceptie Verenigde Staten (niet-UltiPro)-configuratie](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. In een ander browservenster zich aanmelden bij uw bedrijf perceptie Verenigde Staten (niet-UltiPro) site als een beheerder.

8. Klik in de hoofdwerkbalk **Accountinstellingen**.

    ![Perceptie Verenigde Staten (niet-UltiPro) gebruiker](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. Op de **Accountinstellingen** pagina, voert u de volgende stappen uit:

    ![Perceptie Verenigde Staten (niet-UltiPro) gebruiker](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. In de **bedrijfsnaam** textbox, typ de naam van de **bedrijf**.
    
    b. In de **accountnaam** textbox, typ de naam van de **Account**.

    c. In **standaard antwoordadres e** tekst typt u het geldige **e**.

    d. Selecteer **SSO-identiteitsprovider** als **SAML 2.0**.

10. Op de **SSO configuratie** pagina, voert u de volgende stappen uit:

    ![Perceptie Verenigde Staten (niet-UltiPro) SSOConfig](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Selecteer **SAML NameID Type** als **e**.

    b. In de **SSO configuratienaam** textbox, typ de naam van uw **configuratie**.
    
    c. In **identiteit providernaam** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal. 

    d. In **SAML domein textbox**, geef het domein zoals  **@contoso.com** .

    e. Klik op **opnieuw uploaden** voor het uploaden van de **Metadata XML** bestand.

    f. Klik op **Update**.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Maak een testgebruiker perceptie Verenigde Staten (niet-UltiPro)

In deze sectie maakt u een gebruiker met de naam Britta Simon in perceptie Verenigde Staten (niet-UltiPro). Werken met [ondersteuningsteam perceptie Verenigde Staten (niet-UltiPro)](http://www.ultimatesoftware.com/Contact/ContactUs) om toe te voegen de gebruikers van het platform perceptie Verenigde Staten (niet-UltiPro).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent aan perceptie Verenigde Staten (niet-UltiPro) gebruiken.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon naar perceptie Verenigde Staten (niet-UltiPro), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **perceptie Verenigde Staten (niet-UltiPro)**.

    ![De koppeling perceptie Verenigde Staten (niet-UltiPro) in de lijst met toepassingen](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel perceptie Verenigde Staten (niet-UltiPro) in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing perceptie Verenigde Staten (niet-UltiPro).
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

