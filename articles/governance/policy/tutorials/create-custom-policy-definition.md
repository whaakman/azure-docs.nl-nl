---
title: Een aangepaste beleidsdefinitie maken
description: Maak een aangepast beleid voor Azure Policy om aangepaste bedrijfsregels af te dwingen.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: e38eb1315cde3400b70925059d4dd50475a47835
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979671"
---
# <a name="tutorial-create-a-custom-policy-definition"></a>Zelfstudie: Een aangepaste beleidsdefinitie maken

Met een aangepaste beleidsdefinitie kunnen klanten hun eigen regels voor het gebruik van Azure definiëren. Deze regels worden vaak gebruikt voor het afdwingen van:

- Beveiligingsmethoden
- Kostenbeheer
- Organisatie-specifieke regels (zoals namen of locaties)

Welke redenen een bedrijf ook heeft voor het maken van een aangepast beleid, de stappen voor het definiëren ervan zijn hetzelfde.

Bekijk voordat u een aangepast beleid maakt, de [beleidsvoorbeelden](../samples/index.md) om te zien of er al een beleid bestaat dat aan uw behoeften voldoet.

De aanpak voor het maken van een aangepast beleid bevat de volgende stappen:

> [!div class="checklist"]
> - Identificeer de vereisten van uw bedrijf
> - Wijs elke vereiste toe aan een Azure-resource-eigenschap
> - Wijs de eigenschap toe aan een alias
> - Bepaal welk effect moet worden gebruikt
> - Stel de beleidsdefinitie op

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="identify-requirements"></a>Vereisten identificeren

Voordat u de beleidsdefinitie gaat maken, is het belangrijk te weten wat de intentie van het beleid is. Voor deze zelfstudie gebruiken we een veelvoorkomende beveiligingsvereiste voor bedrijven als doel om de stappen te illustreren:

- Voor elk opslagaccount moet HTTPS zijn ingeschakeld
- Voor elk opslagaccount moet HTTP zijn uitgeschakeld

In uw vereisten moet zowel de gewenste als de ongewenste situatie duidelijk worden geïdentificeerd.

