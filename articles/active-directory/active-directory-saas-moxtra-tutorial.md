---
title: 'Zelfstudie: Azure Active Directory-integratie met Moxtra | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Moxtra.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: db2f041a44b6771b0a4f734e58d899298ef0847b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Zelfstudie: Azure Active Directory-integratie met Moxtra

In deze zelfstudie leert u hoe Moxtra integreren met Azure Active Directory (Azure AD).

Moxtra integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Moxtra heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Moxtra (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Moxtra, moet u de volgende items:

- Een Azure AD-abonnement
- Een Moxtra eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Moxtra uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-moxtra-from-the-gallery"></a>Moxtra uit de galerie toevoegen
Voor het configureren van de integratie van Moxtra in Azure AD, moet u Moxtra uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Moxtra uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Moxtra**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. Selecteer in het deelvenster resultaten **Moxtra**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Moxtra op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Moxtra is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Moxtra tot stand worden gebracht.

Wijs in Moxtra, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Moxtra, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Moxtra](#creating-a-moxtra-test-user)**  - Moxtra die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Moxtra configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Moxtra, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Moxtra** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. Op de **Moxtra domein en de URL's** sectie, voert u de volgende stap:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    In de **aanmeldings-URL** textbox, typ een URL als:`https://www.moxtra.com/service/#login`

4. De SAML-asserties verwacht Moxtra toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze configuratie. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam van kenmerk | De waarde van kenmerk |
    | ------------------- | -------------------- |    
    | Voornaam | User.givenName |
    | Achternaam | User.surname |
    | idpid    | < SAML entiteit-ID > 

    > [!Note]
    > De waarde van **idpid** -kenmerk is niet echt. U krijgt de werkelijke waarde van **Naslaggids** onder sectie **Moxtra configuratie**.
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.
    
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. Op de **Moxtra configuratie** sectie, klikt u op **configureren Moxtra** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. In een ander browservenster zich aanmelden bij uw bedrijf Moxtra site als een beheerder.

9. Klik in de werkbalk aan de linkerkant op **-beheerconsole > SAML Single Sign-on**, en klik vervolgens op **nieuw**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. Op de **SAML** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: *SAML*). 
  
    b. In de **IdP entiteit-ID** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal. 
 
    c. In **aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal. 
 
    d. In de **AuthnContextClassRef** textbox type **urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**. 
 
    e. In de **NameID indeling** textbox type **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**. 
 
    f. Open certificaat dat u hebt gedownload vanuit Azure-portal in Kladblok, Kopieer de inhoud en plakt u deze in de **certificaat** textbox.    
 
    g. In het tekstvak voor SAML e-domein, typ uw e-maildomein SAML.    
  
    >[!NOTE]
    >Als u wilt zien welke stappen om te controleren of het domein, klikt u op de '**ik**' hieronder.

    h. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-moxtra-test-user"></a>Een testgebruiker Moxtra maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Moxtra genoemd.

**Als u wilt een gebruiker Britta Simon aangeroepen in Moxtra maakt, moet u de volgende stappen uitvoeren:**

1. Meld u op met uw bedrijf Moxtra site als een beheerder.

2. Klik in de werkbalk aan de linkerkant op **-beheerconsole > Gebruikersbeheer**, en vervolgens **gebruiker toevoegen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. Op de **gebruiker toevoegen** dialoogvenster de volgende stappen uitvoeren:
  
    a. In de **voornaam** textbox type **Britta**.
  
    b. In de **achternaam** textbox type **Simon**.
  
    c. In de **e** textbox type Britta van e-mailadres dezelfde als voor de Azure-portal.
  
    d. In de **deling** textbox type **Dev**.
  
    e. In de **afdeling** textbox type **IT**.
  
    f. Selecteer **beheerder**.
  
    g. Klik op **Add**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Moxtra.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Moxtra, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Moxtra**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Moxtra in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Moxtra.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

