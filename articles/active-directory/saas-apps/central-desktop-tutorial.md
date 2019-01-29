---
title: 'Zelfstudie: Azure Active Directory-integratie met centraal Desktop | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en centrale Desktop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: bc8aec25d6493d87393a45549fb89f978c469912
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179154"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Zelfstudie: Azure Active Directory-integratie met centraal Desktop

In deze zelfstudie leert u hoe u centraal Desktop integreert met Azure Active Directory (Azure AD).

Centrale Desktop integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de centrale Desktop heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij centrale Desktop met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met centraal Desktop, moet u de volgende items:

- Een Azure AD-abonnement
- Centrale Desktop één teken op ingeschakeld abonnement

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Als u nog een proefversie Azure AD-omgeving hebt [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Centrale Desktop uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-central-desktop-from-the-gallery"></a>Centrale Desktop uit de galerie toevoegen
Voor het configureren van de integratie van centrale Desktop in Azure AD, moet u centraal Desktop vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen centrale Desktop vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

1. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Bedrijfstoepassingen][2]
    
1. Als u wilt toevoegen van nieuwe toepassingen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **centrale Desktop**. Selecteer **centrale Desktop** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Centrale bureaublad in de lijst met resultaten](./media/central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met centrale bureaublad op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD weten wie de gebruiker equivalent in Centraal Desktop is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Centraal Desktop vast te stellen.

Geef in het centrale Desktop **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. Nu hebt u de koppeling tussen de twee gebruikers gemaakt.

Om te configureren en testen van Azure AD eenmalige aanmelding met centrale Desktop, moet u de volgende bouwstenen voltooien:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
1. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. [Maak een testgebruiker centrale Desktop](#create-a-central-desktop-test-user) een equivalent van Britta Simon hebben in Centraal Desktop die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw centrale Desktop-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met centrale Desktop, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **centrale Desktop** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Voor het inschakelen van eenmalige aanmelding in de **eenmalige aanmelding** in het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **SAML gebaseerde aanmelding**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

1. In de **centrale Desktop domein en URL's** sectie, de volgende stappen uit:

    ![Centrale bureaublad domein en URL's eenmalige aanmelding informatie](./media/central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. In de **aanmeldings-URL** vak, typ een URL met het volgende patroon: `https://<companyname>.centraldesktop.com`

    b. In de **id** vak, typ een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. In de **antwoord-URL** vak, typ een URL met het volgende patroon: `https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke-id, antwoord-URL en aanmeldings-URL. Neem contact op met de [centrale Desktop client-ondersteuningsteam](https://imeetcentral.com/contact-us) om deze waarden te verkrijgen. 

1. In de **SAML-handtekeningcertificaat** sectie, selecteer **certificaat**. Sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

1. Selecteer de knop **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/central-desktop-tutorial/tutorial_general_400.png)
    
1. In de **configuratie voor het centraal Desktop** sectie, selecteer **centrale Desktop configureren** openen de **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids** sectie.

    ![Configuratie van centrale bureaublad](./media/central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

1. Aanmelden bij uw **centrale Desktop** tenant.

1. Ga naar **instellingen**. Selecteer **Geavanceerd**, en selecteer vervolgens **Single Sign On**.

    ![Setup - geavanceerde](./media/central-desktop-tutorial/ic769563.png "Setup - Geavanceerd")

1. Op de **één teken op instellingen** pagina, de volgende stappen uit:

    ![Eenmalige aanmelding instellingen](./media/central-desktop-tutorial/ic769564.png "één teken op instellingen")
    
    a. Selecteer **inschakelen SAML v2 Single Sign On**.
    
    b. In de **SSO URL** vak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd uit de Azure-portal.
    
    c. In de **aanmeldings-URL voor eenmalige aanmelding** vak, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd uit de Azure-portal.
    
    d. In de **afmeldings-URL voor eenmalige aanmelding** vak, plak de **afmelding URL** waarde die u hebt gekopieerd uit de Azure-portal.

1. In de **bericht handtekening verificatiemethode** sectie, de volgende stappen uit:

    ![Bericht handtekening verificatiemethode](./media/central-desktop-tutorial/ic769565.png "bericht handtekening verificatiemethode") een. Selecteer **Certificaat**.
    
    b. In de **SSO-certificaat** in de lijst met **RSH SHA256**.
    
    c. Open het gedownloade certificaat in Kladblok. Vervolgens kopieert u de inhoud van het certificaat en plak deze in de **SSO-certificaat** veld.
        
    d. Selecteer **een koppeling weergegeven naar de aanmeldingspagina van uw SAMLv2**.
    
    e. Selecteer **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie op [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop Azure Active Directory](./media/central-desktop-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/central-desktop-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/central-desktop-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster Gebruiker](./media/central-desktop-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-central-desktop-test-user"></a>Maak een testgebruiker centrale Desktop

Voor Azure AD-gebruikers kunnen zich aanmelden, moeten ze worden ingericht in de centrale Desktop-toepassing. In deze sectie wordt beschreven hoe u Azure AD-gebruikersaccounts maken in Centraal Desktop.

> [!NOTE]
> Voor het inrichten van gebruikersaccounts van de Azure AD, kunt u een andere hulpprogramma's voor centraal bureaublad gebruiker-account maken of API's die worden geboden door centrale Desktop.

**Voor het inrichten van gebruikersaccounts aan centrale bureaublad:**

1. Aanmelden bij uw tenant centrale Desktop.

1. Ga naar **mensen** > **interne leden**.

1. Selecteer **interne leden toevoegen**.

    ![People](./media/central-desktop-tutorial/ic781051.png "People")
    
1. In de **e-mailadres van de nieuwe leden** typt u een Azure AD-account dat u wilt inrichten, en selecteer vervolgens **volgende**.

    ![E-mailadressen van de nieuwe leden](./media/central-desktop-tutorial/ic781052.png "e-mailadressen van de nieuwe leden")

1. Selecteer **interne toevoegen leden**.

    ![Interne lid toevoegen](./media/central-desktop-tutorial/ic781053.png "interne lid toevoegen")
   
   >[!NOTE]
   >De gebruikers die u ontvangt een e-mail met een bevestigingskoppeling voor het activeren van hun account.
   
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon Azure eenmalige aanmelding gebruiken ze door toegang te verlenen aan centrale bureaublad.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon voor centrale Desktop, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure-portal. Ga naar de directoryweergave en ga vervolgens naar **bedrijfstoepassingen**.

1. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **centrale Desktop**.

    ![De centrale Desktop-koppeling in de lijst met toepassingen](./media/central-desktop-tutorial/tutorial_centraldesktop_app.png)  

1. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202]

1. Selecteer de knop **Add**. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster Toewijzing toevoegen][203]

1. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst.

1. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

1. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie, test u uw Azure AD eenmalige aanmelding-configuratie met behulp van het toegangsvenster.

Wanneer u de tegel centrale Desktop in het toegangsvenster selecteert, ophalen u automatisch aangemeld bij uw centrale Desktop-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/central-desktop-tutorial/tutorial_general_203.png

