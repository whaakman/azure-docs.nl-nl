---
title: Azure Cosmos-Emulator downloaden en opmerkingen bij de release
description: Lees de opmerkingen bij de release van de Azure Cosmos-Emulator en downloaden.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 06/20/2019
ms.openlocfilehash: 5985d0d82341c76993ee91b8dff6927edd1ed8b4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332131"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Gebruik de Azure Cosmos-emulator gebruikt voor lokale ontwikkeling en testen

Dit artikel bevat de releaseopmerkingen voor Azure Cosmos-emulator met een lijst van de functie-updates die zijn aangebracht in elke versie. Het bevat ook de meest recente versie van de emulator downloaden en gebruiken.

## <a name="download"></a>Downloaden

| | |
|---------|---------|
|**MSI-bestand downloaden**|[Microsoft Downloadcentrum](https://aka.ms/cosmosdb-emulator)|
|**Aan de slag**|[Lokaal ontwikkelen met Azure Cosmos-emulator](local-emulator.md)|

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="243"></a>2.4.3

- Een uitgeschakelde de MongoDB-service wordt standaard gestart. Alleen de SQL-eindpunt is ingeschakeld als standaard. De gebruiker moet het eindpunt handmatig met behulp van de emulator beginnen "/ EnableMongoDbEndpoint" opdrachtregeloptie te gebruiken. Het is nu, net als alle de andere service-eindpunten, zoals Gremlin, Cassandra en tabel.
- Opgelost in de emulator bij het starten met ' / AllowNetworkAccess "waar de Gremlin, Cassandra en tabel eindpunten zijn niet de juiste manier verwerken aanvragen van externe clients.
- Rechtstreekse verbindingspoorten toevoegt aan de instellingen voor Firewall-regels.

### <a name="240"></a>2.4.0

- Een probleem opgelost met emulator niet te starten wanneer bewaking netwerk-apps, zoals Pulse Client, aanwezig op de hostcomputer zijn.
