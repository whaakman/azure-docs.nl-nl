---
title: 'Zelfstudie: Azure Active Directory-integratie met Soloinsight-CloudGate SSO | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Soloinsight-CloudGate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9263c241-85a4-4724-afac-0351d6275958
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2dc6dcbcdee8df93f34cf4d68b5e08453554bc8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67090014"
---
# <a name="tutorial-integrate-soloinsight-cloudgate-sso-with-azure-active-directory"></a>Zelfstudie: Soloinsight CloudGate SSO integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Soloinsight CloudGate SSO integreert met Azure Active Directory (Azure AD). Wanneer u Soloinsight CloudGate SSO met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Soloinsight CloudGate eenmalige aanmelding heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Soloinsight CloudGate eenmalige aanmelding met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Soloinsight CloudGate SSO eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor eenmalige aanmelding Soloinsight CloudGate **SP** gestart door SSO.

## <a name="adding-soloinsight-cloudgate-sso-from-the-gallery"></a>Soloinsight-CloudGate SSO toevoegen uit de galerie

Voor de configuratie van de integratie van Soloinsight-CloudGate SSO in Azure Active Directory, moet u Soloinsight-CloudGate SSO vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Soloinsight CloudGate SSO** in het zoekvak in.
1. Selecteer **Soloinsight CloudGate SSO** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Soloinsight CloudGate eenmalige aanmelding met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Soloinsight CloudGate SSO vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Soloinsight CloudGate eenmalige aanmelding, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van eenmalige aanmelding Soloinsight CloudGate](#configure-soloinsight-cloudgate-sso)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak Soloinsight CloudGate SSO testgebruiker](#create-soloinsight-cloudgate-sso-test-user)**  hebben een equivalent van Britta Simon in Soloinsight CloudGate eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Soloinsight CloudGate SSO** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    1. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.sigateway.com/login`

    1. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.sigateway.com/process/sso`

   > [!NOTE]
   > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke URL voor eenmalige aanmelding en de id, zoals later in de zelfstudie wordt uitgelegd, in de sectie **Eenmalige aanmelding voor Soloinsight-CloudGate SSO configureren**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **Soloinsight CloudGate SSO instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-soloinsight-cloudgate-sso"></a>Configure Soloinsight-CloudGate SSO

1. Voor het automatiseren van de configuratie in Soloinsight CloudGate SSO, moet u installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Soloinsight CloudGate SSO** wordt u doorgeleid naar de Soloinsight CloudGate SSO-toepassing. Van daaruit, bieden de referenties van de beheerder zich aanmelden bij Soloinsight CloudGate eenmalige aanmelding. De browserextensie wordt automatisch configureren van de toepassing voor u en automatiseren van stap 3-8.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Soloinsight CloudGate eenmalige aanmelding handmatig instellen wilt, opent u een nieuw browservenster en meld u aan bij uw site van het bedrijf Soloinsight CloudGate SSO als beheerder en voer de volgende stappen uit:

4. Als u de waarden die worden geplakt in Azure portal tijdens het configureren van Basic SAML, zich aanmelden bij de CloudGate Web-Portal met uw referenties en toegang tot de SSO-instellingen, die te in het volgende pad vinden **Start > Beheer > Instellingen > Algemeen**.

    ![CloudGate SSO-instellingen](./media/soloinsight-cloudgate-sso-tutorial/sso-main-settings.png)

5. **URL voor SAML-consument**

    * Kopiëren van de koppelingen op basis van de **URL voor Saml-consument** en de **Omleidings-URL** velden en plak deze in de Azure-portal **SAML-basisconfiguratie** sectie voor  **Id (entiteits-ID)** en **antwoord-URL** respectievelijk in velden.

        ![SAMLIdentifier](./media/soloinsight-cloudgate-sso-tutorial/saml-identifier.png)

6. **SAML-handtekeningcertificaat**

    * Ga naar de bron van het certificaat (Base64)-bestand dat is gedownload van Azure portal SAML-handtekeningcertificaat lijsten en met de rechtermuisknop op het. Kies in de lijst de optie **Bewerken met Notepad++** . 

        ![SAMLcertificate](./media/soloinsight-cloudgate-sso-tutorial/certificate-file.png)

    * Kopieer de inhoud in het (Base64)-certificaat naar het Notepad++-bestand.

        ![Certificaat kopiëren](./media/soloinsight-cloudgate-sso-tutorial/certificate-copy.png)

    * Plak de inhoud in de CloudGate Web Portal SSO-instellingen in het veld **Certificaat** en klik op de knop Opslaan.

        ![Certificaatportal](./media/soloinsight-cloudgate-sso-tutorial/certificate-portal.png)

7. **Standaardgroep**

    * Selecteer in de CloudGate Web Portal in de vervolgkeuzelijst voor de optie **Default Group** (Standaardgroep) **Business Admin** (Bedrijfsbeheerder)

        ![Standaardgroep](./media/soloinsight-cloudgate-sso-tutorial/default-group.png)

8. **AD-id en aanmeldings-URL**

    * De gekopieerde **aanmeldings-URL** vanuit Azure portal **Soloinsight CloudGate SSO instellen** configuraties worden ingevoerd in de sectie van de instellingen voor eenmalige Webaanmelding Portal CloudGate zijn.

    * Plak de **aanmeldings-URL** koppeling vanuit Azure portal in de webportal CloudGate **AD aanmeldings-URL** veld.

    * Plak de **Azure AD-id** koppeling vanuit Azure portal in de webportal CloudGate **AD id** veld

        ![AD-aanmelding](./media/soloinsight-cloudgate-sso-tutorial/ad-login.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `Britta Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie zult u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Soloinsight CloudGate eenmalige aanmelding inschakelen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen **Soloinsight-CloudGate SSO**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-soloinsight-cloudgate-sso-test-user"></a>Soloinsight CloudGate SSO-testgebruiker maken

Om een testgebruiker te maken, selecteert u **Employees** (Werknemers) in het hoofdmenu van uw CloudGate Web Portal en vult u het formulier voor het toevoegen van nieuwe werknemers in. Het machtigingsniveau dat aan de testgebruiker moet worden toegewezen is **Business Admin** (Bedrijfsbeheerder). Klik op **Create** (Maken) wanneer u alle vereiste velden hebt ingevuld.

![Werknemertest](./media/soloinsight-cloudgate-sso-tutorial/employee-test.png)

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Soloinsight CloudGate SSO in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Soloinsight CloudGate SSO waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)