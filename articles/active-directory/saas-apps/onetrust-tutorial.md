---
title: 'Zelfstudie: Azure Active Directory-integratie met OneTrust Privacy beheersoftware | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en beheersoftware voor OneTrust Privacy.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71c2b6d0-3d28-4130-a2c8-1e72ab3d5814
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jeedes
ms.openlocfilehash: 46480119579513839024d89e7657661e12e5509c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167288"
---
# <a name="tutorial-azure-active-directory-integration-with-onetrust-privacy-management-software"></a>Zelfstudie: Azure Active Directory-integratie met beheersoftware voor OneTrust Privacy

In deze zelfstudie leert u hoe u Software voor het beheer van Privacy OneTrust integreren met Azure Active Directory (Azure AD).

Beheersoftware voor OneTrust Privacy integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Software voor beheer van OneTrust Privacy heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij OneTrust Privacy beheersoftware (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met OneTrust Privacy beheersoftware, moet u de volgende items:

- Een Azure AD-abonnement
- Een OneTrust Privacy beheersoftware eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Beheersoftware voor OneTrust Privacy uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-onetrust-privacy-management-software-from-the-gallery"></a>Beheersoftware voor OneTrust Privacy uit de galerie toe te voegen
Voor het configureren van de integratie van Software voor het beheer van Privacy OneTrust in Azure AD, moet u OneTrust Privacy Management Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen OneTrust Privacy-beheersoftware uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **OneTrust Privacy beheersoftware**, selecteer **OneTrust Privacy beheersoftware** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Beheersoftware voor OneTrust Privacy in de lijst met resultaten](./media/onetrust-tutorial/tutorial_onetrust_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Software voor beheer van OneTrust Privacy op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in OneTrust Privacy-beheersoftware is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in OneTrust Privacy-beheersoftware tot stand worden gebracht.

In OneTrust beheersoftware voor Privacy, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met OneTrust Privacy beheersoftware, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker OneTrust Privacy beheersoftware](#create-a-onetrust-privacy-management-software-test-user)**  : als u wilt een equivalent van Britta Simon in OneTrust Privacy Software die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing beheersoftware voor OneTrust Privacy.

**Voor het configureren van Azure AD eenmalige aanmelding met OneTrust Privacy beheersoftware, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **OneTrust Privacy beheersoftware** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/onetrust-tutorial/tutorial_onetrust_samlbase.png)

1. Op de **OneTrust Privacy Management Software domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![OneTrust Privacy Management Software domein en URL's, eenmalige aanmelding informatie](./media/onetrust-tutorial/tutorial_onetrust_url.png)

    a. In de **id** tekstvak, een URL typen: `https://www.onetrust.com/saml2`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<subdomain>.onetrust.com/auth/consumerservice`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![OneTrust Privacy Management Software domein en URL's, eenmalige aanmelding informatie](./media/onetrust-tutorial/tutorial_onetrust_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.onetrust.com/auth/login`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [OneTrust Privacy Beheerclient Software ondersteuningsteam](mailto:support@onetrust.com) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/onetrust-tutorial/tutorial_onetrust_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/onetrust-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **OneTrust Privacy beheersoftware** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [OneTrust Privacy beheersoftware ondersteuningsteam](mailto:support@onetrust.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/onetrust-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/onetrust-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/onetrust-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/onetrust-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-onetrust-privacy-management-software-test-user"></a>Maak een testgebruiker OneTrust Privacy-beheersoftware

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in beheersoftware voor OneTrust Privacy. Beheersoftware voor OneTrust Privacy ondersteunt just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Software voor het beheer van Privacy OneTrust als deze nog niet bestaat.

>[!Note]
>Als u wilt maken van een gebruiker handmatig, neem contact op met [OneTrust Privacy beheersoftware ondersteuningsteam](mailto:support@onetrust.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan OneTrust Privacy beheersoftware.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen OneTrust Privacy beheersoftware, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **OneTrust Privacy beheersoftware**.

    ![De koppeling beheersoftware voor OneTrust Privacy in de lijst met toepassingen](./media/onetrust-tutorial/tutorial_onetrust_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel beheersoftware voor OneTrust Privacy in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing beheersoftware voor OneTrust Privacy.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/onetrust-tutorial/tutorial_general_01.png
[2]: ./media/onetrust-tutorial/tutorial_general_02.png
[3]: ./media/onetrust-tutorial/tutorial_general_03.png
[4]: ./media/onetrust-tutorial/tutorial_general_04.png

[100]: ./media/onetrust-tutorial/tutorial_general_100.png

[200]: ./media/onetrust-tutorial/tutorial_general_200.png
[201]: ./media/onetrust-tutorial/tutorial_general_201.png
[202]: ./media/onetrust-tutorial/tutorial_general_202.png
[203]: ./media/onetrust-tutorial/tutorial_general_203.png

