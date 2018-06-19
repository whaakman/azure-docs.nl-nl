---
title: 'Zelfstudie: Azure Active Directory-integratie met de persoonlijke Portal Mimecast | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en persoonlijke Mimecast-Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: jeedes
ms.openlocfilehash: a1c1f0a3f3a742c1aa5baa55db1796b3a2ae7802
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907401"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast persoonlijke Portal

In deze zelfstudie leert u hoe Mimecast persoonlijke Portal integreren met Azure Active Directory (Azure AD).

Mimecast persoonlijke Portal integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de persoonlijke Portal Mimecast heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Mimecast persoonlijke Portal (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mimecast persoonlijke Portal, moet u de volgende items:

- Een Azure AD-abonnement
- Een persoonlijke Portal Mimecast eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van de persoonlijke Portal Mimecast uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Toevoegen van de persoonlijke Portal Mimecast uit de galerie
Voor het configureren van de integratie van persoonlijke Portal Mimecast in Azure AD, moet u Mimecast persoonlijke Portal uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mimecast persoonlijke Portal uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Mimecast persoonlijke Portal**, selecteer **Mimecast persoonlijke Portal** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Mimecast persoonlijke Portal in de lijst met resultaten](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Mimecast persoonlijke Portal op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Mimecast persoonlijke Portal is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante Mimecast persoonlijke Portal-gebruiker worden gemaakt.

Om te configureren en testen van Azure AD eenmalige aanmelding met Mimecast persoonlijke Portal, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Mimecast persoonlijke Portal](#create-a-mimecast-personal-portal-test-user)**  - Mimecast persoonlijke Portal dat is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Mimecast persoonlijke Portal configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Mimecast persoonlijke Portal, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mimecast persoonlijke Portal** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. Op de **Mimecast persoonlijke Portal domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Mimecast persoonlijke Portal domein eenmalige aanmelding informatie](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL: 

    | Regio  |  Waarde | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Verenigde Staten   | `https://us-api.mimecast.com/login/saml`|
    | Zuid-Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Australië       | `https://au-api.mimecast.com/login/saml`|
    | Drijvende        | `https://jer-api.mimecast.com/login/saml`|

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:

    | Regio  |  Waarde | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Verenigde Staten   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Zuid-Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Australië       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Drijvende        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. In de **antwoord-URL** textbox, typ een URL: 

    | Regio  |  Waarde | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Verenigde Staten   | `https://us-api.mimecast.com/login/saml`|
    | Zuid-Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Australië       | `https://au-api.mimecast.com/login/saml`|
    | Drijvende        | `https://jer-api.mimecast.com/login/saml`|
    
    > [!NOTE] 
    > De id-waarde is geen echte. Werk de waarde met de werkelijke identificatie. Neem contact op met [Mimecast persoonlijke Portal Client ondersteuningsteam](http://www.mimecast.com/customer-success/technical-support/) de waarde op te halen. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. Op de **Mimecast persoonlijke Portal-configuratie** sectie, klikt u op **Mimecast persoonlijke Portal configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van persoonlijke Mimecast-Portal](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. In een ander browservenster, meld u bij uw persoonlijke Mimecast-Portal als beheerder.

8. Ga naar **Services \> toepassingen**.
   
    ![Toepassingen](./media/mimecast-personal-portal-tutorial/ic794998.png "toepassingen")

9. Klik op **verificatie profielen**.
   
    ![Verificatie profielen](./media/mimecast-personal-portal-tutorial/ic794999.png "verificatie-profielen")

10. Klik op **nieuwe Authentication profiel**.
   
    ![Nieuwe Authentication profiel](./media/mimecast-personal-portal-tutorial/ic795000.png "nieuwe profiel voor verificatie")

11. In de **Authentication profiel** sectie, voert u de volgende stappen uit:
   
    ![Authentication profiel](./media/mimecast-personal-portal-tutorial/ic795001.png "Authentication profiel")
   
    a. In de **beschrijving** textbox, typ een naam voor uw configuratie.
   
    b. Selecteer **afdwingen van SAML-verificatie voor Mimecast Personal Portal**.
   
    c. Als **Provider**, selecteer **Azure Active Directory**.
   
    d. In **URL-verlener** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.
   
    e. In **aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
   
    f. In **afmelding URL** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    g. Open uw **base 64-** in Kladblok een gecodeerd certificaat gedownload vanuit Azure-portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **Provider identiteitscertificaat (metagegevens)** textbox.

    h. Selecteer **toestaan voor eenmalige op**.
   
    i. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/mimecast-personal-portal-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/mimecast-personal-portal-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/mimecast-personal-portal-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/mimecast-personal-portal-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-mimecast-personal-portal-test-user"></a>Een testgebruiker Mimecast persoonlijke Portal maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Mimecast persoonlijke Portal, moeten ze worden ingericht bij Mimecast persoonlijke Portal. Inrichting is een handmatige taak in het geval van persoonlijke Mimecast-Portal.

U moet een domein registreren voordat u gebruikers kunt maken.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Mimecast persoonlijke Portal** als administrator.

2. Ga naar **mappen \> interne**.
   
    ![Mappen](./media/mimecast-personal-portal-tutorial/ic795003.png "mappen")

3. Klik op **nieuwe domein registreren**.
   
    ![Nieuwe domein registreren](./media/mimecast-personal-portal-tutorial/ic795004.png "nieuwe domein registreren")

4. Nadat het nieuwe domein is gemaakt, klikt u op **nieuw adres**.
   
    ![Nieuw adres](./media/mimecast-personal-portal-tutorial/ic795005.png "nieuw adres")

5. In het dialoogvenster Nieuw adres Voer de volgende stappen uit van een geldig Azure AD-account die u inrichten wilt:
   
    ![Sla](./media/mimecast-personal-portal-tutorial/ic795006.png "opslaan")
   
    a. In de **e-mailadres** textbox type **e-mailadres** van de gebruiker als **BrittaSimon@contoso.com**.
    
    b. In de **globale naam** textbox type de **gebruikersnaam** als **BrittaSimon**.

    c. In de **wachtwoord**, en **wachtwoord bevestigen** tekstvakken, typ de **wachtwoord** van de gebruiker.
   
    b. Klik op **Opslaan**.

>[!NOTE]
>U kunt geen andere hulpprogramma's voor persoonlijke Portal Mimecast gebruiker-account maken of API's die worden geleverd door Mimecast persoonlijke Portal gebruiken voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot persoonlijke Portal Mimecast.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Mimecast persoonlijke Portal, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Mimecast persoonlijke Portal**.

    ![De Mimecast persoonlijke Portal-koppeling in de lijst met toepassingen](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Mimecast persoonlijke Portal in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Mimecast persoonlijke Portal.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-personal-portal-tutorial/tutorial_general_203.png

