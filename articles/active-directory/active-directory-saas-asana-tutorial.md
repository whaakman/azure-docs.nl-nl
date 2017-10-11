---
title: 'Zelfstudie: Azure Active Directory-integratie met Asana | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Asana.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: a2f0cecb93cc382bcfd710c44eb70f80ae67f9b6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Zelfstudie: Azure Active Directory-integratie met Asana

In deze zelfstudie leert u hoe Asana integreren met Azure Active Directory (Azure AD).

Asana integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Asana heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Asana (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Asana, moet u de volgende items:

- Een Azure AD-abonnement
- Een Asana eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Asana uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-asana-from-the-gallery"></a>Asana uit de galerie toevoegen
Voor het configureren van de integratie van Asana in Azure AD, moet u Asana uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Asana uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Asana**, selecteer **Asana** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Asana op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Asana is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Asana tot stand worden gebracht.

Wijs in Asana, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Asana, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Asana](#create-an-asana-test-user)**  - Asana die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Asana configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Asana, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Asana** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. Op de **Asana domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Asana domein eenmalige aanmelding informatie](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    a. In de **aanmeldings-URL** textbox, typ de URL:`https://app.asana.com/`

    b. In de **id** textbox typewaarde:`https://app.asana.com/`
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. Op de **Asana configuratie** sectie, klikt u op **configureren Asana** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Asana configuratie](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. In een ander browservenster eenmalige aanmelding voor uw toepassing Asana. Voor het configureren van SSO in Asana, toegang krijgen tot de werkruimte-instellingen door te klikken op de naam van de werkruimte in de rechterbovenhoek van het scherm. Klik vervolgens op  **\<naam van uw werkruimte\> instellingen**. 
   
    ![Asana SSO-instellingen](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. Op de **organisatie-instellingen** venster, klikt u op **beheer**. Klik vervolgens op **leden moeten zich aanmelden via SAML** zodat de SSO-configuratie. Het uitvoeren van de volgende stappen:
   
    ![Eenmalige aanmelding, organisatie-instellingen configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     a. In de **aanmelden pagina-URL** textbox, plak de **SAML Single Sign-On Service-URL**.

     b. Klik met de rechtermuisknop op het certificaat dat is gedownload vanuit Azure-portal, open vervolgens het certificaatbestand in Kladblok of uw voorkeur teksteditor. Kopieer de inhoud tussen de begin- en de titel van end-certificaat en plak deze in de **X.509-certificaat** textbox.

9. Klik op **Opslaan**. Ga naar [Asana-handleiding voor het instellen van eenmalige aanmelding](https://asana.com/guide/help/premium/authentication#gl-saml) als u meer hulp nodig hebt.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![De knop toevoegen](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-an-asana-test-user"></a>Een testgebruiker Asana maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Asana maken.

1. Op **Asana**, gaat u naar de **Teams** sectie in het linkerdeelvenster. Klik op het plusteken om. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Typ het e-mailbericht britta.simon@contoso.com in het tekstvak in en selecteer vervolgens **uitnodigen**.

3. Klik op **uitnodiging verzenden**. De nieuwe gebruiker ontvangt een e-mailbericht in haar e-mailaccount. Ze moet maken en het account te valideren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Asana.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Asana, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Asana**.

    ![De koppeling Asana in de lijst met toepassingen](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is voor het testen van uw Azure AD eenmalige aanmelding.

Ga naar de aanmeldingspagina Asana. Invoegen in het tekstvak voor e-mailadres, het e-mailadres britta.simon@contoso.com. Laat het wachtwoordtekstvak in leeg en klik vervolgens op **aanmelden**. U wordt omgeleid naar Azure AD-aanmeldingspagina. Voer uw Azure AD-referenties. U bent nu aangemeld op Asana.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
