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
ms.openlocfilehash: 5ae600142f7e90a7f6185c3a948a4174ce4c7797
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320525"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Zelfstudie: Azure Active Directory-integratie met iPass SmartConnect

In deze zelfstudie leert u hoe iPass SmartConnect integreren met Azure Active Directory (Azure AD).

IPass SmartConnect integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iPass SmartConnect heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij iPass SmartConnect (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iPass SmartConnect, moet u de volgende items:

- Een Azure AD-abonnement
- Een iPass eenmalige aanmelding SmartConnect abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IPass SmartConnect uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>IPass SmartConnect uit de galerie toevoegen
Voor het configureren van de integratie van iPass SmartConnect in Azure AD, moet u iPass SmartConnect uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iPass SmartConnect uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **iPass SmartConnect**, selecteer **iPass SmartConnect** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![iPass SmartConnect in de lijst met resultaten](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met iPass die smartconnect op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in iPass SmartConnect voor een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in iPass SmartConnect moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met iPass SmartConnect, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maakt u een gebruiker iPass SmartConnect test](#create-an-ipass-smartconnect-test-user)**  - hebben een equivalent van Britta Simon in iPass SmartConnect die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw iPass SmartConnect toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met iPass SmartConnect, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iPass SmartConnect** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

3. Op de **iPass SmartConnect domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** geïnitieerd modus, hoeft u niet alle stappen uit te voeren.

    ![iPass SmartConnect domein en de URL's eenmalige aanmelding informatie](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

4. Controleer weergeven geavanceerde instellingen voor de URL en de volgende stap uitvoeren als u wilt configureren van de toepassing in **SP** modus gestart:

    ![iPass SmartConnect domein en de URL's eenmalige aanmelding informatie](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Typ een URL in het tekstvak aanmeldings-URL: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

5. de SAML-asserties verwacht iPass SmartConnect toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/ipasssmartconnect-tutorial/attribute.png)

6. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie uit te breiden, de kenmerken. Voer de volgende stappen uit op elk van de kenmerken weergegeven-

    | Naam kenmerk | Waarde kenmerk | Namespace-waarde|
    | ---------------| --------------- |----------------|
    | Voornaam | User.givenName |   |
    | lastName | User.surname | |
    | e-mail | User.userPrincipalName | |
    | gebruikersnaam | User.userPrincipalName | |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Houd naamruimtewaarde leeg laten voor die rij.

    e. Klik op **OK**.

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

9. Eenmalige aanmelding configureren op **iPass SmartConnect** zijde, moet u de gedownloade verzenden **Metadata XML** en uw **domeinnaam** naar [iPass SmartConnect ondersteuningsteam](mailto:help@ipass.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-an-ipass-smartconnect-test-user"></a>Maken van een gebruiker iPass SmartConnect testen

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in iPass SmartConnect maken. Werken met [iPass SmartConnect ondersteuningsteam](mailto:help@ipass.com) om toe te voegen de gebruikers of het domein die nodig is om de wilt plaatsen in de iPass SmartConnect-platform. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het iPass SmartConnect-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iPass SmartConnect.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen iPass SmartConnect, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **iPass SmartConnect**.

    ![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

**Test de toepassing in de stroom SP geïnitieerd door de volgende stappen uitvoeren:**

a. Download windows iPass SmartConnect client [hier](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing3.png)

b. Installeer de client en starten.

c. Klik op **aan de slag**.

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Voer de naam van de Azure-gebruiker met domein. Klik op **gaan**. Dit wordt omgeleid naar Azure aanmeldingspagina

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Na een geslaagde authenticatie wordt activering van de client gestart. Client zal ophalen geactiveerd.

**Test de toepassing in de stroom IdP geïnitieerd door de volgende stappen uitvoeren:**

a. Meld u aan bij [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Klik op iPass SmartConnect app.

c. Deze pagina SSA wordt gestart, klikt u op **voor Windows-App downloaden** iPass SmartConnect client installeren.

![De iPass SmartConnect koppeling in de lijst met toepassingen](./media/ipasssmartconnect-tutorial/testing4.png)

d. Na installatie, de client op de eerste keer opstarten automatisch wordt start u activering na voorwaarden en bepalingen te accepteren.

e. Als activering niet wordt gestart, klik op de knop activeren op de pagina SSA activeren.

f. Client zal ophalen geactiveerd.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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

