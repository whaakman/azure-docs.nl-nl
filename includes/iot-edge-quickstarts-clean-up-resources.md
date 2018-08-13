---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/26/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 6502ea1733e37e06172833c944c58101b3c049f2
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043780"
---
Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken.

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

> [!IMPORTANT]
> Het verwijderen van de Azure-resources en resourcegroepen kan niet ongedaan worden gemaakt. Wanneer deze items worden verwijderd, worden de resourcegroep en alle bijbehorende resources permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de IoT-hub in een bestaande resourcegroep hebt gemaakt met resources die u wilt behouden, moet u alleen de IoT-hubresource zelf verwijderen in plaats van de resourcegroep te verwijderen.
>

Voer de volgende opdracht uit om alleen de IoT Hub te verwijderen. Vervang \<YourIoTHub > door de naam van de IoT-hub en \<TestResources > door de naam van de resourcegroep:

```azurecli-interactive
az iot hub delete --name <YourIoTHub> --resource-group <TestResources>
```


Ga als volgt te werk om de hele resourcegroep te verwijderen op naam:

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en selecteer **Resourcegroepen**.

2. Voer in het tekstvak **Filteren op naam** de naam van de resourcegroep in die uw IoT-hub bevat. 

3. Selecteer rechts van de resourcegroep, in de resultatenlijst, het weglatingsteken (**...**) en selecteer vervolgens **Resourcegroep verwijderen**.

    ![Resourcegroep verwijderen](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)

4. U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Voer ter bevestiging de naam van de resourcegroep nogmaals in en selecteer **Verwijderen**. Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.






