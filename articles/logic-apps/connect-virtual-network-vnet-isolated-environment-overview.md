---
title: Toegang voor Azure virtual Network (VNET) van Azure Logic Apps
description: In dit overzicht ziet u hoe geïsoleerde logic apps kunnen verbinden met virtuele Azure-netwerken (VNETs) van de integratie van service-omgevingen (ISEs) die gebruikmaken van particuliere en toegewezen resources
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: b012f1ac9c5f08a7e74871ca215299904f6b1deb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958642"
---
# <a name="access-to-azure-virtual-network-vnet-resources-from-isolated-azure-logic-apps"></a>Toegang tot resources van Azure Virtual Network (VNET) van geïsoleerde Azure Logic Apps

> [!NOTE]
> Deze mogelijkheid is in *privépreview*. Om te vragen tot, [maken van uw aanvraag voor deelname aan hier](https://aka.ms/iseprivatepreview).

Soms moeten toegang tot beveiligde resources, zoals virtuele machines (VM's) en andere systemen of -services in uw logic apps en integratieaccounts een [Azure Virtual Network (VNET)](../virtual-network/virtual-networks-overview.md). Dit om toegang te bieden, kunt u [maken een *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) als de locatie voor het maken van uw logic apps en integratieaccounts. 

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Het maken van een ISE, implementeert u een exemplaar van de Logic Apps privé en geïsoleerd in uw VNET. Het privé-exemplaar toegewezen resources, zoals opslag gebruikt en wordt afzonderlijk van de openbare 'global' Logic Apps-service wordt uitgevoerd. Dankzij deze scheiding ook vermindert de invloed die andere Azure-tenants op de prestaties van uw apps hebben kunnen, of de ['luidruchtige buren'-effect](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

In dit overzicht wordt beschreven hoe het maken van een ISE helpt logic apps en rechtstreekse toegang tot resources in uw Azure-VNET-integratieaccounts en vergelijkt de verschillen tussen een ISE en de globale Logic Apps-service.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Ten opzichte van globale (geïsoleerd)

Wanneer u een geïntegreerde service environment (ISE) in Azure maakt, kunt u een Azure VNET als een *peer* voor uw omgeving. Azure heeft een persoonlijke exemplaar van de Logic Apps-service in uw VNET, wat resulteert in een geïsoleerde omgeving waarin u kunt maken en uw logische apps worden uitgevoerd op specifieke resources geïmplementeerd. Als u een logische app maakt, selecteert u in deze omgeving als locatie van uw app, waarmee ook uw logische app direct toegang tot de resources in uw VNET.  

Logische apps in een ISE bieden de dezelfde gebruikerservaringen en vergelijkbare mogelijkheden als de globale Logic Apps-service. Niet alleen kunt u dezelfde dient te worden en connectors die zijn opgegeven door de globale Logic Apps-service gebruiken, maar u kunt kiezen uit de connectors die ISE versies bieden. Dit is bijvoorbeeld sommige standaard-connectors die worden geboden door versies die worden uitgevoerd in een ISE:
 
* Azure Blob Storage, File Storage en Table Storage
* Azure-wachtrijen
* Azure Service Bus
* FTP
* SSH FTP (SFTP)
* SQL Server
* AS2, X 12 en EDIFACT

Het verschil tussen ISE en niet-ISE-connectors is op de locaties waar de triggers en acties uitvoeren:

* Als u ingebouwde triggers en acties, zoals HTTP in uw ISE gebruiken, deze triggers en acties altijd uitgevoerd in dezelfde ISE als uw logische app. 

* Voor connectors die twee opties bieden: één versie wordt uitgevoerd in een ISE, terwijl de andere versie in de globale Logic Apps-service wordt uitgevoerd.  

  Connectors die u hebt de **ISE** label altijd in dezelfde ISE als uw logische app uitvoeren. Connectors zonder de **ISE** label uitgevoerd in de globale Logic Apps-service. 

  ![ISE-connectors selecteren](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Connectors die u in de ISE configureert zijn ook beschikbaar voor gebruik in de globale Logic Apps-service. 

> [!IMPORTANT]
> Logic apps, ingebouwde acties en connectors die worden uitgevoerd in de ISE-gebruik een andere prijsschema, niet de verbruik gebaseerde prijsstelling. Zie voor meer informatie, [prijzen voor Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-vnet-access"></a>Machtigingen voor VNET-toegang

Wanneer u een integratie van service-omgeving (ISE) maakt, selecteert u een Azure-netwerk (VNET) als een *peer* voor uw omgeving. U kunt echter *alleen* deze relatie maken of *peering*, bij het maken van uw ISE. Deze relatie geeft uw ISE-toegang tot resources in uw VNET, waarmee vervolgens logische apps in die ISE rechtstreeks verbinding maken met resources in uw VNET. Voor on-premises systemen in een VNET dat gekoppeld aan een ISE, logische apps rechtstreeks toegang tot deze systemen met behulp van deze items: 

* ISE-connector voor dat systeem, bijvoorbeeld SQL Server

* HTTP-actie 

* Aangepaste connector

Voor on-premises systemen die zich niet in een VNET of geen ISE connectors, kunt u nog steeds verbinding maken nadat u [instellen en gebruiken van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md).

Voordat u een Azure VNET als een peer voor uw omgeving selecteren kunt, moet u machtigingen voor toegangsbeheer op basis van rollen (RBAC) instellen in uw Azure VNET voor de service Azure Logic Apps. Deze taak is vereist dat u toewijst de **Inzender voor netwerken** en **Inzender voor klassieke** rollen naar de service Azure Logic Apps. Zie voor meer informatie over de rolmachtigingen die vereist zijn voor de peering, de [machtigingen sectie in maken, wijzigen of verwijderen van een peering op virtueel netwerk](../virtual-network/virtual-network-manage-peering.md#permissions).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Integratieaccounts met ISE

Kunt u integratieaccounts met logic apps die worden uitgevoerd in een integratieserviceomgeving (ISE), maar deze integratieaccounts moeten ook gebruiken om de *dezelfde ISE* als de gekoppelde logische apps. Logische apps in een ISE kunnen verwijzen naar deze integratieaccounts die zich in de dezelfde ISE. Wanneer u een integratieaccount maakt, kunt u uw ISE selecteren als de locatie voor uw integratie-account.

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forum</a> (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de <a href="http://aka.ms/logicapps-wish" target="_blank">website voor feedback van Logic Apps-gebruikers</a>.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [verbinding maken met virtuele netwerken (VNETs) van geïsoleerde logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
