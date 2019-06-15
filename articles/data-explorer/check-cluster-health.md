---
title: Controleer de status van een cluster van Azure Data Explorer
description: Dit artikel wordt beschreven stappen voor het controleren van de status van uw Azure Data Explorer-cluster.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a280d8869a3790444a97c38f792a3d9eeb6bde1d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60861291"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Controleer de status van een cluster van Azure Data Explorer

Er zijn diverse factoren die invloed hebben op de status van een cluster van Azure Data Explorer, met inbegrip van CPU, geheugen en het schijfsubsysteem. In dit artikel ziet u enkele eenvoudige stappen dat u kunt ondernemen om de status van een cluster te meten.

1. Meld u aan bij [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. In het linkerdeelvenster, selecteert u uw cluster en voer de volgende opdracht uit.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Uitvoer van 1 is in orde; uitvoer van 0 is beschadigd.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com), en navigeer naar uw cluster.

1. Onder **bewaking**, selecteer **metrische gegevens**en selecteer vervolgens **houden keepalive**, zoals wordt weergegeven in de volgende afbeelding. Uitvoer dicht bij 1 betekent dat een cluster in orde.

    ![Cluster houden keepalive metrische gegevens](media/check-cluster-health/portal-metrics.png)

1. Het is mogelijk andere metrische gegevens toevoegen aan de grafiek. Vervolgens selecteert u de grafiek **metrische waarde toevoegen**. Nog een metrische waarde - Selecteer in het volgende voorbeeld laat zien **CPU**.

    ![Waarde toevoegen](media/check-cluster-health/add-metric.png)

1. Als u hulp bij het vaststellen van problemen met de status van een cluster nodig hebt, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).