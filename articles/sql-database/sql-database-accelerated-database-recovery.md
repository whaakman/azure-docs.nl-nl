---
title: Versneld databaseherstel - Azure SQL Database | Microsoft Docs
description: De Azure SQL Database heeft een nieuwe functie waarmee snel en consistent databaseherstel, onmiddellijk transactie wordt teruggedraaid en agressief logboekafkapping voor individuele databases, elastische pools en Azure SQL Data Warehouse.
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/11/2018
ms.openlocfilehash: 49ef31996cb9c55ed244202a85e123faf52fbd2a
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649763"
---
# <a name="accelerated-database-recovery-preview"></a>Versneld databaseherstel (preview)

**Versnelde Database Recovery (ADR)** wordt een nieuwe SQL database-engine-functie maken die aanzienlijk verbetert de beschikbaarheid van de database, met name bij langlopende transacties, uitgevoerd door het herstelproces van SQL database-engine opnieuw. ADR is momenteel beschikbaar voor individuele databases, elastische pools en Azure SQL Data Warehouse. De belangrijkste voordelen van ADR zijn:

- **Snelle en consistente databaseherstel**

  Met de ADR, hebben langlopende transacties geen invloed op de algehele hersteltijd, zodat snel en consistent databaseherstel, ongeacht het aantal actieve transacties in het systeem of de grootte.

- **Onmiddellijke transactie wordt teruggedraaid**

  Met ADR is de transactie wordt teruggedraaid onmiddellijk, ongeacht het moment dat de transactie actief is geweest of het aantal updates dat is uitgevoerd.

- **Agressief logboekafkapping**

  Met de ADR, het transactielogboek doelbewust afgekapt, zelfs met actieve langlopende transacties, die voorkomt u dat het groeiende buiten het besturingselement.

## <a name="the-current-database-recovery-process"></a>Het proces van de huidige database herstellen

Databaseherstel in SQL Server volgt de [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) herstelmodel en bestaat uit drie fasen, die worden weergegeven in het volgende diagram en nader wordt toegelicht in het diagram te volgen.

![huidige herstelproces](./media/sql-database-accelerated-database-recovery/current-recovery-process.png)

- **Analysefase**

  Doorsturen scan van het transactielogboek vanaf het begin van de laatste geslaagde controlepunt (of de oudste pagina LSN) tot het einde, om te bepalen van de status van elke transactie op het moment dat SQL Server is gestopt.

- **Fase opnieuw**

  Doorsturen scan van het transactielogboek van de oudste niet-doorgevoerde transacties tot aan het einde, om de database naar de status op het moment van de crash was door alle bewerkingen opnieuw uitvoeren.

- **Fase ongedaan maken**

  Voor elke transactie die vanaf het moment waarop de crash actief was, passeert het logboek achterwaartse, ongedaan maken van de bewerkingen die deze transactie uitgevoerd.

Op basis van dit ontwerp, is de tijd die nodig is de SQL database-engine voor het herstellen van een onverwacht opnieuw opgestart (ongeveer) evenredig aan de grootte van de langste actieve transactie in het systeem op het moment van de crash. Herstel is vereist voor een ongedaan maken van alle onvolledige transacties. De lengte van de tijd die nodig is, staat in verhouding met het werk dat door de transactie heeft uitgevoerd en de tijd deze actief is geweest. Het proces voor het herstel van SQL Server kan daarom een lange tijd duren in de aanwezigheid van langlopende transacties (zoals grote bulksgewijs invoegen bewerkingen of bewerkingen die index maken op basis van een grote tabel).

Bovendien wordt geannuleerd/terugdraaien van een grote transactie op basis van dit ontwerp kunt ook rekening houden met een lange tijd omdat deze de fase voor herstel van dezelfde ongedaan maken wordt gebruikt, zoals hierboven beschreven.

Bovendien de SQL database-engine kan niet worden afgekapt het transactielogboek wanneer er veel transacties te voeren, omdat de bijbehorende records in logboek registreren nodig zijn voor de processen voor herstel en ongedaan maken. Sommige klanten hebben als gevolg van dit ontwerp van de SQL database-engine, het probleem dat de grootte van het transactielogboek zeer grote groeit en enorme hoeveelheden logboekruimte verbruikt.

## <a name="the-accelerated-database-recovery-process"></a>Het proces versneld databaseherstel

De bovenstaande problemen ADR opgelost door het herstelproces van SQL database-engine op volledig nieuwe:

