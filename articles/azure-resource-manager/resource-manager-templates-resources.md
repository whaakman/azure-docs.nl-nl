---
title: Structuur van Azure Resource Manager-sjabloon en syntaxis | Microsoft Docs
description: Beschrijft de structuur en eigenschappen van Azure Resource Manager-sjablonen met behulp van declaratieve JSON-syntaxis.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2017
ms.author: tomfitz
ms.openlocfilehash: 89e4b52e7d306bd495c426bcf775f59d0f30eb55
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Sectie van de resources van Azure Resource Manager-sjablonen

In de bronnensectie definieert u de resources die worden geïmplementeerd of bijgewerkt. Deze sectie kunt krijgen ingewikkeld omdat de typen die u implementeert de juiste waarden opgeven dat u begrijpt.

## <a name="available-properties"></a>Beschikbare eigenschappen

U definieert resources met de volgende structuur:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
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
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
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
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elementnaam | Vereist | Beschrijving |
|:--- |:--- |:--- |
| voorwaarde | Nee | Booleaanse waarde die aangeeft of de resource is geïmplementeerd. |
| apiVersion |Ja |De versie van de REST-API gebruiken voor het maken van de resource. |
| type |Ja |Type van de resource. Deze waarde is een combinatie van de naamruimte van de resourceprovider en het resourcetype (zoals **Microsoft.Storage/storageAccounts**). |
| naam |Ja |De naam van de resource. De naam moet URI onderdeel beperkingen gedefinieerd in RFC3986 volgen. Bovendien Azure-services die beschikbaar om te valideren buiten partijen de naam om te controleren of deze de naam van de resource is niet een poging tot een andere identiteit vervalsen. |
| location |Varieert |Ondersteunde geografische locaties van de opgegeven bron. U kunt een van de beschikbare locaties selecteren, maar meestal is het verstandig om te selecteren die dicht bij uw gebruikers. Meestal is het ook handig om resources die in dezelfde regio met elkaar communiceren. De meeste brontypen die een locatie vereist, maar sommige typen (zoals een roltoewijzing) hoeven niet een locatie. |
| tags |Nee |Labels die gekoppeld aan de resource zijn. Labels te organiseren logisch resources in uw abonnement toepassen. |
| Opmerkingen |Nee |De notities voor de resources in uw sjabloon documenteren |
| kopiëren |Nee |Als meer dan één exemplaar is vereist, het aantal resources om te maken. Er is de standaardmodus voor parallelle. Seriële modus wanneer u niet dat alle wilt of de resources te implementeren op hetzelfde moment opgeven. Zie voor meer informatie [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nee |Resources die moeten worden geïmplementeerd voordat u deze bron wordt geïmplementeerd. Resource Manager evalueert de afhankelijkheden tussen resources en ze worden geïmplementeerd in de juiste volgorde. Wanneer u resources zijn niet afhankelijk van elkaar, worden ze geïmplementeerd parallel. De waarde kan een door komma's gescheiden lijst van een resource zijn namen of unieke id's voor een resource. Alleen de lijst van resources die zijn geïmplementeerd in deze sjabloon. Bronnen die niet in deze sjabloon zijn gedefinieerd, moeten al bestaan. Vermijd toe te voegen onnodige afhankelijkheden als ze kunnen uw implementatie vertragen en circulaire afhankelijkheden maken. Zie voor instructies over de afhankelijkheden van de instelling [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md). |
| properties |Nee |Resource-specifieke configuratie-instellingen. De waarden voor de eigenschappen zijn hetzelfde als de waarden die u in de aanvraagtekst voor de REST-API-bewerking (PUT-methode opgeeft) om de resource te maken. U kunt ook een matrix kopiëren voor het maken van meerdere exemplaren van een eigenschap opgeven. |
| bronnen |Nee |Onderliggende resources die afhankelijk zijn van de bron wordt gedefinieerd. Geef alleen brontypen die worden toegestaan door het schema van de bovenliggende resource. Het volledig gekwalificeerde type van de onderliggende resource bevat het type van de bovenliggende resource, zoals **Microsoft.Web/sites/extensions**. Afhankelijkheid van de bovenliggende resource niet geïmpliceerd. U moet deze afhankelijkheid expliciet definiëren. |

## <a name="resource-specific-values"></a>Resource-specifieke waarden

De **apiVersion**, **type**, en **eigenschappen** verschillen voor elk resourcetype. Om te bepalen waarden voor deze eigenschappen, Zie [sjabloonverwijzing](/azure/templates/).

## <a name="resource-names"></a>Resourcenamen
In het algemeen werken u met drie soorten resourcenamen in Resource Manager:

* Resourcenamen moeten uniek zijn.
* Resourcenamen die nodig zijn niet uniek te zijn, maar u kunt een naam waarmee u de bron identificeren kunt geven.
* Resourcenamen kunnen niet algemeen zijn.

### <a name="unique-resource-names"></a>Unieke resourcenamen
U moet een unieke bronnaam voor resourcetype dat een data access-eindpunt opgeven. Sommige algemene brontypen waarvoor een unieke naam zijn onder andere:

* Azure Storage<sup>1</sup> 
* Web Apps-functie van Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis-cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> opslagaccountnamen ook moet een kleine letter, 24 tekens of korter is, en geen eventuele verbindingsstreepjes.

Wanneer u de naam, u kunt handmatig een unieke naam of gebruik de [uniqueString()](resource-group-template-functions-string.md#uniquestring) functie voor het genereren van een naam. Ook raadzaam een voorvoegsel toevoegen of een achtervoegsel aan de **uniqueString** resultaat. De unieke naam wijzigen, kunt u gemakkelijk herkennen het brontype van de naam. U kunt bijvoorbeeld een unieke naam voor een opslagaccount genereren met behulp van de volgende variabele:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Resourcenamen voor identificatie
Sommige kunt u de naam, maar hun namen van resourcetypen hoeft geen uniek zijn. U kunt een naam waarmee u zowel de context van de bron en het resourcetype opgeven voor deze resourcetypen.

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
Voor brontypen die u voornamelijk via een andere resource benaderen, kunt u een algemene naam die is vastgelegd in de sjabloon. U kunt bijvoorbeeld een algemene naam op voor firewallregels instellen op een SQL server:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Locatie
Bij het implementeren van een sjabloon, moet u een locatie voor elke resource opgeven. Andere brontypen die worden ondersteund in verschillende locaties. Als een lijst met locaties die beschikbaar voor uw abonnement voor een bepaald brontype zijn weergeven, gebruikt u Azure PowerShell of Azure CLI. 

Het volgende voorbeeld maakt gebruik van PowerShell om op te halen van de locaties voor de `Microsoft.Web\sites` brontype:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Het volgende voorbeeld maakt gebruik van Azure CLI 2.0 ophalen van de locaties voor de `Microsoft.Web\sites` brontype:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Na het vaststellen van de ondersteunde locaties voor uw resources die locatie in uw sjabloon is ingesteld. De eenvoudigste manier om in te stellen van deze waarde is een resourcegroep maken op een locatie die ondersteuning biedt voor de volgende resourcetypen en elke locatie op instellen `[resourceGroup().location]`. U kunt de sjabloon aan resourcegroepen op verschillende locaties opnieuw implementeert en alle waarden in de sjabloon of parameters niet te wijzigen. 

Het volgende voorbeeld ziet u een opslagaccount dat is geïmplementeerd op dezelfde locatie als de resourcegroep:

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

Als u hardcode voor de locatie in de sjabloon wilt, geef de naam van een van de ondersteunde regio's. Het volgende voorbeeld ziet u een opslagaccount die altijd wordt geïmplementeerd op Noordelijk Centraal, VS:

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
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

### <a name="add-tags-to-your-template"></a>Labels toevoegen aan de sjabloon

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Onderliggende resources

In sommige brontypen, kunt u ook een matrix van onderliggende resources definiëren. Onderliggende resources zijn bronnen die alleen binnen de context van een andere resource voorkomen. Bijvoorbeeld, bestaan niet een SQL-database zonder een SQL-server zodat de database een onderliggend element van de server is. U kunt de database binnen de definitie voor de server definiëren.

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

Wanneer genest, het type is ingesteld op `databases` , maar het type volledige resource `Microsoft.Sql/servers/databases`. U geen specifieke `Microsoft.Sql/servers/` omdat ervan wordt uitgegaan van het type van de bovenliggende resource. De naam van de onderliggende resource is ingesteld op `exampledatabase` , maar de naam van de volledige naam van het bovenliggende bevat. U geen specifieke `exampleserver` omdat ervan wordt uitgegaan van de bovenliggende resource.

De indeling van het onderliggende brontype is:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

De indeling van de naam van de onderliggende resource is:`{parent-resource-name}/{child-resource-name}`

Maar u hoeft niet te definiëren van de database binnen de server. U kunt de onderliggende resource op het hoogste niveau definiëren. U kunt deze benadering gebruiken als de bovenliggende resource niet is geïmplementeerd in dezelfde sjabloon of wilt gebruiken `copy` meerdere onderliggende resources te maken. Met deze methode moet u het volledige brontype bieden en de naam van de bovenliggende resource opnemen in de naam van de onderliggende resource.

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

Bij het maken van een volledig gekwalificeerde verwijzing naar een resource wordt de volgorde te combineren segmenten van het type en de naam is niet gewoon een samenvoeging van de twee.  Gebruik in plaats daarvan na de naamruimte, een reeks *typenaam/* paren van minst specifiek voor het meest specifiek:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Bijvoorbeeld:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`juist `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is niet correct

## <a name="recommendations"></a>Aanbevelingen
De volgende informatie kan nuttig zijn wanneer u met resources werkt:

* Andere inzenders leert wat het doel van de resource Geef, om te **opmerkingen** voor elke resource in de sjabloon:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Als u een *openbaar eindpunt* in uw sjabloon (zoals een Azure Blob storage openbaar eindpunt), *komen niet vastleggen* de naamruimte. Gebruik de **verwijzing** functie voor het dynamisch ophalen van de naamruimte. Deze aanpak kunt u de sjabloon kan implementeren op andere openbare naamruimte omgevingen zonder handmatig wijzigen van het eindpunt in de sjabloon. Stel de API-versie naar dezelfde versie die u voor het opslagaccount in de sjabloon gebruikt:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Als het opslagaccount is geïmplementeerd in dezelfde sjabloon die u maakt, hoeft u geen naamruimte van de provider opgeven wanneer u verwijst naar de resource. Het volgende voorbeeld ziet u de vereenvoudigde syntaxis:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Als u andere waarden in de sjabloon die zijn geconfigureerd voor gebruik van een openbare naamruimte hebt, deze waarden zodat dezelfde wijzigen **verwijzing** functie. U kunt bijvoorbeeld instellen de **storageUri** eigenschap van de virtuele machine diagnostische profiel:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Ook kunt u verwijzen naar een bestaand opslagaccount die zich in een andere resourcegroep:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Openbare IP-adressen toewijzen aan een virtuele machine alleen als dit vereist is voor een toepassing. Voor verbinding met een virtuele machine (VM) voor foutopsporing of voor beheer of administratieve doeleinden, inkomende NAT-regels, een virtuele netwerkgateway of een jumpbox te gebruiken.
   
     Zie voor meer informatie over verbinding maken met virtuele machines:
   
   * [Virtuele machines worden uitgevoerd voor een architectuur N-aantal lagen in Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [WinRM toegang instellen voor virtuele machines in Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Toestaan van externe toegang tot uw virtuele machine met behulp van de Azure-portal](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Toestaan van externe toegang tot uw virtuele machine met behulp van PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Toestaan van externe toegang tot uw Linux-VM met Azure CLI](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* De **domainNameLabel** eigenschap voor openbare IP-adressen moet uniek zijn. De **domainNameLabel** waarde moet tussen 3 en 63 tekens lang en volgt u de regels die door deze reguliere expressie opgegeven: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Omdat de **uniqueString** functie een tekenreeks die is 13 tekens lang zijn, genereert de **dnsPrefixString** parameter is beperkt tot 50 tekens bevatten:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Wanneer u een wachtwoord aan een extensie voor aangepaste scripts toevoegt, gebruiken de **commandToExecute** eigenschap in de **protectedSettings** eigenschap:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Om ervoor te zorgen dat de geheimen worden versleuteld wanneer ze als parameters worden doorgegeven aan VM's en -extensies, gebruiken de **protectedSettings** eigenschap van de relevante extensies.
   > 
   > 


## <a name="next-steps"></a>Volgende stappen
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
* Zie voor meer informatie over de functies die u van binnen een sjabloon gebruiken kunt [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md).
* Als u wilt combineren meerdere sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Wellicht moet u bronnen gebruiken die zijn opgeslagen in een andere resourcegroep. Dit scenario is gebruikelijk dat wanneer u werkt met opslagaccounts of virtuele netwerken die worden gedeeld door meerdere resourcegroepen. Zie voor meer informatie de [resourceId functie](resource-group-template-functions-resource.md#resourceid).
* Zie voor meer informatie over de beperkingen voor resource [aanbevolen naamgevingsregels voor Azure-resources](../guidance/guidance-naming-conventions.md).