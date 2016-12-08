---
title: Een Azure Resource Manager-sjabloon exporteren | Microsoft Docs
description: Gebruik Azure Resource Manager om een sjabloon uit een bestaande resourcegroep te exporteren.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/20/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: 4f1e8850aee2cc9578ce80ceb4a5eecf121c4c60


---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Een Azure Resource Manager-sjabloon uit bestaande resources exporteren
Met Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

Het is belangrijk te weten dat er twee verschillende manieren zijn om een sjabloon te exporteren:

* U kunt de daadwerkelijke sjabloon dat u voor een implementatie hebt gebruikt, exporteren. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u resources via de portal hebt geïmplementeerd. Nu krijgt u te zien hoe u de sjabloon moet opbouwen voor het maken van die resources.
* U kunt een sjabloon exporteren met de huidige status van de resourcegroep. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor de implementatie hebt gebruikt. In plaats daarvan wordt er een sjabloon gemaakt die een momentopname is van de resourcegroep. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Deze methode is nuttig wanneer u de resourcegroep via de portal of scripts hebt gewijzigd. U moet nu de resourcegroep vastleggen als sjabloon.

Dit onderwerp bevat beide methoden.

In deze zelfstudie meldt u zich aan bij Azure Portal, maakt u een opslagaccount en exporteert u de sjabloon voor dat opslagaccount. U voegt een virtueel netwerk toe om de resourcegroep te wijzigen. Ten slotte exporteert u een nieuwe sjabloon die de huidige status weergeeft. Hoewel dit artikel gericht is op een vereenvoudigde infrastructuur, kunt u deze zelfde stappen gebruiken om een sjabloon voor een gecompliceerdere oplossing te exporteren.

## <a name="create-a-storage-account"></a>Een opslagaccount maken
1. Selecteer in [Azure Portal](https://portal.azure.com) **Nieuw** > **Opslag** > **Opslagaccount**.
   
      ![opslag maken](./media/resource-manager-export-template/create-storage.png)
2. Maak een opslagaccount met de naam **opslag**, uw initialen en de datum. De naam van het opslagaccount moet uniek zijn in Azure. Als de naam al in gebruik is, wordt dit in een foutbericht meegedeeld. Probeer een variant. Maak voor de resourcegroep een nieuwe resourcegroep en noem deze **ExportGroup**. Voor de overige eigenschappen kunt u de standaardwaarden gebruiken. Selecteer **Maken**.
   
      ![Waarden opgeven voor opslag](./media/resource-manager-export-template/provide-storage-values.png)

De implementatie kan een minuut duren. Nadat de implementatie is voltooid, bevat uw abonnement het opslagaccount.

## <a name="view-a-template-from-deployment-history"></a>De sjabloon weergeven vanuit de implementatiegeschiedenis
1. Ga naar de resourcegroepblade van uw nieuwe resourcegroep. U ziet dat het resultaat van de laatste implementatie op de blade wordt vermeld. Selecteer deze koppeling.
   
      ![resourcegroepblade](./media/resource-manager-export-template/resource-group-blade.png)
2. U ziet nu de geschiedenis van de implementaties voor de groep. In uw geval wordt er op de blade waarschijnlijk slechts één implementatie vermeld. Selecteer deze implementatie.
   
     ![laatste implementatie](./media/resource-manager-export-template/last-deployment.png)
3. In de blade wordt een samenvatting van de implementatie weergegeven. Deze samenvatting bevat de status van de implementatie en bewerkingen, en de waarden die u hebt opgegeven voor de parameters. Selecteer **Sjabloon weergeven** om de sjabloon te bekijken die u voor de implementatie hebt gebruikt.
   
     ![implementatiesamenvatting bekijken](./media/resource-manager-export-template/deployment-summary.png)
4. Resource Manager haalt de volgende zes bestanden voor u op:
   
   1. **Sjabloon**: de sjabloon die de infrastructuur voor uw oplossing definieert. Toen u het opslagaccount via de portal maakte, heeft Resource Manager een sjabloon gebruikt om het te implementeren. De sjabloon is opgeslagen voor toekomstig gebruik.
   2. **Parameters**: een parameterbestand dat u kunt gebruiken om tijdens de implementatie waarden door te geven. Dit bestand bevat de waarden die u hebt opgegeven tijdens de eerste implementatie. Deze waarden kunt u echter wijzigen wanneer u de sjabloon opnieuw implementeert.
   3. **CLI**: een Azure CLI-scriptbestand dat u kunt gebruiken om de sjabloon te implementeren.
   4. **PowerShell**: een Azure PowerShell-scriptbestand dat u kunt gebruiken om de sjabloon te implementeren.
   5. **.NET**: een .NET-klasse die u kunt gebruiken om de sjabloon te implementeren.
   6. **Ruby**: een Ruby-klasse die u kunt gebruiken om de sjabloon te implementeren.
      
      De bestanden zijn beschikbaar via de koppelingen op de blade. In de blade wordt standaard de sjabloon weergegeven.
      
       ![sjabloon weergeven](./media/resource-manager-export-template/view-template.png)
      
      Laten we wat aandachtiger naar de sjabloon kijken. De sjabloon moet er ongeveer zo uitzien:
      
        {
      
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "name": {
              "type": "String"
            },
            "accountType": {
              "type": "String"
            },
            "location": {
              "type": "String"
            },
            "encryptionEnabled": {
              "defaultValue": false,
              "type": "Bool"
            }
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "sku": {
                "name": "[parameters('accountType')]"
              },
              "kind": "Storage",
              "name": "[parameters('name')]",
              "apiVersion": "2016-01-01",
              "location": "[parameters('location')]",
              "properties": {
                "encryption": {
                  "services": {
                    "blob": {
                      "enabled": "[parameters('encryptionEnabled')]"
                    }
                  },
                  "keySource": "Microsoft.Storage"
                }
              }
            }
          ]
        }

