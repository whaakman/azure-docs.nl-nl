---
title: Beveiligde toegang tot Azure Logic Apps | Microsoft Docs
description: Beveiliging voor de beveiliging van toegang tot triggers, invoer en uitvoer, actieparameters en services die worden gebruikt met werkstromen in Azure Logic Apps toevoegen.
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 45a4e476f930e0f5f6633dc5b3b35b66dc6dfa20
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="secure-access-to-your-logic-apps"></a>Veilige toegang tot uw logische apps

Er zijn veel hulpprogramma's beschikbaar om te helpen beveiligen van uw logische app.

* Toegang voor het activeren van een logische app (HTTP-aanvragen Trigger) beveiligen
* Beveiligen van de toegang wilt beheren, bewerken of een logische app lezen
* Toegang tot inhoud van de invoer en uitvoer voor een run beveiligen
* Parameters of invoer binnen acties in een werkstroom beveiligen
* Toegang tot services die aanvragen van een werkstroom ontvangen beveiligen

## <a name="secure-access-to-trigger"></a>Veilige toegang tot activeren

Wanneer u werkt met een logische app, die wordt geactiveerd op een HTTP-aanvraag ([aanvragen](../connectors/connectors-native-reqres.md) of [Webhook](../connectors/connectors-native-webhook.md)), kunt u de toegang beperken zodat alleen geautoriseerde clients kunnen de logische app worden gestart. Alle aanvragen in een logische app zijn versleuteld en beveiligd via SSL.

### <a name="shared-access-signature"></a>Shared Access Signature

Elk eindpunt van de aanvraag voor een logische app bevat een [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) als onderdeel van de URL. Elke URL bevat een `sp`, `sv`, en `sig` queryparameter. Machtigingen zijn opgegeven door `sp`, en komen overeen met de HTTP-methoden die zijn toegestaan, `sv` is de versie die wordt gebruikt voor het genereren, en `sig` wordt gebruikt om te verifiëren van toegang om te activeren. De handtekening wordt gegenereerd op basis van het algoritme SHA256 met een geheime sleutel op de URL-paden en eigenschappen. De geheime sleutel wordt nooit beschikbaar gemaakt en gepubliceerd, en versleuteld en opgeslagen als onderdeel van de logische app wordt opgeslagen. Uw logische app machtigt alleen triggers die een geldige handtekening gemaakt met de geheime sleutel bevatten.

#### <a name="regenerate-access-keys"></a>Toegangstoetsen genereren

U kunt een nieuwe beveiligde sleutel op via de REST-API of Azure-portal op elk gewenst moment opnieuw genereren. Alle huidige URL's die eerder met de oude sleutel zijn gegenereerd zijn ongeldig gemaakt en niet langer gemachtigd de logische app activeert.

1. Open de logische app die u wilt genereren van een sleutel in de Azure-portal
1. Klik op de **toegangssleutels** menu-item onder **instellingen**
1. Kies de sleutel te genereren en het proces te voltooien

URL's die u na het opnieuw genereren ophalen zijn ondertekend met de nieuwe toegangssleutel.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Callback URL's maken met een vervaldatum

Als u de URL met andere partijen deelt, kunt u URL's genereren met de specifieke sleutels en vervaldatums indien nodig. U kunt vervolgens probleemloos sleutels rouleert of zorg ervoor dat de toegang tot het starten van een app is beperkt tot een bepaalde TimeSpan-waarde. U kunt een vervaldatum opgeven voor een URL die u via de [logische apps REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Opnemen in de hoofdtekst van de eigenschap `NotAfter` als een JSON-datumtekenreeks, die een callback-URL die is alleen geldig tot retourneert de `NotAfter` datum en tijd.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>URL's maken met primaire of secundaire geheime sleutel

Wanneer u genereren of URL van de callback voor triggers op aanvraag gebaseerde, kunt u ook welke sleutel moet worden gebruikt voor het ondertekenen van de URL opgeven.  U kunt een URL die is ondertekend door een specifieke sleutel via genereren de [logische apps REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) als volgt:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Opnemen in de hoofdtekst van de eigenschap `KeyType` als `Primary` of `Secondary`.  Hiermee wordt een URL die is ondertekend door de opgegeven beveiligde sleutel.

### <a name="restrict-incoming-ip-addresses"></a>Binnenkomende IP-adressen beperken

U kunt desgewenst het aanroepen van een logische app alleen van specifieke clients beperken naast de Shared Access Signature.  Als u uw eindpunt via Azure API Management beheren, kunt u bijvoorbeeld de logische app alleen de aanvraag worden geaccepteerd wanneer de aanvraag afkomstig van het IP-adres van API Management-exemplaar is beperken.

