---
title: Controleer op fouten van toepassingen en knooppunt - Azure Batch
description: Fouten om te controleren en hoe u deze kunt vermijden bij het maken van pools en knooppunten
services: batch
author: mscurrell
ms.author: markscu
ms.date: 9/25/2018
ms.topic: conceptual
ms.openlocfilehash: 4e1e645c25d2f1e49e222e39ecd719a414e1404e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790466"
---
# <a name="check-for-pool-and-node-errors"></a>Groep van toepassingen en node op fouten controleren

Bepaalde bewerkingen vinden direct plaats bij het maken en beheren van Azure Batch-pools. Sommige bewerkingen zijn echter asynchrone en uitvoeren op de achtergrond. Ze kunnen enkele minuten duren.

Detecteren van fouten voor bewerkingen die direct van kracht is vrij eenvoudig, omdat er fouten worden onmiddellijk geretourneerd door de API, CLI of de gebruikersinterface.

In dit artikel bevat informatie over de bewerkingen op de achtergrond die kunnen optreden voor groepen en de pool. Geeft aan hoe u kunt detecteren en voorkoming van fouten.

## <a name="pool-errors"></a>Groep van toepassingen fouten

### <a name="resize-timeout-or-failure"></a>Time-out voor formaat wijzigen of fout

