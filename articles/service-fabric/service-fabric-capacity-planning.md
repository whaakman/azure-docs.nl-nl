---
title: Capaciteitsplanning voor Service Fabric-apps | Microsoft Docs
description: Hierin wordt beschreven hoe u het aantal rekenknooppunten dat is vereist voor een Service Fabric-toepassing te identificeren
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: ''
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: d7ca566b86ed79aa773d7af2553223c79ed9944a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60342000"
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Capaciteitsplanning voor Service Fabric-toepassingen
Dit document leert u hoe u schat hoeveel resources (disk-opslag, CPU's, RAM,), moet u uw Azure Service Fabric-toepassingen uitvoeren. Het is gebruikelijk voor uw resourcevereisten voor het verloop van tijd veranderen. U moet enkele resources normaal als u uw service ontwikkelen en testen, en klikt u vervolgens meer resources nodig als u in productie gaan en uw toepassing steeds populairder geworden groeit. Bij het ontwerpen van uw toepassing, de lange termijn vereisten bedenken en keuzes waarmee uw service te schalen om te voldoen aan grote vraag.

 Wanneer u een Service Fabric-cluster maakt, kunt u bepalen welke typen virtuele machines (VM's) van het cluster aanbrengen. Elke virtuele machine wordt geleverd met een beperkte hoeveelheid van resources in de vorm van CPU's (kernen en snelheid), netwerkbandbreedte, RAM-geheugen en opslag op de schijf. Als uw service na verloop van tijd groeit, kunt u upgraden naar virtuele machines die meer bronnen voor aanbiedingen en/of meer virtuele machines toevoegen aan uw cluster. Voor de laatste optie, moet u ontwerpen van uw service in eerste instantie, zodat deze kan profiteren van nieuwe virtuele machines die dynamisch toegevoegd aan het cluster.

Sommige services beheren weinig tot geen gegevens meer op de VM's zelf. Capaciteitsplanning voor deze services moet daarom concentreren voornamelijk op de prestaties, wat betekent dat de juiste CPU (kernen en snelheid) van de virtuele machines selecteren. U kunt bovendien moet rekening houden met de netwerkbandbreedte, met inbegrip van hoe vaak de netwerk-overdrachten zijn optreedt en hoeveel gegevens worden overgebracht. Als uw service nodig heeft om uit te voeren en wanneer de service gebruik toeneemt, kunt u meer virtuele machines toevoegen aan het cluster en taakverdeling dat het netwerk voor alle virtuele machines aanvragen.

Voor services die grote hoeveelheden gegevens op de virtuele machines beheren, plannen van capaciteit moet erop zijn gericht voornamelijk grootte. U moet de capaciteit van het RAM-geheugen van de virtuele machine en schijfopslag dus zorgvuldig overwegen. Het systeem voor het beheer van virtueel geheugen in Windows maakt schijfruimte RAM-geheugen toepassingscode uitzien. De Service Fabric-runtime biedt bovendien slim wisselgeheugengebruik alleen hot gegevens blijven in het geheugen en de koude gegevens naar schijf te verplaatsen. Toepassingen kunnen dus meer geheugen dan fysiek beschikbaar is op de virtuele machine gebruiken. Met meer RAM-geheugen gewoon worden de prestaties verbeterd, omdat de virtuele machine kunt meer disk-opslag in RAM-geheugen houden. De virtuele machine die u hebt een schijf die groot genoeg is voor de gegevens die u wilt opslaan op de virtuele machine. De virtuele machine moet op dezelfde manier voldoende RAM-geheugen om te voorzien van de prestaties die u wilt hebben. Als gegevenshoeveelheid gedurende een periode van uw service, kunt u meer virtuele machines toevoegen aan het cluster en de partitie van de gegevens voor alle virtuele machines.

## <a name="determine-how-many-nodes-you-need"></a>Hoeveel knooppunten u moet bepalen
Partitioneren van uw service, kunt u uit de gegevens van uw service te schalen. Zie voor meer informatie over het partitioneren van [partitioneren van Service Fabric](service-fabric-concepts-partitioning.md). Elke partitie moet passen binnen een enkele virtuele machine, maar meerdere partities met (klein) op een enkele virtuele machine kunnen worden geplaatst. Ja, met meer kleine partities hebt u meer flexibiliteit dan met een paar grotere partities. De verhouding is dat veel partities verhoogt de overhead van Service Fabric en u kunt geen transactionele bewerkingen over meerdere partities uitvoeren. Er is ook meer mogelijke netwerkverkeer als uw servicecode is vaak nodig heeft voor toegang tot gegevens die bevinden zich in verschillende partities. Bij het ontwerpen van uw service, moet u zorgvuldig overwegen deze voordelen en nadelen om naar een effectieve strategie voor partitioneren.

Stel dat uw toepassing heeft een stateful service waarmee een store-grootte die u verwacht heeft te laten groeien tot DB_Size GB in een jaar. U bereid bent te meer toepassingen (en partities) toevoegen als u groei voorbij dat jaar ervaring.  De totale DB_Size heeft gevolgen voor de replicatie van meerdere factoren (RF), waarmee wordt bepaald hoe het aantal replica's voor uw service. De totale DB_Size voor alle replica's is de Replicatiefactor DB_Size vermenigvuldigd.  Node_Size vertegenwoordigt de schijf ruimte/RAM-geheugen per knooppunt dat u wilt gebruiken voor uw service. Voor de beste prestaties, de DB_Size moeten passen in het geheugen in het cluster en een Node_Size die rond het RAM-geheugen van de virtuele machine moet worden gekozen. Door toe te wijzen een Node_Size die groter is dan de capaciteit van het RAM-geheugen, bent u vertrouwen op het wisselbestand geleverd door de Service Fabric-runtime. Dus de prestaties van uw mogelijk niet optimaal zijn als de volledige gegevens wordt beschouwd als ' hot ' (sindsdien de gegevens is wisselbaar geheugen in/uit). Voor veel services waarbij slechts een fractie van de gegevens ' hot is ', is het echter rendabeler.

Het aantal knooppunten die nodig zijn voor maximale prestaties wordt als volgt berekend:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Rekening houdt met groei
U kunt voor het berekenen van het aantal knooppunten op basis van de DB_Size die u verwacht dat de service om te groeien, naast de DB_Size waarmee u bent begonnen. Vervolgens meegroeien met het aantal knooppunten uw service zodat u het aantal knooppunten niet te veel inricht. Maar het aantal partities moet worden gebaseerd op het aantal knooppunten die nodig zijn wanneer u uw service op de groei van de maximale uitvoert.

Is het raadzaam om een aantal extra machines beschikbaar zijn op elk gewenst moment hebben, zodat u eventuele onverwachte pieken of fout kan worden verwerkt (bijvoorbeeld, als een paar virtuele machines gaat u naar beneden).  Terwijl de extra capaciteit moet worden bepaald met de verwachte pieken, een beginpunt is een paar extra virtuele machines reserveren (5-10 procent extra).

De voorgaande wordt ervan uitgegaan dat een enkele stateful service. Als u meer dan één stateful service hebt, hebt u de DB_Size die zijn gekoppeld aan de andere services in de vergelijking toevoegen. U kunt ook het aantal knooppunten afzonderlijk voor elke stateful service berekenen.  Uw service heeft replica's of partities die niet zijn verdeeld. Houd er rekening mee dat partities mogelijk ook meer gegevens dan andere. Zie voor meer informatie over het partitioneren van [artikel over aanbevolen procedures voor het partitioneren](service-fabric-concepts-partitioning.md). De voorgaande vergelijking is echter partitie en replica agnostisch is, omdat de Service Fabric zorgt ervoor dat de replica's worden verdeeld tussen de knooppunten in een geoptimaliseerde manier.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Gebruik van een werkblad voor berekening van de kosten
Nu we enkele reële getallen in de formule. Een [voorbeeld werkblad](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) laat zien hoe u de capaciteit voor een toepassing met drie typen van objecten plannen. Voor elk object schatten we de grootte en het aantal objecten dat we verwachten dat. We ook selecteren het aantal replica's die we van elk objecttype wilt. De totale hoeveelheid geheugen beschikbaar om te worden opgeslagen in het cluster worden berekend door het werkblad.

We Voer vervolgens een VM-grootte en de maandelijkse kosten. Op basis van de VM-grootte, ziet het werkblad u het minimum aantal partities die u gebruiken moet voor het splitsen van uw gegevens fysiek past niet op de knooppunten. Gewenste een groter aantal partities voor specifieke berekening van uw toepassing en netwerkverkeer nodig heeft. Het werkblad toont het aantal partities die de gebruiker profiel-objecten beheert is toegenomen van een op zes.

Nu, op basis van al deze gegevens, het werkblad toont dat u alle gegevens die met de gewenste partities en replica's fysiek bij een cluster 26-knooppunten ophalen kan. Maar zou dit cluster worden veel verpakt, zodat u enkele extra knooppunten kunt voor knooppuntfouten en upgrades. Het werkblad ziet ook dat meer dan 57 knooppunten dat u hoeft geen extra waarde biedt omdat u lege knooppunten zou hebben. U wilt opnieuw, gaat u hierboven 57 knooppunten toch voor knooppuntfouten en upgrades. U kunt het werkblad zodat deze overeenkomt met de specifieke behoeften van uw toepassing aanpassen.   

![Werkblad voor berekening van de kosten][Image1]

## <a name="next-steps"></a>Volgende stappen
Bekijk [partitioneren van Service Fabric-services] [ 10] voor meer informatie over het partitioneren van uw service.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
