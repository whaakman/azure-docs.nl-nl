---
title: 'Zelfstudie: Azure Active Directory-integratie met Projectplace | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Projectplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/29/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 179721075484c35c5ebbb3d936b83bc407b75a8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67093526"
---
# <a name="tutorial-integrate-projectplace-with-azure-active-directory"></a>Zelfstudie: Projectplace integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Projectplace integreert met Azure Active Directory (Azure AD). Wanneer u een Projectplace met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Projectplace heeft.
* Kunnen uw gebruikers worden automatisch aangemeld met Projectplace met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.
* Gebruikers kunnen automatisch worden ingericht in Projectplace.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Projectplace eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Projectplace **SP en IDP** geïnitieerde eenmalige aanmelding en ondersteunt **Just In Time** inrichten van gebruikers.

## <a name="adding-projectplace-from-the-gallery"></a>Projectplace uit de galerie toe te voegen

Voor het configureren van de integratie van Projectplace in Azure AD, moet u Projectplace uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Projectplace** in het zoekvak in.
1. Selecteer **Projectplace** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Projectplace met behulp van een testgebruiker met de naam **B. Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Projectplace vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Projectplace, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Projectplace](#configure-projectplace)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B. Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B. Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Projectplace-testgebruiker maken](#create-projectplace-test-user)**  hebben een equivalent van B. Simon in Projectplace die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Projectplace** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus, de toepassing is vooraf geconfigureerd en de vereiste URL's al vooraf zijn ingevuld met Azure . De gebruiker moet de configuratie op te slaan door te klikken op de **opslaan** knop.

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://service.projectplace.com`

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op kopiëren **pictogram** kopiëren de **App-Url voor federatieve metagegevens** , aan de hand van uw behoeften en bewaar deze in Kladblok.

   ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

1. Op de **instellen van Projectplace** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-projectplace"></a>Projectplace configureren

Het configureren van eenmalige aanmelding op de **Projectplace** zijde, moet u voor het verzenden van de gekopieerde **App-Url voor federatieve metagegevens** vanuit Azure portal naar de [Projectplace-ondersteuningsteam](https://success.planview.com/Projectplace/Support) . Dit team zorgt ervoor dat de SAML SSO-verbinding aan beide zijden juist is ingesteld.

>[!NOTE]
>De configuratie voor eenmalige aanmelding moet worden uitgevoerd door de [Projectplace-ondersteuningsteam](https://success.planview.com/Projectplace/Support). U ontvangt een melding zodra de configuratie voltooid is. 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B. Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B. Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon Azure eenmalige aanmelding door toegang te verlenen aan Projectplace gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Projectplace**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B. Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-projectplace-test-user"></a>Projectplace-testgebruiker maken

>[!NOTE]
>U kunt deze stap overslaan als u ingeschakeld in Projectplace wordt ingericht hebt. U kunt stellen de [Projectplace-ondersteuningsteam](https://success.planview.com/Projectplace/Support) rapportcentrumsite, inschakelen als gereed gebruikers worden aangemaakt in Projectplace tijdens de eerste aanmelding.

Om Azure AD-gebruikers zich aanmelden bij Projectplace, moet u ze toevoegen aan Projectplace. U moet deze handmatig toevoegen.

**Voor het maken van een gebruikersaccount, de volgende stappen uitvoeren:**

1. Aanmelden bij uw **Projectplace** bedrijf site als een beheerder.

2. Ga naar **mensen**, en selecteer vervolgens **leden**:
   
    ![Ga naar personen, en selecteer vervolgens leden](./media/projectplace-tutorial/ic790228.png "personen")

3. Selecteer **lid toevoegen**:
   
    ![Selecteer een lid toevoegen](./media/projectplace-tutorial/ic790232.png "leden toevoegen")

4. In de **Add Member** sectie, de volgende stappen uitvoeren.
   
    ![Toevoegen van lid sectie](./media/projectplace-tutorial/ic790233.png "nieuwe leden")
   
    1. In de **nieuwe leden** voert u het e-mailadres van een geldige Azure AD-account dat u wilt toevoegen.
   
    1. Selecteer **Verzenden**.

   Een e-mailbericht met een koppeling om te bevestigen dat het account voordat deze actief is verzonden naar de houder van Azure AD-account.

>[!NOTE]
>U kunt ook een ander gebruikersaccount maken-hulpprogramma gebruiken of API wordt geleverd door Projectplace Azure AD-gebruikersaccounts wilt toevoegen.


### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de Projectplace-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Projectplace waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)