---
title: 'Zelfstudie: Azure Active Directory-integratie met Infor detailhandel – Information Management | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Infor detailhandel – Information Management.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ca5dc17391dd86444a43331c0b161bb375a1ab58
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178559"
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a>Zelfstudie: Azure Active Directory-integratie met Infor detailhandel – Information Management

In deze zelfstudie leert u hoe u integreert Infor detailhandel – Information Management bij Azure Active Directory (Azure AD).

Integratie van Infor detailhandel – Information Management met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Infor detailhandel – Information Management heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Infor detailhandel – Information Management (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Infor detailhandel – Information Management, moet u de volgende items:

- Een Azure AD-abonnement
- Een Infor detailhandel – Information Management eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Infor detailhandel – Information Management vanuit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-infor-retail--information-management-from-the-gallery"></a>Infor detailhandel – Information Management vanuit de galerie toevoegen
Als u wilt configureren van de integratie van Infor detailhandel – Information Management in Azure AD, moet u Infor detailhandel – Information Management vanuit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, Infor detailhandel – Information Management vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Infor detailhandel – Information Management**, selecteer **Infor detailhandel – Information Management** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Infor detailhandel – Information Management in de lijst met resultaten](./media/inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Infor detailhandel – Information Management op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Infor detailhandel – Information Management is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Infor detailhandel – Information Management tot stand worden gebracht.

In de Infor detailhandel – Information Management, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Infor detailhandel – Information Management, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maken van een handelsversie Infor – Information Management testgebruiker](#create-an-infor-retail--information-management-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de Infor detailhandel – Information Management die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Infor detailhandel – Information Management-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Infor detailhandel – Information Management, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Infor detailhandel – Information Management** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

1. Op de **Infor detailhandel – informatie Management domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![Infor detailhandel – informatie Management domein en URL's één informatie-aanmeldings-id-provider](./media/inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    a. In het tekstvak **Id** typt u een URL met de volgende patronen: 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<company name>.mingle.infor.com/sp/ACS.saml2`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Infor detailhandel – informatie beheerdomein en URL's één aanmelding informatie SP](./media/inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<company name>.mingle.infor.com/<company code>`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Infor detailhandel – informatie Beheerclient ondersteuningsteam](mailto:innovate@infor.com) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op **Infor detailhandel – Information Management** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Infor detailhandel – Information Management ondersteuningsteam](mailto:innovate@infor.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/inforretailinformationmanagement-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/inforretailinformationmanagement-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/inforretailinformationmanagement-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-infor-retail--information-management-test-user"></a>Een Infor detailhandel – Information Management testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Infor detailhandel – Information Management. Neem contact op met [Infor Retail-ondersteuningsteam voor informatiebeheer](mailto:innovate@infor.com) om toe te voegen de gebruikers in de detailhandel Infor – Information Management-platform.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Infor detailhandel – Information Management.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar de handelsversie Infor – Information Management, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Infor detailhandel – Information Management**.

    ![De Infor detailhandel – Information Management koppeling in de lijst met toepassingen](./media/inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de Infor detailhandel – Information Management-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Infor detailhandel – Information Management-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/inforretailinformationmanagement-tutorial/tutorial_general_203.png

