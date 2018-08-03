---
title: 'Zelfstudie: Azure Active Directory-integratie met QPrism | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en QPrism.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 72ab75ba-132b-4f83-a34b-d28b81b6d7bc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: ddf22491d7531daecf4448e62e8594c3326d7b77
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420271"
---
# <a name="tutorial-azure-active-directory-integration-with-qprism"></a>Zelfstudie: Azure Active Directory-integratie met QPrism

In deze zelfstudie leert u hoe u QPrism integreren met Azure Active Directory (Azure AD).

QPrism integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot QPrism heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld op QPrism (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met QPrism, moet u de volgende items:

- Een Azure AD-abonnement
- Een QPrism eenmalige aanmelding ingeschakeld abonnement

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. QPrism uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-qprism-from-the-gallery"></a>QPrism uit de galerie toevoegen
Voor het configureren van de integratie van QPrism in Azure AD, moet u QPrism uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**QPrism uit de galerie toevoegen:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Selecteer om een nieuwe toepassing toe aan de bovenkant van het dialoogvenster **nieuwe toepassing**.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **QPrism**, en selecteer **QPrism** van deelvenster met resultaten. Klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![QPrism in de lijst met resultaten](./media/qprism-tutorial/tutorial_qprism_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met QPrism, op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in QPrism is aan een gebruiker in Azure AD. Met andere woorden, moet er een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in QPrism.

Voor het maken van deze relatie in QPrism, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam**.

Als u wilt configureren en Azure AD eenmalige aanmelding met QPrism testen, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
1. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. [Maak een testgebruiker QPrism](#create-a-qprism-test-user) hebben een equivalent van Britta Simon in QPrism die gekoppeld is aan de Azure AD-weergave van de gebruiker.
1. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie kunt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing QPrism.

1. In de Azure-portal op de **QPrism** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** in het dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/qprism-tutorial/tutorial_qprism_samlbase.png)

1. In de **QPrism domein en URL's** sectie, doet u het volgende:

    ![QPrism domein en URL's, eenmalige aanmelding informatie](./media/qprism-tutorial/tutorial_qprism_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u een URL die gebruikmaakt van het volgende patroon: `https://<customer domain>.qmyzone.com/login`

    b. In de **id** tekstvak typt u een URL die gebruikmaakt van het volgende patroon: `https://<customer domain>.qmyzone.com/metadata.php`
         
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id en aanmeldings-URL. Neem contact op met [QPrism Client ondersteuningsteam](mailto:qsupport-ce@quatrro.com) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

     ![De downloadkoppeling certificaat](./media/qprism-tutorial/tutorial_qprism_certificate.png)

1. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/qprism-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op **QPrism** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** naar [QPrism ondersteuningsteam](mailto:qsupport-ce@quatrro.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Een testgebruiker maken in Azure AD:**

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/qprism-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/qprism-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, aan de bovenkant van de **alle gebruikers** in het dialoogvenster, selecteer **toevoegen**.

    ![De knop toevoegen](./media/qprism-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster de volgende handelingen uit:

    ![Het dialoogvenster gebruiker](./media/qprism-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-qprism-test-user"></a>Maak een testgebruiker QPrism

In deze sectie maakt u een gebruiker met de naam van Britta Simon in QPrism. Werken met de [QPrism ondersteuningsteam](mailto:qsupport-ce@quatrro.com) gebruikers toevoegen in het QPrism-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan QPrism.

![De de gebruikersrol toewijzen][200] 

**Britta Simon toewijzen aan QPrism:**

1. In de Azure-portal, opent u de weergave van toepassingen en navigeer vervolgens naar de directoryweergave. Ga naar **bedrijfstoepassingen**, en selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **QPrism**.

    ![De koppeling QPrism in de lijst met toepassingen](./media/qprism-tutorial/tutorial_qprism_app.png)  

1. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Selecteer **Toevoegen**. Klik vervolgens onder **toevoegen toewijzing**, selecteer **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst.

1. Op de **gebruikers en groepen** in het dialoogvenster, selecteer **Selecteer**.

1. Onder **toevoegen toewijzing**, selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

In het toegangsvenster, wanneer u de tegel QPrism selecteert moet u ophalen automatisch aangemeld voor uw toepassing QPrism.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/qprism-tutorial/tutorial_general_01.png
[2]: ./media/qprism-tutorial/tutorial_general_02.png
[3]: ./media/qprism-tutorial/tutorial_general_03.png
[4]: ./media/qprism-tutorial/tutorial_general_04.png

[100]: ./media/qprism-tutorial/tutorial_general_100.png

[200]: ./media/qprism-tutorial/tutorial_general_200.png
[201]: ./media/qprism-tutorial/tutorial_general_201.png
[202]: ./media/qprism-tutorial/tutorial_general_202.png
[203]: ./media/qprism-tutorial/tutorial_general_203.png