Deze sjabloon is de daadwerkelijke sjabloon die is gebruikt voor het maken van uw opslagaccount. Deze sjabloon bevat parameters waarmee u verschillende soorten opslagaccounts kunt implementeren. Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de structuur van een sjabloon. Zie [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md) voor de volledige lijst met functies die u in een sjabloon kunt gebruiken.

## <a name="add-a-virtual-network"></a>Een virtueel netwerk toevoegen
De sjabloon die u in de vorige sectie hebt gedownload, weerspiegelde de infrastructuur voor die oorspronkelijke implementatie. Er wordt echter geen rekening gehouden met eventuele wijzigingen na de implementatie.
Ter illustratie van dit probleem gaan we de resourcegroep wijzigen door via de portal een virtueel netwerk toe te voegen.

1. Selecteer **Toevoegen** op de blade van de resourcegroep.
   
      ![resource toevoegen](./media/resource-manager-export-template/add-resource.png)
2. Selecteer **Virtueel netwerk** uit de beschikbare resources.
   
      ![virtueel netwerk selecteren](./media/resource-manager-export-template/select-vnet.png)
3. Geef het virtuele netwerk de naam **VNET** en gebruik de standaardwaarden voor de overige eigenschappen. Selecteer **Maken**.
   
      ![waarschuwing instellen](./media/resource-manager-export-template/create-vnet.png)
4. Nadat het virtuele netwerk in de resourcegroep is geïmplementeerd, bekijkt u de geschiedenis van de implementatie opnieuw. U ziet nu twee implementaties. Als u de tweede implementatie niet ziet, moet u mogelijk uw resourcegroep-blade sluiten en opnieuw openen. Selecteer de meest recente implementatie.
   
      ![implementatiegeschiedenis](./media/resource-manager-export-template/deployment-history.png)
5. Bekijk de sjabloon voor die implementatie. Hiermee wordt alleen het virtuele netwerk gedefinieerd. Het omvat niet het opslagaccount dat u eerder hebt geïmplementeerd. U hebt geen sjabloon meer die alle resources in uw resourcegroep vertegenwoordigt.

