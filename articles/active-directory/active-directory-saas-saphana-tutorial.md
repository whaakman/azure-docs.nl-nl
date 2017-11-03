---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP HANA | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP HANA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: a7e73f6ee763d1005ad85935cf2d8f6b24ecf116
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Zelfstudie: Azure Active Directory-integratie met SAP HANA

In deze zelfstudie leert u hoe SAP HANA integreren met Azure Active Directory (Azure AD).

SAP HANA integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAP HANA heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP HANA (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP HANA, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP HANA eenmalige aanmelding ingeschakeld abonnement
- Een actieve HANA exemplaar op een openbare IaaS, on-premises virtuele Azure-machines of grote SAP-exemplaren in Azure
- De webinterface voor XSA beheer evenals HANA Studio is geïnstalleerd op het exemplaar HANA.

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving van SAP HANA. De integratie in ontwikkeling of faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP HANA uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-hana-from-the-gallery"></a>SAP HANA uit de galerie toevoegen
Voor het configureren van de integratie van SAP HANA in Azure AD, moet u SAP HANA uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP HANA uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAP HANA**, selecteer **SAP HANA** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing. 

    ![De nieuwe toepassing](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met SAP HANA op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SAP HANA is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante SAP HANA-gebruiker worden gemaakt.

Wijs in SAP HANA de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP HANA, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SAP HANA-testgebruiker](#creating-a-sap-hana-test-user)**  - SAP HANA die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing SAP HANA.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP HANA, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP HANA** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Op de **SAP HANA-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Domein- en URL's van eenmalige aanmelding informatie](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. In de **id** textbox type als:`HA100` 

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [SAP HANA Client ondersteuningsteam](https://cloudplatform.sap.com/contact.html) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Als het certificaat is niet actief vervolgens deze actief maken door te klikken op het selectievakje 'Nieuw certificaat actief maken' in de Azure AD. 

5. De SAML-asserties verwacht SAP HANA-toepassing in een specifieke indeling. De volgende Schermafbeelding toont een voorbeeld voor deze. We hebben hier toegewezen de **gebruikers-id** met **ExtractMailPrefix()** functie van **user.mail**. Dit geeft de waarde van het voorvoegsel van e-mailadres van de gebruiker die de unieke gebruikersnaam. Dit wordt verzonden naar de SAP HANA-toepassing in elke geslaagde reactie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster:

    a. In de **gebruikers-id** vervolgkeuzelijst, selecteer **ExtractMailPrefix**.
    
    b. In de **Mail** vervolgkeuzelijst, selecteer **user.mail**.

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Eenmalige aanmelding configureren op **SAP HANA** side, meld u aan bij uw **HANA XSA webconsole** door te bladeren naar de respectieve HTTPS-eindpunt.

    > [!Note]
    > In de standaardconfiguratie omleidt de URL van de aanvraag naar een aanmeldingsscherm waarvoor de referenties van een geverifieerde gebruiker van SAP HANA-database het aanmeldingsproces voltooien. De gebruiker die zich aanmeldt, moet de vereiste bevoegdheden voor het SAML-beheertaken uitvoeren.

9. Navigeer in de webinterface XSA naar **SAML-identiteitsprovider** en van daaruit, klikt u op de **'+'** -knop aan de onderkant van het scherm voor het weergeven van het deelvenster identiteit Provider Info toevoegen en voer de volgende stappen uit:

    ![ID-Provider toevoegen](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. In de **toevoegen identiteit Provider Info** deelvenster, plak de inhoud van de Metadata XML, die u hebt gedownload vanuit Azure-portal in de **metagegevens** textbox.

    ![Instellingen voor de id-Provider toevoegen](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Als de inhoud van het XML-document geldig zijn, het parseren proces haalt de informatie die vereist worden ingevoegd in de **onderwerp, de entiteit-ID en de verlener** velden in de algemene gegevens scherm gebied en de URL-velden in het scherm doelgebied, bijvoorbeeld  **basis-URL en SingleSignOn (*)**.

    ![Instellingen voor de id-Provider toevoegen](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Voer een naam voor de nieuwe SSO SAML-identiteitsprovider in het vak de naam van het scherm van algemene gegevensgebied.

    > [!Note]
    > De naam van de SAML-IDP is verplicht en moet uniek zijn. deze weergegeven in de lijst met beschikbare SAML IDPs die wordt weergegeven als u de verificatiemethode voor SAP HANA XS toepassingen moet worden gebruikt, bijvoorbeeld in het gebied van het scherm verificatie van het beheerprogramma van XS artefact SAML selecteert.

10. De details van de nieuwe SAML-identiteitsprovider opslaan. Kies **opslaan** de details van de SAML-identiteitsprovider opslaan en de nieuwe SAML IDP toevoegen aan de lijst met bekende SAML IDPs.

    ![knop Opslaan](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. In Studio HANA binnen de eigenschappen van de **configuratie** tabblad, alleen instellingen door te filteren **saml** en pas de **assertion_timeout** van **10 sec** naar **120 sec**.

    ![assertion_timeout instelling](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sap-hana-test-user"></a>De gebruiker van een SAP HANA-test maken

Om Azure AD-gebruikers zich aanmelden bij SAP HANA, moeten ze worden ingericht in SAP HANA.
SAP HANA ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Als u een gebruiker handmatig maken wilt, voert u de volgende stappen uit:

>[!Note]
>U kunt de externe verificatie die wordt gebruikt door de gebruiker wijzigen.
Externe gebruikers worden geverifieerd met behulp van een extern systeem, bijvoorbeeld een Kerberos-systeem. Voor gedetailleerde informatie over externe identiteiten contact op met uw [domeinbeheerder](https://cloudplatform.sap.com/contact.html).

1. Open de [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) als beheerder en kan de database-gebruiker voor SAML-SSO.

    ![Gebruiker maken](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Vink het selectievakje onzichtbaar aan de linkerkant van **SAML** en volg de koppeling configureren.

3. Klik op **toevoegen** de SAML-IDP toevoegen en klik op **OK** de juiste SAML IDP selecteren.

4. Voeg de **externe identiteit** (ex. Hier BrittaSimon) of kies **''** en klik op **OK**.

    >[!Note]
    >Als 'Elke' selectievakje niet is ingeschakeld, wordt de gebruikersnaam van de in HANA moet exact overeenkomen met de naam van de gebruiker in de UPN voordat het domeinachtervoegsel (dat wil zeggen BrittaSimon@contoso.com zou worden BrittaSimon in HANA).

5. Voor testdoeleinden alle toewijzen **'XS'** rollen aan de gebruiker.

    ![rollen toewijzen](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > U moet deze geschikt is voor uw gebruiksvoorbeelden alleen machtigingen op te geven.

6. Sla de gebruiker.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SAP HANA.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SAP HANA, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP HANA**.

    ![Gebruiker toewijzen](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de SAP HANA-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw SAP HANA-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
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

