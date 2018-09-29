---
title: Controleren op Batch-pool en knooppunt-fouten
description: Fouten om te controleren en aanwijzingen om te voorkomen dat bij het maken van pools en knooppunten
services: batch
author: mscurrell
ms.author: markscu
ms.date: 9/25/2018
ms.topic: conceptual
ms.openlocfilehash: bc09089fa9984e9042166938da19499afca21509
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435210"
---
# <a name="checking-for-pool-and-node-errors"></a>Controleren op fouten van toepassingen en -knooppunt

Bij het maken en beheren van Batch-pools, zijn er bewerkingen die vinden direct plaats en er zijn asynchrone bewerkingen die niet direct, op de achtergrond uitgevoerd en kan enkele minuten duren.

Detecteren van fouten voor bewerkingen die direct van kracht is vrij eenvoudig, zoals fouten wordt onmiddellijk geretourneerd door de API, CLI of de gebruikersinterface.

In dit artikel bevat informatie over de bewerkingen op de achtergrond die voor groepen en de pool plaatsvinden-geeft aan hoe fouten kunnen worden gedetecteerd en hoe de fouten kunnen worden vermeden.

## <a name="pool-errors"></a>Groep van toepassingen fouten

### <a name="resize-timeout-or-failure"></a>Time-out voor formaat wijzigen of fout

