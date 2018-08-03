---
title: 'Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligde | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TOPdesk - veilig.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 26d20fbbe81cb4e1a526ea8c776462d9e851e688
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429767"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - beveiligde

In deze zelfstudie leert u hoe u TOPdesk - integreren met Azure Active Directory (Azure AD) beveiligen.

TOPdesk - beveiligde integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TOPdesk - veilig heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TOPdesk - beveiligde (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TOPdesk - beveiligen, moet u de volgende items:

- Een Azure AD-abonnement
- Een TOPdesk - veilige eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen TOPdesk - beveiligen vanuit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-topdesk---secure-from-the-gallery"></a>Toe te voegen TOPdesk - beveiligen vanuit de galerie
Het configureren van de integratie van TOPdesk - beveiligen in Azure AD, moet u TOPdesk add - uit de galerie aan de lijst met beheerde SaaS apps beveiligen.

**Om toe te voegen TOPdesk - beveiligen vanuit de galerie, voer de volgende stappen uit:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **TOPdesk - beveiligde**, selecteer **TOPdesk - beveiligde** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TOPdesk - beveiligen in de lijst met resultaten](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen op Azure AD eenmalige aanmelding met TOPdesk - beveiligd op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de gebruiker equivalent in TOPdesk - veilig is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TOPdesk - beveiligde tot stand worden gebracht.

In TOPdesk - beveiligen, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TOPdesk - beveiligen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maken van een TOPdesk - beveiligde testgebruiker](#create-a-topdesk---secure-test-user)**  : als u wilt een equivalent van Britta Simon in TOPdesk - beveiligde die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw TOPdesk - beveiligde toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met TOPdesk - beveiligen, voert u de volgende stappen uit:**

1. In de Azure-portal op de **TOPdesk - beveiligde** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

1. Op de **TOPdesk - beveiligd domein en URL's** sectie, voert u de volgende stappen uit:

    ![TOPdesk - beveiligd domein en URL's, eenmalige aanmelding informatie](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Antwoord-URL wordt verderop in de zelfstudie. Neem contact op met [TOPdesk - ondersteuningsteam voor beveiligde Client](http://www.topdesk.com/us/support) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/topdesk-secure-tutorial/tutorial_general_400.png)

1. Op de **TOPdesk - configuratie van beveiligde** sectie, klikt u op **TOPdesk configureren - beveiligde** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![TOPdesk - veilige configuratie](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
1. Meld u aan bij uw **TOPdesk - beveiligde** bedrijf site als beheerder.

1. In de **TOPdesk** menu, klikt u op **instellingen**.

    ![Instellingen voor](./media/topdesk-secure-tutorial/ic790598.png "instellingen")

1. Klik op **Login Settings**.

    ![Login Settings](./media/topdesk-secure-tutorial/ic790599.png "Login Settings")

1. Vouw de **Login Settings** menu en klik vervolgens op **algemene**.

    ![Algemene](./media/topdesk-secure-tutorial/ic790600.png "algemeen")

1. In de **Secure** sectie van de **SAML-aanmelding** configuratie sectie, voert u de volgende stappen uit:

    ![Technische instellingen](./media/topdesk-secure-tutorial/ic790855.png "technische instellingen")
   
    a. Klik op **downloaden** -bestand met de openbare metagegevens downloaden en lokaal opslaan op uw computer.
   
    b. Open het bestand met metagegevens en zoek vervolgens de **AssertionConsumerService** knooppunt.
    
    ![Assertion Consumentenservice](./media/topdesk-secure-tutorial/ic790856.png "Assertion Consumentenservice")
   
    c. Kopieer de **AssertionConsumerService** waarde, plak deze waarde in het antwoord-URL-tekstvak in **TOPdesk - beveiligd domein en URL's** sectie.

1. Als u wilt een certificaatbestand maakt, moet u de volgende stappen uitvoeren:
    
    ![Certificaat](./media/topdesk-secure-tutorial/ic790606.png "certificaat")
    
    a. Open het gedownloade metagegevensbestand vanuit Azure portal.

    b. Vouw de **RoleDescriptor** knooppunt dat is een **xsi: type** van **ingevoerd: ApplicationServiceType**.

    c. Kopieer de waarde van de **X509Certificate** knooppunt.

    d. Sla de gekopieerde **X509Certificate** lokaal op uw computer in een bestand.

1. In de **openbare** sectie, klikt u op **toevoegen**.
    
    ![Voeg](./media/topdesk-secure-tutorial/ic790607.png "toevoegen")

1. Op de **SAML-configuratie-assistent** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML-configuratie-assistent](./media/topdesk-secure-tutorial/ic790608.png "SAML-configuratie-assistent")
    
    a. Het uploaden van uw gedownloade metagegevensbestand van Azure-portal onder **Federatiemetagegevens**, klikt u op **Bladeren**.

    b. Het uploaden van het certificaatbestand onder **certificaat (RSA)**, klikt u op **Bladeren**.

    c. Het uploaden van het logobestand dat u hebt verkregen via het ondersteuningsteam TOPdesk onder **Logo pictogram**, klikt u op **Bladeren**.

    d. In de **gebruikersnaamkenmerk** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. In de **weergavenaam** tekstvak, typ een naam voor uw configuratie.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/topdesk-secure-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/topdesk-secure-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/topdesk-secure-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/topdesk-secure-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-topdesk---secure-test-user"></a>Een TOPdesk - beveiligde testgebruiker maken

Om in te schakelen van Azure AD-gebruikers zich aanmelden bij TOPdesk - beveiligen, ze moeten worden ingericht voor TOPdesk - veilig.  
In het geval van TOPdesk - veilige, inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **TOPdesk - beveiligde** bedrijf site als administrator.
1. Klik in het menu aan de bovenkant op **TOPdesk \> nieuw \> ondersteuningsbestanden \> Operator**.
   
    ![Operator](./media/topdesk-secure-tutorial/ic790610.png "Operator")

1. Op de **nieuwe Operator** dialoogvenster, voer de volgende stappen uit:
   
    ![Nieuwe Operator](./media/topdesk-secure-tutorial/ic790611.png "nieuwe Operator")
   
    a. Klik op de **algemene** tabblad.
   
    b. In de **achternaam** tekstvak, type de achternaam van de gebruiker, zoals **Simon**.
   
    c. Selecteer een **Site** voor het account in de **locatie** sectie.
   
    d. In de **aanmeldingsnaam** tekstvak van de **TOPdesk aanmelding** sectie, typt u een aanmeldingsnaam voor de gebruiker.
   
    e. Klik op **Opslaan**.

> [!NOTE]
> U kunt elke andere TOPdesk - hulpmiddelen voor het beveiligde gebruiker-account maken of geleverd door TOPdesk - beveiligde voor het inrichten van gebruikersaccounts van de AAD-API's gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TOPdesk - beveiligde.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen TOPdesk - beveiligen, voert u de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **TOPdesk - beveiligde**.

    ![De TOPdesk - beveiligde-koppeling in de lijst met toepassingen](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de TOPdesk - beveiligde tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw TOPdesk - beveiligde toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-secure-tutorial/tutorial_general_203.png

