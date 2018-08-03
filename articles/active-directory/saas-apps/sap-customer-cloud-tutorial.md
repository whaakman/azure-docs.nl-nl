---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud voor klant | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP-Cloud voor klanten.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 59bbcbf9aaef17394151e7db1471b63b87a46288
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445714"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud voor klanten

In deze zelfstudie leert u over het integreren van SAP-Cloud voor klanten met Azure Active Directory (Azure AD).

Integratie van SAP-Cloud voor klanten met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SAP-Cloud voor klanten heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP-Cloud voor klant (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Cloud voor klanten, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP-Cloud voor klanten eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP-Cloud voor klanten uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>SAP-Cloud voor klanten uit de galerie toevoegen
Voor het configureren van de integratie van SAP-Cloud voor klanten met Azure AD, moet u SAP-Cloud voor klanten uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP-Cloud voor klanten uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **SAP-Cloud voor klant**.

    ![Het maken van een Azure AD-testgebruiker](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

1. Selecteer in het deelvenster resultaten **SAP-Cloud voor klant**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAP Cloud voor de klant op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SAP-Cloud voor de klant is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAP-Cloud voor de klant tot stand worden gebracht.

In SAP-Cloud voor klanten, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Cloud voor klanten, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een SAP-Cloud voor de testgebruiker klant](#creating-a-sap-cloud-for-customer-test-user)**  : als u wilt een equivalent van Britta Simon hebben in SAP-Cloud voor klanten die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw SAP-Cloud voor toepassing van de klant.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud voor klanten, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP-Cloud voor klant** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

1. Op de **SAP-Cloud voor het domein van de klant en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server name>.crm.ondemand.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [SAP-Cloud voor klant Client-ondersteuningsteam](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) om deze waarden te verkrijgen. 

1. Op de **gebruikerskenmerken** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. In **gebruikers-id** in de lijst met de **ExtractMailPrefix()** functie.

    b. Uit de **e-Mail** , selecteert u het gebruikerskenmerk die u wilt gebruiken voor uw implementatie.
    Bijvoorbeeld, als u wilt de werknemer-id gebruiken als unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u user.extensionattribute2.  

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_general_400.png)

1. Op de **SAP-Cloud voor de configuratie van de klant** sectie, klikt u op **SAP-Cloud configureren voor klant** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

1. Als u eenmalige aanmelding die zijn geconfigureerd, moet u de volgende stappen uitvoeren:
   
    a. Meld u aan bij de SAP-Cloud voor Customer portal met beheerdersrechten.
   
    b. Navigeer naar de **toepassings- en algemene Gebruikersbeheertaak** en klikt u op de **id-Provider** tabblad.
   
    c. Klik op **nieuwe id-Provider** en selecteert u de XML-bestand voor metagegevens die u hebt gedownload vanuit Azure portal. Door het importeren van de metagegevens, uploadt het systeem automatisch de vereiste handtekeningcertificaat en versleutelingscertificaat.
   
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory is de URL van de Bevestigingsconsumerservice-element in de SAML-aanvraag, Selecteer daarom vereist de **opnemen URL van de Bevestigingsconsumerservice** selectievakje.
   
    e. Klik op **activeren Single Sign-On**.
   
    f. Sla uw wijzigingen op.
   
    g. Klik op de **mijn systeem** tabblad.
   
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. In **Azure AD aanmeldings-URL** tekstvak plakken **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
   
    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Opgeven of de werknemer handmatig kiezen kunt tussen het aanmelden met gebruikersnaam en wachtwoord of eenmalige aanmelding door het selecteren van de **handmatige Identity Provider selectie**.
   
    j. In de **SSO URL** sectie, geef de URL die moet worden gebruikt door uw werknemers aan te melden op het systeem. 
    In de **URL verzonden naar de werknemer** aanbieden, u kunt kiezen tussen de volgende opties:
   
    **Niet-SSO-URL**
   
    Het systeem wordt alleen de URL van de normale systeem verzonden naar de werknemer. De werknemer kan niet aanmelden met eenmalige aanmelding, en moet gebruik wachtwoord of certificaat in plaats daarvan.
   
    **URL VOOR EENMALIGE AANMELDING** 
   
    Het systeem wordt alleen de URL voor eenmalige aanmelding verzonden naar de werknemer. De werknemer kunt aanmelden met behulp van eenmalige aanmelding. Authenticatie-aanvraag wordt omgeleid via de id-provider.
   
    **Automatische selectie**
   
    Als eenmalige aanmelding niet actief is, verzendt het systeem de normale system-URL naar de werknemer. Als eenmalige aanmelding actief is, controleert het systeem of de werknemer een wachtwoord heeft. Als een wachtwoord beschikbaar is, worden zowel SSO-URL en niet-SSO-URL worden verzonden naar de werknemer. Als de werknemer geen wachtwoord heeft, wordt echter alleen de URL voor eenmalige aanmelding verzonden naar de werknemer.
   
    k. Sla uw wijzigingen op.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sap-customer-cloud-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sap-customer-cloud-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sap-customer-cloud-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Het maken van een SAP-Cloud voor de testgebruiker klant

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SAP-Cloud voor klanten. Neem contact op met [SAP-Cloud voor klant-ondersteuningsteam](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) om toe te voegen de gebruikers in de SAP Cloud platform van de klant. 

> [!NOTE]
> Zorg ervoor dat NameID-waarde met het gebruikersnaamveld in de SAP Cloud platform van de klant overeenkomen moet.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon toegang verlenen tot de SAP-Cloud voor klanten gebruiken Azure eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar SAP-Cloud voor klanten, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SAP-Cloud voor klant**.

    ![Eenmalige aanmelding configureren](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de SAP-Cloud voor klant-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw SAP-Cloud voor toepassing van de klant.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/sap-customer-cloud-tutorial/tutorial_general_203.png

