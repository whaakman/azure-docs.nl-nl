---
title: 'Zelfstudie: Azure Active Directory-integratie met TimeOffManager | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TimeOffManager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: e7f0f6bb778dedeea61b74b5ca0c2edbadd5279b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430284"
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Zelfstudie: Azure Active Directory-integratie met TimeOffManager

In deze zelfstudie leert u hoe u TimeOffManager integreren met Azure Active Directory (Azure AD).

TimeOffManager integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TimeOffManager heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TimeOffManager (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TimeOffManager, moet u de volgende items:

- Een Azure AD-abonnement
- Een TimeOffManager eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TimeOffManager uit de galerie toevoegen
1. Configureren en Azure AD eenmalige aanmelding testen

## <a name="add-timeoffmanager-from-the-gallery"></a>TimeOffManager uit de galerie toevoegen
Voor het configureren van de integratie van TimeOffManager in Azure AD, moet u TimeOffManager uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TimeOffManager uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **TimeOffManager**, selecteer **TimeOffManager** van resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met TimeOffManager op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TimeOffManager is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TimeOffManager tot stand worden gebracht.

In TimeOffManager, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TimeOffManager, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker TimeOffManager](#create-a-timeoffmanager-test-user)**  : als u wilt een equivalent van Britta Simon in TimeOffManager die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TimeOffManager.

**Voor het configureren van Azure AD eenmalige aanmelding met TimeOffManager, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TimeOffManager** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![SAML-aanmelding](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

1. Op de **TimeOffManager domein en URL's** sectie, voer de volgende stappen uit:

     ![Sectie TimeOffManager domein en URL 's](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Krijgt u voor deze waarde **instellingenpagina voor eenmalige aanmelding** die wordt verderop in de zelfstudie of neem contact op met [TimeOffManager ondersteuningsteam](https://www.purelyhr.com/contact-us).
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Sectie voor SAML-handtekeningcertificaat](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

1. Het doel van deze sectie is om een overzicht van hoe u gebruikers wilt verifiëren naar TimeOffManger met hun account in Azure AD gebruikmaakt van Federatie op basis van het SAML-protocol te.
    
    Uw toepassing TimeOffManger wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![SAML-token kenmerken](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml-token kenmerken")
    
    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |
    | Voornaam |User.givenName |
    | Achternaam |User.surname |
    | Email |User.mail |
    
    a.  Voor elke gegevensrij in de bovenstaande tabel, klikt u op **toevoegen gebruikerskenmerk**.
    
    ![SAML-token kenmerken](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml-token kenmerken")
    
    ![SAML-token kenmerken](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml-token kenmerken")
    
    b.  In de **kenmerknaam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c.  In de **kenmerkwaarde** textbox, selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d.  Klik op **OK**.
    
1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/timeoffmanager-tutorial/tutorial_general_400.png)

1. Op de **TimeOffManager configuratie** sectie, klikt u op **configureren TimeOffManager** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratiesectie TimeOffManager](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf TimeOffManager site als beheerder.

1. Ga naar **Account \> accountopties \> Single Sign-On instellingen**.
   
   ![Single Sign-On instellingen](./media/timeoffmanager-tutorial/ic795917.png "Single Sign-On-instellingen")
1. In de **instellingen voor eenmalige aanmelding** sectie, voert u de volgende stappen uit:
   
   ![Single Sign-On instellingen](./media/timeoffmanager-tutorial/ic795918.png "Single Sign-On-instellingen")
   
   a. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u het gehele certificaat in **X.509-certificaat** tekstvak.
   
   b. In **Idp verlener** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
   
   c. In **IdP eindpunt-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
   
   d. Als **afdwingen SAML**, selecteer **Nee**.
   
   e. Als **gebruikers automatisch maakt**, selecteer **Ja**.
   
   f. In **afmeldings-URL van** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
   
   g. Klik op **wijzigingen opslaan**.

1. In **instellingen voor eenmalige aanmelding** pagina-, Kopieer de waarde van **URL van de Bevestigingsconsumerservice** en plak deze in de **antwoord-URL** tekstvak onder **TimeOffManager Domein en URL's** sectie in Azure portal. 

      ![Single Sign-On instellingen](./media/timeoffmanager-tutorial/ic795915.png "Single Sign-On-instellingen")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/timeoffmanager-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Gebruikers en groepen--> alle gebruikers](./media/timeoffmanager-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Knop toevoegen](./media/timeoffmanager-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Dialoogvenster op de gebruikerspagina](./media/timeoffmanager-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Maak een testgebruiker TimeOffManager

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij TimeOffManager, moeten ze worden ingericht op TimeOffManager.  

TimeOffManager biedt ondersteuning voor just-in-tijd van gebruikersinrichting. Er is geen actie-item voor u.  

De gebruikers worden automatisch toegevoegd tijdens de eerste keer aanmelden met behulp van eenmalige aanmelding op.

>[!NOTE]
>U kunt alle andere TimeOffManager gebruiker-account maken van hulpprogramma's of API's geleverd door TimeOffManager voor het inrichten van gebruikersaccounts van de Azure AD.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TimeOffManager.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan TimeOffManager toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **TimeOffManager**.

    ![TimeOffManager in lijst met Apps](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TimeOffManager in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TimeOffManager. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/timeoffmanager-tutorial/tutorial_general_203.png

