---
title: 'Zelfstudie: Azure Active Directory-integratie met iPass SmartConnect | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iPass SmartConnect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: ecfdd3fae1d394e3b57fcd325f44cad0d1a98534
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444891"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Zelfstudie: Azure Active Directory-integratie met iPass SmartConnect

In deze zelfstudie leert u hoe u iPass SmartConnect integreren met Azure Active Directory (Azure AD).

IPass SmartConnect integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iPass SmartConnect heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij iPass SmartConnect (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iPass SmartConnect, moet u de volgende items:

- Een Azure AD-abonnement
- Een iPass SmartConnect eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IPass SmartConnect uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>IPass SmartConnect uit de galerie toe te voegen
Voor het configureren van de integratie van iPass SmartConnect in Azure AD, moet u iPass SmartConnect uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iPass SmartConnect uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **iPass SmartConnect**, selecteer **iPass SmartConnect** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![iPass SmartConnect in de lijst met resultaten](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met iPass die smartconnect op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in iPass SmartConnect aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iPass SmartConnect moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met iPass SmartConnect, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker van iPass SmartConnect](#create-an-ipass-smartconnect-test-user)**  : als u wilt een equivalent van Britta Simon in iPass SmartConnect die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw iPass SmartConnect toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met iPass SmartConnect, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iPass SmartConnect** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

1. Op de **iPass SmartConnect domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus, hoeft u niet alle stappen moeten worden uitgevoerd.

    ![iPass SmartConnect domein en URL's, eenmalige aanmelding informatie](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

1. Controleer de geavanceerde URL-instellingen weergeven en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![iPass SmartConnect domein en URL's, eenmalige aanmelding informatie](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Typ een URL in het tekstvak aanmeldings-URL: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

1. de SAML-asserties ondertekend verwacht iPass SmartConnect toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/ipasssmartconnect-tutorial/attribute.png)

1. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk | Namespace-waarde|
    | ---------------| --------------- |----------------|
    | Voornaam | User.givenName |   |
    | lastName | User.surname | |
    | e-mailen | User.userPrincipalName | |
    | gebruikersnaam | User.userPrincipalName | |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Naamruimtewaarde laat het veld leeg voor die rij.

    e. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **iPass SmartConnect** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** en uw **domeinnaam** naar [iPass SmartConnect ondersteuning voor team](mailto:help@ipass.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-an-ipass-smartconnect-test-user"></a>Maak een testgebruiker van iPass SmartConnect

In deze sectie maakt u een gebruiker met de naam van Britta Simon in iPass SmartConnect. Werken met [iPass SmartConnect ondersteuningsteam](mailto:help@ipass.com) om toe te voegen de gebruikers of het domein dat nodig is om te worden opgenomen in de whitelist in het iPass SmartConnect-platform. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het iPass SmartConnect-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iPass SmartConnect.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan iPass SmartConnect toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **iPass SmartConnect**.

    ![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

**Als u wilt de toepassing testen in de stroom SP geïnitieerd, moet u de volgende stappen uitvoeren:**

a. Download windows iPass SmartConnect client [hier](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing3.png)

b. Installeer de client en starten.

c. Klik op **aan de slag**.

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Voer de naam van de Azure-gebruiker met domein. Klik op **blijven**. Dit wordt omgeleid naar de aanmeldingspagina van Azure

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Na een geslaagde authenticatie wordt activering van de client gestart. Client wordt geactiveerd.

**Als u wilt de toepassing in de IdP gestart door stroom testen, moet u de volgende stappen uitvoeren:**

a. Meld u aan bij [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Klik op iPass SmartConnect app.

c. Start het SSA-pagina, klikt u op **-App voor Windows downloaden** iPass SmartConnect client installeren.

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing4.png)

d. Nadat de installatie van de client op de eerste keer opstarten automatisch wordt gestart activeren na het accepteren van voorwaarden en bepalingen.

e. Als activering niet wordt gestart, klikt u op de knop activeren op SSA-pagina om te activeren.

f. Client wordt geactiveerd.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/ipasssmartconnect-tutorial/tutorial_general_01.png
[2]: ./media/ipasssmartconnect-tutorial/tutorial_general_02.png
[3]: ./media/ipasssmartconnect-tutorial/tutorial_general_03.png
[4]: ./media/ipasssmartconnect-tutorial/tutorial_general_04.png

[100]: ./media/ipasssmartconnect-tutorial/tutorial_general_100.png

[200]: ./media/ipasssmartconnect-tutorial/tutorial_general_200.png
[201]: ./media/ipasssmartconnect-tutorial/tutorial_general_201.png
[202]: ./media/ipasssmartconnect-tutorial/tutorial_general_202.png
[203]: ./media/ipasssmartconnect-tutorial/tutorial_general_203.png

