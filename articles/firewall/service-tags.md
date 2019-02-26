---
title: Overzicht van de Firewall van Azure service-tags
description: In dit artikel wordt een overzicht van de Firewall van Azure service-codes.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 2/25/2019
ms.author: victorh
ms.openlocfilehash: 1d03d896de947fcc938619c52a3690962a0d2d6c
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805617"
---
# <a name="azure-firewall-service-tags"></a>Azure Firewall servicetags

Een servicetag vertegenwoordigt een groep IP-adresvoorvoegsels die het maken van beveiligingsregel vereenvoudigt. U kunt niet uw eigen servicetag maken en ook niet opgeven welke IP-adressen zijn opgenomen in een tag. Microsoft beheert de adresvoorvoegsels die de servicetag omvat en werkt de servicetag automatisch bij wanneer adressen veranderen.

Tags van de service Azure Firewall kunnen worden gebruikt in het veld bestemming van netwerk-regels. U kunt deze gebruiken in plaats van specifieke IP-adressen.

## <a name="supported-service-tags"></a>Servicetags ondersteund

De volgende servicetags zijn beschikbaar voor gebruik in Azure-netwerk firewallregels:

* **AzureCloud** (alleen Resource Manager): Met deze tag wordt de IP-adresruimte voor Azure aangeduid, inclusief alle [openbare IP-adressen van het datacenter](https://www.microsoft.com/download/details.aspx?id=41653). Als u *AzureCloud* opgeeft als waarde, wordt verkeer naar AzureCloud toegestaan of geweigerd. Als u toegang tot AzureCloud alleen wilt toestaan in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio opgeven. Als u toegang tot Azure AzureCloud bijvoorbeeld alleen wilt toestaan in de regio US - oost, kunt u *AzureCloud.EastUS* opgeven als servicetag. 
* **AzureTrafficManager** (alleen Resource Manager): Met deze tag wordt de IP-adresruimte voor de test-IP-adressen van Azure Traffic Manager aangeduid. Meer informatie over de test-IP-adressen van Traffic Manager vindt u in de [Veelgestelde vragen over Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). 
* **Storage** (alleen Resource Manager): Met deze tag wordt de IP-adresruimte voor de service Azure Storage aangeduid. Als u *Storage* opgeeft als waarde, wordt verkeer naar de opslag toegestaan of geweigerd. Als u toegang tot opslag alleen wilt toestaan in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio opgeven. Als u toegang tot Azure Storage bijvoorbeeld alleen wilt toestaan in de regio US - oost, kunt u *Storage.EastUS* opgeven als servicetag. De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. De tag vertegenwoordigt bijvoorbeeld de service Azure Storage, maar geen specifiek Azure Storage-account.
* **Sql** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels aan van de services Azure SQL Database en Azure SQL Data Warehouse aangeduid. Als u *Sql* opgeeft als waarde, wordt verkeer naar Sql toegestaan of geweigerd. Als u toegang tot Sql alleen wilt toestaan in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio opgeven. Als u toegang tot Azure SQL Database bijvoorbeeld alleen wilt toestaan in de regio US - oost, kunt u *Sql.EastUS* opgeven als servicetag. De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. De tag vertegenwoordigt bijvoorbeeld de service Azure SQL Database, maar geen specifieke SQL-database of -server.
* **AzureCosmosDB** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de Azure Cosmos Database-service aangeduid. Als u *AzureCosmosDB* opgeeft als waarde, wordt verkeer naar AzureCosmosDB toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot AzureCosmosDB in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met de volgende indeling AzureCosmosDB. [ regionaam].
* **AzureKeyVault** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Key Vault aangeduid. Als u *AzureKeyVault* opgeeft als waarde, wordt verkeer naar AzureKeyVault toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot AzureKeyVault in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met de volgende indeling AzureKeyVault. [ regionaam].
* **EventHub** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Event Hub aangeduid. Als u *EventHub* opgeeft als waarde, wordt verkeer naar EventHub toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot de EventHub in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met behulp van de EventHub-indeling. [ regionaam]. 
* **ServiceBus** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Service Bus aangeduid. Als u *ServiceBus* opgeeft als waarde, wordt verkeer naar ServiceBus toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot service bus in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met behulp van de service bus-indeling. [ regionaam].
* **MicrosoftContainerRegistry** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Microsoft-containerregister aangeduid. Als u *MicrosoftContainerRegistry* opgeeft als waarde, wordt verkeer naar MicrosoftContainerRegistry toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot MicrosoftContainerRegistry in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met de volgende indeling MicrosoftContainerRegistry. [ regionaam].
* **AzureContainerRegistry** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Container Registry aangeduid. Als u *AzureContainerRegistry* opgeeft als waarde, wordt verkeer naar AzureContainerRegistry toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot AzureContainerRegistry in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met de volgende indeling AzureContainerRegistry. [ regionaam]. 
* **AppService** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure App Service aangeduid. Als u *AppService* opgeeft als waarde, wordt verkeer naar AppService toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot Azure App service in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met de volgende indeling AppService. [ regionaam]. 
* **AppServiceManagement** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service App-servicebeheer van Azure aangeduid. Als u *AppServiceManagement* opgeeft als waarde, wordt verkeer naar AppServiceManagement toegestaan of geweigerd. 
* **ApiManagement** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de Azure API Management-service. Als u *ApiManagement* opgeeft als waarde, wordt verkeer naar ApiManagement toegestaan of geweigerd.  
* **AzureConnectors** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure-connectors aangeduid. Als u *AzureConnectors* opgeeft als waarde, wordt verkeer naar AzureConnectors toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot AzureConnectors in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio met de volgende indeling AzureConnectors. [ regionaam].
* **AzureDataLake** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Data Lake aangeduid. Als u *AzureDataLake* opgeeft als waarde, wordt verkeer naar AzureDataLake toegestaan of geweigerd.
* **AzureActiveDirectory** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Active Directory aangeduid. Als u *AzureActiveDirectory* opgeeft als waarde, wordt verkeer naar AzureActiveDirectory toegestaan of geweigerd.
* **AzureMonitor** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de service AzureMonitor. Als u opgeeft *AzureMonitor* voor de waarde moet verkeer is toegestaan of geweigerd voor AzureMonitor.
* **Service fabric** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de service fabric-service. Als u opgeeft *ServiceFabric* voor de waarde moet verkeer is toegestaan of geweigerd voor service fabric.
* **AzureMachineLearning** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de service AzureMachineLearning. Als u opgeeft *AzureMachineLearning* voor de waarde moet verkeer is toegestaan of geweigerd voor AzureMachineLearning.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-Firewall-regels, [Azure firewallregel verwerking van logica](rule-processing.md).