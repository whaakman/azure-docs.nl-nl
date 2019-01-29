---
title: 'Zelfstudie: Azure Active Directory-integratie met Envi MMIS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Envi MMIS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: b85dc27f6b6a23be6dc89a0f0a7cf9f78681446d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197446"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Zelfstudie: Azure Active Directory-integratie met Envi MMIS

In deze zelfstudie leert u hoe u Envi MMIS integreren met Azure Active Directory (Azure AD).

Envi MMIS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Envi MMIS heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Envi MMIS (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Envi MMIS, moet u de volgende items:

- Een Azure AD-abonnement
- Een Envi MMIS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Envi MMIS uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-envi-mmis-from-the-gallery"></a>Envi MMIS uit de galerie toe te voegen
Voor het configureren van de integratie van Envi MMIS in Azure AD, moet u Envi MMIS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Envi MMIS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Envi MMIS**, selecteer **Envi MMIS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Envi MMIS in de lijst met resultaten](./media/envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Envi MMIS op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Envi MMIS is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Envi MMIS tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Envi MMIS, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Envi MMIS](#create-an-envi-mmis-test-user)**  : als u wilt een equivalent van Britta Simon in Envi MMIS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Envi MMIS.

**Voor het configureren van Azure AD eenmalige aanmelding met Envi MMIS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Envi MMIS** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/envimmis-tutorial/tutorial_envimmis_samlbase.png)

1. Op de **landen MMIS domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Landen MMIS domein en URL's, eenmalige aanmelding informatie](./media/envimmis-tutorial/tutorial_envimmis_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Landen MMIS domein en URL's, eenmalige aanmelding informatie](./media/envimmis-tutorial/tutorial_envimmis_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [landen MMIS Client ondersteuningsteam](mailto:support@ioscorp.com) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/envimmis-tutorial/tutorial_envimmis_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/tutorial_general_400.png)

1. Meld u in een ander browservenster naar uw site Envi MMIS als beheerder.

1. Klik op **mijn domein** tabblad.

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/configure1.png)

1. Klik op **Bewerken**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/configure2.png)

1. Selecteer **externe verificatie gebruiken** selectievakje in en selecteer vervolgens **HTTP-omleiding** uit de **verificatietype** vervolgkeuzelijst.

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/configure3.png)

1. Selecteer **Resources** tabblad en klik vervolgens op **metagegevens uploaden**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/configure4.png)

1. In de **metagegevens uploaden** pop-upvenster de volgende stappen uitvoeren:

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/configure5.png)

    a. Selecteer **bestand** optie uit de **uploaden uit** vervolgkeuzelijst.

    b. De gedownloade metagegevensbestand uploaden vanuit Azure portal door te selecteren de **Kies het bestandspictogram**.

    c. Klik op **OK**.

1. Na het uploaden van het gedownloade metagegevensbestand wordt ophalen automatisch de velden ingevuld. Klik op **Update**

    ![De knop voor enkelvoudige aanmelding configureren](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/envimmis-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/envimmis-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/envimmis-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/envimmis-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-envi-mmis-test-user"></a>Maak een testgebruiker Envi MMIS

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Envi MMIS, moeten ze worden ingericht voor Envi MMIS.  
In het geval van Envi MMIS is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf Envi MMIS site aan als beheerder.

1. Klik op **gebruikerslijst** tabblad.

    ![Werknemer toevoegen](./media/envimmis-tutorial/user1.png)

1. Klik op **gebruiker toevoegen** knop.

    ![Werknemer toevoegen](./media/envimmis-tutorial/user2.png)

1. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/envimmis-tutorial/user3.png)

    a. In de **gebruikersnaam** tekstvak typt u de gebruikersnaam van Britta Simon account, zoals **brittasimon@contoso.com**.
    
    b. In de **voornaam** tekstvak, type de voornaam van BrittaSimon zoals **Julia**.

    c. In de **achternaam** tekstvak, type de achternaam van BrittaSimon zoals **Simon**.

    d. Voer de titel van de gebruiker in de **titel** van het tekstvak.
    
    e. In de **e-mailadres** tekstvak typt u het e-mailadres van Britta Simon account, zoals **brittasimon@contoso.com**.

    f. In de **SSO-gebruikersnaam** tekstvak typt u de gebruikersnaam van Britta Simon account, zoals **brittasimon@contoso.com**.

    g. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Envi MMIS.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Envi MMIS toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Envi MMIS**.

    ![De koppeling Envi MMIS in de lijst met toepassingen](./media/envimmis-tutorial/tutorial_envimmis_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Envi MMIS in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Envi MMIS.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/envimmis-tutorial/tutorial_general_01.png
[2]: ./media/envimmis-tutorial/tutorial_general_02.png
[3]: ./media/envimmis-tutorial/tutorial_general_03.png
[4]: ./media/envimmis-tutorial/tutorial_general_04.png

[100]: ./media/envimmis-tutorial/tutorial_general_100.png

[200]: ./media/envimmis-tutorial/tutorial_general_200.png
[201]: ./media/envimmis-tutorial/tutorial_general_201.png
[202]: ./media/envimmis-tutorial/tutorial_general_202.png
[203]: ./media/envimmis-tutorial/tutorial_general_203.png