Deze instelling kan worden geconfigureerd in de logic app-instellingen:

1. Open in de Azure-portal, de logische app die u wilt toevoegen, IP-adresbeperkingen
1. Klik op de **configuratie voor toegangsbeheer** menu-item onder **instellingen**
1. Geef de lijst met IP-adresbereiken worden geaccepteerd door de trigger

Een geldig IP-bereik heeft de notatie `192.168.1.1/255`. Als u wilt dat de logische app alleen gestart als een geneste logische app, selecteert u de **alleen andere logic apps** optie. Deze optie schrijft een lege matrix naar de resource betekenis alleen aanroepen van de service zelf (bovenliggende logic apps) is gestart.

> [!NOTE]
> U kunt nog steeds een logische app uitvoeren met een trigger aanvraag via de REST-API / Management `/triggers/{triggerName}/run` ongeacht IP. Dit scenario vereist verificatie met de REST-API van Azure en alle gebeurtenissen wordt weergegeven in het controlelogboek Azure. Toegang instellen toegangsbeheerbeleid dienovereenkomstig.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Het instellen van IP-adresbereiken op de resource-uitbreiding

Als u een [implementatiesjabloon](logic-apps-create-deploy-template.md) voor het automatiseren van uw implementaties van het IP-bereik-instellingen kunnen worden geconfigureerd op de resource-sjabloon.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Azure Active Directory, OAuth of andere beveiliging toevoegen

