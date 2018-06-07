---
title: Voorwaarden van de Service en de privacyverklaring voor Azure AD-apps | Microsoft Docs
description: Meer informatie over hoe u de voorwaarden van de service en de privacyverklaring voor apps die zijn geregistreerd voor het gebruik van Azure AD kunt configureren.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: celested
ms.reviwer: lenalepa; sureshja
ms.custom: aaddev
ms.openlocfilehash: 8abd9c35c7d979e1e1a42dd7e178d802f06c4227
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654904"
---
# <a name="terms-of-service-and-privacy-statement-for-registered-azure-active-directory-apps"></a>Voorwaarden van de service en de privacyverklaring voor geregistreerde Azure Active Directory-apps

Ontwikkelaars die ontwikkelen en beheren van apps die zijn geïntegreerd met Azure Active Directory (Azure AD) en Microsoft-accounts moeten bevatten koppelingen naar de app voorwaarden van de service en de privacyverklaring. De voorwaarden van de service en de privacyverklaring worden voor gebruikers via de ervaring van de gebruiker toestemming opgehaald. Ze helpen uw gebruikers laten weten dat ze uw app kunnen vertrouwen. De voorwaarden van de service en de privacyverklaring zijn vooral kritieke voor gebruikersgerichte multitenant apps--apps die worden gebruikt door meerdere mappen of beschikbaar zijn voor alle Microsoft-account.

U bent zelf verantwoordelijk voor het maken van de voorwaarden van de service- en privacy-instructie documenten voor uw app en voor het ontwikkelen van de URL's tot deze documenten. Voor multitenant-apps die deze koppelingen is mislukt, wordt de ervaring van de gebruiker toestemming voor uw app een waarschuwing die kan voorkomen dat gebruikers ermee akkoord dat uw app weergegeven.

> [!NOTE]
> * Apps voor één tenant wordt een waarschuwing weergegeven.
> * Als een of beide van de twee koppelingen ontbreken, wordt uw app een waarschuwing weergegeven.

## <a name="user-consent-experience"></a>Toestemming gebruikerservaring

De volgende voorbeelden ziet de gebruiker toestemming geven ervaring wanneer de voorwaarden van de service en de privacyverklaring zijn geconfigureerd en wanneer deze koppelingen zijn niet geconfigureerd.

![Schermafbeeldingen met en zonder een privacyverklaring en de voorwaarden van de service die wordt geleverd](./media/active-directory-integrating-applications/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Koppelingen naar de voorwaarden van de service- en privacy-instructie documenten opmaak

Voordat u koppelingen met de voorwaarden van uw app service- en privacy-instructie documenten toevoegt, zorg er dan voor dat de URL's aan deze richtlijnen voldoet.

| Richtlijn     | Beschrijving                           |
|---------------|---------------------------------------|
| Indeling        | Geldige URL                             |
| Geldige schema 's | HTTP en HTTPS</br>Aangeraden HTTPS |
| Max. lengte    | 2048 tekens bestaan                       |

Voorbeelden: `https://myapp.com/terms-of-service` en `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Koppelingen toe te voegen met de voorwaarden van de service en de privacyverklaring

Als de voorwaarden van de service en de privacyverklaring klaar bent, kunt u koppelingen toevoegen aan deze documenten in uw app met een van deze methoden:
* [Via de Azure portal](#registered-in-azure-portal)
* [In de Portal voor registratie van toepassing, of Developer Center](#registered-in-app-reg-portal)
* [Het app-object JSON gebruiken](#app-object-json)
* [De bètaversie MSGraph REST-API](#msgraph-beta-rest-api)

### <a name="registered-in-azure-portal"></a>Als u uw app geregistreerd in de Azure portal
Volg deze stappen als u uw app geregistreerd in de Azure portal.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/).
2. Navigeer naar de **App registraties** sectie en selecteer uw app.
3. Open de **eigenschappen** sectie van de app.
4. Vul de **voorwaarden van de URL van de Service** en **URL van privacyverklaring van** velden.
5. Sla uw wijzigingen op.

![Sectie met voorwaarden properties van URL's en het privacybeleid instructie App](./media/active-directory-integrating-applications/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="registered-in-app-reg-portal"></a>Als u uw app in de Portal van de registratie van de toepassing geregistreerd
Volg deze stappen als u uw app geregistreerd in de Portal voor registratie van toepassing of Dev Center.

1. Aanmelden bij de [Registratieportal toepassing](https://apps.dev.microsoft.com/).
2. Uw app selecteren en schuif naar de **profiel** sectie.
3. Vul de **voorwaarden van de URL van de Service** en **URL van privacyverklaring van** velden.
4. Sla uw wijzigingen op.

![App-profielsectie met de voorwaarden van URL's voor service- en privacy-instructie](./media/active-directory-integrating-applications/app-registration-portal-profile-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a>Het app-object JSON gebruiken
Als u liever het app-object JSON rechtstreeks wijzigen, kunt u de manifest-editor in Azure portal of Portal voor registratie van toepassing op bevatten koppelingen naar uw app voorwaarden van de service en de privacyverklaring.

```json
    "informationalUrls": { 
        "termsOfService": “<your_terms_of_service_url>”, 
        "privacy": “<your_privacy_statement_url>” 
    }
```

### <a name="msgraph-beta-rest-api"></a>De bètaversie MSGraph REST-API
Om bij te werken via een programma al uw apps, kunt u de bètaversie MSGraph REST-API om bij te werken van alle apps voor koppelingen met de voorwaarden van de service- en privacy-instructie documenten bevatten.

```
PATCH https://graph.microsoft.com/beta/applications/{application id}
{ 
    "appId": "{your application id}", 
    "info": { 
        "termsOfServiceUrl": "<your_terms_of_service_url>", 
        "supportUrl": null, 
        "privacyStatementUrl": "<your_privacy_statement_url>", 
        "marketingUrl": null, 
        "logoUrl": null 
    }
}
```

> [!NOTE]
> * Zorg ervoor dat u niet alle bestaande waarden die u hebt toegewezen aan een van deze velden overschrijven: `supportUrl`, `marketingUrl`, en `logoUrl`
> * De bètaversie MSGraph REST-API werkt alleen wanneer u zich met een Azure AD-account aanmeldt. Persoonlijke Microsoft-accounts worden niet ondersteund.