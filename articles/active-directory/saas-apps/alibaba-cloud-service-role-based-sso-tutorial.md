---
title: 'Zelfstudie: Azure Active Directory-integratie met Alibaba Cloud Service (Role-based SSO) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Alibaba Cloud Service (Role-based SSO).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3667841e-acfc-4490-acf5-80d9ca3e71e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d4b154b8ce55c381f1398c696bc439067dccfab
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785750"
---
# <a name="tutorial-azure-active-directory-integration-with-alibaba-cloud-service-role-based-sso"></a>Zelfstudie: Azure Active Directory-integratie met Alibaba Cloud Service (Role-based SSO)

In deze zelfstudie leert u hoe u Alibaba Cloud Service (Role-based SSO) integreren met Azure Active Directory (Azure AD).
Alibaba Cloud Service (Role-based SSO) integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Alibaba Cloud Service (Role-based SSO heeft).
* U kunt uw gebruikers worden automatisch aangemeld bij Alibaba Cloud Service (Role-based SSO) inschakelen (Single Sign-On) met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Alibaba Cloud Service (Role-based SSO), moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Alibaba Cloud Service (Role-based SSO) ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Alibaba Cloud Service (Role-based SSO) ondersteunt **IDP** gestart door SSO

## <a name="adding-alibaba-cloud-service-role-based-sso-from-the-gallery"></a>Alibaba Cloud Service (Role-based SSO) uit de galerie toevoegen

Voor het configureren van de integratie van Alibaba Cloud Service (Role-based SSO) in Azure AD, moet u Alibaba Cloud Service (Role-based SSO) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Alibaba Cloud Service (Role-based SSO) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Alibaba Cloud Service (Role-based SSO)**, selecteer **Alibaba Cloud Service (Role-based SSO)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Alibaba-Cloudservice (Role-based SSO) in de lijst met resultaten](common/search-new-app.png)

5. Op de **Alibaba Cloud Service (Role-based SSO)** pagina, klikt u op **eigenschappen** in het navigatiedeelvenster aan de linkerkant en kopieer de **object-ID** en sla deze op uw computer later gebruik.

    ![Eigenschappen van configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/Properties.png)
    
## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Alibaba Cloud Service (Role-based SSO) op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Cloudservice Alibaba (Role-based SSO) tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met Alibaba Cloud Service (Role-based SSO) testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Role-Based Single Sign-On configureren in de Cloudservice Alibaba](#configure-role-based-single-sign-on-in-alibaba-cloud-service)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van eenmalige aanmelding Alibaba Cloud Service (Role-based SSO)](#configure-alibaba-cloud-service-role-based-sso-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Alibaba Cloud Service (Role-based SSO)](#create-alibaba-cloud-service-role-based-sso-test-user)**  : als u wilt een equivalent van Britta Simon Alibaba Cloud Service (Role-based SSO) die is gekoppeld aan de Azure AD-weergave van de gebruiker nodig hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Alibaba Cloud Service (Role-based SSO), moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Alibaba Cloud Service (Role-based SSO)** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    >[!NOTE]
    >U ontvangt de metagegevens van de serviceprovider van dit [URL](https://signin.alibabacloud.com/saml-role/sp-metadata.xml)

    a. Klik op **Metagegevensbestand uploaden**.

    ![image](common/upload-metadata.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.

    ![image](common/browse-upload-metadata.png)

    c. Zodra het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld in de sectie-tekstvak Alibaba Cloud Service (Role-based SSO):

    ![image](common/idp-intiated.png)

    > [!Note]
    > Als de **id** en **antwoord-URL** waarden niet automatisch ingevuld ophalen en vervolgens de vul de waarden handmatig uit volgens uw behoeften.

5. Alibaba Cloud Service (Role-based SSO)-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op **bewerken** pictogram openen **gebruikerskenmerken** dialoogvenster.

    ![image](common/edit-attribute.png)

6. Bovendien met bovenstaande Alibaba Cloud Service (Role-based SSO)-toepassing wordt verwacht dat enkele meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name | Naamruimte | Bronkenmerk|
    | ---------------| ------------| --------------- |
    | Rol | https:\//www.aliyun.com/SAML-Role/Attribute | user.assignedroles |
    | RoleSessionName | https:\//www.aliyun.com/SAML-Role/Attribute | user.userprincipalname |

    > [!NOTE]
    > Klik [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) als u wilt weten hoe u **rollen** in Azure AD moet configureren

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

8. Op de **Alibaba Cloud Service (Role-based SSO) instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-role-based-single-sign-on-in-alibaba-cloud-service"></a>Op basis van rollen Single Sign-On in Alibaba Cloud Service configureren

1. Aanmelden bij de Alibaba Cloud [RAM-geheugen console](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fram.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.8.7d904e167h6Yg9) met behulp van Account1.

2. Selecteer in het navigatiedeelvenster aan de linkerkant **SSO**.

3. Op de **rol gebaseerde SSO** tabblad **IdP maken**.

4. Voer op de weergegeven pagina `AAD` in het veld id-provider, voer een beschrijving in de **Opmerking** veld, klikt u op **uploaden** uploaden van het bestand met federatieve metagegevens die u hebt gedownload voordat en klikt u op  **OK**.

5. Nadat de id-provider is gemaakt, klikt u op **RAM-geheugen rol maken**.

6. In de **RAM-geheugen rolnaam** veld `AADrole`, selecteer `AAD` uit de **Selecteer IdP** vervolgkeuzelijst lijst en klik op OK.

    >[!NOTE]
    >Indien nodig kunt u de machtiging om de rol te verlenen. Na het maken van de id-provider en de bijbehorende functie, wordt u aangeraden dat u de ARNs van de id-provider en de rol voor later gebruik opslaan. U kunt de ARNs op de pagina van de gegevens van id-provider en de functiepagina-informatie verkrijgen.

7. De rol Alibaba Cloud RAM-geheugen (AADrole) koppelen aan de Azure AD-gebruiker (u2): Als u wilt koppelen het RAM-rol met de Azure AD-gebruiker, moet u een rol maken in Azure AD via de volgende stappen:

    a. Meld u aan bij de [Azure AD Graph Explorer](https://developer.microsoft.com/graph/graph-explorer?spm=a2c63.p38356.879954.9.7d904e167h6Yg9).

    b. Klik op **wijzigingsmachtigingen** om op te halen van de vereiste machtigingen voor het maken van een rol.

    ![Graph-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/graph01.png)

    c. Selecteer de volgende machtigingen in de lijst en klikt u op **wijzigingsmachtigingen**, zoals wordt weergegeven in de volgende afbeelding.

    ![Graph-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/graph02.png)

    >[!NOTE]
    >Nadat de machtigingen zijn verleend, meld u aan bij de Graph Explorer het opnieuw.

    d. Selecteer op de pagina Graph Explorer **ophalen** van de eerste vervolgkeuzelijst en **Bèta** van de tweede vervolgkeuzelijst. Voer `https://graph.microsoft.com/beta/servicePrincipals` in het veld naast de vervolgkeuzelijsten en op **Query uitvoeren**.

    ![Graph-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/graph03.png)

    >[!NOTE]
    >Als u meerdere directory's gebruikt, kunt u `https://graph.microsoft.com/beta/contoso.com/servicePrincipals` in het veld van de query.

    e. In de **antwoord Preview** sectie, halen de appRoles-eigenschap van de Service-Principal voor later gebruik.

    ![Graph-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/graph05.png)

    >[!NOTE]
    >U kunt de eigenschap appRoles vinden door te voeren `https://graph.microsoft.com/beta/servicePrincipals/<objectID>` in het veld van de query. Houd er rekening mee dat de `objectID` is de object-ID die u hebt gekopieerd uit de Azure AD **eigenschappen** pagina.

    f. Ga terug naar de Graph Explorer, de methode wijzigen **ophalen** naar **PATCH**, plak de volgende inhoud in de **aanvraagtekst** uit en klikt u op **Query uitvoeren** :
    ```
    { 
    "appRoles": [
        { 
        "allowedMemberTypes":[
            "User"
        ],
        "description": "msiam_access",
        "displayName": "msiam_access",
        "id": "41be2db8-48d9-4277-8e86-f6d22d35****",
        "isEnabled": true,
        "origin": "Application",
        "value": null
        },
        { "allowedMemberTypes": [
            "User"
        ],
        "description": "Admin,AzureADProd",
        "displayName": "Admin,AzureADProd",
        "id": "68adae10-8b6b-47e6-9142-6476078cdbce",
        "isEnabled": true,
        "origin": "ServicePrincipal",
        "value": "acs:ram::187125022722****:role/aadrole,acs:ram::187125022722****:saml-provider/AAD"
        }
    ]
    }
    ```
    > [!NOTE]
    > De `value` is de ARNs van de id-provider en de rol die u hebt gemaakt in de console RAM-geheugen. Hier kunt u meerdere rollen toevoegen, indien nodig. Azure AD wordt de waarde van deze rollen worden verzonden als de waarde van de claim in SAML-antwoord. Echter, kunt u alleen nieuwe rollen na toevoegen de `msiam_access` deel uitmaakt van de patch-bewerking. Vloeiende het proces voor het maken, raden wij aan dat u de aanmaak van een ID's, zoals de GUID-Generator voor het genereren van id's in realtime.

    g. Nadat de Service-Principal is gevuld met de vereiste functieservices, koppelt u de rol met de Azure AD-gebruiker (u2) met de volgende stappen van **toewijzen van de Azure AD-testgebruiker** gedeelte van de zelfstudie.

### <a name="configure-alibaba-cloud-service-role-based-sso-single-sign-on"></a>Alibaba Cloud Service (Role-based SSO) eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **Alibaba Cloud Service (Role-based SSO)** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** en de juiste URL's opgehaald uit Azure portal om te [ Alibaba Cloud Service (Role-based SSO) ondersteuning voor team](https://www.aliyun.com/service/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veld, typt u `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Alibaba Cloud Service (Role-based SSO).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Alibaba Cloud Service (Role-based SSO)**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Alibaba Cloud Service (Role-based SSO)**.

    ![De koppeling Alibaba Cloud Service (Role-based SSO) in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Op de **gebruikers en groepen** tabblad, selecteert u u2 in de lijst met gebruikers en klikt u op **Selecteer**. Klik vervolgens op **toewijzen**.

    ![Test-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/test01.png)

6. Bekijk de toegewezen rol en Alibaba Cloud Service (Role-based SSO) testen.

    ![Test-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/test02.png)

    >[!NOTE]
    >Nadat u de gebruiker (u2) toegewezen, wordt de rol automatisch gekoppeld aan de gebruiker. Als u meerdere rollen hebt gemaakt, moet u de juiste rol koppelen aan de gebruiker indien nodig. Als u implementeren op basis van rollen eenmalige aanmelding van Azure AD met meerdere Alibaba Cloud-accounts wilt, herhaalt u de voorgaande stappen.

### <a name="create-alibaba-cloud-service-role-based-sso-test-user"></a>Testgebruiker Alibaba Cloud Service (Role-based SSO) maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Alibaba Cloud Service (Role-based SSO). Werken met [Alibaba Cloud Service (Role-based SSO) ondersteuning voor team](https://www.aliyun.com/service/) om toe te voegen de gebruikers in het Alibaba Cloud Service (Role-based SSO)-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

Nadat de voorgaande configuraties zijn voltooid, kunt u de Alibaba Cloud Service (Role-based SSO) testen door de volgende stappen:

1. In de Azure-portal, gaat u naar de **Alibaba Cloud Service (Role-based SSO)** weergeeft, schakelt **eenmalige aanmelding**, en klikt u op **Test**.

    ![Test-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/test03.png)

2. Klik op **Aanmelden als huidige gebruiker**.

    ![Test-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/test04.png)

3. Selecteer op de pagina voor het account selecteren, u2.

    ![Test-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/test05.png)

4. De volgende pagina wordt weergegeven, waarmee wordt aangegeven dat op basis van rollen eenmalige aanmelding voltooid is.

    ![Test-configuratie](./media/alibaba-cloud-service-role-based-sso-tutorial/test06.png)

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

