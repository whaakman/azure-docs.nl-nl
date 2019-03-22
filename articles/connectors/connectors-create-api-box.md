---
title: Maak verbinding met Box - Azure Logic Apps | Microsoft Docs
description: Maken en beheren van bestanden met het REST-API's en Azure Logic Apps
author: ecfan
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 971d38fa0fbd47f0deb815577033bbe684aac32f
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310673"
---
# <a name="create-and-manage-files-in-box-with-azure-logic-apps"></a>Maken en beheren van bestanden in Box met Azure Logic Apps

Dit artikel wordt beschreven hoe u kunt maken en beheren van uw bestanden in Box uit in een logische app met de Box-connector. Op die manier kunt u logische apps die automatiseren van taken en werkstromen voor het beheren van uw bestanden en andere acties, bijvoorbeeld:

* Bouw uw bedrijfswerkstroom op basis van de gegevens die u van Box krijgt.

* Geautomatiseerde taken en werkstroom geactiveerd wanneer een bestand wordt gemaakt of bijgewerkt.

* Uitvoeren van acties die een bestand wordt gekopieerd, verwijdert u een bestand, en meer.

  Wanneer deze acties een antwoord, zodat ze de uitvoer beschikbaar voor andere acties. 
  Wanneer een bestand in Box is gewijzigd, kunt u dat bestand verzenden in e-mailbericht met behulp van Office 365.

## <a name="prerequisites"></a>Vereisten

* Een [vak account](https://www.box.com/home)

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* De logische app waar u toegang tot uw Box-account. Als uw logische app met een trigger vak wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md).

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie, zoals triggers en acties limieten, zoals is beschreven in van de connector OpenAPI (voorheen Swagger)-bestand, raadpleegt u de [van de connector-verwijzingspagina](/connectors/box/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)