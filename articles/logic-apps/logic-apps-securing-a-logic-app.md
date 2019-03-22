---
title: Beveiligde toegang tot Azure Logic Apps | Microsoft Docs
description: Beveiliging toevoegen voor Azure Logic Apps, met inbegrip van triggers, invoer en uitvoer, parameters en andere services
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.topic: article
ms.date: 02/05/2019
ms.openlocfilehash: 34076b790a91b1c0e9d8bee224423aab0db7c8f3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57891804"
---
# <a name="secure-access-in-azure-logic-apps"></a>Beveiligde toegang in Azure Logic Apps

Hier volgen de elementen in uw logische app waar u toegang kunt beveiligen:

* [Aanvraag of Webhook-triggers](#secure-triggers)
* [Bewerkingen zoals het beheren, bewerken of weergeven van](#secure-operations) uw logische app
* [Invoer en uitvoer](#secure-run-history) geschiedenis van uw logische app uitvoeren
* [Actieparameters en invoer](#secure-action-parameters)
* [Services die aanvragen](#secure-requests) van uw logische app

<a name="secure-triggers"></a>

## <a name="secure-access-to-request-triggers"></a>Beveiligde toegang tot aanvragen van triggers

Wanneer uw logische app maakt gebruik van een trigger met een HTTP-aanvraag, zoals de [aanvraag](../connectors/connectors-native-reqres.md) of [Webhook](../connectors/connectors-native-webhook.md) trigger, u kunt de toegang beperken zodat alleen geautoriseerde clients met uw logische app beginnen kunnen. Alle aanvragen zijn ontvangen door een logische app worden versleuteld en beveiligd met Secure Sockets Layer (SSL)-protocol. Hier zijn verschillende manieren kunt u toegang tot dit triggertype beveiligen:

* [Handtekeningen voor gedeelde toegang genereren](#sas)
* [Inkomende IP-adressen beperken](#restrict-incoming-IP)
* [Azure Active Directory, OAuth of andere beveiliging toevoegen](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures"></a>Handtekeningen voor gedeelde toegang genereren

Elke aanvraag voor het eindpunt op een logische app bevat een [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) in van de eindpunt-URL. Elke URL bevat een `sp`, `sv`, en `sig` queryparameter:

* `sp` Hiermee geeft u machtigingen die zijn toegewezen aan de HTTP-methoden toegestaan voor gebruik.
* `sv` Hiermee geeft u de versie die wordt gebruikt voor het genereren van de handtekening.
* `sig` wordt gebruikt voor het verifiëren van toegang tot de trigger.

De handtekening is gegenereerd op basis van het algoritme SHA256 met een geheime toegangssleutel op de URL-paden en eigenschappen. De geheime sleutel wordt nooit beschikbaar worden gemaakt of gepubliceerd, en versleuteld en opgeslagen met de logische app wordt bewaard. Uw logische app machtigt alleen deze triggers die geen geldige handtekening die zijn gemaakt met de geheime sleutel bevatten. 

Hier volgt meer informatie over het beveiligen van toegang met Shared Access Signature:

* [Opnieuw genereren van toegangssleutels](#access-keys)
* [Verlopende callback-URL's maken](#expiring-URLs)
* [URL's maken met de primaire of secundaire sleutel](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Toegangssleutels regenereren

Als u wilt genereren op elk gewenst moment een nieuwe sleutel voor beveiligde toegang, gebruik de Azure REST API of Azure-portal. Alle URL's die de oude sleutel ongeldig worden gemaakt en zijn niet langer gemachtigd voor het activeren van de logische app eerder gegenereerd. De URL's die u na het opnieuw genereren zijn ondertekend met de nieuwe toegangssleutel ophalen.

1. Open de logische app die beschikt over de sleutel die u wilt genereren in de Azure-portal.

1. In het menu van de logische app onder **instellingen**, selecteer **toegangssleutels**.

1. Selecteer de sleutel die u wilt opnieuw genereren en het proces te voltooien.

<a name="expiring-urls"></a>

#### <a name="create-callback-urls-with-expiration-dates"></a>Callback-URL's maken met vervaldatums

Als u een trigger op basis van een aanvraag-eindpunt-URL met anderen deelt, kunt u de callback-URL's met specifieke sleutels en vervaldatums zo nodig kunt genereren. U kunt vervolgens naadloos vorig sleutels of beperken van toegang voor het activeren van de logische app een bepaalde periode. U kunt een vervaldatum opgeven voor een URL met behulp van de [REST-API voor Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers), bijvoorbeeld:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Opnemen in de hoofdtekst van de `NotAfter`eigenschap met een JSON-datum tekenreeks. Deze eigenschap retourneert een callback-URL die is geldig tot en met de `NotAfter` datum en tijd.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>URL's maken met de primaire of secundaire geheime sleutel

Wanneer u genereren of lijst callback-URL's voor triggers op basis van een aanvraag, u kunt ook de sleutel te gebruiken voor het ondertekenen van de URL opgeven. U kunt een URL genereren die ondertekend door een specifieke sleutel met behulp van de [REST-API voor Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers), bijvoorbeeld:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Opnemen in de hoofdtekst van de `KeyType` eigenschap als `Primary` of `Secondary`. Deze eigenschap retourneert een URL die ondertekend door de opgegeven beveiligde sleutel.

<a name="restrict-incoming-ip"></a>

### <a name="restrict-incoming-ip-addresses"></a>Inkomende IP-adressen beperken

Samen met Shared Access Signature, is het raadzaam om te beperken van specifieke clients die uw logische app kunnen aanroepen.  
Als u uw eindpunt aanvraag met Azure API Management beheren, kunt u bijvoorbeeld uw logische app voor het accepteren van aanvragen van de IP-adres van de API Management-instantie beperken. 

#### <a name="set-ip-ranges---azure-portal"></a>IP-adresbereiken instellen - Azure portal

Als u deze beperking in de Azure-portal instelt, gaat u naar uw logische app-instellingen: 

1. Open uw logische app in de ontwerper van logische App in de Azure-portal. 

1. In het menu van uw logische app onder **instellingen**, selecteer **Werkstroominstellingen**.

1. Onder **configuratie van de toegangscontrole** > 
**toegestane inkomende IP-adressen**, selecteer **specifieke IP-adresbereiken**.

1. Onder **IP-bereiken voor triggers**, geef het IP-adresbereiken die de trigger accepteert. Een geldig IP-bereik maakt gebruik van deze indelingen: *x.x.x.x/x* of *x.x.x.x-x.x.x.x* 

Als u wilt dat uw logische app moet worden alleen als een geneste logische app wordt gestart vanuit de **toegestane inkomende IP-adressen** in de lijst met **alleen andere logische Apps**. Deze optie schrijft een lege matrix aan uw logische app-resource, zodat alleen aanroepen van de Logic Apps-service (bovenliggende logische apps) de geneste logische app kunnen activeren.

> [!NOTE]
> Ongeacht de IP-adres, kunt u nog steeds een logische app met een trigger op basis van een aanvraag met behulp van uitvoeren `/triggers/{triggerName}/run` via de Azure REST API of via API Management. Echter nog steeds vereist verificatie bij de Azure REST API voor dit scenario en alle gebeurtenissen worden weergegeven in het auditlogboek van Azure. Zorg ervoor dat u beleid voor toegangsbeheer dienovereenkomstig ingesteld.

#### <a name="set-ip-ranges---logic-app-deployment-template"></a>IP-adresbereiken instellen - sjabloon voor logische app-implementatie

Als u een logische app-implementaties automatiseren met behulp van bent een [Azure Resource Manager-implementatiesjabloon](../logic-apps/logic-apps-create-deploy-template.md), u kunt de IP-adresbereiken instellen in de sjabloon, bijvoorbeeld:

``` json
{
   "properties": {
      "definition": {},
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
   "type": "Microsoft.Logic/workflows",
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Azure Active Directory, OAuth of andere beveiliging toevoegen

Overweeg het gebruik van meer autorisatieprotocollen toevoegen aan uw logische app, [Azure API Management](https://azure.microsoft.com/services/api-management/). Deze service biedt uitgebreide bewaking, beveiliging, beleid en documentatie voor een willekeurig eindpunt en biedt u de mogelijkheid om uw logische app als een API beschikbaar te maken. API Management kan een openbare of particuliere eindpunt voor uw logische app, die vervolgens Azure Active Directory, OAuth, certificaat of andere beveiligingsstandaarden gebruiken kunt beschikbaar te maken. Als API Management een aanvraag ontvangt, verzendt de aanvraag door de service aan uw logische app, zodat ook alle benodigde transformaties en beperkingen weg. Zodat alleen API Management aan uw logische app activeren kunt u instellingen voor uw logische app binnenkomende IP-bereik. 

<a name="secure-operations"></a>

## <a name="secure-access-to-logic-app-operations"></a>Beveiligde toegang tot de logische appbewerkingen

U kunt toegang op taken zoals het beheren, bewerken en weergeven van beperken zodat alleen bepaalde gebruikers of groepen die bewerkingen op uw logische app uitvoeren. Biedt ondersteuning voor Logic Apps [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/role-assignments-portal.md), die u kunt aanpassen of ingebouwde rollen toewijzen aan leden in uw abonnement, bijvoorbeeld:

* **Logische App-bijdrager**: Gebruikers kunnen weergeven, bewerken en bijwerken van uw logische app. Deze rol kan de logische app verwijderen of beheerder bewerkingen uitvoeren.
* **Logische App-Operator**: Gebruikers kunnen uw logische app en de uitvoeringsgeschiedenis bekijken en inschakelen of uw logische app uitschakelen. Deze rol kan bewerken of bijwerken van uw logische app.

Om te voorkomen dat anderen wijzigen of verwijderen van uw logische app, kunt u [Azure Resourcevergrendeling](../azure-resource-manager/resource-group-lock-resources.md). Deze mogelijkheid kunt u voorkomen dat anderen wijzigen of verwijderen van productieresources.

<a name="secure-run-history"></a>

## <a name="secure-access-to-logic-app-run-history"></a>Beveiligde toegang tot de logische app uitvoeringsgeschiedenis

Ter bescherming van inhoud die wordt doorgegeven als invoer of uitvoer van eerdere uitvoeringen van logische app, kunt u toegang beperken tot specifieke IP-adresbereiken. Deze mogelijkheid biedt u meer toegangscontrole. Alle gegevens in een logische app uitvoeren worden versleuteld tijdens de overdracht en in rust. Wanneer u de uitvoeringsgeschiedenis van een logische app aanvraagt, Logic Apps worden geverifieerd dat verzoek en vindt u koppelingen naar de invoer en uitvoer van de aanvragen en antwoorden in de werkstroom van uw logische app. U kunt deze koppelingen kunt beveiligen, zodat alleen aanvragen van een specifiek IP-adres terug die inhoud. Bijvoorbeeld, kunt u ook opgeven een IP-adres zoals `0.0.0.0-0.0.0.0` waardoor niemand toegang heeft tot de invoer en uitvoer. Alleen een persoon met administrator-machtigingen kunt deze beperking verwijderen met de mogelijkheid voor 'just-in-time' toegang tot de inhoud van uw logische app.

### <a name="set-ip-ranges---azure-portal"></a>IP-adresbereiken instellen - Azure portal

Als u deze beperking in de Azure-portal instelt, gaat u naar uw logische app-instellingen:

1. Open uw logische app in de ontwerper van logische App in de Azure-portal. 

1. In het menu van uw logische app onder **instellingen**, selecteer **Werkstroominstellingen**.

1. Onder **configuratie van de toegangscontrole** > 
    **toegestane inkomende IP-adressen**, selecteer **specifieke IP-adresbereiken**.

1. Onder **IP-bereiken voor inhoud**, geef de IP-adresbereiken die toegang hebben tot inhoud van de invoer en uitvoer. 
   Een geldig IP-bereik maakt gebruik van deze indelingen: *x.x.x.x/x* of *x.x.x.x-x.x.x.x* 

### <a name="set-ip-ranges---logic-app-deployment-template"></a>IP-adresbereiken instellen - sjabloon voor logische app-implementatie

Als u een logische app-implementaties automatiseren met behulp van bent een [Azure Resource Manager-implementatiesjabloon](../logic-apps/logic-apps-create-deploy-template.md), u kunt de IP-adresbereiken instellen in de sjabloon, bijvoorbeeld:

``` json
{
   "properties": {
      "definition": {},
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
   "type": "Microsoft.Logic/workflows",
}
```

<a name="secure-action-parameters"></a>

## <a name="secure-action-parameters-and-inputs"></a>Actieparameters en invoer beveiligen

Wanneer u deze implementeert in verschillende omgevingen, is het raadzaam om te voorzien van bepaalde elementen in het definitie van de werkstroom van uw logische app. Op die manier kunt u informatie invoeren op basis van de omgevingen die u gebruikt en beveiligen van vertrouwelijke gegevens. Bijvoorbeeld, als u bij het verifiëren van HTTP-acties met [Azure Active Directory](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication), definiëren en beveiligt u de parameters die de client-ID en clientgeheim dat wordt gebruikt voor verificatie accepteren. Voor deze parameters, de definitie van uw logische app heeft een eigen `parameters` sectie.
Voor toegang tot parameterwaarden tijdens runtime, kunt u de `@parameters('parameterName')` expressie die wordt geleverd door de [Werkstroomdefinitietaal](https://aka.ms/logicappsdocs). 

Ter bescherming van parameters en waarden die u niet dat wordt weergegeven wilt tijdens het bewerken van uw logische app of geschiedenis weergeven die worden uitgevoerd, kunt u parameters met de `securestring` typt en indien nodig-codering gebruiken. Parameters waarvoor dit type worden niet geretourneerd met de resourcedefinitie, en niet toegankelijk zijn bij het weergeven van de resource na de implementatie.

> [!NOTE]
> Als u een parameter in de headers of de hoofdtekst van een aanvraag gebruikt, kan deze parameter zichtbaar zijn bij het openen van de uitvoeringsgeschiedenis van uw logische app en uitgaande HTTP-aanvraag. Zorg ervoor dat u uw beleid voor toegang tot de inhoud ook dienovereenkomstig instellen.
> Autorisatie-header zijn nooit zichtbaar via in- of uitvoer. Dus als een geheim er gebruikt wordt, is dit geheim niet worden opgehaald.

Zie voor meer informatie over het beveiligen van parameters in logic app-definities [Secure parameters in logic app-definities](#secure-parameters-workflow) hoger op deze pagina.

Als u bij het automatiseren van implementaties met [sjablonen van Azure Resource Manager-implementatie](../azure-resource-manager/resource-group-authoring-templates.md#parameters), u kunt ook beveiligde parameters in deze sjablonen gebruiken. Bijvoorbeeld, kunt u parameters voor het ophalen van de KeyVault-geheimen bij het maken van uw logische app. De definitie van de implementatie sjabloon heeft een eigen `parameters` sectie, los van uw logische app `parameters` sectie. Zie voor meer informatie over het beveiligen van parameters in implementatiesjablonen [Secure parameters in implementatiesjablonen](#secure-parameters-deployment-template) hoger op deze pagina.

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-logic-app-definitions"></a>Beveiligen van parameters in logic app-definities

Ter bescherming van gevoelige gegevens in uw definitie van de werkstroom van de logische app, beveiligde parameters te gebruiken, zodat deze gegevens niet worden weergegeven nadat u uw logische app hebt opgeslagen. Stel bijvoorbeeld dat u `Basic` verificatie in de definitie van een HTTP-actie. Dit voorbeeld bevat een `parameters` sectie waarmee de parameters voor de definitie van de actie gedefinieerd plus een `authentication` sectie dat accepteert `username` en `password` parameterwaarden. Om de waarden voor deze parameters opgeeft, kunt u een afzonderlijke parameterbestand, bijvoorbeeld:

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('usernameParam')",
               "password": "@parameters('passwordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "passwordParam": {
         "type": "securestring"
      },
      "userNameParam": {
         "type": "securestring"
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
}
```

Als u geheimen gebruikt, kunt u deze geheimen downloaden tijdens de implementatie met behulp van [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-deployment-templates"></a>Parameters in de sjablonen van Azure Resource Manager-implementatie beveiligen

In dit voorbeeld ziet u een sjabloon voor de implementatie van Resource Manager die gebruikmaakt van meer dan één Runtimeparameter met de `securestring` type:

* `armTemplatePasswordParam`, die wordt ingevoerd voor de logische app-definitie `logicAppWfParam` parameter

* `logicAppWfParam`, die wordt ingevoerd voor de HTTP-actie basisverificatie wordt gebruikt

Dit voorbeeld bevat een binnenste `parameters` sectie, die tot de definitie van de werkstroom van uw logische app en een buitenste behoort `parameters` sectie, die deel uitmaakt van uw sjabloon voor de implementatie. De Omgevingswaarden voor parameters wilt opgeven, kunt u een afzonderlijke parameterbestand. 

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "logicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "armTemplatePasswordParam": {
         "type": "securestring"
      },
      "logicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('logicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('logicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('usernameParam')",
                           "password": "@parameters('logicAppWfParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "logicAppWfParam": {
                     "type": "securestring"
                  },
                  "userNameParam": {
                     "type": "securestring"
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
               "logicAppWfParam": {
                  "value": "[parameters('armTemplatePasswordParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

Als u geheimen gebruikt, kunt u deze geheimen downloaden tijdens de implementatie met behulp van [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

<a name="secure-requests"></a>

## <a name="secure-access-to-services-receiving-requests"></a>Beveiligde toegang tot services die zijn ontvangen van aanvragen

Hier volgen enkele manieren waarop u een willekeurig eindpunt waar uw logische app toegang nodig heeft en verzendt aanvragen kunt beveiligen.

### <a name="add-authentication-on-outbound-requests"></a>Verificatie toevoegen aan uitgaande aanvragen

Als u werkt met een HTTP-, HTTP + Swagger (Open API) of webhookactie, kunt u verificatie toevoegen aan de aanvraag is verzonden door uw logische app. U kunt bijvoorbeeld basisverificatie, verificatie via certificaat of Azure Active Directory-verificatie gebruiken. Zie voor meer informatie, [verifiëren triggers of acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

### <a name="restrict-access-to-logic-app-ip-addresses"></a>Toegang beperken tot IP-adressen voor logische app

Alle aanroepen vanuit logic apps is afkomstig van specifieke aangewezen IP-adressen op basis van regio. U kunt toevoegen die aanvragen van deze IP-adressen accepteert filteren. Zie voor deze IP-adressen, [limieten en configuratie voor Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

### <a name="secure-on-premises-connectivity"></a>Beveiligen van on-premises connectiviteit

Met Azure Logic Apps biedt integratie met deze services voor veilige en betrouwbare on-premises communicatie.

#### <a name="on-premises-data-gateway"></a>On-premises gegevensgateway

Veel beheerde connectors voor Azure Logic Apps bieden veilige verbindingen met on-premises systemen, zoals File System, SQL, SharePoint, DB2 en anderen. De gateway stuurt de gegevens van on-premises bronnen via versleutelde kanalen via de Azure Service Bus. Er is al het verkeer afkomstig als beveiligde uitgaand verkeer van de gateway-agent. Informatie over [de werking van de on-premises gegevensgateway](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Met Azure API Management](https://azure.microsoft.com/services/api-management/) biedt opties voor on-premises verbinding, zoals site-naar-site virtueel particulier netwerk en ExpressRoute-integratie voor beveiligde proxy en communicatie met on-premises systemen. In de Logic App Designer, kunt u een API beschikbaar worden gemaakt door API Management vanaf uw logische app-werkstroom, snelle toegang tot on-premises systemen.

## <a name="next-steps"></a>Volgende stappen

* [Een implementatiesjabloon maken](logic-apps-create-deploy-template.md)  
* [Afhandeling van uitzonderingen](logic-apps-exception-handling.md)  
* [Uw logische apps bewaken](logic-apps-monitor-your-logic-apps.md)  
* [Fouten in logische Apps en problemen diagnosticeren](logic-apps-diagnosing-failures.md)  
