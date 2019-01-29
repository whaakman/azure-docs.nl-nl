---
title: 'Zelfstudie: Azure Active Directory-integratie met Palo Alto Networks - scherpstelling | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Palo Alto Networks - scherpstelling.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a5ea18d3-3aaf-4bc6-957c-783e9371d0f1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 7f94921126900c8e124cd41bd2e4f913b28396a2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153671"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---aperture"></a>Zelfstudie: Azure Active Directory-integratie met Palo Alto Networks - scherpstelling

In deze zelfstudie leert u hoe u integreert Palo Alto Networks - scherpstelling met Azure Active Directory (Azure AD).

Integratie van Palo Alto Networks - scherpstelling met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Palo Alto Networks - scherpstelling heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Palo Alto Networks - scherpstelling (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Palo Alto Networks - scherpstelling, moet u de volgende items:

- Een Azure AD-abonnement
- Een Palo Alto Networks - scherpstelling eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Palo Alto Networks - scherpstelling uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-palo-alto-networks---aperture-from-the-gallery"></a>Palo Alto Networks - scherpstelling uit de galerie toevoegen
Voor het configureren van de integratie van Palo Alto Networks - scherpstelling in Azure AD, moet u Palo Alto Networks - scherpstelling uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen van Palo Alto Networks - scherpstelling uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Palo Alto Networks - scherpstelling**, selecteer **Palo Alto Networks - scherpstelling** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Palo Alto Networks - scherpstelling in de lijst met resultaten](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen op Azure AD eenmalige aanmelding met Palo Alto Networks - scherpstelling op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Palo Alto Networks - scherpstelling is aan een gebruiker in Azure AD. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Palo Alto Networks - scherpstelling moet tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Palo Alto Networks - scherpstelling, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maken van een Palo Alto Networks - scherpstelling testgebruiker](#create-a-palo-alto-networks---aperture-test-user)**  : als u wilt een equivalent van Britta Simon in Palo Alto Networks - scherpstelling die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw Palo Alto Networks - scherpstelling toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Palo Alto Networks - scherpstelling, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Palo Alto Networks - scherpstelling** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_samlbase.png)

1. Op de **Palo Alto Networks - scherpstelling domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Palo Alto Networks - scherpstelling domein en URL's, eenmalige aanmelding informatie](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/auth`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Palo Alto Networks - scherpstelling domein en URL's, eenmalige aanmelding informatie](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/sign_in`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Palo Alto Networks - ondersteuningsteam scherpstelling Client](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/paloaltonetworks-aperture-tutorial/tutorial_general_400.png)


1. Op de **Palo Alto Networks - scherpstelling configuratie** sectie, klikt u op **configureren Palo Alto Networks - scherpstelling** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![De koppeling configureren](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_configure.png)

1. In een ander browservenster, meld u aan bij Palo Alto Networks - scherpstelling als beheerder.

1. Klik op de bovenste menubalk **instellingen**.

    ![Het tabblad instellingen](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_settings.png)

1. Navigeer naar **toepassing** sectie Klik **verificatie** vormen aan de linkerkant van het menu.

    ![Het tabblad Auth](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_auth.png)
    
1. Op de **verificatie** pagina de volgende stappen uitvoeren:
    
    ![Het tabblad verificatie](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_singlesignon.png)

    a. Controleer de **inschakelen Single Sign-On(Supported SSP Providers are Okta, Onelogin)** van **Single Sign-On** veld.

    b. In de **Identity Provider-ID** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **bestand kiezen** voor het uploaden van het gedownloade certificaat uit Azure AD in de **Provider identiteitscertificaat** veld.

    d. In de **SSO-URL van de id-Provider** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    e. Lees de informatie van de IdP vanuit **scherpstelling Info** sectie en downloaden van het certificaat uit **scherpstelling sleutel** veld.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/paloaltonetworks-aperture-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/paloaltonetworks-aperture-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/paloaltonetworks-aperture-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/paloaltonetworks-aperture-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-palo-alto-networks---aperture-test-user"></a>Een Palo Alto Networks - scherpstelling testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Palo Alto Networks - scherpstelling. Werken met [Palo Alto Networks - ondersteuningsteam scherpstelling Client](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) om toe te voegen de gebruikers in de Palo Alto Networks - scherpstelling platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Palo Alto Networks - scherpstelling.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen Palo Alto Networks - scherpstelling, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Palo Alto Networks - scherpstelling**.

    ![De Palo Alto Networks - scherpstelling koppeling in de lijst met toepassingen](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op het Palo Alto Networks - scherpstelling tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Palo Alto Networks - scherpstelling toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_01.png
[2]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_02.png
[3]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_03.png
[4]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_04.png

[100]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_100.png

[200]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_200.png
[201]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_201.png
[202]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_202.png
[203]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_203.png