Hoewel we de verwachte status van de resource hebben gedefinieerd, hebben we nog niet gedefinieerd wat we willen doen met niet-compatibele resources. Azure Policy ondersteunt een aantal [effecten](../concepts/effects.md). Voor deze zelfstudie definiëren we de vereiste dat het maken van resources moet worden verhinderd als deze niet voldoen aan de bedrijfsregels. Om dit doel te bereiken, gebruiken we het effect [Deny](../concepts/effects.md#deny) (weigeren). We willen ook de optie hebben om het beleid te onderbreken voor speciale toewijzingen. Hiervoor gebruiken we het effect [Uitgeschakeld](../concepts/effects.md#disabled), waarvan we een [parameter](../concepts/definition-structure.md#parameters) maken in de beleidsdefinitie.

## <a name="determine-resource-properties"></a>Resource-eigenschappen bepalen

Op basis van de vereiste van het bedrijf, is de Azure-resource om te controleren met Azure Policy een opslagaccount. Maar we weten nog niet welke eigenschappen in de beleidsdefinitie moeten worden gebruikt. Azure Policy wordt geëvalueerd op basis van de JSON-weergave van de resource, zodat we moeten begrijpen welke eigenschappen beschikbaar zijn voor die bron.

Er zijn veel manieren om de eigenschappen van een Azure-resource te bepalen. In deze zelfstudie bekijken we elk daarvan:

- Resource Manager-sjablonen
  - Bestaande resource exporteren
  - Een resource maken
  - Quickstart sjablonen (GitHub)
  - Sjabloonreferentiedocumenten
- Azure Resource Explorer

### <a name="resource-manager-templates"></a>Resource Manager-sjablonen

Er zijn verschillende manieren om te kijken naar een [Resource Manager-sjabloon](../../../azure-resource-manager/resource-manager-tutorial-create-encrypted-storage-accounts.md) die de eigenschap bevat die u wilt beheren.

#### <a name="existing-resource-in-the-portal"></a>Bestaande resource in de portal

De eenvoudigste manier om eigenschappen te vinden is een bestaande resource van hetzelfde type te bekijken. Resources die al zijn geconfigureerd met de instelling die u wilt afdwingen, bevatten ook de waarde waarmee moet worden vergeleken.
Bekijk de **sjabloon exporteren** pagina (onder **instellingen**) in de Azure-portal voor die specifieke resource.

![Sjabloonpagina op bestaande resources exporteren](../media/create-custom-policy-definition/export-template.png)

Wanneer u dit doet voor een opslagaccount, ziet u een sjabloon die lijkt op het volgende voorbeeld:

```json
...
"resources": [{
    "comments": "Generalized from resource: '/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount'.",
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "name": "[parameters('storageAccounts_mystorageaccount_name')]",
    "apiVersion": "2018-07-01",
    "location": "westus",
    "tags": {
        "ms-resource-usage": "azure-cloud-shell"
    },
    "scale": null,
    "properties": {
        "networkAcls": {
            "bypass": "AzureServices",
            "virtualNetworkRules": [],
            "ipRules": [],
            "defaultAction": "Allow"
        },
        "supportsHttpsTrafficOnly": false,
        "encryption": {
            "services": {
                "file": {
                    "enabled": true
                },
                "blob": {
                    "enabled": true
                }
            },
            "keySource": "Microsoft.Storage"
        }
    },
    "dependsOn": []
}]
...
```

Onder **properties** is een waarde met de naam **supportsHttpsTrafficOnly** ingesteld op **false**. Het lijkt erop dat deze eigenschap degene kan zijn die we zoeken. Het **type** van de resource is **Microsoft.Storage/storageAccounts**. Met het type kunnen we het beleid beperken tot alleen resources van dit type.

#### <a name="create-a-resource-in-the-portal"></a>Een resource maken in de portal

Een andere manier via de portal is het maken van een resource. Bij het maken van een opslagaccount via de portal is ziet u op het tabblad **Geavanceerd** de optie **Veilige overdracht vereist**. Deze eigenschap heeft de opties _Uitgeschakeld_ en _Ingeschakeld_. Het informatiepictogram biedt aanvullende tekst die bevestigt dat deze optie waarschijnlijk de eigenschap is die we zoeken. In dit scherm kunnen we echter niet zien wat de naam van de eigenschap is.

Op het tabblad **Beoordelen en maken** bevindt zich onderaan de pagina de koppeling **Een sjabloon voor Automation downloaden**. Wanneer u deze koppeling selecteert, wordt de sjabloon geopend voor het maken van de resource die we hebben geconfigureerd. In dit geval zien we twee belangrijke gegevens:

```json
...
"supportsHttpsTrafficOnly": {
    "type": "bool"
}
...
"properties": {
    "accessTier": "[parameters('accessTier')]",
    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
}
...
```

Deze informatie laat het type van de eigenschap zien, en bevestigt ook dat **supportsHttpsTrafficOnly** de eigenschap is die we zoeken.

#### <a name="quickstart-templates-on-github"></a>Quickstart-sjablonen op GitHub

De [Azure-quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates) op GitHub bevatten honderden Resource Manager-sjablonen voor verschillende resources. Deze sjablonen kunnen een uitstekende manier zijn om de resource-eigenschap te vinden die u zoekt. Sommige eigenschappen lijken op wat u zoekt, maar beheren iets anders.

#### <a name="resource-reference-docs"></a>Resourcereferentiedocumenten

Om te controleren of **supportsHttpsTrafficOnly** de juiste eigenschap is, bekijkt u de Resource Manager-sjabloonreferentie voor de [opslagaccountresource](/azure/templates/microsoft.storage/2018-07-01/storageaccounts) op de opslagprovider.
Het eigenschappenobject heeft een lijst met geldige parameters. Door de koppeling [StorageAccountPropertiesCreateParameters-object](/azure/templates/microsoft.storage/2018-07-01/storageaccounts#storageaccountpropertiescreateparameters-object) te selecteren, wordt een tabel met acceptabele eigenschappen weergegeven. **supportsHttpsTrafficOnly** is aanwezig, en de beschrijving ervan komt overeen met wat we zoeken om aan de vereisten van het bedrijf te voldoen.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer

Een andere manier om uw Azure-resources te verkennen is via de [Azure Resource Explorer](https://resources.azure.com) (Preview). Dit hulpprogramma maakt gebruik van de context van uw abonnement, dus u moet zich bij de website aanmelden met uw Azure-referenties. Nadat u zich hebt geverifieerd, kunt u bladeren door providers, abonnementen, resourcegroepen en resources.

Zoek opslagaccountresource en bekijk de eigenschappen. We zien de eigenschap **supportsHttpsTrafficOnly** hier ook. Wanneer we het tabblad **Documentatie** selecteren, zien we dat de beschrijving van de eigenschap overeenkomt met wat we eerder hebben gezien in de referentiedocumenten.

## <a name="find-the-property-alias"></a>De eigenschapalias zoeken

We hebben de resource-eigenschap gevonden, maar we moeten die eigenschap nog toewijzen aan een [alias](../concepts/definition-structure.md#aliases).

Er zijn enkele manieren om de aliassen van een Azure-resource te bepalen. In deze zelfstudie bekijken we elk daarvan:

- Azure-CLI
- Azure PowerShell
- Azure Resource Graph

### <a name="azure-cli"></a>Azure-CLI

In Azure CLI wordt de `az provider`-opdrachtgroep gebruikt om te zoeken naar resource-aliassen. We filteren op de naamruimte **Microsoft.Storage**, op basis van de gegevens die we eerder over de Azure-resource hebben verkregen.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for type Microsoft.Storage
az provider show --namespace Microsoft.Storage --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

In de resultaten zien we een door de opslagaccounts ondersteunde alias met de naam **supportsHttpsTrafficOnly**. Het bestaan van deze alias betekent dat we het beleid voor het afdwingen van onze bedrijfsvereisten kunnen schrijven.

### <a name="azure-powershell"></a>Azure PowerShell

In Azure PowerShell wordt de `Get-AzPolicyAlias`-cmdlet gebruikt om te zoeken naar resource-aliassen. We filteren op de naamruimte **Microsoft.Storage**, op basis van de gegevens die we eerder over de Azure-resource hebben verkregen.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Use Get-AzPolicyAlias to list aliases for Microsoft.Storage
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.Storage').Aliases
```

Net als in Azure CLI zien we in de resultaten een door de opslagaccounts ondersteunde alias met de naam **supportsHttpsTrafficOnly**.

### <a name="azure-resource-graph"></a>Azure Resource Graph

[Azure Resource Graph](../../resource-graph/overview.md) is een nieuwe service in de Preview-versie. Hiermee kunt u op nog een andere manier eigenschappen van Azure-resources vinden. Hier volgt een voorbeeldquery voor het bekijken van één opslagaccount met Resource Graph:

```kusto
where type=~'microsoft.storage/storageaccounts'
| limit 1
```

```azurecli-interactive
az graph query -q "where type=~'microsoft.storage/storageaccounts' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type=~'microsoft.storage/storageaccounts' | limit 1"
```

De resultaten lijken op wat we zien in de Resource Manager-sjablonen en via de Azure Resource Explorer. Resultaten van de grafiek van de Azure-resources kunnen echter ook opnemen [alias](../concepts/definition-structure.md#aliases) details door _projecteren_ de _aliassen_ matrix:

```kusto
where type=~'microsoft.storage/storageaccounts'
| limit 1
| project aliases
```

```azurecli-interactive
az graph query -q "where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
```

Hier volgt een voorbeeld van uitvoer van een opslagaccount voor aliassen:

```json
"aliases": {
    "Microsoft.Storage/storageAccounts/accessTier": null,
    "Microsoft.Storage/storageAccounts/accountType": "Standard_LRS",
    "Microsoft.Storage/storageAccounts/enableBlobEncryption": true,
    "Microsoft.Storage/storageAccounts/enableFileEncryption": true,
    "Microsoft.Storage/storageAccounts/encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true,
                "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
            },
            "file": {
                "enabled": true,
                "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
            }
        }
    },
    "Microsoft.Storage/storageAccounts/encryption.keySource": "Microsoft.Storage",
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyname": null,
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyvaulturi": null,
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyversion": null,
    "Microsoft.Storage/storageAccounts/encryption.services": {
        "blob": {
            "enabled": true,
            "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
        },
        "file": {
            "enabled": true,
            "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
        }
    },
    "Microsoft.Storage/storageAccounts/encryption.services.blob": {
        "enabled": true,
        "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
    },
    "Microsoft.Storage/storageAccounts/encryption.services.blob.enabled": true,
    "Microsoft.Storage/storageAccounts/encryption.services.file": {
        "enabled": true,
        "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
    },
    "Microsoft.Storage/storageAccounts/encryption.services.file.enabled": true,
    "Microsoft.Storage/storageAccounts/networkAcls": {
        "bypass": "AzureServices",
        "defaultAction": "Allow",
        "ipRules": [],
        "virtualNetworkRules": []
    },
    "Microsoft.Storage/storageAccounts/networkAcls.bypass": "AzureServices",
    "Microsoft.Storage/storageAccounts/networkAcls.defaultAction": "Allow",
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*]": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].action": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].id": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].state": [],
    "Microsoft.Storage/storageAccounts/primaryEndpoints": {
        "blob": "https://mystorageaccount.blob.core.windows.net/",
        "file": "https://mystorageaccount.file.core.windows.net/",
        "queue": "https://mystorageaccount.queue.core.windows.net/",
        "table": "https://mystorageaccount.table.core.windows.net/"
    },
    "Microsoft.Storage/storageAccounts/primaryEndpoints.blob": "https://mystorageaccount.blob.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.file": "https://mystorageaccount.file.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.queue": "https://mystorageaccount.queue.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.table": "https://mystorageaccount.table.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.web": null,
    "Microsoft.Storage/storageAccounts/primaryLocation": "eastus2",
    "Microsoft.Storage/storageAccounts/provisioningState": "Succeeded",
    "Microsoft.Storage/storageAccounts/sku.name": "Standard_LRS",
    "Microsoft.Storage/storageAccounts/sku.tier": "Standard",
    "Microsoft.Storage/storageAccounts/statusOfPrimary": "available",
    "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly": false
}
```

Azure Resource Graph (Preview) kan worden gebruikt via [Cloud Shell](https://shell.azure.com), waardoor het een snelle en eenvoudige manier is om de eigenschappen van uw resources te verkennen.

## <a name="determine-the-effect-to-use"></a>Bepalen welk effect moet worden gebruikt

Beslissen wat te doen met uw niet-conforme resources is bijna net zo belangrijk als beslissen wat om te beginnen moet worden geëvalueerd. Elk mogelijk antwoord op een niet-conforme resource heet een [effect](../concepts/effects.md).
Het effect bepaalt of de niet-conforme resource wordt geregistreerd, geblokkeerd, gegevens krijgt toegevoegd of dat er een implementatie aan is gekoppeld om de resource weer conform te maken.

In ons voorbeeld is Deny (weigeren) het gewenste effect, omdat we niet willen dat er niet-conforme resources worden gemaakt in onze Azure-omgeving. Audit (controle) is een goede eerste keuze voor een beleidseffect om te bepalen wat de impact van een beleid is, voordat dit instelt op Weigeren. Eén manier om het wijzigen van het effect per toewijzing gemakkelijker te maken, is door het effect te parametriseren. Zie [parameters](#parameters) hieronder voor informatie over hoe u dat doet.

## <a name="compose-the-definition"></a>De definitie opstellen

We hebben nu de eigenschapsdetails en de alias voor datgene wat we willen beheren. Nu gaan we de beleidsregel zelf samenstellen. Als u nog niet vertrouwd bent met de beleidstaal, kunt u [policy definition structure](../concepts/definition-structure.md) raadplegen voor informatie over het structureren van de beleidsdefinitie. Hier volgt een lege sjabloon die laat zien hoe een beleidsdefinitie eruitziet:

```json
{
    "properties": {
        "displayName": "<displayName>",
        "description": "<description>",
        "mode": "<mode>",
        "parameters": {
                <parameters>
        },
        "policyRule": {
            "if": {
                <rule>
            },
            "then": {
                "effect": "<effect>"
            }
        }
    }
}
```

### <a name="metadata"></a>Metagegevens

De eerste drie onderdelen zijn de metagegevens van het beleid. Voor deze onderdelen kunnen we gemakkelijk waarden opgeven, want we weten waarvoor we de regel maken. [Mode](../concepts/definition-structure.md#mode) gaat voornamelijk over tags en resourcelocatie. Aangezien we de evaluatie niet hoeven te beperken tot resources die tags ondersteunen, gebruiken we de waarde _all_ bij **mode**.

```json
"displayName": "Deny storage accounts not using only HTTPS",
"description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
"mode": "all",
```

### <a name="parameters"></a>Parameters

Hoewel we geen parameter hebben gebruikt om de evaluatie te wijzigen, willen we wel een parameter gebruiken om het **effect** te kunnen wijzigen voor het oplossen van problemen. We definiëren de parameter **effectType**, en beperken deze tot alleen **Deny** (weigeren) en **Disabled** (uitgeschakeld). Deze twee opties komen overeen met onze vereisten. Het voltooide parameterblok ziet eruit zoals in dit voorbeeld:

```json
"parameters": {
    "effectType": {
        "type": "string",
        "defaultValue": "Deny",
        "allowedValues": [
            "Deny",
            "Disabled"
        ],
        "metadata": {
            "displayName": "Effect",
            "description": "Enable or disable the execution of the policy"
        }
    }
},
```

### <a name="policy-rule"></a>Beleidsregel

Het opstellen van de [beleidsregel](../concepts/definition-structure.md#policy-rule) is de laatste stap bij het bouwen van onze aangepaste beleidsdefinitie. We hebben twee instructies geïdentificeerd om op te testen:

- Dat het **type** van het opslagaccount **Microsoft.Storage/storageAccounts** is
- Dat de eigenschap **supportsHttpsTrafficOnly** van het opslagaccount niet **true** is

Omdat we willen dat beide instructies waar zijn, gebruiken we de [logische operator](../concepts/definition-structure.md#logical-operators) **allOf**. We geven de parameter **effectType** door aan het effect in plaats van een statische declaratie te maken. Onze voltooide regel ziet eruit zoals in dit voorbeeld:

```json
"if": {
    "allOf": [
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        },
        {
            "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
            "notEquals": "true"
        }
    ]
},
"then": {
    "effect": "[parameters('effectType')]"
}
```

### <a name="completed-definition"></a>Voltooide definitie

Nu alle drie delen van het beleid zijn gedefinieerd, volgt hier de voltooide definitie:

```json
{
    "properties": {
        "displayName": "Deny storage accounts not using only HTTPS",
        "description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
        "mode": "all",
        "parameters": {
            "effectType": {
                "type": "string",
                "defaultValue": "Deny",
                "allowedValues": [
                    "Deny",
                    "Disabled"
                ],
                "metadata": {
                    "displayName": "Effect",
                    "description": "Enable or disable the execution of the policy"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Storage/storageAccounts"
                    },
                    {
                        "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
                        "notEquals": "true"
                    }
                ]
            },
            "then": {
                "effect": "[parameters('effectType')]"
            }
        }
    }
}
```

De voltooide definitie kan worden gebruikt om een nieuw beleid te maken. Portal en elke SDK (Azure CLI, Azure PowerShell en REST API) accepteren de definitie op verschillende manieren; bekijk de opdrachten voor elk ervan om het juiste gebruik te valideren. Wijs het beleid vervolgens met behulp van het geparameteriseerde effect toe aan de juiste resources om de veiligheid van uw opslagaccounts te beheren.

## <a name="review"></a>Beoordelen

In deze zelfstudie hebt u de volgende taken uitgevoerd:

> [!div class="checklist"]
> - De vereisten van uw bedrijf geïdentificeerd
> - Elke vereiste toegewezen aan een Azure-resource-eigenschap
> - De eigenschap toegewezen aan een alias
> - Bepaald welk effect moet worden gebruikt
> - De beleidsdefinitie opgesteld

## <a name="next-steps"></a>Volgende stappen

Gebruik vervolgens de aangepaste beleidsdefinitie om een beleid te maken en toe te wijzen:

> [!div class="nextstepaction"]
> [Een beleidsdefinitie maken en toewijzen](../how-to/programmatically-create.md#create-and-assign-a-policy-definition)