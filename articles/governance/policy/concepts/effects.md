---
title: Informatie over de werking van effecten
description: Azure Policy definition hebben verschillende effecten die bepalen hoe de naleving wordt beheerd en gerapporteerd.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: c84af250a9e8dbff578f58abc7e3558d95ecbe93
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904536"
---
# <a name="understand-policy-effects"></a>Inzicht in de effecten van het beleid

Elke beleidsdefinitie in Azure Policy is één effect. Dat effect bepaalt wat er gebeurt wanneer de beleidsregel zodat deze overeenkomen met wordt geëvalueerd. De effecten zich anders gedragen als ze voor een nieuwe resource, een bijgewerkte resource of een bestaande resource zijn.

Er zijn momenteel zes effecten die in de beleidsdefinitie van een worden ondersteund:

- Toevoegen
- Controleren
- AuditIfNotExists
- Weigeren
- DeployIfNotExists
- Uitgeschakeld

## <a name="order-of-evaluation"></a>Volgorde van de evaluatie

Aanvragen voor het maken of bijwerken van een resource via Azure Resource Manager worden eerst door het beleid geëvalueerd. Beleid maakt een lijst van alle toewijzingen die gelden voor de resource en evalueert vervolgens de resource op basis van elke definitie. Groepsbeleid verwerkt meerdere van de gevolgen voordat de aanvraag naar de juiste Resource Provider. Zo voorkomt u dat overbodige verwerkingstijd door een Resourceprovider als een resource niet voldoet aan de besturingselementen ontworpen governance van beleid.

- **Uitgeschakelde** wordt eerst gecontroleerd om te bepalen als de beleidsregel moet worden geëvalueerd.
- **Toevoeg-** wordt vervolgens geëvalueerd. Aangezien toevoegen kan de aanvraag wijzigen, een wijziging aangebracht door toevoegen mogelijk te voorkomen dat een controle of effect van het triggeren van weigeren.
- **Weigeren** wordt vervolgens geëvalueerd. Weigeren voordat controleren, dubbele logboekregistratie van een resource die ongewenst is voorkomen door te evalueren.
- **Audit** wordt vervolgens geëvalueerd vóór de aanvraag naar de Resource Provider.

Nadat de Resource Provider een succescode, **AuditIfNotExists** en **DeployIfNotExists** evalueren om te bepalen of extra naleving logboekregistratie of actie vereist is.

## <a name="disabled"></a>Uitgeschakeld

Dit effect is handig voor het testen van situaties of voor wanneer het effect heeft parameters in de beleidsdefinitie. Deze flexibiliteit maakt het mogelijk om uit te schakelen van de toewijzing van een enkel in plaats van alle toewijzingen van dat beleid uit te schakelen.

## <a name="append"></a>Toevoegen

Toevoeg-wordt gebruikt voor het toevoegen van extra velden naar de aangevraagde resource tijdens het maken of bijwerken. Een veelvoorkomend voorbeeld is het toevoegen van tags op resources zoals costCenter of IP-adressen opgeven voor een opslagresource is toegestaan.

### <a name="append-evaluation"></a>Toevoeg-evaluatie

Toevoeg-evalueert voordat de aanvraag wordt verwerkt door een Resourceprovider tijdens het maken of bijwerken van een resource. Toevoeg-velden toevoegt aan de resource als de **als** wordt voldaan aan de voorwaarde van de beleidsregel. Als het effect append een waarde in de oorspronkelijke aanvraag met een andere waarde overschrijven zou, klikt u vervolgens deze fungeert als een weigeractie en weigert de aanvraag.

Wanneer de beleidsdefinitie van een met behulp van het effect toevoegen wordt uitgevoerd als onderdeel van een evaluatiecyclus van een, aanbrengen niet het wijzigingen in resources die al bestaan. In plaats daarvan het markeert een resource die voldoet aan de **als** voorwaarde als niet-compatibel.

### <a name="append-properties"></a>Eigenschappen toevoegen

