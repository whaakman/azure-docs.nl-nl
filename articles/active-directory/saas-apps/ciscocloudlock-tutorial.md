---
title: 'Zelfstudie: Azure Active Directory-integratie met de Cloud Security Fabric | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de Cloud Security Fabric.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 549e8810-1b3b-4351-bf4b-f07de98980d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 5ec729c6f82cec503cae2fa057f5842849004ac7
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318242"
---
# <a name="tutorial-azure-active-directory-integration-with-the-cloud-security-fabric"></a>Zelfstudie: Azure Active Directory-integratie met de Cloud Security Fabric

In deze zelfstudie leert u hoe de beveiliging Cloudfabric integreren met Azure Active Directory (Azure AD).

De Cloud Security Fabric integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Cloud Security Fabric heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de beveiliging van Cloud-Infrastructuurresources (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de Cloud Security-Fabric, moet u de volgende items:

- Een Azure AD-abonnement
- Een infrastructuur voor de Cloud Security eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. De Cloud Security Fabric uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-the-cloud-security-fabric-from-the-gallery"></a>De Cloud Security Fabric uit de galerie toevoegen
Voor het configureren van de integratie van de Cloud Security Fabric in Azure AD, moet u de beveiliging Cloudfabric uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de Cloud Security Fabric uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **beveiliging van de Cloudfabric**, selecteer **Fabric van de Cloud Security** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![De Cloud Security Fabric in de lijst met resultaten](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met de Cloud Security infrastructuur op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de Cloud Security Fabric is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Cloud Security Fabric tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met de Cloud Security-Fabric, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Fabric van de Cloud Security](#create-a-the-cloud-security-fabric-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de Cloud Security-Fabric die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing de Cloudfabric beveiliging configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met de Cloud Security-Fabric, moet u de volgende stappen uitvoeren:**

1. In de Azure portal op de **beveiliging van de Cloudfabric** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_samlbase.png)

3. Op de **URL's en de Cloud Fabric beveiligingsdomein** sectie, voert u de volgende stappen uit:

    ![De URL's en Cloud Fabric beveiligingsdomein één aanmelding informatie](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL:
    | |
    |--|
    | `https://platform.cloudlock.com` |
    | `https://app.cloudlock.com` |

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://platform.cloudlock.com/gate/saml/sso/<subdomain>` |
    | `https://app.cloudlock.com/gate/saml/sso/<subdomain>` |

    > [!NOTE]
    > De id-waarde is geen echte. Werk de waarde met de werkelijke identificatie. Neem contact op met [ondersteuningsteam van de Cloud Security Fabric-Client](mailto:support@cloudlock.com) de waarde op te halen. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_certificate.png)

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/ciscocloudlock-tutorial/tutorial_general_400.png)

6. Eenmalige aanmelding configureren op **beveiliging van de Cloudfabric** zijde, moet u de gedownloade verzenden **Metadata XML** naar [ondersteuningsteam van de Cloud Security Fabric](mailto:support@cloudlock.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/ciscocloudlock-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/ciscocloudlock-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/ciscocloudlock-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/ciscocloudlock-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-the-cloud-security-fabric-test-user"></a>Maak een testgebruiker Fabric van de Cloud-beveiliging

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in de Cloud Security Fabric maken. Werken met [ondersteuningsteam van de Cloud Security Fabric](mailto:support@cloudlock.com) om toe te voegen de gebruikers van het platform van de Cloud Security Fabric. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan de Cloud Security Fabric.

![Toewijzen van de gebruikersrol][200]

**Als u wilt Britta Simon toewijzen aan de Cloud Security-Fabric, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **beveiliging van de Cloudfabric**.

    ![De beveiliging van de Cloudfabric-koppeling in de lijst met toepassingen](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u de beveiliging van de Cloudfabric-tegel in het deelvenster toegang op, u moet ophalen automatisch aangemeld bij uw Fabric van de Cloud Security-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ciscocloudlock-tutorial/tutorial_general_01.png
[2]: ./media/ciscocloudlock-tutorial/tutorial_general_02.png
[3]: ./media/ciscocloudlock-tutorial/tutorial_general_03.png
[4]: ./media/ciscocloudlock-tutorial/tutorial_general_04.png

[100]: ./media/ciscocloudlock-tutorial/tutorial_general_100.png

[200]: ./media/ciscocloudlock-tutorial/tutorial_general_200.png
[201]: ./media/ciscocloudlock-tutorial/tutorial_general_201.png
[202]: ./media/ciscocloudlock-tutorial/tutorial_general_202.png
[203]: ./media/ciscocloudlock-tutorial/tutorial_general_203.png
