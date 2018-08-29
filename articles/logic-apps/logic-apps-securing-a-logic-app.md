---
title: Beveiligde toegang tot Azure Logic Apps | Microsoft Docs
description: Toegang tot triggers, invoer en uitvoer, actieparameters en services in werkstromen voor Azure Logic Apps beveiligen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.topic: article
ms.date: 11/22/2016
ms.openlocfilehash: fc4fdff5080e6ebe13850157e8d560a1d31e7719
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127476"
---
# <a name="secure-access-in-azure-logic-apps"></a>Beveiligde toegang in Azure Logic Apps

Hier zijn manieren dat u toegang tot de verschillende onderdelen in uw logische app kunt beveiligen:

* Beveiligde toegang voor het activeren van een werkstroom voor logische Apps met de HTTP-aanvraag als trigger.
* Beveiligde toegang voor het beheren, te bewerken of te lezen van een logische app.
* Beveiligde toegang tot de inhoud in de invoer en uitvoer voor een logische app uitvoeren.
* Parameters of de invoer voor de acties in een werkstroom voor logische Apps beveiligen.
* Beveiligde toegang tot services die aanvragen van een werkstroom voor logische Apps ontvangen.

## <a name="secure-access-to-trigger"></a>Beveiligde toegang tot activeren

Wanneer u werkt met een logische app die wordt geactiveerd op een HTTP-aanvraag ([aanvragen](../connectors/connectors-native-reqres.md) of [Webhook](../connectors/connectors-native-webhook.md)), kunt u de toegang beperken zodat alleen geautoriseerde clients de logische app kunnen worden gestart. Alle aanvragen naar een logische app worden versleuteld en beveiligd met SSL-beveiliging.

### <a name="shared-access-signature"></a>Shared Access Signature

Elke aanvraag-eindpunt voor een logische app bevat een [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) als onderdeel van de URL. Elke URL bevat een `sp`, `sv`, en `sig` queryparameter. Machtigingen zijn opgegeven door `sp`, en komen overeen met de HTTP-methoden die zijn toegestaan, `sv` is de versie die wordt gebruikt voor het genereren, en `sig` wordt gebruikt om te verifiëren van toegang om te activeren. De handtekening is gegenereerd op basis van het algoritme SHA256 met een geheime sleutel op de URL-paden en eigenschappen. De geheime sleutel wordt nooit beschikbaar gemaakt en gepubliceerd, en versleuteld en opgeslagen als onderdeel van de logische app wordt bewaard. Uw logische app machtigt alleen triggers met een geldige handtekening die zijn gemaakt met de geheime sleutel.

#### <a name="regenerate-access-keys"></a>Opnieuw genereren van toegangssleutels

U kunt een nieuwe beveiligde sleutel op via de REST-API of Azure-portal op elk gewenst moment opnieuw genereren. Alle huidige URL's die zijn gegenereerd eerder met behulp van de oude sleutel zijn ongeldig gemaakt en niet meer geautoriseerd voor het starten van de logische app.

1. Open in de Azure-portal, de logische app die u wilt een sleutel opnieuw genereren
1. Klik op de **toegangssleutels** menu-item onder **instellingen**
1. Kies de sleutel opnieuw genereren en het proces is voltooid

URL's die u na het opnieuw genereren zijn ondertekend met de nieuwe toegangssleutel ophalen.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Callback-URL's maken met een vervaldatum

Als u de URL met anderen deelt, kunt u URL's genereren met specifieke sleutels en vervaldatums indien nodig. U kunt vervolgens naadloos vorig sleutels of zorg ervoor dat de toegang tot het starten van een app is beperkt tot een bepaalde periode. U kunt een vervaldatum opgeven voor een URL via de [logic apps, REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Opnemen in de hoofdtekst van de eigenschap `NotAfter` als een tekenreeks met een JSON-datum waarop een callback-URL die is alleen geldig tot retourneert de `NotAfter` datum en tijd.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>Het maken van URL's met primaire of secundaire geheime sleutel

Wanneer u genereert of callback-URL voor triggers op basis van een aanvraag, kunt u ook welke sleutel voor het ondertekenen van de URL opgeven.  U kunt een URL die is ondertekend door een specifieke sleutel via genereren de [logic apps, REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) als volgt:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Opnemen in de hoofdtekst van de eigenschap `KeyType` als `Primary` of `Secondary`.  Hiermee wordt een URL die is ondertekend door de beveiligde sleutel die is opgegeven.

### <a name="restrict-incoming-ip-addresses"></a>Inkomende IP-adressen beperken

U kunt desgewenst beperken aanroepen van een logische app alleen vanuit specifieke clients naast de Shared Access Signature.  Bijvoorbeeld, als u uw eindpunt via Azure API Management beheren, kunt u de logische app de aanvraag alleen geaccepteerd wanneer de aanvraag afkomstig van het IP-adres van de API Management-exemplaar is beperken.

