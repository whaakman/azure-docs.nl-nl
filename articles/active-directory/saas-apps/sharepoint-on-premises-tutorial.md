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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 8cae1e2670ba449c0db28209c54b740e927bbb73
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36210374"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Zelfstudie: Azure Active Directory-integratie met SharePoint on-premises

In deze zelfstudie leert u hoe SharePoint lokale integreren met Azure Active Directory (Azure AD).

SharePoint lokale integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben tot SharePoint on-premises.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SharePoint on-premises (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SharePoint on-premises, moet u de volgende items:

- Een Azure AD-abonnement
- Een SharePoint lokale eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SharePoint lokale uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>SharePoint lokale uit de galerie toevoegen
Voor het configureren van de integratie van SharePoint lokale in Azure AD, moet u SharePoint lokale uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SharePoint lokale uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SharePoint lokale**, selecteer **SharePoint lokale** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SharePoint lokaal in de lijst met resultaten](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met SharePoint on-premises op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker van het bijbehorende equivalent op SharePoint on-premises in Azure AD voor een gebruiker is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SharePoint lokale moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SharePoint on-premises, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SharePoint on-premises testgebruiker](#create-a-sharePoint-on-premises-test-user)**  - hebben een equivalent van Britta Simon op SharePoint on-premises die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw on-premises SharePoint toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SharePoint on-premises, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SharePoint lokale** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. Op de **SharePoint on-premises domein en URL's** sectie, voert u de volgende stappen uit:

    ![Lokale domein en de URL's eenmalige aanmelding gegevens voor SharePoint](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `urn:sharepoint:<YourSharePointServerURL>`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [SharePoint lokale Client ondersteuningsteam](https://support.office.com/) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op de computer met de extensie .cer. Kopieer en plak het volledige pad van het metagegevensbestand van de gedownloade in Kladblok.

    ![De downloadkoppeling certificaat](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. Op de **SharePoint lokale configuratie** sectie, klikt u op **SharePoint configureren lokale** openen **eenmalige aanmelding configureren** venster. Kopieer de **Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van SharePoint on-premises](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > SharePoint On-Premises toepassing gebruikmaakt van SAML 1.1 token, zodat de Azure AD verwacht aanvraag WS Fed van SharePoint-server en na verificatie deze uitgeeft het SAML-1.1. token.

7. In een ander browservenster, meld u aan bij uw SharePoint-site op het lokale bedrijf als beheerder.

8. **Een nieuwe vertrouwde id-provider configureren in SharePoint Server 2016**

    Meld u aan bij de server voor SharePoint Server 2016 en open de SharePoint 2016-beheershell. Vul de waarden van $realm $wsfedurl en $filepath vanuit Azure-portal en voer de volgende opdrachten om een nieuwe vertrouwde id-provider te configureren.

    > [!TIP]
    > Als u geen ervaring hebt met behulp van PowerShell of Zie voor meer informatie over de werking van PowerShell wilt [SharePoint PowerShell](https://docs.microsoft.com/en-us/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have copied from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Vervolgens Volg deze stappen voor het inschakelen van de vertrouwde id-provider voor uw toepassing:

    a. Navigeer in de Centraalbeheersite naar **webtoepassing beheren** en selecteert u de webtoepassing die u wilt beveiligen met Azure AD.

    b. Klik in het lint op **verificatieproviders** en kies de zone die u wilt gebruiken.

    c. Selecteer **vertrouwde identiteitsprovider** en selecteer de identificeren provider die u zojuist hebt geregistreerd met de naam *AzureAD*.

    d. Selecteer op de aanmeldingspagina URL-instelling **aangepaste aanmeldingspagina** en geef de waarde '/_trust/'.

    e. Klik op **OK**.

    ![Uw provider voor verificatie configureren](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-sharepoint-on-premises-test-user"></a>Een SharePoint on-premises test-gebruiker maken

1. Klik in de Centraalbeheersite op **Toepassingsbeheer**.

2. Op de **Toepassingsbeheer** pagina in de **webtoepassingen** sectie, klikt u op **beheren van webtoepassingen**.

3. Klik op de juiste webtoepassing en klik vervolgens op **gebruikersbeleid**.

4. Klik in het beleid voor webtoepassing **gebruikers toevoegen**.

    ![Zoeken naar een gebruiker door hun naamclaim](./media/sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. In de **gebruikers toevoegen** dialoogvenster vak, klikt u op de juiste zone in **Zones**, en klik vervolgens op **volgende**.

6. In de **beleid voor webtoepassing** het dialoogvenster de **gebruikers kiezen** sectie, klikt u op de **Bladeren** pictogram.

7. In de **vinden** textbox, typ de naam aanmelden voor een gebruiker in uw directory en klikt u op **Search**. </br>Voorbeeld: *demouser@blueskyabove.onmicrosoft.com*.

8. Onder de kop AzureAD in de lijstweergave, selecteer de eigenschap name en klik op **toevoegen** klikt u vervolgens op **OK** om het dialoogvenster te sluiten.

9. Klik in machtigingen op **volledig beheer**.

    ![Volledig beheer verlenen aan een claimgebruiker](./media/sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Klik op **voltooien**, en klik vervolgens op **OK**.

### <a name="fixing-people-picker"></a>Personen selecteren is hersteld

Gebruikers kunnen zich nu aanmelden bij SharePoint 2016 met behulp van id's van Azure AD, maar er zijn nog steeds mogelijkheden voor verbetering van de gebruikerservaring. Zoeken naar een gebruiker geeft bijvoorbeeld meerdere zoekresultaten in de kiezer voor personen. Er is een zoekresultaat voor elk van de 3 claimtypen die zijn gemaakt in de claimtoewijzing. Als u een gebruiker met behulp van de personen selecteren, moet u precies typt u hun gebruikersnaam en kies de **naam** resultaat claim.

![Claims zoekresultaten](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Er is geen validatie van de waarden die u zoekt, wat tot typefouten leiden kan of gebruikers per ongeluk de verkeerde kiezen claimtype om toe te wijzen, zoals de **achternaam** claim. Dit kan voorkomen dat gebruikers wel toegang hebben tot bronnen.

Om te helpen bij dit scenario, is een open source oplossing aangeroepen [AzureCP](https://yvand.github.io/AzureCP/) biedt een aangepaste claimprovider voor SharePoint 2016. De Azure AD Graph wordt gebruikt om op te lossen gebruikers invoeren en voeren validatie. Meer informatie in [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot SharePoint on-premises.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon met SharePoint on-premises, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **SharePoint lokale**.

    ![De SharePoint-koppeling in de lijst met toepassingen](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel van de lokale SharePoint in het deelvenster toegang, moet u voor uw SharePoint on-premises toepassing automatisch aangemelde ophalen.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/sharepoint-on-premises-tutorial/tutorial_general_203.png

