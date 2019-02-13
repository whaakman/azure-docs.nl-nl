---
title: 'Zelfstudie: Azure Active Directory-integratie met Vodeclic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Vodeclic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3dcd39d58089b202d9e9d61cfc5d25e12ff7a6b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217765"
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Zelfstudie: Azure Active Directory-integratie met Vodeclic

In deze zelfstudie leert u hoe u Vodeclic integreren met Azure Active Directory (Azure AD).

Vodeclic integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Vodeclic heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld op Vodeclic (eenmalige aanmelding of SSO) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Vodeclic, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement Vodeclic SSO is ingeschakeld

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Als u een proefversie Azure AD-omgeving, geen [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vodeclic uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-vodeclic-from-the-gallery"></a>Vodeclic uit de galerie toevoegen
Voor het configureren van de integratie van Vodeclic in Azure AD, moet u Vodeclic uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Vodeclic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

1. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Bedrijfstoepassingen][2]
    
1. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Vodeclic**. Selecteer **Vodeclic** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Vodeclic in de lijst met resultaten](./media/vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Vodeclic op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in Vodeclic is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Vodeclic vast te stellen.

Geef de waarde in Vodeclic, **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. Nu hebt u de koppeling tussen de twee gebruikers gemaakt.

Als u wilt configureren en Azure AD eenmalige aanmelding met Vodeclic testen, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
1. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. [Maak een testgebruiker Vodeclic](#create-a-vodeclic-test-user) hebben een equivalent van Britta Simon in Vodeclic die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Vodeclic.

**Voor het configureren van Azure AD eenmalige aanmelding met Vodeclic, voert u de volgende stappen uit:**

1. In de Azure-portal op de **Vodeclic** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. In de **eenmalige aanmelding** dialoogvenster onder **Single-Sign-on modus**, selecteer **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

1. Als u wilt configureren van de toepassing in **IDP** modus gestart de **Vodeclic domein en URL's** sectie, de volgende stappen uit:

    ![Vodeclic domein en URL's, eenmalige aanmelding informatie](./media/vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. In de **id** vak, typ een URL met het volgende patroon: `https://<companyname>.lms.vodeclic.net/auth/saml`

    b. In de **antwoord-URL** vak, typ een URL met het volgende patroon: `https://<companyname>.lms.vodeclic.net/auth/saml/callback`

1. Als u wilt configureren van de toepassing in **SP** gestart modus, selecteer de **geavanceerde URL-instellingen weergeven** selectievakje en de volgende stap uitvoeren:

    ![Vodeclic domein en URL's, eenmalige aanmelding informatie](./media/vodeclic-tutorial/tutorial_vodeclic_url1.png)

    In de **aanmeldings-URL** vak, typ een URL met het volgende patroon: `https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke-id, antwoord-URL en aanmeldings-URL. Neem contact op met de [Vodeclic Client ondersteuningsteam](mailto:hotline@vodeclic.com) om deze waarden te verkrijgen.

1. In de **SAML-handtekeningcertificaat** sectie, selecteer **Metadata XML**. Sla het bestand met metagegevens op uw computer.

    ![De link om het certificaat te downloaden](./media/vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

1. Selecteer **Opslaan**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/vodeclic-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op de **Vodeclic** zijde, verzendt de gedownloade **Metadata XML** naar de [Vodeclic ondersteuningsteam](mailto:hotline@vodeclic.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie op [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop Azure Active Directory](./media/vodeclic-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/vodeclic-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/vodeclic-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster Gebruiker](./media/vodeclic-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-vodeclic-test-user"></a>Maak een testgebruiker Vodeclic

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Vodeclic. Werken met de [Vodeclic ondersteuningsteam](mailto:hotline@vodeclic.com) om toe te voegen de gebruikers in het Vodeclic-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

> [!NOTE]
> Volgens de toepassingsvereisten van moet u mogelijk uw machine in de whitelist opgenomen ophalen. Voor die worden uitgevoerd, moet u voor het delen van uw openbare IP-adres met de [Vodeclic ondersteuningsteam](mailto:hotline@vodeclic.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Vodeclic.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Vodeclic toewijst, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal, opent u de weergave van toepassingen en gaat u naar de directoryweergave. Ga vervolgens naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Vodeclic**.

    ![De koppeling Vodeclic in de lijst met toepassingen](./media/vodeclic-tutorial/tutorial_vodeclic_app.png)  

1. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202]

1. Selecteer de knop **Add**. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster Toewijzing toevoegen][203]

1. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst.

1. In de **gebruikers en groepen** in het dialoogvenster, selecteer de **Selecteer** knop.

1. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel Vodeclic in het toegangsvenster selecteert, u u automatisch aangemeld bij uw toepassing Vodeclic.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/vodeclic-tutorial/tutorial_general_203.png