Deze instelling kan worden geconfigureerd in de logische app-instellingen:

1. Open in de Azure-portal, de logische app die u wilt toevoegen van IP-adresbeperkingen
1. Klik op de **Werkstroominstellingen** menu-item onder **instellingen**
1. Geef de lijst met IP-adresbereiken worden geaccepteerd door de trigger

Een geldig IP-bereik heeft de notatie `192.168.1.1/255`. Als u wilt dat de logische app alleen wordt geactiveerd als een geneste logische app, selecteert u de **alleen andere logische apps** optie. Deze optie schrijft een lege matrix naar de resource, betekenis alleen aanroepen van de service zelf (bovenliggende logische apps) is gestart.

> [!NOTE]
> U kunt nog steeds een logische app uitvoeren met een trigger voor de aanvraag via de REST-API / Management `/triggers/{triggerName}/run` , ongeacht de IP. Dit scenario is vereist voor verificatie op basis van de Azure REST API en alle gebeurtenissen wordt weergegeven in het auditlogboek van Azure. Toegang instellen toegangsbeheerbeleid dienovereenkomstig.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Instelling voor IP-adresbereiken voor de resource-uitbreiding

Als u een [implementatiesjabloon](logic-apps-create-deploy-template.md) voor het automatiseren van uw implementaties, het IP-bereik-instellingen kunnen worden geconfigureerd op de resourcesjabloon.  

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

