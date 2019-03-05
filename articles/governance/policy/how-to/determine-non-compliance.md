---
title: Oorzaken van niet-naleving bepalen
description: Wanneer een resource niet-compatibele is, zijn er veel mogelijke oorzaken. Leer hoe u meer informatie over de oorzaak van de niet-naleving.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: dba8d9413229a0fa236b082e2e11dbd1a9fe5a5f
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314123"
---
# <a name="determine-causes-of-non-compliance"></a>Oorzaken van niet-naleving bepalen

Wanneer een Azure-resource blijkt dat het niet-compatibel is op een beleidsregel, is het handig om te begrijpen welk deel van de regel voor de resource is niet compatibel zijn met. Het is ook handig om te begrijpen welke wijziging gewijzigd een voorheen compliant bron uit voor niet-compatibel te maken. Er zijn twee manieren om deze informatie te vinden:

> [!div class="checklist"]
> - [Details van compatibiliteit](#compliance-details)
> - [Wijzigingsoverzicht (Preview)](#change-history-preview)

## <a name="compliance-details"></a>Nalevingsdetails

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

   ![Deelvenster met details naleving](../media/determine-non-compliance/compliance-details-pane.png)

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

|Reden | Voorwaarde |
|-|-|
|Huidige waarde moet de doelwaarde bevatten als sleutel. |containsKey of **niet** notContainsKey |
|Huidige waarde moet de doelwaarde bevatten. |bevat of **niet** notContains |
|Huidige waarde moet gelijk zijn aan de doelwaarde. |is gelijk aan of **niet** notEquals |
|Huidige waarde moet bestaan. |Er bestaat |
|De huidige waarde moet binnen de doelwaarde vallen. |in of **niet** notIn |
|Huidige waarde moet gelijk zijn aan de doelwaarde. |Net als of **niet** notlike zijn |
|Huidige waarde moet hoofdlettergevoelige overeenkomst de doelwaarde. |overeenkomen met of **niet** notMatch |
|Huidige waarde moet niet-hoofdlettergevoelige overeenkomst de doelwaarde. |matchInsensitively of **niet** notMatchInsensitively |
|Huidige waarde mag de doelwaarde niet als sleutel bevatten. |notContainsKey of **niet** containsKey|
|Huidige waarde mag de doelwaarde niet bevatten. |notContains of **niet** bevat |
|Huidige waarde mag niet gelijk zijn aan de doelwaarde. |notEquals of **niet** is gelijk aan |
|Huidige waarde mag niet bestaan. |**niet** bestaat  |
|Huidige waarde mag niet binnen de doelwaarde vallen. |notIn of **niet** in |
|Huidige waarde mag niet gelijk zijn aan de doelwaarde. |notlike zijn of **niet** zoals |
|Huidige waarde moet niet-hoofdlettergevoelige overeenkomst de doelwaarde. |notMatch of **niet** overeen met |
|Huidige waarde moet geen niet-hoofdlettergevoelige overeenkomst de doelwaarde. |notMatchInsensitively of **niet** matchInsensitively |
|Er zijn geen verwante resources die overeenkomen met de effectdetails in de beleidsdefinitie. |Een resource van het type dat is gedefinieerd in **then.details.type** en gerelateerd zijn aan de resource die is gedefinieerd in de **als** gedeelte van de beleidsregel bestaat niet. |

## <a name="change-history-preview"></a>Wijzigingsoverzicht (Preview)

Als onderdeel van een nieuwe **openbare preview**, de afgelopen 14 dagen van de wijziging geschiedenis is beschikbaar voor alle Azure-resources die ondersteuning bieden voor [modus verwijdering voltooien](../../../azure-resource-manager/complete-mode-deletion.md). Wijzigingsoverzicht biedt meer informatie over wanneer een wijziging is gedetecteerd en een _visual diff_ voor elke wijziging. De detectie van een wijziging wordt geactiveerd wanneer de Resource Manager-eigenschappen zijn toegevoegd, verwijderd of gewijzigd.

1. Start de Azure Policy-service in Azure Portal door **Alle services** te selecteren en dan **Beleid** te zoeken en te selecteren.

1. Op de **overzicht** of **naleving** pagina, selecteert u een beleid in een **nalevingsstatus**.

1. Onder de **resourcenaleving** tabblad van de **beleidsnaleving** pagina, selecteert u een resource.

1. Selecteer de **wijzigingsoverzicht (preview)** tabblad op de **Resourcenaleving** pagina. Een lijst met gedetecteerd wijzigingen, als alle bestaan, worden weergegeven.

   ![Beleid wijzigingsoverzicht - tabblad](../media/determine-non-compliance/change-history-tab.png)

1. Selecteer een van de gedetecteerde wijzigingen. De _visual diff_ voor de resource wordt weergegeven op de **wijzigingsoverzicht** pagina.

   ![Beleid wijzigingsoverzicht - Visual Diff](../media/determine-non-compliance/change-history-visual-diff.png)

De _visual diff_ aides bij het identificeren van wijzigingen aan een resource. De wijzigingen gedetecteerd kunnen niet worden gerelateerd aan de huidige nalevingsstatus van de resource.

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](../concepts/definition-structure.md)
- Beoordeling [effecten beleid begrijpen](../concepts/effects.md)
- Begrijpen hoe u [programmatisch beleid maken](programmatically-create.md)
- Meer informatie over het [Nalevingsgegevens ophalen](getting-compliance-data.md)
- Meer informatie over het [herstellen van niet-compatibele resources](remediate-resources.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)