---
title: Gebruiksrechtovereenkomst en privacyverklaring voor apps | Azure
description: Meer informatie over hoe u de gebruiksvoorwaarden en privacyverklaring voor apps die zijn geregistreerd voor het gebruik van Azure AD kunt configureren.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviwer: lenalepa, sureshja
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 399f72c7819378ddc1566c2bdde94801b2c71687
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235293"
---
# <a name="how-to-configure-terms-of-service-and-privacy-statement-for-an-app"></a>Procedure: Gebruiksvoorwaarden en privacyverklaring voor een app configureren

Ontwikkelaars die ontwikkelen en beheren van apps die met Azure Active Directory (Azure AD) en Microsoft-accounts integreren moeten koppelingen met de gebruiksvoorwaarden en privacyverklaring van de app bevatten. De gebruiksvoorwaarden en privacyverklaring worden opgehaald voor gebruikers via de ervaring van de gebruiker toestemming. Ze helpen uw gebruikers weten dat ze uw app kunnen vertrouwen. De gebruiksvoorwaarden en privacyverklaring zijn vooral van cruciaal belang voor gebruikersgerichte apps met meerdere tenants--apps die worden gebruikt door meerdere mappen of beschikbaar zijn voor alle Microsoft-account.

U bent verantwoordelijk voor het maken van de servicevoorwaarden en privacyverklaring instructie documenten voor uw app en voor het leveren van de URL's tot deze documenten. Voor apps met meerdere tenants die niet voldoen aan deze koppelingen, wordt de ervaring van de gebruiker toestemming voor uw app een waarschuwing, die kan voorkomen dat gebruikers stemt ermee in dat aan uw app weergegeven.

> [!NOTE]
> * Apps met één tenant wordt een waarschuwing weergegeven.
> * Als een of beide van de twee koppelingen ontbreekt, wordt uw app een waarschuwing weergegeven.

## <a name="user-consent-experience"></a>Gebruikerservaring voor toestemming

De volgende voorbeelden ziet de gebruiker toestemming geven ervaring wanneer de voorwaarden van de service en de privacyverklaring zijn geconfigureerd en wanneer deze koppelingen zijn niet geconfigureerd.

![Schermafbeeldingen met en zonder een privacyverklaring en voorwaarden van de service die wordt geleverd](./media/howto-add-terms-of-service-privacy-statement/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Koppelingen naar de servicevoorwaarden en privacyverklaring instructie documenten opmaak

Voordat u koppelingen naar van uw app en de gebruiksvoorwaarden en privacyverklaring instructie documenten toevoegen, controleert u dat de URL's de volgende richtlijnen hanteren.

| Richtlijn     | Description                           |
|---------------|---------------------------------------|
| Indeling        | Geldige URL                             |
| Geldige schema 's | HTTP en HTTPS<br/>We raden aan HTTPS |
| Maximumlengte    | 2048 tekens                       |

Voorbeelden: `https://myapp.com/terms-of-service` en `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Koppelingen toe te voegen met de gebruiksvoorwaarden en privacyverklaring

Als de gebruiksrechtovereenkomst en privacyverklaring klaar bent, kunt u koppelingen toevoegen aan deze documenten in uw app met behulp van een van de volgende methoden:

* [Via de Azure-portal](#azure-portal)
* [Met behulp van de app van JSON-object](#app-object-json)
* [Met behulp van de bètaversie van MSGraph REST-API](#msgraph-beta-rest-api)

### <a name="azure-portal"></a>Met behulp van de Azure portal
Volg deze stappen in de Azure-portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Navigeer naar de **App-registraties** sectie en selecteer uw app.
3. Open de **Branding** deelvenster.
4. Vul de **voorwaarden van de URL van de Service** en **URL van privacyverklaring van** velden.
5. Sla uw wijzigingen op.

    ![App sectie met eigenschappen met de voorwaarden van de service en privacy statement-URL 's](./media/howto-add-terms-of-service-privacy-statement/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a>Met behulp van de app van JSON-object

Als u liever de JSON-object van de app rechtstreeks te wijzigen, kunt u de manifest-editor in Azure portal of de Portal voor Appregistratie gebruiken om op te nemen van koppelingen met de gebruiksvoorwaarden en privacyverklaring van uw app.

```json
    "informationalUrls": { 
        "termsOfService": "<your_terms_of_service_url>", 
        "privacy": "<your_privacy_statement_url>" 
    }
```

### <a name="msgraph-beta-rest-api"></a>Met behulp van de bètaversie van MSGraph REST-API

Om bij te werken via een programma al uw apps, kunt u de bètaversie van MSGraph REST-API om bij te werken van al uw apps zodanig dat koppelingen naar de servicevoorwaarden en privacyverklaring instructie documenten.

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
> * De bètaversie van MSGraph REST-API werkt alleen wanneer u zich met een Azure AD-account aanmeldt. Persoonlijke Microsoft-accounts worden niet ondersteund.
