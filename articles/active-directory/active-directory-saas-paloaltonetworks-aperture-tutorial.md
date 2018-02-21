---
title: 'Zelfstudie: Azure Active Directory-integratie met netwerken Palo Alto - opening | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Palo Alto Networks - opening.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a5ea18d3-3aaf-4bc6-957c-783e9371d0f1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 75633cbf13756b4b2be3e4be055b12021cc396d2
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---aperture"></a>Zelfstudie: Azure Active Directory-integratie met netwerken Palo Alto - opening

In deze zelfstudie leert u hoe integreren Palo Alto Networks - opening met Azure Active Directory (Azure AD).

Netwerken Palo Alto - opening met Azure AD integreren biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben tot netwerken Palo Alto - opening.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Palo Alto Networks - opening (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met netwerken Palo Alto - opening, moet u de volgende items:

- Een Azure AD-abonnement
- Een Palo Alto netwerken - opening eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Netwerken Palo Alto - opening uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-palo-alto-networks---aperture-from-the-gallery"></a>Netwerken Palo Alto - opening uit de galerie toevoegen
Om de integratie van netwerken Palo Alto - opening met Azure AD te configureren die u wilt toevoegen Palo Alto Networks - opening uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen, Palo Alto Networks - opening uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Palo Alto Networks - opening**, selecteer **Palo Alto Networks - opening** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Netwerken Palo Alto - opening in de lijst met resultaten](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met netwerken Palo Alto - opening op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Palo Alto Networks - opening is aan een gebruiker in Azure AD. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in netwerken Palo Alto - opening moet tot stand worden gebracht.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met netwerken Palo Alto - opening, moet u de volgende elementen voltooid:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een netwerken Palo Alto - opening testgebruiker](#create-a-palo-alto-networks---aperture-test-user)**  - Palo Alto Networks - opening die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw netwerken Palo Alto - opening toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met netwerken Palo Alto - opening, de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Palo Alto Networks - opening** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_samlbase.png)

3. Op de **Palo Alto Networks - opening domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Palo Alto netwerken - URL's en opening domein eenmalige aanmelding informatie](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/auth`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Palo Alto netwerken - URL's en opening domein eenmalige aanmelding informatie](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/sign_in`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Palo Alto Networks - ondersteuningsteam opening Client](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_400.png)


7. Op de **Palo Alto Networks - opening configuratie** sectie, klikt u op **Palo Alto-netwerken configureren - opening** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![De koppeling configureren](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_configure.png)

8. In een ander browservenster, meld u aan bij Palo Alto Networks - opening als beheerder.

9. In het bovenste menu, klikt u op **instellingen**.

    ![Het tabblad instellingen](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_settings.png)

10. Navigeer naar **toepassing** sectie Klik **verificatie** vormen aan de linkerkant van het menu.

    ![Het tabblad verificatie](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_auth.png)
    
11. Op de **verificatie** pagina de volgende stappen uitvoeren:
    
    ![Het tabblad verificatie](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_singlesignon.png)

    a. Controleer de **inschakelen Single Sign-On(Supported SSP Providers are Okta, Onelogin)** van **Single Sign-On** veld.

    b. In de **identiteit Provider-ID** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.

    c. Klik op **bestand kiezen** voor het uploaden van het gedownloade certificaat van Azure AD in de **Provider identiteitscertificaat** veld.

    d. In de **identiteit Provider SSO URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    e. Lees de informatie IdP van **opening Info** sectie en downloaden van het certificaat uit **opening sleutel** veld.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-palo-alto-networks---aperture-test-user"></a>Een netwerken Palo Alto - opening testgebruiker maken

In deze sectie maakt maken u een gebruiker met de naam van Britta Simon in netwerken Palo Alto - opening. Werken met [Palo Alto Networks - ondersteuningsteam opening Client](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) om toe te voegen de gebruikers in de netwerken Palo Alto - opening platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot netwerken Palo Alto - opening.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Palo Alto Networks - opening, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Palo Alto Networks - opening**.

    ![De netwerken Palo Alto - opening koppeling in de lijst met toepassingen](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de netwerken Palo Alto - tegel in het deelvenster toegang opening u moet ophalen automatisch aangemeld bij uw netwerken Palo Alto - opening toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_203.png

