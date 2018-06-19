---
title: 'Zelfstudie: Azure Active Directory-integratie met Prestatiemeter AppNeta | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AppNeta Prestatiemeter.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: cc63da64bee4ccd543d2f8f6a108b539967f1d38
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35916335"
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Zelfstudie: Azure Active Directory-integratie met AppNeta Prestatiemeter

In deze zelfstudie leert u hoe de Prestatiemeter AppNeta integreren met Azure Active Directory (Azure AD).

Prestatiemeter AppNeta integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AppNeta Prestatiemeter heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Prestatiemeter AppNeta (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Prestatiemeter AppNeta, moet u de volgende items:

- Een Azure AD-abonnement
- Een AppNeta Prestatiemeter eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van de Prestatiemeter AppNeta uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Toevoegen van de Prestatiemeter AppNeta uit de galerie
Voor het configureren van de integratie van Prestatiemeter AppNeta in Azure AD, moet u AppNeta Prestatiemeter uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AppNeta Prestatiemeter uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **AppNeta Prestatiemeter**, selecteer **AppNeta Prestatiemeter** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![De Prestatiemeter AppNeta in de lijst met resultaten](./media/appneta-tutorial/tutorial_appneta_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Prestatiemeter AppNeta op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Prestatiemeter AppNeta is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Prestatiemeter AppNeta tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Prestatiemeter AppNeta, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Prestatiemeter AppNeta](#create-an-appneta-performance-monitor-test-user)**  - AppNeta Prestatiemeter die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing AppNeta Prestatiemeter.

**Voor het configureren van Azure AD eenmalige aanmelding met Prestatiemeter AppNeta, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AppNeta Prestatiemeter** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/appneta-tutorial/tutorial_appneta_samlbase.png)

3. Op de **AppNeta Performance Monitor domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en AppNeta Performance Monitor domein eenmalige aanmelding informatie](./media/appneta-tutorial/tutorial_appneta_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.pm.appneta.com`

    b. In de **id** textbox, typ de waarde: `PingConnect`

    > [!NOTE] 
    > De waarde van de aanmeldings-URL is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [AppNeta Performance Monitor Client ondersteuningsteam](mailto:support@appneta.com) deze waarde op te halen. 

5. De toepassing AppNeta Prestatiemeter verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/appneta-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en de volgende stappen uitvoeren:
           
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| ----------------|
    | Voornaam| User.givenName|
    | lastName| User.surname|
    | e-mailen| User.userPrincipalName|
    | naam| User.userPrincipalName|
    | groepen   | User.assignedroles |
    | telefoon| User.telephoneNumber |
    | titel| user.jobtitle|

    > [!NOTE]
    > "groups" verwijst naar de beveiligingsgroep in Appneta die is toegewezen aan een 'rol' in Azure AD. Raadpleeg [dit](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) doc waarin wordt uitgelegd hoe u aangepaste rollen in Azure AD maken.
        
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/appneta-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/appneta-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat leeg naamruimte.
    
    e. Klik op **OK**.  

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/appneta-tutorial/tutorial_appneta_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/appneta-tutorial/tutorial_general_400.png)

6. Eenmalige aanmelding configureren op **AppNeta Prestatiemeter** zijde, moet u de gedownloade verzenden **Metadata XML** naar [AppNeta Prestatiemeter ondersteuningsteam](mailto:support@appneta.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/appneta-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/appneta-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/appneta-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/appneta-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-appneta-performance-monitor-test-user"></a>Maken van een testgebruiker AppNeta Prestatiemeter

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Prestatiemeter AppNeta aangeroepen. Prestatiemeter AppNeta ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot de Prestatiemeter AppNeta als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [AppNeta Prestatiemeter ondersteuningsteam](mailto:support@appneta.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent aan AppNeta Prestatiemeter gebruiken.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon AppNeta prestaties bewaken, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **AppNeta Prestatiemeter**.

    ![De Prestatiemeter AppNeta-koppeling in de lijst met toepassingen](./media/appneta-tutorial/tutorial_appneta_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Prestatiemeter AppNeta in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing AppNeta Prestatiemeter.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/appneta-tutorial/tutorial_general_01.png
[2]: ./media/appneta-tutorial/tutorial_general_02.png
[3]: ./media/appneta-tutorial/tutorial_general_03.png
[4]: ./media/appneta-tutorial/tutorial_general_04.png

[100]: ./media/appneta-tutorial/tutorial_general_100.png

[200]: ./media/appneta-tutorial/tutorial_general_200.png
[201]: ./media/appneta-tutorial/tutorial_general_201.png
[202]: ./media/appneta-tutorial/tutorial_general_202.png
[203]: ./media/appneta-tutorial/tutorial_general_203.png