## <a name="export-the-template-from-resource-group"></a>De sjabloon vanuit de resourcegroep exporteren
Als u de huidige status van uw resourcegroep wilt weten, exporteert u een sjabloon die een momentopname van de resourcegroep laat zien.  

> [!NOTE]
> U kunt geen sjablonen voor een resourcegroep met meer dan 200 bronnen exporteren.
> 
> 

1. Selecteer **Automatiseringsscript** om de sjabloon voor een resourcegroep te bekijken.
   
      ![resourcegroep exporteren](./media/resource-manager-export-template/export-resource-group.png)
   
     Niet alle resourcetypen ondersteunen de functie voor het exporteren van sjablonen. Als de resourcegroep alleen het opslagaccount en de virtuele netwerken bevat die in dit artikel worden vermeld, ziet u geen foutmelding. Als u echter andere resourcetypen hebt gemaakt, wordt er mogelijk een foutmelding weergegeven dat er een probleem is met de export. U krijgt meer informatie over het afhandelen van die problemen in de sectie [Problemen met exports oplossen](#fix-export-issues).
2. Weer ziet u de zes bestanden die u kunt gebruiken om de oplossing opnieuw te implementeren, maar deze keer is de sjabloon enigszins anders. Deze sjabloon heeft slechts twee parameters: één voor de opslagaccountnaam en één voor de virtuele-netwerknaam.
   
        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },
   
     Resource Manager heeft de sjablonen die u tijdens de implementatie hebt gebruikt niet opgehaald. In plaats daarvan heeft Resource Manager een nieuwe sjabloon gegenereerd die is gebaseerd op de huidige configuratie van de resources. De sjabloon stelt de locatie- en replicatiewaarde voor het opslagaccount bijvoorbeeld in op:
   
        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },
3. U hebt een aantal opties om verder te werken met deze sjabloon. U kunt de sjabloon downloaden en er lokaal mee werken met een JSON-editor. Of u kunt de sjabloon opslaan in uw bibliotheek en er mee werken via de portal.
   
     Als u ervaring hebt met een JSON-editor, zoals [VS Code](resource-manager-vs-code.md) of [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), wilt u de sjabloon misschien liever lokaal downloaden en die editor gebruiken. Als u geen JSON-editor hebt geïnstalleerd, wilt u de sjabloon misschien liever via de portal bewerken. Hieronder wordt ervan uitgegaan dat u de sjabloon hebt opgeslagen in uw bibliotheek in de portal. U brengt echter dezelfde syntaxiswijzigingen aan in de sjabloon, ongeacht of u lokaal met een JSON-editor werkt of via de portal.
   
     Selecteer **Downloaden** als u lokaal wilt werken.
   
      ![sjabloon downloaden](./media/resource-manager-export-template/download-template.png)
   
     Selecteer **Toevoegen aan bibliotheek** als u via de portal wilt werken.
   
      ![toevoegen aan bibliotheek](./media/resource-manager-export-template/add-to-library.png)
   
     Als u een sjabloon toevoegt aan de bibliotheek, geeft u de sjabloon een naam en een beschrijving. Selecteer vervolgens **Opslaan**.
   
     ![sjabloonwaarden instellen](./media/resource-manager-export-template/set-template-values.png)
4. Als u een sjabloon wilt weergeven die is opgeslagen in uw bibliotheek, selecteert u **Meer services**, typt u **sjablonen** om de resultaten te filteren, en selecteert u **Sjablonen**.
   
      ![sjablonen zoeken](./media/resource-manager-export-template/find-templates.png)
