---
title: Kan geen verbinding maken met een cluster in Azure Data Explorer
description: In dit artikel wordt beschreven welke stappen om verbinding te maken in een cluster in Azure.Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 66d77a11f0f68a2dc48802ed21acca8420012207
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758565"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Problemen oplossen: Kan geen verbinding maken met een cluster in Azure Data Explorer

Als u niet alleen verbinding maken met een cluster in Azure Data Explorer, volg deze stappen.

1. Zorg ervoor dat de verbindingsreeks juist is. Deze moet de vorm: `https://<ClusterName>.<Region>.kusto.windows.net`, zoals in het volgende voorbeeld: `https://docscluster.westus.kusto.windows.net`.

1. Zorg ervoor dat u voldoende machtigingen hebt. Als u dit niet doet, krijgt u een reactie van *niet-geautoriseerde*.

    Zie voor meer informatie over machtigingen [databasemachtigingen beheren](manage-database-permissions.md). Indien nodig, werk met uw Clusterbeheerder, zodat ze u aan de juiste rol toevoegen kunnen.

1. Controleer of het cluster nog niet is verwijderd: Controleer het activiteitenlogboek in uw abonnement.

1. Controleer de [Azure service health-dashboard](https://azure.microsoft.com/status/). Zoek naar de status van Azure Data Explorer in de regio waar u wilt verbinding maken met een cluster.

    Als de status niet **goede** (groen vinkje), maak verbinding met het cluster nadat de status wordt verbeterd.

1. Als u nog steeds hulp bij het oplossen van uw probleem, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).