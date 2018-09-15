---
title: 'Zelfstudie: Azure Active Directory-integratie met dmarcian | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en dmarcian.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2018
ms.author: jeedes
ms.openlocfilehash: 677c40932a557b8a15a51b947794b4281801f65a
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637651"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Zelfstudie: Azure Active Directory-integratie met dmarcian

In deze zelfstudie leert u hoe u dmarcian integreren met Azure Active Directory (Azure AD).

Dmarcian integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot dmarcian heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij dmarcian (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met dmarcian, moet u de volgende items:

- Een Azure AD-abonnement
- Een dmarcian eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dmarcian uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dmarcian-from-the-gallery"></a>Dmarcian uit de galerie toe te voegen
Voor het configureren van de integratie van dmarcian in Azure AD, moet u dmarcian uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen dmarcian uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De zakelijke toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **dmarcian**, selecteer **dmarcian** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![dmarcian in de lijst met resultaten](./media/dmarcian-tutorial/tutorial_dmarcian_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met dmarcian op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in dmarcian is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in dmarcian tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met dmarcian, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker dmarcian](#create-a-dmarcian-test-user)**  : als u wilt een equivalent van Britta Simon in dmarcian die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing dmarcian.

**Voor het configureren van Azure AD eenmalige aanmelding met dmarcian, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **dmarcian** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/dmarcian-tutorial/tutorial_dmarcian_samlbase.png)

3. Op de **dmarcian domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![dmarcian domein en URL's één aanmeldings-informatie](./media/dmarcian-tutorial/tutorial_dmarcian_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian.com-eu/sso/saml/<ACCOUNT_ID>/sp.xml ` |
    | `https://dmarcian.com-ap/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian.com-eu/login/<ACCOUNT_ID>/handle/ `|
    | `https://dmarcian.com-ap/login/<ACCOUNT_ID>/handle/`|

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![dmarcian domein en URL's één aanmeldings-informatie](./media/dmarcian-tutorial/tutorial_dmarcian_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian.com-eu/login/<ACCOUNT_ID>` |
    | `https://dmarcian.com-ap/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. U kunt deze waarden wordt bijgewerkt met de werkelijke-id, de antwoord-URL en aanmeldings-URL die later in de zelfstudie wordt uitgelegd. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/dmarcian-tutorial/tutorial_dmarcian_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/dmarcian-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster aanmelden bij dmarcian als een beveiligingsbeheerder.

8. Klik op **profiel** in de rechterbovenhoek hoek en navigeer naar **voorkeuren**.

    ![De voorkeuren ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

9. Schuif naar beneden en klik op **Single Sign-On** sectie en klik vervolgens op **configureren**.

    ![Eén ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

10. Op de **SAML Single Sign-On** set pagina de **Status** als **ingeschakeld** en voer de volgende stappen uit:

    ![De verificatie ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * Onder **dmarcian toevoegen aan uw id-Provider** sectie, klikt u op **kopie** kopiëren de **URL van de Bevestigingsconsumerservice** voor uw exemplaar en plak deze in  **Antwoord-URL** -tekstvak in **dmarcian domein en URL's sectie** in Azure portal.

    * Onder **dmarcian toevoegen aan uw id-Provider** sectie, klikt u op **kopie** kopiëren de **entiteit-ID** voor uw exemplaar en plak deze in **id**-tekstvak in **dmarcian domein en URL's sectie** in Azure portal.

    * Onder **verificatie instellen** sectie in de **Identity Provider metagegevens** tekstvak plakken de **App-Url voor federatieve metagegevens**, die u hebt gekopieerd vanuit Azure portal.

    * Onder **verificatie instellen** sectie in de **kenmerk instructies** tekstvak plak de url `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    * Onder **aanmeldings-URL instellen** sectie, Kopieer de **aanmeldings-URL** voor uw exemplaar en plak deze in **aanmeldings-URL** -tekstvak in **dmarcian domein en URL's sectie** in Azure portal.

        > [!Note]
        > U kunt de **aanmeldings-URL** op basis van uw organisatie.

    * Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/dmarcian-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/dmarcian-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/dmarcian-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/dmarcian-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-dmarcian-test-user"></a>Maak een testgebruiker dmarcian

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij dmarcian, moeten ze worden ingericht voor dmarcian. In dmarcian is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u bij dmarcian als een beveiligingsbeheerder.

2. Klik op **profiel** in de rechterbovenhoek hoek en navigeer naar **gebruikers beheren**.

    ![De gebruiker ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. Aan de rechterkant van **SSO gebruikers** sectie, klikt u op **Add New User**.

    ![De gebruiker toevoegen ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Op de **Add New User** pop-upvenster de volgende stappen uitvoeren:

    ![De nieuwe gebruiker ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. In de **nieuwe e-mailadres van gebruiker** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    b. Als u verlenen van beheerdersrechten voor de gebruiker wilt, selecteert u **maken van de gebruiker een beheerder**.

    c. Klik op **gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot dmarcian.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan dmarcian toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **dmarcian**.

    ![De koppeling dmarcian in de lijst met toepassingen](./media/dmarcian-tutorial/tutorial_dmarcian_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel dmarcian in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing dmarcian.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/dmarcian-tutorial/tutorial_general_01.png
[2]: ./media/dmarcian-tutorial/tutorial_general_02.png
[3]: ./media/dmarcian-tutorial/tutorial_general_03.png
[4]: ./media/dmarcian-tutorial/tutorial_general_04.png

[100]: ./media/dmarcian-tutorial/tutorial_general_100.png

[200]: ./media/dmarcian-tutorial/tutorial_general_200.png
[201]: ./media/dmarcian-tutorial/tutorial_general_201.png
[202]: ./media/dmarcian-tutorial/tutorial_general_202.png
[203]: ./media/dmarcian-tutorial/tutorial_general_203.png

