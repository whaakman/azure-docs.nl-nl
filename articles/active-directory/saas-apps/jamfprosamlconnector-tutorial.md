---
title: 'Zelfstudie: Azure Active Directory-integratie met Jamf Pro | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 794039ee1a5b1cf3b382e0f0769383b1e033e982
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046935"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Zelfstudie: Azure Active Directory-integratie met Jamf Pro

In deze zelfstudie leert u hoe u Jamf Pro integreren met Azure Active Directory (Azure AD).

Jamf Pro integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Jamf Pro heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Jamf Pro (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met Jamf Pro configureren, moet u de volgende items:

- Een Azure AD-abonnement
- Een door Jamf Pro eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jamf Pro uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro uit de galerie toe te voegen
Voor het configureren van de integratie van Jamf Pro in Azure AD, moet u Jamf Pro uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jamf Pro uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Jamf Pro**, selecteer **Jamf Pro** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Jamf Pro in de lijst met resultaten](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Jamf Pro op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Jamf Pro is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Jamf Pro tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met Jamf Pro testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Jamf Pro](#create-a-jamf-pro-test-user)**  : als u wilt een equivalent van Britta Simon in Jamf Pro die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Jamf Pro.

**Voor het configureren van Azure AD eenmalige aanmelding met Jamf Pro, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Jamf Pro** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

3. Op de **Jamf Pro domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Jamf Pro domein en URL's, eenmalige aanmelding informatie](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.jamfcloud.com/saml/SSO`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Jamf Pro domein en URL's, eenmalige aanmelding informatie](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. U krijgt de werkelijke id-waarde van **Single Sign-On** sectie in Jamf Pro-portal, die later in de zelfstudie wordt uitgelegd. U kunt de werkelijke uitpakken **subdomein** waarde uit de id-waarde en gebruikt u deze **subdomein** informatie in de aanmeldings-URL en antwoord-URL.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
7. Meld u in een ander browservenster in uw bedrijf Jamf Pro site als beheerder.

8. Klik op de **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Jamf Pro-configuratie](./media/jamfprosamlconnector-tutorial/configure1.png)

9. Klik op **eenmalige aanmelding**.

    ![Jamf Pro-configuratie](./media/jamfprosamlconnector-tutorial/configure2.png)

10. Schuif omlaag in maximaal **id-PROVIDER** onder de **Single Sign-On** sectie en voer de volgende stappen uit:

    ![Jamf Pro-configuratie](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecteer **andere** als een optie in de **id-PROVIDER** vervolgkeuzelijst.

    b. In de **andere PROVIDER** tekstvak, voer **Azure AD**.

    c. Selecteer **metagegevens-URL** als een optie in de **IDENTITEITSBRON PROVIDER metagegevens** vervolgkeuzelijst en plak in het volgende tekstvak de **App-Url voor federatieve metagegevens** waarvan de waarde u hebt gekopieerd vanuit Azure portal.

    d. Kopieer de **entiteit-ID** vlaue en plak deze in de **id (entiteits-ID)** -tekstvak in **Jamf Pro domein en URL's** sectie in Azure portal.

    >[!NOTE]
    > Hier `aadsso` is het gedeelte van het subdomein (dit is voor verwijzing doel). Deze waarde gebruiken voor het voltooien van de aanmeldings-URL en de antwoord-URL in de **Jamf Pro domein en URL's** sectie in Azure portal.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-jamf-pro-test-user"></a>Maak een testgebruiker Jamf Pro

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Jamf Pro, moeten ze worden ingericht in Jamf Pro. In het geval van Jamf Pro is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Jamf Pro site aan als beheerder.

2. Klik op de **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Klik op **Jamf Pro-gebruikersaccounts en groepen**.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user1.png)

4. Klik op **Nieuw**.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user2.png)

5. Selecteer **van het type Standard maken**.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user3.png)

6. Op de **nieuw Account** dailog, voer de volgende stappen uit:

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user4.png)

    a. In de **gebruikersnaam** tekstvak typt u de volledige naam van BrittaSimon.

    b. Selecteer opties aan de hand van uw organisatie voor **TOEGANGSNIVEAU**, **bevoegdheden instellen**, en voor **TOEGANGSSTATUS**.
    
    c. In de **volledige naam** tekstvak typt u de volledige naam van Britta Simon.

    d. In de **e-MAILADRES** tekstvak typt u het e-mailadres van Britta Simon-account.

    e. In de **wachtwoord** tekstvak typt u het wachtwoord van de gebruiker.

    f. In de **Bevestig uw wachtwoord** tekstvak typt u het wachtwoord van de gebruiker.

    g. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot Jamf Pro.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon met Jamf Pro, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Jamf Pro**.

    ![De Jamf Pro-koppeling in de lijst met toepassingen](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Jamf Pro in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Jamf Pro.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/jamfprosamlconnector-tutorial/tutorial_general_203.png

