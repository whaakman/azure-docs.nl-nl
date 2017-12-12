---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP HANA | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP HANA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 855525e2c1d3c33cc7134bbc1cd9b53ca59e1a70
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Zelfstudie: Azure Active Directory-integratie met SAP HANA

In deze zelfstudie leert u hoe SAP HANA integreren met Azure Active Directory (Azure AD).

Als u een SAP HANA met Azure AD integreren, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAP HANA heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld SAP HANA met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP HANA, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP HANA-abonnement dat is eenmalige aanmelding (SSO) ingeschakeld
- Een HANA-exemplaar dat wordt uitgevoerd op een openbare IaaS on-premises, Azure VM of grote SAP-exemplaren in Azure
- De webinterface voor het beheer van XSA, evenals de HANA Studio is geïnstalleerd op het exemplaar HANA

> [!NOTE]
> We raden niet met behulp van een productieomgeving van SAP HANA voor het testen van de stappen in deze zelfstudie. De integratie in de ontwikkeling of de faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- [Ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) van Azure AD als u nog een proefabonnement Azure AD-omgeving hebt.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP HANA uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-sap-hana-from-the-gallery"></a>SAP HANA uit de galerie toevoegen
Voor het configureren van de integratie van SAP HANA in Azure AD SAP HANA uit de galerie te toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP HANA uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Selecteer om de nieuwe toepassing toe de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAP HANA**. Selecteer vervolgens **SAP HANA** vanuit het deelvenster resultaten. Tot slot selecteert u de **toevoegen** om toe te voegen van de toepassing. 

    ![De nieuwe toepassing](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met SAP HANA op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in SAP HANA is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker in SAP HANA vast te stellen.

In een SAP HANA geven de **gebruikersnaam** waarde dezelfde waarde voor de **gebruikersnaam** in Azure AD. Deze stap wordt de koppeling tussen de twee gebruikers.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP HANA, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configuring-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een SAP HANA-testgebruiker](#creating-a-sap-hana-test-user) SAP HANA die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#testing-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing SAP HANA.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP HANA, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP HANA** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** dialoogvenster onder **op basis van SAML aanmelding**, selecteer **modus**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. In de **SAP HANA-domein en de URL's** sectie, voert de volgende stappen uit:

    ![Domein- en URL's van eenmalige aanmelding informatie](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. In de **id** typt u het volgende:`HA100` 

    b. In de **antwoord-URL** vak een URL met het volgende patroon volgen:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke identificatie en antwoord-URL. Neem contact op met de [SAP HANA client ondersteuningsteam](https://cloudplatform.sap.com/contact.html) ophalen van deze waarden. 

4. In de **SAML-certificaat voor ondertekening van** sectie **Metadata XML**. Sla het metagegevensbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Als het certificaat is niet actief, vervolgens moet u er actieve door het selecteren van de **nieuwe certificaat activeren** selectievakje in Azure AD. 

5. De toepassing SAP HANA verwacht de SAML-asserties in een specifieke indeling. De volgende Schermafbeelding toont een voorbeeld van deze indeling. 

    Hier we hebt toegewezen de **gebruikers-id** met de **ExtractMailPrefix()** functie van **user.mail**. Dit resulteert in de waarde van het voorvoegsel van de gebruiker e-mailadres waarmee de unieke gebruikersnaam is. Deze gebruikersnaam wordt verzonden naar de SAP HANA-toepassing in elke geslaagde reactie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie van de **eenmalige aanmelding** dialoogvenster vak, voert de volgende stappen uit:

    a. In de **gebruikers-id** vervolgkeuzelijst, selecteer **ExtractMailPrefix**.
    
    b. In de **Mail** vervolgkeuzelijst, selecteer **user.mail**.

7. Selecteer de knop **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Voor het configureren van eenmalige aanmelding de SAP HANA-zijde, moet u zich aanmelden bij uw **HANA XSA webconsole** door te gaan naar de respectieve HTTPS-eindpunt.

    > [!NOTE]
    > In de standaardconfiguratie omleidt de URL van de aanvraag naar een scherm aanmelden, waarvoor de referenties van een geverifieerde gebruiker voor SAP HANA-database. De gebruiker die zich aanmeldt moet machtigingen hebben voor SAML-beheertaken uitvoeren.

9. Ga in de webinterface XSA naar **SAML-identiteitsprovider**. Selecteer de  **+**  knop aan de onderkant van het scherm om weer te geven de **toevoegen identiteit Provider Info** deelvenster. Vervolgens voert u de volgende stappen uit:

    ![ID-Provider toevoegen](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. In de **toevoegen identiteit Provider Info** deelvenster, plak de inhoud van de Metadata-XML (die u hebt gedownload van de Azure-portal) in de **metagegevens** vak.

    ![Instellingen voor de id-Provider toevoegen](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Als de inhoud van het XML-document geldig zijn, het parseren proces haalt de informatie die nodig voor de **onderwerp, de entiteit-ID en de verlener** velden in de **algemene gegevens** scherm. Pakt deze ook de informatie die nodig is voor de URL-velden in de **bestemming** scherm, bijvoorbeeld de  **basis-URL en SingleSignOn (*)** velden.

    ![Instellingen voor de id-Provider toevoegen](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. In de **naam** vak van het **algemene gegevens** scherm, voer een naam voor de nieuwe SSO SAML-identiteitsprovider.

    > [!NOTE]
    > De naam van de SAML-IDP is verplicht en moet uniek zijn. Deze weergegeven in de lijst met beschikbare SAML IDPs dat wordt weergegeven wanneer u SAML als de verificatiemethode voor SAP HANA XS toepassingen selecteren kunnen gebruikmaken van. Bijvoorbeeld, u kunt dit doen de **verificatie** scherm van het beheerprogramma van XS artefacten.

10. Selecteer **opslaan** de details van de SAML-identiteitsprovider opslaan en de nieuwe SAML IDP toevoegen aan de lijst met bekende SAML IDPs.

    ![knop Opslaan](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. In de Studio HANA binnen de eigenschappen van de **configuratie** tabblad, de instellingen door te filteren **saml**. Pas vervolgens de **assertion_timeout** van **10 sec** naar **120 sec**.

    ![assertion_timeout instelling](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app! Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie in de [documentatie van Azure AD ingesloten](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** aan de bovenkant van het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** in het vak de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven**, en noteer het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-sap-hana-test-user"></a>Een SAP HANA-testgebruiker maken

Om Azure AD-gebruikers aan te melden bij SAP HANA, moet u ze in SAP HANA inrichten.
SAP HANA ondersteunt just-in-time-inrichting, die door standaard ingeschakeld.

Als u een gebruiker handmatig maken wilt, moet u de volgende stappen uitvoeren:

>[!NOTE]
>U kunt de externe verificatie met behulp van de gebruiker wijzigen. Ze kunnen worden geverifieerd met een extern systeem, zoals Kerberos. Voor gedetailleerde informatie over externe identiteiten contact op met uw [domeinbeheerder](https://cloudplatform.sap.com/contact.html).

1. Open de [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) als beheerder, en vervolgens kan de database-gebruiker voor SAML-SSO.

    ![Gebruiker maken](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Schakel het selectievakje onzichtbaar aan de linkerkant van **SAML**, en selecteer vervolgens de **configureren** koppeling.

3. Selecteer **toevoegen** de SAML-IDP toevoegen.  Selecteer de juiste SAML IDP, en selecteer vervolgens **OK**.

4. Voeg de **externe identiteit** (in dit geval BrittaSimon) of kies **eventuele**. Selecteer vervolgens **OK**.

    >[!Note]
    >Als de **eventuele** selectievakje niet is ingeschakeld, wordt de gebruikersnaam van de in HANA moet exact overeenkomen met de naam van de gebruiker in de UPN voordat het domeinachtervoegsel. (Bijvoorbeeld BrittaSimon@contoso.com wordt BrittaSimon in HANA.)

5. Voor testdoeleinden alle toewijzen **XS** rollen aan de gebruiker.

    ![rollen toewijzen](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > U moet machtigingen die geschikt voor uw gebruiksvoorbeelden alleen zijn op te geven.

6. Sla de gebruiker.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SAP HANA.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SAP HANA, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal. Ga naar de directoryweergave en Ga naar **bedrijfstoepassingen**. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP HANA**.

    ![Gebruiker toewijzen](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Selecteer de **toevoegen** knop. In de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de **gebruikers** lijst.

6. Klik op de **Selecteer** knop in de **gebruikers en groepen** in het dialoogvenster.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de SAP HANA-tegel in het deelvenster toegang selecteert, moet u automatisch ophalen aangemeld bij uw SAP HANA-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