Toevoegen van meer autorisatie-protocollen boven op een logische app, [Azure API Management](https://azure.microsoft.com/services/api-management/) biedt uitgebreide controle, beveiliging, beleid en documentatie voor een willekeurig eindpunt met de mogelijkheid om een logische app als een API weer te geven. Azure API Management kan een openbare of particuliere eindpunt voor de logische app, die gebruik Azure Active Directory, certificaat, OAuth of andere beveiligingsstandaarden maken kan worden blootgesteld. Wanneer een aanvraag wordt ontvangen, stuurt Azure API Management in de aanvraag aan de logische app (voor het uitvoeren van alle benodigde transformaties en beperkingen onderweg). U kunt de instellingen voor binnenkomende IP-bereik op de logische app toe te staan dat de logische app van API Management worden geactiveerd.

## <a name="secure-access-to-manage-or-edit-logic-apps"></a>Veilige toegang als u wilt beheren of logische apps bewerken

U kunt toegang tot beheertaken uit te voeren op een logische app beperken zodat alleen bepaalde gebruikers of groepen kunnen geen bewerkingen uitvoeren op de resource. Logische apps gebruiken de Azure [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md) functie en met dezelfde hulpmiddelen kunnen worden aangepast.  Er zijn enkele ingebouwde rollen die kunt u ook leden van uw abonnement toewijzen:

* **Logic App Inzender** -toegang als u wilt weergeven, bewerken en bijwerken van een logische app biedt.  Kan de resource verwijderen of admin bewerkingen uitvoeren.
* **Logic App Operator** - kunt bekijken van de logische app geschiedenis uitvoeren en in-of uitschakelen.  Niet kunt bewerken of bijwerken van de definitie.

U kunt ook [Azure Resource vergrendeling](../azure-resource-manager/resource-group-lock-resources.md) om te voorkomen dat wijzigen of verwijderen van logische apps. Deze mogelijkheid is nuttig om te voorkomen dat productiebronnen niet gewijzigd of verwijderd.

## <a name="secure-access-to-contents-of-the-run-history"></a>Veilige toegang tot inhoud van de geschiedenis uitvoeren

U kunt toegang tot inhoud van het in- of uitgangen beperken van eerder uitgevoerde aan specifieke IP-adresbereiken.  

Alle gegevens binnen een werkstroom uitgevoerd is versleuteld in rust en onderweg. Wanneer een oproep naar de geschiedenis uitvoeren wordt gedaan, wordt de service wordt geverifieerd en vindt u koppelingen naar de aanvraag en antwoord-invoer en uitvoer. Deze koppeling kan worden beveiligd, zodat de inhoud alleen aanvragen voor het openen van de inhoud van een aangewezen IP-adresbereik retourneren. Voor extra toegangsbeheer kunt u deze mogelijkheid. U kunt zelfs opgeven met een IP-adres zoals `0.0.0.0` zodat niemand toegang de invoer/uitvoer tot heeft. Alleen gebruikers met beheerdersmachtigingen kan deze beperking verwijderen met de mogelijkheid voor 'just in time' toegang tot inhoud van de werkstroom.

Deze instelling kan worden geconfigureerd in de resource-instellingen van de Azure-portal:

1. Open in de Azure-portal, de logische app die u wilt toevoegen, IP-adresbeperkingen
1. Klik op de **configuratie voor toegangsbeheer** menu-item onder **instellingen**
1. Geef de lijst met IP-adresbereiken voor toegang tot inhoud

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Het instellen van IP-adresbereiken op de resource-uitbreiding

Als u een [implementatiesjabloon](logic-apps-create-deploy-template.md) voor het automatiseren van uw implementaties van het IP-bereik-instellingen kunnen worden geconfigureerd op de resource-sjabloon.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Beveiligde parameters en invoer binnen een werkstroom

Het is raadzaam om te voorzien van sommige aspecten van de werkstroomdefinitie van een voor implementatie in omgevingen. Sommige parameters mogelijk ook beveiligde parameters die u niet weergeven wilt bij het bewerken van een werkstroom, zoals een client-ID en clientgeheim voor [Azure Active Directory-verificatie](../connectors/connectors-native-http.md#authentication) van een HTTP-actie.

### <a name="using-parameters-and-secure-parameters"></a>Parameters en beveiligde parameters gebruiken

Voor toegang tot de waarde van een resourceparameter tijdens runtime, de [werkstroom definition language](http://aka.ms/logicappsdocs) biedt een `@parameters()` bewerking. U kunt ook [parameters opgeven in de resource-implementatiesjabloon](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Maar als u het parametertype als opgeven `securestring`, de parameter niet geretourneerd met de rest van de resourcedefinitie, en niet toegankelijk door de resource na de implementatie.

> [!NOTE]
> Als de parameter wordt gebruikt in de headers of de hoofdtekst van een aanvraag, de parameter mogelijk zichtbaar via de uitvoeringsgeschiedenis en uitgaande HTTP-aanvraag. Zorg ervoor dat uw beleidsregels voor toegang tot inhoud dienovereenkomstig instellen.
> Autorisatie-header zijn nooit zichtbaar via in- of uitgangen. Als het geheim er wordt gebruikt, is het geheim dus niet worden opgehaald.

#### <a name="resource-deployment-template-with-secrets"></a>Resource-implementatiesjabloon met geheimen

Het volgende voorbeeld ziet u een implementatie die verwijst naar een veilige parameter van het `secret` tijdens runtime. In een afzonderlijke parameterbestand kan u de waarde van de omgeving voor de `secret`, of gebruik [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) voor het ophalen van geheimen op tijd implementeren.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is the request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a>Veilige toegang tot services ontvangen van aanvragen van een werkstroom

Er zijn veel manieren te beveiligen van een willekeurig eindpunt dat de logische app toegang moet hebben.

### <a name="using-authentication-on-outbound-requests"></a>Met behulp van verificatie voor uitgaande aanvragen

Als u werkt met een HTTP-, HTTP + Swagger (Open API) of Webhook actie, kunt u verificatie toevoegen aan de aanvraag wordt verzonden. U kunt opnemen basisverificatie, verificatie via certificaat of Azure Active Directory-verificatie. Informatie over het configureren van deze verificatie vindt [in dit artikel](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Beperken van toegang tot logic app IP-adressen

Alle aanroepen vanuit logic apps is afkomstig van een specifieke set IP-adressen per regio. U kunt aanvullende filteren om te accepteren alleen aanvragen van deze aangewezen IP-adressen toevoegen. Zie voor een lijst van deze IP-adressen, [logic app en -configuratie](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>On-premises connectiviteit

Logische apps bieden integratie met verschillende services te bieden veilige en betrouwbare lokale communicatie.

#### <a name="on-premises-data-gateway"></a>On-premises gegevensgateway

Veel beheerde connectors voor logic apps bieden veilige verbinding met on-premises systemen, met inbegrip van bestandssysteem, SQL, SharePoint, DB2 en meer. De gateway stuurt gegevens van lokale bronnen op gecodeerde kanalen via de Azure Service Bus. Al het verkeer afkomstig is als beveiligde uitgaand verkeer van de gateway-agent. Meer informatie over [de werking van de gegevensgateway](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) heeft lokale connectiviteitsopties, met inbegrip van de site-naar-site-VPN en ExpressRoute-integratie voor beveiligde proxy en communicatie met on-premises systemen. In de ontwerpfunctie voor Logic App kunt u snel een API van Azure API Management binnen een werkstroom blootgesteld snel toegang tot on-premises systemen selecteren.

## <a name="next-steps"></a>Volgende stappen
[Maken van een sjabloon voor de implementatie](logic-apps-create-deploy-template.md)  
[Afhandeling van uitzonderingen](logic-apps-exception-handling.md)  
[Uw logische apps bewaken](logic-apps-monitor-your-logic-apps.md)  
[Logic app fouten en problemen oplossen](logic-apps-diagnosing-failures.md)  
