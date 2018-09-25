---
title: Kan geen verbinding maken met een cluster in Azure Data Explorer
description: In dit artikel wordt beschreven welke stappen om verbinding te maken in een cluster in Azure.Data Explorer.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f66dcc55b01b407c59c65ea300757ab4ee1002ac
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965055"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Problemen oplossen: Geen verbinding maken met een cluster in Azure Data Explorer

Als u niet alleen verbinding maken met een cluster in Azure Data Explorer, volg deze stappen.

1. Zorg ervoor dat de verbindingsreeks juist is. Deze moet de vorm: `https://<ClusterName>.<Region>.kusto.windows.net`, zoals in het volgende voorbeeld: `https://docscluster.westus.kusto.windows.net`.

1. Zorg ervoor dat u voldoende machtigingen hebt. Als u dit niet doet, krijgt u een reactie van *niet-geautoriseerde*.

    Zie voor meer informatie over machtigingen [databasemachtigingen beheren](manage-database-permissions.md). Indien nodig, werk met uw Clusterbeheerder, zodat ze u aan de juiste rol toevoegen kunnen.

1. Controleer of het cluster nog niet is verwijderd: Controleer het activiteitenlogboek in uw abonnement.

1. Controleer de [Azure service health-dashboard](https://azure.microsoft.com/status/>). Zoek naar de status van Azure Data Explorer in de regio waar u wilt verbinding maken met een cluster.

    Als de status niet **goede** (groen vinkje), maak verbinding met het cluster nadat de status wordt verbeterd.

1. Als u nog steeds hulp bij het oplossen van uw probleem, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com).