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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c466e811d868403c59d6615882422996442d792a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045824"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Zelfstudie: Azure Active Directory-integratie met SAP HANA

In deze zelfstudie leert u hoe u SAP HANA integreren met Azure Active Directory (Azure AD).

Wanneer u SAP HANA geïntegreerd met Azure AD, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SAP HANA heeft.
- U kunt uw gebruikers automatisch de ophalen wordt aangemeld bij de SAP HANA met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP HANA, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP HANA-abonnement dat is eenmalige aanmelding (SSO) ingeschakeld
- Een HANA-exemplaar dat wordt uitgevoerd op een openbare IaaS, on-premises, Azure-VM of SAP grote instanties in Azure
- De web-XSA beheerinterface, evenals de HANA Studio is geïnstalleerd op de HANA-instantie

> [!NOTE]
> We raden niet met behulp van een productie-omgeving van SAP HANA voor het testen van de stappen in deze zelfstudie. De integratie in de ontwikkeling of de faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- [Een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) van Azure AD als u nog een proefversie Azure AD-omgeving hebt.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP HANA uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-sap-hana-from-the-gallery"></a>SAP HANA uit de galerie toevoegen
Voor het configureren van de integratie van SAP HANA in Azure AD, SAP HANA uit de galerie te toevoegt aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de SAP HANA uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt de nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan in het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SAP HANA**. Selecteer vervolgens **SAP HANA** vanuit het deelvenster resultaten. Selecteer ten slotte de **toevoegen** om toe te voegen van de toepassing. 

    ![De nieuwe toepassing](./media/saphana-tutorial/tutorial_saphana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAP HANA op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD weten wie de gebruiker equivalent in SAP HANA is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker tot stand te brengen in SAP HANA.

In SAP HANA, geven de **gebruikersnaam** waarde dezelfde waarde voor de **gebruikersnaam** in Azure AD. Deze stap stelt de koppeling tussen de twee gebruikers.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SAP HANA, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configuring-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#creating-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker SAP HANA](#creating-a-sap-hana-test-user) een equivalent van Britta Simon hebben in SAP HANA die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Eenmalige aanmelding testen](#testing-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw SAP HANA-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP HANA, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP HANA** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** dialoogvenster onder **SAML gebaseerde aanmelding**, selecteer **modus**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/saphana-tutorial/tutorial_saphana_samlbase.png)

3. In de **SAP HANA-domein en URL's** sectie, de volgende stappen uit:

    ![Domein en URL's, eenmalige aanmelding informatie](./media/saphana-tutorial/tutorial_saphana_url.png)

    a. In de **id** typt u het volgende: `HA100` 

    b. In de **antwoord-URL** vak, typ een URL met het volgende patroon: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id en antwoord-URL. Neem contact op met de [SAP HANA client-ondersteuningsteam](https://cloudplatform.sap.com/contact.html) om deze waarden te verkrijgen. 

4. In de **SAML-handtekeningcertificaat** sectie, selecteer **Metadata XML**. Sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Als het certificaat niet actief is, klikt u vervolgens het te activeren door het selecteren van de **nieuw certificaat activeren** selectievakje in Azure AD. 

5. De SAP HANA-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. De volgende Schermafbeelding toont een voorbeeld van deze indeling. 

    We hebben hier toegewezen de **gebruikers-id** met de **ExtractMailPrefix()** functie van **user.mail**. Dit geeft de waarde van het aanpassingsvoorvoegsel van de gebruiker e-mailbericht, dit de unieke gebruikers-ID is. Deze gebruikersnaam wordt verzonden naar de SAP HANA-toepassing in elke geslaagde reactie.

    ![Eenmalige aanmelding configureren](./media/saphana-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie van de **eenmalige aanmelding** dialoogvenster vak, voer de volgende stappen uit:

    a. In de **gebruikers-id** vervolgkeuzelijst, selecteer **ExtractMailPrefix**.
    
    b. In de **e-Mail** vervolgkeuzelijst, selecteer **user.mail**.

7. Selecteer de knop **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/saphana-tutorial/tutorial_general_400.png)
    
8. Voor het configureren van eenmalige aanmelding aan de SAP HANA, moet u zich aanmelden bij uw **HANA XSA webconsole** door te gaan naar de respectieve HTTPS-eindpunt.

    > [!NOTE]
    > In de standaardconfiguratie leidt de URL van de aanvraag naar een aanmeldingsscherm, die de referenties van een geverifieerde gebruiker van de SAP HANA-database vereist. De gebruiker die zich aanmeldt moet machtigingen hebben voor SAML-beheertaken uitvoeren.

9. Ga in de webinterface XSA naar **SAML-identiteitsprovider**. Selecteer de **+** knop aan de onderkant van het scherm om weer te geven de **identiteit Providerinfo toevoegen** deelvenster. Vervolgens voert u de volgende stappen uit:

    ![ID-Provider toevoegen](./media/saphana-tutorial/sap1.png)

    a. In de **identiteit Providerinfo toevoegen** deelvenster, plak de inhoud van de Metadata-XML (die u hebt gedownload van de Azure-portal) in de **metagegevens** vak.

    ![Instellingen voor id-Provider toevoegen](./media/saphana-tutorial/sap2.png)

    b. Als de inhoud van het XML-document geldig zijn, het parseren proces haalt de informatie die nodig voor de **onderwerp, de entiteit-ID en de verlener** velden in de **algemene gegevens** scherm. Pakt deze ook de informatie die nodig is voor de URL-velden in de **bestemming** scherm, bijvoorbeeld de **basis-URL en SingleSignOn URL (*)** velden.

    ![Instellingen voor id-Provider toevoegen](./media/saphana-tutorial/sap3.png)

    c. In de **naam** vak van het **algemene gegevens** scherm, voer een naam voor de nieuwe SAML SSO-id-provider.

    > [!NOTE]
    > De naam van de SAML-id-provider is verplicht en moet uniek zijn. Deze wordt weergegeven in de lijst met beschikbare SAML id-providers die wordt weergegeven wanneer u SAML als de verificatiemethode voor SAP HANA XS toepassingen selecteren moeten worden gebruikt. Bijvoorbeeld, u kunt dit doen de **verificatie** scherm van het beheerprogramma voor XS artefact.

10. Selecteer **opslaan** om op te slaan van de details van de SAML-id-provider en de nieuwe SAML-id-provider toevoegen aan de lijst met bekende SAML-id-providers.

    ![De knop Opslaan](./media/saphana-tutorial/sap4.png)

11. In HANA Studio, binnen de eigenschappen van de **configuratie** tabblad, het filteren van de instellingen door **saml**. Pas vervolgens de **assertion_timeout** van **10 sec** naar **120 per seconde**.

    ![assertion_timeout instelling](./media/saphana-tutorial/sap7.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app! Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie in de [documentatie over Azure AD embedded](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/saphana-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/saphana-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van het dialoogvenster.
 
    ![De knop toevoegen](./media/saphana-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/saphana-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven**, en noteer het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-sap-hana-test-user"></a>Maak een testgebruiker SAP HANA

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden met SAP HANA, moet u ze voorzien in SAP HANA.
SAP HANA biedt ondersteuning voor just-in-time inrichting, die door standaard is ingeschakeld.

Als u een gebruiker handmatig hebt gemaakt wilt, kunt u de volgende stappen:

>[!NOTE]
>U kunt de externe verificatie die gebruikmaakt van de gebruiker wijzigen. Ze kunnen worden geverifieerd met een extern systeem, zoals Kerberos. Voor gedetailleerde informatie over externe identiteiten, neem contact op met uw [domeinbeheerder](https://cloudplatform.sap.com/contact.html).

1. Open de [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) als beheerder, en vervolgens kan de DB-gebruiker voor SAML SSO.

    ![Gebruiker maken](./media/saphana-tutorial/sap5.png)

2. Schakel het selectievakje onzichtbaar aan de linkerkant van **SAML**, en selecteer vervolgens de **configureren** koppeling.

3. Selecteer **toevoegen** om toe te voegen van de SAML-id-provider.  Selecteer de juiste SAML IDP, en selecteer vervolgens **OK**.

4. Voeg de **externe id** (in dit geval BrittaSimon) of kies **eventuele**. Selecteer vervolgens **OK**.

    >[!Note]
    >Als de **eventuele** selectievakje niet is ingeschakeld, wordt de naam van de gebruiker in HANA moet exact overeenkomen met de naam van de gebruiker in de UPN voor het domeinachtervoegsel. (Bijvoorbeeld BrittaSimon@contoso.com BrittaSimon in HANA wordt.)

5. Voor testdoeleinden alle toewijzen **XS** rollen aan de gebruiker.

    ![Toewijzen van rollen](./media/saphana-tutorial/sap6.png)

    > [!TIP]
    > U moet machtigingen die geschikt voor uw use-cases alleen zijn op te geven.

6. Sla de gebruiker.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot SAP HANA.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon met SAP HANA, kunt u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure-portal. Ga naar de directoryweergave en Ga naar **bedrijfstoepassingen**. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SAP HANA**.

    ![Gebruiker toewijzen](./media/saphana-tutorial/tutorial_saphana_app.png) 

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Selecteer de **toevoegen** knop. In de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst.

6. Klik op de **Selecteer** knop in de **gebruikers en groepen** in het dialoogvenster.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de SAP HANA-tegel in het toegangsvenster selecteert, moet u automatisch ophalen aangemeld bij uw SAP HANA-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/saphana-tutorial/tutorial_general_01.png
[2]: ./media/saphana-tutorial/tutorial_general_02.png
[3]: ./media/saphana-tutorial/tutorial_general_03.png
[4]: ./media/saphana-tutorial/tutorial_general_04.png

[100]: ./media/saphana-tutorial/tutorial_general_100.png

[200]: ./media/saphana-tutorial/tutorial_general_200.png
[201]: ./media/saphana-tutorial/tutorial_general_201.png
[202]: ./media/saphana-tutorial/tutorial_general_202.png
[203]: ./media/saphana-tutorial/tutorial_general_203.png

