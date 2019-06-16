---
title: Verbinding maken met GitHub - Azure Logic Apps | Microsoft Docs
description: GitHub-gebeurtenissen met GitHub REST-API's en Azure Logic Apps bewaken
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 0d2ff9368bc244a5afd6fafc40cf476b90a80a52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61462566"
---
# <a name="connect-to-github"></a>Verbinding maken met GitHub

GitHub is een webgebaseerde Git-opslagplaats hostingservice die alle van de gedistribueerde-versiebeheer en source code management (SCM)-functionaliteit in Git plus andere functies biedt.

Aan de slag met de GitHub-connector, [maakt u eerst een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Maak een verbinding met GitHub

Voor het gebruik van de GitHub-connector in een logische app, moet u eerst maken een *verbinding* en geeft u de details voor deze eigenschappen: 

| Eigenschap | Vereist | Description | 
| -------- | -------- | ----------- | 
| Token | Ja | Geef uw GitHub-referenties. |

Nadat u de verbinding hebt gemaakt, kunt u de acties worden uitgevoerd en luisteren naar de triggers die in dit artikel wordt beschreven.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Connector-specifieke details

Raadpleeg voor triggers en acties die zijn gedefinieerd in Swagger en beperkingen met betrekking de [connectorgegevens](/connectors/github/).

## <a name="find-more-connectors"></a>Meer connectors zoeken

* Controleer de [lijst van Connectors](apis-list.md).