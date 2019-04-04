---
title: 'Zelfstudie: Azure Active Directory-integratie met Teamphoria | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Teamphoria.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 6a65eafb927f70b650218cc8c7ba21d00df6d54c
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918583"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Zelfstudie: Azure Active Directory-integratie met Teamphoria

In deze zelfstudie leert u hoe u Teamphoria integreren met Azure Active Directory (Azure AD).
Teamphoria integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Teamphoria heeft.
* U kunt uw gebruikers worden automatisch aangemeld Teamphoria (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Teamphoria, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Teamphoria ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Teamphoria **SP** gestart door SSO

## <a name="adding-teamphoria-from-the-gallery"></a>Teamphoria uit de galerie toe te voegen

Voor het configureren van de integratie van Teamphoria in Azure AD, moet u Teamphoria uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Teamphoria uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Teamphoria**, selecteer **Teamphoria** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Teamphoria in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Teamphoria op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Teamphoria tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Teamphoria, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Teamphoria](#configure-teamphoria-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Teamphoria](#create-teamphoria-test-user)**  : als u wilt een equivalent van Britta Simon in Teamphoria die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Teamphoria, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Teamphoria** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Teamphoria domein en URL's, eenmalige aanmelding informatie](common/sp-intiated.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<sub-domain>.teamphoria.com/login`

    > [!NOTE]
    > De waarde voor de aanmeldings-URL is niet echt. U moet deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Teamphoria Client ondersteuningsteam](https://www.teamphoria.com/) om op te halen van de aanmeldings-URL. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Teamphoria instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-teamphoria-single-sign-on"></a>Teamphoria voor eenmalige aanmelding configureren

1. Het configureren van eenmalige aanmelding op **Teamphoria** aan clientzijde, meld u aan bij uw toepassing Teamphoria als beheerder.

1. Ga naar **BEHEERDERSINSTELLINGEN** optie in de werkbalk links en op het tabblad configureren klikt u op **EENMALIGE aanmelding** om de SSO-configuratie-venster te openen.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/admin_sso_configure.png)

1. Klik op **nieuwe id-PROVIDER toevoegen** optie in de rechterbovenhoek om het formulier voor het toevoegen van de instellingen voor eenmalige aanmelding te openen.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/add_new_identity_provider.png)

1. Voer de details in de velden, zoals beschreven onder-

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **WEERGAVENAAM**: Voer de weergavenaam van de invoegtoepassing op de beheerpagina.

    b. **NAAM VAN DE KNOP**: De naam van het tabblad dat wordt weergegeven op de aanmeldingspagina voor logboekregistratie via eenmalige aanmelding.

    c. **CERTIFICAAT**: Open het certificaat eerder hebt gedownload vanuit Azure portal in Kladblok en kopieer de inhoud van dezelfde en plak deze hier in het vak.

    d. **TOEGANGSPUNT**: Plak de **aanmeldings-URL** eerder hebt gekopieerd vanuit Azure portal.

    e. Schakel de optie voor het **ON** en klikt u op **opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Teamphoria.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Teamphoria**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Teamphoria**.

    ![De koppeling Teamphoria in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-teamphoria-test-user"></a>Teamphoria testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Teamphoria, moeten ze worden ingericht voor Teamphoria. In het geval van Teamphoria is inrichten een handmatige taak.

**Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:**

1. Meld u aan uw bedrijf Teamphoria site als een beheerder.

1. Klik op **ADMIN** instellingen op de werkbalk links en klikt u onder de **beheren** Klik op tabblad **gebruikers** openen de beheerpagina voor gebruikers.

    ![Werknemer toevoegen](./media/teamphoria-tutorial/admin_manage_users.png)

1. Klik op de **handmatig UITNODIGEN** optie.

    ![Anderen uitnodigen](./media/teamphoria-tutorial/admin_manage_add_users.png)

1. Op deze pagina uitvoeren na de actie.

    ![Anderen uitnodigen](./media/teamphoria-tutorial/manual_user_invite.png)

    a. In de **e-MAILADRES** tekstvak, voer de **e-mailadres** van de gebruiker, zoals BrittaSimon.

    b. In de **VOORNAAM** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    c. In de **ACHTERNAAM** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.

    d. Klik op **uitnodiging 1 gebruiker**. Gebruiker moet de uitdaging aannemen farmherstelpunten gemaakt in het systeem.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Teamphoria in het toegangsvenster, moet u worden automatisch aangemeld bij de Teamphoria waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

