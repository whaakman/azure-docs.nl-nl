---
title: 'Zelfstudie: Azure Active Directory-integratie met Dropbox voor Bedrijven | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en Dropbox voor Bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f2109c3f67a666d0c379cc188c07968c8b31d6a1
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989679"
---
# <a name="tutorial-integrate-dropbox-for-business-with-azure-active-directory"></a>Zelfstudie: Dropbox voor bedrijven integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Dropbox voor bedrijven integreren met Azure Active Directory (Azure AD). Wanneer u Dropbox voor bedrijven met Azure AD integreren, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Dropbox voor bedrijven heeft.
* Kunnen uw gebruikers worden automatisch aangemeld met Dropbox voor bedrijven met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Dropbox voor bedrijven eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

* In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Dropbox voor Bedrijven ondersteunt door **SP** geïnitieerde eenmalige aanmelding

* Dropbox voor Bedrijven ondersteunt het **Just In Time** inrichten van gebruikers

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Dropbox voor Bedrijven toevoegen vanuit de galerie

Voor het configureren van de integratie van Dropbox voor Bedrijven in Azure AD moet u Dropbox voor Bedrijven uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Dropbox voor bedrijven** in het zoekvak in.
1. Selecteer **Dropbox voor bedrijven** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Dropbox voor bedrijven met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Dropbox voor bedrijven vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Dropbox voor bedrijven, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Dropbox configureren voor eenmalige aanmelding voor zakelijke](#configure-dropbox-for-business-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken in Dropbox voor Bedrijven](#create-dropbox-for-business-test-user)**: om in Dropbox voor Bedrijven een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Dropbox voor bedrijven** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.dropbox.com/sso/<id>`

    b. Typ een waarde in het vak **Id (Entiteits-id)**: `Dropbox`

    > [!NOTE]
    > De bovenstaande waarde voor de aanmeldings-URL is geen echte waarde. U werkt de waarde bij met de werkelijke aanmeldings-URL, zoals later in de zelfstudie wordt uitgelegd.

1. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Kopieer onder **Dropbox voor Bedrijven instellen** de URL('s) die u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-dropbox-for-business-sso"></a>Dropbox voor bedrijven eenmalige aanmelding configureren

1. Voor het automatiseren van de configuratie in Dropbox voor bedrijven, moet u installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Dropbox voor bedrijven** wordt u doorgeleid naar Dropbox voor het Business-toepassing. Van daaruit, geef de referenties van de beheerder zich aanmelden bij Dropbox voor bedrijven. De browserextensie wordt automatisch configureren van de toepassing voor u en automatiseren van stap 3-8.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Dropbox voor bedrijven handmatig instellen wilt, opent u een nieuw browservenster en gaat u in uw Dropbox voor bedrijven-tenant en meld u aan bij in uw Dropbox voor bedrijven-tenant. en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/ic769509.png "Eenmalige aanmelding configureren")

4. Klik op het **Gebruikerspictogram** en selecteer het tabblad **Instellingen**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure1.png "Eenmalige aanmelding configureren")

5. Klik in het navigatievenster aan de linkerkant op **Beheerconsole**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure2.png "Eenmalige aanmelding configureren")

6. Klik in de **Beheerconsole** op **Instellingen** in het navigatievenster links.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure3.png "Eenmalige aanmelding configureren")

7. Selecteer de optie **Eenmalige aanmelding** in het gedeelte **Verificatie**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure4.png "Eenmalige aanmelding configureren")

8. Voer in het gedeelte **Eenmalige aanmelding** de volgende stappen uit:  

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure5.png "Eenmalige aanmelding configureren")

    a. Selecteer **vereist** als een optie in de vervolgkeuzelijst voor de **eenmalige aanmelding**.

    b. Klik op **Aanmeldings-URL toevoegen**, plak in het tekstvak **Aanmeldings-URL identiteitsprovider** de **Aanmeldings-URL** die u hebt gekopieerd van de Azure-portal en selecteer **Gereed**.

    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/configure6.png "Eenmalige aanmelding configureren")

    c. Klik op **Certificaat uploaden** en blader naar het **Base64-gecodeerde certificaatbestand** dat u hebt gedownload uit de Azure-portal.

    d. Klik op **Koppeling kopiëren** en plak de gekopieerde waarde in het tekstvak **Aanmeldings-URL** van het gedeelte **Domein en URL’s voor Dropbox voor Bedrijven** in Azure Portal.

    e. Klik op **Opslaan**.

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Dropbox voor bedrijven.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Dropbox voor bedrijven**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-dropbox-for-business-test-user"></a>Een testgebruiker maken in Dropbox voor Bedrijven

In dit gedeelte wordt een gebruiker met de naam Britta Simon gemaakt in Dropbox voor Bedrijven. Dropbox voor Bedrijven ondersteunt het Just-In-Time inrichten van gebruikers, wat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Dropbox voor Bedrijven bestaat, wordt er een nieuwe gemaakt na verificatie.

>[!Note]
>Als u handmatig een gebruiker moet maken, neemt u contact op met [het ondersteuningsteam van Dropbox voor Bedrijven](https://www.dropbox.com/business/contact)

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de Dropbox voor bedrijven-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Dropbox voor bedrijven waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)