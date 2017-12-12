---
title: 'Zelfstudie: Azure Active Directory-integratie met Promapp | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Promapp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: jeedes
ms.openlocfilehash: 4a0630bf015361833ed3a6949ea7b29450d53701
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>Zelfstudie: Azure Active Directory-integratie met Promapp

In deze zelfstudie leert u hoe Promapp integreren met Azure Active Directory (Azure AD).

Promapp integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Promapp heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Promapp (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Promapp, moet u de volgende items:

- Een Azure AD-abonnement
- Een Promapp eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Promapp uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-promapp-from-the-gallery"></a>Promapp uit de galerie toevoegen
Voor het configureren van de integratie van Promapp in Azure AD, moet u Promapp uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Promapp uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Promapp**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_search.png)

5. Selecteer in het deelvenster resultaten **Promapp**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Promapp op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Promapp is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Promapp tot stand worden gebracht.

Wijs in Promapp, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Promapp, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Promapp](#creating-a-promapp-test-user)**  - Promapp die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Promapp configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Promapp, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Promapp** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_samlbase.png)

3. Op de **Promapp domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://demo.promapp.com/TENANTNAME`|
    | `https://go.promapp.com/TENANTNAME`|
    | `https://demoau.promapp.com/TENANTNAME`|
    | `https://au.promapp.com/TENANTNAME`|
    | `https://demous.promapp.com/TENANTNAME`|
    | `https://us.promapp.com/TENANTNAME`|
    | `https://dev.promapp.com/TENANTNAME`|
    | `https://test.promapp.com/TENANTNAME`|
    | `https://staging.promapp.com/TENANTNAME`|
    
    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://DOMAINNAME.promapp.com/azuread/saml/authenticate.aspx`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, -id en antwoord-URL. Neem contact op met [Promapp Client ondersteuningsteam](https://www.promapp.com/about-us/contact-us/) ophalen van deze waarden.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_general_400.png)

7. Op de **Promapp configuratie** sectie, klikt u op **configureren Promapp** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_configure.png) 

8. Aanmelding bij uw bedrijf Promapp site als administrator. 

9. Klik in het menu bovenaan op **Admin**. 
   
    ![Azure AD voor eenmalige aanmelding][12]

10. Klik op **Configureren** 
   
    ![Azure AD voor eenmalige aanmelding][13]

11. Op de **beveiliging** dialoogvenster de volgende stappen uitvoeren:
   
    ![Azure AD voor eenmalige aanmelding][14]
    
    a. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit de Azure-portal in de **SSO-aanmeldings-URL** textbox.
    
    b. Als **SSO - modus Single Sign-on**, selecteer **optioneel**, en klik vervolgens op **opslaan**.

    > [!NOTE]
    > **Optionele** modus is alleen voor testdoeleinden. Wanneer u tevreden over de configuratie, selecteer bent **vereist** modus voor het afdwingen van alle gebruikers te verifiëren met behulp van Azure AD.

    c. Open het gedownloade certificaat in Kladblok en kopieer de certificaatinhoud zonder de eerste regel (---**BEGIN CERTIFICATE**---) en de laatste regel (---**EINDCERTIFICAAT**---), plak deze in de  **SSO-x.509-certificaat** tekstvak en klik vervolgens op **opslaan**.
        
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-promapp-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-promapp-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-promapp-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-promapp-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-promapp-test-user"></a>Een testgebruiker Promapp maken

De toepassing Promapp ondersteunt Just-in-Time-inrichting. Dit betekent een gebruikersaccount wordt automatisch gemaakt indien nodig tijdens een poging tot toegang tot de toepassing die het toegangsvenster gebruikt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Promapp.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Promapp, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Promapp**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Voor het testen van uw toepassing in **IDP** geïnitieerd wanneer u op de tegel Promapp in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw toepassing Promapp modus.

Voor het testen van uw toepassing in **SP** geïnitieerd modus, moet u de verificatie van uw site Promapp initiëren. Dit kan worden gedaan door het wachtwoordveld niets bij het aanmelden wordt aangeroepen terwijl er **optioneel** modus is ingeschakeld.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_203.png

