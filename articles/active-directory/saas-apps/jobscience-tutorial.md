---
title: 'Zelfstudie: Azure Active Directory-integratie met Jobscience | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jobscience.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8199f106c234e216a0982dc9e51413ccf30ae93a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199524"
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Zelfstudie: Azure Active Directory-integratie met Jobscience

In deze zelfstudie leert u hoe u Jobscience integreren met Azure Active Directory (Azure AD).

Jobscience integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Jobscience heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Jobscience (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Jobscience, moet u de volgende items:

- Een Azure AD-abonnement
- Een Jobscience eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u hier een gratis proefversie van één maand krijgen: [Proefversie](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jobscience uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jobscience-from-the-gallery"></a>Jobscience uit de galerie toe te voegen
Voor het configureren van de integratie van Jobscience in Azure AD, moet u Jobscience uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jobscience uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Jobscience**.

    ![Het maken van een Azure AD-testgebruiker](./media/jobscience-tutorial/tutorial_jobscience_search.png)

1. Selecteer in het deelvenster resultaten **Jobscience**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Jobscience op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Jobscience is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Jobscience tot stand worden gebracht.

In Jobscience, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Jobscience, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Jobscience](#creating-a-jobscience-test-user)**  : als u wilt een equivalent van Britta Simon in Jobscience die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Jobscience.

**Voor het configureren van Azure AD eenmalige aanmelding met Jobscience, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Jobscience** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/jobscience-tutorial/tutorial_jobscience_samlbase.png)

1. Op de **Jobscience domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/jobscience-tutorial/tutorial_jobscience_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:  `http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Deze waarde is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Deze waarde krijgen door [Jobscience Client ondersteuningsteam](https://www.jobscience.com/support) of van het profiel voor eenmalige aanmelding wordt u maken die later in de zelfstudie wordt uitgelegd. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/jobscience-tutorial/tutorial_jobscience_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/jobscience-tutorial/tutorial_general_400.png)

1. Op de **Jobscience configuratie** sectie, klikt u op **configureren Jobscience** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/jobscience-tutorial/tutorial_jobscience_configure.png) 

1. Meld u aan bij uw bedrijf Jobscience site aan als beheerder.

1. Ga naar **Setup**.
   
   ![Setup](./media/jobscience-tutorial/IC784358.png "Setup")

1. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein** openen de **Mijn domein** pagina. 
   
   ![My Domain](./media/jobscience-tutorial/ic767825.png "My Domain")

1. Om te bevestigen dat uw domein correct is ingesteld, zorg ervoor dat deze zich op "**stap 4 geïmplementeerd naar gebruikers**' en bekijk uw"**mijn domeininstellingen**'.

    ![Domein geïmplementeerd voor gebruiker](./media/jobscience-tutorial/ic784377.png "domein geïmplementeerd voor gebruiker")

1. Klik op de site van het bedrijf Jobscience **beveiligingsmechanismen**, en klik vervolgens op **instellingen voor eenmalige aanmelding**.
    
    ![Beveiligingscontroles](./media/jobscience-tutorial/ic784364.png "beveiligingscontroles")

1. In de **instellingen voor eenmalige aanmelding** sectie, voert u de volgende stappen uit:
    
    ![Single Sign-On instellingen](./media/jobscience-tutorial/ic781026.png "Single Sign-On-instellingen")
    
    a. Selecteer **SAML ingeschakeld**.

    b. Klik op **Nieuw**.

1. Op de **SAML Single Sign-On instelling bewerken** dialoogvenster, voer de volgende stappen uit:
    
    ![Eenmalige SAML-aanmelding instelling](./media/jobscience-tutorial/ic784365.png "eenmalige SAML-aanmelding instellen")
    
    a. Typ in het tekstvak **Name** een naam voor de configuratie.

    b. In **verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **entiteit-Id** tekstvak, type `https://salesforce-jobscience.com`

    d. Klik op **Bladeren** om uw Azure AD-certificaat te uploaden.

    e. Als **SAML identiteitstype**, selecteer **verklaring bevat de Federation-ID van het gebruikersobject**.

    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentfier van het onderwerp overzicht**.

    g. In **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    h. In **afmeldings-URL van id-Provider** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    i. Klik op **Opslaan**.

1. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein** openen de **Mijn domein** pagina. 
    
    ![My Domain](./media/jobscience-tutorial/ic767825.png "My Domain")

1. Op de **mijn domein** pagina, in de **aanmelden pagina huisstijl** sectie, klikt u op **bewerken**.
    
    ![Login Page Branding](./media/jobscience-tutorial/ic767826.png "Login Page Branding")

1. Op de **aanmelden pagina huisstijl** pagina, in de **verificatieservice** sectie, de naam van uw **SAML SSO-instellingen** wordt weergegeven. Selecteer deze en klik vervolgens op **opslaan**.
    
    ![Login Page Branding](./media/jobscience-tutorial/ic784366.png "Login Page Branding")

1. Gestart om op te halen van de Serviceprovider eenmalige aanmelding bij klikken op de aanmeldings-URL op de **instellingen voor eenmalige aanmelding** in de **beveiligingsmechanismen** sectie menu.

    ![Beveiligingscontroles](./media/jobscience-tutorial/ic784368.png "beveiligingscontroles")
    
    Klik op de SSO-profiel dat u hebt gemaakt in de bovenstaande stap. Deze pagina bevat de eenmalige aanmelding op URL voor uw bedrijf (bijvoorbeeld [ https://companyname.my.salesforce.com?so=companyid ](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/jobscience-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/jobscience-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/jobscience-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/jobscience-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-jobscience-test-user"></a>Het maken van een testgebruiker Jobscience

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Jobscience, moeten ze worden ingericht voor Jobscience. In het geval van Jobscience is inrichten een handmatige taak.

>[!NOTE]
>U kunt elke andere Jobscience gebruiker account hulpmiddelen voor het maken of API's geleverd door Jobscience voor het inrichten van Azure Active Directory-gebruikersaccounts.
>  
        
**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw **Jobscience** bedrijf site als administrator.

1. Ga naar instellingen.
   
   ![Setup](./media/jobscience-tutorial/ic784358.png "Setup")
1. Ga naar **gebruikers beheren \> gebruikers**.
   
   ![Gebruikers](./media/jobscience-tutorial/ic784369.png "Gebruikers")
1. Klik op **New User**.
   
   ![Alle gebruikers](./media/jobscience-tutorial/ic784370.png "alle gebruikers")
1. Op de **gebruiker bewerken** dialoogvenster, voer de volgende stappen uit:
   
   ![Gebruiker bewerken](./media/jobscience-tutorial/ic784371.png "gebruiker bewerken")
   
   a. In de **voornaam** tekstvak, typ een naam van de gebruiker, zoals Julia.
   
   b. In de **achternaam** tekstvak typt u een achternaam van de gebruiker, zoals Simon.
   
   c. In de **Alias** tekstvak typt u de aliasnaam van de gebruiker, zoals brittas.

   d. Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

   e. In de **gebruikersnaam** tekstvak, typ een naam van gebruiker, zoals Brittasimon@contoso.com.

   f. In de **bijnaam** tekstvak typt u een bijnaam van gebruiker, zoals Simon.

   g. Klik op **Opslaan**.

    
> [!NOTE]
> De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Jobscience.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Jobscience toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Jobscience**.

    ![Eenmalige aanmelding configureren](./media/jobscience-tutorial/tutorial_jobscience_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Jobscience in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Jobscience.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/jobscience-tutorial/tutorial_general_01.png
[2]: ./media/jobscience-tutorial/tutorial_general_02.png
[3]: ./media/jobscience-tutorial/tutorial_general_03.png
[4]: ./media/jobscience-tutorial/tutorial_general_04.png

[100]: ./media/jobscience-tutorial/tutorial_general_100.png

[200]: ./media/jobscience-tutorial/tutorial_general_200.png
[201]: ./media/jobscience-tutorial/tutorial_general_201.png
[202]: ./media/jobscience-tutorial/tutorial_general_202.png
[203]: ./media/jobscience-tutorial/tutorial_general_203.png

