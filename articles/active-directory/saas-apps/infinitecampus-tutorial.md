---
title: 'Zelfstudie: Azure Active Directory-integratie met oneindige Campus | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en oneindige Campus.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53eb0c4ad5c0a21f46985062ef8202a87dc0d5e6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189358"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Zelfstudie: Azure Active Directory-integratie met oneindige Campus

In deze zelfstudie leert u hoe u oneindige Campus integreren met Azure Active Directory (Azure AD).

Oneindige Campus integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot oneindige Campus heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij oneindige Campus (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met oneindige Campus, moet u de volgende items:

- Een Azure AD-abonnement
- Een oneindige Campus eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).
- Ten minste moet u een Azure Active Directory-beheerder om de configuratie te voltooien.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Oneindige Campus uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-infinite-campus-from-the-gallery"></a>Oneindige Campus uit de galerie toe te voegen

Voor het configureren van de integratie van oneindige Campus in Azure AD, moet u oneindige Campus uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen oneindige Campus uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **oneindige Campus**, selecteer **oneindige Campus** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Oneindige Campus in de lijst met resultaten](./media/infinitecampus-tutorial/tutorial_infinitecampus_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met oneindige Campus op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in oneindige Campus is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in een oneindige Campus tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met oneindige Campus, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker oneindige Campus](#creating-a-infinite-campus-test-user)**  : als u wilt een equivalent van Britta Simon in oneindige Campus die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing oneindige Campus.

**Voor het configureren van Azure AD eenmalige aanmelding met oneindige Campus, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **oneindige Campus** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, hebt u een **Service Provider-bestand met metagegevens**volledige 4. via 4.d stappen en ga vervolgens verder 11.c stap wilt uitvoeren. Als u een bestand met metagegevens voor Service Provider geen hebt, gaat u naar stap 5.

    a. Klik op **metagegevensbestand uploaden**.

        ![image](common/b9_saml.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![image](common/b9(1)_saml.png)

    c. Zodra het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **SAML-basisconfiguratie** sectie tekstvak, zoals hieronder wordt weergegeven :

    ![image](./media/infinitecampus-tutorial/tutorial_infinitecampus_url.png)

    d. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon (het domein kan verschillen met het Model die als host fungeert): `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

5. Als u nog geen **Service Provider-bestand met metagegevens**, voer de volgende stappen uit (Let erop dat het domein naargelang het Model die als host fungeert verschillen):

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![Oneindige Campus-domein en URL's, eenmalige aanmelding informatie](./media/infinitecampus-tutorial/tutorial_infinitecampus_url1.png)

6. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op de kopiëren **pictogram** kopiëren **App federatieve metagegevens Url**  en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/infinitecampus-tutorial/tutorial_infinitecampus_certificate.png) 

7. Op de **instellen op Infinite Campus** sectie, gebruikt u de volgende waarden om te valideren bij uploaden of met behulp van de Azure metadata-bestand/URL.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![Oneindige Campus-configuratie](common/configuresection.png)

8. In een ander browservenster, meld u aan bij oneindige Campus als een beveiligingsbeheerder.

9. Aan de linkerkant van het menu, klikt u op **Systeembeheer**.

    ![De beheerder](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

10. Navigeer naar **gebruikersbeveiliging** > **SAML Management** > **SSO-configuratie van serviceprovider**.

    ![De saml](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

11. Op de **SSO-configuratie van serviceprovider** pagina, voert u de volgende stappen uit:

    ![De eenmalige aanmelding](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. Selecteer **inschakelen SAML-eenmalige aanmelding**.
    
    b. Op de **een optie voor het ophalen van gegevens van de id-Provider (IDP)-server** sectie, selecteer **metagegevens-URL**, plak de **App-Url voor federatieve metagegevens** in het vak en vervolgens Klik op **synchronisatie**.

    c. Klikt u op **Service Provider metagegevens** koppeling op te slaan de **Service Provider-bestand met metagegevens** op uw computer, en upload dit in **SAML-basisconfiguratie** sectie automatisch Vul de **id** en **antwoord-URL** waarden in de Azure-portal (Zie stap 4 voor uploaden en automatische populatie van waarden of stap 5 voor handmatige invoer).

    d. Nadat u hebt geklikt **synchronisatie** de waarden ophalen automatisch ingevuld in **SSO-configuratie van serviceprovider** pagina.

    e. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een _één_ testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-infinite-campus-test-user"></a>Het maken van een oneindige Campus-testgebruiker

Oneindige Campus heeft een architectuur met demografische gegevens die zijn gericht. Neem contact op met [oneindige Campus-ondersteuningsteam](mailto:sales@infinitecampus.com) om toe te voegen de gebruikers in het oneindige Campus-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan oneindige Campus.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **oneindige Campus**.

    ![Eenmalige aanmelding configureren](./media/infinitecampus-tutorial/tutorial_infinitecampus_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel oneindige Campus in het toegangsvenster, u moet u automatisch aangemeld bij uw oneindige Campus-toepassing. Als u zich bij de toepassing oneindige Camnpus in de browser die u bij het beheer van Azure AD, controleert u of dat u bent aangemeld bij Azure AD als de testgebruiker. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
