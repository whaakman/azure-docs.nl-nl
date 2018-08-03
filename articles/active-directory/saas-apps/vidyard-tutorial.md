---
title: 'Zelfstudie: Azure Active Directory-integratie met Vidyard | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Vidyard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 871942db15d6a3cff45584e33b2191e21d2281a0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426452"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Zelfstudie: Azure Active Directory-integratie met Vidyard

In deze zelfstudie leert u hoe u Vidyard integreren met Azure Active Directory (Azure AD).

Vidyard integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Vidyard heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Vidyard (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Vidyard, moet u de volgende items:

- Een Azure AD-abonnement
- Een Vidyard eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vidyard uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-vidyard-from-the-gallery"></a>Vidyard uit de galerie toe te voegen
Voor het configureren van de integratie van Vidyard in Azure AD, moet u Vidyard uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Vidyard uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Vidyard**, selecteer **Vidyard** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Vidyard in de lijst met resultaten](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Vidyard op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Vidyard is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Vidyard tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Vidyard, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Vidyard](#create-a-vidyard-test-user)**  : als u wilt een equivalent van Britta Simon in Vidyard die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Vidyard.

**Voor het configureren van Azure AD eenmalige aanmelding met Vidyard, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Vidyard** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

1. Op de **Vidyard domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Vidyard domein en URL's, eenmalige aanmelding informatie](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Vidyard domein en URL's, eenmalige aanmelding informatie](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Deze waarden zijn niet echt. U kunt deze waarden wordt bijgewerkt met de werkelijke-id, de antwoord-URL en aanmeldings-URL, die later in de zelfstudie wordt uitgelegd

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/vidyard-tutorial/tutorial_general_400.png)

1. Op de **Vidyard configuratie** sectie, klikt u op **configureren Vidyard** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Vidyard configuratie](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

1. In een ander browservenster aanmelden bij uw bedrijf Vidyard Software site als beheerder.

1. Selecteer in het dashboard Vidyard **groep** > **beveiliging**

    ![Vidyard configuratie](./media/vidyard-tutorial/configure1.png)

1. Klik op **nieuw profiel** tabblad.

    ![Vidyard configuratie](./media/vidyard-tutorial/configure2.png)

1. In de **SAML-configuratie** sectie, voert u de volgende stappen uit:

    ![Vidyard configuratie](./media/vidyard-tutorial/configure3.png)

    a. Voer de naam van het algemene profiel in de **profielnaam** tekstvak.

    b. Kopie **aanmeldingspagina voor eenmalige aanmelding gebruiker** waarde en plak deze in **aanmeldings-URL** -tekstvak in **Vidyard domein en URL's sectie** in Azure portal.

    c. Kopie **ACS URL** waarde en plak deze in **antwoord-URL** -tekstvak in **Vidyard domein en URL's sectie** in Azure portal.

    d. Kopie **URL-verlener/metagegevens** waarde en plak deze in **id** -tekstvak in **Vidyard domein en URL's sectie** in Azure portal.

    e. Open het gedownloade certificaat-bestand vanuit Azure portal in Kladblok en plak deze in de **X.509-certificaat** tekstvak.

    f. In de **eindpunt-URL voor SAML** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** gekopieerd vanuit Azure portal.

    g. Klik op **Bevestigen**.

1. Selecteer in het tabblad Single Sign On **toewijzen** naast een bestaand profiel

    ![Vidyard configuratie](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Wanneer u een SSO-profiel hebt gemaakt, kunt u deze op alle gebruikers waarvoor toegang via Azure moet groep(en) toewijzen. Als de gebruiker niet in de groep waaraan ze zijn toegewezen bestaat, wordt automatisch Vidyard een gebruikersaccount maken en toewijzen van hun rol in realtime.

1. Selecteer de groep van uw organisatie, die zichtbaar is in de **beschikbare groepen toe te wijzen**.

    ![Vidyard configuratie](./media/vidyard-tutorial/configure5.png)

1. U ziet de toegewezen groepen onder de **momenteel groepen toegewezen**. Selecteer een rol voor de groep aan de hand van uw organisatie en klik op **bevestigen**.

    ![Vidyard configuratie](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Raadpleeg voor meer informatie, [dit document](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/vidyard-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/vidyard-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/vidyard-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/vidyard-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-vidyard-test-user"></a>Maak een testgebruiker Vidyard

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Vidyard. Vidyard biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Vidyard als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Vidyard ondersteuningsteam](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Vidyard.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Vidyard toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Vidyard**.

    ![De koppeling Vidyard in de lijst met toepassingen](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Vidyard in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Vidyard.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

