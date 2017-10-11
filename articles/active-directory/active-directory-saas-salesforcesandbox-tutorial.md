---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 90e08b9cf2feb93de4877bec9734352949896dca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox

In deze zelfstudie leert u hoe Salesforce Sandbox integreren met Azure Active Directory (Azure AD).

Salesforce Sandbox integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Salesforce-Sandbox heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce Sandbox (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce Sandbox, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce Sandbox eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce Sandbox uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Salesforce Sandbox uit de galerie toevoegen
Voor het configureren van de integratie van Salesforce Sandbox in Azure AD, moet u Salesforce Sandbox uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce Sandbox uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Salesforce Sandbox**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. Selecteer in het deelvenster resultaten **Salesforce Sandbox**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en test eenmalige aanmelding Azure AD met Salesforce-Sandbox op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Salesforce Sandbox is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Salesforce Sandbox tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Salesforce Sandbox.

Om te configureren en testen van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Salesforce Sandbox](#creating-a-salesforce-sandbox-test-user)**  - Salesforce Sandbox die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Salesforce-Sandbox-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce Sandbox** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Op de **Salesforce sandbox-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Deze waarde is niet de werkelijke. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Salesforce Sandbox Client ondersteuningsteam](https://help.salesforce.com/support) deze waarde op te halen.


4. Als u al eenmalige aanmelding voor een ander exemplaar van Salesforce Sandbox hebt geconfigureerd in uw directory, dan moet u ook configureren de **id** hebben dezelfde waarde als de **aanmelden URL**. 
    
    >[!Note]
    >De **id** veld kan worden gevonden door het controleren van de **weergeven geavanceerde instellingen** selectievakje op het **App-URL configureren** pagina van het dialoogvenster 


5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. Op de **Salesforce Sandbox configuratie** sectie, klikt u op **Salesforce Sandbox configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. Een nieuw tabblad openen in uw browser en meld u aan uw Salesforce-Sandbox-beheerdersaccount.

9. Klik in het menu bovenaan op **Setup**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. Klik in het navigatievenster aan de linkerkant op **beveiligingsmechanismen**, en klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. De volgende stappen uitvoeren in de sectie instellingen voor eenmalige aanmelding: ![configureren eenmalige aanmelding](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Selecteer **SAML ingeschakeld**. 

     b.  Klik op **Nieuw**.

12. Voer de volgende stappen uit in de sectie SAML Single Sign-On-instellingen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    a.In het tekstvak voor de naam, typ de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD\_Test*). 

    b. Plakken **SMAL entiteit-ID** waarde in de **verlener** textbox.

    c. In de **entiteit-Id** textbox type **https://test.salesforce.com** als dit het eerste exemplaar van Salesforce Sandbox die u aan uw directory toevoegen wilt. Als u al hebt toegevoegd een exemplaar van Salesforce-Sandbox vervolgens voor de **entiteit-ID** typt u in de **aanmelding op URL**, die moet worden in deze indeling:`http://company.my.salesforce.com`  
 
    d. Klik op **Bladeren** om de gedownloade certificaat te uploaden.  

    e. Als **SAML identiteitstype**, selecteer **Assertion bevat de Federation-ID van het gebruikersobject**.
 
    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentifier van het onderwerp overzicht**.

    g. Plakken **Single Sign-On Service-URL** in de **identiteit Provider aanmeldings-URL** textbox. 

    h. SFDC biedt geen ondersteuning voor SAML-afmelden.  Als een tijdelijke oplossing 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' te plakken in de **identiteit Provider afmelding URL** textbox.

    ik. Als **Provider geïnitieerd aanvragen servicebinding**, selecteer **HTTP POST**. 

    j. Klik op **Opslaan**.

### <a name="enable-your-domain"></a>Uw domein inschakelen
Deze sectie wordt ervan uitgegaan dat u al hebt gemaakt een domein.  Zie voor meer informatie [definiëren van uw domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Als u wilt maken voor uw domein, moet u de volgende stappen uitvoeren:**

1. Klik in het navigatiedeelvenster links op **domeinbeheer**, en klik vervolgens op **mijn domein.**
   
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Zorg dat uw domein correct is geconfigureerd. 

2. In de **pagina aanmeldingsinstellingen** sectie, klikt u op **bewerken**, naarmate **verificatieservice**, selecteert u de naam van de SAML Single Sign-On-instelling van de vorige sectie en klik tot slot **opslaan**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Als u een domein geconfigureerd hebt, moeten uw gebruikers de domein-URL voor aanmelding bij de sandbox Salesforce gebruiken.  

Als u de waarde van de URL, klikt u op de SSO-profiel dat u hebt gemaakt in de vorige sectie.    

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. Om weer te geven naar de lijst met gebruikers gaan **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>Maken van een testgebruiker Salesforce Sandbox

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce Sandbox. SalesForce Sandbox ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.
Er is geen actie-item voor u in deze sectie. Als een gebruiker in Salesforce Sandbox nog niet bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot de Salesforce-Sandbox.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan de Salesforce Sandbox.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Salesforce Sandbox**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

