---
title: Informatie over het gebruik van de SFTP-connector in logic apps | Microsoft Docs
description: Logic apps maken met Azure App service. Verbinding maken met SFTP-API voor het verzenden en ontvangen van bestanden. U kunt verschillende bewerkingen zoals maken, bijwerken, ophalen of verwijdert u bestanden kunt uitvoeren.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1abc15daaa96e834aedd121a88b543067e53641b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-sftp-connector"></a>Aan de slag met de SFTP-connector
De SFTP-connector gebruiken voor toegang tot een SFTP-account om te verzenden en ontvangen van bestanden. U kunt verschillende bewerkingen zoals maken, bijwerken, ophalen of verwijdert u bestanden kunt uitvoeren.  

Gebruik [elke connector](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [maken van een logische app nu](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-sftp"></a>Verbinding maken met SFTP
Om uw logische app toegang alle services tot, moet u eerst maken een *verbinding* naar de service. Een [verbinding](connectors-overview.md) biedt connectiviteit tussen een logische app en een andere service.  

### <a name="create-a-connection-to-sftp"></a>Maak een verbinding met SFTP
> [!INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>Gebruik een trigger SFTP
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

In dit voorbeeld wordt de **SFTP - wanneer een bestand wordt toegevoegd of gewijzigd** trigger wordt gebruikt voor het initiëren van een logische app werkstroom wanneer een bestand wordt toegevoegd aan of gewijzigd op een SFTP-server. U ook toevoegen een voorwaarde waarmee de inhoud van de nieuwe of gewijzigde bestand controleert en maakt een beslissing uitpakken van het bestand als de inhoud ervan aangeven dat deze moet worden opgehaald voordat u de inhoud. Ten slotte een actie voor het uitpakken van de inhoud van een bestand toevoegen en plaatst de geëxtraheerde inhoud in een map op de SFTP-server. 

In een enterprise-voorbeeld: u kunt deze trigger voor het bewaken van een map voor SFTP voor nieuwe bestanden die die klantorders vertegenwoordigt.  U kunt vervolgens een actie van de connector SFTP zoals **bestandsinhoud ophalen**, de inhoud van de volgorde voor verdere verwerking en opslag in een orderdatabase ophalen.

> [!INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Een voorwaarde toevoegen
> [!INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>Gebruik een SFTP-actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/sftpconnector/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).