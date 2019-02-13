---
title: Beleid voor Nalevingsgegevens ophalen
description: Azure Policy-evaluaties en effecten bepaalt de naleving. Leer hoe u de details naleving ophalen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/01/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 40d0250101e4653cd5ab2a3610473d9c577d8998
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114107"
---
# <a name="getting-compliance-data"></a>Nalevingsgegevens ophalen

Een van de grootste voordelen van Azure Policy is het inzicht en de besturingselementen die het biedt ten opzichte van resources in een abonnement of [beheergroep](../../management-groups/overview.md) van abonnementen. Dit besturingselement kan worden uitgeoefend op veel verschillende manieren, zoals het voorkomen van resources die worden gemaakt in de verkeerde locatie afdwingen van het gebruik van de gemeenschappelijke en consistente tag, of bestaande resources van de controle voor configuraties en instellingen van toepassing. In alle gevallen worden gegevens wordt gegenereerd door beleid waarmee u inzicht krijgt in de compatibiliteitsstatus van uw omgeving.

Er zijn verschillende manieren toegang krijgen tot de informatie over naleving die zijn gegenereerd door uw beleid en initiatieftoewijzingen:

- Met behulp van de [Azure-portal](#portal)
- Via [vanaf de opdrachtregel](#command-line) scripting

Voordat u de methoden voor het rapporteren van naleving bekijkt, laten we kijken wanneer de compatibiliteitsinformatie wordt bijgewerkt en de frequentie en gebeurtenissen die een evaluatiecyclus van een te activeren.

> [!WARNING]
> Als de nalevingsstatus wordt gerapporteerd als **niet geregistreerd**, Controleer de **Microsoft.PolicyInsights** Resourceprovider is geregistreerd en dat de gebruiker de juiste toegang op basis van rollen heeft beheren () RBAC)-machtigingen, zoals wordt beschreven [hier](../overview.md#rbac-permissions-in-azure-policy).

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="evaluation-triggers"></a>Evaluatie-triggers

De resultaten van een voltooide evaluatiecyclus voor installatie zijn beschikbaar in de `Microsoft.PolicyInsights` Resource Provider via `PolicyStates` en `PolicyEvents` bewerkingen. Zie voor meer informatie over de bewerkingen van de Policy Insights REST-API, [Policy Insights](/rest/api/policy-insights/).

Evaluatie van toegewezen beleid en de initiatieven gebeuren als gevolg van diverse gebeurtenissen:

- Een beleid of initiatief is zojuist toegewezen aan een bereik. Het duurt ongeveer 30 minuten voor de toewijzing moet worden toegepast op het bereik zijn gedefinieerd. Zodra deze toegepast, de evaluatiecyclus voor installatie wordt gestart voor resources binnen dat bereik op basis van de zojuist toegewezen beleid of initiatief en afhankelijk van de effecten die worden gebruikt door het beleid of initiatief, resources gemarkeerd als compatibel of niet-compatibel. Een grote beleid of initiatief geëvalueerd op basis van een grote resourcesbereik kan even duren. Er is daarom geen vooraf gedefinieerde verwachting van wanneer u de evaluatiecyclus voor installatie wordt voltooid. Zodra deze is voltooid, is bijgewerkt nalevingsresultaten zijn beschikbaar in de portal en SDK's.

- Een beleid of initiatief is al toegewezen aan een bereik wordt bijgewerkt. De evaluatiefase computerbeleid en het tijdstip voor dit scenario is hetzelfde als voor een nieuwe toewijzing aan een bereik.

- Een resource wordt geïmplementeerd op een scope met een toewijzing via Resource Manager, REST, Azure CLI of Azure PowerShell. In dit scenario wordt de gebeurtenis effect (toevoegen, controleren, weigeren, implementeren) en informatie van de nalevingsstatus voor de afzonderlijke resource beschikbaar is in de portal en SDK's ongeveer 15 minuten later. Deze gebeurtenis niet leidt tot een evaluatie van andere bronnen.

- Standard naleving evaluatiecyclus voor installatie. Elke 24 uur, toewijzingen worden automatisch opnieuw geëvalueerd. Een grote beleid of initiatief van veel bronnen kan tijd duren, zodat er geen vooraf gedefinieerde verwachting van wanneer de evaluatiecyclus voor de wordt voltooid. Zodra deze is voltooid, is bijgewerkt nalevingsresultaten zijn beschikbaar in de portal en SDK's.

- De [Gast configuratie](../concepts/guest-configuration.md) resourceprovider is bijgewerkt met details naleving door een beheerde resourcegroep.

- Scan op aanvraag

### <a name="on-demand-evaluation-scan"></a>Evaluatie op aanvraag scannen

Een evaluatiescan voor een abonnement of resourcegroep kan worden gestart met een aanroep naar de REST-API. Deze scan is een asynchroon proces. De REST-eindpunt om te beginnen de scan wacht niet als zodanig totdat de scan is voltooid om te reageren. In plaats daarvan biedt geen URI voor de status van de aangevraagde evaluatie opvragen.

In elke REST API-URI zijn er verschillende variabelen die worden gebruikt en die u moet vervangen door uw eigen waarden:

- `{YourRG}` -Vervangen door de naam van de resourcegroep
- Vervang `{subscriptionId}` door uw abonnements-ID

De scan biedt ondersteuning voor evaluatie van de resources in een abonnement of in een resourcegroep. Start een scan door een bereik met een REST-API **POST** opdracht met behulp van de volgende URI structuren:

- Abonnement

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

- Resourcegroep

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{YourRG}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

De aanroep retourneert een **202 geaccepteerd** status. Opgenomen in het antwoord-header is een **locatie** eigenschap met de volgende indeling:

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/asyncOperationResults/{ResourceContainerGUID}?api-version=2018-07-01-preview
```

`{ResourceContainerGUID}` statisch is gegenereerd voor het bereik dat is aangevraagd. Als een bereik wordt al uitgevoerd voor een scan op aanvraag, wordt een nieuwe scan is niet gestart. In plaats daarvan de nieuwe aanvraag wordt opgegeven hetzelfde `{ResourceContainerGUID}` **locatie** URI voor de status. Een REST-API **ophalen** opdracht naar de **locatie** URI retourneert een **202 geaccepteerd** tijdens de evaluatie wordt momenteel is. Wanneer de evaluatiescan is voltooid, wordt een **200 OK** status. De hoofdtekst van een voltooide scan is een JSON-antwoord met de status:

```json
{
    "status": "Succeeded"
}
```

## <a name="how-compliance-works"></a>De werking van naleving

In een toewijzing, een resource is **niet-compatibele** als deze niet aan de regels voor beleid of initiatief voldoen. De volgende tabel toont hoe verschillende beleid effecten met de evaluatie van de voorwaarden voor de resulterende nalevingsstatus werken:

| De status van resource | Effect | Evaluatie van het beleid | Nalevingsstatus |
| --- | --- | --- | --- |
| Bestaat | Weigeren, Controleren, Toevoegen\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Niet-compatibel |
| Bestaat | Weigeren, Controleren, Toevoegen\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Compatibel |
| Nieuw | Controleren, AuditIfNotExist\* | True | Niet-compatibel |
| Nieuw | Controleren, AuditIfNotExist\* | False | Compatibel |

\*Voor de acties Toevoegen, DeployIfNotExist en AuditIfNotExist moet de IF-instructie TRUE zijn.
De acties vereisen ook dat de bestaansvoorwaarde FALSE is om niet-compatibel te zijn. Indien TRUE, activeert de IF-voorwaarde de evaluatie van de bestaansvoorwaarde voor de gerelateerde resources.

Bijvoorbeeld, wordt ervan uitgegaan dat u een resourcegroep – ContsoRG, sommige opslagaccounts hebt (rood gemarkeerd) die worden weergegeven met een openbaar netwerk.

![Storage-accounts beschikbaar gemaakt met een openbaar netwerk](../media/getting-compliance-data/resource-group01.png)

In dit voorbeeld moet u het gebruik van beveiligingsrisico's. Nu dat u een beleidstoewijzing gemaakt hebt, wordt deze voor alle opslagaccounts in de resourcegroep ContosoRG geëvalueerd. Het voert een controle uit de drie niet-compatibele storage-accounts, als gevolg hiervan wijzigen van hun status naar **niet-compatibel.**

![Niet-compatibele storage-accounts gecontroleerd](../media/getting-compliance-data/resource-group03.png)

Naast **compatibele** en **niet-compatibele**, beleidsregels en resources hebben andere statussen:

- **Conflicterende**: Er zijn twee of meer sets beleidsregels met conflicterende regels. Bijvoorbeeld: twee beleidsregels die dezelfde tag met verschillende waarden toe te voegen.
- **Niet gestart**: De evaluatiecyclus voor installatie van nog niet is gestart voor het beleid of de resource.
- **Niet geregistreerd**: De Resourceprovider van de Azure-beleid is niet geregistreerd of het account aangemeld is niet gemachtigd om Nalevingsgegevens te lezen.

Maakt gebruik van beleid voor de **type** en **naam** velden in het definitie om te bepalen of een resource een overeenkomst. Wanneer de resource overeenkomt met, deze wordt beschouwd als die van toepassing zijn, en heeft de status van een van beide **compatibele** of **niet-compatibele**. Als een van beide **type** of **naam** is de enige eigenschap in de definitie van alle resources worden beschouwd als die van toepassing zijn en worden geëvalueerd.

Het compatibiliteitspercentage wordt bepaald door het delen van **compatibele** resources door _totaal aantal resources_.
_Totaal aantal resources_ wordt gedefinieerd als de som van de **compatibele**, **niet-compatibele**, en **conflicterende** resources. De algemene naleving getallen zijn de som van afzonderlijke resources die zijn **compatibele** gedeeld door de som van alle afzonderlijke resources. In de onderstaande afbeelding zijn er 20 verschillende resources die van toepassing zijn en is slechts één **niet-compatibele**. De algemene compatibiliteit van de resource is 95% (19 van 20).

![Voorbeeld van eenvoudige naleving](../media/getting-compliance-data/simple-compliance.png)

## <a name="portal"></a>Portal

De Azure portal brengt een grafische interface van visualiseren en inzicht krijgen in de status van naleving in uw omgeving. Op de **beleid** pagina, de **overzicht** optie bevat details voor beschikbare bereiken op de naleving van beleid en de initiatieven. Samen met de compatibiliteitsstatus en het aantal per toewijzing bevat een grafiek met naleving in de afgelopen zeven dagen.
De **naleving** pagina bevat veel van dezelfde informatie (met uitzondering van de grafiek), maar bieden aanvullende filteren en sorteren van opties.

![Pagina voor het naleven van beleid](../media/getting-compliance-data/compliance-page.png)

Omdat een beleid of initiatief kan worden toegewezen aan verschillende bereiken, bevat de tabel in het bereik voor elke toewijzing en het type van de definitie die is toegewezen. Het aantal niet-compatibele resources en niet-conforme beleidsregels voor elke toewijzing worden ook gegeven. Te klikken op een beleid of initiatief in de tabel bevat een stil bij de naleving voor die bepaalde toewijzing.

![Details van compatibiliteit van beleid](../media/getting-compliance-data/compliance-details.png)

De lijst met resources op de **resourcenaleving** tabblad toont de evaluatiestatus van bestaande resources voor de huidige toewijzing. Het tabblad standaard ingesteld op **niet-compatibele**, maar kan worden gefilterd.
Gebeurtenissen (toevoegen, controleren, weigeren, implementeren) geactiveerd door de aanvraag voor het maken van een resource die wordt weergegeven onder de **gebeurtenissen** tabblad.

![Beleid voor naleving-gebeurtenissen](../media/getting-compliance-data/compliance-events.png)

Met de rechtermuisknop op de rij van de gebeurtenis die u wilt meer informatie verzamelen over en selecteer **activiteitlogboeken weergeven**. De pagina activiteitenlogboek wordt geopend en wordt vooraf gefilterd voor de zoekopdracht met de details voor de toewijzing en de gebeurtenissen. Het activiteitenlogboek bevat aanvullende context en informatie over deze gebeurtenissen.

![Beleid voor naleving-activiteitenlogboek](../media/getting-compliance-data/compliance-activitylog.png)

### <a name="change-history-preview"></a>Wijzigingsoverzicht (Preview)

Als onderdeel van een nieuwe **preview-versie**, de laatste veertien dagen van de wijzigingsgeschiedenis zijn beschikbaar voor een niet-compatibele resource. Wijzigingsoverzicht biedt meer informatie over wanneer een wijziging is gedetecteerd en een _visual diff_ voor elke wijziging. De detectie van een wijziging wordt geactiveerd wanneer de Resource Manager-eigenschappen van een niet-compatibele resource worden toegevoegd, verwijderd of gewijzigd.

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

1. Op de **overzicht** of **naleving** pagina, selecteert u een beleid dat is _niet-compatibele_.

1. Onder de **resourcenaleving** tabblad van de **beleidsnaleving** pagina, selecteert u een resource dit is _niet-compatibele_.

1. Selecteer de **wijzigingsoverzicht (preview)** tabblad op de **Resourcenaleving** pagina. Een lijst met gedetecteerd wijzigingen, als alle bestaan, worden weergegeven.

   ![Beleid wijzigingsoverzicht - tabblad](../media/getting-compliance-data/change-history-tab.png)

1. Selecteer een van de gedetecteerde wijzigingen. De _visual diff_ voor de niet-compatibele resource wordt weergegeven op de **wijzigingsoverzicht** pagina.

   ![Beleid wijzigingsoverzicht - Visual Diff](../media/getting-compliance-data/change-history-visual-diff.png)

De _visual diff_ aides bij het identificeren van wijzigingen aan een resource. De wijzigingen gedetecteerd kunnen niet worden gerelateerd aan wat de oorzaak van de resource niet-compatibel is op het geselecteerde beleid.

## <a name="command-line"></a>Opdrachtregel

De dezelfde informatie die beschikbaar zijn in de portal kan worden opgehaald met de REST-API (met inbegrip van met [ARMClient](https://github.com/projectkudu/ARMClient)) of Azure PowerShell. Zie voor meer informatie over de REST-API, de [Policy Insights](/rest/api/policy-insights/) verwijzing. De naslaginformatie over REST-API's hebben een groene 'Try It' knop op elke bewerking die kunt u nu uitproberen rechts in de browser.

Voor het gebruik van de volgende voorbeelden in Azure PowerShell, maakt u een verificatietoken met deze voorbeeldcode. Vervang vervolgens de $restUri door de tekenreeks in de voorbeelden om op te halen van een JSON-object dat vervolgens kan worden geparseerd.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

$azContext = Get-AzContext
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

Met de REST-API kan samenvatting worden uitgevoerd door de container, de definitie of de toewijzing. Hier volgt een voorbeeld van het samenvatten op het abonnementsniveau van het met behulp van beleid inzicht [samenvatten voor abonnement](/rest/api/policy-insights/policystates/summarizeforsubscription):

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

In het bovenstaande voorbeeld **value.policyAssignments.policyDefinitions.results.queryResultsUri** biedt een voorbeeld van een Uri voor alle niet-compatibele resources voor de definitie van een specifiek beleid. Kijken naar de **$filter** waarde, IsCompliant is gelijk aan (eq) op false, PolicyAssignmentId is opgegeven voor de beleidsdefinitie, waarna de PolicyDefinitionId zelf. De reden voor het opnemen van de PolicyAssignmentId in het filter is omdat de PolicyDefinitionId kan bestaan in verschillende beleid of initiatieftoewijzingen met verschillende bereiken. Zowel de PolicyAssignmentId en de PolicyDefinitionId opgeeft, kunnen we in de resultaten die we op zoek bent naar expliciete zijn. Voorheen voor PolicyStates we gebruikt **nieuwste**, die automatisch wordt ingesteld een **van** en **naar** tijdvenster van de afgelopen 24-uur.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Het onderstaande voorbeeldantwoord is bijgesneden tot een enkele niet-compatibele resource kort te houden. Het gedetailleerde antwoord heeft verschillende soorten gegevens over de resource, het beleid of initiatief en de toewijzing. U ziet dat u kunt ook zien welke toewijzing-parameters zijn doorgegeven aan de beleidsdefinitie.

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

De Azure PowerShell-module voor het beleid is beschikbaar op de PowerShell Gallery als [Az.PolicyInsights](https://www.powershellgallery.com/packages/Az.PolicyInsights). U PowerShellGet gebruikt, kunt u Installeer de module met `Install-Module -Name Az.PolicyInsights` (Zorg ervoor dat u hebt de meest recente [Azure PowerShell](/powershell/azure/install-az-ps) geïnstalleerd):

```azurepowershell-interactive
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name Az.PolicyInsights

# Import the downloaded module
Import-Module Az.PolicyInsights

# Login with Connect-AzAccount if not using Cloud Shell
Connect-AzAccount
```

De module heeft de volgende cmdlets:

- `Get-AzPolicyStateSummary`
- `Get-AzPolicyState`
- `Get-AzPolicyEvent`
- `Get-AzPolicyRemediation`
- `Remove-AzPolicyRemediation`
- `Start-AzPolicyRemediation`
- `Stop-AzPolicyRemediation`

Voorbeeld: Ophalen van de status van de samenvatting voor het bovenste toegewezen beleid met het hoogste aantal niet-compatibele resources.

```azurepowershell-interactive
PS> Get-AzPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Voorbeeld: Ophalen van de record van de status voor de meest recent geëvalueerd bron (standaard is door de timestamp in aflopende volgorde).

```azurepowershell-interactive
PS> Get-AzPolicyState -Top 1

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

```azurepowershell-interactive
PS> Get-AzPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

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

Voorbeeld: Het ophalen van gebeurtenissen met betrekking tot niet-compatibele virtuele-netwerkbronnen die zijn opgetreden na een bepaalde datum.

```azurepowershell-interactive
PS> Get-AzPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

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

De **PrincipalOid** veld kan worden gebruikt om op te halen van een specifieke gebruiker met de Azure PowerShell-cmdlet `Get-AzADUser`. Vervang **{principalOid}** met het antwoord u uit het vorige voorbeeld krijgt.

```azurepowershell-interactive
PS> (Get-AzADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="azure-monitor-logs"></a>Logboeken in Azure Monitor

Als u hebt een [Log Analytics-werkruimte](../../../log-analytics/log-analytics-overview.md) met de `AzureActivity` oplossing die zijn gekoppeld aan uw abonnement, kunt u ook niet-naleving-resultaten van de evaluatiefase computerbeleid met behulp van eenvoudige query's van Azure Data Explorer weergeven en de `AzureActivity` de tabel. Met de informatie in Azure Monitor-Logboeken, kunnen waarschuwingen worden geconfigureerd als u wilt bekijken voor niet-naleving.

![Naleving van het beleid met behulp van Azure Monitor-Logboeken](../media/getting-compliance-data/compliance-loganalytics.png)

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](../concepts/definition-structure.md)
- Beoordeling [effecten beleid begrijpen](../concepts/effects.md)
- Begrijpen hoe u [programmatisch beleid maken](programmatically-create.md)
- Meer informatie over het [herstellen van niet-compatibele resources](remediate-resources.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)