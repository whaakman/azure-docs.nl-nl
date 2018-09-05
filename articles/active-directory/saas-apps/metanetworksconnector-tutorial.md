---
title: 'Zelfstudie: Azure Active Directory-integratie met metagegevens netwerken-Connector | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en metagegevens netwerken Connector.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeedes
ms.openlocfilehash: a3f40624e51ef287d70bed547eba7ec9e0882b0e
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669192"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Zelfstudie: Azure Active Directory-integratie met metagegevens netwerken-Connector

In deze zelfstudie leert u hoe u Meta netwerken Connector integreren met Azure Active Directory (Azure AD).

Metagegevens netwerken Connector integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot metagegevens netwerken-Connector heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Meta netwerken Connector (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met metagegevens netwerken-Connector, moet u de volgende items:

- Een Azure AD-abonnement
- Een Connector voor Meta-netwerken eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Metagegevens netwerken Connector uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Metagegevens netwerken Connector uit de galerie toe te voegen
Voor het configureren van de integratie van Meta-Connector voor netwerken in Azure AD, moet u metagegevens netwerken Connector uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Meta netwerken Connector uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 
    
    ![De Azure Active Directory-knop][1]
    
1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.
    
    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.
    
    ![De knop nieuwe toepassing][3]
    
1. Typ in het zoekvak **Meta netwerken Connector**, selecteer **Meta netwerken Connector** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.
    
    ![Connector voor META-netwerken in de lijst met resultaten](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met metagegevens netwerken Connector op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in metagegevens netwerken-Connector is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in metagegevens netwerken-Connector tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met metagegevens netwerken-Connector, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Meta netwerken Connector](#create-a-meta-networks-connector-test-user)**  : als u wilt een equivalent van Britta Simon in metagegevens netwerken Connector die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Connector voor Meta-netwerken.

**Voor het configureren van Azure AD eenmalige aanmelding met metagegevens netwerken-Connector, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Meta netwerken Connector** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_samlbase.png)

1. Op de **Meta netwerken Connector domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Metagegevens netwerken Connector domein en URL's eenmalige aanmelding informatie](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url.png)

    1. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`
    
    1. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`
    
1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Metagegevens netwerken Connector domein en URL's eenmalige aanmelding informatie](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url1.png)

    1. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`
    
    1. In de **Relaystatus** tekstvak, een URL met behulp van het volgende patroon: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`
    
    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke-id en antwoord-URL, en de aanmeldings-URL verderop in de zelfstudie zijn besproken.
    
1. Metagegevens netwerken Connector-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_attribute.png)
    
1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk | NAAMRUIMTE|
    | ---------------| --------------- | -------- |
    | Voornaam | User.givenName | |
    | Achternaam | User.surname | |
    | EmailAddress| User.mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | naam | User.userPrincipalName| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | telefoon | User.telephoneNumber | |

    1. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

        ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/tutorial_attribute_04.png)
    
        ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/tutorial_attribute_05.png)   
    
    1. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    1. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    1. In de **Namespace** tekstvak typt u de naamruimtewaarde die wordt weergegeven voor die rij.
    
    1. Klik op **Ok**
    
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.
    
    ![De downloadkoppeling certificaat](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_certificate.png)
    
1. Op de **Meta netwerken connectorconfiguratie** sectie, klikt u op **Meta netwerken-Connector configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**
    
    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_configure.png)
    
1. Klik op **opslaan** knop.
    
    ![Configureren van eenmalige aanmelding opslaan](./media/metanetworksconnector-tutorial/tutorial_general_400.png)
    
1. Open een nieuw tabblad in uw browser en meld u aan bij uw metagegevens netwerken Connector administrator-account.
    
    > [!NOTE]
    > Metagegevens netwerken-Connector is een beveiligd systeem. Voordat u toegang tot de portal moet u dus voor uw openbare IP-adres in de whitelist opgenomen over hun kant. Als u uw openbare IP-adres, volg de onderstaande koppeling opgegeven [hier](https://whatismyipaddress.com/). Verzenden van uw IP-adres aan de [Meta netwerken Connector Client-ondersteuningsteam](mailto:support@metanetworks.com) om op te halen van uw IP-adres in de whitelist opgenomen.
    
1. Ga naar **beheerder** en selecteer **instellingen**.
    
    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/configure3.png)
    
