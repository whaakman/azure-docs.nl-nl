---
title: Een OpenID/OAuth-toepassing configureren vanuit de Azure AD-app-galerie | Microsoft Docs
description: Stappen voor het configureren van een OpenID/OAuth-toepassing uit de Azure AD-app-galerie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 166452b052313397f1ec17adb59cad3c20fab1f9
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497432"
---
# <a name="configure-an-openidoauth-application-from-the-azure-ad-app-gallery"></a>Een OpenID/OAuth-toepassing uit de Azure AD-app-galerie configureren

## <a name="process-of-adding-an-openid-application-from-the-gallery"></a>Proces voor het toevoegen van een OpenID-toepassing vanuit de galerie

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het linkerdeelvenster **Azure Active Directory**. 

    ![De knop Azure Active Directory](common/select-azuread.png))

2. Ga naar **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Selecteer **Nieuwe toepassing** boven in het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak de naam van de toepassing. Selecteer de gewenste toepassing in het deelvenster met resultaten en registreer u bij de toepassing.

    ![Openid in de lijst met resultaten](common/search-new-app.png)

    > [!NOTE]
    > Voor OpenID Connect- en OAuth-apps is de knop **Toevoegen** standaard uitgeschakeld. De tenantbeheerder moet hier de registratieknop selecteren en toestemming geven voor de toepassing. De toepassing wordt vervolgens toegevoegd aan de klanttenant, waar u configuraties kunt uitvoeren. Het is niet nodig om de toepassing expliciet toe te voegen.

    ![Knop Toevoegen](./media/openidoauth-tutorial/addbutton.png)

5. Wanneer u de registratiekoppeling selecteert, wordt u naar de Azure Active Directory-pagina (Azure AD) geleid voor aanmeldingsreferenties.

6. Wanneer de verificatie is voltooid, accepteert u toestemming op de toestemmingspagina. Hierna wordt de startpagina van de toepassing weergegeven.

    > [!NOTE]
    > U kunt slechts één exemplaar van de toepassing toevoegen. Als u er al een hebt toegevoegd en u opnieuw hebt geprobeerd om toestemming te geven, wordt deze niet opnieuw toegevoegd aan de tenant. Logisch gezien kunt u dus maar één app-exemplaar gebruiken in de tenant.

## <a name="authentication-flow-using-openid-connect"></a>Verificatiestroom waarbij OpenID Connect wordt gebruikt

De meest eenvoudige aanmeldingsstroom bestaat uit de volgende stappen:

![Verificatiestroom waarbij OpenID Connect wordt gebruikt](./media/openidoauth-tutorial/authenticationflow.png)

### <a name="multitenant-application"></a>Toepassing met meerdere tenants 
Toepassingen met meerdere tenants kunnen in vele organisatie worden gebruikt en niet slechts in één organisatie. Dit zijn normaliter Software-as-a-Service-toepassingen (SaaS), geschreven door een onafhankelijke softwareleverancier (ISV). 

Toepassingen met meerdere tenants moeten in elke map waarin ze zullen worden gebruikt, worden ingericht. Er toestemming van een gebruiker of beheerder nodig om ze te kunnen registreren. Het toestemmingsproces begint wanneer een toepassing in de map is geregistreerd en toegang verkrijgt tot de Graph-API of wellicht zelfs een andere web-API. Wanneer een gebruiker of beheerder van een andere organisatie zich registreert voor gebruik van de toepassing, wordt er een dialoogvenster weergegeven met de machtigingen die de toepassing nodig heeft. 

De gebruiker of beheerder geeft dan toestemming voor de toepassing. Met deze toestemming krijgt de toepassing toegang tot de opgegeven gegevens én wordt de toepassing in de map geregistreerd.

