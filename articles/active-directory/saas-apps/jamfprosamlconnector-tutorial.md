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
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: d28e28a2c4f8144da16c4838f07c9b8bb5ce67f0
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268154"
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

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jamf Pro uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro uit de galerie toe te voegen

Voor het configureren van de integratie van Jamf Pro in Azure AD, moet u Jamf Pro uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jamf Pro uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/jamfprosamlconnector-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/jamfprosamlconnector-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/jamfprosamlconnector-tutorial/a_new_app.png)

4. Typ in het zoekvak **Jamf Pro**, selecteer **Jamf Pro** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/jamfprosamlconnector-tutorial/a_add_app.png)

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

1. In de [Azure-portal](https://portal.azure.com/)op de **Jamf Pro** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_select_sso.png)

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_sso.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/jamfprosamlconnector-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    a. In de **id** tekstvak typt u een URL met behulp van het volgende patroon: `https://<subdomain>.jamfcloud.com/saml/metadata`.

    b. In de **antwoord-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://<subdomain>.jamfcloud.com/saml/SSO`.

    ![image](./media/jamfprosamlconnector-tutorial//b2-domains_and_urls.png)

    c. Klik op **extra URL's instellen**.

    d. In de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://<subdomain>.jamfcloud.com`.

    ![image](./media/jamfprosamlconnector-tutorial//b4-domains_and_urls.png)

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. U krijgt de werkelijke id-waarde van **Single Sign-On** sectie in Jamf Pro-portal, die later in de zelfstudie wordt uitgelegd. U kunt de werkelijke uitpakken **subdomein** waarde uit de id-waarde en gebruikt u deze **subdomein** informatie in de aanmeldings-URL en antwoord-URL.

6. Op de **instellen van eenmalige aanmelding met SAML** pagina, In de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en sla deze op uw computer.

    ![image](./media/jamfprosamlconnector-tutorial/C2_certificate.png)

7. Voor het automatiseren van de configuratie in Jamf Pro, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)
 
8. Na het toevoegen van uitbreiding naar de browser, klikt u op **instellen-Jamf Pro** wordt u doorgeleid naar de toepassing Jamf Pro. Van daaruit, geef de referenties van de beheerder zich aanmelden bij Jamf Pro. De browserextensie wordt automatisch configureren van de toepassing voor u en stappen 9-12 automatiseren.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

9. Als u Jamf Pro handmatig instellen wilt, opent u een nieuw browservenster en log in uw site van het bedrijf Jamf Pro als beheerder en voer de volgende stappen uit:

10. Klik op de **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Jamf Pro-configuratie](./media/jamfprosamlconnector-tutorial/configure1.png)

11. Klik op **eenmalige aanmelding**.

    ![Jamf Pro-configuratie](./media/jamfprosamlconnector-tutorial/configure2.png)

12. Op de **Single Sign-On** pagina de volgende stappen uitvoeren:

    ![Jamf Pro één](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Selecteer **Jamf Pro Server** om eenmalige aanmelding toegang te krijgen.

    b. Door het selecteren van **bypass voor alle gebruikers toestaan** gebruikers niet omgeleid naar de aanmeldingspagina van de id-Provider voor verificatie, maar kunt aanmelden bij Jamf Pro rechtstreeks in plaats daarvan. Wanneer een gebruiker probeert toegang tot Jamf Pro via de id-Provider, treedt IdP gestart door SSO-verificatie en autorisatie op.

    c. Selecteer de **NameID** optie voor **GEBRUIKERSTOEWIJZING: SAML**. Deze instelling is standaard ingesteld op **NameID** , maar u kunt een aangepast kenmerk definiëren.

    d. Selecteer **e** voor **GEBRUIKERSTOEWIJZING: JAMF PRO**. Jamf Pro toegewezen SAML-kenmerken die zijn verzonden door de id-provider op de volgende manieren: door gebruikers en groepen. Wanneer een gebruiker probeert toegang te kunnen Jamf Pro standaard Jamf Pro Hiermee haalt u informatie over de gebruiker van de id-Provider en komt overeen met het tegen Jamf Pro-gebruikersaccounts. Als het binnenkomende gebruikersaccount niet in Jamf Pro bestaat, klikt u vervolgens optreedt groep namen matchen.

    e. Plak de waarde `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` in de **KENMERK GROEPSNAAM** tekstvak.

13. Op de dezelfde pagina Schuif omlaag in maximaal **id-PROVIDER** onder de **Single Sign-On** sectie en voer de volgende stappen uit:

    ![Jamf Pro-configuratie](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecteer **andere** als een optie in de **id-PROVIDER** vervolgkeuzelijst.

    b. In de **andere PROVIDER** tekstvak, voer **Azure AD**.

    c. Selecteer **metagegevens-URL** als een optie in de **IDENTITEITSBRON PROVIDER metagegevens** vervolgkeuzelijst en plak in het volgende tekstvak de **App-Url voor federatieve metagegevens** waarvan de waarde u hebt gekopieerd vanuit Azure portal.

    d. Kopieer de **entiteit-ID** waarde en plak deze in de **id (entiteits-ID)** -tekstvak in **Jamf Pro domein en URL's** sectie in Azure portal.

    >[!NOTE]
    > Hier is vage waarde het gedeelte van het subdomein. Deze waarde gebruiken voor het voltooien van de aanmeldings-URL en de antwoord-URL in de **Jamf Pro domein en URL's** sectie in Azure portal.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/jamfprosamlconnector-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/jamfprosamlconnector-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/jamfprosamlconnector-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

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

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Jamf Pro**.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Jamf Pro**.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_proapplications.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/jamfprosamlconnector-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/jamfprosamlconnector-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Jamf Pro in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Jamf Pro.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
