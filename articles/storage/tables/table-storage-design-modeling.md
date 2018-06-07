---
title: Relaties in Azure storage tabelontwerp modelleren | Microsoft Docs
description: Inzicht in het modelleringsproces bij het ontwerpen van uw opslagoplossing tabel.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 561281375273135009a956fd27dfe9f193ab92ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660825"
---
# <a name="modeling-relationships"></a>Relaties te modelleren
Dit artikel wordt het modelleringsproces om te helpen bij het ontwerpen van uw Azure Table storage-oplossingen.

Ontwikkelen van domeinmodellen is een belangrijke stap in het ontwerp van complexe systemen. Doorgaans kunt u het modelleringsproces entiteiten en de relaties tussen deze als een manier om te begrijpen van het domein voor bedrijven en informeren over het ontwerp van uw systeem te identificeren. Deze sectie richt zich op hoe Vertaal enkele van de algemene relatietypen gevonden in domeinmodellen ontwerpen voor de tabel-service. Het proces van toewijzing van een logische gegevensmodel aan een fysieke basis NoSQL-gegevensmodel verschilt van die gebruikt bij het ontwerpen van een relationele database. Relationele databases ontwerp neemt doorgaans een gegevens-normalisatie-proces geoptimaliseerd voor het minimaliseren van redundantie – en een declaratieve opvragen capaciteit die isoleert hoe de uitvoering van hoe u de database werkt.  

## <a name="one-to-many-relationships"></a>Een-op-veel-relaties
Een-op-veel-relaties tussen bedrijven domeinobjecten vaak voorkomen: één afdeling heeft bijvoorbeeld veel werknemers. Er zijn verschillende manieren voor het implementeren van een-op-veel-relaties in de tabelservice elke met de voor- en nadelen die relevant voor een bepaald scenario zijn mogelijk.  

Bekijk het voorbeeld van een grote onderneming met meerdere nationale met tienduizenden afdelingen en werknemer entiteiten waar elke afdeling heeft veel werknemers en elke werknemer die gekoppeld is aan een bepaalde afdeling. Een aanpak is het opslaan van afzonderlijke afdeling en entiteiten van de werknemer zoals deze:  


![Afzonderlijke afdeling en werknemer entiteiten opslaan](media/storage-table-design-guide/storage-table-design-IMAGE01.png)

Dit voorbeeld ziet u een impliciete een-op-veel-relatie tussen de typen op basis van de **PartitionKey** waarde. Elke afdeling kan veel werknemers hebben.  

In dit voorbeeld toont ook een entiteit afdeling en de gerelateerde werknemer entiteiten in dezelfde partitie. U kunt verschillende partities, tabellen of zelfs storage-accounts gebruiken voor de verschillende entiteittypen.  

Er is een alternatieve methode denormalize van uw gegevens en store-alleen werknemer entiteiten met gedenormaliseerd afdelingsgegevens zoals weergegeven in het volgende voorbeeld. In dit specifieke scenario gedenormaliseerd hiervan mogelijk niet het beste als er een vereiste kunnen de details van een afdelingsmanager niet wijzigen omdat hiervoor moet u elke werknemer van de afdeling bijwerken.  

![werknemer-entiteit](media/storage-table-design-guide/storage-table-design-IMAGE02.png)