5. Selecteer de sjabloon met de naam die u hebt opgeslagen.
   
      ![sjabloon selecteren](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>De sjabloon aanpassen
De geëxporteerde sjabloon werkt prima als u voor elke implementatie hetzelfde opslagaccount en virtuele netwerk wilt maken. Resource Manager biedt echter opties die u in staat stellen sjablonen met veel meer flexibiliteit te implementeren. Tijdens de implementatie wilt u bijvoorbeeld mogelijk het type opslagaccount opgeven dat moet worden gemaakt of de waarden die moeten worden gebruikt voor het adresvoorvoegsel en het subnetvoorvoegsel van het virtuele netwerk.

In deze sectie voegt u parameters toe aan de geëxporteerde sjabloon, zodat u de sjabloon opnieuw kunt gebruiken wanneer u deze resources in andere omgevingen implementeert. Ook voegt u bepaalde functies aan de sjabloon toe om de kans te verminderen dat er een fout optreedt wanneer u de sjabloon implementeert. U hoeft niet langer een unieke naam voor uw opslagaccount te bedenken. In plaats daarvan maakt de sjabloon een unieke naam. U beperkt de waarden die voor het type opslagaccount kunnen worden opgegeven, tot uitsluitend geldige opties.

1. Selecteer **Bewerken** om de sjabloon aan te passen.
   
     ![sjabloon weergeven](./media/resource-manager-export-template/show-template.png)
2. Selecteer de sjabloon.
   
     ![sjabloon bewerken](./media/resource-manager-export-template/edit-template.png)
3. Om de waarden te kunnen gebruiken die u tijdens de implementatie misschien wilt opgeven, vervangt u de sectie **parameters** door nieuwe parameterdefinities. Let op de waarden van **allowedValues** voor **storageAccount_accountType**. Als u per ongeluk een ongeldige waarde opgeeft, wordt deze fout herkend voordat de implementatie start. Merk ook op dat u alleen een voorvoegsel voor de opslagaccountnaam opgeeft en het voorvoegsel tot 11 tekens is beperkt. Door het voorvoegsel tot 11 tekens te beperken, zorgt u ervoor dat de volledige naam niet langer is dan het maximum aantal tekens voor een opslagaccount. Met behulp van het voorvoegsel kunt u een naamgevingsconventie toepassen op uw opslagaccounts. In de volgende stap ziet u hoe u een unieke naam kunt maken.
   
        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },
4. De sectie met **variabelen** van uw sjabloon is momenteel leeg. In de sectie met **variabelen** kunt u waarden maken die de syntaxis voor de rest van de sjabloon vereenvoudigen. Vervang deze sectie door een nieuwe variabeledefinitie. De variabele **storageAccount_name** voegt het voorvoegsel van de parameter samen tot een unieke tekenreeks die wordt gegenereerd op basis van de id van de resourcegroep. U hoeft niet langer een unieke naam te bedenken wanneer u een parameterwaarde opgeeft.
   
        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },
5. Als u de parameters en de variabele in de resourcedefinities wilt gebruiken, vervangt u de sectie **resources** door nieuwe resourcedefinities. U ziet dat er, afgezien van de waarde die aan de resource-eigenschap is toegewezen, maar weinig is gewijzigd in de resourcedefinities. De eigenschappen zijn hetzelfde als de eigenschappen van de geëxporteerde sjabloon. U wijst eenvoudigweg eigenschappen toe aan de parameterwaarden in plaats van aan de vastgelegde waarden. De locatie van de resources is ingesteld voor gebruik van dezelfde locatie als de resourcegroep via de expressie **resourceGroup () .location**. Naar de variabele die u voor de opslagaccountnaam hebt gemaakt, wordt verwezen via de expressie **variabelen**.
   
        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]
6. Selecteer **OK** wanneer u klaar bent om de sjabloon te bewerken.
7. Klik op **Opslaan** om de wijzigingen in de sjabloon op te slaan.
   
     ![sjabloon opslaan](./media/resource-manager-export-template/save-template.png)
8. Selecteer **Implementeren** om de bijgewerkte sjabloon te implementeren.
   
     ![sjabloon implementeren](./media/resource-manager-export-template/deploy-template.png)
9. Geef parameterwaarden op en selecteer een nieuwe resourcegroep waarin u de resources wilt implementeren.

