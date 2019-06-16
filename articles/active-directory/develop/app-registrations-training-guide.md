---
title: App-registraties in de Azure-portal training-guide - Azure
description: Ingesloten en browser zonder verificatie stromen met behulp van het apparaat verlenen.
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: aragra
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 118c6ecb16d325a384246a0b3d9e685f6f6f04ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870115"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>Handleiding voor training: App-registraties in Azure portal  

Vindt u talloze verbeteringen in de nieuwe [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) -ervaring in Azure portal. Als u meer wilt weten over de traditionele ervaring bent, moet u deze training-handleiding gebruiken om u aan de slag met de nieuwe ervaring.

## <a name="key-changes"></a>Belangrijke wijzigingen in het

- App-registraties zijn niet beperkt tot die ofwel een **web-app/API** of een **systeemeigen** app. U kunt de dezelfde app-registratie gebruiken voor al deze door de respectieve omleidings-URI's registreren.
- De verouderde ervaring ondersteund-apps die zich aanmelden-organisatie (Azure AD) alleen accounts. Apps zijn geregistreerd als één tenant (ondersteunen alleen organisatieaccounts uit de map die de app is geregistreerd) en kan worden gewijzigd om te worden van meerdere tenants (ondersteunen alle organisatie-accounts). De nieuwe ervaring kunt u apps die u kunnen ondersteuning voor zowel deze opties, evenals een derde optie registreren: alle organisatie-accounts, evenals persoonlijke Microsoft-accounts.
- De traditionele ervaring is alleen beschikbaar wanneer u bent aangemeld bij de Azure-portal met behulp van een organisatie-account. Met de nieuwe ervaring, kunt u persoonlijke Microsoft-accounts die niet gekoppeld aan een directory zijn.

## <a name="list-of-applications"></a>Lijst met toepassingen

