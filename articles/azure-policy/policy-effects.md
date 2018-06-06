---
title: Wat is Azure beleid effecten?
description: Azure beleidsdefinitie hebben verschillende effecten die bepalen hoe de naleving wordt beheerd en gerapporteerd.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 23bbbe9cf86268f93ae1f8fcec9303efa8a673de
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796713"
---
# <a name="understanding-policy-effects"></a>Informatie over de gevolgen van beleid

Elke beleidsdefinitie in Azure beleid heeft één effect die wat er gebeurt bepaalt tijdens het scannen wanneer de **als** segment van de beleidsregel is geëvalueerd als overeen met de resource wordt gescand. De gevolgen kunnen ook zich anders gedragen als ze voor een nieuwe resource, een bijgewerkte resource of een bestaande resource zijn.

Er zijn vijf effecten die worden ondersteund in een definitie voor:

- Toevoegen
- Controleren
- AuditIfNotExists
- Weigeren
- DeployIfNotExists

## <a name="order-of-evaluation"></a>Volgorde van de evaluatie

Wanneer een aanvraag maken of bijwerken van een resource via Azure Resource Manager wordt gedaan, wordt beleid verwerkt verschillende van de gevolgen voor het overdragen van de aanvraag naar de juiste Resource Provider.
Zo voorkomt u dat onnodige verwerking door een Resourceprovider wanneer een resource niet aan de besturingselementen ontworpen governance van beleid voldoet. Beleid maakt een lijst met alle beleidsdefinities toegewezen door een beleid of het initiatief toewijzing die door een bereik (min uitsluitingen) van toepassing op de bron en bereidt het evalueren van de resource tegen elke definitie.

- **Append** wordt eerst geëvalueerd. Aangezien toevoegen kan de aanvraag wijzigen, een wijziging aangebracht door toevoegen mogelijk te voorkomen dat een controle of effect vanaf activerende weigeren.
- **Weigeren** vervolgens geëvalueerd. Weigeren voordat audit, dubbele het vastleggen van een ongewenste resource wordt voorkomen door te evalueren.
- **Audit** vervolgens geëvalueerd voordat de aanvraag naar de Resourceprovider.

Zodra de aanvraag is opgegeven voor de Resourceprovider en het Resource-Provider een code van de status geslaagd retourneert **AuditIfNotExists** en **DeployIfNotExists** worden geëvalueerd om te bepalen of vervolgzelfstudie logboekregistratie van naleving of actie is vereist.

## <a name="append"></a>Toevoegen

Append extra velden toevoegen aan de aangevraagde bron tijdens het maken of bijwerken wordt gebruikt. Dit kan nuttig zijn voor het toevoegen van labels op resources zoals costCenter zijn of IP-adressen opgeven voor een opslagresource is toegestaan.

### <a name="append-evaluation"></a>Evaluatie toevoegen

Zoals gezegd, toevoegen voordat de aanvraag wordt verwerkt door een Resource Provider tijdens het maken of bijwerken van een resource wordt geëvalueerd. Toevoeg-velden toegevoegd aan de bron wanneer de **als** wordt voldaan aan de voorwaarde van de beleidsregel. Als u het effect append zou een waarde in de oorspronkelijke aanvraag met een andere waarde overschrijven, vervolgens fungeert als een effect weigeren en de aanvraag weigeren.

Wanneer een beleidsdefinitie van een met behulp van het effect append wordt uitgevoerd als onderdeel van een evaluatiecyclus voor installatie, brengt het geen wijzigingen aan resources die al bestaan. In plaats daarvan deze markeert een resource die voldoet aan de **als** voorwaarde als niet-compatibel.

### <a name="append-properties"></a>Eigenschappen toevoegen

