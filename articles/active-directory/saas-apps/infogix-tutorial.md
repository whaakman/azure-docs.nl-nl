---
title: 'Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty reguleren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Infogix Data3Sixty reguleren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: c1fd1e5c83b311c4af23401aa85f25a4264216ba
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907230"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty bepalen

In deze zelfstudie leert u hoe Infogix Data3Sixty reguleren integreren met Azure Active Directory (Azure AD).

Infogix Data3Sixty reguleren integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Infogix Data3Sixty reguleren heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Infogix Data3Sixty reguleren (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Infogix Data3Sixty bepalen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Infogix Data3Sixty reguleren eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Het toevoegen van Infogix Data3Sixty bepalen van de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Het toevoegen van Infogix Data3Sixty bepalen van de galerie
Voor het configureren van de integratie van Infogix Data3Sixty bepalen in Azure AD, moet u Infogix Data3Sixty reguleren toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Infogix Data3Sixty reguleren uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Infogix Data3Sixty reguleren**, selecteer **Infogix Data3Sixty reguleren** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Infogix Data3Sixty bepalen in de lijst met resultaten](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Infogix Data3Sixty bepalen op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Infogix Data3Sixty reguleren is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Infogix Data3Sixty reguleren tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Infogix Data3Sixty bepalen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Infogix Data3Sixty reguleren](#create-an-infogix-data3sixty-govern-test-user)**  - Infogix Data3Sixty bepalen die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Infogix Data3Sixty reguleren.

**Voor het configureren van Azure AD eenmalige aanmelding met Infogix Data3Sixty bepalen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Infogix Data3Sixty reguleren** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

3. Op de **Infogix Data3Sixty reguleren domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Infogix Data3Sixty reguleren domein eenmalige aanmelding informatie](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. In de **id** textbox, typ een URL: `https://data3sixty.com/ui`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.data3sixty.com/sso/acs`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Infogix Data3Sixty reguleren domein eenmalige aanmelding informatie](./media/infogix-tutorial/tutorial_infogix_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Infogix Data3Sixty reguleren Client ondersteuningsteam](mailto:data3sixtysupport@infogix.com) ophalen van deze waarden.

5. De SAML-asserties verwacht Infogix Data3Sixty reguleren toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Single Sign-On attb configureren](./media/infogix-tutorial/tutorial_infogix_attribute.png)
    
6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | Voornaam           | User.givenName |
    | Achternaam        | User.surname |
    | gebruikersnaam       | User.mail    |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/infogix-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding Addattb configureren](./media/infogix-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/infogix-tutorial/tutorial_infogix_certificate.png)

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/infogix-tutorial/tutorial_general_400.png)
    
9. Op de **Infogix Data3Sixty reguleren configuratie** sectie, klikt u op **configureren Infogix Data3Sixty reguleren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Infogix Data3Sixty reguleren configuratie](./media/infogix-tutorial/tutorial_infogix_configure.png) 

10. Eenmalige aanmelding configureren op **Infogix Data3Sixty reguleren** zijde, moet u de gedownloade verzenden **certificaat (Raw), Sign-Out URL SAML entiteit-ID en SAML Single Sign-On Service-URL** naar [ Ondersteuningsteam Infogix Data3Sixty reguleren](mailto:data3sixtysupport@infogix.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/infogix-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/infogix-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/infogix-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/infogix-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Maken van een testgebruiker Infogix Data3Sixty bepalen


Het doel van deze sectie is het maken van een gebruiker die is aangeroepen terwijl Britta Simon Infogix Data3Sixty reguleren. Infogix Data3Sixty reguleren biedt ondersteuning voor just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Infogix Data3Sixty reguleren als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Infogix Data3Sixty reguleren ondersteuningsteam](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen om te bepalen van Infogix Data3Sixty.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Infogix Data3Sixty bepalen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Infogix Data3Sixty reguleren**.

    ![De koppeling Infogix Data3Sixty bepalen in de lijst met toepassingen](./media/infogix-tutorial/tutorial_infogix_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Infogix Data3Sixty bepalen in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Infogix Data3Sixty reguleren.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/infogix-tutorial/tutorial_general_01.png
[2]: ./media/infogix-tutorial/tutorial_general_02.png
[3]: ./media/infogix-tutorial/tutorial_general_03.png
[4]: ./media/infogix-tutorial/tutorial_general_04.png

[100]: ./media/infogix-tutorial/tutorial_general_100.png

[200]: ./media/infogix-tutorial/tutorial_general_200.png
[201]: ./media/infogix-tutorial/tutorial_general_201.png
[202]: ./media/infogix-tutorial/tutorial_general_202.png
[203]: ./media/infogix-tutorial/tutorial_general_203.png

