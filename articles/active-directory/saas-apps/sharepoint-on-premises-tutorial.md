---
title: 'Zelfstudie: Azure Active Directory-integratie met SharePoint on-premises | Microsoft Docs'
description: Informatie over de configuratie van eenmalige aanmelding tussen Azure Active Directory en SharePoint on-premises.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dca14f4c74c130145ba6792d2a3ee5c43f3c72b0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57874793"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Zelfstudie: Azure Active Directory-integratie met SharePoint on-premises

In deze zelfstudie leert u hoe u SharePoint on-premises integreert met Azure Active Directory (Azure AD).
De integratie van SharePoint on-premises met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang heeft tot SharePoint on-premises.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij SharePoint on-premises (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor de configuratie van Azure AD-integratie met SharePoint on-premises, hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op SharePoint on-premises waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SharePoint on-premises ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>SharePoint on-premises toevoegen uit de galerie

Voor het configureren van de integratie van SharePoint on-premises in Azure AD, moet u SharePoint on-premises uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om SharePoint on-premises uit de galerie toe te voegen:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SharePoint on-premises**, selecteer in het deelvenster met resultaten **SharePoint on-premises** en klik vervolgens op **Toevoegen** om de toepassing toe te voegen.

    ![SharePoint on-premises in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u Azure AD-eenmalige aanmelding met SharePoint on-premises op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SharePoint on-premises tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met SharePoint on-premises wilt configureren en testen, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[SharePoint on-premises-eenmalige aanmelding configureren](#configure-sharepoint-on-premises-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[Een Azure AD-beveiligingsgroep maken in de Azure-Portal](#create-an-azure-ad-security-group-in-the-azure-portal)**  : als u wilt een nieuwe beveiligingsgroep in Azure AD voor eenmalige aanmelding inschakelen.
5. **[Toegang verlenen tot SharePoint on-premises beveiligingsgroep](#grant-access-to-sharepoint-on-premises-security-group)**  -toegangsmachtigingen voor een bepaalde groep naar Azure AD.
6. **[De Azure AD-beveiligingsgroep in Azure Portal toewijzen](#assign-the-azure-ad-security-group-in-the-azure-portal)**  - groep toewijzen aan Azure AD voor verificatie.
7. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor de configuratie van Azure AD-eenmalige aanmelding met SharePoint on-premises, moet u de volgende stappen uitvoeren:

1. Selecteer in [Azure Portal](https://portal.azure.com/) op de integratiepagina van de **SharePoint on-premises**-toepassing de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding bij het SharePoint on-premises-domein en -URL's](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `urn:sharepoint:federation`

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<YourSharePointServerURL>/_trust/default.aspx`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met het [klantondersteuningsteam van SharePoint on-premises](https://support.office.com/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

    > [!Note]
    > Noteer het bestandspad waarnaar u het certificaatbestand hebt gedownload. U hebt dit later nodig in het PowerShell-script voor de configuratie.

6. Kopieer in de sectie **SharePoint on-premises instellen** de juiste URL('s) overeenkomstig wat u nodig hebt. Gebruik voor de **URL van de service Eenmalige aanmelding** een waarde met de volgende notatie: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ is de tenant-id van het Azure AD-abonnement.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    > [!NOTE]
    > De SharePoint on-premises-toepassing maakt gebruik van een SAML 1.1-token, zodat Azure AD een WS Fed-aanvraag verwacht van de SharePoint-server en na verificatie het SAML 1.1-token. uitgeeft.

### <a name="configure-sharepoint-on-premises-single-sign-on"></a>SharePoint on-premises-eenmalige aanmelding configureren

1. Meld u in een ander browservenster als beheerder aan bij de bedrijfssite van SharePoint on-premises.

2. **Een nieuwe vertrouwde id-provider in SharePoint Server 2016 configureren**

    Meld u aan bij de server voor SharePoint Server 2016 en open de SharePoint 2016 Management Shell. Vul de waarden in voor $realm (de id-waarde in de sectie SharePoint on-premises-domein en -URL in Azure Portal), $wsfedurl (URL van de service Eenmalige aanmelding) en $filepath (bestandspad waarnaar u het certificaatbestand hebt gedownload) van Azure Portal en voer de volgende opdrachten uit om een nieuwe vertrouwde id-provider te configureren.

    > [!TIP]
    > Zie [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps) als u geen ervaring hebt met PowerShell of meer wilt weten over hoe PowerShell werkt.

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $map5 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Voer vervolgens deze stappen uit om de vertrouwde id-provider voor uw toepassing in te schakelen:

    a. Navigeer in Centraal beheer naar **Webtoepassing beheren** en selecteer de webtoepassing die u wilt beveiligen met Azure AD.

    b. Klik in het lint op **Verificatieproviders** en kies de zone die u wilt gebruiken.

    c. Selecteer **Vertrouwde id-provider** en selecteer de id-provider die u zojuist hebt geregistreerd met de naam *AzureAD*.

    d. Selecteer in de instelling Aanmeldingspagina-URL **Aangepaste aanmeldingspagina** en geef de waarde /_trust/ op.

    e. Klik op **OK**.

    ![Uw verificatieprovider configureren](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Sommige externe gebruikers kunnen deze integratie van eenmalige aanmelding niet gebruiken, doordat hun UPN een niet-herkende waarde bevat, zoals `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. Binnenkort kunnen klanten hun app configureren voor de verwerking van de UPN, afhankelijk van het type gebruiker. Hierna moeten alle gastgebruikers eenmalige aanmelding probleemloos kunnen gebruiken als werknemers van de organisatie.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Een Azure AD-beveiligingsgroep maken in Azure Portal

1. Klik op **Azure Active Directory > alle groepen**.

    ![Een Azure AD-beveiligingsgroep maken](./media/sharepoint-on-premises-tutorial/allgroups.png)

2. Klik op **nieuwe groep**:

    ![Een Azure AD-beveiligingsgroep maken](./media/sharepoint-on-premises-tutorial/newgroup.png)

3. Vul in **groepstype**, **groepsnaam**, **beschrijving van de groep**, **lidmaatschapstype**. Klik op de pijl om door te selecteren leden, en vervolgens zoeken of klik op het lid dat u wilt toevoegen aan de groep. Klik op **Selecteer** om toe te voegen van de geselecteerde leden, en klik vervolgens op **maken**.

    ![Een Azure AD-beveiligingsgroep maken](./media/sharepoint-on-premises-tutorial/addingmembers.png)

    > [!NOTE]
    > Als u wilt toewijzen Azure Active Directory-beveiligingsgroepen aan SharePoint on-premises, wordt het nodig zijn om te installeren en configureren van worden [AzureCP](https://yvand.github.io/AzureCP/) in de on-premises SharePoint-farm of ontwikkelen en een alternatieve aangepaste claims configureren provider voor SharePoint.  Zie de sectie voor meer informatie over aan het einde van het document voor het maken van uw eigen aangepaste claims-provider, als u AzureCP niet gebruikt.

### <a name="grant-access-to-sharepoint-on-premises-security-group"></a>Toegang verlenen tot SharePoint on-premises beveiligingsgroep

**Configureer beveiligingsgroepen en machtigingen op de App-registratie**

1. Selecteer in de Azure portal, **Azure Active Directory**en selecteer vervolgens **App-registraties**.

    ![De blade Bedrijfstoepassingen](./media/sharepoint-on-premises-tutorial/appregistrations.png)

2. Typ in het zoekvak en selecteer **SharePoint on-premises**.

    ![SharePoint on-premises in de lijst met resultaten](./media/sharepoint-on-premises-tutorial/appsearch.png)

3. Klik op **Manifest**.

    ![Manifest van de optie](./media/sharepoint-on-premises-tutorial/manifest.png)

4. Wijzigen `groupMembershipClaims`: `NULL`naar `groupMembershipClaims`: `SecurityGroup`. Klik op Opslaan

    ![Manifest bewerken](./media/sharepoint-on-premises-tutorial/manifestedit.png)

5. Klik op **instellingen**, klikt u op **vereiste machtigingen**.

    ![Vereiste machtigingen](./media/sharepoint-on-premises-tutorial/settings.png)

6. Klik op **toevoegen** en vervolgens **Select an API**.

    ![API-toegang](./media/sharepoint-on-premises-tutorial/required_permissions.png)

7. Voegt **Windows Azure Active Directory** en **Microsoft Graph API**, maar het is alleen mogelijk om een tegelijk te selecteren.

    ![API selecteren](./media/sharepoint-on-premises-tutorial/permissions.png)

8. Selecteer Windows Azure Active Directory, mapgegevens lezen controleren en klik op selecteren. Ga terug en toevoegen van Microsoft Graph en selecteert u gegevens lezen-directory, evenals.  Klik op selecteren en klik op gereed.

    ![Toegang inschakelen](./media/sharepoint-on-premises-tutorial/readpermission.png)

9. Nu onder instellingen vereist, klik op **machtigingen verlenen** en vervolgens klikt u op Ja om machtigingen verlenen.

    ![Machtigingen verlenen](./media/sharepoint-on-premises-tutorial/grantpermission.png)

    > [!NOTE]
    > Controleer onder meldingen om te bepalen als de machtigingen met succes zijn verleend.  Als dat niet het geval is, klikt u vervolgens de AzureCP werkt niet goed en het niet mogelijk SharePoint on-premises configureren met Azure Active Directory-beveiligingsgroepen.

10. Configureer de AzureCP op de on-premises SharePoint-farm of een alternatieve aangepaste claims provider-oplossing.  In dit voorbeeld gebruiken we AzureCP.

    > [!NOTE]
    > Houd er rekening mee dat AzureCP niet een Microsoft-product is of wordt ondersteund door technische ondersteuning van Microsoft. Downloaden, installeren en configureren van AzureCP op de on-premises SharePoint-farm per https://yvand.github.io/AzureCP/ 

11. **Toegang verlenen tot de Azure Active Directory-beveiligingsgroep in de on-premises SharePoint** :-de groepen, moeten toegangsrechten hebben voor de toepassing in SharePoint op-permise.  Gebruik de volgende stappen uit om in te stellen de machtigingen voor toegang tot de web-App.

12. In Centraal beheer, klikt u op beheer van toepassingen, web-apps beheren en selecteert u de web-App te activeren van het lint en gebruikersbeleid op.

    ![Centrale beheersite](./media/sharepoint-on-premises-tutorial/centraladministration.png)

13. Klik onder beleid voor Web-App en op gebruikers toevoegen en selecteer vervolgens de zone, klik op volgende.  Klik op het adresboek.

    ![Beleid voor webtoepassing](./media/sharepoint-on-premises-tutorial/webapp-policy.png)

14. Vervolgens zoekt en de beveiligingsgroep Azure Active Directory toevoegen en klik op OK.

    ![Beveiligingsgroep toe te voegen](./media/sharepoint-on-premises-tutorial/securitygroup.png)

15. Selecteer de machtigingen en klik op voltooien.

    ![Beveiligingsgroep toe te voegen](./media/sharepoint-on-premises-tutorial/permissions1.png)

16. Zie onder beleid voor Web-App, de Azure Active Directory-groep is toegevoegd.  De groepclaim bevat de Azure Active Directory Security groepsobject-Id voor de gebruikersnaam.

    ![Beveiligingsgroep toe te voegen](./media/sharepoint-on-premises-tutorial/addgroup.png)

17. Blader naar de SharePoint-siteverzameling en voeg de groep, worden er ook toe. Klik op de Site-instellingen en klik vervolgens op machtigingen voor de Site en machtigingen verlenen.  Zoeken naar de claim van de rol van de groep, het machtigingsniveau toewijzen en klik op delen.

    ![Beveiligingsgroep toe te voegen](./media/sharepoint-on-premises-tutorial/grantpermission1.png)

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Eén vertrouwde id-provider voor meerdere webtoepassingen configureren

De configuratie werkt voor één webtoepassing. Als u dezelfde vertrouwde id-provider wilt gebruiken voor meerdere webtoepassingen, moet u aanvullende configuratiestappen uitvoeren. Stel, u hebt een webtoepassing uitgebreid voor gebruik van de URL `https://portal.contoso.local` en u wilt de gebruikers ook verifiëren voor `https://sales.contoso.local`. Hiervoor moet de id-provider worden bijgewerkt om te voldoen aan de WReply-parameter en moet de toepassingsregistratie in Azure AD worden bijgewerkt om een antwoord-URL toe te voegen.

1. Open in Azure Portal de Azure AD-directory. Klik op **App-registraties** en klik vervolgens op **Alle toepassingen weergeven**. Klik op de toepassing die u eerder hebt gemaakt (SharePoint SAML-integratie).

2. Klik op **Instellingen**.

3. Klik op de blade Instellingen op **Antwoord-URL's**.

4. Voeg de URL toe voor de extra webtoepassing en voeg `/_trust/default.aspx` toe aan de URL (bijvoorbeeld `https://sales.contoso.local/_trust/default.aspx`) en klik vervolgens op **Opslaan**.

5. Open op de SharePoint-server de **SharePoint 2016 Management Shell** en voer de volgende opdrachten uit met de naam van de vertrouwde id-tokenuitgever die u eerder hebt gebruikt.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
6. Ga in Centraal beheer naar de webtoepassing en schakel de bestaande vertrouwde id-provider in. Configureer de URL van de aanmeldingspagina ook als een aangepaste aanmeldingspagina `/_trust/`.

7. Klik in Centraal beheer op de webtoepassing en kies **Gebruikersbeleid**. Voeg een gebruiker toe met de juiste machtigingen, zoals eerder beschreven.

### <a name="fixing-people-picker"></a>De personenkiezer herstellen

Gebruikers kunnen zich nu aanmelden bij SharePoint 2016 met identiteiten van Azure AD, maar er zijn nog steeds mogelijkheden voor verbetering van de gebruikerservaring. Wanneer u bijvoorbeeld zoekt naar een gebruiker, worden meerdere zoekresultaten weergegeven in de personenkiezer. Er is een zoekresultaat voor elk van de 3 claimtypen die zijn gemaakt in de claimtoewijzing. Als u een gebruiker wilt kiezen met de personenkiezer, moet u de gebruikersnaam exact typen en het resultaat van de **naam**claim kiezen.

![Zoekresultaten van claims](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Er wordt geen validatie uitgevoerd op de waarden die u zoekt, wat tot spelfouten kan leiden of ervoor kan zorgen dat gebruikers per ongeluk het verkeerde claimtype kiezen voor toewijzing, zoals de claim **Achternaam**. Hierdoor hebben gebruikers mogelijk geen toegang tot resources.

De open-sourceoplossing [AzureCP](https://yvand.github.io/AzureCP/) biedt een aangepaste claimprovider voor SharePoint 2016. Deze maakt gebruik van Azure AD Graph om te corrigeren wat gebruikers invoeren en validatie uit te voeren. U vindt hierover meer informatie op [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-security-group-in-the-azure-portal"></a>Toewijzen van de Azure AD-beveiligingsgroep in Azure Portal

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **SharePoint on-premises**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **SharePoint on-premises** in de lijst met toepassingen.

    ![De koppeling van SharePoint on-premises in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de **gebruiker toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Zoeken naar de beveiligingsgroep die u wilt gebruiken en klik vervolgens op de groep toe te voegen aan de sectie selecteren leden. Klik op **Selecteer**, klikt u vervolgens op **toewijzen**.

    ![Beveiligingsgroep zoeken](./media/sharepoint-on-premises-tutorial/securitygroup1.png)

    > [!NOTE]
    > Controleer de meldingen in de menubalk om te worden geïnformeerd dat de groep is toegewezen aan de Enterprise-toepassing in Azure Portal.

### <a name="create-sharepoint-on-premises-test-user"></a>Een testgebruiker maken voor SharePoint on-premises

In deze sectie maakt u in SharePoint on-premises een gebruiker met de naam Britta Simon. Neem contact op met het  [ondersteuningsteam van SharePoint on-premises](https://support.office.com/) om de gebruikers toe te voegen aan het SharePoint on-premises-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel SharePoint on-premises in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van SharePoint on-premises waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)