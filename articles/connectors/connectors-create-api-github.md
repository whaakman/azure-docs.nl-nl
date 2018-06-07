---
title: Verbinding maken met de GitHub - Azure Logic Apps | Microsoft Docs
description: GitHub-gebeurtenissen met GitHub REST-API's en Azure Logic Apps bewaken
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: c6bd21626c1934f40520b0ddc2d78eeb97eee9a9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34609909"
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