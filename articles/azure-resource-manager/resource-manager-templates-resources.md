---
title: Sjabloonresources van Azure Resource Manager-| Microsoft Docs
description: Hierin wordt beschreven in de sectie met resources van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: 5a2b38e5d627341b3684ee55d13ee06881fbae55
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728360"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Sectie met resources van Azure Resource Manager-sjablonen

In de sectie resources definieert u de resources die worden geïmplementeerd of bijgewerkt. In deze sectie krijgt ingewikkeld omdat u de typen die u implementeert zodat de juiste waarden moet begrijpen.

## <a name="available-properties"></a>Beschikbare eigenschappen

Definieert u resources met de volgende structuur:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| De naam van element | Vereist | Description |
|:--- |:--- |:--- |
| voorwaarde | Nee | Booleaanse waarde die aangeeft of de resource tijdens deze implementatie worden ingericht. Wanneer `true`, de resource is gemaakt tijdens de implementatie. Wanneer `false`, de bron voor deze implementatie wordt overgeslagen. |
| apiVersion |Ja |De versie van de REST-API moet worden gebruikt voor het maken van de resource. |
| type |Ja |Het type van de resource. Deze waarde is een combinatie van de naamruimte van de resourceprovider en het resourcetype (zoals **Microsoft.Storage/storageAccounts**). |
| naam |Ja |Naam van de resource. De naam moet URI-onderdeel beperkingen gedefinieerd in RFC3986 volgen. Azure-services die beschikbaar maken van de naam van de resource buiten partijen valideren de naam om te controleren of het is bovendien een poging tot het vervalsen van een andere identiteit niet. |
| location |Varieert |Geografische locaties van de opgegeven resource wordt ondersteund. U kunt een van de beschikbare locaties selecteren, maar meestal is het zinvol om te kiezen die zich in de buurt van uw gebruikers. Meestal is het ook verstandig om de resources die met elkaar in dezelfde regio communiceren te plaatsen. De meeste resourcetypen een locatie vereist, maar sommige typen (zoals een roltoewijzing) vereisen een locatie. |
| tags |Nee |Tags die gekoppeld aan de resource zijn. Labels toevoegen om in te delen logisch resources in uw abonnement. |
| opmerkingen |Nee |Uw notities voor het documenteren van de resources in uw sjabloon |
| kopiëren |Nee |Als meer dan één exemplaar is vereist, het aantal resources om te maken. Er is de standaardmodus voor parallelle. Seriële modus wanneer u niet dat alle wilt of de resources om te implementeren op hetzelfde moment opgeven. Zie voor meer informatie, [verschillende exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nee |Resources die moeten worden geïmplementeerd voordat deze resource is geïmplementeerd. Resource Manager evalueert de afhankelijkheden tussen resources en ze implementeert in de juiste volgorde. Als resources niet van elkaar afhankelijk zijn, zijn ze parallel geïmplementeerd. De waarde kan een door komma's gescheiden lijst van een resource zijn namen of resource-id's uniek. Alleen lijst met resources die in deze sjabloon zijn geïmplementeerd. Resources die niet zijn gedefinieerd in deze sjabloon moeten al bestaan. Vermijd onnodige afhankelijkheden toevoegen als ze kunnen uw implementatie vertragen en circulaire afhankelijkheden maken. Zie voor meer informatie over de afhankelijkheden van de instelling [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md). |
| properties |Nee |Resource-specifieke configuratie-instellingen. De waarden voor de eigenschappen zijn hetzelfde als de waarden die u in de hoofdtekst van de aanvraag voor de REST-API-bewerking (PUT-methode opgeeft) om de resource te maken. U kunt ook een matrix kopiëren voor het maken van meerdere exemplaren van een eigenschap opgeven. |
| sku | Nee | Sommige resources zijn waarden toegestaan die definiëren van de SKU om het te implementeren. Bijvoorbeeld, kunt u het type redundantie voor een opslagaccount. |
| type | Nee | Sommige resources kunnen een waarde die bepaalt het type resource dat u implementeert. Bijvoorbeeld, kunt u het type van de Cosmos DB te maken. |
| plan | Nee | Sommige resources zijn waarden toegestaan die in de planning definiëren wilt implementeren. U kunt bijvoorbeeld opgeven dat de marketplace-installatiekopie voor een virtuele machine. | 
| bronnen |Nee |Onderliggende resources die afhankelijk zijn van de resource wordt gedefinieerd. Geef alleen resourcetypen die zijn toegestaan door het schema van de bovenliggende resource. De volledig gekwalificeerde type van de onderliggende resource bevat het type van de bovenliggende resource, zoals **Microsoft.Web/sites/extensions**. Afhankelijkheid van de bovenliggende resource is niet impliciet. Afhankelijkheid zijn opgetreden, moet u expliciet definiëren. |

## <a name="condition"></a>Voorwaarde

Wanneer u tijdens de implementatie van al dan niet om een resource te maken beslissen moet, gebruikt u de `condition` element. De waarde voor dit element wordt omgezet in waar of ONWAAR. Wanneer de waarde true is, wordt de resource is gemaakt. Wanneer de waarde false is, wordt de resource is niet gemaakt. De waarde kan alleen worden toegepast op de hele resource.

Meestal gebruikt u deze waarde als u wilt maken van een nieuwe resource of gebruik een bestaande resourcegroep. Bijvoorbeeld, om op te geven of een nieuw opslagaccount wordt geïmplementeerd of een bestaand opslagaccount wordt gebruikt, gebruikt u het:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Voor een compleet voorbeeld-sjabloon die gebruikmaakt van de `condition` -element, Zie [VM met een nieuwe of bestaande Virtueelnetwerk, opslag- en openbare IP-adres](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="resource-specific-values"></a>Resource-specifieke waarden

De **apiVersion**, **type**, en **eigenschappen** elementen verschillen voor elk resourcetype. De **sku**, **soort**, en **plan** elementen zijn beschikbaar voor sommige resourcetypen, maar niet alle. Om te bepalen van waarden voor deze eigenschappen, Zie [sjabloonverwijzing](/azure/templates/).

## <a name="resource-names"></a>Namen van voorbeeldresources

In het algemeen, werkt u met drie typen resourcenamen in Resource Manager:

* Namen van voorbeeldresources die moeten uniek zijn.
* Namen van voorbeeldresources die niet zijn vereist voor het uniek zijn, maar u kiest een naam waarmee u identificeren van de resource op te geven.
* Namen van voorbeeldresources die algemene worden kunnen.

### <a name="unique-resource-names"></a>De unieke resourcenamen

Geef een unieke resourcenaam voor elk resourcetype dat een eindpunt van de toegang tot gegevens heeft. Sommige algemene resourcetypen waarvoor een unieke naam zijn onder andere:

* Azure Storage<sup>1</sup> 
* Web Apps-functie van Azure App Service
* SQL Server
* Azure Key Vault
* Azure Cache voor Redis
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> opslagaccountnamen ook moet een kleine letter, 24 tekens of korter is, en niet alle afbreekstreepjes bevatten.

Bij het instellen van de naam, u kunt handmatig maken van een unieke naam of gebruik de [uniqueString()](resource-group-template-functions-string.md#uniquestring) functie voor het genereren van een naam. Ook kunt u voegt u een voorvoegsel of achtervoegsel aan de **uniqueString** resultaat. De unieke naam wijzigen, kunt u eenvoudig kunt identificeren het brontype van de naam. U kunt bijvoorbeeld een unieke naam voor een opslagaccount genereren met behulp van de volgende variabele:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Namen van voorbeeldresources voor de identificatie
Sommige resourcetypen die kunt u de naam, maar hun namen niet moeten uniek zijn. U kunt een naam ter identificatie van zowel de bron-context en het resourcetype opgeven voor deze resourcetypen.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Algemene resourcenamen
Voor de resourcetypen die voornamelijk toegankelijk zijn via een andere resource, kunt u een algemene naam die is vastgelegd in de sjabloon. U kunt bijvoorbeeld een algemene naam voor de firewall-regels instellen op een SQL server:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Locatie
Bij het implementeren van een sjabloon, moet u een locatie voor elke resource opgeven. Andere resource-typen worden ondersteund op verschillende locaties. Een lijst met locaties die beschikbaar voor uw abonnement voor een bepaald brontype zijn wilt bekijken, gebruikt u Azure PowerShell of Azure CLI. 

Het volgende voorbeeld wordt PowerShell gebruikt om op te halen van de locaties voor de `Microsoft.Web\sites` resourcetype:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Het volgende voorbeeld maakt gebruik van Azure CLI om op te halen van de locaties voor de `Microsoft.Web\sites` resourcetype:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Na het vaststellen van de ondersteunde locaties voor uw resources, die locatie in uw sjabloon is ingesteld. De eenvoudigste manier om deze waarde hebt ingesteld, wordt een resourcegroep maken op een locatie die ondersteuning biedt voor de resourcetypen en elke locatie ingesteld op `[resourceGroup().location]`. U kunt opnieuw implementeren van de sjabloon aan resourcegroepen op verschillende locaties en alle waarden in de sjabloon of parameters niet te wijzigen. 

Het volgende voorbeeld ziet u een opslagaccount dat wordt geïmplementeerd op dezelfde locatie als de resourcegroep:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Als u nodig in hardcode vastlegt de locatie in de sjabloon hebt, geef de naam van een van de ondersteunde regio's. Het volgende voorbeeld ziet u een opslagaccount die altijd is geïmplementeerd in Noord-centraal VS:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="tags"></a>Tags
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>Labels toevoegen aan uw sjabloon

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Onderliggende resources

In sommige resourcetypen, kunt u ook een reeks onderliggende resources definiëren. Onderliggende resources zijn resources die alleen zijn opgeslagen in de context van een andere resource. Een SQL-database niet kan bijvoorbeeld bestaan zonder een SQL-server, zodat de database een onderliggend element van de server is. U kunt de database in de definitie voor de server definiëren.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Wanneer genest, het type is ingesteld op `databases` , maar het type volledige resource `Microsoft.Sql/servers/databases`. U geen opgeeft `Microsoft.Sql/servers/` omdat ervan wordt uitgegaan van het type van de bovenliggende resource. De naam van de onderliggende bron is ingesteld op `exampledatabase` , maar de volledige naam bevat de naam van de bovenliggende. U geen opgeeft `exampleserver` omdat ervan wordt uitgegaan van de bovenliggende resource.

De indeling van het type van de onderliggende bron is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

De indeling van de naam van de onderliggende bron is: `{parent-resource-name}/{child-resource-name}`

Maar u hoeft de database in de server. U kunt de onderliggende resource op het hoogste niveau definiëren. U kunt deze methode gebruiken als de bovenliggende resource is niet geïmplementeerd in dezelfde sjabloon of wilt gebruiken `copy` om meer dan één onderliggende resource te maken. Met deze methode moet u het type volledige resource en omvatten de naam van de bovenliggende resource in de naam van de onderliggende resource.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Bij het maken van een volledig gekwalificeerde verwijzing naar een resource, is de volgorde te combineren segmenten van het type en de naam niet gewoon een samenvoeging van de twee. In plaats daarvan nadat de naamruimte van het gebruik van een reeks *typenaam/* paren van minst specifiek voor meest specifieke:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Bijvoorbeeld:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` klopt `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is niet correct



## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u uit in een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Zie voor aanbevelingen over het maken van sjablonen, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md).
* Mogelijk moet u de resources die zijn opgeslagen in een andere resourcegroep gebruiken. In dit scenario is gebruikelijk bij het werken met opslagaccounts of virtuele netwerken die zijn verdeeld over verschillende resourcegroepen. Zie voor meer informatie de [resourceId functie](resource-group-template-functions-resource.md#resourceid).
* Zie voor meer informatie over de beperkingen op basis van resource [aanbevolen naamgevingsregels voor Azure-resources](../guidance/guidance-naming-conventions.md).