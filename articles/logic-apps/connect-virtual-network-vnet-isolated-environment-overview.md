---
title: Toegang tot Azure-netwerken van Azure Logic Apps met integratie van service-omgevingen (ISEs)
description: In dit overzicht wordt beschreven hoe integration service-omgevingen (ISEs) helpen logic apps toegang krijgen tot Azure-netwerken (VNETs)
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 1ef8c8eec3865f2a6e363e7da1dbda9504b81c05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65546407"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Toegang tot resources van Azure Virtual Network van Azure Logic Apps met behulp van de integratie van service-omgevingen (ISEs)

Soms moeten toegang tot beveiligde bronnen, zoals virtuele machines (VM's) en andere systemen of -services, die in uw logic apps en integratieaccounts een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md). Als u deze toegang instelt, kunt u [maken een *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) waar u uw logische apps uitvoeren en maken van uw integratie-accounts.

Wanneer u een ISE maakt, wordt een privé- en geïsoleerde instantie van de Logic Apps-service in uw Azure-netwerk in Azure geïmplementeerd. Dit privé-exemplaar toegewezen resources, zoals opslag gebruikt en wordt afzonderlijk van de openbare 'global' Logic Apps-service wordt uitgevoerd. Het scheiden van uw geïsoleerde privé-instantie en de openbare globale exemplaar ook vermindert de invloed die andere Azure-tenants kunnen hebben op de prestaties van uw apps, die ook wel bekend als de ['luidruchtige buren'-effect](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Na het maken van uw ISE, wanneer u gaat maken van uw logische app of integratie-account, kunt u uw ISE als locatie voor uw logische app of integratie van account:

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Uw logische app kan nu rechtstreeks toegang tot systemen die zich binnen of verbonden met uw virtuele netwerk met behulp van deze items:

* Een **ISE**-connector voor dat besturingssysteem, zoals SQL Server met het label
* Een **Core**-label ingebouwde trigger of actie, zoals de HTTP-trigger of actie
* Een aangepaste connector

Dit overzicht beschrijft meer informatie over hoe een ISE biedt voor uw logische apps en integratie van directe toegang tot uw Azure-netwerk-accounts en ziet u de verschillen tussen een ISE en de globale Logic Apps-service.

> [!NOTE]
> Logic apps, ingebouwde triggers, ingebouwde acties en connectors die worden uitgevoerd in de ISE-gebruik een prijsstelling verschilt van de prijsstelling op basis van gebruik. Zie voor meer informatie, [prijzen voor Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Ten opzichte van globale (geïsoleerd)

Wanneer u een geïntegreerde service environment (ISE) in Azure maakt, kunt u de Azure-netwerk waar u *invoeren* uw ISE. Azure injects, of is geïmplementeerd, een persoonlijke exemplaar van de Logic Apps-service in uw virtuele netwerk. Deze actie wordt een geïsoleerde omgeving waarin u kunt maken en uw logische apps worden uitgevoerd op specifieke resources gemaakt. Wanneer u uw logische app maakt, kunt u uw ISE selecteren als de locatie van uw app, die uw logische app direct toegang tot het virtuele netwerk en de resources in dat netwerk biedt.

Logische apps in een ISE bieden de dezelfde gebruikerservaringen en vergelijkbare mogelijkheden als de globale Logic Apps-service. Niet alleen kunt u de dezelfde ingebouwde triggers, ingebouwde acties en connectors van de globale Logic Apps-service gebruiken, maar u kunt ook de ISE-specifieke connectors gebruiken. Dit is bijvoorbeeld sommige standaard-connectors die worden geboden door versies die worden uitgevoerd in een ISE:

* Azure Blob Storage, File Storage en Table Storage
* Azure-wachtrijen, Azure Servicebus, Azure Eventhubs en IBM MQ
* FTP- en SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X 12 en EDIFACT

Het verschil tussen ISE en niet-ISE-connectors is op de locaties waar de triggers en acties uitvoeren:

* In de ISE ingebouwde triggers en acties, zoals HTTP, altijd uitgevoerd in de dezelfde ISE als uw logische app en de weergave de **Core** label.

  ![Selecteer "Core" ingebouwde triggers en acties](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* Connectors die worden uitgevoerd in een ISE hebt openbaar gehost versies die beschikbaar zijn in de globale Logic Apps-service. Voor connectors bieden twee versies, connectors met de **ISE** label altijd in dezelfde ISE als uw logische app uitvoeren. Connectors zonder de **ISE** label uitgevoerd in de globale Logic Apps-service.

  ![ISE-connectors selecteren](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

### <a name="access-to-on-premises-data-sources"></a>Toegang tot on-premises gegevensbronnen

Injecteer een ISE voor on-premises systemen die zijn verbonden met een Azure-netwerk, in dat netwerk, zodat uw logische apps rechtstreeks toegang deze systemen tot met behulp van deze items:

* ISE-versie-connector voor dat systeem, bijvoorbeeld SQL Server
  
* HTTP-actie
  
* Aangepaste connector

  * Als u aangepaste connectors die de on-premises gegevensgateway nodig hebt en u deze connectors buiten een ISE hebt gemaakt, kunnen deze connectors ook gebruiken in logische apps in een ISE.
  
  * Aangepaste connectors die zijn gemaakt in een ISE werken niet met de on-premises gegevensgateway. Deze connectors kunnen echter rechtstreeks toegang tot on-premises gegevensbronnen die zijn verbonden met het virtuele netwerk die als host fungeert voor de ISE. Dus logische apps in een ISE waarschijnlijk niet de gegevensgateway nodig hebt bij het communiceren met deze resources.

Voor on-premises systemen die niet zijn verbonden met een virtueel netwerk of geen connectors voor ISE-versie, moet u eerst [instellen van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md) voordat u uw logische apps kunnen verbinden met deze systemen.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Integratieaccounts met ISE

Integratieaccounts kunt u met logic apps in een integratieserviceomgeving (ISE). Deze integratieaccounts moeten echter gebruiken de *dezelfde ISE* als de gekoppelde logische apps. Logische apps in een ISE kunnen verwijzen naar deze integratieaccounts die zich in de dezelfde ISE. Wanneer u een integratieaccount maakt, kunt u uw ISE selecteren als de locatie voor uw integratie-account.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [verbinding maken met virtuele Azure-netwerken van geïsoleerde logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
