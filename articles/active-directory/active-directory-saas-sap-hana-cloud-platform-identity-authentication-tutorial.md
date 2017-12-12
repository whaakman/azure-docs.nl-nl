---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform identiteitsverificatie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en identiteitsverificatie voor SAP Cloud-Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: 0c7dd884eaadd1fba4fcbc19b6c9cf92c68a59ac
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform identiteitsverificatie

In deze zelfstudie leert u hoe SAP Cloud Platform identiteitsverificatie integreren met Azure Active Directory (Azure AD). SAP Cloud Platform-ID-verificatie wordt gebruikt als een IdP-proxy voor toegang tot SAP-toepassingen die gebruikmaken van Azure AD als de belangrijkste IdP.

Als u SAP Cloud Platform identiteitsverificatie met Azure AD integreren, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SAP-toepassingen heeft.
- U kunt uw gebruikers automatisch aanmelden bij het SAP-toepassingen met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie het artikel voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Cloud Platform-ID-verificatie, moet u de volgende items:

- Een Azure AD-abonnement.
- Een enkel teken op ingeschakeld abonnement voor identiteitsverificatie voor SAP Cloud-Platform.

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Als u een proefabonnement Azure AD-omgeving geen [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Cloud Platform identiteitsverificatie uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

Voordat u Duik in de technische details, is het essentieel om te begrijpen van de concepten die u van plan bent om te kijken. De SAP Cloud Platform identiteitsverificatie en Active Directory Federation Services kunt u eenmalige aanmelding via toepassingen of services die worden beveiligd door Azure AD (als een IdP) met SAP-toepassingen en services die worden beveiligd door SAP Cloud implementeren Identiteitsverificatie platform.

Op dit moment fungeert SAP Cloud Platform identiteitsverificatie als een Proxy-id-Provider voor SAP-toepassingen. Azure Active Directory wordt op zijn beurt fungeert als de toonaangevende id-Provider in deze installatie. 

Het volgende diagram ziet u deze relatie:

![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

Uw tenant SAP Cloud Platform identiteitsverificatie wordt met deze instelling geconfigureerd als een vertrouwde toepassing in Azure Active Directory. 

Alle SAP-toepassingen en services die u wilt beveiligen op deze manier worden vervolgens geconfigureerd in de beheerconsole voor identiteitsverificatie voor SAP Cloud-Platform.

Daarom moet de autorisatie voor het verlenen van toegang tot SAP-toepassingen en services te kunnen uitvoeren in SAP Cloud Platform identiteitsverificatie (in plaats van Azure Active Directory).

SAP Cloud Platform identiteitsverificatie configureren als een toepassing via de Azure Active Directory Marketplace, hoeft u niet om afzonderlijke claims of SAML asserties te configureren.

>[!NOTE] 
>Momenteel wordt alleen Web-SSO is getest door beide partijen. De stromen die nodig voor de App naar API of API-API-communicatie zijn moeten werken maar nog niet is getest. Ze worden tijdens de volgende activiteiten getest.
>

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>SAP Cloud Platform identiteitsverificatie uit de galerie toevoegen
Voor het configureren van de integratie van SAP Cloud Platform identiteitsverificatie in Azure AD, moet u SAP Cloud Platform identiteitsverificatie uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP Cloud Platform identiteitsverificatie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatievenster aan de linkerkant de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt de nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAP Cloud Platform identiteitsverificatie**. 

5. Selecteer **SAP Cloud Platform identiteitsverificatie** vanuit het deelvenster resultaten en selecteer vervolgens de **toevoegen** knop.

    ![SAP Cloud Platform identiteitsverificatie in de lijst met resultaten](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met identiteitsverificatie voor SAP Cloud-Platform. U configureert en getest met een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in SAP Cloud Platform identiteitsverificatie is. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker in SAP Cloud Platform identiteitsverificatie vast te stellen.

Geef de waarde in SAP Cloud Platform identiteitsverificatie **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. U kunt de koppeling tussen de twee gebruikers nu hebt vastgesteld.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Cloud Platform identiteitsverificatie, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maken van een testgebruiker SAP Cloud Platform identiteitsverificatie](#create-an-sap-cloud-platform-identity-authentication-test-user) SAP Cloud Platform verificatie van de identiteit die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing identiteitsverificatie voor SAP Cloud-Platform.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud Platform-ID-verificatie, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP Cloud Platform identiteitsverificatie** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** dialoogvenster onder **op basis van SAML aanmelding**, selecteer **modus** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Als u wilt configureren van de toepassing in **IDP** modus gestart in de **SAP Cloud Platform identiteit verificatiedomein en de URL's** sectie in het **id** vak Typ een URL met het volgende patroon volgen: `https://<entity-id>.accounts.ondemand.com`.  

    ![URL's en SAP Cloud Platform Identity verificatie Domain eenmalige aanmelding informatie](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde door de werkelijke ID bijwerken. Neem contact op met de [SAP Cloud Platform Identity-verificatieclient ondersteuningsteam](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) deze waarde op te halen. Als u deze waarde niet begrijpt, de SAP Cloud Platform identiteitsverificatie documentatie te lezen over [Tenant SAML 2.0 configuratie](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

4. Als u wilt configureren van de toepassing in **SP** geïnitieerd modus, selecteer **weergeven geavanceerde instellingen voor URL**. 

    ![URL's en SAP Cloud Platform Identity verificatie Domain eenmalige aanmelding informatie](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    In de **aanmelding op URL** vak een URL met het volgende patroon volgen: `https://<entity-id>.accounts.ondemand.com/admin`.

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met de [SAP Cloud Platform Identity-verificatieclient ondersteuningsteam](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) deze waarde op te halen.

5. In de **SAML-certificaat voor ondertekening van** sectie **Metadata XML**. Sla het metagegevensbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. De SAML-asserties verwacht SAP Cloud Platform identiteitsverificatie toepassing in een specifieke indeling. Beheren van de waarden van deze kenmerken van de **gebruikerskenmerken** sectie op de pagina van de integratie van toepassingen. De volgende Schermafbeelding toont een voorbeeld van de indeling. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. Als uw toepassing SAP een kenmerk, zoals verwacht **firstName**, voeg de **firstName** kenmerk in de **gebruikerskenmerken** sectie. Deze optie is beschikbaar in de **eenmalige aanmelding** in het dialoogvenster van de **SAML-token kenmerken** in het dialoogvenster...

    a. Openen van de **kenmerk toevoegen** dialoogvenster, **toevoegen kenmerk**. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** typt u de kenmerknaam **firstName**.
    
    c. Van de **waarde** , selecteert u de kenmerkwaarde **user.givenname**.
    
    d. Selecteer **Ok**.

8. Selecteer de knop **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. In de **SAP Cloud Identity verificatie platformconfiguratie** sectie **configureren SAP Cloud Platform identiteitsverificatie** openen de **configureren aanmelding**venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![SAP Cloud Platform identiteit verificatie-configuratie](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Als u eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, gaat u naar de SAP Cloud Platform identiteitsverificatie-beheerconsole. De URL is het volgende patroon volgen: `https://<tenant-id>.accounts.ondemand.com/admin`. Lees de documentatie over SAP Cloud Platform identiteitsverificatie op [integratie met Microsoft Azure AD](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. Selecteer in de Azure-portal, de **opslaan** knop.

12. Ga door met de volgende alleen als u wilt toevoegen en eenmalige aanmelding inschakelen voor een andere SAP-toepassing. Herhaal de stappen in de sectie **toe te voegen SAP Cloud Platform identiteitsverificatie uit de galerie**.

13. In de Azure-portal op de **SAP Cloud Platform identiteitsverificatie** toepassing Integratiepagina **gekoppelde aanmelding**.

    ![Gekoppelde aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. De configuratie op te slaan.

>[!NOTE] 
>De nieuwe toepassing maakt gebruik van de configuratie voor één aanmelding van de vorige SAP-toepassing. Zorg ervoor dat u dezelfde zakelijke id-Providers gebruiken in de beheerconsole identiteitsverificatie voor SAP Cloud-Platform.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Een SAP Cloud Platform identiteitsverificatie testgebruiker maken

U hoeft niet te maken van een gebruiker in identiteitsverificatie voor SAP Cloud-Platform. Gebruikers die zich in het archief van de Azure AD-gebruiker kunnen de functionaliteit van eenmalige aanmelding gebruiken.

SAP Cloud Platform identiteitsverificatie ondersteunt de optie identiteitsfederatie. Deze optie kunt de toepassing om te controleren of gebruikers die zijn geverifieerd door de provider van de zakelijke identiteit in het archief van SAP Cloud Platform identiteit gebruikersverificatie bestaan. 

De optie identiteitsfederatie is standaard uitgeschakeld. Als identiteitsfederatie is ingeschakeld, kan alleen de gebruikers die worden geïmporteerd in SAP Cloud Platform identiteitsverificatie toegang krijgen tot de toepassing. 

Zie voor meer informatie over het in- of uitschakelen van identiteitsfederatie met SAP Cloud Platform identiteitsverificatie 'Identiteit Federation met SAP Cloud Platform identiteit Authentication inschakelen' in [identiteitsfederatie configureren met de Archief van SAP Cloud Platform identiteit gebruikersverificatie](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SAP Cloud Platform-ID-verificatie.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen identiteitsverificatie voor SAP Cloud-Platform, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal en gaat u naar de directoryweergave. Ga vervolgens naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP Cloud Platform identiteitsverificatie**.

    ![De koppeling SAP Cloud Platform identiteitsverificatie in de lijst met toepassingen](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de gebruikerslijst.

6. Klik op de **Selecteer** knop in de **gebruikers en groepen** in het dialoogvenster.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de tegel SAP Cloud Platform identiteitsverificatie in het deelvenster toegang selecteert, krijgen u automatisch aangemeld bij uw toepassing identiteitsverificatie voor SAP Cloud Platform.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_203.png