Een toevoeg-effect heeft slechts een **details** matrix, maar dit vereist is. Als **details** is een matrix, duurt het één **veld/waarde** paar of veelvouden. Raadpleeg [beleidsdefinitie](policy-definition.md#fields) voor de lijst met toegestane velden.

### <a name="append-examples"></a>Append-voorbeelden

Voorbeeld 1: Één **veld/waarde** paar toevoegen van een label.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Voorbeeld 2: Meerdere **veld/waarde** paren toe te voegen, een reeks labels.

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

Voorbeeld 3: Single **veld/waarde** koppelen met een [alias](policy-definition.md#aliases) met een matrix **waarde** voor het instellen van IP-regels voor een opslagaccount.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

## <a name="deny"></a>Weigeren

Weigeren wordt gebruikt om te voorkomen dat een resource-aanvraag komt niet overeen met de gewenste standaarden via de beleidsdefinitie van een en de aanvraag is mislukt.

### <a name="deny-evaluation"></a>Evaluatie weigeren

Wanneer maken of bijwerken van een resource weigeren voorkomt u dat de aanvraag voordat het wordt verzonden naar de Resourceprovider. De aanvraag wordt geretourneerd als een 403 (verboden). In de portal kan de verboden worden weergegeven als een status voor de implementatie die is geblokkeerd vanwege de beleidstoewijzing.

Tijdens een beoordelingscyclus beleidsdefinities met een effect weigeren die overeenkomen met de resources worden gemarkeerd als niet-compatibel, maar er wordt geen actie uitgevoerd op de bron.

### <a name="deny-properties"></a>Eigenschappen voor weigeren

Het effect weigeren heeft geen eventuele aanvullende eigenschappen voor gebruik in de **vervolgens** voorwaarde van de beleidsdefinitie.

### <a name="deny-example"></a>Voorbeeld weigeren

Voorbeeld: Met behulp van het effect weigeren.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Controleren

Audit effect wordt gebruikt voor het maken van een waarschuwingsgebeurtenis in controlelogboek wanneer een niet-compatibele bron wordt geëvalueerd, maar de aanvraag niet stopt.

### <a name="audit-evaluation"></a>Evaluatie van de audit

Het effect van de audit is de laatste worden uitgevoerd tijdens het maken of bijwerken van een resource voorafgaand aan de resource is verzonden naar de Resourceprovider. Controle werkt hetzelfde voor een resource-aanvraag en een evaluatiecyclus voor installatie en wordt uitgevoerd een `Microsoft.Authorization/policies/audit/action` bewerking is het activiteitenlogboek. De resource is gemarkeerd als niet-compatibel in beide gevallen.

### <a name="audit-properties"></a>Eigenschappen van de audit

Het effect van de audit heeft geen eventuele aanvullende eigenschappen voor gebruik in de **vervolgens** voorwaarde van de beleidsdefinitie.

### <a name="audit-example"></a>Audit-voorbeeld

Voorbeeld: Met behulp van het effect van de audit.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists kunt controle van een resource die overeenkomt met de **als** voorwaarde, maar beschikt niet over de onderdelen die zijn opgegeven in de **details** van de **vervolgens** voorwaarde.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists evaluatie

AuditIfNotExists uitgevoerd nadat een Resourceprovider is afgehandeld door een aanvraag maken of bijwerken naar een resource en een statuscode succes heeft geretourneerd. Het effect wordt geactiveerd als er geen verwante resources zijn of als de bronnen die zijn gedefinieerd door **ExistenceCondition** niet geëvalueerd als waar. Wanneer het effect is geactiveerd, een `Microsoft.Authorization/policies/audit/action` naar het activiteitenlogboek-bewerking wordt uitgevoerd op dezelfde manier als het effect van de audit. Wanneer deze worden geactiveerd, de resource die voldoet aan de **als** voorwaarde de resource die is gemarkeerd als niet-compatibel is.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists eigenschappen

De **details** eigenschap van de gevolgen AuditIfNotExists heeft de subeigenschappen die de bijbehorende resources overeen te definiëren.

- **Type** [vereist]
  - Geeft het type van de bijbehorende resource moet worden gezocht.
  - Hiermee wordt gestart bij het ophalen van een resource onder het de **als** voorwaarde resource en vervolgens query's in dezelfde resourcegroep als de **als** resource-voorwaarde.
- **Naam** (optioneel)
  - Hiermee geeft u de exacte naam van de resource moet worden gezocht en zorgt ervoor dat het beleid voor het ophalen van een specifieke bron, in plaats van alle resources van het opgegeven type.
- **ResourceGroupName** (optioneel)
  - Kan het overeenkomende van de gerelateerde bron afkomstig zijn van een andere resourcegroep.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Standaard is de **als** resourcegroep van de bron-voorwaarde.
- **ExistenceScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het bereik van waar u de gerelateerde bron moet worden gezocht van ophalen.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Voor _ResourceGroup_, zou beperken tot de **als** voorwaarde van de resource, resourcegroep of de resourcegroep die is opgegeven in **ResourceGroupName**.
  - Voor _abonnement_, vraagt het hele abonnement voor de gerelateerde bron.
  - Standaard is _ResourceGroup_.
- **ExistenceCondition** (optioneel)
  - Als u niet opgeeft, worden alle verwante resource van **type** voldoet aan de kracht en wordt de controle niet geactiveerd.
  - Maakt gebruik van dezelfde taal als de beleidsregel voor de **als** voorwaarde, maar wordt geëvalueerd op basis van elke gerelateerde resource afzonderlijk.
  - Als overeenkomende gerelateerde bron in waar resulteert, wordt het effect is voldaan en wordt de controle niet geactiveerd.
  - [Field()] kunt gebruiken om te controleren equivalentie met waarden in de **als** voorwaarde.
  - Bijvoorbeeld, kan worden gebruikt om te valideren dat de bovenliggende resource (in de **als** voorwaarde) bevindt zich in dezelfde Resourcelocatie als de overeenkomende gerelateerde bron.

### <a name="auditifnotexists-example"></a>Voorbeeld van AuditIfNotExists

Voorbeeld: Evalueert virtuele Machines om te bepalen of de Antimalware-uitbreiding bestaat audits als deze ontbreekt.

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

Net als bij AuditIfNotExists, DeployIfNotExists wordt uitgevoerd een sjabloonimplementatie wanneer de voorwaarde wordt voldaan.

### <a name="deployifnotexists-evaluation"></a>DeployIfNotExists evaluatie

DeployIfNotExists wordt ook uitgevoerd nadat een Resourceprovider is afgehandeld door een maken of bijwerken aanvragen aan een resource en een statuscode succes heeft geretourneerd. Het effect wordt geactiveerd als er geen verwante resources zijn of als de bronnen die zijn gedefinieerd door **ExistenceCondition** niet geëvalueerd als waar. Wanneer het effect is geactiveerd, wordt de sjabloonimplementatie van een wordt uitgevoerd.

Tijdens een beoordelingscyclus beleidsdefinities met een effect DeployIfNotExists die overeenkomen met de resources worden gemarkeerd als niet-compatibel, maar er wordt geen actie uitgevoerd op de bron.

### <a name="deployifnotexists-properties"></a>DeployIfNotExists eigenschappen

De **details** eigenschap van de gevolgen DeployIfNotExists heeft de subeigenschappen die de gerelateerde bronnen om overeen te definiëren en de sjabloonimplementatie uit te voeren.

- **Type** [vereist]
  - Geeft het type van de bijbehorende resource moet worden gezocht.
  - Hiermee wordt gestart bij het ophalen van een resource onder het de **als** voorwaarde resource en vervolgens query's in dezelfde resourcegroep als de **als** resource-voorwaarde.
- **Naam** (optioneel)
  - Hiermee geeft u de exacte naam van de resource moet worden gezocht en zorgt ervoor dat het beleid voor het ophalen van een specifieke bron, in plaats van alle resources van het opgegeven type.
- **ResourceGroupName** (optioneel)
  - Kan het overeenkomende van de gerelateerde bron afkomstig zijn van een andere resourcegroep.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Standaard is de **als** resourcegroep van de bron-voorwaarde.
  - Als een sjabloonimplementatie wordt uitgevoerd, wordt geïmplementeerd in de brongroep van deze waarde.
- **ExistenceScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het bereik van waar u de gerelateerde bron moet worden gezocht van ophalen.
  - Niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Voor _ResourceGroup_, zou beperken tot de **als** voorwaarde van de resource, resourcegroep of de resourcegroep die is opgegeven in **ResourceGroupName**.
  - Voor _abonnement_, vraagt het hele abonnement voor de gerelateerde bron.
  - Standaard is _ResourceGroup_.
- **ExistenceCondition** (optioneel)
  - Als u niet opgeeft, worden alle verwante resource van **type** voldoet aan de kracht en wordt de implementatie niet geactiveerd.
  - Maakt gebruik van dezelfde taal als de beleidsregel voor de **als** voorwaarde, maar wordt geëvalueerd op basis van elke gerelateerde resource afzonderlijk.
  - Als een overeenkomende gerelateerde bron in waar resulteert, wordt het effect is voldaan en wordt niet geactiveerd voor de implementatie.
  - [Field()] kunt gebruiken om te controleren equivalentie met waarden in de **als** voorwaarde.
  - Bijvoorbeeld, kan worden gebruikt om te valideren dat de bovenliggende resource (in de **als** voorwaarde) bevindt zich in dezelfde Resourcelocatie als de overeenkomende gerelateerde bron.
- **Implementatie** [vereist]
  - Deze eigenschap moet de implementatie van de volledige sjabloon bevatten als zou worden doorgegeven aan de `Microsoft.Resources/deployments` API plaatsen. Zie voor meer informatie de [implementaties REST-API](/rest/api/resources/deployments).

  > [!NOTE]
  > Alle functies in de **implementatie** eigenschap worden beoordeeld als onderdelen van de sjabloon, niet van het beleid. De uitzondering hierop is de **parameters** eigenschap waarmee waarden uit het beleid worden doorgegeven aan de sjabloon. De **waarde** in deze sectie onder een sjabloon parameternaam wordt gebruikt om deze waarde wordt doorgegeven (Zie _fullDbName_ in het voorbeeld DeployIfNotExists).

### <a name="deployifnotexists-example"></a>Voorbeeld van DeployIfNotExists

Voorbeeld: Evalueert SQL Server-databases om te bepalen of transparentDataEncryption is ingeschakeld. Als dat niet het geval is, klikt u vervolgens een implementatie in te schakelen wordt uitgevoerd.

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

Een resource wordt mogelijk beïnvloed door meerdere toewijzingen. Deze toewijzingen mogelijk op hetzelfde bereik (specifieke resource, resourcegroep, abonnement of beheergroep) of op verschillende bereiken. Elk van deze toewijzingen is waarschijnlijk ook een andere effect gedefinieerd. Ongeacht, wordt de voorwaarde en van kracht voor elk beleid (rechtstreeks of als onderdeel van een initiatief toegewezen) afzonderlijk geëvalueerd. Bijvoorbeeld, als beleid 1 bevat een voorwaarde die de locatie voor een abonnement worden gemaakt in 'westus' met de weigeren kracht en het beleid 2 waarmee bronnen in de resource beperkt groep B (die zich in een abonnement) worden gemaakt in 'eastus' met de controle effect zijn beide toegewezen, is het uiteindelijke resultaat zou:

- Een resource al in de resourcegroep B in 'eastus' is voldoen aan beleid 2, maar gemarkeerd als niet-compatibele aan het beleid 1.
- Een resource al in de resourcegroep B niet in 'eastus' worden gemarkeerd als niet-compatibel beleid 2 en zou ook worden gemarkeerd als niet-compatibel op beleid 1 als dat niet 'westus'.
- Een nieuwe resource in een abonnement anders niet in 'westus' worden geweigerd door het beleid voor 1.
- Een nieuwe resource in een abonnement / resourcegroep B in 'westus' zou worden gemarkeerd als niet-compatibel op beleid 2, maar zou worden gemaakt (voldoen aan beleid voor 1 en 2 is controleren en niet weigeren).

Als zowel beleid 1 en 2 had invloed van weigeren, de situatie wilt wijzigen in:

- Een resource al in de resourcegroep B niet in 'eastus' worden gemarkeerd als niet-compatibel beleid 2.
- Een resource al in de resourcegroep B niet in 'westus' worden gemarkeerd als niet-compatibel beleid 1.
- Een nieuwe resource in een abonnement anders niet in 'westus' worden geweigerd door het beleid voor 1.
- Een nieuwe resource in een abonnement / resourcegroep B anders worden geweigerd (omdat de locatie nooit, zowel beleid 1 en 2 voldoen kan).

Omdat elke toewijzing wordt afzonderlijk geëvalueerd, is er geen gelegenheid voor een bron voor de vertraging via een gap vanwege verschillen in het bereik. Daarom het resultaat van gelaagdheid beleid of beleid overlapping wordt beschouwd als **cumulatieve meest beperkende**. Met andere woorden, kan een bron die u wilt dat gemaakt als gevolg van overlappende en conflicterende beleidsregels, zoals het bovenstaande voorbeeld worden geblokkeerd als zowel beleid 1 en 2 had een effect weigeren. Als u de bron moet worden gemaakt in het doelbereik nog nodig hebt, raadpleegt u de uitsluitingen op elke toewijzing om te controleren of het juiste beleid invloed zijn op de juiste bereiken.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een beter begrip van beleid definitie gevolgen hebt, controleert u de beleid-voorbeelden:

- Bekijk meer voorbeelden op [Azure beleid voorbeelden](json-samples.md).
