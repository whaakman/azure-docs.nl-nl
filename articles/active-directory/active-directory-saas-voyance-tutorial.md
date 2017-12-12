---
title: 'Zelfstudie: Azure Active Directory-integratie met Voyance | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Voyance.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: ea3f8ff903c051ac2147408092e6f35421ed4011
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a>Zelfstudie: Azure Active Directory-integratie met Voyance

In deze zelfstudie leert u hoe Voyance integreren met Azure Active Directory (Azure AD).

Voyance integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Voyance heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Voyance (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Voyance, moet u de volgende items:

- Een Azure AD-abonnement
- Een Voyance eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Voyance uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-voyance-from-the-gallery"></a>Voyance uit de galerie toevoegen
Voor het configureren van de integratie van Voyance in Azure AD, moet u Voyance uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Voyance uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Voyance**, selecteer **Voyance** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Voyance in de lijst met resultaten](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Voyance op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Voyance is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Voyance tot stand worden gebracht.

Wijs in Voyance, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Voyance, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Voyance](#create-a-voyance-test-user)**  - Voyance die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Voyance configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Voyance, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Voyance** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_samlbase.png)

3. Op de **Voyance domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Voyance domein en URL's eenmalige aanmelding-informatie voor IDP](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url1.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.nyansa.com`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.nyansa.com/saml/create/`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Voyance domein en URL's eenmalige aanmelding-informatie voor SP](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url2.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.nyansa.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Voyance Client ondersteuningsteam](mailto:support@nyansa.com) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-voyance-tutorial/tutorial_general_400.png)
    
7. Op de **Voyance configuratie** sectie, klikt u op **configureren Voyance** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Voyance configuratie](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_configure.png) 

8. In een ander browservenster aanmelding bij uw tenant Voyance als beheerder.

9. Ga naar de rechterbovenhoek van de navigatiebalk en klik op de vervolgkeuzelijst met de tekst "**Top universiteit**'.
    
    ![Eenmalige aanmelding op App aan clientzijde Top universiteit configureren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_001.png) 

10. Klik op '**beheerdersinstellingen**'.

    ![Eenmalige aanmelding op App aan clientzijde beheerdersinstellingen configureren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_002.png)

11. Klik op '**gebruikerstoegang**' tabblad.

    ![Eenmalige aanmelding configureren voor toegang voor App-zijde van gebruiker](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_003.png)

12. Klik op de '**SSO is uitgeschakeld**' om de Azure AD configureren als een IdP SAML 2.0 gebruikt.

    ![Configureren van eenmalige aanmelding op App aan clientzijde SSO is uitgeschakeld knop](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_004.png)

13. Ga naar **SAML v2** sectie en Voer onderstaande stappen te volgen:

    ![Eenmalige aanmelding configureren op App aan clientzijde SAML v2](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_005.png)
    
    a. Selecteer **ingeschakeld**.
    
    b. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit de Azure-portal in de **IdP aanmeldings-URL** textbox.

    c. Open uw gedownloade Base64-gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **IdP Cert** textbox.
    
    d. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-voyance-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-voyance-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-voyance-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-voyance-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-voyance-test-user"></a>Een testgebruiker Voyance maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Voyance genoemd. Voyance ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Voyance als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met [Voyance ondersteuningsteam](maiLto:support@nyansa.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Voyance.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Voyance, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Voyance**.

    ![De koppeling Voyance in de lijst met toepassingen](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Voyance in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Voyance.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_203.png