## <a name="update-the-downloaded-parameters-file"></a>Het gedownloade parameterbestand bijwerken
Als u met de gedownloade bestanden werkt (en niet met de portal-bibliotheek), moet u het gedownloade parameterbestand bijwerken. Het komt immers niet meer overeen met de parameters in de sjabloon. U hoeft geen parameterbestand te gebruiken, maar dit kan het proces vereenvoudigen bij het opnieuw implementeren van een omgeving. Voor veel van de parameters gebruikt u de standaardwaarden die in de sjabloon zijn gedefinieerd, zodat uw parameterbestand maar twee waarden nodig heeft.

Vervang de inhoud van het bestand parameters.json door:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Het bijgewerkte parameterbestand biedt alleen waarden voor parameters die geen standaardwaarde hebben. Voor de andere parameters kunt u waarden opgeven als u een waarde wilt die verschilt van de standaardwaarde.

## <a name="fix-export-issues"></a>Problemen met exports oplossen
Niet alle resourcetypen ondersteunen de functie voor het exporteren van sjablonen. Resource Manager exporteert specifiek sommige resourcetypen niet om te voorkomen dat gevoelige gegevens zichtbaar worden. Als u bijvoorbeeld een verbindingsreeks in de configuratie van uw site hebt, wilt u deze waarschijnlijk niet expliciet weergeven in een geëxporteerde sjabloon. U kunt dit probleem omzeilen door handmatig de ontbrekende resources terug te zetten in de sjabloon.

> [!NOTE]
> Exporteren geeft alleen problemen bij het exporteren vanuit een resourcegroep, niet bij het exporteren vanuit de geschiedenis van uw implementatie. Als uw laatste implementatie een nauwkeurige weergave is van de huidige status van de resourcegroep, kunt u het beste de sjabloon exporteren uit de implementatiegeschiedenis in plaats van uit de resourcegroep. Exporteer alleen vanuit een resourcegroep wanneer u wijzigingen hebt aangebracht aan de resourcegroep die niet zijn gedefinieerd in een sjabloon.
> 
> 

Als u bijvoorbeeld een sjabloon exporteert voor een resourcegroep met een web-app, SQL Database en een verbindingsreeks in de site-configuratie, ziet u het volgende bericht:

![fout weergeven](./media/resource-manager-export-template/show-error.png)

Wanneer u het bericht selecteert, ziet u precies welke resourcetypen niet zijn geëxporteerd. 

![fout weergeven](./media/resource-manager-export-template/show-error-details.png)

Dit onderwerp bevat algemene oplossingen.

### <a name="connection-string"></a>Verbindingsreeks
Voeg in de websiteresource een definitie voor de verbindingsreeks toe aan de database:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Website-extensie
Voeg in de websiteresource een definitie toe voor de te installeren code:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Extensie voor de virtuele machine
Zie voor meer voorbeelden van extensies voor de virtuele machine [Voorbeelden van Azure Windows VM-extensieconfiguratie](../virtual-machines/virtual-machines-windows-extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="virtual-network-gateway"></a>Gateway van een virtueel netwerk
Voeg een resourcetype voor de gateway van het virtuele netwerk toe.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Lokale netwerkgateway
Voeg een resourcetype voor de gateway van het lokale netwerk toe.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Verbinding
Voeg een resourcetype voor de verbinding toe.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd. U hebt geleerd hoe u een sjabloon kunt exporteren uit resources die u in de portal hebt gemaakt.

* U kunt een sjabloon implementeren via [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) of [REST API](resource-group-template-deploy-rest.md).
* Zie [Azure PowerShell gebruiken met Azure Resource Manager](powershell-azure-resource-manager.md) voor het exporteren van een sjabloon via PowerShell.
* Zie [De Azure CLI voor Mac, Linux en Windows gebruiken met Azure Resource Manager](xplat-cli-azure-resource-manager.md) voor het exporteren van een sjabloon via Azure CLI.




<!--HONumber=Nov16_HO3-->