Wanneer u een nieuwe groep maakt vergroten of verkleinen van een bestaande pool, geeft u het doelaantal knooppunten.  Direct door de bewerking is voltooid, maar de werkelijke toewijzing van nieuwe knooppunten die of het verwijderen van de bestaande knooppunten kan enkele minuten duren.  Geeft u de time-out voor formaat wijzigen in de [maken](https://docs.microsoft.com/rest/api/batchservice/pool/add) of [vergroten of verkleinen](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API. Als Batch het doelaantal knooppunten niet tijdens de time-outperiode voor formaat wijzigen verkrijgen kan, stopt de bewerking. De groep verplaatst naar een stabiele status en rapporten vergroten of verkleinen van fouten.

De [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) eigenschap voor de meest recente beoordeling rapporten is een time-out voor formaat wijzigen en een lijst met alle fouten die zijn opgetreden.

Algemene oorzaken voor formaat wijzigen-outs zijn onder andere:

- Time-out voor formaat wijzigen is te kort
  - In de meeste gevallen is de standaardtime-out van 15 minuten lang genoeg is voor knooppunten in de pool worden toegewezen aan of verwijderd.
  - Als u een groot aantal knooppunten toewijzen wilt, wordt u aangeraden de time-out voor formaat instellen op 30 minuten. Bijvoorbeeld, wanneer u bent vergroten of verkleinen op meer dan 1000 knooppunten uit een Azure Marketplace-installatiekopie, of op meer dan 300 knooppunten van een aangepaste VM-installatiekopie.
- Er is onvoldoende kerngeheugenquotum
  - Een Batch-account wordt beperkt het aantal kernen dat u deze aan alle groepen toewijzen kunt. Batch wordt gestopt bij het toewijzen van knooppunten wanneer het quotum is bereikt. U [kunt verhogen](https://docs.microsoft.com/azure/batch/batch-quota-limit) het quotum voor kerngeheugens, zodat die partij meer knooppunten kunt toewijzen.
- Er is onvoldoende subnet IP-adressen wanneer een [groep is in een virtueel netwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Een virtueel netwerksubnet moet er voldoende vrije IP-adressen toewijzen aan elk knooppunt in de aangevraagde pool. Anders kan de knooppunten kunnen niet worden gemaakt.
- Onvoldoende bronnen wanneer een [groep is in een virtueel netwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - U kunt resources zoals load balancers, openbare IP-adressen en netwerk-beveiligingsgroepen maken in hetzelfde abonnement bevinden als het Batch-account. Controleer of de abonnementquota voldoende voor deze resources zijn.
- Grote groepen met aangepaste VM-installatiekopieën
  - Grote toepassingen die gebruikmaken van aangepaste VM-installatiekopieën kunnen langer duren om te wijzen en time-outs kunnen optreden.  Zie [een aangepaste installatiekopie gebruiken om een pool van virtuele machines te maken](https://docs.microsoft.com/azure/batch/batch-custom-images) voor aanbevelingen over limieten en configuratie.

### <a name="automatic-scaling-failures"></a>Automatisch vergroten/verkleinen fouten

U kunt ook Azure Batch voor het automatisch schalen van het aantal knooppunten in een pool instellen. Definieert u de parameters voor de [automatisch vergroten/verkleinen formule voor een groep](https://docs.microsoft.com/azure/batch/batch-automatic-scaling). De Batch-service maakt gebruik van de formule periodiek evalueren van het aantal knooppunten in de groep en stelt u een nieuw doel. De volgende typen problemen kunnen optreden:

- De evaluatie van automatisch schalen is mislukt.
- De resulterende-/ verkleinbewerking mislukt en er een time-out.
- Een probleem met de formule voor automatisch vergroten/verkleinen leidt tot een onjuiste knooppunt doelwaarden. De grootte is gewijzigd werkt of een time-out optreedt.

U krijgt informatie over de laatste evaluatie voor automatisch schalen met behulp van de [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) eigenschap. Deze eigenschap rapporteert de evaluatietijd, de waarden en resultaat en eventuele prestatiefouten.

De [pool vergroten of verkleinen van de gebeurtenis](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event) gegevens over alle evaluaties worden vastgelegd.

### <a name="delete"></a>Verwijderen

Wanneer u een pool met knooppunten verwijdert, verwijdert de eerste Batch de knooppunten. Vervolgens wordt de pool-object zelf verwijderd. Het kan duren voordat een paar minuten voor de pool-knooppunten moeten worden verwijderd.

Batch-stelt de [status van toepassingen](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) naar **verwijderen** tijdens het verwijderen. De aanroepende toepassing kan worden gedetecteerd als het verwijderen van toepassingen te lang duurt met behulp van de **status** en **stateTransitionTime** eigenschappen.

## <a name="pool-compute-node-errors"></a>Pool-compute knooppuntfouten

Zelfs als Batch kan worden knooppunten in een groep is toegewezen, kunnen enkele van de knooppunten die moeten worden beschadigd en onbruikbaar door verschillende problemen veroorzaken. Deze knooppunten worden kosten in rekening gebracht. Het is belangrijk voor het detecteren van problemen, zodat u onbruikbaar knooppunten worden niet betaalt.

### <a name="start-task-failure"></a>Fout bij starten van taak

U misschien wilt opgeven van een optionele [begintaak](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) voor een groep. U kunt net als bij elke taak een opdrachtregel en bronbestanden gebruiken om te downloaden uit storage. De begintaak wordt uitgevoerd voor elk knooppunt nadat deze is gestart. De **waitForSuccess** eigenschap geeft aan of batchverwerking wordt er gewacht tot de begintaak voltooid is voordat het plant taken aan een knooppunt.

Wat gebeurt er als u hebt geconfigureerd op het knooppunt moet worden gewacht op voltooiing van taken voor succes gestart, maar de taak start mislukt? In dat geval het knooppunt is niet bruikbaar maar worden nog steeds kosten in rekening gebracht.

U kunt de mislukte taken start detecteren met behulp van de [resultaat](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) en [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) eigenschappen van het hoogste niveau [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) eigenschap van het knooppunt.

Een mislukte begintaak veroorzaakt ook Batch om het knooppunt [status](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) naar **starttaskfailed** als u stelt **waitForSuccess** naar **waar**.

Net als bij elke taak, kunnen er veel oorzaken voor het mislukken van de taak start.  Om op te lossen, controleert u de stdout, stderr en alle verdere taakspecifieke logboekbestanden.

### <a name="application-package-download-failure"></a>Fout bij het downloaden van het Application-pakket

U kunt een of meer toepassingspakketten voor een pool opgeven. Batch-bestanden opgegeven pakket aan elk knooppunt worden gedownload en decomprimeert de bestanden nadat het knooppunt is gestart, maar voordat taken worden gepland. Het is gebruikelijk voor het gebruik van de opdrachtregel van een start-taak in combinatie met toepassingspakketten. Bijvoorbeeld, om te kopiëren van bestanden naar een andere locatie of setup uit te voeren.

Het knooppunt [fouten](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) eigenschap meldt een fout opgetreden bij het downloaden en een toepassingspakket decomprimeren. Batch wordt de status ingesteld op **onbruikbaar**.

### <a name="node-in-unusable-state"></a>Knooppunt in een onbruikbare status

Azure Batch kan instellen de [status clusterknooppunt](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) naar **onbruikbaar** om verschillende redenen. Met de status ingesteld op **onbruikbaar**, taken kunnen niet worden gepland op het knooppunt, maar nog steeds leidt tot kosten.

Batch altijd probeert te herstellen van onbruikbaar knooppunten, maar herstel mogelijk of wellicht niet mogelijk zijn afhankelijk van de oorzaak.

Als Batch kan de oorzaak, het knooppunt [fouten](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) eigenschap gerapporteerd.

Meer voorbeelden van oorzaken van **onbruikbaar** knooppunten bevatten:

- Een aangepaste VM-installatiekopie is ongeldig. Bijvoorbeeld, een afbeelding die niet goed voorbereid.
- Een virtuele machine is verplaatst vanwege een fout infrastructuur of een upgrade van een laag niveau. Batch Hiermee herstelt u het knooppunt.

### <a name="node-agent-log-files"></a>Knooppunt-agent-logboekbestanden

Het agentproces van Batch die wordt uitgevoerd op elk knooppunt in de pool kan logboekbestanden, wat nuttig zijn kunnen als u contact opnemen met ondersteuning over een probleem met het knooppunt toepassingen moet bieden. Logboekbestanden voor een knooppunt kan worden geüpload via de Azure portal, Batch Explorer, of een [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs). Dit is handig om te uploaden en opslaan van de logboekbestanden. Daarna kunt u het knooppunt of de groep van toepassingen om op te slaan van de kosten van de actieve knooppunten verwijderen.

## <a name="next-steps"></a>Volgende stappen

Controleer dat u uw toepassing voor het implementeren van uitgebreide fout controleren, met name voor asynchrone bewerkingen hebt ingesteld. Kan het zijn essentieel voor het snel problemen detecteren en onderzoeken.
