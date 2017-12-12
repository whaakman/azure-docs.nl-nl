---
title: 'Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Thoughtworks Singleplayer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4406524f7ba85c33c0e309d3cc7cd21e2f5e327b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer

In deze zelfstudie leert u hoe Thoughtworks Singleplayer integreren met Azure Active Directory (Azure AD).

Thoughtworks Singleplayer integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Thoughtworks Singleplayer heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Thoughtworks Singleplayer (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Thoughtworks Singleplayer, moet u de volgende items:

- Een Azure AD-abonnement
- Een Thoughtworks Singleplayer eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Thoughtworks Singleplayer uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Toe te voegen Thoughtworks Singleplayer uit de galerie
Voor het configureren van de integratie van Thoughtworks Singleplayer in Azure AD, moet u Thoughtworks Singleplayer toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Thoughtworks Singleplayer uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Thoughtworks Singleplayer**, selecteer **Thoughtworks Singleplayer** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Thoughtworks Singleplayer in de lijst met resultaten](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met Thoughtworks Singleplayer op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Thoughtworks Singleplayer is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Thoughtworks Singleplayer tot stand worden gebracht.

Wijs in het Thoughtworks Singleplayer, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Thoughtworks Singleplayer, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Thoughtworks Singleplayer](#create-a-thoughtworks-mingle-test-user)**  - Thoughtworks Singleplayer die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Thoughtworks Singleplayer configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Thoughtworks Singleplayer, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Thoughtworks Singleplayer** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. Op de **Thoughtworks Singleplayer domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Thoughtworks Singleplayer domein eenmalige aanmelding informatie](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > De waarde is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Thoughtworks Singleplayer Client ondersteuningsteam](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) de waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. Meld u aan bij uw **Thoughtworks Singleplayer** bedrijf site als administrator.

7. Klik op de **Admin** tabblad en klik vervolgens op **SSO-Config**.
   
    ![Tabblad beheer](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO-configuratie")

8. In de **SSO-Config** sectie, voert u de volgende stappen uit:
   
    ![SSO-Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO-configuratie")
    
    a. Als u wilt uploaden van het metagegevensbestand, klikt u op **bestand kiezen**. 

    b. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Een testgebruiker Thoughtworks Singleplayer maken

Azure AD-gebruikers moeten kunnen aanmelden, als ze worden ingericht voor de toepassing Thoughtworks Singleplayer met behulp van de namen van de Azure Active Directory-gebruiker. In het geval van Thoughtworks Singleplayer is inrichting een handmatige taak.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Thoughtworks Singleplayer site als administrator.

2. Klik op **profiel**.
   
    ![Uw eerste Project](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "uw eerste Project")

3. Klik op de **Admin** tabblad en klik vervolgens op **gebruikers**.
   
    ![Gebruikers](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "gebruikers")

4. Klik op **nieuwe gebruiker**.
   
    ![Nieuwe gebruiker](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "nieuwe gebruiker")

5. Op de **nieuwe gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Dialoogvenster Nieuwe gebruiker](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "nieuwe gebruiker")  
 
    a. Type de **aanmeldingsnaam**, **weergavenaam**, **Kies wachtwoord**, **wachtwoord bevestigen** van een geldig Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt. 

    b. Als **gebruikerstype**, selecteer **volledige gebruiker**.

    c. Klik op **maken van dit profiel**.

>[!NOTE]
>U kunt andere Thoughtworks Singleplayer gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Thoughtworks Singleplayer aan inrichten AAD-gebruikersaccounts.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen Thoughtworks Singleplayer.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Thoughtworks Singleplayer, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Thoughtworks Singleplayer**.

    ![De koppeling Thoughtworks Singleplayer in de lijst met toepassingen](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel Thoughtworks Singleplayer in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Thoughtworks Singleplayer.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

