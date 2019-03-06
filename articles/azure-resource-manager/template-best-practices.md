---
title: Aanbevolen procedures voor Azure Resource Manager-sjablonen
description: Hierin wordt beschreven aanbevolen benaderingen voor het ontwerpen van Azure Resource Manager-sjablonen. Biedt suggesties om te voorkomen dat bekende problemen bij het gebruik van sjablonen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2019
ms.author: tomfitz
ms.openlocfilehash: bcc529b02505359e6e4e320d4991a082797c5261
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440469"
---
# <a name="azure-resource-manager-template-best-practices"></a>Azure Resource Manager sjabloon aanbevolen procedures

In dit artikel biedt aanbevelingen over het maken van uw Resource Manager-sjabloon. Deze aanbevelingen kunnen u voorkomen dat bekende problemen bij het gebruik van een sjabloon om een oplossing te implementeren.

Zie voor aanbevelingen over het beheren van uw Azure-abonnementen, [Azure enterprise-platform: Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption/appendix/azure-scaffold?toc=%2Fen-us%2Fazure%2Fazure-resource-manager%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json).

Zie voor aanbevelingen over het bouwen van sjablonen die in alle Azure-cloud-omgevingen werken [ontwikkelen van Azure Resource Manager-sjablonen voor de consistentie van de cloud](templates-cloud-consistency.md).

## <a name="template-limits"></a>Limieten voor sjabloon

Beperkt de grootte van uw sjabloon 1 MB en elk parameterbestand aan 64 KB. De limiet van 1 MB is van toepassing op de laatste status van de sjabloon nadat deze is uitgebreid met iteratieve resourcedefinities en waarden voor parameters en variabelen. 

U bent ook beperkt tot:

* 256-parameters
* 256 variabelen
* 800 bronnen (zoals aantal kopieën)
* 64 uitvoerwaarden
* 24.576 tekens in een sjabloonexpressie voor een

U kunt sommige limieten sjabloon met behulp van een geneste sjabloon overschrijdt. Zie voor meer informatie, [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](resource-group-linked-templates.md). Als u wilt verkleinen het aantal parameters, variabelen of uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie, [objecten als parameters](resource-manager-objects-as-parameters.md).

## <a name="resource-group"></a>Resourcegroep

Wanneer u resources in een resourcegroep implementeert, wordt de resourcegroep slaat metagegevens over de resources. De metagegevens worden opgeslagen in de locatie van de resourcegroep.

Als de resourcegroep regio tijdelijk niet beschikbaar is, kunt u resources in de resourcegroep niet bijwerken omdat de metagegevens niet beschikbaar is. De resources in andere regio's wordt nog steeds werken zoals verwacht, maar kan niet worden bijgewerkt. Zoeken om risico te beperken, de resourcegroep en resources in dezelfde regio.

