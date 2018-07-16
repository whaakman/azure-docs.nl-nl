---
title: 'Zelfstudie: Azure Active Directory-integratie met SharePoint on-premises | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SharePoint on-premises.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: jeedes
ms.openlocfilehash: a792db670602f736489ee962df5078531e0a8e88
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050947"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Zelfstudie: Azure Active Directory-integratie met SharePoint on-premises

In deze zelfstudie leert u hoe u SharePoint on-premises integreren met Azure Active Directory (Azure AD).

SharePoint on-premises integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SharePoint on-premises heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SharePoint on-premises (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SharePoint on-premises, moet u de volgende items:

- Een Azure AD-abonnement
- Een SharePoint on-premises eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van SharePoint on-premises uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Toevoegen van SharePoint on-premises uit de galerie
Voor het configureren van de integratie van SharePoint on-premises in Azure AD, moet u SharePoint on-premises uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SharePoint on-premises uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SharePoint on-premises**, selecteer **SharePoint on-premises** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SharePoint on-premises beschikken in de lijst met resultaten](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SharePoint on-premises op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de SharePoint on-premises is aan een gebruiker in Azure AD. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SharePoint on-premises moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SharePoint on-premises, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Toegang verlenen tot SharePoint on-premises testgebruiker](#grant-access-to-sharePoint-on-premises-test-user)**  : als u wilt een equivalent van Britta Simon SharePoint on-premises die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw SharePoint on-premises toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SharePoint on-premises, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SharePoint on-premises** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. Op de **SharePoint on-premises domein en URL's** sectie, voert u de volgende stappen uit:

    ![SharePoint on-premises domein en URL's één aanmeldings-informatie](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. In de **id** tekstvak typt u de URL: `urn:sharepoint:federation`

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media\sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. Op de **SharePoint on-premises configuratie** sectie, klikt u op **configureren SharePoint on-premises** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL** uit de **Naslaggids sectie.**

    ![SharePoint on-premises-configuratie](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > SharePoint On-Premises toepassing maakt gebruik van SAML 1.1 token, zodat Azure AD wordt verwacht dat de WS Fed aanvraag van SharePoint server en na de verificatie, deze problemen met het SAML 1.1. token.

7. In een ander browservenster aan te melden bij uw SharePoint on-premises bedrijf site als een beheerder.

8. **Een nieuwe vertrouwde id-provider configureren in SharePoint Server 2016**

    Meld u aan bij de server voor SharePoint Server 2016 en open de SharePoint 2016 Management Shell. Vul de waarden van $realm, $wsfedurl en $filepath vanuit Azure portal en voer de volgende opdrachten voor het configureren van een nieuwe vertrouwde id-provider.

    > [!TIP]
    > Als u geen ervaring hebt met behulp van PowerShell of lees de informatie over de werking van PowerShell wilt [PowerShell voor SharePoint](https://docs.microsoft.com/en-us/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have copied from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Voer vervolgens deze stappen voor het inschakelen van de vertrouwde id-provider voor uw toepassing:

    a. Navigeer in de centrale beheersite naar **webtoepassing beheren** en selecteert u de web-App die u wilt beveiligen met Azure AD.

    b. Klik in het lint, op **verificatieproviders** en kiest u de zone die u wilt gebruiken.

    c. Selecteer **vertrouwde id-provider** en selecteer de identificeren provider die u zojuist hebt geregistreerd met de naam *AzureAD*.

    d. Selecteer op de URL-instelling van aanmeldingspagina **aangepaste aanmeldingspagina** en geef de waarde '/_trust/'.

    e. Klik op **OK**.

    ![Uw verificatieprovider configureren](./media\sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media\sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media\sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media\sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media\sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>Toegang verlenen tot SharePoint on-premises testgebruiker

De gebruikers die toegang tot SharePoint en meld u aan bij Azure AD, moeten toegangsrechten hebben tot de toepassing gebruikt u de volgende stappen uit om in te stellen de machtigingen voor toegang tot de web-App.

1. Klik in de centrale beheersite op **Toepassingsbeheer**.

2. Op de **Toepassingsbeheer** pagina, in de **webtoepassingen** sectie, klikt u op **beheren van webtoepassingen**.

3. Klik op de juiste web-App en klik vervolgens op **gebruikersbeleid**.

4. Klik in het beleid voor Web-App op **gebruikers toevoegen**.

    ![Zoeken naar een gebruiker door de van naamclaim](./media\sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. In de **gebruikers toevoegen** dialoogvenster vak, klikt u op de juiste zone in **Zones**, en klik vervolgens op **volgende**.

6. In de **beleid voor webtoepassing** in het dialoogvenster de **kiezen gebruikers** sectie, klikt u op de **Bladeren** pictogram.

7. In de **vinden** tekstvak, type de **gebruiker principal name(UPN)** waarde waarvoor u de SharePoint on-premises toepassing hebt geconfigureerd in de Azure AD en klikt u op **zoeken**. </br>Voorbeeld: *brittasimon@contoso.com*.

8. Onder de kop AzureAD in de lijstweergave, selecteert u de eigenschap name en klikt u op **toevoegen** klikt u vervolgens op **OK** om het dialoogvenster te sluiten.

9. Klik in machtigingen op **volledig beheer**.

    ![Volledig beheer verlenen aan een claimgebruiker](./media\sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Klik op **voltooien**, en klik vervolgens op **OK**.

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Een vertrouwde id-provider voor meerdere webtoepassingen configureren

De configuratie voor een enkele webtoepassing werkt, maar aanvullende configuratie nodig als u van plan bent om met de dezelfde vertrouwde id-provider voor meerdere webtoepassingen. Stel dat we een webtoepassing voor gebruik van de URL heeft uitgebreid `https://portal.contoso.local` en nu wilt verifiëren voor de toegang tot `https://sales.contoso.local` ook. Om dit te doen, moet de id-provider om te voldoen aan de parameter WReply en de registratie van de toepassing bijwerken in Azure AD om toe te voegen een antwoord-URL bijwerken.

1. Open de Azure AD-directory in de Azure-Portal. Klik op **App-registraties**, klikt u vervolgens op **alle toepassingen weergeven**. Klik op de toepassing die u eerder hebt gemaakt (integratie van SharePoint-SAML).

2. Klik op **instellingen**.

3. Klik in de instellingenblade op **antwoord-URL's**. 

4. Voeg de URL's voor de extra web-App met `/_trust/default.aspx` toegevoegd aan de URL (zoals `https://sales.contoso.local/_trust/default.aspx`) en klikt u op **opslaan**.

5. Open op de SharePoint-server, de **SharePoint 2016-beheershell** en voer de volgende opdrachten, met de naam van de vertrouwde id-tokenuitgever die u eerder hebt gebruikt.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
6. Ga naar de web-App en inschakelen van de bestaande vertrouwde id-provider in Centraal beheer. Houd er rekening mee aan de URL van de aanmeldingspagina ook configureren als een aangepaste aanmeldingspagina `/_trust/`.

7. In de centrale beheersite, klikt u op de web-App en kies **gebruikersbeleid**. Voeg een gebruiker met de juiste machtigingen, zoals eerder in dit artikel wordt gedemonstreerd.

### <a name="fixing-people-picker"></a>Oplossen van personen selecteren

Gebruikers kunnen zich nu aanmelden bij SharePoint 2016 met gebruikmaking van identiteiten van Azure AD, maar er zijn nog steeds mogelijkheden voor verbetering van de gebruikerservaring. Zoeken naar een gebruiker geeft bijvoorbeeld meerdere zoekresultaten in de kiezer voor personen. Er is een zoekresultaat voor elk van de 3 claimtypen die zijn gemaakt in de claimtoewijzing. Als u een gebruiker met behulp van de personen selecteren, moet u Typ de naam van de gebruiker exact en kies de **naam** claim resultaat.

![Claims van zoekresultaten](./media\sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Er is geen validatie van de waarden die u zoekt, wat tot spelfouten leiden kan of kiest per ongeluk de verkeerde gebruikers claimtype om toe te wijzen, zoals de **achternaam** claim. Dit kan voorkomen dat gebruikers toegang hebben tot resources.

Om te helpen bij dit scenario, is een open-source oplossing met de naam [AzureCP](https://yvand.github.io/AzureCP/) waarmee een aangepaste claims-provider voor SharePoint 2016. Azure AD Graph wordt gebruikt om op te lossen gebruikers invoeren en uitvoeren van validatie. Meer informatie over [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot SharePoint on-premises.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon naar SharePoint on-premises, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **SharePoint on-premises**.

    ![De SharePoint-koppeling in de lijst met toepassingen](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de on-premises SharePoint-tegel in het toegangsvenster klikt, krijgt u automatisch aangemeld op uw SharePoint on-premises toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Met behulp van Azure AD voor SharePoint Server-verificatie](https://docs.microsoft.com/en-us/office365/enterprise/using-azure-ad-for-sharepoint-server-authentication)

<!--Image references-->

[1]: ./media\sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media\sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media\sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media\sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media\sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media\sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media\sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media\sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media\sharepoint-on-premises-tutorial/tutorial_general_203.png

