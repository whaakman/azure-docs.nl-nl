---
title: 'Zelfstudie: Azure Active Directory-integratie met AwardSpring | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AwardSpring.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f115be6-4fbe-42aa-9319-7462e7a75736
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeedes
ms.openlocfilehash: e9c67a9ac733a1dd7e4810f3f8a3185ff7142041
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212601"
---
# <a name="tutorial-azure-active-directory-integration-with-awardspring"></a>Zelfstudie: Azure Active Directory-integratie met AwardSpring

In deze zelfstudie leert u hoe AwardSpring integreren met Azure Active Directory (Azure AD).

AwardSpring integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AwardSpring heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij AwardSpring (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AwardSpring, moet u de volgende items:

- Een Azure AD-abonnement
- Een AwardSpring eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AwardSpring uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-awardspring-from-the-gallery"></a>AwardSpring uit de galerie toevoegen
Voor het configureren van de integratie van AwardSpring in Azure AD, moet u AwardSpring uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AwardSpring uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **AwardSpring**, selecteer **AwardSpring** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![AwardSpring in de lijst met resultaten](./media/awardspring-tutorial/tutorial_awardspring_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met AwardSpring op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in AwardSpring is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in AwardSpring tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met AwardSpring, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker AwardSpring](#create-an-awardspring-test-user)**  - AwardSpring die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing AwardSpring configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met AwardSpring, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AwardSpring** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/awardspring-tutorial/tutorial_awardspring_samlbase.png)

3. Op de **AwardSpring domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en AwardSpring domein eenmalige aanmelding informatie](./media/awardspring-tutorial/tutorial_awardspring_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.awardspring.com/SignIn/SamlMetaData`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.awardspring.com/SignIn/SamlAcs`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en AwardSpring domein eenmalige aanmelding informatie](./media/awardspring-tutorial/tutorial_awardspring_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.awardspring.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [AwardSpring Client ondersteuningsteam](mailto:support@awardspring.com) ophalen van deze waarden. 

5. De SAML-asserties verwacht AwardSpring toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/awardspring-tutorial/tutorial_awardSpring_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | Voornaam | User.givenName |
    | Achternaam | User.surname |
    | Email | User.mail |
    | Gebruikersnaam | User.userprinicipalname |
    | studentID | < Student-ID > |

    > [!NOTE]
    > Het kenmerk StudentID is toegewezen met de werkelijke studenten-ID die moet worden doorgegeven in claims. Neem contact op met [AwardSpring Client ondersteuningsteam](mailto:support@awardspring.com) deze waarde op te halen.

    a. Verwijder bestaande kenmerken en nieuwe kenmerken toevoegen. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/awardspring-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/awardspring-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    d. Klik op **Ok**

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/awardspring-tutorial/tutorial_awardspring_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/awardspring-tutorial/tutorial_general_400.png)
    
9. Eenmalige aanmelding configureren op **AwardSpring** zijde, moet u de gedownloade verzenden **Metadata XML** naar [AwardSpring ondersteuningsteam](mailto:support@awardspring.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/awardspring-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/awardspring-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/awardspring-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/awardspring-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-awardspring-test-user"></a>Een testgebruiker AwardSpring maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in AwardSpring genoemd. AwardSpring ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot AwardSpring als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [AwardSpring ondersteuningsteam](maito:support@awardspring.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan AwardSpring.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen AwardSpring, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **AwardSpring**.

    ![De koppeling AwardSpring in de lijst met toepassingen](./media/awardspring-tutorial/tutorial_awardspring_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel AwardSpring in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing AwardSpring.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/awardspring-tutorial/tutorial_general_01.png
[2]: ./media/awardspring-tutorial/tutorial_general_02.png
[3]: ./media/awardspring-tutorial/tutorial_general_03.png
[4]: ./media/awardspring-tutorial/tutorial_general_04.png

[100]: ./media/awardspring-tutorial/tutorial_general_100.png

[200]: ./media/awardspring-tutorial/tutorial_general_200.png
[201]: ./media/awardspring-tutorial/tutorial_general_201.png
[202]: ./media/awardspring-tutorial/tutorial_general_202.png
[203]: ./media/awardspring-tutorial/tutorial_general_203.png