Een toevoeg-effect heeft alleen een **details** matrix, maar dit vereist is. Als **details** is een matrix, duurt het één **veld/waarde** paar of veelvouden. Raadpleeg [structuur van beleidsdefinities](definition-structure.md#fields) voor een lijst van toegestane velden.

### <a name="append-examples"></a>Append-voorbeelden

Voorbeeld 1: Één **veld/waarde** paar toe te voegen een tag.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Voorbeeld 2: Twee **veld/waarde** paren toe te voegen een set van labels.

```json
"then": {
    "effect": "append",
    "details": [{
            "field": "tags.myTag",
            "value": "myTagValue"
        },
        {
            "field": "tags.myOtherTag",
            "value": "myOtherTagValue"
        }
    ]
}
```

Voorbeeld 3: Één **veld/waarde** worden gekoppeld met behulp van een [alias](definition-structure.md#aliases) met een matrix **waarde** IP-regels instellen op een storage-account.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

## <a name="deny"></a>Weigeren

Weigeren wordt gebruikt om te voorkomen dat een resource-aanvraag komt niet overeen met de gedefinieerde standaarden via de beleidsdefinitie van een en de aanvraag is mislukt.

### <a name="deny-evaluation"></a>Evaluatie weigeren

Wanneer het maken of bijwerken van een overeenkomende resource weigeren, wordt voorkomen dat de aanvraag voordat het wordt verzonden naar de Resource Provider. De aanvraag wordt geretourneerd als een `403 (Forbidden)`. In de portal, kan de verboden worden weergegeven als de status van de implementatie die is verhinderd doordat de beleidstoewijzing.

Tijdens de evaluatie van bestaande resources, resources die overeenkomen met een beleidsdefinitie weigeren gemarkeerd als niet-compatibel.

### <a name="deny-properties"></a>Eigenschappen weigeren

Het effect weigeren geen eventuele aanvullende eigenschappen voor gebruik in de **vervolgens** voorwaarde van de beleidsdefinitie.

### <a name="deny-example"></a>Voorbeeld weigeren

Voorbeeld: Met behulp van het effect weigeren.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Controleren

Controle wordt gebruikt om u te maken van een waarschuwingsgebeurtenis in het activiteitenlogboek bij het evalueren van een niet-compatibele resource, maar deze de aanvraag niet stoppen.

### <a name="audit-evaluation"></a>Audit-evaluatie

Controle is het laatste effect is door het beleid wordt ingeschakeld tijdens het maken of bijwerken van een resource. Beleid verzendt vervolgens de resource bij de Resourceprovider. Controle werkt hetzelfde voor een resource-aanvraag en een evaluatiecyclus van een. Beleid wordt toegevoegd een `Microsoft.Authorization/policies/audit/action` bewerking voor het activiteitenlogboek en de resource als niet-compatibel worden gemarkeerd.

### <a name="audit-properties"></a>Audit-eigenschappen

Het effect van de audit heeft geen eventuele aanvullende eigenschappen voor gebruik in de **vervolgens** voorwaarde van de beleidsdefinitie.

### <a name="audit-example"></a>Audit-voorbeeld

Voorbeeld: Met behulp van het effect van de audit.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists kunt controleren voor bronnen die voldoen aan de **als** voorwaarde, maar beschikt niet over de onderdelen die zijn opgegeven in de **details** van de **vervolgens** voorwaarde.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists-evaluatie

AuditIfNotExists wordt uitgevoerd nadat een Resourceprovider is afgehandeld door een resourceaanvraag maken of bijwerken en een code van de status geslaagd heeft geretourneerd. De controle vindt plaats als er geen verwante bronnen zijn of als de resources die zijn gedefinieerd door **ExistenceCondition** niet resulteren in waar. Beleid wordt toegevoegd een `Microsoft.Authorization/policies/audit/action` bewerking naar de activiteit Meld u aan de dezelfde manier als het effect van de audit. Wanneer ze worden geactiveerd, de resource die voldaan aan de **als** situatie doet zich voor de resource die is gemarkeerd als niet-compatibel.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists eigenschappen

De **details** eigenschap van de gevolgen AuditIfNotExists heeft alle subeigenschappen die de gerelateerde resources zodat deze overeenkomen met definiëren.

- **Type** (vereist)
  - Hiermee geeft u het type van de bijbehorende resource aan.
  - Begint met het ophalen van een resource onder de **als** voorwaarde resource en vervolgens query's in dezelfde resourcegroep bevinden als de **als** resource-voorwaarde.
- **Naam** (optioneel)
  - Hiermee geeft u de exacte naam van de resource waarop en zorgt ervoor dat het beleid voor het ophalen van een bepaalde resource in plaats van alle resources van het opgegeven type.
- **ResourceGroupName** (optioneel)
  - Kan de overeenkomst van de bijbehorende resource afkomstig zijn van een andere resourcegroep.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Standaard is de **als** resourcegroep van de resource-voorwaarde.
- **ExistenceScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het bereik van de locatie voor het ophalen van de bijbehorende resource waarop uit.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Voor _ResourceGroup_, wilt beperken tot de **als** voorwaarde van de resource, resourcegroep of de resourcegroep die is opgegeven in **ResourceGroupName**.
  - Voor _abonnement_, vraagt het hele abonnement voor de bijbehorende resource.
  - De standaardwaarde is _ResourceGroup_.
- **ExistenceCondition** (optioneel)
  - Indien niet opgegeven, alle verwante resources van **type** voldoet aan de kracht en de controle niet activeren.
  - Maakt gebruik van dezelfde taal als de beleidsregel voor de **als** voorwaarde echter geëvalueerd op basis van elke resource gerelateerde afzonderlijk.
  - Als een overeenkomende gerelateerde resource in waar resulteert, wordt het effect is voldaan aan en de controle niet activeren.
  - [Field()] kunt gebruiken om te controleren of gelijkwaardig met waarden in de **als** voorwaarde.
  - Bijvoorbeeld, kan worden gebruikt om te controleren of de bovenliggende resource (in de **als** voorwaarde) is op dezelfde Resourcelocatie als de overeenkomende resource gerelateerde.

### <a name="auditifnotexists-example"></a>Voorbeeld van de AuditIfNotExists

Voorbeeld: Evalueert de virtuele Machines om te bepalen of de anti-malware-extensie bestaat voert een controle uit als deze ontbreekt.

```json
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "auditIfNotExists",
        "details": {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "existenceCondition": {
                "allOf": [{
                        "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                        "equals": "Microsoft.Azure.Security"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/extensions/type",
                        "equals": "IaaSAntimalware"
                    }
                ]
            }
        }
    }
}
```

## <a name="deployifnotexists"></a>DeployIfNotExists

Net als bij AuditIfNotExists, DeployIfNotExists voert een sjabloonimplementatie wanneer de voorwaarde wordt voldaan.

> [!NOTE]
> [Geneste sjablonen](../../../azure-resource-manager/resource-group-linked-templates.md#nested-template) worden ondersteund met **deployIfNotExists**, maar [gekoppelde sjablonen](../../../azure-resource-manager/resource-group-linked-templates.md) worden momenteel niet ondersteund.

### <a name="deployifnotexists-evaluation"></a>DeployIfNotExists-evaluatie

DeployIfNotExists uitgevoerd nadat een Resourceprovider is afgehandeld door een resourceaanvraag maken of bijwerken en een code van de status geslaagd heeft geretourneerd. De sjabloonimplementatie van een treedt op als er geen verwante bronnen zijn of als de resources die zijn gedefinieerd door **ExistenceCondition** niet resulteren in waar.

Tijdens een evaluatiecyclus beleidsdefinities met een DeployIfNotExists-effect die overeenkomen met de resources zijn gemarkeerd als niet-compatibel, maar er is geen actie ondernomen voor die bron.

### <a name="deployifnotexists-properties"></a>DeployIfNotExists-eigenschappen

De **details** eigenschap van het DeployIfNotExists-effect heeft de subeigenschappen die definieert de gerelateerde resources te vergelijken en de sjabloonimplementatie om uit te voeren.

- **Type** (vereist)
  - Hiermee geeft u het type van de bijbehorende resource aan.
  - Begint met het ophalen van een resource onder de **als** voorwaarde resource en vervolgens query's in dezelfde resourcegroep bevinden als de **als** resource-voorwaarde.
- **Naam** (optioneel)
  - Hiermee geeft u de exacte naam van de resource waarop en zorgt ervoor dat het beleid voor het ophalen van een bepaalde resource in plaats van alle resources van het opgegeven type.
- **ResourceGroupName** (optioneel)
  - Kan de overeenkomst van de bijbehorende resource afkomstig zijn van een andere resourcegroep.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Standaard is de **als** resourcegroep van de resource-voorwaarde.
  - Als de sjabloonimplementatie van een wordt uitgevoerd, wordt deze geïmplementeerd in de resourcegroep van deze waarde.
- **ExistenceScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het bereik van de locatie voor het ophalen van de bijbehorende resource waarop uit.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Voor _ResourceGroup_, wilt beperken tot de **als** voorwaarde van de resource, resourcegroep of de resourcegroep die is opgegeven in **ResourceGroupName**.
  - Voor _abonnement_, vraagt het hele abonnement voor de bijbehorende resource.
  - De standaardwaarde is _ResourceGroup_.
- **ExistenceCondition** (optioneel)
  - Indien niet opgegeven, alle verwante resources van **type** voldoet aan de kracht en de implementatie niet worden geactiveerd.
  - Maakt gebruik van dezelfde taal als de beleidsregel voor de **als** voorwaarde echter geëvalueerd op basis van elke resource gerelateerde afzonderlijk.
  - Als een overeenkomende gerelateerde resource in waar resulteert, wordt het effect is voldaan aan en de implementatie niet worden geactiveerd.
  - [Field()] kunt gebruiken om te controleren of gelijkwaardig met waarden in de **als** voorwaarde.
  - Bijvoorbeeld, kan worden gebruikt om te controleren of de bovenliggende resource (in de **als** voorwaarde) is op dezelfde Resourcelocatie als de overeenkomende resource gerelateerde.
- **roleDefinitionIds** (vereist)
  - Deze eigenschap moet een matrix met tekenreeksen die overeenkomen met toegankelijk is op basis van de rol beheer rol-ID van het abonnement zijn. Zie voor meer informatie, [herstel - beleidsdefinitie configureren](../how-to/remediate-resources.md#configure-policy-definition).
- **DeploymentScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het type van de implementatie die moet worden uitgevoerd. _Abonnement_ geeft aan dat een [implementatie op abonnementsniveau]((../../../azure-resource-manager/deploy-to-subscription)), _ResourceGroup_ geeft aan dat een implementatie naar een resourcegroep.
  - Een _locatie_ eigenschap moet worden opgegeven in de _implementatie_ bij het gebruik van abonnement niveau implementaties.
  - De standaardwaarde is _ResourceGroup_.
- **Implementatie** (vereist)
  - Deze eigenschap moet de volledige sjabloonimplementatie bevatten, zoals deze zou worden doorgegeven aan de `Microsoft.Resources/deployments` API plaatsen. Zie voor meer informatie de [implementaties REST-API](/rest/api/resources/deployments).

  > [!NOTE]
  > Alle functies in de **implementatie** eigenschap worden geëvalueerd als onderdelen van de sjabloon, niet van het beleid. De uitzondering hierop is de **parameters** eigenschap waarmee waarden worden doorgegeven van het beleid aan de sjabloon. De **waarde** in deze sectie onder een sjabloon de parameternaam van de wordt gebruikt om uit te voeren van deze waarde wordt doorgegeven (Zie _fullDbName_ in het voorbeeld DeployIfNotExists).

### <a name="deployifnotexists-example"></a>DeployIfNotExists-voorbeeld

Voorbeeld: Evalueert de SQL Server-databases om te bepalen of transparentDataEncryption is ingeschakeld. Als dat niet het geval is, klikt u vervolgens een implementatie om in te schakelen wordt uitgevoerd.

```json
"if": {
    "field": "type",
    "equals": "Microsoft.Sql/servers/databases"
},
"then": {
    "effect": "DeployIfNotExists",
    "details": {
        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
        "name": "current",
        "roleDefinitionIds": [
            "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
            "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
        ],
        "existenceCondition": {
            "field": "Microsoft.Sql/transparentDataEncryption.status",
            "equals": "Enabled"
        },
        "deployment": {
            "properties": {
                "mode": "incremental",
                "template": {
                    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "fullDbName": {
                            "type": "string"
                        }
                    },
                    "resources": [{
                        "name": "[concat(parameters('fullDbName'), '/current')]",
                        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
                        "apiVersion": "2014-04-01",
                        "properties": {
                            "status": "Enabled"
                        }
                    }]
                },
                "parameters": {
                    "fullDbName": {
                        "value": "[field('fullName')]"
                    }
                }
            }
        }
    }
}
```

## <a name="layering-policies"></a>Beleid voor lagen

Een resource wordt mogelijk beïnvloed door meerdere toewijzingen. Deze toewijzingen mogelijk hetzelfde bereik of op verschillende niveaus. Elk van deze toewijzingen waarschijnlijk ook een andere effect gedefinieerd hebben. De voorwaarde en de gevolgen voor elk beleid wordt afzonderlijk geëvalueerd. Bijvoorbeeld:

- Beleid 1
  - Hiermee beperkt u Resourcelocatie naar 'westus'
  - Toegewezen aan een abonnement
  - Effect weigeren
- Beleid 2
  - Hiermee beperkt u Resourcelocatie 'eastus'
  - Toegewezen aan de resourcegroep B in een abonnement
  - Audit-effect
  
Deze installatie zou leiden tot het volgende resultaat:

- Alle bronnen in resourcegroep B in 'eastus' al is compatibel met beleid 2 en niet-compatibele beleid 1
- Elke resource al in de resourcegroep B niet in 'eastus' is niet-compatibele beleid 2 en niet-compatibele beleid 1 als dat niet in 'westus'
- Een nieuwe resource in abonnement A niet in 'westus' wordt geweigerd door beleid 1
- Een nieuwe resource in abonnement A en B-resourcegroep in 'westus' wordt gemaakt en niet-compatibel is op beleid 2

Als heeft gevolgen voor zowel beleid 1 en 2 van weigeren, wordt de situatie gewijzigd in:

- Elke resource al in de resourcegroep B niet in 'eastus' is niet-compatibele beleid 2
- Elke resource al in de resourcegroep B niet in 'westus' is niet-compatibele beleid 1
- Een nieuwe resource in abonnement A niet in 'westus' wordt geweigerd door beleid 1
- Een nieuwe resource in de resourcegroep B voor abonnement die a is geweigerd

Elke toewijzing afzonderlijk geëvalueerd. Daarom is er geen een kans op een resource voor vertraging via een onderbreking van de verschillen in het bereik. Het resultaat van het beleid voor lagen of het beleid overlapping wordt beschouwd als **cumulatieve meest beperkende**. Een voorbeeld: als beide beleid 1 en 2 had een weigeractie een bron zou worden geblokkeerd door de overlappende en conflicterende beleidsregels. Als u nog steeds de resource moet worden gemaakt in de doel-scope, Controleer de uitsluitingen voor elke toewijzing voor het valideren van de juiste beleidsregels van invloed zijn op de juiste bereiken.

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](definition-structure.md)
- Begrijpen hoe u [programmatisch beleid maken](../how-to/programmatically-create.md)
- Meer informatie over het [Nalevingsgegevens ophalen](../how-to/getting-compliance-data.md)
- Meer informatie over het [herstellen van niet-compatibele resources](../how-to/remediate-resources.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)
