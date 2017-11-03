---
title: Complexe waarden doorgeven tussen Azure-sjablonen | Microsoft Docs
description: Bevat aanbevolen methoden voor het delen van gegevens van de gebruikersstatus met Azure Resource Manager-sjablonen en gekoppelde sjablonen met behulp van complexe objecten.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: e163f3c2e9a78b057dc2a7a42924c59d0aac3fab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a>Status van de share naar en van Azure Resource Manager-sjablonen
Dit onderwerp bevat aanbevolen procedures voor het beheren en delen van de status in sjablonen. De parameters en variabelen die worden weergegeven in dit onderwerp vindt u voorbeelden van het type objecten u kunt gemakkelijk uw implementatievereisten te ordenen. U kunt uw eigen objecten met eigenschapswaarden die geschikt zijn voor uw omgeving implementeren van deze voorbeelden.

In dit onderwerp maakt deel uit van een grotere technisch document. Om te lezen van het volledige papier, downloaden [World klasse Resource Manager-sjablonen overwegingen en procedures bewezen](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Geef de standaardconfiguratie-instellingen
In plaats van een sjabloon die voorziet in totaal flexibiliteit en talloze variaties bieden, wordt een algemene patroon een selectie van bekende configuraties. Gebruikers kunnen in feite standaard t-shirt grootten zoals sandbox, kleine, middelgrote en grote selecteren. Andere voorbeelden van t-shirt grootten zijn de producten, zoals community edition of enterprise edition. In andere gevallen mogelijk werklastspecifiek configuraties van een technologie – zoals kaart verminderen of er zijn geen sql.

U kunt met complexe objecten variabelen maken die verzamelingen van gegevens, ook wel aangeduid als 'Eigenschappenverzamelingen' bevatten en die gegevens gebruiken om de resource-declaratie in uw sjabloon. Deze aanpak geeft goede, bekende configuraties van verschillende grootten die vooraf zijn geconfigureerd voor klanten. Zonder bekende configuraties moeten gebruikers van de sjabloon cluster formaat op hun eigen bepalen, rekening te houden in resourcebeperkingen platform en rekenen om te identificeren van de resulterende partitionering van storage-accounts en andere bronnen (vanwege beperkingen van cluster grootte en resource). Naast het maken van een betere ervaring voor de klant, enkele bekende configuraties zijn gemakkelijker te ondersteunen en kunt u een hoger niveau van de dichtheid leveren.

Het volgende voorbeeld laat zien hoe variabelen met complexe objecten voor het voorstellen van verzamelingen van gegevens definiëren. De verzamelingen definiëren waarden die worden gebruikt voor de grootte van virtuele machine, netwerkinstellingen, besturingssysteeminstellingen en instellingen voor beschikbaarheid.

```json
"variables": {
  "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  },
  "tshirtSizeMedium": {
    "vmSize": "Standard_A3",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1],
      "jumpbox": 0
    }
  },
  "tshirtSizeLarge": {
    "vmSize": "Standard_A4",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
    "vmCount": 3,
    "slaveCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1,1],
      "jumpbox": 0
    }
  },
  "osSettings": {
    "scripts": [
      "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
    ],
    "imageReference": {
      "publisher": "Canonical",
      "offer": "UbuntuServer",
      "sku": "14.04.2-LTS",
      "version": "latest"
    }
  },
  "networkSettings": {
    "vnetName": "[parameters('virtualNetworkName')]",
    "addressPrefix": "10.0.0.0/16",
    "subnets": {
      "dmz": {
        "name": "dmz",
        "prefix": "10.0.0.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      },
      "data": {
        "name": "data",
        "prefix": "10.0.1.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      }
    }
  },
  "availabilitySetSettings": {
    "name": "pgsqlAvailabilitySet",
    "fdCount": 3,
    "udCount": 5
  }
}
```

U ziet dat de **tshirtSize** variabele kan de grootte van de t-shirt die u hebt opgegeven via een parameter toevoegen (**kleine**, **gemiddeld**, **grote**) en de tekst **tshirtSize**. U kunt deze variabele gebruiken om op te halen van de bijbehorende complexe objectvariabele voor die t-shirt-grootte.

U kunt vervolgens verwijzen naar deze variabelen verderop in de sjabloon. De mogelijkheid om te verwijzen met de naam van de variabelen en hun eigenschappen vereenvoudigt de sjabloonsyntaxis van de en kunt u gemakkelijk om context te begrijpen. Het volgende voorbeeld definieert een resourcegroep implementeren met behulp van de objecten die eerder weergegeven waarden in te stellen. Bijvoorbeeld, de VM-grootte is ingesteld door op te halen van de waarde voor `variables('tshirtSize').vmSize` terwijl de waarde voor de schijfgrootte is opgehaald uit `variables('tshirtSize').diskSize`. Bovendien de URI voor een gekoppelde sjabloon is ingesteld met de waarde voor `variables('tshirtSize').vmTemplate`.

```json
"name": "master-node",
"type": "Microsoft.Resources/deployments",
"apiVersion": "2015-01-01",
"dependsOn": [
    "[concat('Microsoft.Resources/deployments/', 'shared')]"
],
"properties": {
    "mode": "Incremental",
    "templateLink": {
      "uri": "[variables('tshirtSize').vmTemplate]",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "value": "[parameters('adminPassword')]"
      },
      "replicatorPassword": {
        "value": "[parameters('replicatorPassword')]"
      },
      "osSettings": {
        "value": "[variables('osSettings')]"
      },
      "subnet": {
        "value": "[variables('networkSettings').subnets.data]"
      },
      "commonSettings": {
        "value": {
          "region": "[parameters('region')]",
          "adminUsername": "[parameters('adminUsername')]",
          "namespace": "ms"
        }
      },
      "storageSettings": {
        "value":"[variables('tshirtSize').storage]"
      },
      "machineSettings": {
        "value": {
          "vmSize": "[variables('tshirtSize').vmSize]",
          "diskSize": "[variables('tshirtSize').diskSize]",
          "vmCount": 1,
          "availabilitySet": "[variables('availabilitySetSettings').name]"
        }
      },
      "masterIpAddress": {
        "value": "0"
      },
      "dbType": {
        "value": "MASTER"
      }
    }
  }
}
```

## <a name="pass-state-to-a-template"></a>Status doorgeven aan een sjabloon
U delen staat in een sjabloon via parameters die u tijdens de implementatie opgeeft.

De volgende tabel bevat de meest gebruikte parameters in sjablonen.

| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| location |De tekenreeks in een beperkte lijst met Azure-regio 's |De locatie waar de resources worden geïmplementeerd. |
| storageAccountNamePrefix |Tekenreeks |Unieke DNS-naam voor het Opslagaccount waarin de VM-schijven worden geplaatst |
| Domeinnaam |Tekenreeks |Domeinnaam van het openbaar toegankelijk jumpbox VM in de indeling: **{domainName}. { Location}.cloudapp.com** bijvoorbeeld: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Tekenreeks |Gebruikersnaam voor de virtuele machines |
| adminPassword |Tekenreeks |Wachtwoord voor de virtuele machines |
| tshirtSize |Tekenreeks van een beperkte lijst aangeboden t-shirt grootten |De grootte van de eenheid benoemde schalen om in te richten. Bijvoorbeeld 'Klein', 'Gemiddeld', 'Groot' |
| virtualNetworkName |Tekenreeks |Naam van het virtuele netwerk dat de gebruiker wil gebruiken. |
| enableJumpbox |De tekenreeks in een beperkte lijst (ingeschakeld/uitgeschakeld) |De parameter die aangeeft of een jumpbox voor de omgeving wordt ingeschakeld. Waarden: "ingeschakeld", "uitgeschakeld" |

De **tshirtSize** parameter die wordt gebruikt in de vorige sectie is gedefinieerd als:

```json
"parameters": {
  "tshirtSize": {
    "type": "string",
    "defaultValue": "Small",
    "allowedValues": [
      "Small",
      "Medium",
      "Large"
    ],
    "metadata": {
      "Description": "T-shirt size of the MongoDB deployment"
    }
  }
}
```

## <a name="pass-state-to-linked-templates"></a>Status geven aan gekoppelde sjablonen
Bij het verbinden met gekoppelde sjablonen, vaak gebruik van een combinatie van statische en variabelen gegenereerd.

### <a name="static-variables"></a>Statische variabelen
Statische variabelen worden vaak gebruikt base waarden, zoals URL's die worden gebruikt in een sjabloon op te geven.

In het volgende fragment van de sjabloon `templateBaseUrl` geeft de hoofdmaplocatie voor de sjabloon in GitHub. De volgende regel maakt een nieuwe variabele `sharedTemplateUrl` die de basis-URL met de bekende naam van de sjabloon gedeelde bronnen worden aaneengeschakeld. Onder deze regel een variabele complexe object wordt gebruikt voor het opslaan van de grootte van een t-shirt, waarbij de basis-URL naar de locatie van de sjabloon bekende configuratie samengevoegd en opgeslagen in de `vmTemplate` eigenschap.

Het voordeel van deze benadering is dat als de sjabloonlocatie wijzigt, u alleen hoeft de statische variabele op één plek die wordt doorgegeven in de gekoppelde sjablonen te wijzigen.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "slaveCount": 1,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  }
}
```

### <a name="generated-variables"></a>Variabelen voor de gegenereerde
Naast statische variabelen worden verschillende variabelen dynamisch gegenereerd. Deze sectie worden enkele van de algemene typen gegenereerde variabelen.

#### <a name="tshirtsize"></a>tshirtSize
U bent bekend met deze gegenereerde variabele van de bovenstaande voorbeelden.

#### <a name="networksettings"></a>networkSettings
In een capaciteit, de mogelijkheid of de end-to-end bereik oplossingssjabloon maken de gekoppelde sjablonen meestal resources die bestaan op een netwerk. Een eenvoudige benadering is het gebruik van een complex object op te slaan netwerkinstellingen en aan de gekoppelde sjablonen door te geven.

Hieronder ziet u een voorbeeld van netwerkinstellingen communiceren.

```json
"networkSettings": {
  "vnetName": "[parameters('virtualNetworkName')]",
  "addressPrefix": "10.0.0.0/16",
  "subnets": {
    "dmz": {
      "name": "dmz",
      "prefix": "10.0.0.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    },
    "data": {
      "name": "data",
      "prefix": "10.0.1.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    }
  }
}
```

#### <a name="availabilitysettings"></a>availabilitySettings
Resources die zijn gemaakt in de gekoppelde sjablonen worden vaak geplaatst in een beschikbaarheidsset. In het volgende voorbeeld wordt de naam van de beschikbaarheidsset is opgegeven en ook het domein met fouten en het updatedomein voor het gebruik van tellen.

```json
"availabilitySetSettings": {
  "name": "pgsqlAvailabilitySet",
  "fdCount": 3,
  "udCount": 5
}
```

Als u meerdere beschikbaarheidssets (bijvoorbeeld één voor hoofdknooppunten) en een andere voor gegevensknooppunten, kunt u een naam op als een voorvoegsel moet, meerdere beschikbaarheidssets opgeven of het model eerder weergegeven voor het maken van een variabele voor een specifieke t-shirt grootte volgen.

#### <a name="storagesettings"></a>storageSettings
Details van de opslag worden vaak gedeeld met gekoppelde sjablonen. In het voorbeeld hieronder, een *storageSettings* object bevat informatie over de namen van de storage-account en de container.

```json
"storageSettings": {
    "vhdStorageAccountName": "[parameters('storageAccountName')]",
    "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
    "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
}
```

#### <a name="ossettings"></a>osSettings
Met gekoppelde sjablonen, moet u wellicht besturingssysteeminstellingen doorgeven aan verschillende knooppunten in verschillende andere configuratie voor bekende typen. Een complex object is een eenvoudige manier voor het opslaan en delen van informatie over het besturingssysteem en is het eenvoudiger voor de ondersteuning van meerdere besturingssystemen kiezen voor implementatie.

Het volgende voorbeeld ziet u een object voor *osSettings*:

```json
"osSettings": {
  "imageReference": {
    "publisher": "Canonical",
    "offer": "UbuntuServer",
    "sku": "14.04.2-LTS",
    "version": "latest"
  }
}
```

#### <a name="machinesettings"></a>machineSettings
Een gegenereerde variabele *machineSettings* is een complex object met een combinatie van core variabelen voor het maken van een virtuele machine. De variabelen zijn gebruikersnaam en wachtwoord, een voorvoegsel voor de VM-namen en een verwijzing naar het afbeelding van het besturingssysteem.

```json
"machineSettings": {
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "machineNamePrefix": "mongodb-",
    "osImageReference": {
        "publisher": "[variables('osFamilySpec').imagePublisher]",
        "offer": "[variables('osFamilySpec').imageOffer]",
        "sku": "[variables('osFamilySpec').imageSKU]",
        "version": "latest"
    }
},
```

Houd er rekening mee dat *osImageReference* haalt de waarden van de *osSettings* variabele gedefinieerd in de belangrijkste sjabloon. Dit betekent dat u kunt gemakkelijk het besturingssysteem wijzigen voor een VM-geheel of op basis van de voorkeur van de consument van een sjabloon.

#### <a name="vmscripts"></a>vmScripts
De *vmScripts* object bevat informatie over de scripts voor het downloaden en uitvoeren op een VM-instantie, inclusief externe en interne verwijzingen. Verwijzingen zijn buiten de infrastructuur.
Binnen verwijzingen bevatten de geïnstalleerde software die is geïnstalleerd en de configuratie.

U gebruikt de *scriptsToDownload* eigenschap voor een lijst met de scripts voor het downloaden van de virtuele machine. Dit object bevat ook verwijzingen naar opdrachtregelargumenten voor verschillende soorten acties. Deze acties omvatten het uitvoeren van de standaardinstallatie voor elke afzonderlijke knooppunten, een installatie die wordt uitgevoerd nadat alle knooppunten worden geïmplementeerd en eventuele extra scripts die specifiek zijn voor een bepaalde sjabloon.

In dit voorbeeld is van een sjabloon die wordt gebruikt voor het implementeren van MongoDB, waarvoor een instellingen voor het leveren van hoge beschikbaarheid. De *arbiterNodeInstallCommand* is toegevoegd aan *vmScripts* voor het installeren van de instellingen.

De sectie met sjabloonvariabelen is waar het vinden van de variabelen die de specifieke tekst voor het uitvoeren van het script met de juiste waarden te definiëren.

```json
"vmScripts": {
    "scriptsToDownload": [
        "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
        "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
    ],
    "regularNodeInstallCommand": "[variables('installCommand')]",
    "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
    "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
},
```

## <a name="return-state-from-a-template"></a>Geretourneerde status van een sjabloon
Niet alleen kunt u gegevens kunt doorgeven in een sjabloon kunt u ook gegevens terug naar de aanroepende sjabloon delen. In de **levert** gedeelte van een gekoppelde sjabloon, kunt u sleutel-waardeparen die kunnen worden gebruikt door de bronsjabloon opgeven.

Het volgende voorbeeld laat zien hoe de privé IP-adres in een gekoppelde sjabloon gegenereerd doorgeven.

```json
"outputs": {
    "masterip": {
        "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
        "type": "string"
     }
}
```

In de belangrijkste sjabloon kunt u die gegevens gebruiken met de volgende syntaxis:

    "[reference('master-node').outputs.masterip.value]"

U kunt deze expressie in de sectie uitvoer of de sectie resources van de belangrijkste sjabloon gebruiken. U kunt de expressie in het gedeelte variabelen niet gebruiken omdat deze afhankelijk van de runtimestatus van de is. U kunt deze waarde van de belangrijkste sjabloon gebruiken:

```json
"outputs": {
  "masterIpAddress": {
    "value": "[reference('master-node').outputs.masterip.value]",
    "type": "string"
  }
```

Zie voor een voorbeeld van het gebruik van de uitvoer-gedeelte van een gekoppelde sjabloon om terug te keren gegevensschijven voor een virtuele machine [meerdere gegevensschijven maken voor een virtuele Machine](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Verificatie-instellingen voor virtuele machine definiëren
U kunt hetzelfde patroon eerder weergegeven voor configuratie-instellingen gebruiken om op te geven van de verificatie-instellingen voor een virtuele machine. U maakt een parameter voor het doorgeven in het type verificatie.

```json
"parameters": {
  "authenticationType": {
    "allowedValues": [
      "password",
      "sshPublicKey"
    ],
    "defaultValue": "password",
    "metadata": {
      "description": "Authentication type"
    },
    "type": "string"
  }
}
```

Toevoegen van variabelen voor de verschillende typen en een variabele voor het opslaan van welk type voor deze implementatie op basis van de waarde van de parameter wordt gebruikt.

```json
"variables": {
  "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
  "osProfilepassword": {
    "adminPassword": "[parameters('adminPassword')]",
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]"
  },
  "osProfilesshPublicKey": {
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]",
    "linuxConfiguration": {
      "disablePasswordAuthentication": "true",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "[parameters('sshPublicKey')]",
            "path": "/home/notused/.ssh/authorized_keys"
          }
        ]
      }
    }
  }
}
```

Bij het definiëren van de virtuele machine, stelt u de **osProfile** aan de variabele die u hebt gemaakt.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  ...
  "osProfile": "[variables('osProfile')]"
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de secties van de sjabloon, [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md)
* Zie voor de functies die beschikbaar in een sjabloon zijn [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md)
