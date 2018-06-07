---
title: Ophalen van Nalevingsgegevens op Azure-beleid
description: Bepaalt de naleving Azure evaluaties van beleid en de gevolgen. Informatie over het ophalen van de nalevingsdetails.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d36ecb18811901fb781e151c06badc0697c2d769
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654925"
---
# <a name="getting-compliance-data"></a>Ophalen van Nalevingsgegevens

Een van de grootste voordelen van Azure-beleid is het inzicht en besturingselementen over resources in een abonnement biedt of [beheergroep](../azure-resource-manager/management-groups-overview.md) van abonnementen. Dit besturingselement kan worden uitgeoefend op veel verschillende manieren, zoals voorkomen dat bronnen worden gemaakt in de verkeerde locatie afdwingen van het gebruik van de gemeenschappelijke en consistente tag of controle bestaande resources voor configuraties en instellingen van toepassing. In alle gevallen wordt gegevens gegenereerd door beleid waarmee u inzicht in de compatibiliteitsstatus van uw omgeving.

Er zijn verschillende manieren toegang krijgen tot de informatie over de naleving door het beleid en door initiatief toewijzingen gegenereerd:

- Met behulp van de [Azure-Portal](#portal)
- Via [opdrachtregel](#command_line) scripting

Voordat u de methoden voor het rapporteren van naleving, bekijken we wanneer informatie over de compatibiliteit is bijgewerkt en de frequentie en gebeurtenissen die een evaluatiecyclus activeren.

## <a name="evaluation-triggers"></a>Evaluatie-triggers

De resultaten van een voltooide evaluatiecyclus worden doorgevoerd in de `Microsoft.PolicyInsights` Resource Provider via `PolicyStates` en `PolicyEvents` bewerkingen. Zie voor meer informatie over de opties en mogelijkheden van de REST-API van beleid Insights [beleid Insights](/rest/api/policy-insights/).

Evaluatie van toegewezen beleid en de initiatieven gebeuren als gevolg van de diverse gebeurtenissen:

- Een beleid of het initiatief is zojuist toegewezen aan een bereik. Wanneer dit gebeurt, duurt het ongeveer 30 minuten voor de toewijzing moet worden toegepast op het gedefinieerde bereik. Zodra deze is geïnstalleerd, de beoordelingscyclus begint voor resources binnen dat bereik tegen de zojuist toegewezen beleid of het initiatief en afhankelijk van de effecten die wordt gebruikt door het beleid of initiatief, resources zijn gemarkeerd als compatibel of niet-compatibel. Een grote beleid of het initiatief geëvalueerd op basis van een grote bereik van bronnen kan duren, zodat er geen vooraf gedefinieerde verwachting van wanneer de evaluatiecyclus voor de wordt voltooid. Nadat deze is voltooid, is bijgewerkt nalevingsresultaten zijn beschikbaar in de portal en SDK's.
- Een beleid of het initiatief is al toegewezen aan een scope wordt bijgewerkt. De evaluatiefase computerbeleid en het tijdstip voor dit scenario is hetzelfde als voor een nieuwe toewijzing aan een bereik.
- Een resource wordt geïmplementeerd op een scope met een toewijzing via Resource Manager, REST, Azure CLI of Azure PowerShell. In dit scenario wordt de gebeurtenis effect (toevoegen, controleren, weigeren, implementeren) en de compatibele status-informatie is beschikbaar in de portal en SDK's ongeveer 15 minuten later.
- Standaard naleving evaluatiecyclus voor installatie. Elke 24 uur, toewijzingen worden automatisch opnieuw worden geëvalueerd. Een grote beleid of het initiatief geëvalueerd op basis van een grote bereik van bronnen kan duren, zodat er geen vooraf gedefinieerde verwachting van wanneer de evaluatiecyclus voor de wordt voltooid. Nadat deze is voltooid, is bijgewerkt nalevingsresultaten zijn beschikbaar in de portal en SDK's.

## <a name="how-compliance-works"></a>De werking van naleving

Een bron is niet-compatibel als deze niet aan beleid of initiatief regels in een toewijzing. De volgende tabel toont hoe verschillende beleid bij de evaluatie van de voorwaarde voor de resulterende compatibiliteitsstatus:

| Resource-status | Effect | Evaluatie van het beleid | Compatibiliteitsstatus |
| --- | --- | --- | --- |
| Bestaat | Weigeren, Controleren, Toevoegen\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Niet-compatibel |
| Bestaat | Weigeren, Controleren, Toevoegen\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Compatibel |
| Nieuw | Controleren, AuditIfNotExist\* | True | Niet-compatibel |
| Nieuw | Controleren, AuditIfNotExist\* | False | Compatibel |

\*Voor de acties Toevoegen, DeployIfNotExist en AuditIfNotExist moet de IF-instructie TRUE zijn.
De acties vereisen ook dat de bestaansvoorwaarde FALSE is om niet-compatibel te zijn. Indien TRUE, activeert de IF-voorwaarde de evaluatie van de bestaansvoorwaarde voor de gerelateerde resources.

Om beter te begrijpen hoe resources worden gemarkeerd als niet-compatibel, gaan we gebruiken de toewijzing van voorbeeld van een beleid dat hierboven is gemaakt.

Stel bijvoorbeeld dat er een resourcegroep: ContsoRG, met bepaalde opslagaccounts (rood gemarkeerd) die zichtbaar zijn voor openbare netwerken.

![Storage-accounts die zijn blootgesteld aan openbare netwerken](media/policy-insights/resource-group01.png)

In dit voorbeeld moet u Let van beveiligingsrisico's. Nu dat u een beleidstoewijzing gemaakt hebt, wordt deze voor alle opslagaccounts in de resourcegroep ContosoRG geëvalueerd. Gebeurtenissen uit te voeren op de drie niet-compatibele storage-accounts, als gevolg daarvan wijzigen van hun statussen voor **niet-compatibel.**

![Niet-compatibele storage-accounts gecontroleerd](media/policy-insights/resource-group03.png)

## <a name="portal"></a>Portal

De Azure-portal gepresenteerd een grafische ervaring voor het visualiseren en kennis van de status van naleving in uw omgeving. Op de **beleid** pagina de **overzicht** optie biedt details van de beschikbare scopes op de naleving van beleid en de initiatieven. Naast de compatibiliteitsstatus en het aantal per toewijzing, maar deze bevat een grafiek met naleving gedurende de afgelopen zeven dagen. De **naleving** pagina bevat veel van dezelfde gegevens (met uitzondering van de grafiek), maar bieden aanvullende filteren en sorteren op opties.

![Pagina naleving van beleid](media/policy-compliance/compliance-page.png)

Als er beleid of een initiatief kan worden toegewezen aan verschillende bereiken, houd er rekening mee in de tabel de scope voor elke toewijzing en het type van definitie die is toegewezen aan dit bereik. Het aantal niet-compatibele beleidsregels en niet-compatibele bronnen voor elke toewijzing worden ook gegeven. Te klikken op een beleid of het initiatief in de tabel biedt een diepgaande blik op de compatibiliteit voor die specifieke toewijzing.

![Details van compatibiliteit van beleid](media/policy-compliance/compliance-details.png)

Terwijl u de lijst met resources op de **niet-compatibel resources** tabblad geeft de status van de evaluatie van bestaande resources voor de huidige toewijzing, gebeurtenissen (toevoegen, controleren, weigeren, implementeren) geactiveerd door de aanvraag voor het maken van een resource zijn die wordt weergegeven onder de **gebeurtenissen** tabblad.

![Naleving beleidsgebeurtenissen](media/policy-compliance/compliance-events.png)

Met de rechtermuisknop op de rij van de gebeurtenis die u wilt meer informatie verzamelen over en selecteer **activiteitenlogboeken weergeven**. De pagina activiteitenlogboek wordt geopend en wordt vooraf gefilterd voor de zoekopdracht met details voor de toewijzing en de gebeurtenissen. Het activiteitenlogboek biedt aanvullende context en informatie over deze gebeurtenissen.

![Activiteitenlogboek voor naleving van beleid](media/policy-compliance/compliance-activitylog.png)

## <a name="command-line"></a>Opdrachtregel

Dezelfde informatie die beschikbaar is in de portal kan worden opgehaald met de REST API rechtstreeks (met inbegrip van met [ARMClient](https://github.com/projectkudu/ARMClient)) of Azure PowerShell gebruiken met de REST-API. Zie voor volledige informatie over de REST-API de [beleid Insights](/rest/api/policy-insights/) verwijzing. De verwijzing REST-API's zich op elke bewerking waarmee u kunt proberen een groen 'Deze probeer' knop rechts in de browser.

Als u de volgende voorbeelden in Azure PowerShell, geen verificatietoken met deze voorbeeldcode te maken. Vervang de $restUri door de gewenste tekenreeks in de voorbeelden voor het ophalen van een JSON-object dat vervolgens kan worden geparseerd.

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell

$azContext = Get-AzureRmContext
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
$token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
$authHeader = @{
    'Content-Type'='application/json'
    'Authorization'='Bearer ' + $token.AccessToken
}

# Define the REST API to communicate with
# Use double quotes for $restUri as some endpoints take strings passed in single quotes
$restUri = "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04"

# Invoke the REST API
$response = Invoke-RestMethod -Uri $restUri -Method POST -Headers $authHeader

# View the response object (as JSON)
$response
```

### <a name="summarize-results"></a>Resultaten samenvatten

Met de REST API, worden samenvatting uitgevoerd door de beheergroep, abonnement, resourcegroep, resource, initiatief, beleid, abonnement van een toewijzing of resource-groepstoewijzing niveau. Hier volgt een voorbeeld van de samenvatting op het abonnementsniveau met behulp van beleid inzicht [samenvatten voor abonnement](/rest/api/policy-insights/policystates/summarizeforsubscription):

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

De uitvoer bevat een overzicht van het abonnement. In de onderstaande voorbeelduitvoer de samengevatte compatibiliteit zijn onder **value.results.nonCompliantResources** en **value.results.nonCompliantPolicies**. Deze aanvraag biedt meer informatie, zoals elke toewijzing die uit de niet-compatibele cijfers en de sitedefinitie-informatie voor elke toewijzing bestaat. Elk beleidsobject in de hiërarchie biedt een **queryResultsUri** die kunnen worden gebruikt om aanvullende details ophalen op dat niveau.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Query voor bronnen

In het voorbeeld uit het bovenstaande **value.policyAssignments.policyDefinitions.results.queryResultsUri** met een voorbeeld van een Uri hebt opgegeven voor het ophalen van alle niet-compatibele bronnen voor de definitie van een specifiek beleid. Kijken naar de **$filter** waarde, IsCompliant is gelijk (eq) op false, PolicyAssignmentId is opgegeven voor de beleidsdefinitie en de PolicyDefinitionId zelf.
De reden voor het opnemen van de PolicyAssignmentId in het filter is omdat de PolicyDefinitionId kan bestaan in verschillende beleid of initiatief toewijzingen met tal van bereiken. Door te geven de PolicyAssignmentId zowel de PolicyDefinitionId, mag er expliciet in de resultaten die we zoekt. Eerder kon we gebruikt **nieuwste** voor de PolicyStates (de enige toegestane waarde voor **policyStatesSummaryResource** op de operator samenvatten voor abonnement), die automatisch ingesteld een  **van** en **naar** tijdvenster van de laatste 24 uur.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Het onderstaande voorbeeld-antwoord is bijgesneden voor het weergeven van slechts één niet-compatibele bron als beknopt alternatief bevat (Houd er rekening mee dat @odata.count daadwerkelijk 15 en komt overeen met het aantal niet-compatibele bronnen van het bovenstaande voorbeeld). De gedetailleerde reactie biedt verschillende soorten gegevens over de bron, het beleid (of initiatief) en de toewijzing. U ziet dat u kunt ook zien welke toewijzing-parameters zijn doorgegeven aan de beleidsdefinitie.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 15,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "timestamp": "2018-05-19T04:41:09Z",
        "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Compute/virtualMachines/linux",
        "policyAssignmentId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Authorization/policyAssignments/37ce239ae4304622914f0c77",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "effectiveParameters": "",
        "isCompliant": false,
        "subscriptionId": "{subscriptionId}",
        "resourceType": "/Microsoft.Compute/virtualMachines",
        "resourceLocation": "westus2",
        "resourceGroup": "RG-Tags",
        "resourceTags": "tbd",
        "policyAssignmentName": "37ce239ae4304622914f0c77",
        "policyAssignmentOwner": "tbd",
        "policyAssignmentParameters": "{\"tagName\":{\"value\":\"costCenter\"},\"tagValue\":{\"value\":\"Contoso-Test\"}}",
        "policyAssignmentScope": "/subscriptions/{subscriptionId}/resourceGroups/RG-Tags",
        "policyDefinitionName": "1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "policyDefinitionAction": "deny",
        "policyDefinitionCategory": "tbd",
        "policySetDefinitionId": "",
        "policySetDefinitionName": "",
        "policySetDefinitionOwner": "",
        "policySetDefinitionCategory": "",
        "policySetDefinitionParameters": "",
        "managementGroupIds": "",
        "policyDefinitionReferenceId": ""
    }]
}
```

### <a name="view-events"></a>Gebeurtenissen weergeven

Wanneer een bron wordt gemaakt of bijgewerkt, wordt een resultaat van evaluatie van beleid wordt gegenereerd. Resultaten heten _Beleidsgebeurtenissen_. Gebruik de volgende Uri recente Beleidsgebeurtenissen die zijn gekoppeld aan het abonnement wilt weergeven.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2018-04-04
```

De resultaten zien er ongeveer als volgt uit:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 16
    }]
}
```

Zie voor meer informatie over het uitvoeren van query's Beleidsgebeurtenissen de [Beleidsgebeurtenissen](/rest/api/policy-insights/policyevents) artikel.

### <a name="azure-powershell-preview"></a>Azure PowerShell (Preview)

De Azure PowerShell-module voor het beleid nog niet final is, maar is momenteel beschikbaar op de PowerShell-galerie als een [preview-release](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights).
Als PowerShellGet ten minste is versie 1.6.0 (vereist voor de ondersteuning van de voorlopige versie items), kunt u downloaden de preview-versie via `Install-Module` (Zorg ervoor dat de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) geïnstalleerd):

```powershell
# Download preview from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights -AllowPrerelease

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

De preview-module bevat drie cmdlets:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Voorbeeld: Ophalen van de status van de samenvatting voor het hoogste niveau toegewezen beleid met de hoogste aantal niet-compatibele bronnen.

```powershell
PS > Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Voorbeeld: De record status ophalen voor de meest recent geëvalueerd resource (de standaardwaarde is door een tijdstempel in aflopende volgorde).

```powershell
PS > Get-AzureRmPolicyState -Top 1

Timestamp                  : 5/22/2018 3:47:34 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/networkInterfaces/linux316
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/networkInterfaces
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Voorbeeld: Ophalen van de details voor alle resources voor niet-compatibele virtuele netwerk.

```powershell
PS > Get-AzureRmPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

Timestamp                  : 5/22/2018 4:02:20 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Voorbeeld: Ophalen van gebeurtenissen met betrekking tot niet-compatibele virtuele netwerkbronnen die is opgetreden na een bepaalde datum.

```powershell
PS > Get-AzureRmPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

Timestamp                  : 5/19/2018 5:18:53 AM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : eastus
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
TenantId                   : {tenantId}
PrincipalOid               : {principalOid}
```

De **PrincipalOid** veld kan worden gebruikt voor het ophalen van een specifieke gebruiker met de Azure PowerShell-cmdlet `Get-AzureRmADUser`. Vervang **{principalOid}** met het antwoord u van het vorige voorbeeld ophalen.

```powershell
PS > (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Als u hebt een [logboekanalyse](../log-analytics/log-analytics-overview.md) werkruimte met de `AzureActivity` oplossing gekoppeld aan uw abonnement en u kunt ook de resultaten van de niet-naleving van de beoordelingscyclus met eenvoudige Kusto query's weergeven en de `AzureActivity` tabel. Met de details van de niet-naleving in Log Analytics betekent dit ook dat waarschuwingen kunnen worden geconfigureerd om te controleren of niet-naleving van een bepaalde resource, resourcegroep of zelfs een drempelwaarde voor niet-compatibele items, zoals meer dan 10 in de afgelopen 24 uur.

![Naleving van beleid met Log Analytics](media/policy-compliance/compliance-loganalytics.png)

## <a name="next-steps"></a>Volgende stappen

- Controleer de [definitie beleidsstructuur](policy-definition.md).
- Bekijk [effecten beleid begrijpen](policy-effects.md).
- Bekijk een beheergroep met is [ordenen van uw resources met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md)