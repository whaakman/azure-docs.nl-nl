---
title: 'Zelfstudie: Azure Active Directory-integratie met Sprinklr | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Sprinklr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 13eea1b7889c6382f1f4a6fb5c9bc02578c52711
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Zelfstudie: Azure Active Directory-integratie met Sprinklr

In deze zelfstudie leert u hoe Sprinklr integreren met Azure Active Directory (Azure AD).

Sprinklr integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Sprinklr heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Sprinklr (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Sprinklr, moet u de volgende items:

- Een Azure AD-abonnement
- Een Sprinklr eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Sprinklr uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sprinklr-from-the-gallery"></a>Sprinklr uit de galerie toevoegen
Voor het configureren van de integratie van Sprinklr in Azure AD, moet u Sprinklr uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Sprinklr uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Sprinklr**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. Selecteer in het deelvenster resultaten **Sprinklr**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Sprinklr op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Sprinklr is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Sprinklr tot stand worden gebracht.

Wijs in Sprinklr, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Sprinklr, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Sprinklr](#creating-a-sprinklr-test-user)**  - Sprinklr die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Sprinklr configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Sprinklr, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Sprinklr** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. Op de **Sprinklr domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.sprinklr.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk de waarde met het werkelijke aanmeldings-URL en de id. Neem contact op met [Sprinklr Client ondersteuningsteam](https://www.sprinklr.com/contact-us/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. Op de **Sprinklr configuratie** sectie, klikt u op **configureren Sprinklr** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

7. In een ander browservenster, meld u aan bij uw bedrijf Sprinklr site als beheerder.

8. Ga naar **beheer \> instellingen**.
   
    ![Beheer](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "beheer")

9. Ga naar **beheren Partner \> eenmalige** op in het linkerdeelvenster.
   
    ![Partner beheren](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Partner beheren")

10. Klik op **+ toevoegen voor eenmalige aanmeldingen**.
   
    ![Eenmalige aanmeldingen](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "eenmalige aanmeldingen")

11. Op de **voor eenmalige aanmelding** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmeldingen](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "eenmalige aanmeldingen")

    a. In de **naam** textbox, typ een naam voor uw configuratie (bijvoorbeeld: *WAADSSOTest*).

    b. Selecteer **ingeschakeld**.

    c. Selecteer **nieuw certificaat voor eenmalige aanmelding gebruiken**.
             
    e. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **Provider identiteitscertificaat** textbox.

    f. Plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure Portal naar de **entiteit-Id** textbox.

    g. Plak de **SAML Single Sign-On Service-URL** waarde die u hebt gekopieerd vanuit Azure Portal naar de **identiteit Provider aanmeldings-URL** textbox.

    h. Plak de **Sign-Out URL** waarde die u hebt gekopieerd vanuit Azure Portal naar de **identiteit Provider afmelding URL** textbox.
     
    ik. Als **SAML-ID gebruikerstype**, selecteer **Assertion gebruiker bevat ' s sprinklr.com gebruikersnaam**.

    j. Als **SAML-ID gebruikerslocatie**, selecteer **gebruikers-ID is in het element met naam-id van het onderwerp overzicht**.

    k. Klik op **Opslaan**.
       
    ![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sprinklr-test-user"></a>Een testgebruiker Sprinklr maken

1. Meld u aan bij uw bedrijf Sprinklr site als beheerder.

2. Ga naar **beheer \> instellingen**.
   
    ![Beheer](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "beheer")

3. Ga naar **Client beheren \> gebruikers** in het linkerdeelvenster.
   
    ![Instellingen](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "instellingen")

4. Klik op **gebruiker toevoegen**.
   
    ![Instellingen](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "instellingen")

5. Op de **bewerken gebruiker** dialoogvenster de volgende stappen uitvoeren:
   
    ![Gebruiker bewerken](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "gebruiker bewerken") 

    a. In de **e**, **voornaam** en **achternaam** tekstvakken, typt u de gegevens van een Azure AD-gebruikersaccount dat u inrichten wilt.

    b. Selecteer **wachtwoord uitgeschakeld**.

    c. Selecteer **taal**.

    d. Selecteer **gebruikerstype**.

    e. Klik op **Update**.
   
     >[!IMPORTANT]
     >**Wachtwoord uitgeschakeld** zodat een gebruiker zich aanmelden via een id-provider moet worden geselecteerd. 
     
6. Ga naar **rol**, en voer de volgende stappen uit:
   
    ![Rollen partner](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner rollen")

    a. Van de **Global** selecteert **alle\_machtigingen**.  

    b. Klik op **Update**.

>[!NOTE]
>U kunt andere Sprinklr gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Sprinklr voor het inrichten van Azure AD-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Sprinklr.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Sprinklr, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Sprinklr**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Sprinklr in het deelvenster toegang, krijgt u automatisch aangemeld bij uw toepassing Sprinklr Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

