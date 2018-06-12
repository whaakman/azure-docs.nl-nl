---
title: Verbinding maken met de GitHub - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: ce567dc631c3a147b795eb2355a4961faa8881d6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295809"
---
# <a name="connect-to-github"></a>Verbinding maken met GitHub

GitHub is een webgebaseerde Git-opslagplaats hostingservice waarin het besturingselement gedistribueerde revisie en source code (SCM) beheerfunctionaliteit in Git plus andere functies.

Aan de slag met de GitHub-connector [eerst een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Maak een verbinding met GitHub

Als u de GitHub-connector in een logische app, moet u eerst maken een *verbinding* en Geef details voor deze eigenschappen: 

| Eigenschap | Vereist | Beschrijving | 
| -------- | -------- | ----------- | 
| Token | Ja | Geef uw referenties op GitHub. |

Nadat u de verbinding hebt gemaakt, kunt u de acties uitvoeren en luisteren naar de triggers die in dit artikel wordt beschreven.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Connector-specifieke details

Bekijk voor triggers en acties die zijn gedefinieerd in Swagger en beperkingen, de [connector details](/connectors/github/).

## <a name="find-more-connectors"></a>Meer connectors zoeken

* Controleer de [lijst met Connectors](apis-list.md).