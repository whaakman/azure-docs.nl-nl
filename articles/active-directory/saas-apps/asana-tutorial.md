---
title: 'Zelfstudie: Azure Active Directory-integratie met Asana | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Asana.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.openlocfilehash: f5ea7a330891d4befeb6388bbe7f37b2a4aa848f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438205"
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Zelfstudie: Azure Active Directory-integratie met Asana

In deze zelfstudie leert u hoe u Asana integreren met Azure Active Directory (Azure AD).

Asana integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Asana heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Asana (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Asana, moet u de volgende items:

- Een Azure AD-abonnement
- Een Asana eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Asana uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-asana-from-the-gallery"></a>Asana uit de galerie toe te voegen
Voor het configureren van de integratie van Asana in Azure AD, moet u Asana uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Asana uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Asana**, selecteer **Asana** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Asana op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Asana is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Asana tot stand worden gebracht.

In Asana, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Asana, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Asana](#create-an-asana-test-user)**  : als u wilt een equivalent van Britta Simon in Asana die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Asana.

**Voor het configureren van Azure AD eenmalige aanmelding met Asana, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Asana** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/asana-tutorial/tutorial_asana_samlbase.png)

1. Op de **Asana-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Asana-domein en URL's, eenmalige aanmelding informatie](./media/asana-tutorial/tutorial_asana_url.png)

    a. In de **aanmeldings-URL** tekstvak, typ de URL: `https://app.asana.com/`

    b. In de **id** tekstvak, waarde: `https://app.asana.com/`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/asana-tutorial/tutorial_asana_certificate.png)

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/asana-tutorial/tutorial_general_400.png)

1. Op de **Asana configuratie** sectie, klikt u op **configureren Asana** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Asana-configuratie](./media/asana-tutorial/tutorial_asana_configure.png)

1. In een ander browservenster aanmelden voor uw toepassing Asana. Voor het configureren van eenmalige aanmelding in Asana, toegang krijgen tot instellingen van de werkruimte door te klikken op de naam van de werkruimte in de rechterbovenhoek van het scherm. Klik vervolgens op  **\<naam van uw werkruimte\> instellingen**.

    ![Asana sso-instellingen](./media/asana-tutorial/tutorial_asana_09.png)

1. Op de **organisatie-instellingen** venster, klikt u op **beheer**. Klik vervolgens op **leden moeten zich aanmelden via SAML** om in te schakelen van de SSO-configuratie. Het uitvoeren van de volgende stappen uit:

    ![Single Sign-On, organisatie-instellingen configureren](./media/asana-tutorial/tutorial_asana_10.png)  

     a. In de **aanmelden pagina-URL** tekstvak, plak de **Single Sign-On Service URL voor SAML**.

     b. Klik met de rechtermuisknop op het certificaat dat is gedownload vanuit Azure portal en open vervolgens het certificaatbestand in Kladblok of uw favoriete teksteditor. Kopieer de inhoud tussen de begin- en de titel van de end-certificaat en plak deze in de **X.509-certificaat** tekstvak.

1. Klik op **Opslaan**. Ga naar [Asana-handleiding voor het instellen van eenmalige aanmelding](https://asana.com/guide/help/premium/authentication#gl-saml) als u meer hulp nodig hebt.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/asana-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/asana-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/asana-tutorial/create_aaduser_03.png)

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![De knop toevoegen](./media/asana-tutorial/create_aaduser_04.png)

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="create-an-asana-test-user"></a>Maak een testgebruiker Asana

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Asana. Asana ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](asana-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, kunt voert u de volgende stappen:**

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Asana.

1. Op **Asana**, gaat u naar de **Teams** sectie in het linkerdeelvenster. Klik op de knop plusteken (+).

    ![Het maken van een Azure AD-testgebruiker](./media/asana-tutorial/tutorial_asana_12.png)

1. Typ het e-mailbericht britta.simon@contoso.com in het tekstvak in en selecteer vervolgens **uitnodigen**.

1. Klik op **uitnodiging verzenden**. De nieuwe gebruiker ontvangt een e-mailbericht in haar e-mailaccount. Ze moet maken en valideren van het account.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Asana.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Asana toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **Asana**.

    ![De Asana-koppeling in de lijst met toepassingen](./media/asana-tutorial/tutorial_asana_app.png)

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is voor het testen van uw Azure AD eenmalige aanmelding.

Ga naar de aanmeldingspagina Asana. Voeg in het tekstvak e-mailadres het e-mailadres britta.simon@contoso.com. Laat het tekstvak voor het wachtwoord in leeg en klik vervolgens op **aanmelden**. U wordt omgeleid naar Azure AD-aanmeldingspagina. Voer uw Azure AD-referenties. Nu bent u aangemeld Asana.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](asana-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/asana-tutorial/tutorial_general_01.png
[2]: ./media/asana-tutorial/tutorial_general_02.png
[3]: ./media/asana-tutorial/tutorial_general_03.png
[4]: ./media/asana-tutorial/tutorial_general_04.png

[100]: ./media/asana-tutorial/tutorial_general_100.png

[200]: ./media/asana-tutorial/tutorial_general_200.png
[201]: ./media/asana-tutorial/tutorial_general_201.png
[202]: ./media/asana-tutorial/tutorial_general_202.png
[203]: ./media/asana-tutorial/tutorial_general_203.png
[10]: ./media/asana-tutorial/tutorial_general_060.png
[11]: ./media/asana-tutorial/tutorial_general_070.png