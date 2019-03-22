---
title: Maak verbinding met Dropbox - Azure Logic Apps
description: Uploaden en beheren van bestanden met Dropbox REST-API's en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: 5a1bfe8ca38fc23f09b13195fb8ca5bd443a4afd
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314413"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Uploaden en beheren van bestanden in Dropbox met behulp van Azure Logic Apps

Met de Dropbox-connector en Azure Logic Apps, kunt u geautomatiseerde werkstromen uploaden en beheren van bestanden in uw Dropbox-account maken. 

Dit artikel wordt beschreven hoe u verbinding maken met Dropbox van uw logische app en voegt u de Dropbox **wanneer een bestand wordt gemaakt** trigger en de Dropbox **bestandsinhoud ophalen via pad** actie.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Een [Dropbox-account](https://www.dropbox.com/), waarmee u gratis kunt aanmelden. Referenties voor uw account zijn nodig voor het maken van een verbinding tussen uw logische app en uw Dropbox-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). In dit voorbeeld moet u een lege, logische app.

## <a name="add-trigger"></a>Trigger toevoegen

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Kies **Alle** onder het zoekvak. Typ 'dropbox' als filter in het zoekvak.
Selecteer deze trigger uit de lijst met triggers: **Wanneer een bestand wordt gemaakt**

   ![Dropbox-trigger selecteren](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Meld u aan met uw referenties voor Dropbox-account en verleent toegang tot uw gegevens Dropbox voor Azure Logic Apps.

1. Geef de vereiste gegevens voor de trigger. 

   Selecteer de map waar u wilt maken van het bestand bijhouden in dit voorbeeld. Als u wilt uw mappen bladeren, kies het pictogram van de map naast de **map** vak.

## <a name="add-action"></a>Actie toevoegen

Nu een actie toevoegen die de inhoud opgehaald van een nieuw bestand.

1. Kies onder de trigger **volgende stap**. 

1. Kies **Alle** onder het zoekvak. Typ 'dropbox' als filter in het zoekvak.
Selecteer deze actie uit de lijst met acties: **Bestandsinhoud ophalen via pad**

1. Als u dit nog niet hebt al Azure Logic Apps voor toegang tot Dropbox gemachtigd, verleent u nu toegang.

1. Om te bladeren naar het bestandspad dat u gebruiken wilt, naast de **bestandspad** Kies het beletselteken (**...** ) knop. 

   U kunt ook klikken op in de **bestandspad** vak en selecteer in de lijst met dynamische inhoud, **bestandspad**, waarvan de waarde is beschikbaar als uitvoer van de trigger die u in de vorige sectie hebt toegevoegd.

1. Wanneer u klaar bent, kunt u uw logische app opslaan.

1. Maak een nieuw bestand in Dropbox voor het activeren van uw logische app.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie, zoals triggers en acties limieten, zoals is beschreven in van de connector OpenAPI (voorheen Swagger)-bestand, raadpleegt u de [van de connector-verwijzingspagina](/connectors/dropbox/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
