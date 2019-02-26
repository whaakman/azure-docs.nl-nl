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
ms.date: 02/24/2019
ms.openlocfilehash: ed172db6aaa064cfed319a4190306d91aa846c48
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806824"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Toegang tot resources van Azure Virtual Network van Azure Logic Apps met behulp van de integratie van service-omgevingen (ISEs)

> [!NOTE]
> Deze mogelijkheid is in *preview-versie*.

Soms uw logic apps en integratieaccounts toegang tot beveiligde bronnen, zoals virtuele machines (VM's) en andere systemen of -services, moeten een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md). Als u deze toegang instelt, kunt u [maken een *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) voor het uitvoeren van uw logic apps en integratieaccounts. Wanneer u een ISE maakt, wordt een privé- en geïsoleerde instantie van de Logic Apps-service in uw Azure-netwerk in Azure geïmplementeerd. Dit privé-exemplaar toegewezen resources, zoals opslag gebruikt en wordt afzonderlijk van de openbare 'global' Logic Apps-service wordt uitgevoerd. Het scheiden van uw geïsoleerde privé-instantie en de openbare globale exemplaar ook vermindert de invloed die andere Azure-tenants kunnen hebben op de prestaties van uw apps, die ook wel bekend als de ['luidruchtige buren'-effect](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Na het maken van uw ISE, wanneer u gaat maken van uw logische app of integratie-account, kunt u uw ISE als locatie voor uw logische app of integratie van account:

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Uw logische app kan nu rechtstreeks toegang tot systemen die zich binnen of verbonden met uw virtuele netwerk met behulp van deze items:

* Een samengesteld ISE-connector voor dat systeem, bijvoorbeeld SQL Server
* Een ingebouwde trigger of actie, zoals de HTTP-trigger of actie
* Een aangepaste connector

Dit overzicht beschrijft meer informatie over hoe een ISE biedt voor uw logische apps en integratie van directe toegang tot uw Azure-netwerk-accounts en ziet u de verschillen tussen een ISE en de globale Logic Apps-service.
Voor on-premises systemen die niet zijn verbonden met een virtueel netwerk of geen connectors voor ISE-versie, u verbinding kunt maken met deze systemen door [instellen en gebruiken van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md).

> [!IMPORTANT]
> Logic apps, ingebouwde acties en connectors die worden uitgevoerd in de ISE-gebruik een andere prijsschema, niet de verbruik gebaseerde prijsstelling. Zie voor meer informatie, [prijzen voor Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Ten opzichte van globale (geïsoleerd)

Wanneer u een geïntegreerde service environment (ISE) in Azure maakt, kunt u de Azure-netwerk waar u *invoeren* uw ISE. Azure injects, of is geïmplementeerd, een persoonlijke exemplaar van de Logic Apps-service in uw virtuele netwerk. Deze actie wordt een geïsoleerde omgeving waarin u kunt maken en uw logische apps worden uitgevoerd op specifieke resources gemaakt. Wanneer u uw logische app maakt, kunt u uw ISE selecteren als de locatie van uw app, die uw logische app direct toegang tot het virtuele netwerk en de resources in dat netwerk biedt.

Logische apps in een ISE bieden de dezelfde gebruikerservaringen en vergelijkbare mogelijkheden als de globale Logic Apps-service. Niet alleen kunt u de dezelfde ingebouwde acties en connectors in de globale Logic Apps-service, maar u kunt ook de ISE-specifieke connectors gebruiken. Dit is bijvoorbeeld sommige standaard-connectors die worden geboden door versies die worden uitgevoerd in een ISE:

* Azure Blob Storage, File Storage en Table Storage
* Azure-wachtrijen, Azure Servicebus, Azure Eventhubs en IBM MQ
* FTP- en SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X 12 en EDIFACT

Het verschil tussen ISE en niet-ISE-connectors is op de locaties waar de triggers en acties uitvoeren:

* In de ISE ingebouwde triggers en acties, zoals HTTP altijd uitgevoerd in dezelfde ISE als uw logische app.

* Voor connectors bieden twee versies, één versie wordt uitgevoerd in een ISE, terwijl de andere versie in de globale Logic Apps-service wordt uitgevoerd.  

  Connectors die u hebt de **ISE** label altijd in dezelfde ISE als uw logische app uitvoeren. Connectors zonder de **ISE** label uitgevoerd in de globale Logic Apps-service.

  ![ISE-connectors selecteren](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Connectors die worden uitgevoerd in een ISE zijn ook beschikbaar in de globale Logic Apps-service.

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Machtigingen voor toegang tot het virtuele netwerk

Voordat u een Azure-netwerk voor het injecteren van uw omgeving selecteren kunt, moet u machtigingen voor toegangsbeheer op basis van rollen (RBAC) instellen in uw virtuele netwerk voor de service Azure Logic Apps. Deze taak is vereist dat u toewijst de **Inzender voor netwerken** en **Inzender voor klassieke** rollen naar de service Azure Logic Apps.
Als u deze machtigingen instelt, Zie [verbinding maken met virtuele netwerken van Azure vanuit logische apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#vnet-access).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Integratieaccounts met ISE

Integratieaccounts kunt u met logic apps in een integratieserviceomgeving (ISE). Deze integratieaccounts moeten echter gebruiken de *dezelfde ISE* als de gekoppelde logische apps. Logische apps in een ISE kunnen verwijzen naar deze integratieaccounts die zich in de dezelfde ISE. Wanneer u een integratieaccount maakt, kunt u uw ISE selecteren als de locatie voor uw integratie-account.

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forum</a> (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de <a href="https://aka.ms/logicapps-wish" target="_blank">website voor feedback van Logic Apps-gebruikers</a>.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [verbinding maken met virtuele Azure-netwerken van geïsoleerde logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
