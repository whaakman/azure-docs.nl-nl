---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 55dc712d323c43849fa3de23c83e29fc348a0f1e
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403951"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

Ga als volgt te werk om een beleid voor gedeelde toegang te maken dat machtigingen voor het verbinden en het **REGI ster** van **Services** verleent en een Connection String voor dit beleid krijgt:

1. Open uw IoT-hub in de [Azure Portal](https://portal.azure.com). De eenvoudigste manier om uw IoT-hub te bereiken, is door **resource groepen**te selecteren, de resource groep te selecteren waar uw IOT-hub zich bevindt en vervolgens uw IOT-hub te selecteren in de lijst met resources.

2. Selecteer in het linkerdeel venster van uw IoT-hub het **beleid voor gedeelde toegang**.

3. Selecteer in het bovenste menu boven de lijst met beleids regels **toevoegen**.

4. Voer in het deel venster **een gedeeld toegangs beleid toevoegen** een beschrijvende naam in voor uw beleid. bijvoorbeeld: *serviceAndRegistryRead*. Onder **machtigingen**selecteert u **REGI ster lezen** en **service verbinding**maken en selecteert u **maken**.

    ![Weer geven hoe een nieuw beleid voor gedeelde toegang moet worden toegevoegd](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

5. Selecteer in het deel venster **Shared Access policies** uw nieuwe beleid in de lijst met beleids regels.

6. Onder **gedeelde toegangs sleutels**selecteert u het Kopieer pictogram voor de **verbindings reeks--primaire sleutel** en slaat u de waarde op.

    ![Het ophalen van de verbindingsreeks](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

Zie [toegangs beheer en machtigingen](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)voor meer informatie over het IOT hub van het beleid voor gedeelde toegang en machtigingen.
