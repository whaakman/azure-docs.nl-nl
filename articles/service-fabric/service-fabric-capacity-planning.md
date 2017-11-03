---
title: Capaciteitsplanning voor Service Fabric-apps | Microsoft Docs
description: Hierin wordt beschreven hoe u het aantal rekenknooppunten die zijn vereist voor een Service Fabric-toepassing te identificeren
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: 
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: fc98bdd8b3597810b0c07563af507e93c611f769
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Capaciteitsplanning voor Service Fabric-toepassingen
Dit document leert u hoe u schat de hoeveelheid resources (CPU, RAM-geheugen, schijf-opslag), moet u uw Azure Service Fabric-toepassingen uitvoeren. Het is gebruikelijk voor de resourcevereisten wijzigen gedurende een bepaalde periode. U moet enkele bronnen normaal als u uw service ontwikkelen en testen en vervolgens meer resources nodig als u gaat u naar de productie en uw toepassing populariteit groeit. Bij het ontwerpen van uw toepassing Denk via de lange termijn vereisten en opties waarmee u uw service te schalen om te voldoen aan de vraag van klanten.

 Wanneer u een Service Fabric-cluster maakt, kunt u bepalen welke typen virtuele machines (VM's) van het cluster aanbrengen. Elke virtuele machine wordt geleverd met een beperkte hoeveelheid resources in de vorm van CPU (kernen en snelheid), netwerkbandbreedte, RAM-geheugen en schijfruimte. Als uw service gedurende een bepaalde periode groeit, kunt u upgraden naar virtuele machines die bieden grotere bronnen en/of meer virtuele machines toevoegen aan het cluster. De laatste doet moet u bouwen uw service in eerste instantie zodat deze kan profiteren van nieuwe virtuele machines die dynamisch zijn toegevoegd aan het cluster.

Sommige services beheren weinig tot geen gegevens op de virtuele machines zelf. Capaciteitsplanning voor deze services moet daarom zich richten voornamelijk op de prestaties, wat betekent dat de juiste CPU (kernen en snelheid) van de virtuele machines selecteren. Bovendien moet u netwerkbandbreedte, met inbegrip van hoe vaak u netwerk overdrachten zijn optreedt en hoeveel gegevens worden overgebracht. Als uw service moet worden uitgevoerd en als service gebruik toeneemt, kunt u meer virtuele machines toevoegen aan het cluster en de belasting saldo dat het netwerk via de virtuele machines aanvraagt.

Voor services die grote hoeveelheden gegevens op de virtuele machines beheren, plannen van capaciteit moet zich richten voornamelijk op grootte. Daarom moet u de capaciteit van de VM-RAM-geheugen en schijfopslag zorgvuldig overwegen. Het virtuele geheugen management system in Windows maakt schijfruimte eruitzien als RAM-geheugen voor toepassingscode. De Service Fabric-runtime biedt bovendien slim wisselgeheugengebruik alleen hot gegevens behouden in het geheugen en de koude gegevens naar schijf te verplaatsen. Toepassingen kunnen daarom gebruikmaken van meer geheugen hebben dan fysiek beschikbaar is op de virtuele machine. Prestaties, met meer RAM-geheugen gewoon worden verhoogd omdat de virtuele machine meer schijfopslag in RAM-geheugen houden kunt. De virtuele machine die u hebt een schijf die groot genoeg zijn om de gegevens die u wilt opslaan op de virtuele machine. Op deze manier hebben de virtuele machine voldoende RAM-geheugen om te voorzien van de prestaties die u wenst. Als uw service gegevens gedurende een bepaalde periode groeit, kunt u meer virtuele machines toevoegen aan het cluster en de partitie de gegevens over alle VM's.

## <a name="determine-how-many-nodes-you-need"></a>Het aantal knooppunten, u moet bepalen
Partitioneren van uw service, kunt u gegevens van uw service uitbreiden. Zie voor meer informatie over het partitioneren [partitioneren Service Fabric](service-fabric-concepts-partitioning.md). Elke partitie moet passen binnen één VM, maar meerdere (klein) partities kunnen worden geplaatst op een enkele virtuele machine. Dus hebt met meer kleine partities u meer flexibiliteit dan een paar grotere partities. De verhouding is dat u veel partities verhoogt de overhead van de Service Fabric en u transactiebewerkingen meerdere partities niet uitvoeren. Er is ook meer potentiële netwerkverkeer als uw servicecode vaak toegang nodig heeft tot gegevens die bevinden zich in verschillende partities. Bij het ontwerpen van uw service, moet u zorgvuldig overwegen deze voordelen en nadelen moet worden uitgevoerd op een effectieve partitionering strategie.

Stel dat uw toepassing heeft een stateful service waarmee een grootte van de opslagplaats die u verwacht heeft te laten groeien tot DB_Size GB in een jaar. U bereid bent te meer toepassingen (en partities) toevoegen als u groei afgezien van dat jaar ervaren.  De replicatie-factor (RF) die het aantal replica's voor uw service bepaalt heeft impact op de totale DB_Size. De totale DB_Size over alle replica's is de replicatie-Factor vermenigvuldigd met DB_Size.  Node_Size vertegenwoordigt de schijf ruimte/RAM-geheugen per knooppunt dat u wilt gebruiken voor uw service. De DB_Size in het geheugen op het cluster moeten passen voor de beste prestaties en een Node_Size die rond het RAM-geheugen van de virtuele machine moeten worden gekozen. Door het toewijzen van een Node_Size die groter is dan de capaciteit van het RAM-geheugen zijn u afhankelijk te zijn van het wisselbestand dat door de Service Fabric-runtime. Dus uw prestaties mogelijk niet optimaal als uw volledige gegevens wordt beschouwd als hot (sindsdien de gegevens is wisselbare in/uit). Voor veel services waarbij slechts een fractie van de gegevens hot is, is het echter kosteneffectiever zijn.

Het aantal knooppunten dat is vereist voor maximale prestaties worden als volgt berekend:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Account voor groei
U wilt bereken het aantal knooppunten op basis van de DB_Size die u verwacht dat uw service groeien, naast de DB_Size waarmee u bent begonnen. Het aantal knooppunten vervolgens groeien wanneer uw service groeit, zodat u het aantal knooppunten zijn te veel niet inrichten. Maar het aantal partities moet worden gebaseerd op het aantal knooppunten die nodig zijn wanneer u uw service op maximale groei uitvoert.

Is het raadzaam om een aantal extra machines beschikbaar zijn op elk gewenst moment hebben, zodat u eventuele onverwachte pieken of fout verwerken kan (bijvoorbeeld als een paar virtuele machines uitvallen).  Terwijl de extra capaciteit moet worden bepaald met behulp van de verwachte pieken, een beginpunt te reserveren van een paar extra virtuele machines is (5 tot 10 procent extra).

De voorgaande wordt ervan uitgegaan dat een stateful service. Als u meer dan één stateful service hebt, hebt u de DB_Size die zijn gekoppeld aan de andere services in de vergelijking toevoegen. U kunt ook het aantal knooppunten afzonderlijk voor elke stateful service berekenen.  Uw service heeft replica's of partities die niet zijn verdeeld. Houd er rekening mee dat partities ook meer gegevens dan andere hebben. Zie voor meer informatie over het partitioneren [artikel over aanbevolen procedures partitioneren](service-fabric-concepts-partitioning.md). De voorgaande vergelijking is echter partitie en replica agnostisch, omdat de Service Fabric zorgt ervoor dat de replica's zijn verdeeld tussen de knooppunten op een geoptimaliseerde manier.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Gebruik van een werkblad voor kostenberekening
Nu we enkele echte getallen in de formule. Een [voorbeeld werkblad](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) laat zien hoe u van plan bent de capaciteit voor een toepassing met drie typen van gegevensobjecten. Voor elk object geschatte we de grootte en hoeveel objecten dat we verwachten hebben. We ook selecteren het aantal replica's we van elk objecttype willen. De totale hoeveelheid geheugen moeten worden opgeslagen in het cluster worden berekend door het werkblad.

We Voer vervolgens een VM-grootte en de maandelijkse kosten. Op basis van de VM-grootte, ziet het werkblad u het minimum aantal partities die moet u uw gegevens fysiek past niet op de knooppunten splitsen. Gewenste een groter aantal partities voor uw toepassing specifieke berekening en netwerkverkeer moet. Het werkblad toont het aantal partities die u de gebruikersobjecten profiel beheert van een op zes is toegenomen.

Op basis van deze informatie, het werkblad wordt nu dat u de gegevens met de gewenste partities en replica's op een 26-node cluster fysiek kan ophalen. Echter, dit cluster zou worden veel verpakt, zodat u een aantal extra knooppunten kunt voor knooppuntfouten en upgrades. Ook ziet u het werkblad dat meer dan 57 knooppunten te hoeven geen extra waarde biedt omdat u leeg knooppunten zou hebben. U wilt opnieuw gaan hierboven 57 knooppunten toch voor knooppuntfouten en upgrades. U kunt het werkblad zodat deze overeenkomen met de specifieke behoeften van uw toepassing aanpassen.   

![Werkblad voor kostenberekening][Image1]

## <a name="next-steps"></a>Volgende stappen
Bekijk [partitioneren Service Fabric-services] [ 10] voor meer informatie over het partitioneren van uw service.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
