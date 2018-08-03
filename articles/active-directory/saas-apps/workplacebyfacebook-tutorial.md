---
title: 'Zelfstudie: Azure Active Directory-integratie met werkplek door Facebook | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en werk met Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: jeedes
ms.openlocfilehash: 1f83dd64c7f6773ddb8956e6ebbc37b8c55aacec
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423868"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Zelfstudie: Azure Active Directory-integratie met werkplek op Facebook

In deze zelfstudie leert u hoe werkplek door Facebook integreren met Azure Active Directory (Azure AD).

Werkplek op Facebook te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de werkplek op Facebook heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij werkplek door Facebook (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkplek met Facebook, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkplek op Facebook eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Facebook heeft twee producten, werkplek-standaard (gratis) en werkplek Premium (betaald). Een werkplek Premium-tenant kunt SCIM en SSO-integratie configureren met geen andere implicaties kosten of licenties die zijn vereist. Eenmalige aanmelding en SCIM zijn niet beschikbaar in Standard werkplek exemplaren.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Werkplek door Facebook uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Werkplek door Facebook uit de galerie toe te voegen
Voor het configureren van de integratie van werkplek door Facebook in Azure AD, moet u werkplek door Facebook uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen werkplek door Facebook uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **werkplek door Facebook**.

    ![Het maken van een Azure AD-testgebruiker](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

1. Selecteer in het deelvenster resultaten **werkplek door Facebook**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met werkplek door Facebook op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de werkplek van Facebook is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de werkplek op Facebook tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in werkplek op Facebook.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met werkplek met Facebook, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Configureren van verificatie frequentie](#configuring-reauthentication-frequency)**  - werkplek te vragen om een SAML-controle configureren.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een werkplek op Facebook testgebruiker](#creating-a-workplace-by-facebook-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de werkplek op Facebook die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw werkplek met Facebook-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met werkplek met Facebook, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **werkplek door Facebook** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

1. Op de **werkplek op Facebook-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<instancename>.facebook.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Zie de pagina verificatie van het Dashboard van het bedrijf werkplek voor de juiste waarden voor uw werkplek door de community. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/workplacebyfacebook-tutorial/tutorial_general_400.png)

1. Op de **werkplek op Facebook-configuratie** sectie, klikt u op **werkplek configureren door Facebook** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/workplacebyfacebook-tutorial/config.png) 

1. In een ander browservenster, meld u aan bij uw werkplek door Facebook bedrijf site als beheerder.
  
   > [!NOTE] 
   > Als onderdeel van het SAML-verificatieproces, eventueel werkplek queryreeksen van de grootte van maximaal 2,5 kB gebruiken om de parameters kunt toevoegen aan Azure AD.

1. In de **bedrijf Dashboard**, gaat u naar de **verificatie** tabblad.

1. Onder **SAML-verificatie**, selecteer **SSO alleen** uit de vervolgkeuzelijst.

1. De waarden die zijn gekopieerd uit de invoer **werkplek op Facebook-configuratie** sectie van de Azure portal in de bijbehorende velden:

    *   In **URL voor SAML** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.
    *   In **tekstvak URL voor SAML-verlener**, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.
    *   In **omleiden van SAML-afmelding** (optioneel), plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.
    *   Open uw **base-64 gecodeerde certificaat** in Kladblok gedownload vanuit Azure portal, kopieert u de inhoud ervan naar het Klembord en plakt u deze naar de **SAML-certificaat** tekstvak.

1. U moet mogelijk de URL van de doelgroep, ontvanger URL invoeren en de URL van de ACS (Assertion Consumer Service) die worden vermeld onder de **SAML-configuratie** sectie.

1. Ga naar de onderkant van de sectie en klikt u op de **Test SSO** knop. Dit resulteert in een pop-upvenster weergegeven met Azure AD-aanmeldingspagina weergegeven. Voer uw referenties in als normale om te verifiëren. 

    **Probleemoplossing:** Zorg ervoor dat het e-mailadres dat wordt geretourneerd terug van Azure AD is hetzelfde als het werkplekaccount dat u bent aangemeld.

1. Zodra de test is voltooid, Ga naar de onderkant van de pagina en klik op de **opslaan** knop.

1. Alle gebruikers met behulp van werkplek wordt nu weergegeven met Azure AD-aanmeldingspagina voor verificatie.

1. **SAML afmelding omleiden (optioneel)** - 

    U kunt eventueel een SAML afmelding-Url, die kan worden gebruikt om te verwijzen naar Azure AD afmelden te gaan configureren. Als deze instelling is ingeschakeld en geconfigureerd, worden de gebruiker niet meer worden omgeleid naar de pagina Werkplek afmelden. In plaats daarvan wordt de gebruiker omgeleid naar de url die is toegevoegd in de instelling van de SAML-afmelding omleiden.

### <a name="configuring-reauthentication-frequency"></a>Herauthenticatie frequentie configureren

U kunt configureren dat werkplek om te vragen voor een SAML-controle elke dag, drie dagen, week, twee weken, maanden of nooit.

> [!NOTE] 
>De minimumwaarde voor de SAML-controle voor mobiele toepassingen is ingesteld op één week.

U kunt ook afdwingen dat een SAML opnieuw instellen voor alle gebruikers met behulp van de knop: vereisen SAML-verificatie voor alle gebruikers nu.


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/workplacebyfacebook-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/workplacebyfacebook-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/workplacebyfacebook-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Het maken van een werkplek op Facebook testgebruiker

In deze sectie een gebruiker met de naam Britta Simon gemaakt in de werkplek op Facebook. Werkplek door Facebook biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld.

Er is geen actie voor u in deze sectie. Als een gebruiker niet in de werkplek van Facebook bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot werkplek op Facebook.

>[!Note]
>Als u wilt maken van een gebruiker handmatig, neem contact op met [werkplek door het ondersteuningsteam Facebook-Client](https://workplace.fb.com/faq/)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan een werkplek op Facebook.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon werkplek koppelen met Facebook, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **werkplek door Facebook**.

    ![Eenmalige aanmelding configureren](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/workplacebyfacebook-tutorial/tutorial_general_203.png
