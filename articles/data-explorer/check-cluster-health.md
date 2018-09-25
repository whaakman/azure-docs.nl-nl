---
title: Controleer de status van een cluster van Azure Data Explorer
description: Dit artikel beschrijft de stappen uit om te bepalen of uw Azure Data Explorer-cluster in orde is.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0746247d2c912ba66e81b95f45b168e32b522130
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988424"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Controleer de status van een cluster van Azure Data Explorer

Er zijn diverse factoren die invloed hebben op de status van een cluster van Azure Data Explorer, met inbegrip van CPU, geheugen en het schijfsubsysteem. In dit artikel ziet u enkele eenvoudige stappen dat u kunt ondernemen om de status van een cluster te meten.

1. Aanmelden bij [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com).

1. In het linkerdeelvenster, selecteert u uw cluster en voer de volgende opdracht uit.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Uitvoer van 1 is in orde; uitvoer van 0 is beschadigd.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com), en navigeer naar uw cluster.

1. Onder **bewaking**, selecteer **metrische gegevens**en selecteer vervolgens **houden keepalive**, zoals wordt weergegeven in de volgende afbeelding. Uitvoer dicht bij 1 betekent dat een cluster in orde.

    ![Cluster houden keepalive metrische gegevens](media/check-cluster-health/portal-metrics.png)

1. Andere metrische gegevens zoals de CPU en geheugen opslaan in cache om te meten van gebruik van resources voor het cluster toevoegen.

1. Als u hulp bij het vaststellen van problemen met de status van een cluster nodig hebt, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com).