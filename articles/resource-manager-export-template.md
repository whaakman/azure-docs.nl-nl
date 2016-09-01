<properties
    pageTitle="Een Azure Resource Manager-sjabloon exporteren | Microsoft Azure"
    description="Gebruik Azure Resource Manager om een sjabloon uit een bestaande resourcegroep te exporteren."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/03/2016"
    ms.author="tomfitz"/>

# Een Azure Resource Manager-sjabloon uit bestaande resources exporteren

Met Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

Het is belangrijk te weten dat er twee verschillende manieren zijn om een sjabloon te exporteren:

- U kunt de daadwerkelijke sjabloon dat u voor een implementatie hebt gebruikt, exporteren. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u resources via de portal hebt geïmplementeerd. Nu krijgt u te zien hoe u de sjabloon moet opbouwen voor het maken van die resources.
- U kunt een sjabloon exporteren met de huidige status van de resourcegroep. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor de implementatie hebt gebruikt. In plaats daarvan wordt er een sjabloon gemaakt die een momentopname is van de resourcegroep. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Deze methode is nuttig wanneer u de resourcegroep via de portal of scripts hebt gewijzigd. U moet nu de resourcegroep vastleggen als sjabloon.

Dit onderwerp bevat beide methoden. In het artikel [Een geëxporteerde Azure Resource Manager-sjabloon aanpassen](resource-manager-customize-template.md) leest u hoe u een sjabloon die u hebt gegenereerd op basis van de huidige status van de resourcegroep kunt pakken en deze nuttiger kunt maken voor het opnieuw implementeren van uw oplossing.

In deze zelfstudie meldt u zich aan bij de Azure Portal, maakt u een opslagaccount en exporteert u de sjabloon voor dat opslagaccount. U voegt een virtueel netwerk toe om de resourcegroep te wijzigen. Ten slotte exporteert u een nieuwe sjabloon die de huidige status weergeeft. Hoewel dit artikel gericht is op een vereenvoudigde infrastructuur, kunt u deze zelfde stappen gebruiken om een sjabloon voor een gecompliceerdere oplossing te exporteren.

## Een opslagaccount maken