1. Zorg ervoor dat **Log internetverkeer** en **Force VPN MFA** zijn ingesteld op uitgeschakeld.
    
    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/configure1.png)
    
1. Ga naar **beheerder** en selecteer **SAML**.
    
    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/configure4.png)
    
1. Voer de volgende stappen uit op de **DETAILS** pagina:
    
    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/configure2.png)
    
    1. Kopie **SSO URL** waarde en plak deze in de **aanmelden URL** -tekstvak in de **Meta netwerken Connector domein en URL's** sectie.
    
    1. Kopie **ontvanger URL** waarde en plak deze in de **antwoord-URL** -tekstvak in de **Meta netwerken Connector domein en URL's** sectie.
    
    1. Kopie **doelgroep-URI (SP entiteits-ID)** waarde en plak deze in de **id (entiteits-ID)** -tekstvak in de **Meta netwerken Connector domein en URL's** sectie.
    
    1. De SAML inschakelen
    
1. Op de **algemene** tabblad. Voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/metanetworksconnector-tutorial/configure5.png)

    1. In de **Identity Provider aanmeldings-URL voor eenmalige**, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    1. In de **Identity Provider Issuer**, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    1. Open het gedownloade certificaat vanuit Azure portal in Kladblok, plak deze in de **X.509-certificaat** tekstvak.

    1. Schakel de **Just-in-Time inrichting**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.
    
![Maak een testgebruiker Azure AD][100]
    
**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.
    
    ![De Azure Active Directory-knop](./media/metanetworksconnector-tutorial/create_aaduser_01.png)
    
1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/metanetworksconnector-tutorial/create_aaduser_02.png)
    
1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.
    
    ![De knop toevoegen](./media/metanetworksconnector-tutorial/create_aaduser_03.png)
    
1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/metanetworksconnector-tutorial/create_aaduser_04.png)
    
    1. In de **naam** in het vak **BrittaSimon**.

    1. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.
    
    1. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.
    
    1. Klik op **Create**.
    
### <a name="create-a-meta-networks-connector-test-user"></a>Maak een testgebruiker Meta netwerken-Connector

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in metagegevens netwerken-Connector. Connector voor META-netwerken ondersteunt just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot metagegevens netwerken Connector als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Meta netwerken Connector Client-ondersteuningsteam](mailto:support@metanetworks.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot metagegevens netwerken Connector.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon met metagegevens netwerken-Connector, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.
    
    ![Gebruiker toewijzen][201]
    
1. Selecteer in de lijst met toepassingen, **Meta netwerken Connector**.
    
    ![De metagegevens netwerken Connector-koppeling in de lijst met toepassingen](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_app.png)  
    
1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.
    
    ![De koppeling 'Gebruikers en groepen'][202]
    
1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.
    
    ![Het deelvenster toewijzing toevoegen][203]
    
1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.
    
1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.
    
1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Meta-Connector voor netwerken in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Connector voor Meta-netwerken.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/metanetworksconnector-tutorial/tutorial_general_01.png
[2]: ./media/metanetworksconnector-tutorial/tutorial_general_02.png
[3]: ./media/metanetworksconnector-tutorial/tutorial_general_03.png
[4]: ./media/metanetworksconnector-tutorial/tutorial_general_04.png

[100]: ./media/metanetworksconnector-tutorial/tutorial_general_100.png

[200]: ./media/metanetworksconnector-tutorial/tutorial_general_200.png
[201]: ./media/metanetworksconnector-tutorial/tutorial_general_201.png
[202]: ./media/metanetworksconnector-tutorial/tutorial_general_202.png
[203]: ./media/metanetworksconnector-tutorial/tutorial_general_203.png