- De nieuwe applijst worden toepassingen weergegeven die zijn geregistreerd via de verouderde app registraties ervaring in de Azure portal (apps die zich in Azure AD-accounts), evenals apps geregistreerd via de [Portal voor Appregistratie](https://apps.dev.microsoft.com/) (apps die zich in Azure AD en privé-Microsoft-accounts).
- De nieuwe applijst beschikt niet over een **toepassingstype** kolom (omdat de registratie van een één-app kan bestaan uit verschillende typen) en twee extra kolommen bevat: een **gemaakt op** kolom en een **certificaten & geheimen** kolom met de status (huidig, verloopt binnenkort of verlopen) van de referenties die zijn geregistreerd op de app.

## <a name="new-app-registration"></a>Nieuwe app-registratie

In de traditionele ervaring voor het registreren van een app moest u bieden: **Naam**, **toepassingstype**, en **aanmeldings-URL/omleidings-URI**. De apps die zijn gemaakt zijn Azure AD slechts één tenant toepassingen, wat betekent dat ze alleen organisatieaccounts uit de map die de app is geregistreerd in ondersteund.

In de nieuwe ervaring, moet u een **naam** voor de app en kies de **accounttypen ondersteund**. U kunt desgewenst een **omleidings-URI**. Als u een omleidings-URI hebt opgegeven, moet u om op te geven als het web/openbare (mobiele en bureaubladtoepassingen). Voor meer informatie over het registreren van een app met behulp van de nieuwe app-registraties-ervaring, Zie [in deze Quick Start](quickstart-register-app.md).

## <a name="the-legacy-properties-page"></a>De verouderde pagina eigenschappen

De traditionele ervaring had een **eigenschappen** -pagina waarop de nieuwe ervaring beschikt niet over. De **eigenschappen** blade heeft de volgende velden: **Naam**, **Object-ID**, **toepassings-ID**, **App ID URI**, **Logo**, **URL van startpagina** , **Afmeldings-URL van**, **URL voor servicevoorwaarden**, **URL van privacyverklaring van**, **toepassingstype**, en  **Meerdere tenants.**

Dit is waar u de dezelfde functionaliteit kunt vinden in de nieuwe ervaring:

- **Naam**, **Logo**, **URL van startpagina**, **URL voor servicevoorwaarden**, en **URL van privacyverklaring van** is nu op vandeapp **Huisstijl** pagina.
- **Object-ID** en **(client) toepassings-ID** is op de **overzicht** pagina.
- De functionaliteit die wordt beheerd door de **multitenant** wisselknop in de traditionele ervaring is vervangen door **ondersteund accounttypen** op de **verificatie** pagina. Zie voor meer informatie over hoe meerdere tenants aan de ondersteunde opties toegewezen [in deze Quick Start](quickstart-modify-supported-accounts.md).
- **Afmeldings-URL van** is nu op de **verificatie** pagina.
- **Toepassingstype** is niet langer geldig veld. In plaats daarvan omleidings-URI's (die u kunt vinden op de **verificatie** pagina) te bepalen welke typen Apps die worden ondersteund.
- **App-ID-URI** heet nu **URI toepassings-ID** en u kunt dit vinden op de **beschikbaar maken van een API** blade. In de traditionele ervaring kunnen deze eigenschap is automatisch-geregistreerd met de volgende notatie: `https://{tenantdomain}/{appID}` (bijvoorbeeld `https://microsoft.onmicrosoft.com/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`). In de nieuwe indeling is automatisch gegenereerd als `api://{appID}`, maar ze moeten expliciet worden opgeslagen.

## <a name="reply-urlsredirect-urls"></a>Antwoord-URL/Omleidings-URL 's

In de traditionele ervaring kunnen een app heeft een **antwoord-URL's** pagina. In de nieuwe ervaring antwoord-URL's kan worden gevonden op van een app **verificatie** sectie. Bovendien ze worden aangeduid als **omleidings-URI's**. Bovendien de indeling voor het omleiden van URI's is gewijzigd. Ze moeten worden gekoppeld aan een app-type (web- of openbaar). Bovendien uit veiligheidsoverwegingen jokertekens en http:// schema's worden niet ondersteund (met uitzondering van http://localhost).

## <a name="keyscertificates--secrets"></a>Sleutels/certificaten en geheimen

In de traditionele ervaring kunnen een app moest **sleutels** pagina. In de nieuwe ervaring, deze is gewijzigd in **certificaten en geheimen**. Bovendien **openbare sleutels** worden aangeduid als **certificaten** en **wachtwoorden** worden aangeduid als **Client geheimen**.

## <a name="required-permissionsapi-permissions"></a>Vereiste machtigingen/API-machtigingen

- In de traditionele ervaring kunnen een app heeft een **vereiste machtigingen** pagina. In de nieuwe ervaring, deze is gewijzigd in **API-machtigingen**.
- Bij het selecteren van een API in de traditionele ervaring, kunt u kiezen uit een kleine lijst Microsoft APIs of zoek via service-principals in de tenant. In de nieuwe ervaring, kunt u kiezen uit meerdere tabbladen: **Microsoft APIs**, **API's maakt gebruik van mijn organisatie**, of **mijn API's**. De zoekbalk op **API's van mijn organisatie** maakt gebruik van tabblad zoekopdrachten via service-principals in de tenant. 

   > [!NOTE]
   > Als uw toepassing is niet gekoppeld aan een tenant, kunt u op dit tabblad niet zien. Zie voor meer informatie over het aanvragen van machtigingen met behulp van de nieuwe ervaring [in deze Quick Start](quickstart-configure-app-access-web-apis.md).

- De traditionele ervaring had een **machtigingen verlenen** knop aan de bovenkant van de **aangevraagd machtigingen** pagina. In de nieuwe ervaring, er is een **toestemming verlenen** sectie met een **verlenen beheerderstoestemming** knop op van een app **API-machtigingen** sectie. Bovendien, zijn er enkele verschillen in de manieren waarop de functie knoppen:
   - In de traditionele ervaring kunnen de logica variëren afhankelijk van de aangemelde gebruiker en de machtigingen worden aangevraagd. De logica is:
      - Als er slechts zijn gebruikers toestemming kunnen machtigingen worden aangevraagd en de aangemelde gebruiker niet een beheerder is, kan de gebruiker toestemming van de gebruiker voor de aangevraagde machtigingen verlenen.
      - Als ten minste één machtiging die beheerderstoestemming vereist is dat wordt aangevraagd en de aangemelde gebruiker niet een beheerder is, is de gebruiker een fout opgetreden bij het verlenen van toestemming.
      - Als de aangemelde gebruiker een beheerder is, wordt toestemming van een beheerder gekregen voor de aangevraagde machtigingen.
   - Alleen een beheerder kan toestemming verlenen in de nieuwe ervaring. Wanneer een beheerder selecteert de **verlenen van toestemming van een beheerder** knop, toestemming van een beheerder is verleend aan de vereiste machtigingen.

## <a name="deleting-an-app-registration"></a>Een app-registratie verwijderen

In de traditionele ervaring had een app moet één tenant moet worden verwijderd. De verwijderknop is uitgeschakeld voor apps met meerdere tenants. In de nieuwe ervaring apps kunnen worden verwijderd in een staat, maar u moet bevestigen dat de actie. Zie voor meer informatie over het verwijderen van app-registraties [in deze Quick Start](quickstart-remove-app.md).

## <a name="application-manifest"></a>Manifest van de toepassing

De oude en nieuwe ervaringen verschillende versies gebruiken voor de indeling van de JSON in de editor van het manifest. Zie voor meer informatie, [toepassingsmanifest](reference-app-manifest.md).

## <a name="new-ui"></a>Nieuwe gebruikersinterface

Er is een nieuwe gebruikersinterface voor eigenschappen die kan eerder alleen worden ingesteld met behulp van de editor van het manifest of de API of niet bestaat.

- **Impliciet verlenen stroom** (oauth2AllowImplicitFlow) kunt u vinden op de **verificatie** pagina. In tegenstelling tot in de traditionele ervaring, kunt u inschakelen **toegangstokens** of **id-tokens**, of beide.
- **Bereiken die zijn gedefinieerd door deze API** (oauth2Permissions) en **geautoriseerd clienttoepassingen** (preAuthorizedApplications) kan worden geconfigureerd via de **beschikbaar maken van een API** pagina. Zie voor meer informatie over het configureren van een app om te worden van een web-API en machtigingen /-bereiken beschikbaar [in deze Quick Start](quickstart-configure-app-expose-web-apis.md).
- **Uitgeversdomein** (die wordt weergegeven aan gebruikers op de [van de toepassing toestemmingsprompt](application-consent-experience.md)) kunt u vinden op de **Branding-blade** pagina. Zie voor meer informatie over het configureren van een uitgeversdomein [deze instructies](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Beperkingen

De nieuwe ervaring heeft de volgende beperkingen:

- De nieuwe ervaring is momenteel niet beschikbaar in Azure AD B2C-tenants.
- De indeling van de client-geheimen (app-wachtwoorden) is anders dan die van de traditionele ervaring en CLI verbroken.
- Wijzigen van de waarde voor ondersteunde accounts wordt niet ondersteund in de gebruikersinterface. U moet de app-manifest gebruiken, tenzij u tussen één tenant en meerdere tenants met Azure AD overstapt.
