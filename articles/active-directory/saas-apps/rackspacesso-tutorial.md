---
title: 'Zelfstudie: Azure Active Directory-integratie met Rackspace SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Rackspace SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 36b398be-2f7e-4ce8-9031-53587299bc4a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.openlocfilehash: fd420ea3fc4faae7fe4510a72204d71acaa3549a
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65890786"
---
# <a name="tutorial-azure-active-directory-integration-with-rackspace-sso"></a>Zelfstudie: Azure Active Directory-integratie met Rackspace SSO

In deze zelfstudie leert u hoe u Rackspace SSO integreren met Azure Active Directory (Azure AD).
Rackspace SSO integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Rackspace SSO heeft.
* U kunt uw gebruikers worden automatisch aangemeld bij Rackspace SSO (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Rackspace SSO, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Rackspace SSO ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor SSO Rackspace **SP** gestart door SSO

## <a name="adding-rackspace-sso-from-the-gallery"></a>Rackspace SSO uit de galerie toe te voegen

Voor het configureren van de integratie van Rackspace SSO in Azure AD, moet u Rackspace SSO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Rackspace SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Rackspace SSO**, selecteer **Rackspace SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Rackspace SSO in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Rackspace SSO op basis van een testgebruiker met de naam **Britta Simon**.
Als u eenmalige aanmelding voor Rackspace, wordt de gebruikers Rackspace automatisch gemaakt de eerste keer dat ze zich aanmelden bij de portal Rackspace. 

Om te configureren en testen van Azure AD eenmalige aanmelding met Rackspace SSO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Rackspace SSO Single Sign-On](#configure-rackspace-sso-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Instellen van het kenmerk toewijzen in het Configuratiescherm Rackspace](#set-up-attribute-mapping-in-the-rackspace-control-panel)**  - Rackspace rollen toewijzen aan Azure AD-gebruikers.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Rackspace SSO, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Rackspace SSO** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, Upload het **Service Provider-bestand met metagegevens** die u kunt downloaden van de [URL](https://login.rackspace.com/federate/sp.xml) en voer de volgende stappen uit:

    a. Klik op **Metagegevensbestand uploaden**.

    ![image](common/upload-metadata.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.

    ![image](common/browse-upload-metadata.png)

    c. Zodra het bestand met metagegevens is geüpload, krijgen de vereiste URL's automatisch automatisch ingevuld.

    d. Typ een URL in het tekstvak **Aanmeldings-URL**: `https://login.rackspace.com/federate/`

    ![Rackspace SSO-domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)   

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

Dit bestand worden geüpload naar Rackspace voor het vullen van de vereiste configuratie-instellingen van identiteitsfederatie.

### <a name="configure-rackspace-sso-single-sign-on"></a>Configureren van eenmalige aanmelding Rackspace Single Sign-On

Het configureren van eenmalige aanmelding op **Rackspace SSO** aan clientzijde:

1. Zie de documentatie bij [een id-Provider toevoegen aan het Configuratiescherm](https://developer.rackspace.com/docs/rackspace-federation/gettingstarted/add-idp-cp/)
1. Dit leidt u door de stappen voor het:
    1. Een nieuwe id-Provider maken
    1. Geef een e-mailadresdomein dat gebruikers wordt gebruikt voor het identificeren van uw bedrijf bij het aanmelden.
    1. Upload de **federatieve metagegevens-XML** eerder hebt gedownload van het Configuratiescherm van Azure.

Hiermee correct configureert u de standaard SSO-instellingen die nodig zijn voor Azure en Rackspace om verbinding te maken.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Rackspace SSO.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Rackspace SSO**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Rackspace SSO**.

    ![De Rackspace SSO-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="set-up-attribute-mapping-in-the-rackspace-control-panel"></a>Instellen van het kenmerk toewijzen in het Configuratiescherm Rackspace

Rackspace maakt gebruik van een **kenmerk toewijzen van beleid** Rackspace rollen en groepen toewijzen aan uw gebruikers voor eenmalige aanmelding. De **kenmerk toewijzen van beleid** zet Azure AD-SAML-claims in de configuratie van gebruikersvelden Rackspace is vereist. Meer documentatie kunt u vinden in de Rackspace [kenmerk toewijzing basisbeginselen documentatie](https://developer.rackspace.com/docs/rackspace-federation/attribmapping-basics/). Enkele overwegingen:

* Als u toewijzen van verschillende niveaus van Rackspace toegang met behulp van Azure AD-groepen wilt, moet u de claim van groepen in Azure inschakelen **Rackspace SSO** instellingen voor eenmalige aanmelding. De **kenmerk toewijzen van beleid** wordt vervolgens gebruikt zodat deze overeenkomen met die groepen op de gewenste Rackspace rollen en groepen:

    ![De groepen claim instellingen](common/sso-groups-claim.png)

* Standaard verzendt Azure AD de UID van Azure AD-groepen in de SAML-claim, ten opzichte van de naam van de groep. Als u uw on-premises Active Directory naar Azure AD synchroniseert, hebt u echter de optie voor het verzenden van de werkelijke namen van de groepen:

    ![De groepen claim instellingen voor de naam van](common/sso-groups-claims-names.png)

Het volgende voorbeeld **kenmerk toewijzen van beleid** laat zien:
1. Als u de naam van de gebruiker van de Rackspace op de `user.name` SAML-claim. Een claim kan worden gebruikt, maar het meest worden gebruikt in dit instellen op een veld met de e-mailadres van de gebruiker is.
1. Instellen van de rollen Rackspace `admin` en `billing:admin` van een gebruiker die door die overeenkomt met een Azure AD-groep, door de naam of groep UID. Een *vervanging* van `"{0}"` in de `roles` veld wordt gebruikt en wordt vervangen door de resultaten van de `remote` expressies.
1. Met behulp van de `"{D}"` *standaard vervanging* om te laten Rackspace aanvullende SAML-velden ophalen door te zoeken naar standaard en bekende SAML-claims in de SAML-exchange.

```yaml
---
mapping:
    rules:
    - local:
        user:
          domain: "{D}"
          name: "{At(http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name)}"
          email: "{D}"
          roles:
              - "{0}"
          expire: "{D}"
      remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.microsoft.com/ws/2008/06/identity/claims/groups')='7269f9a2-aabb-9393-8e6d-282e0f945985') then ('admin', 'billing:admin') else (),
                if (mapping:get-attributes('http://schemas.microsoft.com/ws/2008/06/identity/claims/groups')='MyAzureGroup') then ('admin', 'billing:admin') else ()
              )
            multiValue: true
  version: RAX-1
```
> [!TIP]
> Zorg ervoor dat u een teksteditor die YAML-syntaxis valideert bij het bewerken van uw beleid-bestand.

Zie de Rackspace [kenmerk toewijzing basisbeginselen documentatie](https://developer.rackspace.com/docs/rackspace-federation/attribmapping-basics/) voor meer voorbeelden.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Rackspace SSO in het toegangsvenster, moet u worden automatisch aangemeld bij de Rackspace SSO waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

U kunt ook de **valideren** knop in de **Rackspace SSO** eenmalige aanmelding-instellingen:

   ![Eenmalige aanmelding knop valideren](common/sso-validate-sign-on.png)

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

