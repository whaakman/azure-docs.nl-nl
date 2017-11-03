---
title: Resource Provider-overzicht | Microsoft Docs
description: Meer informatie over de nieuwe Resource Provider van het netwerk in Azure Resource Manager
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 2428c707ddeed281fddd1e57bc5574603f0b9b1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="network-resource-provider"></a>Netwerkresourceprovider
Een basis nodig in het succes van vandaag, is de mogelijkheid om te bouwen en op grote schaal netwerk-toepassingen op een flexibele, flexibele, veilige en herhaalbare manier beheren. Azure Resource Manager kunt u toepassingen, zoals één verzameling van resources in resourcegroepen maken. Deze resources worden beheerd via verschillende resourceproviders onder Resource Manager.

Azure Resource Manager, is afhankelijk van andere resourceproviders voor toegang tot uw resources. Er zijn drie belangrijkste resourceproviders: netwerk, opslag en berekeningen. Dit document wordt besproken, de kenmerken en voordelen van Resource Provider van het netwerk, met inbegrip van:

* **Metagegevens** – u kunt informatie toevoegen aan de resources met behulp van labels. Deze labels kunnen worden gebruikt voor het bijhouden van Resourcegebruik in resourcegroepen en abonnementen.
* **Meer controle over uw netwerk** - netwerkbronnen los zijn gekoppeld en u ze op een meer gedetailleerd wijze kunt beheren. Dit betekent dat u meer flexibiliteit bij het beheren van de netwerkbronnen.
* **Snellere configuratie** -omdat netwerkbronnen los zijn gekoppeld, kunt u maken en organiseren netwerkbronnen parallel. Dit is aanzienlijk verminderd configuratie.
* **Op rollen gebaseerd toegangsbeheer** -RBAC standaardrollen aan specifieke beveiligingsbereik, naast de mogelijkheid van het maken van aangepaste rollen voor beveiligde management biedt.
* **Eenvoudiger beheer en implementatie** -is het eenvoudiger te implementeren en beheren van toepassingen sinds kunt kunt u een volledige toepassing stack als één verzameling van resources in een resourcegroep. En sneller te implementeren, omdat u implementeren kunt met alleen het leveren van een sjabloon JSON-nettolading.
* **Snelle aanpassing** -u kunt declaratieve-stijl sjablonen gebruiken om in te schakelen herhaalbare en snelle aanpassing van implementaties.
* **Herhaalbare aanpassing** -u kunt declaratieve-stijl sjablonen gebruiken om in te schakelen herhaalbare en snelle aanpassing van implementaties.
* **Beheerinterfaces** -u kunt een van de volgende interfaces gebruiken om uw resources te beheren:
  * REST gebaseerde API
  * PowerShell
  * .NET SDK
  * Node.JS-SDK
  * Java-SDK
  * Azure CLI
  * Preview-portal
  * Sjabloontaal van Resource Manager

## <a name="network-resources"></a>Netwerkbronnen
U kunt nu netwerkbronnen onafhankelijk, beheren in plaats van ze allemaal beheerd via een enkel compute-resource (een virtuele machine). Dit zorgt ervoor dat een hogere mate van flexibiliteit en aanpassingsvermogen bij het schrijven van een complex en grote schaal infrastructuur in een resourcegroep.

Een conceptuele weergave van een voorbeeldimplementatie met betrekking tot een toepassing met meerdere lagen wordt hieronder weergegeven. Elke bron die u, zoals NIC's, openbare IP-adressen en virtuele machines ziet, kan afzonderlijk worden beheerd.

![Netwerk resourcemodel](./media/resource-groups-networking/Figure2.png)

Elke bron bevat een gemeenschappelijke set eigenschappen en hun afzonderlijke eigenschap ingesteld. De algemene eigenschappen zijn:

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **naam** |Unieke resourcenaam. Elk resourcetype heeft een eigen naamsbeperkingen. |PIP01, VM01, NIC01 |
| **location** |Azure-regio waarin de bron zich bevindt |westus, eastus |
| **ID** |Unieke identificatie van de URI op basis van |/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP |

U kunt de afzonderlijke eigenschappen van bronnen in de onderstaande secties controleren.

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Beheerinterfaces
U kunt met behulp van verschillende interfaces Azure VPN-resources kunt beheren. In dit document gaan we in op kabel van deze interfaces: REST-API en sjablonen.

### <a name="rest-api"></a>REST API
Zoals eerder vermeld, kunnen de netwerkbronnen worden beheerd via diverse interfaces, inclusief REST-API, .NET SDK, SDK voor Node.JS, Java SDK, PowerShell, CLI, Azure Portal en sjablonen.

