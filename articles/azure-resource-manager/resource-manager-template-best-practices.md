---
title: Aanbevolen procedures voor het maken van Resource Manager-sjablonen | Microsoft Docs
description: Richtlijnen voor het vereenvoudigen van uw Azure Resource Manager-sjablonen.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen
Deze richtlijnen kunt u bij het maken van Azure Resource Manager-sjablonen die betrouwbaar en eenvoudig te gebruiken zijn. De richtlijnen zijn alleen suggesties. Ze zijn geen vereisten, tenzij anders wordt vermeld. Uw scenario mogelijk een variant van een van de volgende methoden of voorbeelden.

## <a name="resource-names"></a>Resourcenamen
In het algemeen werken u met drie soorten resourcenamen in Resource Manager:

* Resourcenamen moeten uniek zijn.
* Resourcenamen die nodig zijn niet uniek te zijn, maar u kunt een naam waarmee u een resource op basis van context identificeren kunt geven.
* Resourcenamen kunnen niet algemeen zijn.

 Zie voor meer informatie over de beperkingen voor resource [aanbevolen naamgevingsregels voor Azure-resources](../guidance/guidance-naming-conventions.md).

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

Als u een parameter voor een resourcenaam opgeeft, moet u een unieke naam opgeven bij het implementeren van de resource. Desgewenst kunt u een variabele die gebruikmaakt van de [uniqueString()](resource-group-template-functions-string.md#uniquestring) functie voor het genereren van een naam. 

Ook raadzaam een voorvoegsel toevoegen of een achtervoegsel aan de **uniqueString** resultaat. De unieke naam wijzigen, kunt u gemakkelijk herkennen het brontype van de naam. U kunt bijvoorbeeld een unieke naam voor een opslagaccount genereren met behulp van de volgende variabele:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Resourcenamen voor identificatie
Sommige kunt u de naam, maar hun namen van resourcetypen hoeft geen uniek zijn. U kunt een naam waarmee u zowel de context van de bron en het resourcetype opgeven voor deze resourcetypen. Geef een beschrijvende naam waarmee u de resource in een lijst met bronnen identificeren. Als u de naam van een andere bron voor verschillende implementaties gebruiken moet, kunt u een parameter voor de naam:

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

Als u niet een naam opgeeft tijdens de implementatie wilt, kunt u een variabele gebruiken: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

U kunt ook een waarde vastgelegde gebruiken:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
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

## <a name="parameters"></a>Parameters
De volgende informatie kan nuttig zijn wanneer u met parameters werkt:

* Beperk het gebruik van parameters. Gebruik indien mogelijk een variabele of een letterlijke waarde. Parameters gebruiken om alleen voor deze scenario's:
   
   * Instellingen die u wilt gebruiken variaties van de basis van de omgeving (SKU, grootte, capaciteit).
   * Resourcenamen die u wilt opgeven voor eenvoudige identificatie.
   * De waarden die u vaak gebruikt om andere taken (zoals een beheerdersgebruikersnaam) te voltooien.
   * Geheimen (zoals wachtwoorden).
   * Het nummer of de matrix met waarden te gebruiken als u meerdere exemplaren van een resourcetype.
* Gebruik kamelen voor namen van parameters.
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

* Definieer de standaardwaarden voor parameters (met uitzondering van wachtwoorden en SSH-sleutels):
   
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

* Gebruik **SecureString** voor alle wachtwoorden en geheimen: 
   
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

* Gebruik indien mogelijk niet een parameter locatie op te geven. Gebruik in plaats daarvan de **locatie** eigenschap van de resourcegroep. Met behulp van de **resourceGroup () .location** -expressie voor al uw resources resources in de sjabloon zijn geïmplementeerd op dezelfde locatie als de resourcegroep:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Als u een resourcetype wordt ondersteund in een beperkt aantal locaties, is het raadzaam om op te geven van een geldige locatie rechtstreeks in de sjabloon. Als u moet een **locatie** parameter, die zo veel mogelijk parameterwaarde met bronnen die zich waarschijnlijk op dezelfde locatie delen. Hiermee wordt het aantal keren dat gebruikers worden gevraagd om informatie over de locatie geminimaliseerd.
* Vermijd het gebruik van een parameter of variabele voor de API-versie voor een resourcetype. Resource-eigenschappen en waarden kunnen variëren door versienummer. IntelliSense in een code-editor kan niet het juiste schema bepalen wanneer de API-versie is ingesteld op een parameter of variabele. In plaats daarvan harde code de API-versie in de sjabloon.

## <a name="variables"></a>Variabelen
De volgende informatie kan nuttig zijn wanneer u met variabelen werkt:

* Variabelen voor waarden die u moet meer dan één keer gebruiken in een sjabloon gebruiken. Als een waarde slechts één keer gebruikt wordt, een vastgelegde waarde maakt de sjabloon gemakkelijker te lezen.
* U kunt geen gebruiken de [verwijzing](resource-group-template-functions-resource.md#reference) werken in de **variabelen** gedeelte van de sjabloon. De **verwijzing** functie is afgeleid van de waarde van de runtimestatus van de resource. Variabelen zijn echter opgelost bij het eerste parseren van de sjabloon. Constructie waarden die moeten de **verwijzing** werken rechtstreeks in de **resources** of **levert** gedeelte van de sjabloon.
* Variabelen voor de resourcenamen die uniek zijn, zoals beschreven in [Resourcenamen](#resource-names).
* U kunt variabelen groeperen in complexe objecten. Gebruik de **variable.subentry** notatie om te verwijzen naar een waarde van een complex object. Variabelen groeperen, kunt u gerelateerde variabelen volgen. Dit verbetert de leesbaarheid van de sjabloon. Hier volgt een voorbeeld:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Een complex object kan niet een expressie die verwijst naar een waarde van een complex object bevatten. Definieer een afzonderlijke variabele voor dit doel.
   > 
   > 
   
     Zie voor geavanceerde voorbeelden van het gebruik van complexe objecten als variabelen [delen status in Azure Resource Manager-sjablonen](best-practices-resource-manager-state.md).

## <a name="resources"></a>Resources
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

* U kunt tags metagegevens aan resources toevoegen. Informatie over uw resources toevoegen met behulp van de metagegevens. U kunt bijvoorbeeld metagegevens om vast te leggen Factureringsdetails voor een resource toevoegen. Zie voor meer informatie [met labels om uw Azure-resources te organiseren](resource-group-using-tags.md).
* Als u een *openbaar eindpunt* in uw sjabloon (zoals een Azure Blob storage openbaar eindpunt), *komen niet vastleggen* de naamruimte. Gebruik de **verwijzing** functie voor het dynamisch ophalen van de naamruimte. Deze aanpak kunt u de sjabloon kan implementeren op andere openbare naamruimte omgevingen zonder handmatig wijzigen van het eindpunt in de sjabloon. Stel de API-versie naar dezelfde versie die u voor het opslagaccount in de sjabloon gebruikt:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Als het opslagaccount is geïmplementeerd in dezelfde sjabloon die u maakt, hoeft u geen naamruimte van de provider opgeven wanneer u verwijst naar de resource. Dit is de vereenvoudigde syntaxis:
   
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

## <a name="outputs"></a>uitvoer
Als u een sjabloon voor het openbare IP-adressen maken, voegt een **levert** sectie gegevens van het IP-adres en de volledig gekwalificeerde domeinnaam (FQDN retourneert). Uitvoerwaarden kunt u eenvoudig ophalen van informatie over de openbare IP-adressen en FQDN's na de implementatie. Wanneer u verwijst naar de resource, gebruikt u de API-versie die u hebt gebruikt om deze te maken: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Één sjabloon versus geneste sjablonen
Voor het implementeren van uw oplossing, kunt u één sjabloon of een belangrijkste sjabloon kunt gebruiken met meerdere geneste sjablonen. Geneste sjablonen gelden voor meer geavanceerde scenario's. Met behulp van een geneste sjabloon biedt u de volgende voordelen:

* U kunt een oplossing in de betreffende onderdelen uitgesplitst.
* U kunt geneste sjablonen met verschillende belangrijke sjablonen hergebruiken.

Als u ervoor kiest om geneste sjablonen te gebruiken, kunt de volgende richtlijnen u uw sjabloonontwerp standaardiseren. Deze richtlijnen zijn gebaseerd op [patronen voor Azure Resource Manager-sjablonen ontwerpen](best-practices-resource-manager-design-templates.md). U wordt aangeraden een ontwerp dat de volgende sjablonen bevat:

* **Belangrijkste sjabloon** (azuredeploy.json). Gebruik dit voor de invoerparameters.
* **Gedeelde bronnen sjabloon**. Gebruiken voor het implementeren van gedeelde resources die gebruikmaken van alle andere bronnen (bijvoorbeeld virtuele-netwerk en beschikbaarheid sets). Gebruik de **dependsOn** expressie om ervoor te zorgen dat deze sjabloon wordt geïmplementeerd voordat andere sjablonen.
* **Optionele resources sjabloon**. Gebruik voor het implementeren van voorwaardelijk bronnen op basis van een parameter (bijvoorbeeld een jumpbox).
* **Lid resources sjabloon**. Elk exemplaartype binnen een toepassingslaag heeft een eigen configuratie. Binnen een laag, kunt u een ander exemplaar typen definiëren. (Bijvoorbeeld de eerste instantie wordt gemaakt van een cluster en extra exemplaren worden toegevoegd aan het bestaande cluster.) Elk exemplaartype heeft een eigen sjabloon voor de implementatie.
* **Scripts**. Algemeen herbruikbare scripts zijn van toepassing voor elk van exemplaartype (bijvoorbeeld, initialiseren en formatteren extra schijven). Aangepaste scripts die u voor een specifieke aanpassing doel maakt zijn verschillend, op basis van het type.

![Geneste sjabloon](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Zie voor meer informatie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-templates"></a>Voorwaardelijk koppeling naar geneste sjablonen
Een parameter kunt u voorwaardelijk koppelen aan geneste sjablonen. De parameter maakt deel uit van de URI voor de sjabloon:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Sjabloon
Het is raadzaam om door te geven van uw sjabloon via een JSON-validator. Een validatiefunctie kunt u Verwijder overbodige komma's, haakjes en haakjes ertoe leiden een fout opgetreden tijdens de implementatie dat kunnen. Probeer [JSONLint](http://jsonlint.com/) of een pakket linter voor uw favoriete omgeving (Visual Studio Code, Atom, Sublime tekst, Visual Studio) bewerken.

Het is ook een goed idee om uw JSON voor een betere leesbaarheid. Voor uw lokale editor kunt u een pakket van JSON-indeling. In Visual Studio om het document, drukt u op **Ctrl + K, Ctrl + D**. Druk in Visual Studio Code op **Alt + Shift + F**. Als uw lokale editor het document niet opmaken, kunt u een [online formatter](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Volgende stappen
* Zie voor instructies over het aanpassen van uw oplossing voor virtuele machines, [een virtuele machine van Windows worden uitgevoerd in Azure](../guidance/guidance-compute-single-vm.md) en [een Linux-VM in Azure uitvoeren](../guidance/guidance-compute-single-vm-linux.md).
* Zie voor instructies over het instellen van een opslagaccount [controlelijst voor prestaties en schaalbaarheid van Azure Storage](../storage/common/storage-performance-checklist.md).
* Zie voor meer informatie over hoe een onderneming met Resource Manager effectief abonnementen wilt beheren, [Azure enterprise scaffold: Prescriptieve abonnement governance](resource-manager-subscription-governance.md).

