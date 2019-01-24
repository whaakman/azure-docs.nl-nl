---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform-identiteitsverificatie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Cloud Platform-identiteitsverificatie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: ecd9df8efdcd6d7fd7da26ff5cf569ef7e63573f
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822056"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform-identiteitsverificatie

In deze zelfstudie leert u hoe u SAP Cloud Platform-identiteitsverificatie integreren met Azure Active Directory (Azure AD). SAP Cloud Platform-identiteitsverificatie wordt gebruikt als een proxy-id-provider voor toegang tot de SAP-toepassingen die gebruikmaken van Azure AD als de belangrijkste IdP.

Wanneer u SAP Cloud Platform-identiteitsverificatie met Azure AD integreert, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SAP-toepassingen heeft.
- U kunt uw gebruikers automatisch aanmelden bij het SAP-toepassingen met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie het artikel voor meer informatie over SaaS-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Cloud Platform-identiteitsverificatie, moet u de volgende items:

- Een Azure AD-abonnement
- Een enkel teken op ingeschakeld abonnement voor SAP Cloud Platform-identiteitsverificatie.

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Als u een proefversie Azure AD-omgeving, geen [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Cloud Platform-identiteitsverificatie in de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

Voordat u Duik in de technische details, is het essentieel om de concepten die u gaat kijken. De SAP Cloud Platform-identiteitsverificatie en Active Directory Federation Services kunt u eenmalige aanmelding tussen toepassingen of services die worden beveiligd door Azure AD (als een IdP) met SAP-toepassingen en services die worden beveiligd door SAP-Cloud implementeren Platform-identiteitsverificatie.

Op dit moment fungeert SAP Cloud Platform-identiteitsverificatie als een Proxy-id-Provider naar SAP-toepassingen. Azure Active Directory wordt op zijn beurt fungeert als de toonaangevende id-Provider in deze installatie. 

Het volgende diagram illustreert deze relatie:

![Het maken van een Azure AD-testgebruiker](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Met deze instelling is de SAP Cloud Platform-identiteitsverificatie-tenant geconfigureerd als een betrouwbare toepassing in Azure Active Directory. 

Alle SAP-toepassingen en services die u wilt beveiligen op deze manier worden vervolgens geconfigureerd in de SAP Cloud Platform-identiteitsverificatie-beheerconsole.

Daarom moet de machtiging voor het verlenen van toegang tot de SAP-toepassingen en services om te worden toegepast in SAP Cloud Platform-identiteitsverificatie (in plaats van Azure Active Directory).

Door SAP Cloud Platform-identiteitsverificatie te configureren als een toepassing via de Azure Active Directory Marketplace, moet u geen afzonderlijke claims of SAML-asserties ondertekend configureren.

>[!NOTE] 
>Momenteel wordt alleen Web-SSO is getest door beide partijen. De stromen die nodig voor App-naar-API of API naar API-communicatie zijn moeten werken, maar nog niet zijn getest. Ze worden tijdens de volgende activiteiten getest.
>

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>SAP Cloud Platform-identiteitsverificatie uit de galerie toevoegen
Voor het configureren van de integratie van de SAP Cloud Platform-identiteitsverificatie in Azure AD, moet u SAP Cloud Platform-identiteitsverificatie uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de SAP Cloud Platform-identiteitsverificatie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatievenster aan de linkerkant de **Azure Active Directory** pictogram. 

    ![De knop Azure Active Directory][1]

1. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Bedrijfstoepassingen][2]
    
1. Als u wilt de nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **SAP Cloud Platform-identiteitsverificatie**. 

1. Selecteer **SAP Cloud Platform-identiteitsverificatie** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** knop.

    ![SAP Cloud Platform-identiteitsverificatie in de lijst met resultaten](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAP Cloud Platform-identiteitsverificatie. U configureert en test deze met een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in de SAP Cloud Platform-identiteitsverificatie is. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de SAP Cloud Platform-identiteitsverificatie vast te stellen.

Geef de waarde in de SAP Cloud Platform-identiteitsverificatie, **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. Nu hebt u de koppeling tussen de twee gebruikers gemaakt.

Als u wilt configureren en Azure AD eenmalige aanmelding met SAP Cloud Platform-identiteitsverificatie testen, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
1. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. [Maak een testgebruiker SAP Cloud Platform-identiteitsverificatie](#create-an-sap-cloud-platform-identity-authentication-test-user) een equivalent van Britta Simon hebben in SAP Cloud Platform-identiteitsverificatie die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SAP Cloud Platform-identiteitsverificatie.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud Platform-identiteitsverificatie, voert u de volgende stappen uit:**

1. In de Azure-portal op de **SAP Cloud Platform-identiteitsverificatie** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. In de **eenmalige aanmelding** dialoogvenster onder **SAML gebaseerde aanmelding**, selecteer **modus** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_samlbase.png)

1. Als u wilt configureren van de toepassing in **IDP** modus gestart de **SAP Cloud Platform identiteit verificatiedomein en URL's** sectie, voert u de volgende stappen uit:  

    ![SAP Cloud Platform identiteit verificatiedomein en URL's, eenmalige aanmelding informatie](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url.png)

    a. In de **id** vak, typ een URL met het volgende patroon: `<IAS-tenant-id>.accounts.ondemand.com`

    b. In de **antwoord-URL** vak, typ een URL met het volgende patroon: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met de [SAP Cloud Platform-Identity-verificatieclient ondersteuningsteam](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) om deze waarden te verkrijgen. Als u de id-waarde niet begrijpt, lees de documentatie van SAP Cloud Platform-identiteitsverificatie [Tenant SAML 2.0-configuratie](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

1. Als u wilt configureren van de toepassing in **SP** gestart modus, selecteer **geavanceerde URL-instellingen weergeven**.

    ![SAP Cloud Platform identiteit verificatiedomein en URL's, eenmalige aanmelding informatie](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url1.png)

    In de **aanmelding URL** vak, typ een URL met het volgende patroon: `{YOUR BUSINESS APPLICATION URL}`.

    > [!NOTE]
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Gebruik uw specifieke zakelijke toepassing aanmeldings-URL. Neem contact op met de [SAP Cloud Platform-Identity-verificatieclient ondersteuningsteam](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) hebt u niet zeker.

1. In de **SAML-handtekeningcertificaat** sectie, selecteer **Metadata XML**. Sla het bestand met metagegevens op uw computer.

    ![De link om het certificaat te downloaden](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_certificate.png)

1. SAP Cloud Platform-identiteitsverificatie-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Beheren van de waarden van deze kenmerken van de **gebruikerskenmerken** sectie op de pagina van de integratie van toepassingen. De volgende Schermafbeelding toont een voorbeeld van de indeling. 

    ![Eenmalige aanmelding configureren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/attribute.png)

1. Als uw SAP-toepassing, zoals wordt verwacht een kenmerk dat **firstName**, toevoegen de **firstName** kenmerk in de **gebruikerskenmerken** sectie. Deze optie is beschikbaar in de **eenmalige aanmelding** in het dialoogvenster van de **SAML-token kenmerken** in het dialoogvenster...

    a. Om te openen de **kenmerk toevoegen** in het dialoogvenster, selecteer **kenmerk toevoegen**. 
    
    ![Eenmalige aanmelding configureren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** typt u de kenmerknaam **firstName**.
    
    c. Uit de **waarde** , selecteert u de waarde van het kenmerk **user.givenname**.
    
    d. Selecteer **OK**.

1. Selecteer de knop **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_400.png)

1. In de **SAP Cloud Platform-Identity-verificatieconfiguratie** sectie, selecteer **configureren SAP Cloud Platform-identiteitsverificatie** openen de **aanmeldingconfigureren**venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![SAP Cloud Platform identiteit verificatie-configuratie](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_configure.png) 

1. Als u eenmalige aanmelding configureren voor uw toepassing, gaat u naar de SAP Cloud Platform-identiteitsverificatie-beheerconsole. De URL heeft het volgende patroon: `https://<tenant-id>.accounts.ondemand.com/admin`. Lees de documentatie over SAP Cloud Platform-identiteitsverificatie op [integratie met Microsoft Azure AD](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

1. Selecteer in de Azure portal, de **opslaan** knop.

1. Ga door met de volgende alleen als u wilt toevoegen en eenmalige aanmelding inschakelen voor een andere SAP-toepassing. Herhaal de stappen in de sectie **toe te voegen SAP Cloud Platform-identiteitsverificatie in de galerie**.

1. In de Azure-portal op de **SAP Cloud Platform-identiteitsverificatie** toepassing integratie weergeeft, schakelt **gekoppelde Sign-on**.

    ![Gekoppelde aanmelding configureren](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

1. Sla de configuratie op.

>[!NOTE] 
>De nieuwe toepassing maakt gebruik van de configuratie voor eenmalige aanmelding van de vorige SAP-toepassing. Zorg ervoor dat u de dezelfde zakelijke id-Providers gebruiken in de SAP Cloud Platform-identiteitsverificatie-beheerconsole.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie op [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop Azure Active Directory](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster Gebruiker](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Maak een testgebruiker SAP Cloud Platform-identiteitsverificatie

U hoeft te maken van een gebruiker in de SAP Cloud Platform-identiteitsverificatie. Gebruikers die zich in het archief van de Azure AD-gebruiker kunnen de SSO-functionaliteit gebruiken.

De optie identiteitsfederatie biedt ondersteuning voor SAP Cloud Platform-identiteitsverificatie. Deze optie kunt de toepassing om te controleren of gebruikers die worden geverifieerd door de zakelijke id-provider in de gebruiker-archief van de SAP Cloud Platform-identiteitsverificatie bestaan. 

De optie identiteitsfederatie is standaard uitgeschakeld. Identiteitsfederatie is ingeschakeld, alleen de gebruikers die worden geïmporteerd in de SAP Cloud Platform-identiteitsverificatie toegang tot de toepassing. 

Zie voor meer informatie over het in- of uitschakelen van identiteitsfederatie met SAP Cloud Platform-identiteitsverificatie "Inschakelen identiteit Federation met SAP Cloud Platform-identiteitsverificatie" in [identiteitsfederatie configureren met de Gebruiker Store van de SAP Cloud Platform-identiteitsverificatie](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot de SAP Cloud Platform-identiteitsverificatie.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan SAP Cloud Platform-identiteitsverificatie toewijst, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal, opent u de weergave van toepassingen en gaat u naar de directoryweergave. Ga vervolgens naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SAP Cloud Platform-identiteitsverificatie**.

    ![De SAP Cloud Platform-identiteitsverificatie-koppeling in de lijst met toepassingen](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_app.png)  

1. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202]

1. Selecteer de knop **Add**. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster Toewijzing toevoegen][203]

1. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de gebruikerslijst.

1. Klik op de **Selecteer** knop in de **gebruikers en groepen** in het dialoogvenster.

1. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de SAP Cloud Platform-identiteitsverificatie-tegel in het toegangsvenster selecteert, krijgen u automatisch aangemeld bij uw toepassing SAP Cloud Platform-identiteitsverificatie.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/sapcloudauth-tutorial/tutorial_general_203.png
