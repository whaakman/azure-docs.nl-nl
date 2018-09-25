---
title: Een cluster maken in Azure Data Explorer is mislukt
description: In dit artikel wordt beschreven welke stappen voor het maken van een cluster in Azure Data Explorer.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6009953ece78eefd52fca9f12e3db80a6d2cc3eb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953205"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Oplossen: Fout voor het maken van een cluster in Azure Data Explorer

In het onwaarschijnlijke geval dat een cluster maken in Azure Data Explorer mislukt, de volgende stappen uit.

1. Zorg ervoor dat u voldoende machtigingen hebt. Voor het maken van een cluster, moet u lid zijn van de *Inzender* of *eigenaar* rol voor het Azure-abonnement. Indien nodig, werken met de beheerder van het abonnement, zodat ze u aan de juiste rol toevoegen kunnen.

1. Zorg ervoor dat er geen validatiefouten die betrekking hebben op de clusternaam u hebt opgegeven onder **-cluster maken** in Azure portal.

1. Controleer de [Azure service health-dashboard](https://azure.microsoft.com/status/>). Zoek naar de status van Azure Data Explorer in de regio waar u probeert te maken van het cluster.

    Als de status niet **goede** (groen vinkje), probeert u het cluster is gemaakt nadat de status wordt verbeterd.

1. Als u nog steeds hulp bij het oplossen van uw probleem, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com).