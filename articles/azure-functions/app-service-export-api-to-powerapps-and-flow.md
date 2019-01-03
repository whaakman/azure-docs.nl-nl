---
title: Een Azure gehoste API exporteren naar PowerApps en Microsoft Flow | Microsoft Docs
description: Overzicht van hoe u een API die wordt gehost in App Service naar PowerApps en Microsoft Flow beschikbaar
services: app-service
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: app-service
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/15/2017
ms.author: glenga
ms.reviewer: sunayv
ms.openlocfilehash: 53aa40c047b2b5201cd74c0409d56e97d2880eb0
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602848"
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Een Azure gehoste API exporteren naar PowerApps en Microsoft Flow

[PowerApps](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) is een service voor het bouwen en gebruiken van aangepaste zakelijke apps die verbinding maken met uw gegevens en alle platforms. [Microsoft Flow](https://flow.microsoft.com/guided-learning/learning-introducing-flow/) kunt u eenvoudig voor het automatiseren van werkstromen en bedrijfsprocessen tussen uw favoriete apps en services. PowerApps en Microsoft Flow worden geleverd met tal van ingebouwde connectors met gegevensbronnen, zoals Office 365, Dynamics 365, Salesforce en meer. In sommige gevallen willen builders van Apps en stromen ook verbinding maken met gegevensbronnen en API's die zijn gemaakt door hun organisatie.

Op deze manier kunnen ontwikkelaars die beschikbaar wilt stellen hun API's breder binnen een organisatie hun API's beschikbaar maken voor builders van Apps en stromen. Dit onderwerp leest u hoe u een API die zijn gebouwd met exporteert [Azure Functions](../azure-functions/functions-overview.md) of [Azure App Service](../app-service/overview.md). De geëxporteerde API wordt een *aangepaste connector*, die wordt gebruikt in PowerApps en Microsoft Flow net als een ingebouwde connector.

## <a name="create-and-export-an-api-definition"></a>Maken en exporteren van een API-definitie
Voordat u exporteert een API, beschrijft u de API met behulp van een OpenAPI-definitie (voorheen bekend als een [Swagger](https://swagger.io/) bestand). Deze definitie bevat informatie over welke bewerkingen beschikbaar zijn in een API en hoe de gegevens van de aanvraag en respons voor de API moeten worden opgebouwd. PowerApps en Microsoft Flow kunt maken van aangepaste connectors voor de definitie van een OpenAPI 2.0. Azure Functions en Azure App Service hebben ingebouwde ondersteuning voor het maken, hosten en beheren van OpenAPI-definities. Zie voor meer informatie, [een RESTful API hosten met CORS in Azure App Service](../app-service/app-service-web-tutorial-rest-api.md).

> [!NOTE]
> U kunt ook aangepaste connectors in PowerApps en Microsoft Flow-gebruikersinterface bouwen zonder gebruik van een OpenAPI-definitie. Zie voor meer informatie, [registreren en gebruik een aangepaste connector (PowerApps)](https://powerapps.microsoft.com/tutorials/register-custom-api/) en [registreren en gebruik een aangepaste connector (Microsoft Flow)](https://flow.microsoft.com/documentation/register-custom-api/).

Volg deze stappen voor het exporteren van de API-definitie:

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw Azure Functions of een andere App Service-toepassing.

    Als u Azure Functions, selecteer uw functie-app, kies **platformfuncties**, en vervolgens **API-definitie**.

    ![Azure Functions API-definitie](media/app-service-export-api-to-powerapps-and-flow/api-definition-function.png)

    Als u Azure App Service, selecteert u **API-definitie** uit de lijst met instellingen.

    ![App Service API-definitie](media/app-service-export-api-to-powerapps-and-flow/api-definition-app.png)

2. De **exporteren naar PowerApps en Microsoft Flow** knop beschikbaar moet zijn (als dat niet het geval is, wordt u moet eerst een OpenAPI-definitie maken). Klik op deze knop om te beginnen met de export.

    ![Exporteren naar PowerApps en Microsoft Flow-knop](media/app-service-export-api-to-powerapps-and-flow/export-apps-flow.png)

3. Selecteer de **exportmodus**:

    **Express** kunt u de aangepaste connector uit in Azure portal maken. Hiervoor is dat u zich hebt aangemeld bij PowerApps of Microsoft Flow en gemachtigd connectors maken in de doelomgeving. Dit is de aanbevolen aanpak als deze twee vereisten kan worden voldaan. Als u dit gebruikt, volgt u de [snelle exports](#express) onderstaande instructies.

    **Handmatige** kunt u de API-definitie, dat u kunt exporteren en importeren met behulp van de portals PowerApps of Microsoft Flow. Dit is de aanbevolen aanpak als de Azure-gebruiker en de gebruiker met de machtiging voor het maken van connectors verschillende personen zijn of als de connector moet worden gemaakt in een andere Azure-tenant. Als u dit gebruikt, volgt u de [gebruik de handmatige exportfunctie](#manual) onderstaande instructies.

    ![Exportmodus](media/app-service-export-api-to-powerapps-and-flow/export-mode.png)

> [!NOTE]
> De aangepaste connector gebruikt een *kopie* van de API-definitie, zodat PowerApps en Microsoft Flow weet niet meteen dat als u wijzigingen in de toepassing en de API-definitie aanbrengt. Als u wijzigingen maakt, herhaalt u de Exportstappen voor de nieuwe versie.

<a name="express"></a>
## <a name="use-express-export"></a>Gebruik snelle exporteren

Om uit te voeren van de export in **Express** modus, als volgt te werk:

1. Zorg ervoor dat u bent aangemeld bij PowerApps of Microsoft Flow-tenant die u wilt exporteren. 

2. Gebruik de instellingen zoals opgegeven in de tabel.

    |Instelling|Description|
    |--------|------------|
    |**Omgeving**|Selecteer de omgeving waarin de aangepaste connector moet worden opgeslagen. Zie voor meer informatie, [overzicht van omgevingen](https://powerapps.microsoft.com/tutorials/environments-overview/).|
    |**Naam van de aangepaste API**|Voer een naam en welke PowerApps en Microsoft Flow builders in de lijst van de connector zien.|
    |**Beveiligingsconfiguratie voorbereiden**|Als vereist, geeft u de configuratiegegevens van de beveiliging die nodig zijn voor gebruikers toegang verlenen tot uw API. In dit voorbeeld ziet u een API-sleutel. Zie voor meer informatie, [Geef het verificatietype](#auth) hieronder.|
 
    ![Snelle exporteren naar PowerApps en Microsoft Flow](media/app-service-export-api-to-powerapps-and-flow/export-express.png)

3. Klik op **OK**. De aangepaste connector is nu gemaakt en toegevoegd aan de omgeving die u hebt opgegeven.

Voor voorbeelden van het gebruik van **Express** modus met Azure Functions, Zie [een functie van PowerApps aanroepen](functions-powerapps-scenario.md) en [een functie van Microsoft Flow aanroepen](functions-flow-scenario.md).

<a name="manual"></a>
## <a name="use-manual-export"></a>Gebruik de handmatige exportfunctie

Om uit te voeren van de export in **handmatig** modus, als volgt te werk:

1. Klik op **downloaden** en sla het bestand, of klik op de knop kopiëren en opslaan van de URL. U gebruikt tijdens het importeren van het gedownloade bestand of de URL.
 
    ![Voor het handmatig exporteren naar PowerApps en Microsoft Flow](media/app-service-export-api-to-powerapps-and-flow/export-manual.png)
 
2. Als uw API-definitie beveiligingsdefinities bevat, worden deze in stap 2 # out genoemd. Tijdens het importeren, PowerApps en Microsoft Flow detecteert deze en geeft voor informatie over beveiliging. Verzamel de referenties die betrekking hebben op elke definitie voor gebruik in de volgende sectie. Zie voor meer informatie, [Geef het verificatietype](#auth) hieronder.

    ![Beveiliging voor het handmatig exporteren](media/app-service-export-api-to-powerapps-and-flow/export-manual-security.png)

    Dit voorbeeld toont de definitie van de API-sleutels die is opgenomen in de OpenAPI-definitie.

Nu dat u de API-definitie hebt geëxporteerd, importeert u het voor het maken van een aangepaste connector in PowerApps en Microsoft Flow. Aangepaste connectors worden gedeeld tussen de twee services, dus u alleen moet voor het importeren van de definitie van één keer.

U kunt de API-definitie importeren in PowerApps en Microsoft Flow, de volgende stappen uit:

1. Ga naar [powerapps.com](https://web.powerapps.com) of [flow.microsoft.com](https://flow.microsoft.com).

2. Klik op het tandwielpictogram in de rechterbovenhoek en klik op **aangepaste connectors**.

   ![Tandwielpictogram in service](media/app-service-export-api-to-powerapps-and-flow/icon-gear.png)

3. Klik op **aangepaste connector maken**, klikt u vervolgens op **importeren van een OpenAPI-definitie**.

   ![Een aangepaste connector maken](media/app-service-export-api-to-powerapps-and-flow/flow-apps-create-connector.png)

4. Voer een naam voor de aangepaste connector, navigeer naar de OpenAPI-definitie die u hebt geëxporteerd, en klikt u op **doorgaan**.

   ![OpenAPI-definitie uploaden](media/app-service-export-api-to-powerapps-and-flow/flow-apps-upload-definition.png)

4. Op de **algemene** tabblad, lees de informatie die afkomstig zijn uit de OpenAPI-definitie.

5. Op de **Security** tabblad als u wordt gevraagd om te voorzien van verificatiegegevens, voert u de waarden die geschikt is voor het verificatietype. Klik op **Doorgaan**.

    ![Tabblad Beveiliging](media/app-service-export-api-to-powerapps-and-flow/tab-security.png)

    In dit voorbeeld ziet u de vereiste velden voor verificatie van API-sleutel. De velden variëren, afhankelijk van het verificatietype.

6. Op de **definities** tabblad alle bewerkingen die zijn gedefinieerd in het OpenAPI-bestand worden automatisch ingevuld. Als al uw vereiste bewerkingen zijn gedefinieerd, gaat u naar de volgende stap. Zo niet, u kunt toevoegen en bewerkingen hier wijzigen.

    ![Tabblad definities](media/app-service-export-api-to-powerapps-and-flow/tab-definitions.png)

    In dit voorbeeld heeft één bewerking, met de naam `CalculateCosts`. De metagegevens, bijvoorbeeld **beschrijving**, alle afkomstig is van het OpenAPI-bestand.

7. Klik op **-connector maken** aan de bovenkant van de pagina.

U kunt nu verbinding maken met de aangepaste connector in PowerApps en Microsoft Flow. Zie voor meer informatie over het maken van connectors in PowerApps en Microsoft Flow-portals [Registreer uw aangepaste connector (PowerApps)](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector) en [Registreer uw aangepaste connector (Microsoft Flow)](https://flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector).

<a name="auth"></a>
## <a name="specify-authentication-type"></a>Verificatietype opgeven

Een verzameling van id-providers die verificatie voor aangepaste connectors bieden ondersteuning voor PowerApps en Microsoft Flow. Als uw API is verificatie vereist, zorgt u ervoor dat deze wordt vastgelegd als een _beveiligingsdefinitie_ in uw OpenAPI-document, zoals in het volgende voorbeeld:

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
Tijdens het exporteren bieden u configuratiewaarden waarmee PowerApps en Microsoft Flow om gebruikers te verifiëren.

In deze sectie worden de verificatietypen die worden ondersteund in **Express** modus: API-sleutel, Azure Active Directory en algemene OAuth 2.0. PowerApps en Microsoft Flow ook ondersteuning voor basisverificatie en OAuth 2.0 voor specifieke services zoals Dropbox, Facebook en SalesForce.

### <a name="api-key"></a>API-sleutel
Wanneer u een API-sleutel gebruikt, worden de gebruikers van de connector wordt gevraagd de sleutel opgeven bij het maken van een verbinding. U de sleutelnaam van een API om te begrijpen welke sleutel is vereist. In het vorige voorbeeld gebruiken we de naam van de `API Key (contact meganb@contoso.com)` zodat iedereen waar weet u informatie over de API-sleutel. Voor Azure Functions is de sleutel meestal een van de hostsleutels, die betrekking hebben op verschillende functies in de functie-app.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
Wanneer u Azure AD, moet u twee Azure AD-toepassingsregistraties: één voor de API zelf, en één voor de aangepaste connector:

- Gebruik voor het configureren van inschrijving voor de API de [App Service-verificatie/autorisatie](../app-service/configure-authentication-provider-aad.md) functie.

- Volg de stappen in om u te registreren voor de connector configureert, [toevoegen van een Azure AD-toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). De registratie moet gedelegeerde toegang tot uw API en een antwoord-URL van `https://msmanaged-na.consent.azure-apim.net/redirect`. 

Zie voor meer informatie, de Azure AD-registratie-voorbeelden voor [PowerApps](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) en [Microsoft Flow](https://flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/). Deze voorbeelden als de API, Azure Resource Manager gebruiken vervangen door uw API als u de stappen volgen.

De volgende configuratiewaarden zijn vereist:
- **Client-ID** -de client-ID van uw Azure AD-registratie-connector
- **Clientgeheim** -het clientgeheim van uw Azure AD-registratie-connector
- **Aanmeldings-URL** -de basis-URL voor Azure AD. In Azure, is dit doorgaans `https://login.windows.net`.
- **Tenant-ID** -de ID van de tenant moet worden gebruikt voor de aanmelding. Dit moet 'algemene' of de ID van de tenant waarin de connector is gemaakt.
- **Bron-URL** -de bron-URL van de Azure AD-registratie voor uw API

> [!IMPORTANT]
> Als iemand anders de API-definitie in PowerApps en Microsoft Flow als onderdeel van de handmatige stroom importeren wordt, moet u deze opgeven met de client-ID en clientgeheim van de *connectorregistratie*, evenals de bron-URL van uw API. Zorg ervoor dat deze geheimen veilig worden beheerd. **De beveiligingsreferenties van de API zelf niet delen.**

### <a name="generic-oauth-20"></a>Generieke OAuth 2.0
Wanneer u algemene OAuth 2.0, kunt u integreren met een OAuth 2.0-provider. Hiermee kunt u werken met aangepaste providers die niet systeemeigen worden ondersteund.

De volgende configuratiewaarden zijn vereist:
- **Client-ID** -de OAuth 2.0-client-ID
- **Clientgeheim** -het clientgeheim van OAuth 2.0
- **Autorisatie-URL** -de autorisatie-URL voor OAuth 2.0
- **URL van token** -de URL van de OAuth 2.0-toegangstoken
- **URL vernieuwen** -de vernieuwings-URL voor OAuth 2.0


