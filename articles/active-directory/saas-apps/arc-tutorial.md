---
title: 'Zelfstudie: Azure Active Directory-integratie met publiceren boog - SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en publiceren boog - eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: c1bf5ba68d8242a0ef0831987ac6fd041c62ace9
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969364"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Zelfstudie: Azure Active Directory-integratie met publiceren boog - eenmalige aanmelding

In deze zelfstudie leert u hoe u integreert boog Publishing - eenmalige aanmelding met Azure Active Directory (Azure AD).

Integratie van boog publicatie - eenmalige aanmelding met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot publiceren boog - eenmalige aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij publicatie boog - SSO (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met publiceren boog - SSO, moet u de volgende items:

- Een Azure AD-abonnement
- De publicatie van een boog - SSO-eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Boog publicatie - eenmalige aanmelding in de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Boog publicatie - eenmalige aanmelding in de galerie toevoegen
Als u wilt configureren van de integratie van publiceren boog - eenmalige aanmelding bij Azure AD die u wilt toevoegen boog Publishing - eenmalige aanmelding in de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt publiceren boog - eenmalige aanmelding in de galerie toevoegen de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. In het zoekvak, typ **Publishing boog - SSO**, selecteer **Publishing boog - SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Boog publicatie - eenmalige aanmelding in de lijst met resultaten](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen op Azure AD eenmalige aanmelding met boog publiceren - eenmalige aanmelding op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding voor het werk, Azure AD moet weten wat de equivalente-gebruiker in boog Publishing - eenmalige aanmelding is voor een gebruiker in Azure AD. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in boog Publishing - eenmalige aanmelding moet tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met boog Publishing - SSO, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maken van een boog publiceren - eenmalige aanmelding testen gebruiker](#create-an-arc-publishing---sso-test-user)**  : als u wilt een equivalent van Britta Simon in boog Publishing - eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw boog Publishing - SSO-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met publiceren boog - SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Publishing boog - SSO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/arc-tutorial/tutorial_arc_samlbase.png)

1. Op de **Publishing boog - SSO-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Boog publicatie - SSO-domein en één URL's aanmeldings-informatie](./media/arc-tutorial/tutorial_arc_url.png)

    1. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    1. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Boog publicatie - SSO-domein en één URL's aanmeldings-informatie](./media/arc-tutorial/tutorial_arc_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Publishing boog - ondersteuningsteam voor SSO-Client](mailto:inf@washpost.com) om deze waarden te verkrijgen. 

1. ARC publiceren - SSO-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/arc-tutorial/tutorial_arc_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | Voornaam | User.givenName |
    | lastName | User.surname |
    | e-mailen | User.mail |
    | groepen | User.assignedroles |

    1. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

     ![Eenmalige aanmelding configureren](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Eenmalige aanmelding configureren](./media/arc-tutorial/tutorial_attribute_05.png)
    
    1. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    1. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    1. Laat de **Namespace** leeg.
    
    1. Klik op **Ok**

    > [!NOTE]
    > Hier de **groepen** kenmerk wordt toegewezen met **user.assignedroles**. Dit zijn aangepaste rollen in Azure AD om toe te wijzen de groepsnamen terug in de toepassing hebt gemaakt. U vindt meer richtlijnen [hier](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) voor het maken van aangepaste rollen in Azure AD. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/arc-tutorial/tutorial_arc_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/arc-tutorial/tutorial_general_400.png)
    
1. Op de **Publishing boog - configuratie voor eenmalige aanmelding** sectie, klikt u op **configureren boog Publishing - SSO** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Boog publicatie - SSO-configuratie](./media/arc-tutorial/tutorial_arc_configure.png) 

1. Het configureren van eenmalige aanmelding op **Publishing boog - SSO** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64), URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [boog Publicatie - ondersteuningsteam voor eenmalige aanmelding](mailto:inf@washpost.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/arc-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/arc-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/arc-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/arc-tutorial/create_aaduser_04.png)

    1. In de **naam** in het vak **BrittaSimon**.

    1. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    1. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    1. Klik op **Create**.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Maak een boog Publishing - gebruiker voor eenmalige aanmelding testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in boog Publishing - eenmalige aanmelding. ARC publiceren - eenmalige aanmelding ondersteunt just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot publiceren boog - eenmalige aanmelding als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Publishing boog - ondersteuningsteam voor eenmalige aanmelding](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor publicatie boog - eenmalige aanmelding.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar boog Publishing - SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Publishing boog - SSO**.

    ![Het publiceren van de boog - koppeling voor eenmalige aanmelding in de lijst met toepassingen](./media/arc-tutorial/tutorial_arc_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de boog Publishing - tegel in het toegangsvenster en SSO-u moet u automatisch aangemeld bij uw boog Publishing - SSO-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