## <a name="parameters"></a>Parameters
De informatie in deze sectie kan nuttig zijn wanneer u met werkt [parameters](resource-group-authoring-templates.md#parameters).

### <a name="general-recommendations-for-parameters"></a>Algemene aanbevelingen voor de parameters

* Uw gebruik van parameters minimaliseren. Gebruik in plaats daarvan variabelen of letterlijke waarden voor eigenschappen die niet hoeven te worden opgegeven tijdens de implementatie.

* Gebruik kamelen voor namen van parameters.

* Gebruik de parameters voor instellingen die afhankelijk van de omgeving, zoals SKU, grootte of capaciteit zijn.

* Gebruik de parameters voor de namen van voorbeeldresources die u wilt opgeven voor de eenvoudige identificatie.

* Geef een beschrijving van elke parameter in de metagegevens:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Standaardwaarden voor parameters die niet gevoelige definiëren. Door op te geven een standaardwaarde is opgegeven, is het eenvoudiger om de sjabloon te implementeren en gebruikers van uw sjabloon bekijken een voorbeeld van een geschikte waarde op. Standaardwaarde voor een parameter moet geldig zijn voor alle gebruikers in de standaardconfiguratie van de implementatie. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Als u een optionele parameter, gebruik niet een lege tekenreeks als een standaardwaarde. Een letterlijke waarde of taalexpressie voor een in plaats daarvan gebruik te maken van een waarde.

   ```json
   "storageAccountName": {
     "type": "string",
     "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
     "metadata": {
       "description": "Name of the storage account"
     }
   },
   ```

* Een parameter niet worden gebruikt voor de API-versie voor een resourcetype. Resource-eigenschappen en waarden kunnen variëren per versienummer. IntelliSense in een code-editor kan niet het juiste schema bepalen wanneer de API-versie is ingesteld op een parameter. In plaats daarvan programmeren de API-versie in de sjabloon.

* Gebruik `allowedValues` spaarzaam. Gebruik deze optie alleen als moet u ervoor zorgen dat bepaalde waarden niet zijn opgenomen in de toegestane opties. Als u `allowedValues` te breed u geldige implementaties mogelijk blokkeren door de lijst met niet up-to-date houden.

* Wanneer een parameternaam in uw sjabloon overeenkomt met een parameter in de PowerShell-opdracht voor implementatie, Resource Manager deze naamgevingsconventie conflict opgelost doordat de postfix **FromTemplate** aan de sjabloonparameter. Bijvoorbeeld, als u een parameter met de naam opgeeft **ResourceGroupName** in uw sjabloon, deze conflicteert met de **ResourceGroupName** parameter in de [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) cmdlet. Tijdens de implementatie van u wordt gevraagd om een waarde voor **ResourceGroupNameFromTemplate**.

### <a name="security-recommendations-for-parameters"></a>Aanbevelingen voor beveiliging voor de parameters

* Gebruik altijd de parameters voor gebruikersnamen en wachtwoorden (of geheime informatie).

* Gebruik `securestring` voor alle wachtwoorden en geheimen. Als u gevoelige gegevens in een JSON-object doorgeven, gebruikt u de `secureObject` type. Sjabloonparameters met beveiligde tekenreeks of beveiligde objecttypen kunnen niet worden gelezen na de implementatie van de resource. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Niet voor de standaardwaarden voor gebruikersnamen, wachtwoorden of een willekeurige waarde waarvoor een `secureString` type.

* Niet u standaardwaarden opgeven voor eigenschappen die de aanval surface area van de toepassing te verhogen.

### <a name="location-recommendations-for-parameters"></a>Locatie-aanbevelingen voor de parameters

* Gebruik een parameter om op te geven van de locatie voor resources en de standaardwaarde ingesteld op `resourceGroup().location`. De parameter van een locatie bieden, kunnen gebruikers van de sjabloon om een locatie die ze gemachtigd zijn om te implementeren op te geven.

   ```json
   "parameters": {
     "location": {
       "type": "string",
       "defaultValue": "[resourceGroup().location]",
       "metadata": {
         "description": "The location in which the resources should be deployed."
       }
     }
   },
   ```

* Geef geen `allowedValues` voor de locatieparameter. De locaties die u opgeeft zijn mogelijk niet in alle clouds beschikbaar.

* Gebruik de waarde van de locatie voor resources die kunnen worden op dezelfde locatie. Deze aanpak minimaliseert het aantal keer dat gebruikers wordt gevraagd om locatie-informatie te geven.

* Voor resources die niet beschikbaar zijn op alle locaties, gebruikt u een afzonderlijke parameter of geef een locatiewaarde voor letterlijke.

## <a name="variables"></a>Variabelen

De volgende informatie kan nuttig zijn wanneer u met werkt [variabelen](resource-group-authoring-templates.md#variables):

* Variabelen voor waarden die u wilt meer dan één keer gebruiken in een sjabloon gebruiken. Als een waarde slechts één keer gebruikt wordt, een vastgelegde waarde maakt de sjabloon voor het gemakkelijker te lezen.

* Variabelen gebruiken voor waarden die u uit een complexe rangschikking van de sjabloonvariabelen samenstelt. De sjabloon is gemakkelijker te lezen wanneer de complexe expressie alleen wordt weergegeven in de variabelen.

* Gebruik geen variabelen voor `apiVersion` op een resource. De API-versie bepaalt het schema van de resource. U kunt de versie vaak niet wijzigen zonder het wijzigen van de eigenschappen voor de resource.

* U kunt geen gebruiken de [verwijzing](resource-group-template-functions-resource.md#reference) werken in de **variabelen** gedeelte van de sjabloon. De **verwijzing** functie is afgeleid van de waarde van de runtimestatus van de resource. Variabelen zijn echter opgelost bij het eerste parseren van de sjabloon. Om voor te bereiden waarden die moeten de **verwijzing** functie rechtstreeks in de **resources** of **levert** gedeelte van de sjabloon.

* Variabelen voor de namen van voorbeeldresources die moeten uniek zijn.

* Gebruik een [kopie-lus in variabelen](resource-group-create-multiple.md#variable-iteration) herhaalde patronen van JSON-objecten maken.

* Niet-gebruikte variabelen verwijderen.

## <a name="resource-dependencies"></a>Bronafhankelijkheden

Wanneer u beslist wat [afhankelijkheden](resource-group-define-dependencies.md) wilt instellen, gebruikt u de volgende richtlijnen:

* Gebruik de **verwijzing** functioneren en geef de resourcenaam een impliciete afhankelijkheid tussen resources die nodig hebt voor het delen van een eigenschap instellen. Voeg een expliciete niet `dependsOn` element als u al een impliciete afhankelijkheid hebt gedefinieerd. Deze aanpak vermindert het risico dat onnodige afhankelijkheden.

* Een onderliggende resource instellen als afhankelijk van de bovenliggende resource.

* Resources met de [voorwaarde element](resource-group-authoring-templates.md#condition) ingesteld op false, worden automatisch verwijderd uit de volgorde van de afhankelijkheid. De afhankelijkheden instellen als de resource is altijd geïmplementeerd.

* Laat afhankelijkheden cascade zonder expliciet instelt. Bijvoorbeeld: uw virtuele machine is afhankelijk van een virtuele netwerkinterface en de virtuele netwerkinterface is afhankelijk van een virtueel netwerk en openbare IP-adressen. Daarom wordt de virtuele machine is geïmplementeerd nadat alle drie resources, maar de virtuele machine niet expliciet worden ingesteld als afhankelijk is van alle drie resources. Deze aanpak wordt uitleg gegeven over de volgorde van de afhankelijkheid en kunt u gemakkelijker de sjabloon later wijzigen.

* Als een waarde kan worden bepaald vóór de implementatie, kunt u de implementatie van de resource zijn zonder een afhankelijkheid. Bijvoorbeeld, als een configuratiewaarde de naam van een andere bron moet, moet u mogelijk niet een afhankelijkheid. In deze richtlijnen werken niet altijd, omdat sommige resources controleren of er sprake van de andere resource. Als u een foutbericht ontvangt, moet u een afhankelijkheid toevoegen.

## <a name="resources"></a>Resources

De volgende informatie kan nuttig zijn wanneer u met werkt [resources](resource-group-authoring-templates.md#resources):

* Geef andere inzenders leert wat het doel van de resource, zodat **opmerkingen** voor elke resource in de sjabloon:
   
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

* Als u een *openbaar eindpunt* in uw sjabloon (zoals een Azure Blob storage openbaar eindpunt), *programmeren niet* de naamruimte. Gebruik de **verwijzing** functie voor het dynamisch de naamruimte niet ophalen. Deze aanpak kunt u de sjabloon implementeren in andere openbare naamruimte omgevingen zonder handmatig wijzigen van het eindpunt in de sjabloon. Stel de API-versie naar dezelfde versie die u voor het opslagaccount in de sjabloon gebruikt:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Als het opslagaccount dat is geïmplementeerd in dezelfde sjabloon die u maakt, moet u geen naamruimte van de provider opgeven wanneer u verwijst naar de resource. Het volgende voorbeeld ziet u de vereenvoudigde syntaxis:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Als u andere waarden in de sjabloon die zijn geconfigureerd voor het gebruik van een openbare-naamruimte hebt, wijzigt u deze waarden om dezelfde **verwijzing** functie. U kunt bijvoorbeeld instellen de **storageUri** eigenschap van de virtuele machine diagnostische profiel:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   U kunt ook een bestaand opslagaccount die zich in een andere resourcegroep verwijzen:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Openbare IP-adressen toewijzen aan een virtuele machine alleen wanneer een toepassing vereist. Voor verbinding met een virtuele machine (VM) voor het opsporen van fouten, of voor beheer of administratieve doeleinden, inkomende NAT-regels, een virtuele netwerkgateway of een jumpbox te gebruiken.
   
     Zie voor meer informatie over verbinding maken met virtuele machines:
   
   * [VM's uitvoeren voor een architectuur met meerdere lagen in Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [WinRM-toegang instellen voor virtuele machines in Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Externe toegang tot uw virtuele machine toestaan met behulp van Azure portal](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Externe toegang tot uw virtuele machine toestaan met behulp van PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Externe toegang tot uw Linux-VM geven met behulp van Azure CLI](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)

* De **Domeinnaamlabel** eigenschap voor openbare IP-adressen moet uniek zijn. De **Domeinnaamlabel** waarde moet tussen 3 en 63 tekens lang en volgt u de regels die door deze reguliere expressie opgegeven: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Omdat de **uniqueString** functie genereert een tekenreeks van 13 tekens lang zijn en de **dnsPrefixString** parameter is beperkt tot 50 tekens bevatten:

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

* Wanneer u een wachtwoord aan een extensie voor aangepaste scripts toevoegen, gebruikt u de **commandToExecute** eigenschap in de **protectedSettings** eigenschap:
   
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
   > Om ervoor te zorgen dat er geheimen worden versleuteld wanneer ze als parameters worden doorgegeven aan VM's en -extensies, gebruikt u de **protectedSettings** eigenschap van de relevante extensies.
   > 
   > 

## <a name="outputs"></a>Uitvoer

Als u een sjabloon te maken van openbare IP-adressen gebruiken, voegt een [sectie uitvoer](resource-group-authoring-templates.md#outputs) die retourneert gegevens van het IP-adres en de volledig gekwalificeerde domeinnaam (FQDN). U kunt uitvoerwaarden gebruiken om op te halen eenvoudig meer informatie over openbare IP-adressen en FQDN's na de implementatie.

```json
"outputs": {
    "fqdn": {
        "value": "[reference(parameters('publicIPAddresses_name')).dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]",
        "type": "string"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de structuur van het Resource Manager-sjabloonbestand [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor aanbevelingen over het bouwen van sjablonen die in alle Azure-cloud-omgevingen werken [ontwikkelen van Azure Resource Manager-sjablonen voor de consistentie van de cloud](templates-cloud-consistency.md).
