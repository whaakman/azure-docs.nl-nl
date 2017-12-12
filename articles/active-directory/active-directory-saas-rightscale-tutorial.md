---
title: 'Zelfstudie: Azure Active Directory-integratie met Rightscale | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Rightscale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 62c95d798e851ddf6030714a052afb32b977b7e3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Zelfstudie: Azure Active Directory-integratie met Rightscale

In deze zelfstudie leert u hoe Rightscale integreren met Azure Active Directory (Azure AD).

Rightscale integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Rightscale heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Rightscale (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Rightscale, moet u de volgende items:

- Een Azure AD-abonnement
- Een Rightscale eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Rightscale uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-rightscale-from-the-gallery"></a>Rightscale uit de galerie toevoegen
Voor het configureren van de integratie van Rightscale in Azure AD, moet u Rightscale uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Rightscale uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Rightscale**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. Selecteer in het deelvenster resultaten **Rightscale**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Rightscale op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Rightscale is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Rightscale tot stand worden gebracht.

Wijs in Rightscale, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Rightscale, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Rightscale](#creating-a-rightscale-test-user)**  - Rightscale die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Rightscale.

**Voor het configureren van Azure AD eenmalige aanmelding met Rightscale, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Rightscale** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. Op de **Rightscale domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP geïnitieerd modus** hoeft u niet alle stappen uitvoeren als de app al vooraf geïntegreerde met Azure is.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. Op de **Rightscale domein en de URL's** sectie als u wilt configureren van de toepassing in **SP geïnitieerd modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    a. Klik op de **weergeven geavanceerde instellingen voor URL**.

    b. In de **aanmelding op URL** textbox, typ de URL:`https://login.rightscale.com/`

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. Op de **Rightscale configuratie** sectie, klikt u op **configureren Rightscale** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS>
8. Als u eenmalige aanmelding voor uw toepassing is geconfigureerd, moet u eenmalige aanmelding voor uw tenant RightScale als beheerder.

    a. Klik in het menu bovenaan op de **instellingen** tabblad en selecteer **Single Sign-On**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Klik op de '**nieuwe**' om toe te voegen **uw SAML identiteitsproviders**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. In het tekstvak van **weergavenaam**, voer de naam van uw bedrijf.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Selecteer **RightScale toestaan geïnitieerde eenmalige aanmelding met een hint detectie** en invoer uw **domeinnaam** in het onderstaande tekstvak.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    e. Plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal in **SAML SSO eindpunt** in RightScale.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    f. Plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal in **SAML id van de entiteit** in RightScale.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    g. Klik op **Browser** knop voor het uploaden van het certificaat dat u hebt gedownload van Azure-portal.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    h. Klik op **Opslaan**.
<CE>
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-rightscale-test-user"></a>Een testgebruiker Rightscale maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in RightScale maken. Werken met [Rightscale Client ondersteuningsteam](mailto:support@rightscale.com) de gebruikers van het platform RightScale toevoegen.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Rightscale.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Rightscale, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Rightscale**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.  

Als u op de tegel RightScale in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing RightScale.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