Wanneer u een nieuwe groep maakt of het formaat van een bestaande pool, de doel-nummers van de knooppunten wordt opgegeven.  De bewerking onmiddellijk wordt voltooid, maar de werkelijke toewijzing van nieuwe knooppunten of verwijdering van de bestaande knooppunten plaatsvindt op de achtergrond op wat mogelijk enkele minuten.  Er is een time-out voor formaat wijzigen is opgegeven in de [maken](https://docs.microsoft.com/rest/api/batchservice/pool/add) of [vergroten of verkleinen](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API - als het doelaantal knooppunten kan niet worden verkregen in de time-outperiode van het formaat wijzigen, klikt u vervolgens de bewerking, niet aan de pool naar een stabiele status en met de formaatwijzigingsfouten.

Er is een time-out voor formaat wordt gerapporteerd door de [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) eigenschap voor de laatste evaluatie, waarin een of meer fouten die zijn opgetreden.

Algemene oorzaken voor formaat wijzigen-outs zijn onder andere:
- Time-out voor formaat wijzigen is te kort
  - De standaardtime-out is 15 minuten, dit is normaal gesproken voldoende tijd voor knooppunten in de pool worden toegewezen aan of verwijderd.
  - Bij het toewijzen van een groot aantal knooppunten (meer dan 1000 knooppunten van een Marketplace-installatiekopie) of meer dan 300 knooppunten van een aangepaste installatiekopie tijdens het maken van de pool of formaat wijzigen, wordt klikt u vervolgens een time-out van 30 minuten aanbevolen.
- Er is onvoldoende kerngeheugenquotum
  - Een batch-account heeft een quotum voor het aantal kernen dat worden toegewezen voor alle groepen.  Batch wordt geen knooppunten toewijzen wanneer het quotum is bereikt.  Het quotum voor kerngeheugens [kan worden verhoogd](https://docs.microsoft.com/azure/batch/batch-quota-limit) om in te schakelen meer knooppunten zijn toegewezen.
- Er is onvoldoende subnet IP-adressen wanneer een [groep is in een virtueel netwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Een virtueel netwerksubnet moet er voldoende vrije IP-adressen worden toegewezen aan alle van de pool-knooppunten wordt gevraagd, anders de knooppunten kunnen niet worden gemaakt.
- Onvoldoende bronnen wanneer een [groep is in een virtueel netwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Resources, zoals load balancers, openbare IP-adressen en nsg's worden gemaakt in het abonnement dat wordt gebruikt voor het maken van het Batch-account.  De abonnementquota voor deze resources moet voldoende zijn.
- Met behulp van een aangepaste VM-installatiekopie voor grote groepen
  - Grote groepen met behulp van aangepaste installatiekopieën kunt nemen meer toewijzen en het formaat van time-outs kunnen optreden.  Limieten en aanbevelingen voor de configuratie vindt u in een [specifieke artikel](https://docs.microsoft.com/azure/batch/batch-custom-images). 

### <a name="auto-scale-failures"></a>Fouten voor automatisch schalen

In plaats van expliciet instellen van het doelaantal knooppunten voor een groep in de pool worden gemaakt of het formaat wijzigen, kan het aantal knooppunten in een pool automatisch kan worden geschaald.  Een [formule voor automatisch schalen kan worden gemaakt voor een groep](https://docs.microsoft.com/azure/batch/batch-automatic-scaling), die wordt geëvalueerd in een interval van reguliere kunnen worden geconfigureerd om in te stellen de beoogd aantal knooppunten voor de pool.  De volgende typen problemen kunnen optreden en gedetecteerd:

- De evaluatie van automatisch schalen kan mislukken.
- De resulterende-/ verkleinbewerking kan mislukken en time-out.
- Het is mogelijk dat er een probleem met de formule voor automatisch schalen, waardoor er onjuiste knooppunt doelwaarden, met de grootte is gewijzigd werkt, of een time-out opgetreden.

Informatie over de laatste evaluatie van automatisch schalen wordt verkregen met behulp van de [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) eigenschap, die rapporten op het moment van de evaluatie, de waarden en het resultaat van de evaluatie en eventuele fouten voor het uitvoeren van de evaluatie.

Informatie over alle evaluaties automatisch wordt vastgelegd door een [pool vergroten of verkleinen van de gebeurtenis](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event).

### <a name="delete"></a>Verwijderen

Ervan uitgaande dat er knooppunten zijn in een groep, en vervolgens een groep verwijderen bewerking resulteert in de knooppunten wordt eerst verwijderd gevolgd door het object groep zelf.  Het kan duren voordat een paar minuten voor de pool-knooppunten moeten worden verwijderd.

De [staat een pool](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) worden ingesteld om te verwijderen tijdens het verwijderen.  De aanroepende toepassing kan worden gedetecteerd als de groep verwijderen met behulp van de eigenschappen 'staat' en 'stateTransitionTime' is duurt te lang.

## <a name="pool-compute-node-errors"></a>Pool-compute knooppuntfouten

Knooppunten is in een pool kunnen worden toegewezen, maar verschillende problemen kunnen leiden tot de knooppunten wordt niet in orde en niet worden gebruikt.  Wanneer knooppunten zijn toegewezen in een pool, ze kosten in rekening gebracht en het is daarom belangrijk voor het detecteren van problemen om te voorkomen dat betaalt voor de knooppunten die niet kunnen worden gebruikt.

### <a name="start-task-failure"></a>Fout bij starten van taak

Een optionele [begintaak](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) kan worden opgegeven voor een groep.  Net als bij elke taak kunnen een opdrachtregel en bestanden te downloaden uit storage worden opgegeven.  De begintaak is opgegeven voor de groep, maar uitgevoerd op elk knooppunt - wanneer elk knooppunt is gestart, en vervolgens de begintaak wordt uitgevoerd.  Een nieuwe eigenschap van de [begintaak](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask), 'waitForSuccess' geeft aan of Batch moet wachten totdat de begintaak is voltooid voordat er taken aan een knooppunt worden gepland.

Als een begintaak mislukt en de configuratie van de taak start opgegeven na afloop van de bewerking is voltooid, klikt u vervolgens het knooppunt is onbruikbaar en nog steeds kosten in rekening gebracht.

Start taakfouten kunnen worden gedetecteerd met behulp van de [resultaat](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) en [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) eigenschappen van het hoogste niveau [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) eigenschap van het knooppunt.

Een mislukte begintaak ook leiden tot het knooppunt [status](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) wordt ingesteld op 'starttaskfailed', maar alleen als 'waitForSuccess' is ingesteld op 'true'.

Net als bij elke taak, kunnen er veel oorzaken voor het mislukken van de taak start.  Als u wilt oplossen, moeten stdout, stderr en alle verdere taakspecifieke logboekbestanden worden gecontroleerd.

### <a name="application-package-download-failure"></a>Fout bij het downloaden van het Application-pakket

Een of meer toepassingspakketten kunnen optioneel worden opgegeven voor een groep, met het opgegeven pakket-bestanden worden gedownload op elk knooppunt en ongecomprimeerd nadat het knooppunt is gestart, maar voordat taken worden gepland.  Het is gebruikelijk voor het gebruik van een opdrachtregel start taak in combinatie met toepassingspakketten kunt bestanden kopiëren naar een andere locatie of het installatieprogramma uitvoert, bijvoorbeeld.

Een fout opgetreden bij het downloaden en decomprimeren van een toepassingspakket worden gerapporteerd door het knooppunt [fouten](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) eigenschap.  De status wordt worden ingesteld op 'onbruikbaar'.

### <a name="node-in-unusable-state"></a>Knooppunt in een onbruikbare status

De [status clusterknooppunt](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) kan worden ingesteld op onbruikbaar om verschillende redenen.  Wanneer 'onbruikbaar', worden de taken kunnen niet worden gepland op het knooppunt, maar het knooppunt nog steeds kosten in rekening gebracht.

Batch wordt altijd proberen te herstellen onbruikbaar knooppunten, maar herstel al dan niet mogelijk, afhankelijk van de oorzaak.

Wanneer de oorzaak kan worden bepaald, wordt dit apparaat worden gerapporteerd door het knooppunt [fouten](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) eigenschap.

Enkele andere voorbeelden van oorzaken voor 'onbruikbaar' knooppunten:

- Ongeldige aangepaste afbeelding. niet voorbereid goed, bijvoorbeeld.
- Infrastructuur mislukt of upgrade van laag niveau leidt tot de onderliggende virtuele machine wordt verplaatst. Batch wordt het knooppunt hersteld.

### <a name="node-agent-log-files"></a>Knooppunt-agent-logboekbestanden

Als het contact opnemen met ondersteuning met betrekking tot een probleem met het knooppunt toepassingen nodig is, kunnen de logboekbestanden van het agentproces van Batch die wordt uitgevoerd op elk knooppunt in de pool worden verkregen.  De logboekbestanden voor een knooppunt kunnen worden geüpload via de Azure portal, Batch Explorer, of een [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs).  Uploaden en opslaan van de logboekbestanden kunnen zeer nuttig zijn als het knooppunt of groep van toepassingen om op te slaan van de kosten van de actieve knooppunten kan worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

Zorg ervoor dat uw toepassing heeft geïmplementeerd uitgebreide foutcontrole, met name voor asynchrone bewerkingen, zodat problemen kunnen snel worden gedetecteerd en gediagnosticeerd.