Zie voor meer informatie de [Denormalization patroon](table-storage-design-patterns.md#denormalization-pattern) verderop in deze handleiding.  

De volgende tabel geeft een overzicht van de voordelen en nadelen van elk van de methoden die hierboven worden beschreven voor het opslaan van de werknemer en afdeling entiteiten met een-op-veel-relatie een. U moet ook overwegen hoe vaak verwacht u dat kunt u verschillende bewerkingen uitvoeren: kan het zijn aanvaardbaar is voor een ontwerp waarin een dure bewerking als die voor deze bewerking alleen zelden gebeurt hebben.  

<table>
<tr>
<th>Benadering</th>
<th>Professionals</th>
<th>Nadelen</th>
</tr>
<tr>
<td>Afzonderlijke Entiteitstypen, dezelfde partitie, dezelfde tabel</td>
<td>
<ul>
<li>U kunt een entiteit afdeling bijwerken met één bewerking.</li>
<li>U kunt een EGT om consistentie te behouden als u een vereiste voor het wijzigen van een entiteit afdeling hebt wanneer u update, invoegen, verwijderen een werknemer entiteit. Als bijvoorbeeld een aantal afdelingen medewerkers voor elke afdeling te behouden.</li>
</ul>
</td>
<td>
<ul>
<li>Mogelijk moet u een entiteit van de afdeling voor sommige activiteiten van de client en een werknemer worden opgehaald.</li>
<li>Opslagbewerkingen gebeuren op dezelfde partitie. Hoge transactie volumes, kan dit resulteren in een hotspot.</li>
<li>U kunt een werknemer niet verplaatsen naar een nieuwe afdeling met behulp van een EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Afzonderlijke Entiteitstypen, verschillende partities of tabellen of storage-accounts</td>
<td>
<ul>
<li>U kunt een entiteit van de afdeling of de werknemer entiteit bijwerken met één bewerking.</li>
<li>Op hoog transactie volumes, kan dit helpen de werklast verdeeld over meer partities.</li>
</ul>
</td>
<td>
<ul>
<li>Mogelijk moet u een entiteit van de afdeling voor sommige activiteiten van de client en een werknemer worden opgehaald.</li>
<li>U kunt EGTs niet gebruiken om consistentie te behouden wanneer u update, invoegen, verwijderen van een werknemer en update een afdeling. Een aantal werknemers in een entiteit afdeling bijvoorbeeld wordt bijgewerkt.</li>
<li>U kunt een werknemer niet verplaatsen naar een nieuwe afdeling met behulp van een EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Denormalize in één entiteitstype</td>
<td>
<ul>
<li>U kunt alle informatie die u nodig hebt met één aanvraag ophalen.</li>
</ul>
</td>
<td>
<ul>
<li>Het is mogelijk dure om consistentie te behouden als u bijwerken afdelingsgegevens wilt (dit vereist bij te werken van alle werknemers in een afdeling).</li>
</ul>
</td>
</tr>
</table>

Hoe u kiezen tussen deze opties en welke van de voor- en nadelen zijn de meest significante, is afhankelijk van uw specifieke toepassingsscenario's. Bijvoorbeeld, hoe vaak u Wijzig afdeling entiteiten; moeten alle werknemers-query's de afdelingen aanvullende informatie; hoe sluiten weet u de limieten voor schaalbaarheid van de partities of uw storage-account?  

## <a name="one-to-one-relationships"></a>-Op-een-relaties
Domeinmodellen kunnen-op-een-relaties tussen entiteiten bevatten. Als u nodig hebt voor het implementeren van een-op-een relatie in de tabel-service, moet u ook het koppelen van de twee gerelateerde entiteiten wanneer moet u beide ophalen. Deze koppeling kan impliciete, op basis van een overeenkomst in de sleutelwaarden of expliciete worden door het opslaan van een koppeling in de vorm van **PartitionKey** en **RowKey** waarden in elke entiteit naar de gerelateerde entiteit. Zie de sectie voor een bespreking van of moet u de gerelateerde entiteiten opslaan op dezelfde partitie [een-op-veel relaties](#one-to-many-relationships).  

Er zijn ook overwegingen bij de implementatie die kunnen leiden bij de implementatie-op-een-relaties in de tabel-service:  

* Verwerking van grote entiteiten (Zie voor meer informatie [grote entiteiten patroon](table-storage-design-patterns.md#large-entities-pattern)).  
* Toegangsbeheer implementeren (Zie voor meer informatie [beheren van toegang met Shared Access Signatures](#controlling-access-with-shared-access-signatures)).  

## <a name="join-in-the-client"></a>Deelnemen aan de client
Hoewel er manieren om relaties in de tabel-service, moet u niet vergeet dat de twee belangrijkste redenen voor het gebruik van de tabelservice schaalbaarheid en prestaties zijn. Als u dat u veel-relaties die een bedreiging vormen van de prestaties en schaalbaarheid van uw oplossing modelleert vindt, vraagt u uzelf als het is nodig om alle gegevensrelaties in uw tabelontwerp samen te stellen. U kunt mogelijk het ontwerp te vereenvoudigen en verbeteren van de schaalbaarheid en prestaties van uw oplossing als u toestaat dat de clienttoepassing alle benodigde joins uitvoeren.  

Bijvoorbeeld, hebt u een kleine tabellen die gegevens bevatten die niet vaak veranderen, kunt vervolgens u deze gegevens eenmaal ophalen en cache te plaatsen op de client. Dit kunt voorkomen dat herhaalde interactie om op te halen van dezelfde gegevens. In de voorbeelden die we in deze handleiding hebt bekeken, heeft de reeks afdelingen in een kleine organisatie waarschijnlijk klein en wijzig zelden waardoor het een goede kandidaat voor de gegevens eenmaal in client-toepassing kan worden gedownload en cache als opzoeken van gegevens.  

## <a name="inheritance-relationships"></a>Relaties voor overname
Als u de clienttoepassing gebruikmaakt van een set klassen die deel uitmaken van een overnamerelatie bedrijfsentiteiten vertegenwoordigt, kunt u eenvoudig deze entiteiten in de tabel-service behouden. Bijvoorbeeld, u wellicht de volgende set van klassen die zijn gedefinieerd in uw clienttoepassing waar **persoon** is een abstracte klasse.

![Abstracte klasse persoon](media/storage-table-design-guide/storage-table-design-IMAGE03.png)

U kunt deze persistent maken exemplaren van de twee concrete klassen in de tabel-service met één personentabel met entiteiten in die zijn opgemaakt als volgt:  

![Tabel](media/storage-table-design-guide/storage-table-design-IMAGE04.png)

Zie de sectie voor meer informatie over het werken met meerdere Entiteitstypen in dezelfde tabel in de clientcode [werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types) verderop in deze handleiding. Dit vindt u voorbeelden van het herkennen van het entiteitstype in de clientcode.  


## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Tabelgegevens versleutelen](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)