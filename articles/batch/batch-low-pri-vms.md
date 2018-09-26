---
title: Azure Batch-workloads uitvoeren op rendabele VM's met lage prioriteit | Microsoft Docs
description: Informatie over het inrichten van virtuele machines met lage prioriteit om de kosten van Azure Batch-workloads.
services: batch
author: mscurrell
manager: jeconnoc
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 03/19/2018
ms.author: markscu
ms.openlocfilehash: d42cef944c3b971804ef1417a3877bf919784a02
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093000"
---
# <a name="use-low-priority-vms-with-batch"></a>Virtuele machines met lage prioriteit gebruiken met Batch

Azure Batch biedt met lage prioriteit virtuele machines (VM's) om de kosten van Batch-workloads te beperken. Virtuele machines met lage prioriteit maken nieuwe typen van Batch werkbelastingen mogelijk doordat een grote hoeveelheid rekenkracht moet worden gebruikt voor zeer lage kosten.
 
Virtuele machines met lage prioriteit profiteren van de overtollige capaciteit in Azure. Wanneer u virtuele machines met lage prioriteit in uw pools opgeeft, is Azure Batch kunt deze overschot, indien beschikbaar.
 
De verhouding voor het gebruik van virtuele machines met lage prioriteit is dat deze virtuele machines mogelijk niet beschikbaar moet worden toegewezen of kunnen worden verschoven op elk gewenst moment, afhankelijk van de beschikbare capaciteit. Virtuele machines met lage prioriteit zijn om deze reden uitermate geschikt voor bepaalde soorten werkbelastingen. Gebruik virtuele machines met lage prioriteit voor batch- en asynchrone verwerking van workloads waarbij de voltooiingstijd van de taak is flexibel en het werk verdeeld is over veel virtuele machines.
 
Virtuele machines met lage prioriteit worden aangeboden tegen een aanzienlijk gereduceerde prijs vergeleken met de toegewezen virtuele machines. Zie voor prijsgegevens [prijzen van Batch](https://azure.microsoft.com/pricing/details/batch/).

## <a name="use-cases-for-low-priority-vms"></a>Use cases voor virtuele machines met lage prioriteit

Gezien de kenmerken van virtuele machines met lage prioriteit, werkbelastingen kunnen en deze niet gebruiken? Batch-verwerkingsworkloads zijn in het algemeen geschikt, zoals taken worden onderverdeeld in veel parallelle taken of er zijn veel taken die worden geschaald en verdeeld over veel virtuele machines.

-   Als u wilt het gebruik van de overtollige capaciteit in Azure maximaliseren, schalen geschikt taken.

-   Van tijd tot tijd virtuele machines mogelijk niet beschikbaar of zijn gereserveerd, wat resulteert in minder capaciteit voor taken en kan leiden tot de taak wordt onderbroken en herhalingen. Taken moet daarom flexibele in de tijd die ze uitvoeren kunnen om uit te voeren.

-   Taken met meer taken kunnen meer worden beïnvloed als onderbroken. Als langdurige taken implementeren plaatsen van controlepunten voor voortgang opslaan als ze worden uitgevoerd, klikt u vervolgens de impact van onderbreking teruggebracht. Taken met een kortere uitvoertijd vaak werken het beste met virtuele machines met lage prioriteit, omdat de impact van de onderbreking veel minder is.

-   Langlopende MPI-opdrachten die gebruikmaken van meerdere virtuele machines zijn niet geschikt voor het gebruik van virtuele machines met lage prioriteit, omdat één verschoven VM kan leiden tot de hele taak opnieuw uit te voeren met.

Enkele voorbeelden van batch verwerking van use cases uitermate geschikt voor het gebruik van virtuele machines met lage prioriteit zijn:

-   **Ontwikkelen en testen**: In het bijzonder als grootschalige oplossingen worden ontwikkeld, aanzienlijke besparingen kunnen worden gerealiseerd. Alle typen testen kunnen profiteren, maar grootschalige belastingtests uitvoeren en testen van regressie zijn uitstekende gebruikt.

-   **Ter aanvulling van capaciteit op aanvraag**: VM's met lage prioriteit kunnen worden gebruikt om te vormen een aanvulling op regular toegewezen virtuele machines - indien beschikbaar, taken kunnen schalen en daarom sneller voltooid voor lagere kosten; wanneer deze niet beschikbaar is, de basislijn die is toegewezen virtuele machines beschikbaar blijft .

-   **Flexibele uitvoeringstijd**: als er flexibiliteit in de time-taken hebt voltooid, en vervolgens mogelijke val in capaciteit kunnen worden getolereerd; echter met het toevoegen van virtuele machines met lage prioriteit taken regelmatig uitgevoerd sneller en tegen lagere kosten.

Batch-pools kunnen worden geconfigureerd voor het gebruik van virtuele machines met lage prioriteit in een aantal manieren, afhankelijk van de flexibiliteit in uitvoeringstijd:

-   Virtuele machines met lage prioriteit kunnen uitsluitend worden gebruikt in een pool. In dit geval herstelt Batch alle preempted capaciteit indien beschikbaar. Deze configuratie is de goedkoopste manier voor het uitvoeren van taken, zoals virtuele machines alleen met lage prioriteit worden gebruikt.

-   Virtuele machines met lage prioriteit kunnen worden gebruikt in combinatie met een vaste basislijn toegewezen virtuele machines. De vast aantal toegewezen virtuele machines voor zorgen dat er altijd een capaciteit dat een taak wordt uitgevoerd.

-   Kunnen er dynamische combinatie van virtuele machines toegewezen en met lage prioriteit, zodat de goedkoper met lage prioriteit VM's worden uitsluitend gebruikt indien beschikbaar, maar de volledige prijs toegewezen virtuele machines worden opgeschaald als dat nodig. Deze configuratie blijft een minimale hoeveelheid capaciteit beschikbaar te houden van de voortgang van taken.

## <a name="batch-support-for-low-priority-vms"></a>Batch-ondersteuning voor virtuele machines met lage prioriteit

Azure Batch biedt verschillende mogelijkheden die eenvoudig te gebruiken en profiteren van virtuele machines met lage prioriteit:

-   Batch-pools kunnen zowel toegewezen virtuele machines en virtuele machines met lage prioriteit bevatten. Het aantal van elk type virtuele machine kan worden opgegeven wanneer een groep wordt gemaakt of gewijzigd op elk gewenst moment voor een bestaande pool, met behulp van de expliciete-/ verkleinbewerking of automatisch schalen. Verzending van taken kan blijven ongewijzigd, ongeacht de VM-typen in de groep. U kunt ook een groep voor het gebruik van virtuele machines met lage prioriteit volledig taken uit te voeren als voordelig als mogelijk, maar de toegewezen virtuele machines implementeren als de capaciteit onder een minimale drempelwaarde komt, te houden van taken die worden uitgevoerd.

-   Batch-pools proberen automatisch het doelaantal VM's met lage prioriteit. Als VM's zijn gereserveerd, klikt u vervolgens probeert Batch te vervangen door de verloren capaciteit en Ga terug naar het doel.

-   Wanneer taken worden onderbroken, wordt Batch wordt gedetecteerd en automatisch requeues taken opnieuw uit te voeren.

-   Virtuele machines met lage prioriteit hebben een afzonderlijk vCPU-quotum die van de oplossing voor toegewezen virtuele machines afwijkt. 
    Het quotum voor virtuele machines met lage prioriteit is hoger dan het quotum voor toegewezen virtuele machines, omdat virtuele machines met lage prioriteit minder kosten. Zie voor meer informatie, [Batch-servicequota en limieten](batch-quota-limit.md#resource-quotas).    

> [!NOTE]
> Virtuele machines met lage prioriteit worden momenteel niet ondersteund voor Batch-accounts die zijn gemaakt in [gebruikersabonnementmodus](batch-api-basics.md#account).
>

## <a name="create-and-update-pools"></a>Maken en bijwerken van toepassingen

Een Batch-pool kan zowel toegewezen als lage prioriteit VM's (ook aangeduid als rekenknooppunten) bevatten. U kunt het doelaantal rekenknooppunten instellen voor virtuele machines toegewezen en met lage prioriteit. Het doelaantal knooppunten Hiermee geeft u het aantal VM's die u wilt hebben in de groep.

Bijvoorbeeld, een groep gemaakt met behulp van Azure-cloudservice, virtuele machines met een doel van 5 toegewezen VM's en 20 VM's met lage prioriteit:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5") // WS 2016
);
```

Een groep gemaakt met behulp van Azure virtuele machines (in dit geval Linux-VM's) met een doel van 5 toegewezen VM's en 20 VM's met lage prioriteit:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

U kunt het huidige aantal knooppunten voor VM's toegewezen en met lage prioriteit krijgen:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

De pool hebben een eigenschap om aan te geven als het knooppunt een virtuele machine toegewezen of lage prioriteit is:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Een of meer knooppunten in een pool preëmptief zijn gemaakt, een bewerking van de lijst met knooppunten in de pool nog steeds is, retourneert deze knooppunten. Het huidige aantal knooppunten met lage prioriteit blijft ongewijzigd, maar deze knooppunten hebben de status ingesteld op de **vervallen** staat. Batch wil vervanging VM's vinden en, als dit lukt, gaat u de knooppunten via **maken** en vervolgens **vanaf** Staten voordat deze beschikbaar voor uitvoering van de taak, net als bij nieuwe knooppunten.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Schalen van een pool met VM's met lage prioriteit

Als bij groepen die uitsluitend bestaan uit toegewezen virtuele machines, is het mogelijk voor het schalen van een groep met virtuele machines met lage prioriteit door het aanroepen van de methode grootte of met behulp van automatisch schalen.

De pool resize-bewerking wordt een tweede optionele parameter waarmee de waarde van werkt **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

De formule van de groep van toepassingen voor automatisch schalen ondersteunt virtuele machines met lage prioriteit als volgt:

-   U kunt opvragen of stel de waarde van de service gedefinieerde variabele **$TargetLowPriorityNodes**.

-   Krijgt u de waarde van de service gedefinieerde variabele **$CurrentLowPriorityNodes**.

-   Krijgt u de waarde van de service gedefinieerde variabele **$PreemptedNodeCount**. 
    Deze variabele geeft als resultaat het aantal knooppunten in de status van de preempted en kunt u omhoog of omlaag het aantal toegewezen knooppunten, afhankelijk van het aantal afgebroken knooppunten die niet beschikbaar zijn.

## <a name="jobs-and-tasks"></a>Jobs en taken

Jobs en taken vereist weinig aanvullende configuratie voor knooppunten met lage prioriteit. de enige ondersteuning is als volgt:

-   De eigenschap JobManagerTask van een taak heeft een nieuwe eigenschap **AllowLowPriorityNode**. 
    Als deze eigenschap waar is, kan de jobbeheertaak worden gepland op een knooppunt toegewezen of lage prioriteit. Als deze eigenschap ingesteld op false is, worden de jobbeheertaak is gepland voor alleen een toegewezen knooppunt.

-   Een [omgevingsvariabele](batch-compute-node-environment-variables.md) is beschikbaar voor de taaktoepassing van een, zodat deze bepalen kan of deze wordt uitgevoerd op een knooppunt met lage prioriteit of toegewezen. De omgevingsvariabele is AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Voorrang verwerken

Virtuele machines kunnen van tijd tot tijd worden verschoven; Als voorrang gebeurt, is Batch gebeurt het volgende:

-   De preempted virtuele machines hebben de status bijgewerkt naar **vervallen**.
-   Als taken werden uitgevoerd op de verschoven knooppunt VM's, worden deze taken de ingepland en voert het opnieuw.
-   De virtuele machine is effectief verwijderd, leiden tot verlies van gegevens die lokaal zijn opgeslagen op de virtuele machine.
-   De pool wordt voortdurend probeert te bereiken het doelaantal knooppunten met lage prioriteit beschikbaar. Als vervangingscapaciteit wordt gevonden, de knooppunten behouden hun id's, maar zijn opnieuw geïnitialiseerd, doorlopen **maken** en **vanaf** aangegeven voordat ze beschikbaar voor het plannen van taken zijn.
-   Telt het aantal voorrang zijn beschikbaar als een metrische waarde in de Azure-portal.

## <a name="metrics"></a>Metrische gegevens

Nieuwe metrische gegevens zijn beschikbaar in de [Azure-portal](https://portal.azure.com) voor knooppunten met lage prioriteit. Deze metrische gegevens zijn:

- Aantal knooppunten met lage prioriteit
- Aantal kernen met lage prioriteit 
- Aantal knooppunten verschoven

Metrische gegevens weergeven in Azure portal:

1. Navigeer naar uw Batch-account in de portal en bekijk de instellingen voor uw Batch-account.
2. Selecteer **metrische gegevens** uit de **bewaking** sectie.
3. Selecteer de metrische gegevens van het gewenste de **beschikbare metrische gegevens** lijst.

![Metrische gegevens voor knooppunten met lage prioriteit](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Volgende stappen

* Bekijk het [overzicht met Batch-functies voor ontwikkelaars](batch-api-basics.md), essentiële informatie voor iedereen die Batch wil gaan gebruiken. Het artikel bevat meer gedetailleerde informatie over de Batch-serviceresources zoals groepen, knooppunten, jobs en taken, en de vele API-functies die u tijdens het maken van de Batch-toepassing kunt gebruiken.
* Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.
