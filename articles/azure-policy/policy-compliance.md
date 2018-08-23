---
title: Nalevingsgegevens ophalen in Azure Policy
description: Azure Policy-evaluaties en effecten bepaalt de naleving. Leer hoe u de details naleving ophalen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/29/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: bd3eeb5ebb9b30ac315fee1597348f3bd34f3bb6
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42059501"
---
# <a name="getting-compliance-data"></a>Ophalen van Nalevingsgegevens

Een van de grootste voordelen van Azure Policy is het inzicht en de besturingselementen die het biedt ten opzichte van resources in een abonnement of [beheergroep](../azure-resource-manager/management-groups-overview.md) van abonnementen. Dit besturingselement kan worden uitgeoefend op veel verschillende manieren, zoals het voorkomen van resources die worden gemaakt in de verkeerde locatie afdwingen van het gebruik van de gemeenschappelijke en consistente tag, of bestaande resources van de controle voor configuraties en instellingen van toepassing. In alle gevallen worden gegevens wordt gegenereerd door beleid waarmee u inzicht krijgt in de compatibiliteitsstatus van uw omgeving.

Er zijn verschillende manieren toegang krijgen tot de informatie over naleving die zijn gegenereerd door uw beleid en initiatieftoewijzingen:

- Met behulp van de [Azure-Portal](#portal)
- Via [vanaf de opdrachtregel](#command_line) scripting

Voordat u de methoden voor het rapporteren van naleving bekijkt, laten we kijken wanneer de compatibiliteitsinformatie wordt bijgewerkt en de frequentie en gebeurtenissen die een evaluatiecyclus van een te activeren.

> [!WARNING]
> Als de nalevingsstatus wordt gerapporteerd als **'N.V.T.'**, Controleer de **Microsoft.PolicyInsights** Resourceprovider is geregistreerd en de gebruiker beschikt over de juiste op rollen gebaseerd toegangsbeheer (RBAC) de machtigingen zoals wordt beschreven [hier](azure-policy-introduction.md#rbac-permissions-in-azure-policy).

## <a name="evaluation-triggers"></a>Evaluatie-triggers

De resultaten van een voltooide evaluatiecyclus voor installatie worden weerspiegeld in de `Microsoft.PolicyInsights` Resource Provider via `PolicyStates` en `PolicyEvents` bewerkingen. Zie voor meer informatie over de opties en mogelijkheden van de Policy Insights REST-API, [Policy Insights](/rest/api/policy-insights/).

Evaluatie van toegewezen beleid en de initiatieven gebeuren als gevolg van diverse gebeurtenissen:

- Een beleid of initiatief is zojuist toegewezen aan een bereik. Wanneer dit gebeurt, duurt het ongeveer 30 minuten voor de toewijzing moet worden toegepast op het bereik zijn gedefinieerd. Zodra deze is toegepast, de evaluatiecyclus voor installatie wordt gestart voor resources binnen dat bereik op basis van de zojuist toegewezen beleid of initiatief en afhankelijk van de effecten die worden gebruikt door het beleid of initiatief, resources gemarkeerd als compatibel of niet-compatibel. Een grote beleid of initiatief geëvalueerd op basis van een grote resourcesbereik kan tijd duren, zodat er geen vooraf gedefinieerde verwachting van wanneer de evaluatiecyclus voor de wordt voltooid. Zodra deze is voltooid, is bijgewerkt nalevingsresultaten zijn beschikbaar in de portal en SDK's.
- Een beleid of initiatief is al toegewezen aan een bereik wordt bijgewerkt. De evaluatiefase computerbeleid en het tijdstip voor dit scenario is hetzelfde als voor een nieuwe toewijzing aan een bereik.
- Een resource wordt geïmplementeerd op een scope met een toewijzing via Resource Manager, REST, Azure CLI of Azure PowerShell. In dit scenario wordt de gebeurtenis effect (toevoegen, controleren, weigeren, implementeren) en informatie van de nalevingsstatus voor de afzonderlijke resource beschikbaar is in de portal en SDK's ongeveer 15 minuten later. Deze gebeurtenis leidt niet tot een evaluatie van andere bronnen.
- Standard naleving evaluatiecyclus voor installatie. Elke 24 uur, toewijzingen worden automatisch opnieuw worden geëvalueerd. Een grote beleid of initiatief geëvalueerd op basis van een grote resourcesbereik kan tijd duren, zodat er geen vooraf gedefinieerde verwachting van wanneer de evaluatiecyclus voor de wordt voltooid. Zodra deze is voltooid, is bijgewerkt nalevingsresultaten zijn beschikbaar in de portal en SDK's.

## <a name="how-compliance-works"></a>De werking van naleving

Een resource is in een toewijzing, niet-compatibel als het beleid of initiatief regels niet volgen. De volgende tabel toont hoe verschillende beleid effecten met de evaluatie van de voorwaarden voor de resulterende nalevingsstatus werken:

| De status van resource | Effect | Evaluatie van het beleid | Nalevingsstatus |
| --- | --- | --- | --- |
| Bestaat | Weigeren, Controleren, Toevoegen\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Niet-compatibel |
| Bestaat | Weigeren, Controleren, Toevoegen\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Compatibel |
| Nieuw | Controleren, AuditIfNotExist\* | True | Niet-compatibel |
| Nieuw | Controleren, AuditIfNotExist\* | False | Compatibel |

\*Voor de acties Toevoegen, DeployIfNotExist en AuditIfNotExist moet de IF-instructie TRUE zijn.
De acties vereisen ook dat de bestaansvoorwaarde FALSE is om niet-compatibel te zijn. Indien TRUE, activeert de IF-voorwaarde de evaluatie van de bestaansvoorwaarde voor de gerelateerde resources.

Bijvoorbeeld, wordt ervan uitgegaan dat u een resourcegroep – ContsoRG, sommige opslagaccounts hebt (rood gemarkeerd) die worden weergegeven met een openbaar netwerk.

![Storage-accounts beschikbaar gemaakt met een openbaar netwerk](media/policy-insights/resource-group01.png)

In dit voorbeeld moet u het gebruik van beveiligingsrisico's. Nu dat u een beleidstoewijzing gemaakt hebt, wordt deze voor alle opslagaccounts in de resourcegroep ContosoRG geëvalueerd. Het voert een controle uit de drie niet-compatibele storage-accounts, als gevolg hiervan wijzigen van hun status naar **niet-compatibel.**

![Niet-compatibele storage-accounts gecontroleerd](media/policy-insights/resource-group03.png)

## <a name="portal"></a>Portal

De Azure portal brengt een grafische interface van visualiseren en inzicht krijgen in de status van naleving in uw omgeving. Op de **beleid** pagina, de **overzicht** optie bevat details voor beschikbare bereiken op de naleving van beleid en de initiatieven. Naast de compatibiliteitsstatus en het aantal per toewijzing bevat een grafiek met naleving in de afgelopen zeven dagen. De **naleving** pagina bevat veel van dezelfde informatie (met uitzondering van de grafiek), maar bieden aanvullende filteren en sorteren van opties.

![Pagina voor het naleven van beleid](media/policy-compliance/compliance-page.png)

Als een beleid of initiatief kan worden toegewezen aan verschillende bereiken, houd er rekening mee in de tabel het bereik voor elke toewijzing en het type van de definitie die is toegewezen aan dat bereik. Het aantal niet-conforme beleidsregels en niet-compatibele resources voor elke toewijzing worden ook gegeven. Te klikken op een beleid of initiatief in de tabel bevat een stil bij de naleving voor die bepaalde toewijzing.

![Details van compatibiliteit van beleid](media/policy-compliance/compliance-details.png)

Terwijl u de lijst met resources op de **niet-compatibele resources** tabblad geeft de evaluatiestatus van bestaande resources voor de huidige toewijzing gebeurtenissen (toevoegen, controleren, weigeren, implementeren) geactiveerd door de aanvraag voor het maken van een resource zijn die wordt weergegeven onder de **gebeurtenissen** tabblad.

![Beleid voor naleving-gebeurtenissen](media/policy-compliance/compliance-events.png)

Met de rechtermuisknop op de rij van de gebeurtenis die u wilt meer informatie verzamelen over en selecteer **activiteitlogboeken weergeven**. De pagina activiteitenlogboek wordt geopend en wordt vooraf gefilterd voor de zoekopdracht met de details voor de toewijzing en de gebeurtenissen. Het activiteitenlogboek bevat aanvullende context en informatie over deze gebeurtenissen.

![Beleid voor naleving-activiteitenlogboek](media/policy-compliance/compliance-activitylog.png)

## <a name="command-line"></a>Opdrachtregel

Dezelfde informatie die beschikbaar is in de portal kan worden opgehaald met behulp van de REST-API rechtstreeks (met inbegrip van met [ARMClient](https://github.com/projectkudu/ARMClient)) of Azure PowerShell gebruiken met de REST-API. Zie voor meer informatie over de REST-API, de [Policy Insights](/rest/api/policy-insights/) verwijzing. De naslaginformatie over REST-API's hebben een groene 'Try It' knop op elke bewerking die kunt u nu uitproberen rechts in de browser.

Voor het gebruik van de volgende voorbeelden in Azure PowerShell, maakt u een verificatietoken met deze voorbeeldcode. Vervang vervolgens de $restUri door de gewenste tekenreeks in de voorbeelden om op te halen van een JSON-object dat vervolgens kan worden geparseerd.

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

Met behulp van de REST-API, worden samenvatting uitgevoerd door de beheergroep, abonnement, resourcegroep, resource, initiatief, beleid, toewijzing van abonnement of niveau groepstoewijzing van de resource. Hier volgt een voorbeeld van het samenvatten op het abonnementsniveau van het met behulp van beleid inzicht [samenvatten voor abonnement](/rest/api/policy-insights/policystates/summarizeforsubscription):

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

De uitvoer bevat een overzicht van het abonnement. In de onderstaande voorbeelduitvoer de samengevatte naleving zijn onder **value.results.nonCompliantResources** en **value.results.nonCompliantPolicies**. Deze aanvraag biedt meer details, met inbegrip van elke toewijzing die uit de niet-compatibele cijfers en de sitedefinitie-informatie voor elke toewijzing bestaat. Elk beleidsobject in de hiërarchie biedt een **queryResultsUri** die kunnen worden gebruikt om aanvullende informatie op dat niveau krijgen.

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

In het voorbeeld uit het bovenstaande **value.policyAssignments.policyDefinitions.results.queryResultsUri** voorzien van ons een voorbeeld van een Uri voor het ophalen van alle niet-compatibele resources voor de definitie van een specifiek beleid. Kijken naar de **$filter** waarde, IsCompliant is gelijk aan (eq) op false, PolicyAssignmentId is opgegeven voor de beleidsdefinitie, waarna de PolicyDefinitionId zelf.
De reden voor het opnemen van de PolicyAssignmentId in het filter is omdat de PolicyDefinitionId kan bestaan in verschillende beleid of initiatieftoewijzingen met verschillende bereiken. Zowel de PolicyAssignmentId en de PolicyDefinitionId opgeeft, kunnen we zijn expliciete in de resultaten die we op zoek bent. Eerder moesten we gebruikt **nieuwste** voor de PolicyStates (de enige toegestane waarde voor **policyStatesSummaryResource** op de operator samenvatten voor abonnement), die automatisch wordt ingesteld een  **van** en **naar** tijdvenster van de afgelopen 24-uur.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Het onderstaande voorbeeldantwoord is bijgesneden om slechts één niet-compatibele resource beknopt alternatief (Houd er rekening mee dat @odata.count daadwerkelijk 15 is en overeenkomt met het aantal niet-compatibele resources uit het bovenstaande voorbeeld). Het gedetailleerde antwoord biedt verschillende soorten gegevens over de resource, het beleid (of initiatief) en de toewijzing. U ziet dat u kunt ook zien welke toewijzing-parameters zijn doorgegeven aan de beleidsdefinitie.

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

Wanneer een resource wordt gemaakt of bijgewerkt, wordt een resultaat van evaluatie van beleid wordt gegenereerd. Resultaten worden genoemd _Beleidsgebeurtenissen_. Gebruik de volgende Uri om recente voor Beleidsgebeurtenissen die zijn gekoppeld aan het abonnement weer te geven.

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

Zie voor meer informatie over het uitvoeren van query's Beleidsgebeurtenissen, de [Beleidsgebeurtenissen](/rest/api/policy-insights/policyevents) naslagartikel.

### <a name="azure-powershell"></a>Azure PowerShell

De Azure PowerShell-module voor het beleid is beschikbaar op de PowerShell Gallery als [AzureRM.PolicyInsights](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights). U PowerShellGet gebruikt, kunt u Installeer de module met `Install-Module -Name AzureRM.PolicyInsights` (Zorg ervoor dat u hebt de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) geïnstalleerd):

```powershell
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

De module heeft drie cmdlets:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Voorbeeld: Ophalen van de status van de samenvatting voor het bovenste toegewezen beleid met het hoogste aantal niet-compatibele resources.

```powershell
PS > Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Voorbeeld: Ophalen van de record van de status voor de meest recent geëvalueerd bron (standaard is door de timestamp in aflopende volgorde).

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

Voorbeeld: Ophalen van de details voor alle niet-compatibele virtuele-netwerkbronnen.

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

Voorbeeld: Ophalen van gebeurtenissen met betrekking tot niet-compatibele virtuele-netwerkbronnen die zijn opgetreden na een bepaalde datum.

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

De **PrincipalOid** veld kan worden gebruikt om op te halen van een specifieke gebruiker met de Azure PowerShell-cmdlet `Get-AzureRmADUser`. Vervang **{principalOid}** met het antwoord u uit het vorige voorbeeld krijgt.

```powershell
PS > (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Als u hebt een [Log Analytics](../log-analytics/log-analytics-overview.md) werkruimte met de `AzureActivity` oplossing die is gekoppeld aan uw abonnement, en vervolgens kunt u ook niet-naleving-resultaten van de evaluatiefase computerbeleid met behulp van eenvoudige Kusto-query's weergeven en de `AzureActivity` tabel. Met de details van de niet-naleving in Log Analytics betekent dit ook dat waarschuwingen kunnen worden geconfigureerd om te kijken voor niet-naleving op een specifieke resource, resourcegroep of zelfs een drempelwaarde voor niet-compatibele items, zoals meer dan 10 in de afgelopen 24 uur.

![Naleving van het beleid met behulp van Log Analytics](media/policy-compliance/compliance-loganalytics.png)

## <a name="next-steps"></a>Volgende stappen

- Bekijk de [structuur van Azure Policy-definities](policy-definition.md).
- Lees [Informatie over de effecten van het beleid](policy-effects.md).
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md)