---
title: Conflict resolutie typen en resolutie beleidsregels in Azure Cosmos DB
description: In dit artikel beschrijft de categorieën van conflicten en het conflict resolutie beleidsregels in Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: c682b61a39224f2c80db8fe5fa153ea5e5d82922
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958550"
---
# <a name="conflict-types-and-resolution-policies"></a>Conflicttypen en resolutie beleidsregels

Een conflict veroorzaakt en conflictoplossing beleidsregels zijn van toepassing als uw Cosmos-account is geconfigureerd met meerdere regio's worden geschreven.

Voor Cosmos DB-accounts die zijn geconfigureerd met meerdere regio's voor schrijven, worden Bijwerkconflicten kunnen optreden wanneer meerdere schrijvers hetzelfde item in meerdere regio's tegelijkertijd bijwerken. Update veroorzaakt een conflict in de volgende drie categorieën ingedeeld:

1. **Invoegen van conflicten:** deze conflicten kunnen optreden wanneer een toepassing tegelijkertijd twee of meer items met dezelfde unieke index (bijvoorbeeld de eigenschap ID) van twee of meer regio's voegt. In dit geval alle schrijfbewerkingen in eerste instantie kunnen slagen in hun respectieve lokale regio's, maar op basis van het conflict resolutie beleid dat u kiest, slechts één item met de ID van de oorspronkelijke is definitief doorgevoerd.

1. **Vervang conflicten:** deze conflicten kunnen optreden wanneer een item tegelijk uit twee of meer regio's wordt bijgewerkt.

1. **Verwijderen van conflicten:** deze conflicten kunnen optreden wanneer een toepassing tegelijkertijd wordt een item wordt verwijderd van de ene regio en werkt de uit andere regio.

## <a name="conflict-resolution-policies"></a>Beleid voor conflictoplossing

Cosmos DB biedt een flexibel beleid-gebaseerd mechanisme om op te lossen Bijwerkconflicten. U kunt kiezen uit de volgende twee conflict resolutie beleidsregels op een Cosmos-container:

- **Laatste schrijven Wins (LWW):** dit resolutie beleid standaard maakt gebruik van een systeem gedefinieerde tijdstempeleigenschap (op basis van het clock voor tijdsynchronisatie protocol). U kunt ook wanneer u de SQL-API, kunt Cosmos DB u een andere aangepaste numerieke eigenschap (ook wel het 'conflict resolutie pad' genoemd) om te worden gebruikt voor conflictoplossing opgeven.  

  Als twee of meer items conflict op invoegen of vervangen, wordt het item met de hoogste waarde voor de 'conflict resolutie pad' 'gewonnen'. Als meerdere items dezelfde numerieke waarde voor het pad van de oplossing conflict hebben, worden de geselecteerde "winnaar"-versie wordt bepaald door het systeem. Alle regio's worden te convergeren met een één winnaar en eindigen met de dezelfde versie van de toegezegde item gegarandeerd. Als er conflicten verwijderen die betrokken zijn, wordt de verwijderde versie altijd wins boven een van beide invoegen of vervangen van conflicten, ongeacht de waarde van het conflict resolutie-pad.

  > [!NOTE]
  > Laatste schrijven Wins is het standaardbeleid voor de oplossing van conflicten en is beschikbaar voor SQL, tabel, MongoDB, Cassandra en Gremlin-API-accounts.

  Zie voor meer informatie, [voorbeelden van het gebruik van LWW resolutie beleidsregels conflicteren](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Aangepast:** dit beleid resolutie is ontworpen voor de toepassing gedefinieerde semantiek voor afstemming van conflicten. Bij het instellen van dit beleid van de Cosmos-container, moet u ook een samenvoegen opgeslagen procedure, die automatisch wordt aangeroepen wanneer Bijwerkconflicten worden gedetecteerd in een databasetransactie op de server registreren. Het systeem biedt exact één keer voor het uitvoeren van een procedure samenvoegen als onderdeel van het protocol toezegging garanderen.  

  Echter, als u de container configureren met de aangepaste resolutie-optie, maar een mislukken voor het registreren van een merge-procedure voor de container, of als de samenvoegen-procedure een uitzondering genereert tijdens runtime, de conflicten worden geschreven naar de feed conflicten. Uw toepassing moet vervolgens handmatig de conflicten in de conflicten-feed. Zie voor meer informatie, [voorbeelden van het gebruik van aangepaste resolutie beleid en het gebruik van conflicten feed](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Aangepaste conflict resolutie beleid is alleen beschikbaar voor de SQL API-accounts.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u informatie over het configureren van conflict resolutie beleidsregels met behulp van de volgende artikelen:

* [Het gebruik van de LWW conflict resolutie beleid](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Het gebruik van de aangepaste conflict resolutie beleid](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Het gebruik van de conflicten-feed](how-to-manage-conflicts.md#read-from-conflict-feed)
