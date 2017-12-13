---
title: 'Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Atlassian Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
ms.openlocfilehash: db9e9c7ae8380612bac9d0aeaaaf6df78cba523f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud

In deze zelfstudie leert u hoe Atlassian Cloud integreren met Azure Active Directory (Azure AD).

Atlassian Cloud integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Atlassian Cloud heeft.
- U kunt uw gebruikers zich hebben aangemeld automatisch inschakelen (eenmalige aanmelding) naar de Atlassian Cloud met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over software als een dienst (SaaS)-app-integratie met Azure AD [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Atlassian Cloud, moet u de volgende items:

- Een Azure AD-abonnement.
- Om in te schakelen Security Assertion Markup Language (SAML) eenmalige aanmelding voor Atlassian Cloud-producten, moet u Stel Identity Manager. Meer informatie over [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie test, wordt u aangeraden niet een productie-omgeving te gebruiken.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in de zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Atlassian Cloud uit de galerie toevoegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-atlassian-cloud-from-the-gallery"></a>Atlassian Cloud uit de galerie toevoegen
Voor het configureren van de integratie van Atlassian Cloud met Azure AD, Atlassian Cloud uit de galerie toevoegt aan de lijst met beheerde SaaS-apps als volgt:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** knop. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster Enterprise-toepassingen][2]
    
3. Selecteer om een toepassing toe **nieuwe toepassing**.

    !['Nieuwe application' knop][3]

4. Typ in het zoekvak **Atlassian Cloud**, selecteer in de lijst met resultaten **Atlassian Cloud**, en selecteer vervolgens **toevoegen**.

    ![Atlassian Cloud in de lijst met resultaten](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Atlassian Cloud, op basis van een testgebruiker met de naam *Britta Simon*.

Voor eenmalige aanmelding werkt, moet Azure AD de Atlassian Cloud-gebruiker en het bijbehorende equivalent identificeren in Azure AD. Met andere woorden, moet u een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Atlassian Cloud instellen.

Voor het opzetten van de relatie koppeling toewijzen als de Atlassian Cloud *gebruikersnaam* dezelfde waarde die toegewezen aan de Azure AD *gebruikersnaam*.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Atlassian Cloud, moet u de bouwstenen in de volgende secties worden voltooid.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Atlassian Cloud.

Voor het configureren van Azure AD eenmalige aanmelding met Atlassian Cloud, het volgende doen:

1. In de Azure-portal in de **Atlassian Cloud** toepassing integratie deelvenster **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** de optie **op basis van SAML aanmelding**.
 
    ![Venster voor eenmalige aanmelding](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Voor het configureren van de toepassing in de modus IDP geïnitieerde onder **Atlassian Cloud-domein en de URL's**, het volgende doen:

    ![Atlassian Cloud-domein en één URL's aanmelding informatie](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. In de **id** in het vak  **`https://auth.atlassian.com/saml/<unique ID>`** .
    
    b. In de **antwoord-URL** in het vak  **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`** .

    c. In de **Relay status** vak een URL met de volgende syntaxis:  **`https://<instancename>.atlassian.net`** .

4. Voor het configureren van de toepassing in de modus Serviceprovider geïnitieerde, selecteer de **weergeven geavanceerde instellingen voor URL** en klikt u op de **aanmelden URL** vak een URL met de volgende syntaxis:  **`https://<instancename>.atlassian.net`**  .

    ![Atlassian Cloud-domein en één URL's aanmelding informatie](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke-id en antwoord-URL en eenmalige aanmelding URL waarden. De werkelijke waarden kunt u krijgen via het scherm Atlassian Cloud SAML-configuratie. Kunnen de waarden later in de zelfstudie uitgelegd.

5. Onder **SAML-certificaat voor ondertekening van**, selecteer **Certificate(Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Uw toepassing Atlassian Cloud wordt verwacht dat de SAML-bevestigingen niet vinden in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw Token kenmerken van SAML-configuratie. 

    Standaard de **gebruikers-id** waarde is toegewezen aan user.userprincipalname. Deze waarde toewijzen aan user.mail wijzigen. U kunt ook een andere geschikte waarde volgens de instellingen van uw organisatie, maar in de meeste gevallen e-mail moet werken.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Selecteer **Opslaan**.

    ![De configureren eenmalige aanmelding knop Opslaan](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Openen de **eenmalige aanmelding configureren** venster in de **Atlassian Cloudconfiguratie** sectie **Atlassian Cloud configureren**. 

9. In de **Naslaggids** sectie, Kopieer de **SAML entiteit-ID** en **SAML Single Sign-On Service-URL**. 

    ![Atlassian cloudconfiguratie](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

10. Als u eenmalige aanmelding voor uw toepassing is geconfigureerd, moet u zich aanmelden bij de portal Atlassian met beheerdersreferenties.

11. Ga naar **Atlassian Sitebeheer** > **organisaties & beveiliging**. Als u dit nog niet hebt gedaan, maakt en naam van uw organisatie en selecteer vervolgens in het linkerdeelvenster **domeinen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

12. Selecteer de manier waarop dat u wilt controleren of uw domein: **DNS** of **HTTPS**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

13. Voor de DNS-verificatie in de **domeinen** Selecteer de **DNS** tabblad en doe vervolgens het volgende:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Als de waarde voor de tekstrecord (TXT-record) kopiëren, selecteert u **kopie**.

    b. Als u wilt een record toevoegen, gaat u naar de instellingenpagina in uw DNS.

    c. Selecteer de optie voor het toevoegen van een nieuwe record en plak de waarde die u hebt genoteerd in de **domeinen** venster de **waarde** veld. Uw DNS-record mogelijk ook verwijzen naar dit als **antwoord** of **beschrijving**.

    d. Uw DNS-record kan ook de volgende velden bevatten:
    
    * In de **recordtype** Voer **TXT**.
    * In de **Host-naam-Alias** vak, laat de standaardwaarde (@ of leeg).
    * In de **Time to live (TTL)** Voer **86400**.
    
    e.  Sla de record.

14. Ga terug naar de **domeinen** venster in de Organisatiebeheer en selecteer vervolgens **domein controleren**. In de **domein** vak, typ de naam van uw domein en selecteer vervolgens **domein controleren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
    > Omdat dit tot 72 uur om de TXT-recordwijzigingen pas van kracht op te nemen kan, weet u niet meteen of de verificatie van uw domein voltooid is. De als verificatiestatus wilt weergeven, Controleer de **domeinen** venster kort nadat u deze procedure hebt voltooid. De bijgewerkte status wordt weergegeven als *gecontroleerd*, zoals wordt weergegeven in de volgende afbeelding:
    > 
    > ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)
    > 
    > 

15. Voor de HTTPS-verificatie in de **domeinen** Selecteer de **HTTPS** tabblad en doe vervolgens het volgende:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a. Selecteer de HTML-bestand te downloaden **gedownloade bestand**.

    b. Het HTML-bestand uploaden naar de hoofdmap van uw domein.

16. Ga terug naar de **domeinen** pagina in Organisatiebeheer en selecteer **domein controleren**. In de **domein controleren** venster in de **domein** in het vak uw **domeinnaam**, en selecteer vervolgens **domein controleren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

17. Als het verificatieproces het bestand dat u hebt geüpload in de hoofdmap localiseren kan, de status van het domein is bijgewerkt naar *gecontroleerd*, zoals hier wordt weergegeven:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Zie voor meer informatie [Atlassian domeinverificatie](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

18. Selecteer in het linkerdeelvenster **eenmalige aanmelding SAML**. Als u dit nog niet hebt gedaan, zich abonneren op Atlassian Identity Manager.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

19. In de **toevoegen de SAML-configuratie** venster de volgende handelingen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. In de **identiteitsprovider entiteit-ID** vak, plak de SAML-entiteit-ID die u hebt gekopieerd uit de Azure-portal.

    b. In de **identiteitsprovider URL SSO** vak, plak de URL van het SAML-service voor eenmalige aanmelding die u hebt gekopieerd uit de Azure-portal.

    c. Open het gedownloade certificaat van de Azure-portal op een txt-bestand, de waarde kopiëren (zonder de *Begin Certificate* en *eindcertificaat* regels), en plak deze in de **openbare X509 certificaat** vak.
    
    d. Selecteer **configuratie op te slaan**.
     
20. Om ervoor te zorgen dat u de juiste URL's hebt ingesteld, moet u de Azure AD-instellingen bijwerken als volgt:
  
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. In het venster SAML kopiëren de **SP identiteit ID** en klikt u op de Azure-portal onder Atlassian Cloud **domein en de URL's**, plak deze in de **id** vak.
    
    b. In het venster SAML kopiëren de **SP Assertion Consumer Service-URL** en klikt u op de Azure-portal onder Atlassian Cloud **domein en de URL's**, plak deze in de **antwoord-URL** vak.  
        De aanmeldings-URL is de tenant-URL van uw Atlassian Cloud. 

    > [!NOTE]
    > Als u een bestaande klant bent, na het bijwerken van de **SP identiteit ID** en **SP Assertion Consumer Service-URL** waarden in de Azure-portal, selecteer **Ja, werk de configuratie van**. Als u een nieuwe klant bent, kunt u deze stap overslaan. 
    
21. Selecteer in de Azure-portal **opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de ingesloten documentatie in de **configuratie** sectie aan de onderkant van het venster. Zie voor meer informatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie kunt u testgebruiker Britta Simon in de Azure portal maken door het volgende te doen:

   ![Een Azure AD-testgebruiker maken][100]

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, selecteer **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. In de **alle gebruikers** Selecteer **toevoegen**.

    ![De knop toevoegen](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster gebruiker](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Een testgebruiker Atlassian Cloud maken

Inrichten om Azure AD-gebruikers aan te melden bij Atlassian Cloud, de gebruikersaccounts handmatig in de Atlassian Cloud als volgt:

1. In de **beheer** deelvenster **gebruikers**.

    ![De koppeling Atlassian Cloud-gebruikers](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Voor het maken van een gebruiker in de Atlassian Cloud selecteert **uitnodiging gebruiker**.

    ![Maakt u een gebruiker Atlassian Cloud](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. In de **e-mailadres** vak, voer het e-mailadres van de gebruiker en wijs de toegang tot toepassingen. 

    ![Maakt u een gebruiker Atlassian Cloud](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Als u wilt een uitnodiging voor een e-mailbericht naar de gebruiker, selecteer **gebruikers uitnodigen**.  
    Een uitnodiging voor een e-mailbericht wordt verzonden naar de gebruiker en na de uitnodiging accepteert, de gebruiker is actief in het systeem. 

>[!NOTE] 
>U kunt ook bulksgewijs-gebruikers maken door het selecteren van de **bulksgewijs maken** knop in de **gebruikers** sectie.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Atlassian Cloud. Ga als volgt te werk om dit te doen:

![Toewijzen van de gebruikersrol][200] 

1. Open in de Azure-portal, de **toepassingen** bekijken, gaat u naar de directoryweergave en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. In de **toepassingen** selecteert **Atlassian Cloud**.

    ![De koppeling Atlassian Cloud in de lijst met toepassingen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** selecteert **Britta Simon**.

6. In de **gebruikers en groepen** Selecteer **Selecteer**.

7. In de **toevoegen toewijzing** Selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u selecteert de **Atlassian Cloud** tegel in het deelvenster toegang u moet zich hebben aangemeld automatisch aan uw toepassing Atlassian Cloud.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

