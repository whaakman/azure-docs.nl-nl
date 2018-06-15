---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe teken | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: c3b7e7178ef68475f331edf058ca0f23661af3ea
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34338870"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Zelfstudie: Azure Active Directory-integratie met Adobe teken

In deze zelfstudie leert u hoe Adobe aanmelding integreren met Azure Active Directory (Azure AD).

Meld u Adobe integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Adobe aanmelding heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Adobe aanmelding (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Adobe teken, moet u de volgende items:

- Een Azure AD-abonnement
- Een Adobe aanmelding eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adobe teken uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adobe-sign-from-the-gallery"></a>Adobe teken uit de galerie toevoegen
Voor het configureren van de integratie van Adobe aanmelding in Azure AD, moet u Adobe teken uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Adobe teken uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Adobe aanmelding**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Selecteer in het deelvenster resultaten **Adobe aanmelding**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Adobe aanmelding op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Adobe aanmelding is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Adobe aanmelding tot stand worden gebracht.

Wijs in Adobe teken, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Adobe teken, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Adobe aanmelding](#creating-an-adobe-sign-test-user)**  - Adobe-aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Adobe aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met Adobe teken, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Adobe aanmelding** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. Op de **Adobe aanmelding domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<companyname>.echosign.com/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. Op de **Adobe aanmelding configuratie** sectie, klikt u op **Adobe aanmelding configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Voordat de configuratie moet u contact opnemen met [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) naar geaccepteerde uw domein in het Adobe-teken. Volg de onderstaande stappen voor het toevoegen van het domein:

    a. [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) stuurt u een willekeurig gegenereerde token. Token is voor uw domein, zoals: **adobe aanmelding verifiëren xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx =**

    b. U moet de verificatietoken publiceren in een DNS-record voor tekst en contactpersonen waarschuwen dat [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > U kunt verwachten dat dit slechts enkele dagen of misschien langer kan duren. Houd er rekening mee dat het vertragingen bij het doorgeven van de DNS-betekent dat een waarde in DNS gepubliceerd mogelijk niet zichtbaar is voor een uur of langer. We verwachten dat de IT-beheerder moet grondige kennis van hoe u dit token publiceert in een DNS-record voor tekst.
    
    c. Zodra u de hoogte stellen [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) via de ondersteuningsticket nadat het token is gepubliceerd, ze zullen valideren van het domein en toe te voegen aan uw account.
    
    d. Algemene stappen die u kunt nemen voor het publiceren van het token op een DNS-record-

    * Aanmelden bij uw domeinaccount
    * De pagina voor het bijwerken van de DNS-record vinden. Deze pagina kan worden aangeroepen voor DNS-beheer, de naam van Serverbeheer of geavanceerde instellingen.
    * De TXT-records vinden voor uw domein.
    * Een TXT-record met de opgegeven door Adobe volledige token waarde toevoegen
    * Sla uw wijzigingen op.

8. In een ander browservenster, meld u aan bij uw bedrijf Adobe aanmelding site als beheerder.

9. Klik in het menu SAML **Accountinstellingen**, en klik vervolgens op **SAML instellingen**.
   
    ![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")

10. In de **SAML instellingen** sectie, voert u de volgende stappen uit:
  
    ![Instellingen voor SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-instellingen")
   
    a. Als **SAML modus**, selecteer **SAML verplichte**.
   
    b. Selecteer **toestaan Adobe aanmelding accountbeheerders aan te melden met hun referenties van de aanmelding Adobe**.
   
    c. Als **maken van een gebruikersaccount**, selecteer **automatisch toevoegen van gebruikers worden geverifieerd via SAML**.

    d. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal in de **URL van de entiteit-ID/verlener** textbox.
    
    e. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal in de **aanmelding URL/SSO eindpunt** textbox.
   
    f. Plakken **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal in de **afmelding URL/SLO eindpunt** textbox.

    g. Open uw gedownloade **Certificate(Base64)** bestand in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **IdP certificaat** tekstvak

    h. Klik op **wijzigingen opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-adobe-sign-test-user"></a>Een testgebruiker Adobe aanmelding maken

Om Azure AD-gebruikers zich aanmelden bij Adobe aanmelding, moeten ze worden ingericht in Adobe aanmelding. In het geval van Adobe teken is inrichting een handmatige taak.

>[!NOTE]
>U kunt andere Adobe aanmelding gebruiker account hulpmiddelen voor het maken of API's geleverd door Adobe aanmelding aan inrichten AAD-gebruikersaccounts. 

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Adobe aanmelding** bedrijf site als administrator.

2. Klik in het menu bovenaan op **Account**, en klik vervolgens in het navigatievenster aan de linkerkant op **gebruikers en groepen**, en klik vervolgens op **Maak een nieuwe gebruiker**.
   
    ![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")
   
3. In de **nieuwe gebruiker maken** sectie, voert u de volgende stappen uit:
   
    ![Gebruiker maken](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "gebruiker maken")
   
    a. Typ de **e-mailadres**, **voornaam**, en **achternaam** van een geldige AAD-account dat u inrichten in de bijbehorende tekstvakken wilt.
   
    b. Klik op **gebruiker maken**.

>[!NOTE]
>De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen van het account bevat voordat deze geactiveerd wordt. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen voor het ondertekenen van Adobe.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Adobe ondertekenen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Adobe aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u op de tegel Adobe teken in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Adobe aanmelding.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png
