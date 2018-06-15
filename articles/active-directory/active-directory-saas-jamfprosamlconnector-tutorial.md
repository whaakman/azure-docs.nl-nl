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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 687453b37e15f5d3dd1fa161f89b6d18f90ba279
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34343416"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Zelfstudie: Azure Active Directory-integratie met Jamf Pro

In deze zelfstudie leert u hoe Jamf Pro integreren met Azure Active Directory (Azure AD).

Jamf Pro integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Jamf Pro heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Jamf Pro (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Jamf Pro, moet u de volgende items:

- Een Azure AD-abonnement
- Een Jamf Pro eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jamf Pro uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro uit de galerie toevoegen
Voor het configureren van de integratie van Jamf Pro in Azure AD, moet u Jamf Pro uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jamf Pro uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Jamf Pro**, selecteer **Jamf Pro** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Jamf Pro in de lijst met resultaten](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Jamf Pro op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Jamf Pro is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante Jamf Pro-gebruiker worden gemaakt.

Om te configureren en testen van Azure AD eenmalige aanmelding met Jamf Pro, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Jamf Pro](#create-a-jamf-pro-test-user)**  - Jamf Pro die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Jamf Pro.

**Voor het configureren van Azure AD eenmalige aanmelding met Jamf Pro, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Jamf Pro** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

3. Op de **Jamf Pro domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Jamf Pro domein eenmalige aanmelding informatie](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. In de **id (entiteit-ID)** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.jamfcloud.com/saml/SSO`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Jamf Pro domein eenmalige aanmelding informatie](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. U krijgt de werkelijke waarde van de id van **Single Sign-On** sectie in Jamf Pro-portal, die verderop in de zelfstudie wordt beschreven. U kunt de werkelijke uitpakken **subdomein** waarde van de id-waarde en wordt deze gebruikt **subdomein** informatie in de aanmeldings-URL en de antwoord-URL.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster, meld u bij uw bedrijf Jamf Pro site als beheerder.

8. Klik op de **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Jamf Pro-configuratie](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure1.png)

9. Klik op **eenmalige aanmelding**.

    ![Jamf Pro-configuratie](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure2.png)

10. Schuif omlaag maximaal **IDENTITEITSPROVIDER** onder de **Single Sign-On** sectie en voer de volgende stappen uit:

    ![Jamf Pro-configuratie](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure3.png)

    a. Selecteer **andere** als een optie uit de **IDENTITEITSPROVIDER** vervolgkeuzelijst.

    b. In de **andere PROVIDER** textbox Voer **Azure AD**.

    c. Selecteer **metagegevens-URL** als een optie uit de **identiteit PROVIDER METAGEGEVENSBRON** dropdown en plak in het volgende tekstvak de **App-Url voor federatieve metagegevens** die waarde u hebt gekopieerd uit de Azure portal.

    d. Kopieer de **entiteit-ID** vlaue en plak deze in de **id (entiteit-ID)** textbox in **Jamf Pro domein en de URL's** sectie op Azure-portal.

    >[!NOTE]
    > Hier `aadsso` is het gedeelte subdomein (dit is voor verwijzing doel). Deze waarde wordt gebruikt voor het voltooien van de aanmeldings-URL en de antwoord-URL in de **Jamf Pro domein en de URL's** sectie op Azure-portal.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-jamf-pro-test-user"></a>Maak een testgebruiker Jamf Pro

Om Azure AD-gebruikers zich aanmelden bij Jamf Pro, moeten ze worden ingericht in Jamf Pro. In het geval van Jamf Pro is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Jamf Pro site als beheerder.

2. Klik op de **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Werknemer toevoegen](./media/active-directory-saas-jamfprosamlconnector-tutorial/configure1.png)

3. Klik op **gebruikersaccounts Jamf Pro & groepen**.

    ![Werknemer toevoegen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user1.png)

4. Klik op **Nieuw**.

    ![Werknemer toevoegen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user2.png)

5. Selecteer **standaardaccount maken**.

    ![Werknemer toevoegen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user3.png)

6. Op de **nieuwe Account** dailog, voer de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-jamfprosamlconnector-tutorial/user4.png)

    a. In de **gebruikersnaam** textbox, typt u de volledige naam van BrittaSimon.

    b. Selecteer juiste opties aan de hand van uw organisatie voor **TOEGANGSNIVEAU**, **bevoegdheid ingesteld**, en voor **TOEGANGSSTATUS**.
    
    c. In de **volledige naam** textbox, typt u de volledige naam van Britta Simon.

    d. In de **e-MAILADRES** textbox, typ de e-mailadres van Britta Simon account.

    e. In de **wachtwoord** textbox, typt u het wachtwoord van de gebruiker.

    f. In de **wachtwoord bevestigen** textbox, typt u het wachtwoord van de gebruiker.

    g. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Jamf Pro.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Jamf Pro, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Jamf Pro**.

    ![De Jamf Pro-koppeling in de lijst met toepassingen](./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Jamf Pro in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Jamf Pro.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jamfprosamlconnector-tutorial/tutorial_general_203.png