De Rest-API voldoen aan de specificatie HTTP 1.1-protocol. De algemene structuur van de URI van de API wordt hieronder weergegeven:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

En de parameters in accolades vertegenwoordigen de volgende elementen:

* **abonnement-id** -uw Azure-abonnement-id.
* **naamruimte van een resource provider** -naamruimte voor de provider die wordt gebruikt. De waarde voor de netwerkresourceprovider is *Microsoft.Network*.
* **naam van het gebied** -de naam van de Azure-regio

De volgende HTTP-methoden worden ondersteund bij het aanroepen van de REST-API maken:

* **PUT** : wordt gebruikt een bron van een bepaald type maken, wijzigen van een broneigenschap of een koppeling tussen resources wijzigen.
* **OPHALEN van** : wordt gebruikt voor het ophalen van gegevens van een ingerichte resource.
* **VERWIJDEREN** : wordt gebruikt om te verwijderen van een bestaande resource.

De aanvraag en de reactie voldoen aan de indeling van een JSON-nettolading. Zie voor meer informatie [Azure Resource Management API's](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="resource-manager-template-language"></a>Sjabloontaal van Resource Manager
Naast het beheren van resources imperatively (via API's of SDK), kunt u ook een declaratieve programming stijl te netwerkbronnen beheren met behulp van de taal van de Resource Manager-sjabloon gebruiken.

Een voorbeeld-weergave van een sjabloon wordt hieronder:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

De sjabloon is voornamelijk een JSON-beschrijving van de resources en de exemplaarwaarden opgenomen via parameters. Het onderstaande voorbeeld kan worden gebruikt voor het maken van een virtueel netwerk met 2 subnetten.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

U hebt de mogelijkheid van het bieden van de parameterwaarden handmatig wanneer een sjabloon gebruikt, of kunt u een parameterbestand. Het volgende voorbeeld toont een mogelijke aantal parameterwaarden moet worden gebruikt met de bovenstaande sjabloon:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


De belangrijkste voordelen van het gebruik van sjablonen zijn:

* U kunt een complexe infrastructuur in een resourcegroep in een declaratieve style opbouwen. De indeling van het maken van de resources, waaronder het Afhankelijkheidsbeheer van wordt verwerkt door Resource Manager.
* De infrastructuur kan worden gemaakt op een herhaalbare manier over verschillende regio's en binnen een regio gewoon door parameters te wijzigen.
* De stijl die wordt declaratieve leidt tot kortere doorlooptijd in de sjablonen maken en implementeren van de infrastructuur.

Zie voor voorbeeldsjablonen [Azure-snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates).

Zie voor meer informatie over de taal van de Resource Manager-sjabloon [Azure Resource Manager sjabloontaal](../azure-resource-manager/resource-group-authoring-templates.md).

De bovenstaande voorbeeldsjabloon maakt gebruik van het virtuele netwerk en bronnen van het subnet. Er zijn andere netwerkbronnen die kunt u onderstaande:

### <a name="using-a-template"></a>Met behulp van een sjabloon
U kunt services naar Azure uit een sjabloon implementeren met behulp van PowerShell, AzureCLI, of door het uitvoeren van een klik om te implementeren vanuit GitHub. Implementatie van services met een sjabloon in GitHub worden uitgevoerd de volgende stappen uit:

1. Open het bestand template3 vanuit GitHub. Open bijvoorbeeld [virtueel netwerk met twee subnetten](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klik op **implementeren in Azure**, en vervolgens weer aanmelden op bij de Azure portal met uw referenties.
3. Controleer of de sjabloon en klik vervolgens op **opslaan**.
4. Klik op **parameters bewerken** en selecteer een locatie zoals *VS-West*, voor het vnet en subnetten.
5. Wijzig zo nodig de **ADDRESSPREFIX** en **SUBNETPREFIX** parameters en klik vervolgens op **OK**.
6. Klik op **Selecteer een resourcegroep** en klik vervolgens op de resourcegroep die u wilt het vnet en subnetten aan toevoegen. U kunt ook een nieuwe resourcegroep maken door te klikken op **of nieuwe maken**.
7. Klik op **Create**. Let op de tegel om weer te geven **inrichting sjabloonimplementatie**. Als de implementatie is voltooid, ziet u een vergelijkbaar is met een scherm hieronder.

![Voorbeeld sjabloonimplementatie](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a>Volgende stappen
[Taal van Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure-netwerken – gebruikte sjablonen](https://github.com/Azure/azure-quickstart-templates)

[Azure Resource Manager versus klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md)

[Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)

