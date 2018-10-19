---
title: 'Zelfstudie: Azure Active Directory-integratie met GetThere | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en GetThere.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0441087e-953f-4b51-9842-316da7b72392
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: jeedes
ms.openlocfilehash: bcefa3966a6c854f02ce7b3a75306b3d1c888ecd
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430866"
---
# <a name="tutorial-azure-active-directory-integration-with-getthere"></a>Zelfstudie: Azure Active Directory-integratie met GetThere

In deze zelfstudie leert u hoe u GetThere integreren met Azure Active Directory (Azure AD).

GetThere integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot GetThere heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij GetThere (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met GetThere, moet u de volgende items:

- Een Azure AD-abonnement
- Een GetThere eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. GetThere uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-getthere-from-the-gallery"></a>GetThere uit de galerie toe te voegen
Voor het configureren van de integratie van GetThere in Azure AD, moet u GetThere uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen GetThere uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/getthere-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/getthere-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/getthere-tutorial/a_new_app.png)

4. Typ in het zoekvak **GetThere**, selecteer **GetThere** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/getthere-tutorial/tutorial_getthere_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met GetThere op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in GetThere is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in GetThere tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met GetThere, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker GetThere](#create-a-getthere-test-user)**  : als u wilt een equivalent van Britta Simon in GetThere die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing GetThere.

**Voor het configureren van Azure AD eenmalige aanmelding met GetThere, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **GetThere** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/getthere-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/getthere-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/getthere-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![image](./media/getthere-tutorial/tutorial_getthere_url.png)

    a. In de **id** tekst typt u een van de onderstaande URL's:
    | |
    |--|
    | `getthere.com` |
    | `http://idp.getthere.com` |

    b. In de **antwoord-URL** tekst typt u een van de onderstaande URL's:
    | |
    |--|
    | `https://wx1.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/ssoaasvalidate.act` |
    | `https://wx1.getthere.net/login/saml/ssoaavalidate.act` |
    
5. GetThere toepassing wordt verwacht dat de unieke **gebruikersnaam** waarde in de claim gebruikersnaam. Klanten kan de juiste waarde voor de gebruikersnaam-claim worden toegewezen. In onderstaand voorbeeld we zijn toegewezen de **gebruikersnaam** naar **user.mail**, maar u kunt de toewijzing wijzigen op basis van de instellingen van uw organisatie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![image](./media/getthere-tutorial/i4-attribute.png)

6. In de **gebruikersclaims** sectie op de **gebruikerskenmerken** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam |  Kenmerk van de gegevensbron |  Naamruimte |
    | ---------------| --------------- | --------------- |
    | Sitenaam | "Geef de waarde aan de hand van uw organisatie" | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sitename |
    | Gebruikersnaam |  User.mail | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/username |
    
    a. Klik op **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/getthere-tutorial/i2-attribute.png)

    ![image](./media/getthere-tutorial/i3-attribute.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. In de **Namespace** tekstvak typt u de naamruimte van het kenmerk wordt weergegeven voor die rij.

    d. Selecteer de bron als **kenmerk**.

    e. Uit de **bronkenmerk** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    f. Klik op **Opslaan**.

7. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Base64)** en sla deze op uw computer.

    ![image](./media/getthere-tutorial/tutorial_getthere_certficate.png) 

8. Op de **GetThere instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure Ad-id

    c. URL voor afmelden

    ![image](./media/getthere-tutorial/d1_samlsonfigure.png) 

9. Het configureren van eenmalige aanmelding op **GetThere** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)** en gekopieerde **afmeldings-URL van aanmeldings-URL, Azure Ad-id,** naar [ Het ondersteuningsteam GetThere Client](mailto:dataintegration@sabre.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/getthere-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/getthere-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/getthere-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-getthere-test-user"></a>Maak een testgebruiker GetThere

In deze sectie maakt u een gebruiker met de naam van Britta Simon in GetThere. Werken met [GetThere Client ondersteuningsteam](mailto:dataintegration@sabre.com) om toe te voegen de gebruikers in het GetThere-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan GetThere.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/getthere-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **GetThere**.

    ![image](./media/getthere-tutorial/tutorial_getthere_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/getthere-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/getthere-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel GetThere in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing GetThere.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