1. Selecteer in de [Azure Portal](https://portal.azure.com) **Nieuw** > **Gegevens en opslag** > **Opslagaccount**.

      ![opslag maken](./media/resource-manager-export-template/create-storage.png)

2. Maak een opslagaccount met de naam **opslag**, uw initialen en de datum. De naam van het opslagaccount moet uniek zijn in Azure. Als u in eerste instantie een naam opgeeft die al wordt gebruikt, probeert u een variant. Gebruik voor de resourcegroep **ExportGroup**. Voor de overige eigenschappen kunt u de standaardwaarden gebruiken. Selecteer **Maken**.

      ![Waarden opgeven voor opslag](./media/resource-manager-export-template/provide-storage-values.png)

Nadat de implementatie is voltooid, bevat uw abonnement het opslagaccount.

## De sjabloon exporteren uit de implementatiegeschiedenis

1. Ga naar de resourcegroepblade van uw nieuwe resourcegroep. U ziet dat het resultaat van de laatste implementatie in de blade wordt vermeld. Selecteer deze koppeling.

      ![resourcegroepblade](./media/resource-manager-export-template/resource-group-blade.png)

2. U ziet nu de geschiedenis van de implementaties voor de groep. In uw geval wordt er in de blade waarschijnlijk slechts één implementatie vermeld. Selecteer deze implementatie.

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

     De bestanden zijn beschikbaar via de koppelingen in de blade. In de blade wordt standaard de sjabloon weergegeven.

       ![sjabloon weergeven](./media/resource-manager-export-template/view-template.png)

     Laten we wat aandachtiger naar de sjabloon kijken. De sjabloon moet er ongeveer zo uitzien:

        {     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",     "contentVersion": "1.0.0.0",     "parameters": {       "name": {         "type": "String"       },       "accountType": {         "type": "String"       },       "location": {         "type": "String"       },       "encryptionEnabled": {         "defaultValue": false,         "type": "Bool"       }     },     "resources": [       {         "type": "Microsoft.Storage/storageAccounts",         "sku": {           "name": "[parameters('accountType')]"         },         "kind": "Storage",         "name": "[parameters('name')]",         "apiVersion": "2016-01-01",         "location": "[parameters('location')]",         "properties": {           "encryption": {             "services": {               "blob": {                 "enabled": "[parameters('encryptionEnabled')]"               }             },             "keySource": "Microsoft.Storage"           }         }       }     ]   }
 
Deze sjabloon is de daadwerkelijke sjabloon die is gebruikt voor het maken van uw opslagaccount. Deze sjabloon bevat parameters waarmee u verschillende soorten opslagaccounts kunt implementeren. Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de structuur van een sjabloon. Zie [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md) voor de volledige lijst met functies die u in een sjabloon kunt gebruiken.


## Een virtueel netwerk toevoegen

De sjabloon die u in de vorige sectie hebt gedownload, weerspiegelde de infrastructuur voor die oorspronkelijke implementatie. Er wordt echter geen rekening gehouden met eventuele wijzigingen na de implementatie.
Ter illustratie van dit probleem gaan we de resourcegroep wijzigen door via de portal een virtueel netwerk toe te voegen.

1. Selecteer **Toevoegen** in de blade van de resourcegroep.

      ![resource toevoegen](./media/resource-manager-export-template/add-resource.png)

2. Selecteer **Virtueel netwerk** uit de beschikbare resources.

      ![virtueel netwerk selecteren](./media/resource-manager-export-template/select-vnet.png)

2. Geef het virtuele netwerk de naam **VNET** en gebruik de standaardwaarden voor de overige eigenschappen. Selecteer **Maken**.

      ![waarschuwing instellen](./media/resource-manager-export-template/create-vnet.png)

3. Nadat het virtuele netwerk in de resourcegroep is geïmplementeerd, bekijkt u de geschiedenis van de implementatie opnieuw. U ziet nu twee implementaties. Als u de tweede implementatie niet ziet, moet u mogelijk uw resourcegroep-blade sluiten en opnieuw openen. Selecteer de meest recente implementatie.

      ![implementatiegeschiedenis](./media/resource-manager-export-template/deployment-history.png)

4. Bekijk de sjabloon voor deze implementatie. U ziet dat deze alleen de wijzigingen definieert die u hebt aangebracht om het virtuele netwerk toe te voegen.

Het is doorgaans een goed idee om te werken met een sjabloon die de gehele infrastructuur voor uw oplossing in één bewerking implementeert. Deze aanpak is betrouwbaarder dan de verschillende sjablonen die moeten worden geïmplementeerd, te onthouden.


## De sjabloon vanuit de resourcegroep exporteren

Hoewel elke implementatie alleen de wijzigingen toont die u aan de resourcegroep hebt aangebracht, kunt u op elk gewenst moment een sjabloon exporteren om de kenmerken van uw hele resourcegroep weer te geven.  

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

3. Download de sjabloon zodat u er lokaal mee kunt werken.

      ![sjabloon downloaden](./media/resource-manager-export-template/download-template.png)

4. Zoek het ZIP-bestand op dat u hebt gedownload en pak de inhoud uit. U kunt deze gedownloade sjabloon gebruiken om uw infrastructuur opnieuw te implementeren.

## Problemen met exports oplossen

Niet alle resourcetypen ondersteunen de functie voor het exporteren van sjablonen. Resource Manager exporteert specifiek sommige resourcetypen niet om te voorkomen dat gevoelige gegevens zichtbaar worden. Als u bijvoorbeeld een verbindingsreeks in de configuratie van uw site hebt, wilt u deze waarschijnlijk niet expliciet weergeven in een geëxporteerde sjabloon. U kunt dit probleem omzeilen door handmatig de ontbrekende resources terug te zetten in de sjabloon.

> [AZURE.NOTE] Exporteren geeft alleen problemen bij het exporteren vanuit een resourcegroep, niet bij het exporteren vanuit de geschiedenis van uw implementatie. Als uw laatste implementatie een nauwkeurige weergave is van de huidige status van de resourcegroep, kunt u het beste de sjabloon exporteren uit de implementatiegeschiedenis in plaats van uit de resourcegroep. Exporteer alleen vanuit een resourcegroep wanneer u wijzigingen hebt aangebracht aan de resourcegroep die niet zijn gedefinieerd in een sjabloon.

Als u bijvoorbeeld een sjabloon exporteert voor een resourcegroep met een web-app, SQL Database en een verbindingsreeks in de site-configuratie, ziet u het volgende bericht.

![fout weergeven](./media/resource-manager-export-template/show-error.png)

Wanneer u het bericht selecteert, ziet u precies welke resourcetypen niet zijn geëxporteerd. 
     
![fout weergeven](./media/resource-manager-export-template/show-error-details.png)

Dit onderwerp bevat de volgende algemene oplossingen. Voor het implementeren van deze resources, moet u parameters toevoegen aan de sjabloon. Zie voor meer informatie [Geëxporteerde sjabloon aanpassen en opnieuw implementeren](resource-manager-customize-template.md).

### Verbindingsreeks

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

### Website-extensie

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

### Extensie voor de virtuele machine

Zie voor meer voorbeelden van extensies voor de virtuele machine [Voorbeelden van Azure Windows VM-extensieconfiguratie](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### Gateway van een virtueel netwerk

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

### Lokale netwerkgateway

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

### Verbinding

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


## Volgende stappen

Gefeliciteerd. U hebt geleerd hoe u een sjabloon kunt exporteren uit resources die u in de portal hebt gemaakt.

- In het tweede gedeelte van deze zelfstudie past u de sjabloon aan die u hebt gedownload, door meer parameters toe te voegen en de sjabloon opnieuw te implementeren via een script. Zie [Geëxporteerde sjabloon aanpassen en opnieuw implementeren](resource-manager-customize-template.md).
- Zie [Azure PowerShell gebruiken met Azure Resource Manager](powershell-azure-resource-manager.md) voor het exporteren van een sjabloon via PowerShell.
- Zie [De Azure CLI voor Mac, Linux en Windows gebruiken met Azure Resource Manager](xplat-cli-azure-resource-manager.md) voor het exporteren van een sjabloon via Azure CLI.



<!--HONumber=ago16_HO4-->


