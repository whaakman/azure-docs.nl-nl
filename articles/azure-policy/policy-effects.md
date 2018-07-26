---
title: Wat is Azure Policy gevolgen?
description: Azure Policy definition hebben verschillende effecten die bepalen hoe de naleving wordt beheerd en gerapporteerd.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 17ad631e2441e4b8d6314557c17be143fd2f3de0
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248723"
---
# <a name="understanding-policy-effects"></a>Understanding beleid effecten

Elke beleidsdefinitie in Azure Policy is één effect waarmee wordt bepaald wat er gebeurt tijdens het scannen van wanneer de **als** segment van de beleidsregel wordt geëvalueerd, zodat deze overeenkomt met de resource wordt gescand. De gevolgen kunnen ook zich anders gedragen als ze voor een nieuwe resource, een bijgewerkte resource of een bestaande resource zijn.

Er zijn vijf effecten die in de beleidsdefinitie van een worden ondersteund:

- Toevoegen
- Controleren
- AuditIfNotExists
- Weigeren
- DeployIfNotExists (alleen beschikbaar voor **ingebouwde** beleid)

## <a name="order-of-evaluation"></a>Volgorde van de evaluatie

Wanneer een aanvraag maken of bijwerken van een resource via Azure Resource Manager wordt gedaan, wordt beleid verwerkt meerdere van de gevolgen voorafgaand aan de aanvraag toekent aan de juiste Resource Provider.
Zo voorkomt u dat overbodige verwerkingstijd door een Resourceprovider als een resource niet aan de besturingselementen ontworpen governance van beleid voldoet. Beleid maakt een lijst met alle beleidsdefinities toegewezen, door een beleid of een initiatieftoewijzing, die door een bereik (min uitsluitingen) van toepassing op de resource en bereidt het evalueren van de resource op basis van elke definitie.

- **Toevoeg-** wordt eerst geëvalueerd. Aangezien toevoegen kan de aanvraag wijzigen, een wijziging aangebracht door toevoegen mogelijk te voorkomen dat een controle of effect van het triggeren van weigeren.
- **Weigeren** wordt vervolgens geëvalueerd. Weigeren voordat controleren, dubbele logboekregistratie van een resource die ongewenst is voorkomen door te evalueren.
- **Audit** wordt vervolgens geëvalueerd vóór de aanvraag naar de Resource Provider.

Zodra de aanvraag wordt geleverd bij de Resourceprovider en de Resource Provider een code van de status geslaagd retourneert **AuditIfNotExists** en **DeployIfNotExists** worden geëvalueerd om te bepalen of follow-up naleving-logboekregistratie of actie is vereist.

## <a name="append"></a>Toevoegen

Toevoeg-wordt gebruikt voor het toevoegen van extra velden naar de aangevraagde resource tijdens het maken of bijwerken. Kan het nuttig zijn voor het toevoegen van tags op resources zoals costCenter zijn of IP-adressen opgeven voor een opslagresource is toegestaan.

### <a name="append-evaluation"></a>Toevoeg-evaluatie

Zoals gezegd, append evalueert voordat de aanvraag wordt verwerkt door een Resourceprovider tijdens het maken of bijwerken van een resource. Toevoeg-velden toevoegt aan de resource als de **als** wordt voldaan aan de voorwaarde van de beleidsregel. Als het effect append zou een waarde in de oorspronkelijke aanvraag met een andere waarde overschrijft, vervolgens fungeert als een weigeractie en het verzoek afwijzen.

Wanneer de beleidsdefinitie van een met behulp van het effect toevoegen als onderdeel van een evaluatiecyclus van een wordt uitgevoerd, wordt er geen wijzigingen aanbrengen in resources die al bestaan. In plaats daarvan het markeert een resource die voldoet aan de **als** voorwaarde als niet-compatibel.

### <a name="append-properties"></a>Eigenschappen toevoegen

