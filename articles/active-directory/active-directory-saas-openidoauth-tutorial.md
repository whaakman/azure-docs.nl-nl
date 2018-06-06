---
title: Stappen voor het configureren van een toepassing OpenID/OAuth vanuit Azure AD-App-galerie | Microsoft Docs
description: Stappen voor het configureren van een toepassing OpenID/OAuth vanuit Azure AD-App-galerie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 109ee4f2d723d3dbebba5374e95a39d058e47f42
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34727630"
---
# <a name="steps-to-configure-an-openidoauth-application-from-azure-ad-app-gallery"></a>Stappen voor het configureren van een toepassing OpenID/OAuth vanuit Azure AD-App-galerie

## <a name="process-of-open-id-application-addition-from-gallery"></a>Proces van het Open ID toepassing toevoegen vanuit galerie

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop](./media/active-directory-saas-openidoauth-tutorial/tutorial_general_01.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen](./media/active-directory-saas-openidoauth-tutorial/tutorial_general_02.png)

3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing](./media/active-directory-saas-openidoauth-tutorial/tutorial_general_03.png)

4. Typ in het zoekvak **toepassingsnaam**, selecteer de **gewenst toepassing** van resultaat Configuratiescherm en meld u tot de toepassing.

    ![Toepassing toevoegen](./media/active-directory-saas-openidoauth-tutorial/addfromgallery.png)

    > [!NOTE]
    > Knop is standaard uitgeschakeld voor Open ID Connect en OAuth apps toevoegen. Hier de tenantbeheerder klikt op de **aanmelding** knop en geef de toestemming voor de toepassing. Tenant, hoeft u niet expliciet toevoegen en de configuraties komen met die dat de toepassing ophalen toegevoegd in de klant.

    ![Knop Toevoegen](./media/active-directory-saas-openidoauth-tutorial/addbutton.png)

5. Wanneer u op de registratiepagina van de koppeling klikt, wordt u omgeleid naar Azure AD-pagina voor aanmeldingsreferenties.

6. Gebruiker heeft de toestemming van de pagina toestemming en daarna accepteren, de startpagina van de toepassing wordt weergegeven na een geslaagde authenticatie.

    > [!NOTE]
    > Klanten kunnen slechts één exemplaar van de toepassing toevoegen. Als u al hebt toegevoegd en er is geprobeerd de toestemming geven om wordt opnieuw het niet opnieuw worden toegevoegd in de tenant. Dus logisch kan slechts één exemplaar van de app worden gebruikt in de tenant.

## <a name="authentication-flow-using-openid-connect"></a>Verificatiestroom met OpenID Connect

De meest eenvoudige aanmelding-stroom bevat de volgende stappen: elk van deze hieronder in detail wordt beschreven.

![Verificatiestroom met OpenID Connect](./media/active-directory-saas-openidoauth-tutorial/authenticationflow.png)

