---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn verhogen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en LinkedIn met verhoogde bevoegdheden.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 8b11b5e3e420577590e95c6839673f54c52d078b
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116173"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn met verhoogde bevoegdheden uitvoeren

In deze zelfstudie leert u hoe u met verhoogde bevoegdheden uitvoeren LinkedIn integreren met Azure Active Directory (Azure AD).

LinkedIn met verhoogde bevoegdheden te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LinkedIn met verhoogde bevoegdheden uitvoeren heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LinkedIn met verhoogde bevoegdheden (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn met verhoogde bevoegdheden, moet u de volgende items:

- Een Azure AD-abonnement
- Een LinkedIn met verhoogde bevoegdheden eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van LinkedIn met verhoogde bevoegdheden uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Toevoegen van LinkedIn met verhoogde bevoegdheden uit de galerie
Voor het configureren van de integratie van LinkedIn met verhoogde bevoegdheden in Azure AD, moet u LinkedIn met verhoogde bevoegdheden uit de galerie aan de lijst met beheerde SaaS-apps toevoegt.

**Als u wilt toevoegen LinkedIn met verhoogde bevoegdheden uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **LinkedIn met verhoogde bevoegdheden**. Klik in het deelvenster met resultaten, op **LinkedIn met verhoogde bevoegdheden** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LinkedIn met verhoogde bevoegdheden op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in LinkedIn met verhoogde bevoegdheden is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LinkedIn met verhoogde bevoegdheden tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in LinkedIn met verhoogde bevoegdheden.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met LinkedIn met verhoogde bevoegdheden, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker LinkedIn met verhoogde bevoegdheden](#creating-a-linkedin-elevate-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure Management portal inschakelen en configureren van eenmalige aanmelding in uw toepassing LinkedIn met verhoogde bevoegdheden.

**Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn met verhoogde bevoegdheden, kunt u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **LinkedIn met verhoogde bevoegdheden** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial-linkedin_01.png)

1. In een ander browservenster aanmelden voor uw tenant LinkedIn met verhoogde bevoegdheden als beheerder.

1. In **Accountcentrum**, klikt u op **globale instellingen** onder **instellingen**. Schakel ook **toestaan - AAD-Test met verhoogde bevoegdheden** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Klik op **of Klik hier om te laden en kopiëren van afzonderlijke velden van het formulier** en kopieert u **entiteit-Id** en **Assertion Consumer Access (ACS)-Url**

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

1. In Azure Portal onder **LinkedIn met verhoogde bevoegdheden domein en URL's**, voer de volgende stappen uit als u wilt configureren van eenmalige aanmelding in **IdP gestart door** modus

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_signon_01.png)

    a. In de **id** tekstvak, voer de **entiteit-ID** opgehaald uit de LinkedIn-Portal 

    b. In de **antwoord-URL** tekstvak, voer de **Assertion Consumer Access (ACS) Url** opgehaald uit de LinkedIn-Portal

1. Als u wilt configureren van eenmalige aanmelding in **SP geïnitieerde**, klikt u op geavanceerde URL weergeven instelling-optie in het gedeelte over configuratie en het teken op URL configureren met het volgende patroon:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_signon_02.png) 

1. Uw toepassing LinkedIn met verhoogde bevoegdheden wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** , maar dit moet worden toegewezen met de e-mailadres van de gebruiker LinkedIn met verhoogde bevoegdheden wordt verwacht. Hiervoor kunt u **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/updateusermail.png)

1. In **gebruikerskenmerken** sectie, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** en de kenmerken instellen. U wilt toevoegen van een andere claim met de naam **afdeling** en de waarde moet worden toegewezen aan **user.department**.

    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |
    | Afdeling| User.Department |

      ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/userattribute.png)

      a. Klik op kenmerk toevoegen om te openen van de pagina met details van kenmerk toevoegen het kenmerk voor afdeling zoals hieronder:

      ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/adduserattribute.png)

      b. Klik op **Ok** om op te slaan van het kenmerk.

      c. Wijzig de naam van het kenmerk **emailaddress** naar **e**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_certificate.png) 

1. Klik op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_general_400.png)

1. Ga naar **LinkedIn beheerdersinstellingen** sectie. Upload het XML-bestand dat u zojuist hebt gedownload vanuit Azure portal door te klikken op de optie uploaden XML-bestand.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Klik op **op** SSO inschakelen. SSO-status wordt gewijzigd van **niet verbonden** naar **verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-linkedin-elevate-test-user"></a>Het maken van een testgebruiker LinkedIn met verhoogde bevoegdheden uitvoeren

LinkedIn met verhoogde bevoegdheden toepassing ondersteunt Just in time gebruikersinrichting en na-verificatiegebruikers, in de toepassing automatisch worden gemaakt. Op de beheerder van het tabblad instellingen op de portal spiegelen met LinkedIn met verhoogde bevoegdheden de switch **automatisch toewijzen van licenties** op actieve Just-in-time inrichting en dit wordt ook een licentie toewijzen aan de gebruiker. LinkedIn met verhoogde bevoegdheden biedt ook ondersteuning voor automatisch inrichten van gebruikers, vindt u meer details [hier](linkedinelevate-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

   ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot LinkedIn met verhoogde bevoegdheden.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar LinkedIn met verhoogde bevoegdheden, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **LinkedIn met verhoogde bevoegdheden**.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial-linkedinElevate_0001.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de tegel LinkedIn met verhoogde bevoegdheden in het toegangsvenster, krijgt u de pagina Azure Sign-on en op na geslaagde aanmelding, krijgt u rechtstreeks in uw toepassing LinkedIn met verhoogde bevoegdheden.

## <a name="additional-resources"></a>Aanvullende resources

* [Zelfstudie: LinkedIn, met verhoogde bevoegdheden configureren voor automatisch gebruikers inrichten met Azure Active Directory](linkedinelevate-provisioning-tutorial.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](linkedinelevate-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/linkedinElevate-tutorial/tutorial_general_203.png