Een toevoeg-effect heeft alleen een **details** matrix, maar dit vereist is. Als **details** is een matrix, duurt het één **veld/waarde** paar of veelvouden. Raadpleeg [beleidsdefinitie](policy-definition.md#fields) voor een lijst van toegestane velden.

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

Voorbeeld 2: Meerdere **veld/waarde** paren toe te voegen een set van labels.

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

Voorbeeld 3: Één **veld/waarde** worden gekoppeld met behulp van een [alias](policy-definition.md#aliases) met een matrix **waarde** IP-regels instellen op een storage-account.

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

Weigeren wordt gebruikt om te voorkomen dat een resource-aanvraag komt niet overeen met de gewenste standaarden via de beleidsdefinitie van een en de aanvraag is mislukt.

### <a name="deny-evaluation"></a>Evaluatie weigeren

Wanneer het maken of bijwerken van een resource, weigeren wordt voorkomen dat de aanvraag voordat deze worden verzonden naar de Resource Provider. De aanvraag wordt geretourneerd als een 403 (verboden). In de portal, kan de verboden worden weergegeven als de status van de implementatie die is geblokkeerd vanwege de beleidstoewijzing.

Tijdens een evaluatiecyclus beleidsdefinities met een weigeractie die overeenkomen met de resources zijn gemarkeerd als niet-compatibel, maar er wordt geen actie uitgevoerd voor die bron.

### <a name="deny-properties"></a>Eigenschappen weigeren

Het effect weigeren heeft geen eventuele aanvullende eigenschappen voor gebruik in de **vervolgens** voorwaarde van de beleidsdefinitie.

### <a name="deny-example"></a>Voorbeeld weigeren

Voorbeeld: Met behulp van het effect weigeren.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Controleren

Audit-effect wordt gebruikt om een waarschuwingsgebeurtenis in het activiteitenlogboek maken wanneer een niet-compatibele resource wordt geëvalueerd, maar het stopt niet de aanvraag.

### <a name="audit-evaluation"></a>Audit-evaluatie

Het effect van de audit is de laatste om uit te voeren tijdens het maken of bijwerken van een resource voorafgaand aan de resource wordt verzonden naar de Resource Provider. Controle werkt op dezelfde manier voor een resourceaanvraag en een evaluatiecyclus van een en wordt uitgevoerd een `Microsoft.Authorization/policies/audit/action` bewerking voor het activiteitenlogboek. In beide gevallen is de resource gemarkeerd als niet-compatibel.

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

AuditIfNotExists kunt controleren voor een resource die overeenkomt met de **als** voorwaarde, maar heeft geen de onderdelen die zijn opgegeven in de **details** van de **vervolgens** voorwaarde.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists-evaluatie

AuditIfNotExists wordt uitgevoerd nadat een Resourceprovider is afgehandeld door een aanvraag maken of bijwerken naar een resource en een code van de status geslaagd heeft geretourneerd. Het effect wordt geactiveerd als er geen verwante bronnen zijn of als de resources die zijn gedefinieerd door **ExistenceCondition** niet geëvalueerd op waar. Wanneer het effect is geactiveerd, een `Microsoft.Authorization/policies/audit/action` bewerking voor het activiteitenlogboek op dezelfde manier als het effect van de controle wordt uitgevoerd. Wanneer ze worden geactiveerd, de resource die voldaan aan de **als** situatie doet zich voor de resource die is gemarkeerd als niet-compatibel.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists eigenschappen

De **details** eigenschap van de gevolgen AuditIfNotExists heeft alle subeigenschappen die de gerelateerde resources zodat deze overeenkomen met definiëren.

- **Type** (vereist)
  - Hiermee geeft u het type van de bijbehorende resource aan.
  - Begint met het ophalen van een resource onder de **als** voorwaarde resource en vervolgens query's in dezelfde resourcegroep bevinden als de **als** resource-voorwaarde.
- **Naam** (optioneel)
  - Hiermee geeft u de exacte naam van de resource waarop en zorgt ervoor dat het beleid voor het ophalen van een bepaalde resource in plaats van alle resources van het opgegeven type.
- **ResourceGroupName** (optioneel)
  - Kan de overeenkomst van de bijbehorende resource afkomstig zijn van een andere resourcegroep.
  - Is niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Standaard is de **als** resourcegroep van de resource-voorwaarde.
- **ExistenceScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het bereik van de locatie voor het ophalen van de bijbehorende resource waarop uit.
  - Is niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Voor _ResourceGroup_, wilt beperken tot de **als** voorwaarde van de resource, resourcegroep of de resourcegroep die is opgegeven in **ResourceGroupName**.
  - Voor _abonnement_, vraagt het hele abonnement voor de bijbehorende resource.
  - De standaardwaarde is _ResourceGroup_.
- **ExistenceCondition** (optioneel)
  - Indien niet opgegeven, alle verwante resources van **type** voldoet aan de kracht en wordt de controle niet geactiveerd.
  - Maakt gebruik van dezelfde taal als de beleidsregel voor de **als** voorwaarde echter geëvalueerd op basis van elke resource gerelateerde afzonderlijk.
  - Als een overeenkomende gerelateerde resource in waar resulteert, wordt het effect is voldaan aan en wordt de controle niet geactiveerd.
  - [Field()] kunt gebruiken om te controleren of gelijkwaardig met waarden in de **als** voorwaarde.
  - Bijvoorbeeld, kan worden gebruikt om te controleren of de bovenliggende resource (in de **als** voorwaarde) is op dezelfde Resourcelocatie als de overeenkomende resource gerelateerde.

### <a name="auditifnotexists-example"></a>Voorbeeld van de AuditIfNotExists

Voorbeeld: Evalueert virtuele Machines om te bepalen of de anti-malware-extensie bestaat voert een controle uit als deze ontbreekt.

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

> [!WARNING]
> DeployIfNotExists is alleen beschikbaar voor **ingebouwde** beleid.

### <a name="deployifnotexists-evaluation"></a>DeployIfNotExists-evaluatie

DeployIfNotExists wordt ook uitgevoerd nadat een Resourceprovider is afgehandeld door een maken of bijwerken vragen tot een resource en een code van de status geslaagd heeft geretourneerd. Het effect wordt geactiveerd als er geen verwante bronnen zijn of als de resources die zijn gedefinieerd door **ExistenceCondition** niet geëvalueerd op waar. Wanneer het effect is geactiveerd, wordt de sjabloonimplementatie van een wordt uitgevoerd.

Tijdens een evaluatiecyclus beleidsdefinities met een DeployIfNotExists-effect die overeenkomen met de resources zijn gemarkeerd als niet-compatibel, maar er wordt geen actie uitgevoerd voor die bron.

### <a name="deployifnotexists-properties"></a>DeployIfNotExists-eigenschappen

De **details** eigenschap van het DeployIfNotExists-effect heeft de subeigenschappen die definieert de gerelateerde resources te vergelijken en de sjabloonimplementatie om uit te voeren.

- **Type** (vereist)
  - Hiermee geeft u het type van de bijbehorende resource aan.
  - Begint met het ophalen van een resource onder de **als** voorwaarde resource en vervolgens query's in dezelfde resourcegroep bevinden als de **als** resource-voorwaarde.
- **Naam** (optioneel)
  - Hiermee geeft u de exacte naam van de resource waarop en zorgt ervoor dat het beleid voor het ophalen van een bepaalde resource in plaats van alle resources van het opgegeven type.
- **ResourceGroupName** (optioneel)
  - Kan de overeenkomst van de bijbehorende resource afkomstig zijn van een andere resourcegroep.
  - Is niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Standaard is de **als** resourcegroep van de resource-voorwaarde.
  - Als de sjabloonimplementatie van een wordt uitgevoerd, wordt deze geïmplementeerd in de resourcegroep van deze waarde.
- **ExistenceScope** (optioneel)
  - Toegestane waarden zijn _abonnement_ en _ResourceGroup_.
  - Hiermee stelt u het bereik van de locatie voor het ophalen van de bijbehorende resource waarop uit.
  - Is niet van toepassing als **type** is een resource die onder de **als** resource-voorwaarde.
  - Voor _ResourceGroup_, wilt beperken tot de **als** voorwaarde van de resource, resourcegroep of de resourcegroep die is opgegeven in **ResourceGroupName**.
  - Voor _abonnement_, vraagt het hele abonnement voor de bijbehorende resource.
  - De standaardwaarde is _ResourceGroup_.
- **ExistenceCondition** (optioneel)
  - Indien niet opgegeven, alle verwante resources van **type** voldoet aan de kracht en de implementatie, wordt niet geactiveerd.
  - Maakt gebruik van dezelfde taal als de beleidsregel voor de **als** voorwaarde echter geëvalueerd op basis van elke resource gerelateerde afzonderlijk.
  - Als een overeenkomende gerelateerde resource in waar resulteert, wordt het effect is voldaan aan en de implementatie, wordt niet geactiveerd.
  - [Field()] kunt gebruiken om te controleren of gelijkwaardig met waarden in de **als** voorwaarde.
  - Bijvoorbeeld, kan worden gebruikt om te controleren of de bovenliggende resource (in de **als** voorwaarde) is op dezelfde Resourcelocatie als de overeenkomende resource gerelateerde.
- **Implementatie** (vereist)
  - Deze eigenschap moet de volledige sjabloonimplementatie bevatten, zoals deze zou worden doorgegeven aan de `Microsoft.Resources/deployments` API plaatsen. Zie voor meer informatie de [implementaties REST-API](/rest/api/resources/deployments).

  > [!NOTE]
  > Alle functies in de **implementatie** eigenschap worden geëvalueerd als onderdelen van de sjabloon, niet van het beleid. De uitzondering hierop is de **parameters** eigenschap waarmee waarden worden doorgegeven van het beleid aan de sjabloon. De **waarde** in deze sectie onder een sjabloon de parameternaam van de wordt gebruikt om uit te voeren van deze waarde wordt doorgegeven (Zie _fullDbName_ in het voorbeeld DeployIfNotExists).

### <a name="deployifnotexists-example"></a>DeployIfNotExists-voorbeeld

Voorbeeld: Evalueert SQL Server-databases om te bepalen of transparentDataEncryption is ingeschakeld. Als dat niet het geval is, klikt u vervolgens een implementatie zodat het wordt uitgevoerd.

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

Een resource wordt mogelijk beïnvloed door meerdere toewijzingen. Deze toewijzingen mogelijk hetzelfde bereik (specifieke resource, resourcegroep, abonnement of beheergroep) of op verschillende niveaus. Elk van deze toewijzingen waarschijnlijk ook een andere effect gedefinieerd hebben. Ongeacht wordt de voorwaarde en de gevolgen voor elk beleid (rechtstreeks of als onderdeel van een initiatief toegewezen) afzonderlijk geëvalueerd. Bijvoorbeeld, als beleid 1 een voorwaarde die de Resourcelocatie voor het abonnement een bevat moet alleen worden gemaakt in 'westus' met de weigeractie beperkt en beleid 2 heeft een voorwaarde waarmee wordt alleen Resourcelocatie voor resourcegroep B beperkt (dat zich in abonnement A) om te worden met de kracht van de audit in 'eastus' gemaakt, zijn beide toegewezen, het uiteindelijke resultaat zou zijn::

- Alle bronnen in resourcegroep B in 'eastus' al is compatibel met beleid 2, maar gemarkeerd als niet-compatibele beleid 1.
- Elke resource al in de resourcegroep B niet in 'eastus' worden gemarkeerd als niet-compatibel beleid 2 en kan ook worden gemarkeerd als niet-compatibel aan beleid 1 als dat niet 'westus'.
- Een nieuwe resource in abonnement A worden niet in 'westus' geweigerd door het beleid 1.
- Een nieuwe resource in abonnement A / resourcegroep B in 'westus' zou worden gemarkeerd als niet-compatibel op beleid 2, maar zou worden gemaakt (compatibel is met het beleid voor 1 en 2 is controleren en mag niet worden geweigerd).

Als zowel beleid 1 en 2 heeft gevolgen voor de situatie's van weigeren, gewijzigd in:

- Elke resource al in de resourcegroep B niet in 'eastus' worden gemarkeerd als niet-compatibel beleid 2.
- Elke resource al in de resourcegroep B niet in 'westus' worden gemarkeerd als niet-compatibel beleid 1.
- Een nieuwe resource in abonnement A worden niet in 'westus' geweigerd door het beleid 1.
- Een nieuwe resource in abonnement A / B voor resourcegroep worden geweigerd (omdat de locatie kan niet voldoen aan zowel beleid 1 en 2).

Omdat elke toewijzing afzonderlijk wordt geëvalueerd, is er geen mogelijkheid voor een resource voor vertraging tot en met een lege ruimte als gevolg van verschillen in het bereik. Daarom het resultaat van het beleid voor lagen of het beleid overlapping wordt beschouwd als **cumulatieve meest beperkende**. Met andere woorden, kan een resource die u wilt dat gemaakt worden geblokkeerd vanwege overlappende en conflicterende beleidsregels zoals in het bovenstaande voorbeeld als zowel beleid 1 en 2 had een weigeractie. Als u de resource moet worden gemaakt in het doelbereik nog steeds nodig hebt, raadpleegt u de uitsluitingen voor elke toewijzing om te controleren of de juiste beleidsregels zijn die betrekking hebben op de juiste bereiken.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een beter begrip van beleid definitie effecten hebt, controleert u de beleid-voorbeelden:

- Bekijk meer voorbeelden op [Voorbeelden van Azure Policy](json-samples.md).
