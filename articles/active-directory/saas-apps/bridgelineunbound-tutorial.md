---
title: 'Zelfstudie: Azure Active Directory-integratie met Bridgeline is losgekoppeld | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Bridgeline is losgekoppeld.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b018472f-c8b3-403d-ae66-9ed26a35f413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: jeedes
ms.openlocfilehash: c429afa12bc11db68d041fef96f66b3f4c7f0b1b
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206107"
---
# <a name="tutorial-azure-active-directory-integration-with-bridgeline-unbound"></a>Zelfstudie: Azure Active Directory-integratie met Bridgeline is losgekoppeld

In deze zelfstudie leert u hoe u Bridgeline is losgekoppeld integreren met Azure Active Directory (Azure AD).

Voor niet-afhankelijke Bridgeline integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Bridgeline is losgekoppeld heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Bridgeline is losgekoppeld (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Bridgeline is losgekoppeld, moet u de volgende items:

- Een Azure AD-abonnement
- Een afhankelijke Bridgeline eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Bridgeline is losgekoppeld van de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bridgeline-unbound-from-the-gallery"></a>Toe te voegen Bridgeline is losgekoppeld van de galerie
Voor het configureren van de integratie van Bridgeline is losgekoppeld in Azure AD, moet u voor niet-afhankelijke Bridgeline toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Bridgeline is losgekoppeld van de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Bridgeline is losgekoppeld**, selecteer **Bridgeline is losgekoppeld** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Afhankelijke Bridgeline in de lijst met resultaten](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Bridgeline kader op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Bridgeline is losgekoppeld is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Bridgeline is losgekoppeld tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Bridgeline is losgekoppeld, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Bridgeline is losgekoppeld](#create-a-bridgeline-unbound-test-user)**  : als u wilt een equivalent van Britta Simon in Bridgeline afhankelijke die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Bridgeline niet-afhankelijke toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Bridgeline is losgekoppeld, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Bridgeline is losgekoppeld** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_samlbase.png)
 
3. Op de **Bridgeline is losgekoppeld domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Bridgeline domein en URL's, eenmalige aanmelding informatie](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `iApps_UPSTT_<ENVIRONMENTNAME>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.iapps.com/SAMLAssertionService.aspx`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Bridgeline domein en URL's, eenmalige aanmelding informatie](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.iapps.com/CommonLogin/login?<INSTANCENAME>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Bridgeline niet-afhankelijke clients ondersteuningsteam](mailto:support@iapps.com) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/bridgelineunbound-tutorial/tutorial_general_400.png)

6. Op de **Bridgeline niet-afhankelijke configuratie** sectie, klikt u op **Bridgeline is losgekoppeld configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Niet-afhankelijke Bridgeline-configuratie](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_configure.png) 

7. Het configureren van eenmalige aanmelding op **Bridgeline is losgekoppeld** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)**, **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Bridgeline is losgekoppeld ondersteuningsteam](mailto:support@iapps.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/bridgelineunbound-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en grobridgelineinbound**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en grobridgelineinbound' en 'Alle gebruikers' koppelingen](./media/bridgelineunbound-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/bridgelineunbound-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/bridgelineunbound-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-bridgeline-unbound-test-user"></a>Maak een testgebruiker Bridgeline is losgekoppeld

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Bridgeline is losgekoppeld. Voor niet-afhankelijke Bridgeline biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot Bridgeline is losgekoppeld, als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Bridgeline is losgekoppeld ondersteuningsteam](mailto:support@iapps.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Bridgeline is losgekoppeld.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen Bridgeline is losgekoppeld, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Bridgeline is losgekoppeld**.

    ![De afhankelijke Bridgeline koppeling in de lijst met toepassingen](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en grobridgelineinbound**.

    ![De koppeling 'Gebruikers en grobridgelineinbound'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en grobridgelineinbound** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en grobridgelineinbound** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en grobridgelineinbound** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel voor niet-afhankelijke Bridgeline in het toegangsvenster, u moet u automatisch aangemeld bij uw Bridgeline niet-afhankelijke toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bridgelineunbound-tutorial/tutorial_general_01.png
[2]: ./media/bridgelineunbound-tutorial/tutorial_general_02.png
[3]: ./media/bridgelineunbound-tutorial/tutorial_general_03.png
[4]: ./media/bridgelineunbound-tutorial/tutorial_general_04.png

[100]: ./media/bridgelineunbound-tutorial/tutorial_general_100.png

[200]: ./media/bridgelineunbound-tutorial/tutorial_general_200.png
[201]: ./media/bridgelineunbound-tutorial/tutorial_general_201.png
[202]: ./media/bridgelineunbound-tutorial/tutorial_general_202.png
[203]: ./media/bridgelineunbound-tutorial/tutorial_general_203.png

