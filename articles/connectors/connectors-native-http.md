---
title: Communicatie met een willekeurig eindpunt via HTTP - Azure Logic Apps | Microsoft Docs
description: Logische apps die kunnen communiceren met een willekeurig eindpunt te maken via HTTP
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 3eae7a4a47680fc36849fd413b76a80865cf3c9f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-http-action"></a>Aan de slag met de HTTP-actie

U kunt met de HTTP-actie werkstromen voor uw organisatie uitbreiden en communiceren met een willekeurig eindpunt via HTTP.

U kunt:

* Logic app-werkstromen waarmee (trigger) wordt geactiveerd wanneer een website die u beheert uitvalt maken.
* Naar een willekeurig eindpunt communiceren via HTTP voor het uitbreiden van uw werkstromen bij andere services.

Om te beginnen met de HTTP-actie in een logische app, Zie [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-http-trigger"></a>De HTTP-trigger te gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](connectors-overview.md).

Hier volgt een voorbeeld van de volgorde van het instellen van de HTTP-trigger in de ontwerpfunctie voor Logic App.

1. De HTTP-trigger toevoegen in uw logische app.
2. Vul de parameters voor het HTTP-eindpunt dat u wilt pollen.
3. Wijzig het terugkeerpatroon op hoe vaak het moet peilen.

   De logische app wordt nu gestart met inhoud die is geretourneerd tijdens elke controle.

   ![HTTP-trigger](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>De werking van de HTTP-trigger

De HTTP-trigger stuurt een aanroep van HTTP-eindpunt op een terugkerend interval. Standaard een HTTP-antwoordcode die lager is dan 300 zorgt ervoor dat een logische app om uit te voeren. U kunt om op te geven of de logische app moet worden geactiveerd, de logische app in de codeweergave bewerken en toevoegen van een voorwaarde die wordt geëvalueerd als na de HTTP-aanroep. Hier volgt een voorbeeld van een HTTP-trigger wordt geactiveerd wanneer de geretourneerde statuscode is groter dan of gelijk zijn aan `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Volledige details over de HTTP-trigger-parameters zijn beschikbaar op [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Gebruik de HTTP-actie

Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een logische app. 
[Meer informatie over acties](connectors-overview.md).

1. Kies **nieuwe stap** > **een actie toevoegen**.
3. Typ in het zoekvak actie **http** voor een lijst met de HTTP-acties.
   
    ![Selecteer de HTTP-actie](./media/connectors-native-http/using-action-1.png)

4. Voeg eventueel vereiste parameters voor de HTTP-aanroep.
   
    ![De HTTP-actie niet voltooien](./media/connectors-native-http/using-action-2.png)

5. Klik op de werkbalk designer **opslaan**. Uw logische app worden opgeslagen en gepubliceerd (geactiveerd) op hetzelfde moment.

## <a name="http-trigger"></a>HTTP-trigger
Hier volgen de details voor de trigger die ondersteuning biedt voor deze connector. De HTTP-connector heeft een trigger.

| Trigger | Beschrijving |
| --- | --- |
| HTTP |Maakt een HTTP-aanroep en retourneert de antwoordinhoud. |

## <a name="http-action"></a>HTTP-actie
Hier volgen de details voor de actie die ondersteuning biedt voor deze connector. De HTTP-connector heeft een mogelijke actie.

| Bewerking | Beschrijving |
| --- | --- |
| HTTP |Maakt een HTTP-aanroep en retourneert de antwoordinhoud. |

## <a name="http-details"></a>HTTP-details
De volgende tabellen beschrijven de vereiste en optionele invoervelden voor de actie en de bijbehorende uitvoerdetails die zijn gekoppeld met de actie.

#### <a name="http-request"></a>HTTP-aanvraag
Hier volgen de invoervelden voor de actie, waardoor een uitgaande HTTP-aanvraag.
A * houdt in dat een vereist veld.

| Weergavenaam | Naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Methode * |methode |De HTTP-term moet worden gebruikt |
| URI * |uri |De URI voor de HTTP-aanvraag |
| Kopteksten |headers |De HTTP-headers te nemen van een JSON-object |
| Hoofdtekst |hoofdtekst |De hoofdtekst van de HTTP-aanvraag |
| Verificatie |verificatie |Gegevens in de [verificatie](#authentication) sectie |

<br>

#### <a name="output-details"></a>Uitvoergegevens
Hier volgen de uitvoerdetails voor het HTTP-antwoord.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Kopteksten |object |Antwoordheaders |
| Hoofdtekst |object |Response-object |
| Statuscode |int |HTTP-statuscode |

## <a name="authentication"></a>Verificatie
De functie Logic Apps kunt u verschillende soorten verificatie op basis van HTTP-eindpunten. U kunt deze verificatie met de **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, en  **[HTTP-Webhook](connectors-native-webhook.md)**  connectors. De volgende soorten authenticatie worden geconfigureerd:

* [Basisverificatie](#basic-authentication)
* [Verificatie van clientcertificaten](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth-verificatie](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Basisverificatie

De volgende verificatieobject is nodig voor basisverificatie.
A * houdt in dat een vereist veld.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Type* |type |Type verificatie (moet `Basic` voor basisverificatie) |
| Gebruikersnaam * |gebruikersnaam |Gebruikersnaam voor verificatie |
| Wachtwoord * |wachtwoord |Wachtwoord voor verificatie |

> [!TIP]
> Als u wilt geen wachtwoord gebruiken dat niet uit de definitie van gebruik ophalen een `securestring` parameter en de `@parameters()`  
>  [definitie Werkstroomfunctie](http://aka.ms/logicappdocs).

Bijvoorbeeld:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Clientverificatie via certificaat

De volgende verificatieobject is nodig voor verificatie van clientcertificaten. A * houdt in dat een vereist veld.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Type* |type |Het type verificatie (moet `ClientCertificate` voor SSL-certificaten voor client) |
| PFX* |pfx |De Base64-gecodeerde inhoud van het bestand (Personal Information Exchange (PFX) |
| Wachtwoord * |wachtwoord |Het wachtwoord voor toegang tot het PFX-bestand |

> [!TIP]
> Als u wilt een parameter die niet leesbaar in de definitie na het opslaan van de logische app gebruikt, kunt u een `securestring` parameter en de `@parameters()`  
>  [definitie Werkstroomfunctie](http://aka.ms/logicappdocs).

Bijvoorbeeld:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD-OAuth-verificatie
De volgende verificatieobject is nodig voor Azure AD OAuth-verificatie. A * houdt in dat een vereist veld.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Type* |type |Het type verificatie (moet `ActiveDirectoryOAuth` voor Azure AD OAuth) |
| Tenant* |tenant |De tenant-id voor de Azure AD-tenant |
| Doelgroep * |doelgroep |De bron die u aanvraagt autorisatie moet worden gebruikt. Bijvoorbeeld: `https://management.core.windows.net/` |
| Client -ID * |clientId |De client-id voor de Azure AD-toepassing |
| Geheim * |geheim |Het geheim van de client die het token is aangevraagd |

> [!TIP]
> U kunt een `securestring` parameter en de `@parameters()` [Werkstroomfunctie definitie](http://aka.ms/logicappdocs) gebruiken een parameter die niet leesbaar in de definitie nadat ze zijn opgeslagen.
> 
> 

Bijvoorbeeld:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). U kunt de beschikbare connectors in Logic Apps verkennen door te kijken onze [API's lijst](apis-list.md).