- Geef deze constante tijd/direct door te vermijden om moet scannen van het logboek van/naar het begin van de actieve transactie. Met de ADR, wordt het transactielogboek alleen verwerkt in de laatste geslaagde controlepunt (of een oudste vervuild pagina Log Sequence Number(LSN). Hersteltijd wordt hierdoor niet beïnvloed door lange transacties uitgevoerd.
- Kan de vereiste transactielogboekruimte maken omdat er niet meer nodig voor het verwerken van het logboek voor de hele transactie. Als gevolg hiervan kan het transactielogboek doelbewust als controlepunten worden afgekapt en back-ups uitgevoerd.

Op hoog niveau realiseert ADR snelle databaseherstel door versiebeheer alle fysieke database aanpassingen en alleen ongedaan maken logische bewerkingen, die zijn beperkt en vrijwel onmiddellijk ongedaan kunnen worden. De transactie die vanaf het moment waarop een crash actief was zijn gemarkeerd als afgebroken en daarom alle versies die worden gegenereerd door deze transacties kunnen worden genegeerd door query's van gelijktijdige gebruikers.

Het herstelproces ADR heeft de dezelfde drie fasen als het huidige herstelproces. De werking van deze fasen met ADR is in het volgende diagram wordt geïllustreerd en nader wordt toegelicht in het diagram te volgen.

![ADR-herstelproces](./media/sql-database-accelerated-database-recovery/adr-recovery-process.png)

- **Analysefase**

  Het proces blijft hetzelfde als vandaag nog met het toevoegen van sLog reconstrueren en records in logboek registreren voor niet-samengestelde ops kopiëren.
- **Opnieuw** fase

  Onderverdeeld in twee fasen (P)
  - Fase 1

      Opnieuw uitvoeren van sLog (oudste niet-doorgevoerde transactie maximaal laatste controlepunt). Opnieuw uitvoeren is een snelle bewerking als alleen nodig voor het verwerken van een paar records uit de sLog.
  - Fase 2

     Opnieuw uitvoeren van het logboek voor databasetransacties wordt gestart vanaf het laatste controlepunt (in plaats van de oudste niet-doorgevoerde transactie)
- **Fase ongedaan maken**

   De fase voor ongedaan maken met de ADR is voltooid vrijwel direct met behulp van sLog ongedaan maken van niet-samengestelde operations en opgeslagen versie Store (PV's) met logische terugkeren om uit te voeren rij niveau op basis van versie ongedaan maken.

## <a name="adr-recovery-components"></a>ADR-recovery-onderdelen

Er zijn vier belangrijke onderdelen van de ADR:

- **Persistente versie Store (PV's)**

  De permanente versieopslag is een nieuwe SQL database-engine-mechanisme voor het opslaan van de rij-versies die worden gegenereerd in de database zelf in plaats van de traditionele `tempdb` versieopslag. PV's kunt isolatie van resources, evenals verbetert de beschikbaarheid van de leesbare secundaire databases.

- **Logische ongedaan maken**

  Logische terugkeren asynchroon proces verantwoordelijk is voor het uitvoeren van de rij versie op basis van ongedaan maken: directe transactie wordt teruggedraaid en ongedaan maken voor alle versies bewerkingen bieden.

  - Houdt van alle afgebroken transacties
  - Terugdraaien met behulp van PV's voor alle gebruikerstransacties wordt uitgevoerd
  - Releases alle vergrendelingen onmiddellijk na de transactie afgebroken

- **sLog**

  sLog is een secundaire in-memory-logboekstream dat winkels zich registreert voor niet-samengestelde bewerkingen (zoals metagegevens invalidatie, vergrendeling overnames, enzovoort). De sLog is:

  - Laag volume en in het geheugen
  - Opgeslagen op schijf door tijdens het Controlepuntproces wordt geserialiseerd
  - Periodiek worden afgekapt als transacties doorvoeren
  - Versnelt opnieuw en ongedaan maken door het verwerken van alleen de niet-samengestelde bewerkingen  
  - Schakelt de logboekafkapping agressief transactie door alleen de vereiste logboekrecords te behouden

- **Schoner**

  De reiniging is het asynchroon proces dat periodiek ontwaakt en opschonen van paginaversies die niet nodig zijn.

## <a name="who-should-consider-accelerated-database-recovery"></a>Wie moet rekening houden met versneld databaseherstel is

De volgende typen klanten moeten rekening houden met ADR inschakelen:

- Klanten die workloads, met hebt extreem lange actieve transacties.
- Klanten die gevallen waarin actieve transacties wordt veroorzaakt door het transactielogboek te laten groeien aanzienlijk hebt gezien.  
- Klanten die langere perioden van de database niet beschikbaar zijn vanwege een SQL Server langlopende recovery (zoals onverwachte SQL Server opnieuw opstarten of handmatige transactie wordt teruggedraaid) zijn opgetreden.

## <a name="to-enable-adr-during-this-preview-period"></a>Om in te schakelen ADR tijdens deze preview-periode

Tijdens de preview-periode voor deze functie, stuur een e-mail naar [ adr@microsoft.com ](mailto:adr@microsoft.com) voor meer informatie en probeer het uit versnelde Database Recovery (ADR). In het e-mailbericht, moet u de naam van de logische server (voor individuele databases, elastische pools en Azure Data Warehouse) bevatten. Omdat dit een preview-functie, moet uw testserver een niet-productie-server.
