---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 4dcf487afdad899853c97d3d2a1493a6123b3bab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440708"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex

In deze zelfstudie leert u hoe u Cisco Webex integreren met Azure Active Directory (Azure AD).

Cisco Webex integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Cisco Webex heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Cisco Webex met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cisco Webex, moet u de volgende items:

- Een Azure AD-abonnement
- Cisco Webex één teken op ingeschakeld abonnement

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cisco Webex uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-cisco-webex-from-the-gallery"></a>Cisco Webex uit de galerie toevoegen
Voor het configureren van de integratie van Cisco Webex in Azure AD, moet u Cisco Webex uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cisco Webex uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Cisco Webex**. 

1. Selecteer **Cisco Webex** vanuit het deelvenster resultaten. Selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Cisco Webex in de lijst met resultaten](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Cisco Webex op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in de Webex Cisco is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cisco Webex vast te stellen.

Geef de waarde in de Webex Cisco, **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. Nu hebt u de koppeling tussen de twee gebruikers gemaakt. 

Als u wilt configureren en Azure AD eenmalige aanmelding met Cisco Webex testen, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
1. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. [Maak een testgebruiker Cisco Webex](#create-a-cisco-webex-test-user) hebben een equivalent van Britta Simon in Cisco Webex die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Cisco Webex-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Cisco Webex, voert u de volgende stappen uit:**

1. In de Azure-portal op de **Cisco Webex** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Voor het inschakelen van eenmalige aanmelding in de **eenmalige aanmelding** in het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **SAML gebaseerde aanmelding**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

1. In de **Cisco Webex-domein en URL's** sectie, de volgende stappen uit:

    ![Cisco Webex-domein en URL's, eenmalige aanmelding informatie](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. In de **aanmeldings-URL** vak, typ een URL met het volgende patroon: `https://<subdomain>.webex.com`

    b. In de **id** vak, typ de URL `http://www.webex.com`.

    c. In de **antwoord-URL** vak, typ een URL met het volgende patroon: `https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke antwoord-URL en aanmeldings-URL. Neem contact op met [Cisco Webex-Client-ondersteuningsteam](https://www.webex.co.in/support/support-overview.html) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

1. Selecteer **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
1. In de **Cisco Webex configuratie** sectie, selecteer **configureren Cisco Webex** openen de **aanmelding configureren** venster. Kopiëren de **afmelding URL**, **SAML entiteit-ID**, en **Single Sign-On Service URL voor SAML** uit de **Naslaggids** sectie.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf Cisco Webex site als beheerder.

1. Selecteer in het menu bovenaan de **Sitebeheer**.

    ![Sitebeheer](./media/cisco-webex-tutorial/ic777621.png "Sitebeheer")

1. In de **-Site beheren** sectie, selecteer **SSO-configuratie**.
   
    ![Configuratie van eenmalige aanmelding](./media/cisco-webex-tutorial/ic777622.png "SSO-configuratie")

1. In de **federatief Web SSO-configuratie** sectie, de volgende stappen uit:
   
    ![Federatieve SSO-configuratie](./media/cisco-webex-tutorial/ic777623.png "federatieve SSO-configuratie")  

    a. In de **Federation-Protocol** in de lijst met **SAML 2.0**.

    b. Voor **SSO profiel**, selecteer **SP geïnitieerde**.

    c. Open het gedownloade certificaat in Kladblok en kopieer vervolgens de inhoud.

    d. Selecteer **SAML-metagegevens importeren**, en plak de gekopieerde inhoud van het certificaat.

    e. In de **verlener voor SAML (IdP-ID)** vak, plak de waarde van de **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    f. In de **aanmeldings-URL van de klant SSO Service** vak, plak **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    g. Uit de **NameID-indeling** in de lijst met **e-mailadres**.

    h. In de **AuthnContextClassRef** in het vak **urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**.

    i. In de **afmeldings-URL van de klant SSO Service** vak, plak **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.
   
    j. Selecteer **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie op [documentatie over Azure AD embedded](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/cisco-webex-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/cisco-webex-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/cisco-webex-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-cisco-webex-test-user"></a>Maak een testgebruiker Cisco Webex

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Cisco Webex, moeten ze worden ingericht in Cisco Webex. In het geval van Cisco Webex is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw **Cisco Webex** tenant.

1. Ga naar **gebruikers beheren** > **gebruiker toevoegen**.
   
    ![Gebruikers toevoegen](./media/cisco-webex-tutorial/ic777625.png "gebruikers toevoegen")

1. In de **gebruiker toevoegen** sectie, de volgende stappen uit:
   
    ![Gebruiker toevoegen](./media/cisco-webex-tutorial/ic777626.png "gebruiker toevoegen")   

    a. Voor **accounttype**, selecteer **Host**.

    b. In de **voornaam** typt u de voornaam van de gebruiker (in dit geval **Julia**).

    c. In de **achternaam** typt u de achternaam van de gebruiker (in dit geval **Simon**).

    d. In de **gebruikersnaam** typt u het e-mailadres van de gebruiker (in dit geval **Brittasimon@contoso.com**).

    e. In de **e** typt u het e-mailadres van de gebruiker (in dit geval **Brittasimon@contoso.com**).

    f. In de **wachtwoord** typt u het wachtwoord van gebruikers.

    g. In de **bevestigen** wachtwoord vak, van de gebruiker-wachtwoord opnieuw invoeren.

    h. Selecteer **Toevoegen**.

>[!NOTE]
>U kunt een andere hulpprogramma's voor Cisco Webex gebruiker-account maken of API's die worden geboden door Cisco Webex voor het inrichten van gebruikersaccounts van de Azure AD gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u de gebruiker Britta Simon Azure eenmalige aanmelding gebruiken ze door toegang te verlenen aan Cisco Webex.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Cisco Webex toewijst, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure-portal. Volgende, gaat u naar de directoryweergave, en vervolgens naar **bedrijfstoepassingen**.  

1. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Cisco Webex**.

    ![De Cisco Webex-koppeling in de lijst met toepassingen](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

1. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst.

1. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

1. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de Cisco Webex-tegel in het toegangsvenster selecteert, ophalen u automatisch aangemeld bij uw Cisco Webex-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