* **Multitenant toepassing** -een multitenant-toepassing is niet bedoeld voor gebruik in veel organisaties is slechts één organisatie. Dit zijn doorgaans software-as-a-service (SaaS)-toepassingen geschreven door een onafhankelijke softwareleverancier (ISV). Multitenant-toepassingen moeten worden ingericht in elke map waar ze worden gebruikt, waarvoor een gebruiker of beheerder toestemming om u te registreren. Dit proces toestemming wordt gestart wanneer een toepassing is geregistreerd in de map en toegang tot de Graph API of een andere web-API krijgt. Wanneer een gebruiker of beheerder van een andere organisatie die zich aanmeldt om de toepassing te gebruiken, moeten ze wordt weergegeven met een dialoogvenster waarin u de machtigingen die vereist zijn voor de toepassing. De gebruiker of beheerder kan vervolgens instemming met de toepassing dat geeft de toepassing toegang tot de opgegeven gegevens, en ten slotte registreert u de toepassing in de directory.

    > [!NOTE]
    > Als u uw toepassing beschikbaar voor gebruikers in meerdere mappen, moet u een mechanisme om te bepalen welke tenant ze zich in. Een toepassing voor één tenant moet alleen zoeken in de eigen map voor een gebruiker, terwijl een multitenant-toepassing moet een specifieke gebruiker uit alle mappen identificeren in Azure AD. Als u wilt uitvoeren van deze taak levert Azure AD een algemene eindpunt voor authenticatie, waarbij elke toepassing met meerdere tenants aanmelden, in plaats van een tenantspecifieke-eindpunt wenden kan. Dit eindpunt is [ https://login.microsoftonline.com/common ](https://login.microsoftonline.com/common) voor alle mappen in Azure AD, terwijl een eindpunt tenantspecifieke mogelijk [ https://login.microsoftonline.com/contoso.onmicrosoft.com ](https://login.microsoftonline.com/contoso.onmicrosoft.com). Het algemene eindpunt is vooral belangrijk bij het ontwikkelen van uw toepassing omdat u de benodigde logica moet voor het afhandelen van meerdere tenants tijdens het aanmelden, afmelden en validatie van tokens.

Azure AD-team standaard bijdraagt aan meerdere tenants toepassing als die gemakkelijk toegankelijk zijn in verschillende organisaties en eenvoudig te gebruiken is nadat u hebt de toestemming geaccepteerd.

## <a name="what-is-consent-framework"></a>Wat is Framework toestemming geven?

Het framework van Azure AD toestemming kunt eenvoudig ontwikkelen van multitenant-web- en native client-toepassingen. Deze toepassingen kunnen aanmelden door gebruikersaccounts van een Azure AD-tenant, verschilt van de waar de toepassing is geregistreerd. Ze kunnen ook toegang nodig tot web-API's zoals de Microsoft Graph-API (voor toegang tot Azure Active Directory, Intune en services in Office 365) en andere Microsoft-services-API's, naast uw eigen web-API's. Het framework is gebaseerd op een gebruiker of beheerder toestemming verlenen tot een toepassing waarin u wordt gevraagd om te worden geregistreerd in de directory, waarbij toegang tot Active directory-gegevens. Nadat toestemming is opgegeven, is de clienttoepassing worden kunnen de Microsoft Graph-API aanroepen namens de gebruiker en gebruik de informatie, indien nodig.

De [Microsoft Graph API](https://graph.microsoft.io/) biedt toegang tot gegevens in Office 365 (zoals agenda's en berichten van Exchange, sites en lijsten van SharePoint, documenten van OneDrive, OneNote-notitieblokken, taken van de Planner, werkmappen van Excel, enz.) en de gebruikers en groepen van Azure AD en andere gegevensobjecten van meer Microsoft-cloudservices.

De volgende stappen ziet u hoe de toestemming ondervinden werkt voor de ontwikkelaar van toepassingen en de gebruiker.

1. Stel dat u hebt een web-clienttoepassing die nodig zijn om aan te vragen van specifieke machtigingen voor toegang tot een resource/API. De Azure-portal wordt gebruikt om te declareren machtigingsaanvragen tijdens de configuratie. Net als andere configuratie-instellingen uitmaken ze deel van de Azure AD-registratie van de toepassing:

    ![Graph API](./media/active-directory-saas-openidoauth-tutorial/graphapi.png)

2. U kunt uw toepassing worden machtigingen zijn bijgewerkt, de toepassing wordt uitgevoerd en een gebruiker is gebruikt voor het eerst. De toepassing moet eerst een autorisatiecode verkrijgen van Azure AD / eindpunt autoriseren. De autorisatiecode kan vervolgens worden gebruikt bij het aanschaffen van een nieuwe toegang en token vernieuwen.

3. Als de gebruiker nog niet is geverifieerd, Azure AD van / autoriseren eindpunt wordt u gevraagd om de aanmeldingspagina.

    ![Verificatie](./media/active-directory-saas-openidoauth-tutorial/authentication.png)

4. Nadat de gebruiker is aangemeld, wordt Azure AD bepalen of de gebruiker moet een pagina toestemming worden weergegeven. Deze beslissing is gebaseerd op of de gebruiker (of de beheerder van hun organisatie) is al verleend de toepassing toestemming. Toestemming heeft geen toestemming gekregen, Azure AD wordt de gebruiker om toestemming gevraagd als staan de vereiste machtigingen nodig om te functioneren. De reeks machtigingen die wordt weergegeven in het dialoogvenster toestemming overeen met de die u hebt geselecteerd in de gedelegeerde machtigingen in de Azure portal.

    ![Toestemming pagina](./media/active-directory-saas-openidoauth-tutorial/consentpage.png)

Sommige machtigingen kunnen door een gewone gebruiker wil terwijl anderen toestemming van de tenantbeheerder vereisen.

## <a name="whats-the-difference-between-admin-consent-and-user-consent"></a>Wat is het verschil tussen Admin toestemming en toestemming van de gebruiker?

Als beheerder, kunt u ook akkoord gedelegeerde machtigingen van een toepassing namens de gebruikers in uw tenant. Administratieve toestemming voorkomt u het dialoogvenster toestemming worden weergegeven voor elke gebruiker in de tenant en kan worden uitgevoerd in de Azure-portal door gebruikers met de beheerdersrol. Op de instellingenpagina voor uw toepassing vereist machtigingen op en klik op de knop machtiging verlenen.

![Machtiging verlenen](./media/active-directory-saas-openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> Expliciete verlenen via de knop machtiging verlenen u hoeft momenteel voor één pagina toepassingen (SPA) die gebruikmaken van ADAL.js. Anders mislukt de toepassing wanneer het toegangstoken is aangevraagd.

Machtigingen voor alleen-App moeten altijd toestemming van de tenantbeheerder. Als uw toepassing een machtiging app alleen worden aangevraagd en een gebruiker probeert aan te melden bij de toepassing, wordt een foutbericht weergegeven dat de gebruiker is niet kunnen toestemming geven.

Als uw toepassing gebruikmaakt van machtigingen die toestemming van de beheerder vereisen, moet u een gebaar zoals een knop of koppeling hebben waar de beheerder de actie kan initiëren. De aanvraag de toepassing verzendt voor deze actie is de gebruikelijke OAuth2/OpenID Connect autorisatieaanvraag die ook de prompt = admin_consent queryreeksparameter opgeven. Als de beheerder heeft ingestemd en de service-principal gemaakt in de tenant van de klant, volgende aanmelden aanvragen hoeft niet de prompt = admin_consent-parameter. Omdat de beheerder heeft besloten dat de machtigingen die zijn aangevraagd, worden geaccepteerd, er zijn geen andere gebruikers in de tenant om toestemming wordt gevraagd vanaf dat moment. Een tenantbeheerder kan de mogelijkheid voor reguliere gebruikers om toestemming voor toepassingen kunt uitschakelen. Als deze mogelijkheid is uitgeschakeld, is altijd admin toestemming vereist voor de toepassing moet worden gebruikt in de tenant. Als u wilt dat uw toepassing testen met eindgebruikers toestemming is uitgeschakeld, vindt u de configuratieswitch in de [Azure-portal](https://portal.azure.com/) in de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) onder sectie **Enterprise toepassingen**

De prompt = admin_consent parameter kan ook worden gebruikt door toepassingen die machtigingen die niet te worden admin toestemming hoeven te vragen. Een voorbeeld van wanneer dit wordt gebruikt, is als de toepassing een ervaring waar de tenantbeheerder vereist 'aanmeldt' een tijdstip en wordt geen enkele andere gebruikers om toestemming vanaf dat moment wordt gevraagd.

Als beheerder toestemming nodig om een toepassing en een beheerder zich aanmeldt zonder de prompt = admin_consent-parameter worden verzonden wanneer de beheerder is instemt met de toepassing dit alleen voor hun gebruikersaccount geldt. Gewone gebruikers nog steeds zich niet aanmelden of instemming met de toepassing. Deze functie is handig als u de tenantbeheerder bieden de mogelijkheid om te verkennen van uw toepassing wilt voordat u andere gebruikers toegang toestaat.