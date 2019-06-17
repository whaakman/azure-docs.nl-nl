---
title: 'Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Atlassian Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: celested
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdbfb07b73d591bbab64f38e8c986fb1b7e072a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106590"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>Zelfstudie: Atlassian Cloud integreren met Azure Active Directory

In deze zelfstudie leert u over het integreren van Atlassian Cloud met Azure Active Directory (Azure AD). Wanneer u Atlassian Cloud hebt geïntegreerd met Azure AD, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot de Atlassian-Cloud heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Atlassian Cloud met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Atlassian Cloud eenmalige aanmelding (SSO).
* Als u eenmalige aanmelding van Security Assertion Markup Language (SAML) wilt instellen voor Atlassian Cloud-producten, moet u Atlassian Access instellen. Meer informatie over [Atlassian Access]( https://www.atlassian.com/enterprise/cloud/identity-manager).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Atlassian Cloud ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Atlassian Cloud toevoegen vanuit de galerie

Als u de integratie van Atlassian Cloud met Azure AD wilt configureren, voegt u Atlassian Cloud vanuit de galerie toe aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Atlassian Cloud** in het zoekvak in.
1. Selecteer **Atlassian Cloud** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Atlassian Cloud met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Atlassian Cloud vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Atlassian Cloud, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van Atlassian Cloud SSO](#configure-atlassian-cloud-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak Atlassian Cloud testgebruiker](#create-atlassian-cloud-test-user)**  : als u wilt een equivalent van B.Simon in Atlassian-Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Atlassian Cloud** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus, voer de waarden voor de volgende velden:

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://auth.atlassian.com/saml/<unique ID>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Klik op **Extra URL's instellen**.

    d. In het tekstvak **Relaystatus** typt u een URL met de volgende notatie: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > De bovenstaande waarden zijn niet echt. Werk deze waarden bij met de werkelijke id en antwoord-URL. U krijgt deze echte waarden uit de **SAML-configuratie van Atlassian Cloud** scherm die later in wordt uitgelegd de **configureren Atlassian Cloud Single Sign-On** van de zelfstudie.

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > De waarde van de aanmeldings-URL is niet echt. Plak de waarde van het exemplaar waarmee u kunt aanmelden met naar de Atlassian Cloud-beheerportal.

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-10.png)

1. De Atlassian Cloud-toepassing verwacht SAML-asserties in een specifieke indeling. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van uw SAML-tokenkenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen met **user.userprincipalname**. In de Atlassian Cloud-toepassing wordt verwacht dat **nameidentifier** is toegewezen aan **user.mail**. Daarom moet u de kenmerktoewijzing bewerken door op het pictogram **Bewerken** te klikken en de kenmerktoewijzing te wijzigen.

    ![image](common/edit-attribute.png)

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Kopieer in de sectie **Atlassian Cloud instellen** de juiste URL('s) overeenkomstig wat u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-atlassian-cloud-sso"></a>Atlassian Cloud eenmalige aanmelding configureren

1. Voor het automatiseren van de configuratie in Atlassian Cloud, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Atlassian Cloud** wordt u doorgeleid naar de cloudtoepassing Atlassian. Geef de beheerdersreferenties voor aanmelding bij Atlassian Cloud daar. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 3-7 geautomatiseerd.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Atlassian Cloud handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf Atlassian Cloud als beheerder en voer de volgende stappen uit:

4. U moet uw domein verifiëren voordat u eenmalige aanmelding configureert. Zie het document [Atlassian-domeinverificatie](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) voor meer informatie.

5. Selecteer in het linkerdeelvenster **Security** > **eenmalige aanmelding SAML**. Abonneer u op Atlassian Identity Manager als u dat nog niet had gedaan.

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-11.png)

6. Doe het volgende in het venster **SAML-configuratie toevoegen**:

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-12.png)

    a. In de **id-provider entiteit-ID** vak, plak de **Azure AD-id** die u hebt gekopieerd vanuit Azure portal.

    b. In de **id-provider voor eenmalige aanmelding URL** vak, plak de **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. Open het certificaat dat u uit Azure Portal hebt gedownload als TXT-bestand, kopieer de waarde (zonder de regels *Begin certificaat* en *Einde certificaat*) en plak deze in het vak **Openbaar X509-certificaat**.

    d. Klik op **Configuratie opslaan**.

7. Om er zeker van te zijn dat u de juiste URL's hebt ingesteld, werkt u de Azure AD-instellingen bij door het volgende te doen:

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-13.png)

    a. Kopieer in het SAML-venster de **SP identiteit-ID** en klikt u op de Azure-portal, onder Atlassian Cloud **SAML-basisconfiguratie**, plak deze in de **id** vak.

    b. Kopieer in het SAML-venster de **SP URL van de Bevestigingsconsumerservice** en klikt u op de Azure-portal, onder Atlassian Cloud **SAML-basisconfiguratie**, plak deze in de **antwoord-URL**vak. De aanmeldings-URL is de tenant-URL van uw Atlassian Cloud.

    > [!NOTE]
    > Als u een bestaande klant bent, selecteert u na het bijwerken van de **SP-id** en de **URL voor Assertion Consumer Service van de SP** in Azure Portal de optie **Ja, configuratie bijwerken**. Als u een nieuwe klant bent, kunt u deze stap overslaan.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Atlassian-Cloud.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Atlassian Cloud**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-atlassian-cloud-test-user"></a>Een Atlassian Cloud-testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij Atlassian Cloud, richt u in Atlassian Cloud de gebruikersaccounts op de volgende manier handmatig in:

1. In het deelvenster **Beheer** selecteert u **Gebruikers**.

    ![De koppeling Gebruikers in Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

2. Als u in Atlassian Cloud een gebruiker wilt maken, selecteert u **Gebruiker uitnodigen**.

    ![Een Atlassian Cloud-gebruiker maken](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

3. In het vak **E-mailadres** voert u het e-mailadres van de gebruiker in. Wijs dan toegang tot de toepassing toe.

    ![Een Atlassian Cloud-gebruiker maken](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

4. Als u per e-mail een uitnodiging naar de gebruiker wilt verzenden, selecteert u **Gebruikers uitnodigen**. Er wordt een e-mailuitnodiging naar de gebruiker verzonden. Als de gebruiker de uitnodiging heeft geaccepteerd, wordt deze actief in het systeem.

> [!NOTE]
> U kunt ook bulksgewijs gebruikers maken door de optie **Bulksgewijs maken** in het gedeelte **Gebruikers** te selecteren.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Atlassian Cloud in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Atlassian-Cloud waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)