---
title: 'Zelfstudie: Azure Active Directory-integratie met Prestatiemeter AppNeta | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AppNeta Performance Monitor.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: 19d79f65746b5ee03209bfd7d8405ddaa24bb825
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194879"
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Zelfstudie: Azure Active Directory-integratie met AppNeta Performance Monitor

In deze zelfstudie leert u hoe u de Prestatiemeter AppNeta integreren met Azure Active Directory (Azure AD).

Prestatiemeter AppNeta integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AppNeta Performance Monitor heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Prestatiemeter AppNeta (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AppNeta Performance Monitor, moet u de volgende items:

- Een Azure AD-abonnement
- Een AppNeta Prestatiemeter eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Prestatiemeter AppNeta uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Prestatiemeter AppNeta uit de galerie toe te voegen
Voor het configureren van de integratie van Prestatiemeter AppNeta in Azure AD, moet u AppNeta Performance Monitor uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de Prestatiemeter AppNeta uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **AppNeta Prestatiemeter**, selecteer **AppNeta Prestatiemeter** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![De Prestatiemeter AppNeta in de lijst met resultaten](./media/appneta-tutorial/tutorial_appneta_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met AppNeta Performance Monitor op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Prestatiemeter AppNeta is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Prestatiemeter AppNeta tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met AppNeta Performance Monitor, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker AppNeta Prestatiemeter](#create-an-appneta-performance-monitor-test-user)**  : als u wilt een equivalent van Britta Simon in AppNeta Prestatiemeter die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing AppNeta Performance Monitor.

**Voor het configureren van Azure AD eenmalige aanmelding met AppNeta Performance Monitor, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AppNeta Prestatiemeter** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/appneta-tutorial/tutorial_appneta_samlbase.png)

3. Op de **AppNeta Performance Monitor domein en URL's** sectie, voert u de volgende stappen uit:

    ![AppNeta Performance Monitor domein en URL's, eenmalige aanmelding informatie](./media/appneta-tutorial/tutorial_appneta_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.pm.appneta.com`

    b. In de **id** tekstvak typt u de waarde: `PingConnect`

    > [!NOTE] 
    > De waarde voor de aanmeldings-URL is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [AppNeta Performance Monitor Client ondersteuningsteam](mailto:support@appneta.com) deze waarde op te halen. 

5. De toepassing AppNeta Prestatiemeter wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/appneta-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:
           
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| ----------------|
    | firstName| user.givenname|
    | lastName| user.surname|
    | e-mail| user.userprincipalname|
    | naam| user.userprincipalname|
    | groepen   | user.assignedroles |
    | telefoon| user.telephonenumber |
    | titel| user.jobtitle|

    > [!NOTE]
    > 'groepen' verwijst naar de beveiligingsgroep in Appneta die is toegewezen aan een 'rol' in Azure AD. Raadpleeg [dit](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) doc-bestand waarin wordt uitgelegd hoe u aangepaste rollen maken in Azure AD.
        
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/appneta-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/appneta-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Naamruimte leeg laten.
    
    e. Klik op **OK**.  

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/appneta-tutorial/tutorial_appneta_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/appneta-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **AppNeta Prestatiemeter** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [AppNeta Prestatiemeter-ondersteuningsteam](mailto:support@appneta.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/appneta-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/appneta-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/appneta-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/appneta-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-appneta-performance-monitor-test-user"></a>Maak een testgebruiker AppNeta Prestatiemeter

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Prestatiemeter AppNeta. Prestatiemeter AppNeta biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot de Prestatiemeter AppNeta als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [AppNeta Prestatiemeter-ondersteuningsteam](mailto:support@appneta.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon toegang verlenen tot AppNeta Prestatiemeter gebruiken Azure eenmalige aanmelding.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon AppNeta prestaties bewaken, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **AppNeta Prestatiemeter**.

    ![De Prestatiemeter AppNeta-koppeling in de lijst met toepassingen](./media/appneta-tutorial/tutorial_appneta_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel AppNeta Performance Monitor in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing AppNeta Performance Monitor.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

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

