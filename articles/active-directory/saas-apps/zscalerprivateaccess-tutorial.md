---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler persoonlijke toegang (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 984498a2b9d4d72ee6bb6b9f0a9e62636bf870bf
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226440"
---
# <a name="tutorial-integrate-zscaler-private-access-zpa-with-azure-active-directory"></a>Zelfstudie: Zscaler persoonlijke toegang (ZPA) integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Zscaler persoonlijke toegang (ZPA) integreren met Azure Active Directory (Azure AD). Wanneer u Zscaler persoonlijke toegang (ZPA) met Azure AD integreren, kunt u het volgende doen:

* Beheren in Azure AD die toegang hebben naar Zscaler persoonlijke toegang (ZPA).
* Kunnen uw gebruikers worden automatisch aangemeld op Zscaler persoonlijke toegang (ZPA) met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement Zscaler persoonlijke toegang (ZPA) eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor persoonlijke toegang (ZPA) Zscaler **SP** gestart door SSO.

## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Zscaler persoonlijke toegang (ZPA) uit de galerie toe te voegen

Voor het configureren van de integratie van de Zscaler persoonlijke toegang (ZPA) in Azure AD, moet u Zscaler persoonlijke toegang (ZPA) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Zscaler persoonlijke toegang (ZPA)** in het zoekvak in.
1. Selecteer **Zscaler persoonlijke toegang (ZPA)** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA) met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een relatie tot stand brengt een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler persoonlijke toegang (ZPA).

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van de Zscaler persoonlijke toegang (ZPA)](#configure-zscaler-private-access-zpa)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak de testgebruiker Zscaler persoonlijke toegang (ZPA)](#create-zscaler-private-access-zpa-test-user)**  hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang (ZPA) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Zscaler persoonlijke toegang (ZPA)** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    1. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    1. Typ een URL in het vak **Id (Entiteits-id)** : `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE]
    > De waarde voor **Aanmeldings-URL** is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Zscaler persoonlijke toegang (ZPA) Client-ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **federatieve metagegevens-XML** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de **van Zscaler persoonlijke toegang (ZPA) ingesteld** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-zscaler-private-access-zpa"></a>Zscaler persoonlijke toegang (ZPA) configureren

1. Voor het automatiseren van de configuratie in Zscaler persoonlijke toegang (ZPA), die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Zscaler persoonlijke toegang (ZPA)** wordt u doorgeleid naar de toepassing Zscaler persoonlijke toegang (ZPA). Van daaruit, geef de beheerdersreferenties aan te melden bij Zscaler persoonlijke toegang (ZPA). De browserextensie wordt automatisch configureren van de toepassing voor u en stap 3-6 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Zscaler persoonlijke toegang (ZPA) handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf Zscaler persoonlijke toegang (ZPA) als beheerder en voer de volgende stappen uit:

4. Aan de linkerkant van het menu, klikt u op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheer van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-administration.png)

5. Klik in de rechterbovenhoek, **IdP-configuratie toevoegen**. 

    ![Zscaler particuliere Access-beheerder-id-provider](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-idp.png)

6. Op de **IdP-configuratie toevoegen** pagina de volgende stappen uitvoeren:
 
    ![Selecteer Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-select.png)

    a. Klik op **bestand selecteren** voor het uploaden van het gedownloade bestand met metagegevens van Azure AD in de **IdP metagegevens bestand uploaden** veld.

    b. Is het ingesteld op de **IdP metagegevens** van Azure AD en alle informatie van de velden gevuld, zoals hieronder wordt weergegeven.

    ![Configuratie van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccess-tutorial/config.png)

    c. Selecteer uw domein uit **domeinen** veld.
    
    d. Klik op **Opslaan**.

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler persoonlijke toegang (ZPA).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Zscaler persoonlijke toegang (ZPA)** .
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-zscaler-private-access-zpa-test-user"></a>Testgebruiker Zscaler persoonlijke toegang (ZPA) maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in Zscaler persoonlijke toegang (ZPA). Neem contact op met [Zscaler persoonlijke toegang (ZPA)-ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om toe te voegen de gebruikers in het platform Zscaler persoonlijke toegang (ZPA).

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Zscaler persoonlijke toegang (ZPA) in het toegangsvenster selecteert, moet u worden automatisch aangemeld op de Zscaler persoonlijke toegang (ZPA) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)