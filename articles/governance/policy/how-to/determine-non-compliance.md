---
title: Oorzaken van niet-naleving bepalen
description: Wanneer een resource niet-compatibele is, zijn er veel mogelijke oorzaken. Leer hoe u meer informatie over de oorzaak van de niet-naleving.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: fb7f238bb5c04bb03ee500b1b953895cc88c0596
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298925"
---
# <a name="determine-causes-of-non-compliance"></a>Oorzaken van niet-naleving bepalen

Wanneer een Azure-resource blijkt dat het niet-compatibel is op een beleidsregel, is het handig om te begrijpen welk deel van de regel voor de resource is niet compatibel zijn met. Het is ook handig om te begrijpen welke wijziging gewijzigd een voorheen compliant bron uit voor niet-compatibel te maken. Er zijn twee manieren om deze informatie te vinden:

> [!div class="checklist"]
> - [Details van compatibiliteit](#compliance-details)
> - [Wijzigingsoverzicht (Preview)](#change-history-preview)

## <a name="compliance-details"></a>Details van compatibiliteit

Wanneer een resource niet-compatibel is, de compatibiliteitsdetails voor die bron zijn beschikbaar via de **beleidsnaleving** pagina. Het deelvenster met details naleving bevat de volgende informatie:

- Details van de resource, zoals naam, type, locatie en resourcegroep-ID
- De nalevingsstatus en de tijdstempel van de laatste evaluatie voor de huidige beleidstoewijzing
- Een lijst met _redenen_ voor de resource niet-naleving

> [!IMPORTANT]
> Als de compatibiliteitsdetails voor een _niet-compatibele_ resource geeft de huidige waarde van eigenschappen voor die bron, de gebruiker moet beschikken over **lezen** bewerking naar de **type** van de bron. Bijvoorbeeld, als de _niet-compatibele_ resource **Microsoft.Compute/virtualMachines** en vervolgens de gebruiker moet beschikken over de **Microsoft.Compute/virtualMachines/read** de bewerking. Als de gebruiker beschikt niet over de benodigde bewerking, wordt een toegangsfout weergegeven.

Als u wilt weergeven van de details van de naleving, de volgende stappen uit:

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

1. Op de **overzicht** of **naleving** pagina, selecteert u een beleid in een **nalevingsstatus** dat wil zeggen _niet-compatibele_.

1. Onder de **resourcenaleving** tabblad van de **beleidsnaleving** pagina, met de rechtermuisknop op of Selecteer het beletselteken van een bron in een **nalevingsstatus** dat wil zeggen  _Niet-compatibele_. Selecteer vervolgens **compatibiliteitsdetails weergeven**.

   ![Optie voor naleving details weergeven](../media/determine-non-compliance/view-compliance-details.png)

1. De **compatibiliteitsdetails** in gegevens van de meest recente beoordeling van de resource naar de huidige toewijzing van configuratiebeleid wordt weergegeven. In dit voorbeeld wordt het veld **Microsoft.Sql/servers/version** is geconstateerd _12.0_ terwijl de beleidsdefinitie verwacht _14.0_. Als de resource om verschillende redenen niet-compatibel is, wordt elk vermeld in dit deelvenster.

   ![Deelvenster met details naleving en de redenen voor niet-naleving](../media/determine-non-compliance/compliance-details-pane.png)

   Voor een **auditIfNotExists** of **deployIfNotExists** beleidsdefinitie, de volgende gegevens bevatten de **details.type** eigenschap en de optionele eigenschappen. Zie voor een lijst [auditIfNotExists eigenschappen](../concepts/effects.md#auditifnotexists-properties) en [deployIfNotExists eigenschappen](../concepts/effects.md#deployifnotexists-properties). **Laatst geëvalueerd resource** is een gerelateerde bron van de **details** sectie van de definitie.

   Voorbeeld van de gedeeltelijke **deployIfNotExists** definitie:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Deelvenster met details naleving - * ifNotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> Om gegevens te beveiligen, wanneer een eigenschapswaarde is een _geheim_ sterretjes wordt de huidige waarde weergegeven.

Deze gegevens wordt uitgelegd waarom een resource is momenteel niet-compatibel, maar niet weergeven wanneer de wijziging is doorgevoerd naar de resource, waardoor er worden niet-compatibel. Voor die informatie, Zie [wijzigingsoverzicht (Preview)](#change-history-preview) hieronder.

### <a name="compliance-reasons"></a>Nalevingsredenen

De volgende matrix wijst elke mogelijke _reden_ naar de verantwoordelijk [voorwaarde](../concepts/definition-structure.md#conditions) in de beleidsdefinitie:

|Reason | Voorwaarde |
|-|-|
|Huidige waarde moet de doelwaarde bevatten als een sleutel. |containsKey of **niet** notContainsKey |
|Huidige waarde moet de doelwaarde bevatten. |bevat of **niet** notContains |
|Huidige waarde moet gelijk zijn aan de doel-waarde. |is gelijk aan of **niet** notEquals |
|Huidige waarde moet kleiner zijn dan de doelwaarde. |minder of **niet** greaterOrEquals |
|Huidige waarde moet groter zijn dan of gelijk zijn aan de doel-waarde. |greaterOrEquals of **niet** minder |
|Huidige waarde moet groter zijn dan de doelwaarde. |groter of **niet** lessOrEquals |
|Huidige waarde moet kleiner dan of gelijk zijn aan de doel-waarde. |lessOrEquals of **niet** groter |
|Huidige waarde moet bestaan. |Er bestaat |
|Huidige waarde moet in de doel-waarde. |in of **niet** notIn |
|Huidige waarde moet liggen, zoals de doelwaarde. |Net als of **niet** notlike zijn |
|Huidige waarde moet hoofdlettergevoelige overeenkomst de doelwaarde. |overeenkomen met of **niet** notMatch |
|Huidige waarde moet niet-hoofdlettergevoelige overeenkomst de doelwaarde. |matchInsensitively of **niet** notMatchInsensitively |
|Huidige waarde mag niet de doel-waarde als een sleutel. |notContainsKey of **niet** containsKey|
|Huidige waarde mag niet de doelwaarde. |notContains of **niet** bevat |
|Huidige waarde mag niet gelijk zijn aan de doel-waarde zijn. |notEquals of **niet** is gelijk aan |
|Huidige waarde mag niet bestaan. |**niet** bestaat  |
|Huidige waarde mag geen van de doelwaarde. |notIn of **niet** in |
|Huidige waarde mag niet zijn, zoals de doelwaarde. |notlike zijn of **niet** zoals |
|Huidige waarde moet niet-hoofdlettergevoelige overeenkomst de doelwaarde. |notMatch of **niet** overeen met |
|Huidige waarde moet geen niet-hoofdlettergevoelige overeenkomst de doelwaarde. |notMatchInsensitively of **niet** matchInsensitively |
|Er zijn geen verwante resources komt overeen met de details van het beleidseffect in de beleidsdefinitie. |Een resource van het type dat is gedefinieerd in **then.details.type** en gerelateerd zijn aan de resource die is gedefinieerd in de **als** gedeelte van de beleidsregel bestaat niet. |

## <a name="compliance-details-for-guest-configuration"></a>Compatibiliteitsdetails voor de configuratie van de Gast

Voor _controleren_ -beleid in de _Gast configuratie_ categorie, kunnen er meerdere instellingen geëvalueerd binnen de virtuele machine en moet u om details per instelling weer te geven. Bijvoorbeeld, als u een controle voor een lijst met geïnstalleerde toepassingen en de status van de toewijzing is _niet-compatibele_, moet u weten welke specifieke toepassingen ontbreken.

Ook kan er geen toegang tot het rechtstreeks aanmelden bij de virtuele machine, maar u wilt rapporteren over waarom de virtuele machine is _niet-compatibele_. U kunt bijvoorbeeld dat virtuele machines zijn gekoppeld aan het juiste domein en het lidmaatschap van het huidige domein in de details van de reporting opnemen controleren.

### <a name="azure-portal"></a>Azure Portal

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

1. Op de **overzicht** of **naleving** pagina, selecteert u een beleidstoewijzing van een initiatief een definitie van de configuratie van de Gast-beleid bevat dat _niet-compatibele_.

1. Selecteer een _controleren_ beleid in het initiatief dat _niet-compatibele_.

   ![Details van de definitie van audit weergeven](../media/determine-non-compliance/guestconfig-audit-compliance.png)

1. Op de **resourcenaleving** tabblad en de volgende informatie wordt verstrekt:

   - **Naam** -de naam van de Gast-configuratie-toewijzingen.
   - **Bovenliggende Resource** -de virtuele machine in een _niet-compatibele_ staat voor de geselecteerde configuratie van de Gast-toewijzing.
   - **Resourcetype** : de _guestConfigurationAssignments_ volledige naam.
   - **Laatst geëvalueerd** : de laatste keer dat de Gast Configuration-service op de hoogte gesteld Azure Policy over de status van de virtuele doelmachine.

   ![Details van de naleving weergeven](../media/determine-non-compliance/guestconfig-assignment-view.png)

1. Selecteer de naam van de toewijzing van Gast-configuratie in de **naam** kolom openen de **Resourcenaleving** pagina.

1. Selecteer de **Resource weergeven** knop aan de bovenkant van de pagina te openen de **Gast toewijzing** pagina.

De **Gast toewijzing** compatibiliteitsdetails voor alle beschikbare pagina worden weergegeven. Elke rij in de weergave vertegenwoordigt een evaluatieversie die binnen de virtuele machine is uitgevoerd. In de **reden** kolom, een wachtwoordzin met een beschrijving van waarom de Gast-toewijzing is _niet-compatibele_ wordt weergegeven. Bijvoorbeeld, als u wilt controleren, dat virtuele machines moeten worden toegevoegd aan een domein, de **reden** kolom weergegeven tekst met inbegrip van het lidmaatschap van het huidige domein.

![Details van de naleving weergeven](../media/determine-non-compliance/guestconfig-compliance-details.png)

### <a name="azure-powershell"></a>Azure PowerShell

U kunt ook de details van de naleving van Azure PowerShell weergeven. Controleer eerst of dat u de configuratie van de Gast-module is geïnstalleerd.

```azurepowershell-interactive
Install-Module Az.GuestConfiguration
```

U kunt de huidige status van alle toewijzingen van de gast bekijken voor een virtuele machine met de volgende opdracht:

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname>
```

```output
PolicyDisplayName                                                         ComplianceReasons
-----------------                                                         -----------------
Audit that an application is installed inside Windows VMs                 {[InstalledApplication]bwhitelistedapp}
Audit that an application is not installed inside Windows VMs.            {[InstalledApplication]NotInstalledApplica...
```

Om weer te geven alleen de _reden_ woordgroep die wordt beschreven waarom de virtuele machine is _niet-compatibele_, alleen de reden onderliggende eigenschap geretourneerd.

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname> | % ComplianceReasons | % Reasons | % Reason
```

```output
The following applications are not installed: '<name>'.
```

U kunt ook een geschiedenis van naleving van Gast-toewijzingen binnen het bereik van de virtuele machine uitvoeren. De uitvoer van deze opdracht bevat de details van elk rapport voor de virtuele machine.

> [!NOTE]
> De uitvoer kan een grote hoeveelheid gegevens retourneren. Het is raadzaam om op te slaan van de uitvoer in een variabele.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname>
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
[Preview]: Audit that an application is installed inside Windows VMs      NonCompliant                       02/10/2019 12:00:38 PM 02/10/2019 12:00:41 PM VM01  ../17fg0...
<truncated>
```

Gebruik ter vereenvoudiging van deze weergave de **ShowChanged** parameter. De uitvoer van deze opdracht bevat alleen de rapporten die een wijziging in de status van naleving gevolgd.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname> -ShowChanged
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/10/2019 10:00:38 PM 02/10/2019 10:00:41 PM VM01  ../12ab0...
Audit that an application is installed inside Windows VMs.                Compliant                          02/09/2019 11:00:38 AM 02/09/2019 11:00:39 AM VM01  ../e3665...
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/09/2019 09:00:20 AM 02/09/2019 09:00:23 AM VM01  ../15ze1...
```

## <a name="a-namechange-historychange-history-preview"></a><a name="change-history"/>Wijzigingsoverzicht (Preview)

Als onderdeel van een nieuwe **openbare preview**, de afgelopen 14 dagen van de wijzigingsgeschiedenis zijn beschikbaar voor alle Azure-resources die ondersteuning bieden voor [modus verwijdering voltooien](../../../azure-resource-manager/complete-mode-deletion.md). Wijzigingsoverzicht biedt meer informatie over wanneer een wijziging is gedetecteerd en een _visual diff_ voor elke wijziging. De detectie van een wijziging wordt geactiveerd wanneer de Resource Manager-eigenschappen zijn toegevoegd, verwijderd of gewijzigd.

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

1. Op de **overzicht** of **naleving** pagina, selecteert u een beleid in een **nalevingsstatus**.

1. Onder de **resourcenaleving** tabblad van de **beleidsnaleving** pagina, selecteert u een resource.

1. Selecteer de **wijzigingsoverzicht (preview)** tabblad op de **Resourcenaleving** pagina. Een lijst met gedetecteerd wijzigingen, als alle bestaan, worden weergegeven.

   ![Azure Policy wijzigingsoverzicht tabblad op naleving van de Resource-pagina](../media/determine-non-compliance/change-history-tab.png)

1. Selecteer een van de gedetecteerde wijzigingen. De _visual diff_ voor de resource wordt weergegeven op de **wijzigingsoverzicht** pagina.

   ![Azure Policy wijziging geschiedenis Visual Diff op de pagina overzicht wijzigen](../media/determine-non-compliance/change-history-visual-diff.png)

De _visual diff_ aides bij het identificeren van wijzigingen aan een resource. De wijzigingen gedetecteerd kunnen niet worden gerelateerd aan de huidige nalevingsstatus van de resource.

Gegevens van de wijzigingsgeschiedenis wordt geleverd door [Azure Resource Graph](../../resource-graph/overview.md). Als u wilt deze gegevens buiten de Azure-portal een query uitvoert, Zie [wijzigingen van resources ophalen](../../resource-graph/how-to/get-resource-changes.md).

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [voorbeelden voor Azure Policy](../samples/index.md).
- Lees over de [structuur van Azure Policy-definities](../concepts/definition-structure.md).
- Lees [Informatie over de effecten van het beleid](../concepts/effects.md).
- Begrijpen hoe u [programmatisch beleid maken](programmatically-create.md).
- Meer informatie over het [ophalen compatibiliteitsgegevens](getting-compliance-data.md).
- Meer informatie over het [herstellen van niet-compatibele resources](remediate-resources.md).
- Lees wat een beheergroep met is [organiseren van uw resources met Azure-beheergroepen](../../management-groups/overview.md).