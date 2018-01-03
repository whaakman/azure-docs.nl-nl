---
title: Een Azure gehoste API exporteren naar PowerApps en Microsoft-stroom | Microsoft Docs
description: Overzicht van het blootstellen van een gehoste API in App Service PowerApps en Microsoft-Flow
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 12/15/2017
ms.author: mahender; mblythe
ms.openlocfilehash: 7482ca27c2edcb281180fb8fbbfb1884a515d379
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Een Azure gehoste API exporteren naar PowerApps en Microsoft-stroom

[PowerApps](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) is een service voor het bouwen en met behulp van aangepaste zakelijke apps die verbinding maken met uw gegevens en alle platforms. [Microsoft-Flow](https://flow.microsoft.com/guided-learning/learning-introducing-flow/) kunt u gemakkelijk om werkstromen en bedrijfsprocessen tussen uw favoriete apps en services te automatiseren. Zowel PowerApps als Microsoft Flow worden geleverd met diverse ingebouwde connectors met gegevensbronnen, zoals Office 365, Dynamics 365 en Salesforce. In sommige gevallen wilt app en stroom opbouwfuncties ook verbinding maken met gegevensbronnen en API's die zijn gebouwd door hun organisatie.

Op deze manier kunnen ontwikkelaars die u beschikbaar wilt maken van hun API's binnen een organisatie grotere schaal beschikbaar maken hun API's voor app en stroom opbouwfuncties. Dit onderwerp leest u het exporteren van een API die is gebouwd met [Azure Functions](../azure-functions/functions-overview.md) of [Azure App Service](../app-service/app-service-web-overview.md). De geëxporteerde API wordt een *aangepaste connector*, die wordt gebruikt in PowerApps en Microsoft-Flow net als een ingebouwde connector.

## <a name="create-and-export-an-api-definition"></a>Maken en exporteren van een API-definitie
Voordat u exporteert een API, moet u de API met behulp van de definitie van een OpenAPI beschrijven (voorheen bekend als een [Swagger](http://swagger.io/) bestand). Deze definitie bevat informatie over welke bewerkingen zijn beschikbaar in een API en hoe de gegevens van de aanvraag en -antwoord voor de API moet worden opgebouwd. PowerApps en Microsoft-Flow kunt maken van aangepaste connectors voor de definitie van een OpenAPI 2.0. Azure Functions en Azure App Service hebben een ingebouwde ondersteuning voor het maken, die als host fungeert en het beheren van OpenAPI definities. Zie voor meer informatie [een RESTful-API maken in Azure Web Apps](../app-service/app-service-web-tutorial-rest-api.md).

> [!NOTE]
> U kunt ook aangepaste connectors in de PowerApps en Microsoft stromen gebruikersinterface maken zonder gebruik van de definitie van een OpenAPI. Zie voor meer informatie [registreren en gebruik een aangepaste connector (PowerApps)](https://powerapps.microsoft.com/tutorials/register-custom-api/) en [registreren en gebruik een aangepaste connector (Microsoft Flow)](https://flow.microsoft.com/documentation/register-custom-api/).

Als u wilt exporteren van de API-definitie, de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), navigeer naar uw Azure-functies of een andere App Service-toepassing.

    Als u Azure Functions, selecteert u de functie-app, kiest u **platformfuncties**, en vervolgens **API-definitie**.

    ![Azure Functions-API-definitie](media/app-service-export-api-to-powerapps-and-flow/api-definition-function.png)

    Als u Azure App Service gebruikt, selecteert u **API-definitie** uit de lijst met instellingen.

    ![App Service API-definitie](media/app-service-export-api-to-powerapps-and-flow/api-definition-app.png)

2. De **exporteren naar PowerApps + Microsoft Flow** knop beschikbaar moet zijn (als dit niet, moet u eerst de definitie van een OpenAPI maken). Klik op deze knop om te beginnen met de export.

    ![Exporteren naar PowerApps + Microsoft Flow knop](media/app-service-export-api-to-powerapps-and-flow/export-apps-flow.png)

3. Selecteer de **modus exporteren**:

    **Express** kunt u de aangepaste connector uit binnen de Azure portal maken. Hiervoor is dat u bent aangemeld bij PowerApps of Microsoft Flow en gemachtigd connectors maken in de doelomgeving. Dit is de aanbevolen aanpak als deze twee vereisten kunnen worden voldaan. Als u deze modus gebruikt, volgt u de [snelle exporteren gebruiken](#express) onderstaande instructies.

    **Handmatige** kunt u het exporteren van de API-definitie die u vervolgens importeren met behulp van de portals PowerApps of Flow van Microsoft. Dit is de aanbevolen aanpak als de Azure-gebruiker en de gebruiker met machtigingen voor het maken van connectors verschillende personen of als de connector moet worden gemaakt in een andere Azure-tenant. Als u deze modus gebruikt, volgt u de [gebruiken voor het handmatig exporteren](#manual) onderstaande instructies.

    ![Modus exporteren](media/app-service-export-api-to-powerapps-and-flow/export-mode.png)

> [!NOTE]
> De aangepaste connector gebruikt een *kopie* van de API-definitie, zodat PowerApps en Microsoft-Flow niet onmiddellijk weten als u wijzigingen in de toepassing en de API-definitie aanbrengt. Als u wijzigingen aanbrengen, herhaalt u de Exportstappen voor de nieuwe versie.

<a name="express"></a>
## <a name="use-express-export"></a>Gebruik snelle exporteren

Voltooien van de uitvoer in **Express** -modus als volgt te werk:

1. Zorg ervoor dat u bent aangemeld bij de PowerApps of Microsoft-Flow-tenant waarmee u wilt exporteren. 

2. Gebruik de instellingen die zijn opgegeven in de tabel.

    |Instelling|Beschrijving|
    |--------|------------|
    |**Omgeving**|Selecteer de omgeving waarin de aangepaste connector moet worden opgeslagen. Zie voor meer informatie [omgevingen overzicht](https://powerapps.microsoft.com/tutorials/environments-overview/).|
    |**De naam van de aangepaste API gebruiken**|Voer een naam, welke PowerApps en Microsoft-Flow opbouwfuncties te krijgen in de lijst van de connector zien.|
    |**Configuratie van beveiliging voorbereiden**|Als vereist, geeft u de configuratiegegevens van de beveiliging die nodig zijn voor gebruikers toegang verlenen tot uw API. In dit voorbeeld ziet een API-sleutel. Zie voor meer informatie [Geef verificatietype](#auth) hieronder.|
 
    ![Exporteren naar PowerApps en Microsoft-Flow Express](media/app-service-export-api-to-powerapps-and-flow/export-express.png)

3. Klik op **OK**. De aangepaste connector is nu gemaakt en toegevoegd aan de omgeving die u hebt opgegeven.

Voor voorbeelden van het gebruik van **Express** modus met Azure Functions, Zie [aanroepen van een functie van PowerApps](functions-powerapps-scenario.md) en [aanroepen van een functie van Microsoft-Flow](functions-flow-scenario.md).

<a name="manual"></a>
## <a name="use-manual-export"></a>Gebruik voor het handmatig exporteren

Voltooien van de uitvoer in **handmatige** -modus als volgt te werk:

1. Klik op **downloaden** en sla het bestand op of klik op de knop kopiëren en opslaan van de URL. U gebruikt tijdens het importeren van het gedownloade bestand of de URL.
 
    ![Handmatige exporteren naar PowerApps en Microsoft-Flow](media/app-service-export-api-to-powerapps-and-flow/export-manual.png)
 
2. Als uw API-definitie definities beveiliging bevat, worden deze specifiek genoemd in stap #2. Tijdens het importeren, PowerApps en Microsoft-Flow detecteert deze en geeft voor informatie over beveiliging. Verzamel de referenties die zijn gerelateerd aan elke definitie voor gebruik in de volgende sectie. Zie voor meer informatie [Geef verificatietype](#auth) hieronder.

    ![Beveiliging voor het handmatig exporteren](media/app-service-export-api-to-powerapps-and-flow/export-manual-security.png)

    In dit voorbeeld toont de definitie van de API-sleutelbeveiliging die is opgenomen in de definitie van OpenAPI.

Nu dat u de API-definitie hebt geëxporteerd, importeren om te maken van een aangepaste connector in PowerApps en Microsoft-Flow. Aangepaste connectors worden gedeeld tussen de twee services, dus u moet slechts één keer in de definitie voor het importeren.

U kunt de API-definitie importeren in PowerApps en Microsoft-Flow, de volgende stappen uit:

1. Ga naar [powerapps.com](https://web.powerapps.com) of [flow.microsoft.com](https://flow.microsoft.com).

2. Klik op het pictogram tandwielpictogram in de rechterbovenhoek en klik op **aangepaste connectors**.

   ![Versnelling pictogram in de service](media/app-service-export-api-to-powerapps-and-flow/icon-gear.png)

3. Klik op **maken van aangepaste connector**, klikt u vervolgens op **importeren van de definitie van een OpenAPI**.

   ![Aangepaste connector maken](media/app-service-export-api-to-powerapps-and-flow/flow-apps-create-connector.png)

4. Voer een naam voor de aangepaste connector vervolgens gaat u naar de OpenAPI definitie die u hebt geëxporteerd en klik op **doorgaan**.

   ![OpenAPI definitie uploaden](media/app-service-export-api-to-powerapps-and-flow/flow-apps-upload-definition.png)

4. Op de **algemene** tabblad, lees de informatie die afkomstig zijn van de definitie van de OpenAPI.

5. Op de **beveiliging** tabblad als u wordt gevraagd om te voorzien van verificatiegegevens, voer de waarden die geschikt is voor het verificatietype. Klik op **gaan**.

    ![Tabblad Beveiliging](media/app-service-export-api-to-powerapps-and-flow/tab-security.png)

    Dit voorbeeld ziet de vereiste velden voor verificatie van API-sleutel. De velden verschillen afhankelijk van het verificatietype.

6. Op de **definities** tabblad alle bewerkingen die zijn gedefinieerd in uw bestand OpenAPI worden automatisch ingevuld. Als alle vereiste bewerkingen zijn gedefinieerd, kunt u gaan met de volgende stap. Zo niet, u kunt toevoegen en bewerkingen hier wijzigen.

    ![Tabblad definities](media/app-service-export-api-to-powerapps-and-flow/tab-definitions.png)

    In dit voorbeeld heeft één bewerking, met de naam `CalculateCosts`. De metagegevens, bijvoorbeeld **beschrijving**, alle afkomstig is van het bestand OpenAPI.

7. Klik op **-connector maken** boven aan de pagina.

U kunt nu verbinding met de aangepaste connector in PowerApps en Microsoft-Flow. Zie voor meer informatie over het maken van connectors in de PowerApps en Microsoft-Flow-portals [registreren van uw aangepaste connector (PowerApps)](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector) en [registreren van uw aangepaste connector (Microsoft Flow)](https://flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector).

<a name="auth"></a>
## <a name="specify-authentication-type"></a>Geef een verificatie

PowerApps en Microsoft-Flow ondersteuning voor een verzameling van id-providers die verificatie voor aangepaste connectors bieden. Als uw API is verificatie vereist, zorg ervoor dat deze wordt vastgelegd als een _beveiliging definitie_ in uw document OpenAPI op het volgende voorbeeld:

```json
"securityDefinitions": {
    "AAD": {
    "type": "oauth2",
    "flow": "accessCode",
    "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
    "scopes": {}
    }
}
``` 
Tijdens het exporteren bieden u configuratiewaarden waarmee PowerApps en Microsoft-Flow om gebruikers te verifiëren.

Deze sectie bevat informatie over de verificatietypen die worden ondersteund in **Express** modus: API-sleutel, Azure Active Directory en algemene OAuth 2.0. PowerApps en Microsoft-Flow ook ondersteuning voor basisverificatie en OAuth 2.0 voor specifieke services zoals Dropbox, Facebook en SalesForce.

### <a name="api-key"></a>API-sleutel
Wanneer u een API-sleutel gebruikt, worden de gebruikers van de connector wordt gevraagd naar de sleutel opgeven wanneer ze een verbinding maakt. U opgeven de naam om te begrijpen welke sleutel is vereist in een API-sleutel. In het vorige voorbeeld gebruiken we de naam van de `API Key (contact meganb@contoso.com)` zodat mensen waar weten u informatie ophalen over de API-sleutel. Voor Azure Functions is de sleutel meestal een van de sleutels van de host die betrekking hebben op de verschillende functies in de functie-app.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
Als u Azure AD, moet u twee registraties voor Azure AD-toepassing: één voor de API zelf en één voor de aangepaste connector:

- Voor meer informatie over het configureren van inschrijving voor de API gebruiken de [App Service-verificatie/autorisatie](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md) functie.

- Volg de stappen in registratie voor de connector configureren [toevoegen van een Azure AD-toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). De registratie als er toegang tot uw API en antwoord-URL van de `https://msmanaged-na.consent.azure-apim.net/redirect`. 

Zie voor meer informatie de voorbeelden van Azure AD-registratie voor [PowerApps](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) en [Microsoft Flow](https://flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/). Deze voorbeelden Azure Resource Manager gebruiken als de API; vervangen door uw API als u deze stappen uitvoert.

De volgende configuratiewaarden zijn vereist:
- **Client-ID** -de client-ID van uw Azure AD-registratie-connector
- **Clientgeheim** -het clientgeheim van uw Azure AD-registratie-connector
- **Aanmeldings-URL** -de basis-URL voor Azure AD. In Azure, is dit doorgaans `https://login.windows.net`.
- **Tenant-ID** -de ID van de tenant moet worden gebruikt voor de aanmelding. Dit moet 'algemene' of de ID van de tenant waarin de connector is gemaakt.
- **Bron-URL** -de bron-URL van de Azure AD-registratie voor uw API

> [!IMPORTANT]
> Als iemand anders de API-definitie in PowerApps en Microsoft-Flow als onderdeel van de handmatige stroom importeert wordt, moet u ze opgeven met de client-ID en clientgeheim van de *registratie van de connector*, evenals de bron-URL van uw API. Zorg ervoor dat deze geheime gegevens veilig worden beheerd. **De beveiligingsreferenties van de API zelf niet delen.**

### <a name="generic-oauth-20"></a>Generieke OAuth 2.0
Wanneer u algemene OAuth 2.0, die u kunt integreren met elke OAuth 2.0-provider. Hiermee kunt u werken met aangepaste providers die niet systeemeigen worden ondersteund.

De volgende configuratiewaarden zijn vereist:
- **Client-ID** -de OAuth 2.0-client-ID
- **Clientgeheim** -het clientgeheim OAuth 2.0
- **Autorisatie-URL** -OAuth 2.0-autorisatie-URL
- **URL token** -de URL van OAuth 2.0-token
- **Vernieuwen van de URL** -URL van de OAuth 2.0-vernieuwen


