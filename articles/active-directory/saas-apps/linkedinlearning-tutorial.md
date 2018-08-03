---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn Learning | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en LinkedIn Learning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 02e7d9d26b389e82365f3447cceb5566244236f1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440905"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn Learning

In deze zelfstudie leert u hoe u LinkedIn Learning integreren met Azure Active Directory (Azure AD).

LinkedIn Learning integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LinkedIn Learning heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LinkedIn Learning (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn Learning, moet u de volgende items:

- Een Azure AD-abonnement
- Een LinkedIn Learning eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LinkedIn Learning uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-linkedin-learning-from-the-gallery"></a>LinkedIn Learning uit de galerie toe te voegen
Voor het configureren van de integratie van LinkedIn Learning in Azure AD, moet u LinkedIn Learning uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LinkedIn Learning uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **LinkedIn Learning**. Klik in het deelvenster met resultaten, op **LinkedIn Learning** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LinkedIn Learning op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in LinkedIn Learning in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LinkedIn Learning tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in LinkedIn Learning.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met LinkedIn Learning, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing LinkedIn Learning.

**Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn Learning, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LinkedIn Learning** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial-linkedin_01.png)

1. In een ander browservenster aanmelden voor uw tenant LinkedIn Learning als beheerder.

1. In **Accountcentrum**, klikt u op **globale instellingen** onder **instellingen**. Schakel ook **Learning - standaard** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

1. Klik op **of Klik hier om te laden en kopiëren van afzonderlijke velden van het formulier** en kopieert u **entiteit-Id** en **Assertion Consumer Access (ACS)-Url**

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

1. In Azure portal onder **LinkedIn Learning domein en URL's**, voer de volgende stappen uit als u wilt configureren van eenmalige aanmelding in **IdP gestart door** modus

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. In de **id** tekstvak, voer de **entiteit-ID** opgehaald uit de LinkedIn-Portal 

    b. In de **antwoord-URL** tekstvak, voer de **Assertion Consumer Access (ACS) Url** opgehaald uit de LinkedIn-Portal

1. Als u wilt configureren van eenmalige aanmelding in **SP geïnitieerde**, klikt u op geavanceerde URL weergeven instelling-optie in het gedeelte over configuratie en de aanmeldings-URL configureren met het volgende patroon:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
1. Uw toepassing LinkedIn Learning wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar LinkedIn Learning wordt verwacht dat deze optie om te worden toegewezen met de e-mailadres van de gebruiker. Hiervoor kunt u **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw. 

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/updateusermail.png)
    
1. In **gebruikerskenmerken** sectie, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** en de kenmerken instellen. De gebruiker nodig heeft om toe te voegen vier claims met de naam **e**, **afdeling**, **firstname**, en **lastname** en de waarde moet worden toegewezen met **user.mail**, **user.department**, **user.givenname**, en **user.surname** respectievelijk

    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |
    | e-mailen| User.mail |    
    | Afdeling| User.Department |
    | Voornaam| User.givenName |
    | Achternaam| User.surname |
    
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/userattribute.png)
    
    a. Klik op **kenmerk toevoegen** om het dialoogvenster kenmerk te openen.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/tutorial_attribute_04.png)

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

1. Voer de volgende stappen uit op de **naam** kenmerk -

    a. Klik op het kenmerk te openen de **kenmerk bewerken** venster.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/url_update.png)

    b. Verwijderen van de URL-waarde uit de **naamruimte**.
    
    c. Klik op **Ok** om op te slaan van de instelling.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png)

1. Klik op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_general_400.png)

1. Ga naar **LinkedIn beheerdersinstellingen** sectie. Upload het XML-bestand dat u hebt gedownload van de Azure-portal door te klikken op de optie uploaden XML-bestand.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

1. Klik op **op** SSO inschakelen. SSO-status verandert van **niet verbonden** naar **verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-linkedin-learning-test-user"></a>Het maken van een testgebruiker LinkedIn Learning

LinkedIn Learning-toepassing biedt ondersteuning voor Just in tijd gebruikersinrichting en na-verificatiegebruikers automatisch in de toepassing gemaakt worden. Op de beheerder van het tabblad instellingen op de portal spiegelen LinkedIn Learning de switch **automatisch toewijzen van licenties** op actieve Just-in-time inrichting en dit wordt ook een licentie toewijzen aan de gebruiker.

   ![Het maken van een Azure AD-testgebruiker](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot LinkedIn Learning.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon LinkedIn Learning, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **LinkedIn Learning**.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png)

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de tegel LinkedIn Learning in het toegangsvenster, krijgt u de pagina Azure Sign-on en op na geslaagde aanmelding, krijgt u rechtstreeks in uw toepassing LinkedIn Learning.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/linkedinlearning-tutorial/tutorial_general_203.png
