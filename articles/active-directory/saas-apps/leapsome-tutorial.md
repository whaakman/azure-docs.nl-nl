---
title: 'Zelfstudie: Azure Active Directory-integratie met Leapsome | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Leapsome.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: b23a93db7912aa25b420157241c41533f4f48a27
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938391"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Zelfstudie: Azure Active Directory-integratie met Leapsome

In deze zelfstudie leert u hoe Leapsome integreren met Azure Active Directory (Azure AD).

Leapsome integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Leapsome heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Leapsome (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Leapsome, moet u de volgende items:

- Een Azure AD-abonnement
- Een Leapsome eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Leapsome uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-leapsome-from-the-gallery"></a>Leapsome uit de galerie toevoegen
Voor het configureren van de integratie van Leapsome in Azure AD, moet u Leapsome uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Leapsome uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Leapsome**, selecteer **Leapsome** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Leapsome in de lijst met resultaten](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Leapsome op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Leapsome is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Leapsome tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Leapsome, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Leapsome](#create-a-leapsome-test-user)**  - Leapsome die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Leapsome configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Leapsome, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Leapsome** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

3. Op de **Leapsome domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Leapsome domein eenmalige aanmelding informatie](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. In de **id** textbox, typ een URL: `https://www.leapsome.com`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Leapsome domein eenmalige aanmelding informatie](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > De voorgaande antwoord-URL en eenmalige aanmelding URL-waarde is geen echte waarde. U werkt met de werkelijke waarden, die verderop in de zelfstudie wordt uitgelegd.

5. De SAML-asserties verwacht Leapsome toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld.
    
    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk | Naamruimte |
    | ---------------| --------------- | --------- |   
    | Voornaam | User.givenName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Achternaam | User.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | titel | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | afbeelding | URL van de werknemer afbeelding | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > De waarde van kenmerk van de afbeelding is niet echt. Deze waarde bijwerken met de werkelijke afbeeldings-URL. Deze waarde contactpersoon ophalen [Leapsome Client ondersteuningsteam](mailto:support@leapsome.com).
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. In de **Namespace** textbox, typt u de naamruimte-uri voor die rij.
    
    e. Klik op **Ok**

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/leapsome-tutorial/tutorial_general_400.png)
    
9. Op de **Leapsome configuratie** sectie, klikt u op **configureren Leapsome** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Leapsome configuratie](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

10. In een ander browservenster, meld u aan bij Leapsome als een beveiligingsbeheerder.

11. Klik op instellingen logo in de rechterbovenhoek, en klik vervolgens op **beheerdersinstellingen**. 

    ![Leapsome instellen](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

12. Klik op de linkermenubalk **eenmalige aanmelding op (SSO)**, en klik op de **op basis van SAML eenmalige aanmelding (SSO)** pagina de volgende stappen uitvoeren:
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Selecteer **op basis van SAML inschakelen eenmalige aanmelding**.

    b. Kopiëren de **aanmeldings-URL (uw gebruikers hier punt voor het starten van de aanmelding)** waarde en plak deze in de **aanmeldings-URL** textbox in **Leapsome domein en de URL's** sectie op Azure-portal.

    c. Kopieer de **antwoord-URL (reactie ontvangen van uw id-provider)** waarde en plak deze in de **antwoord-URL** textbox in **Leapsome domein en de URL's** sectie op Azure-portal.

    d. In de **SSO aanmeldings-URL (geleverd door de identiteitsprovider)** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd uit de Azure-portal.

    e. Kopieer het certificaat dat u hebt gedownload vanuit Azure-portal zonder--certificaat BEGIN en einde certificaat--opmerkingen en plak deze in de **certificaat (geleverd door de identiteitsprovider)** textbox.

    f. Klik op **SSO-UPDATE-instellingen**.
    
### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/leapsome-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/leapsome-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/leapsome-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/leapsome-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-leapsome-test-user"></a>Een testgebruiker Leapsome maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Leapsome maken. Werken met [Leapsome Client ondersteuningsteam](mailto:support@leapsome.com) de gebruikers of het domein dat moet worden goedgekeurde lijst van het platform Leapsome toe te voegen. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het platform Leapsome. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Leapsome.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Leapsome, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Leapsome**.

    ![De koppeling Leapsome in de lijst met toepassingen](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Leapsome in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Leapsome.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