Om toe te voegen meer autorisatieprotocollen boven op een logische app, [Azure API Management](https://azure.microsoft.com/services/api-management/) biedt uitgebreide bewaking, beveiliging, beleid en documentatie voor een willekeurig eindpunt met de mogelijkheid om een logische app als een API beschikbaar te maken. Met Azure API Management kan een openbare of particuliere eindpunt voor de logische app, die van Azure Active Directory, certificaat, OAuth of andere beveiligingsstandaarden gebruikmaken kan beschikbaar te maken. Wanneer een aanvraag wordt ontvangen, stuurt Azure API Management de aanvraag naar de logische app (voor het uitvoeren van alle benodigde transformaties en beperkingen die onderweg zijn). U kunt de inkomende IP-bereik-instellingen op de logische app toe te staan dat de logische app van API Management wordt geactiveerd.

## <a name="secure-access-to-manage-or-edit-logic-apps"></a>Veilige toegang als u wilt beheren of bewerken van logische apps

U kunt toegang beperken tot beheerbewerkingen op een logische app, zodat alleen bepaalde gebruikers of groepen kunnen bewerkingen op de resource uit te voeren. Logische apps gebruiken de Azure [Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) Solid-State en kunnen worden aangepast met dezelfde hulpprogramma's.  Er zijn een aantal ingebouwde rollen die kunt u ook leden van uw abonnement op toewijzen:

* **Logische App-bijdrager** -toegang biedt om te bekijken, bewerken en bijwerken van een logische app.  Kan de resource verwijdert of beheerder bewerkingen uitvoeren.
* **Logische App-Operator** - kan de logische app weergeven en uitvoeringsgeschiedenis en in-of uitschakelen.  Kan bewerken of bijwerken van de definitie.

U kunt ook [Azure Resourcevergrendeling](../azure-resource-manager/resource-group-lock-resources.md) om te voorkomen dat wijzigen of verwijderen van logische apps. Deze mogelijkheid is nuttig om te voorkomen dat de productieresources van wijzigingen of verwijderingen.

## <a name="secure-access-to-contents-of-the-run-history"></a>Beveiligde toegang tot de inhoud van de uitvoeringsgeschiedenis

U kunt de toegang beperken tot de inhoud van de invoer of uitvoer van voorgaande uitvoeringen aan specifieke IP-adresbereiken.  

Alle gegevens binnen een uitvoering van de werkstroom worden in-transit en in rust versleuteld. Wanneer een aanroep van uitvoeringsgeschiedenis wordt gedaan, wordt de service verifieert de aanvraag en vindt u koppelingen naar de aanvraag- en invoer en uitvoer. Deze koppeling kan worden beveiligd zodat alleen aanvragen voor het weergeven van inhoud van een aangewezen IP-adresbereik de inhoud retourneert. Voor extra toegangsbeheer kunt u deze mogelijkheid. U kunt zelfs opgeven met een IP-adres, zoals `0.0.0.0` zodat niemand toegang de invoer/uitvoer tot heeft. Alleen gebruikers met beheerdersmachtigingen kan deze beperking verwijderen met de mogelijkheid voor 'just-in-time' toegang tot de inhoud van de werkstroom.

Deze instelling kan worden geconfigureerd in de resource-instellingen van de Azure-portal:

1. Open in de Azure-portal, de logische app die u wilt toevoegen van IP-adresbeperkingen
2. Klik op de **configuratie van de toegangscontrole** menu-item onder **instellingen**
3. Geef de lijst met IP-adresbereiken voor toegang tot inhoud

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Instelling voor IP-adresbereiken voor de resource-uitbreiding

Als u een [implementatiesjabloon](logic-apps-create-deploy-template.md) voor het automatiseren van uw implementaties, het IP-bereik-instellingen kunnen worden geconfigureerd op de resourcesjabloon.  

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

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Parameters en invoer binnen een werkstroom beveiligen

Het is raadzaam om te voorzien van sommige aspecten van een definitie van de werkstroom voor implementatie in omgevingen. Sommige parameters mogelijk ook beveiligde parameters die u niet wilt dat wordt weergegeven tijdens het bewerken van een werkstroom, zoals een client-ID en clientgeheim voor [Azure Active Directory-verificatie](../connectors/connectors-native-http.md#authentication) van een HTTP-actie.

### <a name="using-parameters-and-secure-parameters"></a>Met behulp van parameters en veilige parameters

Voor toegang tot de waarde van een resourceparameter tijdens runtime, de [werkstroomdefinitietaal](http://aka.ms/logicappsdocs) biedt een `@parameters()` bewerking. U kunt ook [parameters opgeven in de sjabloon van de resource-implementatie](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Maar als u het parametertype als `securestring`, de parameter niet geretourneerd met de rest van de resourcedefinitie, en niet toegankelijk is door de resource na de implementatie weer te geven.

> [!NOTE]
> Als de parameter wordt gebruikt in de headers of de hoofdtekst van een aanvraag, de parameter mogelijk zichtbaar door het openen van de uitvoeringsgeschiedenis en uitgaande HTTP-aanvraag. Zorg ervoor dat uw beleid voor toegang tot inhoud dienovereenkomstig ingesteld.
> Autorisatie-header zijn nooit zichtbaar via in- of uitvoer. Dus als er de geheime sleutel wordt gebruikt, kan het geheim niet worden opgehaald.

#### <a name="resource-deployment-template-with-secrets"></a>Resource-implementatiesjabloon met geheimen

Het volgende voorbeeld toont een implementatie die verwijst naar een beveiligde parameter van `secret` tijdens runtime. In een afzonderlijke parameterbestand kan geeft u de omgevingswaarde voor de `secret`, of gebruik [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) om op te halen geheimen op tijd implementeren.

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
   "resources": [ {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
         "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
         "definition": {
            "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
            "actions": {
               "Call_External_API": {
                  "type": "Http",
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
   } ],
   "outputs": {}
}
```

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a>Beveiligde toegang tot services die zijn ontvangen van aanvragen van een werkstroom

Er zijn veel manieren om u te helpen beveiligen van elk willekeurig eindpunt dat de logische app moet toegang hebben tot.

### <a name="using-authentication-on-outbound-requests"></a>Met behulp van verificatie op uitgaande aanvragen

Als u werkt met een HTTP-, HTTP + Swagger (Open API) of webhookactie, kunt u verificatie toevoegen aan de aanvraag wordt verzonden. U kunt opnemen basisverificatie, verificatie via certificaat of Azure Active Directory-verificatie. Meer informatie over het configureren van deze verificatie vindt [in dit artikel](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Beperken van toegang tot de logische app IP-adressen

Alle aanroepen vanuit logic apps is afkomstig van een specifieke set IP-adressen per regio. U kunt aanvullende filters voor het accepteren van aanvragen van deze aangewezen IP-adressen alleen toevoegen. Zie voor een lijst van deze IP-adressen, [logic app limieten en configuratie](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>On-premises connectiviteit

Logische apps integratie met diverse services voor veilige en betrouwbare on-premises bieden communicatie.

#### <a name="on-premises-data-gateway"></a>On-premises gegevensgateway

Veel beheerde connectors voor logische apps bieden veilige verbinding met on-premises systemen, met inbegrip van File System, SQL, SharePoint, DB2 en meer. De gateway stuurt de gegevens van on-premises bronnen via versleutelde kanalen via de Azure Service Bus. Er is al het verkeer afkomstig als beveiligde uitgaand verkeer van de gateway-agent. Meer informatie over [de werking van de data gateway](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Met Azure API Management](https://azure.microsoft.com/services/api-management/) beschikt over on-premises connectiviteitsopties, met inbegrip van site-naar-site VPN en ExpressRoute-integratie voor beveiligde proxy en communicatie met on-premises systemen. In de Logic App Designer, kunt u snel een API beschikbaar gemaakt vanaf Azure API Management binnen een werkstroom, snelle toegang tot on-premises systemen selecteren.

## <a name="next-steps"></a>Volgende stappen
[Een implementatiesjabloon maken](logic-apps-create-deploy-template.md)  
[Afhandeling van uitzonderingen](logic-apps-exception-handling.md)  
[Uw logische apps bewaken](logic-apps-monitor-your-logic-apps.md)  
[Oplossen van problemen en fouten in logische Apps](logic-apps-diagnosing-failures.md)  