> [!NOTE]
> Als u uw toepassing beschikbaar maakt voor gebruikers in meerdere mappen, hebt u een mechanisme nodig om te bepalen in welke tenant deze zich bevinden. Toepassingen met één tenant hoeven alleen in hun eigen map op zoek te gaan naar een gebruiker. Toepassingen met meerdere tenants moeten specifieke gebruikers identificeren vanuit álle mappen in Azure AD.
> 
> Om deze taak uit te voeren, biedt Azure AD een algemeen verificatie-eindpunt waar toepassingen met meerdere tenants aanmeldingsaanvragen naartoe kunnen sturen (in plaats van een tenantspecifiek eindpunt). Dit eindpunt is [https://login.microsoftonline.com/common](https://login.microsoftonline.com/common) voor alle mappen in Azure AD. Een tenantspecifiek eindpunt kan bijvoorbeeld [https://login.microsoftonline.com/contoso.onmicrosoft.com](https://login.microsoftonline.com/contoso.onmicrosoft.com) zijn. 
>
> Het algemene eindpunt is belangrijk om in overweging te nemen bij het ontwikkelen van uw toepassing. U hebt de vereiste logica nodig om meerdere tenants tijdens het aanmelden, het afmelden en het valideren van tokens te kunnen verwerken.

Azure AD promoot standaard toepassingen met meerdere tenants. Ze zijn eenvoudig toegankelijk in meerdere organisaties en zijn eenvoudig te gebruiken nadat de toestemming is geaccepteerd.

## <a name="consent-framework"></a>Toestemmingsframework

U kunt u het Azure AD-toestemmingsframework gebruiken om clienttoepassingen met meerdere tenants (op internet en systeemeigen) te maken. Bij deze toepassingen kunnen gebruikersaccounts zich aanmelden uit Azure AD-tenants die verschillen van de tenant waarin de toepassing is geregistreerd. Mogelijk hebben ze ook toegang nodig tot web-API's zoals:
- De Microsoft Graph-API, voor toegang tot Azure AD, Intune en services in Office 365. 
- De API's van andere Microsoft-services.
- Uw eigen web-API's. 

Het framework is gebaseerd op een gebruiker of beheerder die toestemming geeft aan een toepassing die vraagt in de map te worden geregistreerd. De registratie kan toegang tot mapgegevens omvatten. Wanneer er toestemming is gegeven, kan de clienttoepassing de Microsoft Graph-API namens de gebruiker aanroepen en op basis van behoefte gebruikmaken van gegevens.

De [Microsoft Graph-API](https://developer.microsoft.com/graph/) biedt toegang tot gegevens in Office 365, zoals:

- Agenda's en berichten uit Exchange.
- Sites en lijsten uit SharePoint.
- Documenten uit OneDrive.
- Notebooks uit OneNote.
- Taken uit Planner.
- Werkmappen uit Excel.

De Graph-API biedt ook toegang tot gebruikers en groepen uit Azure AD en andere gegevensobjecten uit andere Microsoft-cloudservices.

In de volgende stappen ontdekt u hoe de toestemmingservaring werkt voor de toepassingsontwikkelaar en -gebruiker:

1. Stel dat u een webclienttoepassing hebt die specifieke machtigingen moet aanvragen voor toegang tot een resource of API. De Azure-portal wordt gebruikt om tijdens de configuratie machtigingsaanvragen te declareren. Net als andere configuratie-instellingen worden ze onderdeel van Azure AD-registraties van de toepassing. Voor het pad van de aanvraag machtiging moet u de Volg de onderstaande stappen te volgen:

    a. Klik op de **App-registraties** vanaf de linkerkant van het menu en open uw toepassing door te typen van de toepassing een naam in het zoekvak.

    ![Graph API](./media/openidoauth-tutorial/application.png)

    b. Klik op **API-machtigingen weergeven**.

    ![Graph API](./media/openidoauth-tutorial/api-permission.png)

    c. Klik op **toevoegen van een machtiging**.

    ![Graph API](./media/openidoauth-tutorial/add-permission.png)

    d. Klik op **Microsoft Graph**.

    ![Graph API](./media/openidoauth-tutorial/microsoft-graph.png)

    e. Selecteer de vereiste opties van **overgedragen machtigingen** en **Toepassingsmachtigingen**.

    ![Graph API](./media/openidoauth-tutorial/graphapi.png)

2. Stel dat de machtigingen van de toepassing zijn bijgewerkt. De toepassing is actief en een gebruiker staat op het punt deze voor het eerst te gebruiken. Om te beginnen moet de toepassing een verificatiecode ophalen bij het Azure AD-eindpunt /authorize. De autorisatiecode kan vervolgens worden gebruikt bij het verkrijgen van een nieuw token voor toegang en vernieuwen.

3. Als de gebruiker nog niet is geverifieerd, vraagt het Azure AD-eindpunt /authorize om aanmelding.

    ![Verificatie](./media/openidoauth-tutorial/authentication.png)

4. Wanneer de gebruiker zich heeft aangemeld, bepaalt Azure AD of de gebruiker een toestemmingspagina te zien krijgt. Hierbij wordt gecontroleerd of de gebruiker (of de beheerder in de organisatie) de toepassing al toestemming heeft gegeven.

   Als er nog geen toestemming is gegeven, vraagt Azure AD de gebruiker om toestemming en wordt weergegeven welke machtigingen nodig zijn om de app goed te laten werken. De machtigingen die in het dialoogvenster voor toestemming worden weergegeven, komen overeen met de gedelegeerde toestemmingen die in de Azure-portal zijn geselecteerd.

    ![Toestemmingspagina](./media/openidoauth-tutorial/consentpage.png)

Gewone gebruikers kunnen toestemming geven voor verschillende machtigingen. Voor andere machtigingen is de toestemming van een tenantbeheerder nodig.

## <a name="difference-between-admin-consent-and-user-consent"></a>Het verschil tussen de toestemming van een beheerder en de toestemming van een gebruiker

Beheerders kunnen toestemming geven voor de gedelegeerde machtigingen van een toepassing voor alle gebruikers in de tenant. Met beheerderstoestemming wordt het dialoogvenster voor toestemming niet meer weergegeven aan alle gebruikers in de tenant. Gebruikers die beschikken over de beheerdersrol kunnen in de Azure-portal toestemming geven. Uit de **instellingen** -pagina voor uw toepassing, selecteert **vereiste machtigingen** > **verlenen van toestemming van een beheerder**.

![De knop Machtigingen verlenen](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> Expliciete toestemming verlenen met behulp van de **verlenen van toestemming van een beheerder** knop is nu vereist voor één pagina toepassingen (kuuroorden) die gebruikmaken van ADAL.js. Als dit niet gebeurt, treedt er een fout op in de toepassing wanneer het toegangstoken wordt aangevraagd.

Bij app-specifieke machtigingen is er altijd toestemming van een tenantbeheerder nodig. Als uw toepassing een app-specifieke machtiging aanvraagt en een gebruiker zich bij de toepassing probeert aan te melden, wordt er een foutmelding weergegeven. In het bericht staat dat de gebruiker geen toestemming kan geven.

Als voor uw toepassing gebruik wordt gemaakt van machtigingen waarvoor beheerderstoestemming nodig is, moet u een knop of een koppeling toevoegen waarmee de beheerder toestemming kan geven. De aanvraag die uw toepassing met deze actie verzendt, is de gebruikelijke OAuth2/OpenID Connect-verificatieaanvraag. De aanvraag bevat de queryreeksparameter *prompt=admin_consent*. 

Wanneer de beheerder toestemming heeft gegeven en de service-principal in de tenant van de klant is gemaakt, is de parameter *prompt=admin_consent* bij latere aanmeldingsaanvragen niet meer nodig. Omdat de beheerder heeft besloten dat de aangevraagde machtigingen worden geaccepteerd, wordt andere gebruikers in de tenant niet meer om toestemming gevraagd.

Tenantbeheerders kunnen uitschakelen dat normale gebruikers toestemming kunnen geven voor toepassingen. Als dit wordt uitgeschakeld, is er altijd beheerderstoestemming nodig om een toepassing in een tenant te kunnen gebruiken. Als u uw toepassing wilt testen terwijl toestemming voor eindgebruikers is uitgeschakeld, kunt u toestemming uitschakelen in de [Azure-portal](https://portal.azure.com/). Ga hiervoor naar het gedeelte [Gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) onder **Bedrijfstoepassingen**.

De parameter *prompt=admin_consent* kan ook worden gebruikt door toepassingen die machtigingen aanvragen waarvoor geen beheerderstoestemming nodig is. Een voorbeeld is een toepassing waarbij de tenantbeheerder zich één keer registreert. Andere gebruikers wordt vervolgens nooit meer om toestemming gevraagd.

Stel dat een toepassing beheerderstoestemming vereist en een beheerder zich aanmeldt zonder dat de parameter *prompt=admin_consent* wordt verzonden. Als de beheerder toestemming geeft voor de toepassing, is dit alleen van toepassing op zijn of haar eigen gebruikersaccount. Gewone gebruikers kunnen zich nog altijd niet aanmelden en geen toestemming geven voor de toepassing. Deze functie is handig als u de tenantbeheerder de mogelijkheid wilt bieden om uw toepassing te bekijken voordat andere gebruikers toegang wordt geboden.
