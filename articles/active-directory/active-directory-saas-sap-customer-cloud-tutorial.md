---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud voor klant | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Cloud voor de klant.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: b6ea3cfd8e50d82b5044708e78150fefb39b4c38
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud voor klant

In deze zelfstudie leert u SAP Cloud voor klanten met Azure Active Directory (Azure AD) integreren.

Integratie van SAP Cloud voor klanten met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAP Cloud voor klant heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP Cloud voor klant (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met SAP Cloud configureren voor de klant, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cloud SAP voor eenmalige aanmelding klant abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Cloud toe te voegen voor de klant uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>SAP Cloud toe te voegen voor de klant uit de galerie
Voor het configureren van de integratie van SAP Cloud voor klanten met Azure AD, moet u SAP Cloud voor klant uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP Cloud voor klant uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **SAP Cloud voor klant**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Selecteer in het deelvenster resultaten **SAP Cloud voor klant**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAP Cloud voor klant op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de Cloud SAP voor de klant is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Cloud SAP voor klant tot stand worden gebracht.

In de Cloud SAP voor de klant, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Cloud voor de klant, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Cloud SAP voor klant testgebruiker](#creating-a-sap-cloud-for-customer-test-user)**  - bevatten een equivalent van Britta Simon SAP Cloud voor klant die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Cloud SAP voor toepassing van de klant.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud voor de klant, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP Cloud voor klant** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. Op de **SAP Cloud voor domein van de klant en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server name>.crm.ondemand.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [SAP Cloud voor klant Client ondersteuningsteam](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) ophalen van deze waarden. 

4. Op de **gebruikerskenmerken** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. In **gebruikers-id** lijst, selecteert de **ExtractMailPrefix()** functie.

    b. Van de **Mail** , selecteert u het gebruikerskenmerk die u wilt gebruiken voor uw implementatie.
    Bijvoorbeeld, als u wilt gebruiken van de werknemer-id als unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u user.extensionattribute2.  

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. Op de **SAP Cloud voor de configuratie van de klant** sectie, klikt u op **SAP Cloud configureren voor de klant** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. Als u eenmalige aanmelding die zijn geconfigureerd, moet u de volgende stappen uitvoeren:
   
    a. De aanmelding in de Cloud SAP voor Customer portal met beheerdersrechten.
   
    b. Navigeer naar de **toepassings- en algemene beheertaak gebruiker** en klik op de **identiteitsprovider** tabblad.
   
    c. Klik op **nieuwe identiteitsprovider** en selecteert u het XML-bestand voor metagegevens die u hebt gedownload vanuit de Azure-portal. U importeert de metagegevens, uploadt het systeem automatisch de vereiste handtekeningcertificaat en een versleutelingscertificaat.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory is vereist voor het element Assertion Consumer Service-URL in de SAML-aanvraag, dus selecteer de **Assertion Consumer Service-URL opnemen** selectievakje.
   
    e. Klik op **activeren Single Sign-On**.
   
    f. Sla uw wijzigingen op.
   
    g. Klik op de **mijn systeem** tabblad.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. In **Azure AD aanmelding URL** textbox plakken **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    ik. Opgeven of de werknemer handmatig kiezen kunt tussen het aanmelden met gebruikersnaam en wachtwoord of eenmalige aanmelding door het selecteren van de **handmatige identiteit Provider selectie**.
   
    j. In de **URL SSO** sectie, de URL opgeven die moet worden gebruikt door uw werknemers zich aanmelden op het systeem. 
    In de **URL verzonden naar werknemer** wilt weergeven, kunt u kiezen uit de volgende opties:
   
    **Niet-SSO-URL**
   
    Het systeem wordt de URL van de normale system verzendt naar de werknemer. De werknemer kan niet aanmelden met eenmalige aanmelding, en moet gebruik wachtwoord of certificaat in plaats daarvan.
   
    **URL VOOR EENMALIGE AANMELDING** 
   
    Het systeem verzendt alleen de SSO-URL naar de werknemer. De werknemer kunt aanmelden met behulp van eenmalige aanmelding. Authenticatie-aanvraag wordt via de IdP omgeleid.
   
    **Automatische selectie**
   
    Als eenmalige aanmelding niet actief is, wordt de URL van de normale systeem door het systeem verzonden naar de werknemer. Als eenmalige aanmelding actief is, controleert het systeem of de werknemer een wachtwoord heeft. Als een wachtwoord beschikbaar is, worden zowel de URL van de SSO- en Non-SSO-verzonden naar de werknemer. Als de werknemer geen wachtwoord heeft, wordt echter alleen de URL voor eenmalige aanmelding verzonden naar de werknemer.
   
    k. Sla uw wijzigingen op.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Maken van een Cloud SAP voor klant testgebruiker

In deze sectie maakt u Britta Simon aangeroepen in SAP Cloud voor klant van een gebruiker. Neem contact op met [SAP Cloud voor afdeling Klantenondersteuning](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) om toe te voegen de gebruikers in de Cloud SAP voor klant-platform. 

> [!NOTE]
> Zorg dat NameID waarde met het veld username in de Cloud SAP voor klant-platform overeenkomen moet.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot SAP Cloud voor klant.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen SAP Cloud voor de klant, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP Cloud voor klant**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Cloud SAP voor klant-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw SAP-Cloud voor de toepassing van de klant.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

