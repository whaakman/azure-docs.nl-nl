---
title: Integratie service omgevingen in Azure Logic Apps beheren
description: Controleer de netwerk status en beheer logische apps, verbindingen, aangepaste connectors en integratie accounts in uw integratie service omgeving (ISE) voor Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: 8f10e3d3fd7c67d1e803e8f85c9918c91bb81d59
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517460"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Uw ISE (Integration service Environment) beheren in Azure Logic Apps

Volg de stappen in dit onderwerp om de netwerk status van uw [ISE (Integration service Environment)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) te controleren en de Logic apps, verbindingen, integratie accounts en aangepaste connectors die in uw ISE bestaan, te beheren.

## <a name="view-your-ise"></a>Uw ISE weer geven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Voer in het zoekvak van de portal "integratie service omgevingen" in en selecteer vervolgens **integratie service omgevingen**.

   ![Integratie service omgevingen zoeken](./media/ise-manage-integration-service-environment/find-integration-service-environment.png)

1. Selecteer uw integratie service omgeving in de lijst met resultaten.

   ![Integratie service omgeving selecteren](./media/ise-manage-integration-service-environment/select-integration-service-environment.png)

1. Ga door naar de volgende secties om logische apps, verbindingen, connectors of integratie accounts in uw ISE te vinden.

<a name="check-network-health"></a>

## <a name="check-network-health"></a>De netwerk status controleren

Selecteer in het menu ISE onder **instellingen**de optie **netwerk status**. Dit deel venster toont de integriteits status voor uw subnetten en uitgaande afhankelijkheden op andere services.

![De netwerk status controleren](./media/ise-manage-integration-service-environment/ise-check-network-health.png)

<a name="find-logic-apps"></a>

## <a name="manage-your-logic-apps"></a>Uw Logic apps beheren

1. Selecteer in het menu ISE onder **instellingen**de optie **Logic apps**.

   ![Logische apps zoeken](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Als u logische apps uit uw ISE wilt verwijderen wanneer u deze niet meer nodig hebt, selecteert u deze Logic apps en selecteert u vervolgens **verwijderen**.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>API-verbindingen beheren

1. Als u de API-verbindingen wilt weer geven die zijn gemaakt door Logic apps die in uw ISE worden uitgevoerd, selecteert u in het menu ISE onder **instellingen**de optie **API-verbindingen**.

   ![API-verbindingen zoeken](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Als u verbindingen van uw ISE wilt verwijderen wanneer deze niet meer nodig zijn, selecteert u deze verbindingen en selecteert u vervolgens **verwijderen**.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Aangepaste connectors beheren

1. Als u aangepaste connectors wilt weer geven die zijn gemaakt in uw ISE, selecteert u in het menu ISE onder **instellingen**de optie **aangepaste connectors**.

   ![Aangepaste connectoren vinden](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Als u aangepaste connectors uit uw ISE wilt verwijderen wanneer u deze niet meer nodig hebt, selecteert u deze connectors en selecteert u vervolgens **verwijderen**.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Integratieaccounts beheren

1. Selecteer in uw ISE-menu onder **instellingen**de optie **integratie accounts**.

   ![Integratie accounts zoeken](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Als u integratie accounts uit uw ISE wilt verwijderen wanneer u deze niet meer nodig hebt, selecteert u deze integratie accounts en selecteert u vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [maken van verbinding met virtuele netwerken van Azure vanuit geïsoleerde Logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
