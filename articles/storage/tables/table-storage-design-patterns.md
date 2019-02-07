---
title: Ontwerppatronen voor Azure storage-table | Microsoft Docs
description: Patronen voor oplossingen voor Azure table-service gebruiken.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 40062cfb2e646fd6befef1e746f9493f3e4b20f9
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821355"
---
# <a name="table-design-patterns"></a>Tabelontwerppatronen
Dit artikel beschrijft enkele patronen die geschikt is voor gebruik met oplossingen voor tabel-service. Ook ziet u hoe u enkele van de problemen en wisselwerking besproken in de andere artikelen van tabel storage ontwerp nagenoeg kunt oplossen. Het volgende diagram geeft een overzicht van de relaties tussen de verschillende patronen:  

![gerelateerde gegevens opzoeken](media/storage-table-design-guide/storage-table-design-IMAGE05.png)


De kaart patroon hierboven ziet u enkele relaties tussen (blauw) van patronen en antipatronen (oranje) die worden beschreven in deze handleiding. Er zijn veel andere patronen die moeten overwogen worden. Bijvoorbeeld, een van de belangrijkste scenario's voor Table-Service is met de [gerealiseerde weergave-patroon](https://msdn.microsoft.com/library/azure/dn589782.aspx) uit de [opdracht Query Responsibility Segregation (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) patroon.  

## <a name="intra-partition-secondary-index-pattern"></a>Intra-partitie secundaire index patroon
Store meerdere kopieën van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) voor de snelle en efficiënte zoekopdrachten en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden. Updates tussen exemplaren altijd consistent met behulp van EGT.  

### <a name="context-and-problem"></a>Context en probleem
De Table-service automatisch geïndexeerd entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing een entiteit efficiënt met behulp van deze waarden worden opgehaald. Bijvoorbeeld met behulp van de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kan een punt query gebruiken om op te halen van een individuele werknemer-entiteit met behulp van de naam van de afdeling en de werknemer-ID (de **PartitionKey** en **RowKey**  waarden). Een client kan ook gesorteerd op basis van werknemer-ID binnen elke afdeling entiteiten ophalen.

![Image06](media/storage-table-design-guide/storage-table-design-IMAGE06.png)

Als u ook wilt kunnen vinden van een werknemer-entiteit op basis van de waarde van een ander domein, zoals e-mailadres, moet u een minder efficiënt partitie scan gebruiken om een overeenkomst te vinden. Dit komt doordat de table-service geen secundaire indexen biedt. Bovendien is er geen optie om aan te vragen van een lijst met werknemers in een andere volgorde dan gesorteerd **RowKey** volgorde.  

### <a name="solution"></a>Oplossing
Als tijdelijke oplossing voor het ontbreken van secundaire indexen, kunt u meerdere exemplaren van elke entiteit bij elk exemplaar met behulp van een andere opslaan **RowKey** waarde. Als u een entiteit met de structuur opslaat hieronder wordt weergegeven, kunt u efficiënt werknemer entiteiten op basis van e-adres of de werknemer-id ophalen Het voorvoegsel van de waarden voor de **RowKey**, "empid_" en 'email_' kunt u met behulp van een bereik van e-mailadressen of werknemer-id's op te vragen voor één werknemer of een bereik van werknemers.  

![Werknemer-entiteiten](media/storage-table-design-guide/storage-table-design-IMAGE07.png)

De volgende twee filtercriteria (één opzoeken op basis van de werknemer-ID en een opzoeken op basis van e-mailadres) opgeven point-query's:  

* $filter = (PartitionKey eq 'Verkoop') en (RowKey eq 'empid_000223')  
* $filter = (PartitionKey eq 'Verkoop') en (RowKey eq 'email_jonesj@contoso.com')  

Als u een query voor een bereik van de werknemer entiteiten, kunt u een bereik in werknemer-ID volgorde gesorteerd, of een bereik in e-mailadres volgorde gesorteerd op query's uitvoeren voor entiteiten met het juiste voorvoegsel in de **RowKey**.  

* Vinden alle werknemers in de afdeling verkoop met een werknemer-ID in het bereik 000100-000199 gebruik: $filter = (PartitionKey eq 'Verkoop') en (RowKey ge 'empid_000100') en (RowKey le 'empid_000199')  
* Aan alle werknemers in de afdeling verkoop met een e-mailadres begint met de letter 'a' gebruik zoeken: $filter = (PartitionKey eq 'Verkoop') en (RowKey ge 'email_a') en (RowKey lt 'email_b')  
  
  Merk op dat de filtersyntaxis van de gebruikt in de bovenstaande voorbeelden uit de tabelservice REST-API voor meer informatie naar [entiteiten opvragen](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Tabelopslag is relatief goedkope te gebruiken, dus de kosten-overhead van het opslaan van dubbele gegevens mag geen groot belang. U moet echter altijd de kosten van het ontwerp op basis van uw verwachte opslagvereisten te evalueren en dubbele entiteiten ter ondersteuning van de query's die de clienttoepassing wordt uitgevoerd alleen toe te voegen.  
* Omdat de secundaire index entiteiten worden opgeslagen in dezelfde partitie als de oorspronkelijke entiteiten, moet u ervoor zorgen dat u de schaalbaarheidsdoelen voor een afzonderlijke partitie niet overschrijden.  
* U kunt uw dubbele entiteiten consistent zijn met elkaar houden met behulp van EGTs atomisch bijwerken van de twee kopieën van de entiteit. Dit betekent dat u alle exemplaren van een entiteit moet opslaan in dezelfde partitie. Zie voor meer informatie de sectie [entiteit-groepstransacties](table-storage-design.md#entity-group-transactions).  
* De waarde die wordt gebruikt voor de **RowKey** moet uniek zijn voor elke entiteit. Overweeg het gebruik van de samengestelde sleutel waarden.  
* Opvulling van numerieke waarden in de **RowKey** (bijvoorbeeld de werknemer-ID 000223), kunnen sorteren en filteren op basis van de boven- en ondergrenzen corrigeren.  
* U hoeft niet per se te dupliceren van de eigenschappen van de entiteit. Bijvoorbeeld, als de query's die opzoeken van de entiteiten met behulp van het e-mailbericht adres in de **RowKey** hoeft u nooit leeftijd van de werknemer, deze entiteiten kunnen hebben de volgende structuur:

   ![De entiteitsstructuur werknemer](media/storage-table-design-guide/storage-table-design-IMAGE08.png)


* Is het doorgaans beter dubbele gegevens opslaan en zorg ervoor dat u alle gegevens die u nodig hebt met een enkele query kunt ophalen dan gebruiken om een query om een entiteit en een andere voor het opzoeken van de vereiste gegevens te vinden.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer de clienttoepassing nodig heeft om op te halen van entiteiten met behulp van tal van verschillende sleutels, wanneer de client nodig heeft om op te halen van entiteiten in verschillende sorteervolgorde en waar u elke entiteit met behulp van een aantal unieke waarden kunt identificeren. Echter, moet u ervoor dat de schaalbaarheidslimieten van de partitie niet te overschrijden wanneer u entiteit zoekacties met behulp van de verschillende uitvoert **RowKey** waarden.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Secundaire index tussen partitie-patroon](#inter-partition-secondary-index-pattern)
* [Samengestelde sleutel patroon](#compound-key-pattern)
* Entiteit-groepstransacties
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)

## <a name="inter-partition-secondary-index-pattern"></a>Secundaire index tussen partitie-patroon
Store meerdere kopieën van elke entiteit met behulp van verschillende **RowKey** waarden in afzonderlijke partities of in afzonderlijke tabellen voor de snelle en efficiënte zoekopdrachten en alternatieve sorteervolgorde met behulp van verschillende **RowKey**waarden.  

### <a name="context-and-problem"></a>Context en probleem
De Table-service automatisch geïndexeerd entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing een entiteit efficiënt met behulp van deze waarden worden opgehaald. Bijvoorbeeld met behulp van de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kan een punt query gebruiken om op te halen van een individuele werknemer-entiteit met behulp van de naam van de afdeling en de werknemer-ID (de **PartitionKey** en **RowKey**  waarden). Een client kan ook gesorteerd op basis van werknemer-ID binnen elke afdeling entiteiten ophalen.  

![Werknemer-id](media/storage-table-design-guide/storage-table-design-IMAGE09.png)

Als u ook wilt kunnen vinden van een werknemer-entiteit op basis van de waarde van een ander domein, zoals e-mailadres, moet u een minder efficiënt partitie scan gebruiken om een overeenkomst te vinden. Dit komt doordat de table-service geen secundaire indexen biedt. Bovendien is er geen optie om aan te vragen van een lijst met werknemers in een andere volgorde dan gesorteerd **RowKey** volgorde.  

U bent anticiperen op een zeer groot aantal transacties op basis van deze entiteiten en wilt de risico's van de tabelservice beperking van de client.  

### <a name="solution"></a>Oplossing
Als tijdelijke oplossing voor het ontbreken van secundaire indexen, kunt u meerdere exemplaren van elke entiteit met elk exemplaar met behulp van verschillende opslaan **PartitionKey** en **RowKey** waarden. Als u een entiteit met de structuur opslaat hieronder wordt weergegeven, kunt u efficiënt werknemer entiteiten op basis van e-adres of de werknemer-id ophalen Het voorvoegsel van de waarden voor de **PartitionKey**, "empid_" en 'email_' kunnen u bepalen welke index die u wilt gebruiken voor een query.  

![Index van primaire en secundaire index](media/storage-table-design-guide/storage-table-design-IMAGE10.png)


De volgende twee filtercriteria (één opzoeken op basis van de werknemer-ID en een opzoeken op basis van e-mailadres) opgeven point-query's:  

* $filter = (PartitionKey eq ' empid_Sales') en (RowKey eq '000223')
* $filter = (PartitionKey eq ' email_Sales') en (RowKey eq 'jonesj@contoso.com')  

Als u een query voor een bereik van de werknemer entiteiten, kunt u een bereik in werknemer-ID volgorde gesorteerd, of een bereik in e-mailadres volgorde gesorteerd op query's uitvoeren voor entiteiten met het juiste voorvoegsel in de **RowKey**.  

* Alle werknemers vinden op de afdeling verkoop met een werknemer-ID in het bereik **000100** naar **000199** gesorteerd werknemer-ID gebruiken: $filter = (PartitionKey eq ' empid_Sales') en (RowKey ge '000100') en (RowKey le '000199')  
* Alle werknemers vinden op de afdeling verkoop met een e-mailadres dat met 'a' gesorteerde e-mailadres gebruiken begint: $filter = (PartitionKey eq ' email_Sales') en (RowKey ge 'a') en (RowKey lt "b")  

Merk op dat de filtersyntaxis van de gebruikt in de bovenstaande voorbeelden uit de tabelservice REST-API voor meer informatie naar [entiteiten opvragen](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* U uw dubbele entiteiten uiteindelijk consistent zijn met elkaar kunt houden met behulp van de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) te handhaven van de primaire en secundaire index-entiteiten.  
* Tabelopslag is relatief goedkope te gebruiken, dus de kosten-overhead van het opslaan van dubbele gegevens mag geen groot belang. U moet echter altijd de kosten van het ontwerp op basis van uw verwachte opslagvereisten te evalueren en dubbele entiteiten ter ondersteuning van de query's die de clienttoepassing wordt uitgevoerd alleen toe te voegen.  
* De waarde die wordt gebruikt voor de **RowKey** moet uniek zijn voor elke entiteit. Overweeg het gebruik van de samengestelde sleutel waarden.  
* Opvulling van numerieke waarden in de **RowKey** (bijvoorbeeld de werknemer-ID 000223), kunnen sorteren en filteren op basis van de boven- en ondergrenzen corrigeren.  
* U hoeft niet per se te dupliceren van de eigenschappen van de entiteit. Bijvoorbeeld, als de query's die opzoeken van de entiteiten met behulp van het e-mailbericht adres in de **RowKey** hoeft u nooit leeftijd van de werknemer, deze entiteiten kunnen hebben de volgende structuur:
  
   ![Werknemer-entiteit (secundaire index)](media/storage-table-design-guide/storage-table-design-IMAGE11.png)

* Is het doorgaans beter dubbele gegevens opslaan en zorg ervoor dat u alle gegevens die u nodig hebt met één query dan gebruiken om een query te vinden van een entiteit met behulp van de secundaire index en een ander voor het opzoeken van de vereiste gegevens in de primaire index kunt ophalen.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer de clienttoepassing nodig heeft om op te halen van entiteiten met behulp van tal van verschillende sleutels, wanneer de client nodig heeft om op te halen van entiteiten in verschillende sorteervolgorde en waar u elke entiteit met behulp van een aantal unieke waarden kunt identificeren. Gebruik dit patroon wanneer u voorkomen dat de schaalbaarheidslimieten van de partitie dan wilt wanneer u entiteit zoekacties met behulp van de verschillende uitvoert **RowKey** waarden.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Intra-partitie secundaire index patroon](#intra-partition-secondary-index-pattern)  
* [Samengestelde sleutel patroon](#compound-key-pattern)  
* Entiteit-groepstransacties  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

## <a name="eventually-consistent-transactions-pattern"></a>Uiteindelijk consistent transacties patroon
Uiteindelijk consistent gedrag partitiegrenzen of wanneer de grenzen van de storage-systeem inschakelen met behulp van Azure-wachtrijen.  

### <a name="context-and-problem"></a>Context en probleem
Atomische transacties inschakelen EGTs voor meerdere entiteiten die dezelfde partitiesleutel delen. Voor betere prestaties en schaalbaarheid, u besluiten om op te slaan entiteiten met vereisten voor consistentie in afzonderlijke partities of in een afzonderlijke opslag: in zo'n geval moet u EGTs niet gebruiken om consistentie te garanderen. Bijvoorbeeld, mogelijk hebt u een vereiste voor het handhaven van de uiteindelijke consistentie tussen:  

* Entiteiten die zijn opgeslagen in twee verschillende partities in dezelfde tabel, in verschillende tabellen of in verschillende opslagaccounts.  
* Een entiteit die zijn opgeslagen in de Table-service en een blob die zijn opgeslagen in de Blob-service.  
* Een entiteit in de Table-service en een bestand opgeslagen in een bestandssysteem.  
* Een entiteit opslaan in de Table-service wordt nog geïndexeerd met behulp van de Azure Search-service.  

### <a name="solution"></a>Oplossing
Met behulp van Azure-wachtrijen, kunt u een oplossing die zorgt voor uiteindelijke consistentie tussen twee of meer partities of opslagsystemen implementeren.
Ter illustratie van deze benadering, wordt ervan uitgegaan dat u een vereiste voor het archiveren van oude werknemer entiteiten kunnen hebben. Oude werknemer entiteiten zelden worden opgevraagd en moeten worden uitgesloten van alle activiteiten die betrekking hebben op huidige werknemers. Voor het implementeren van deze vereiste slaat u actieve medewerkers in de **huidige** tabel en de oude werknemers in de **archief** tabel. Archiveren van een werknemer vereist dat u wilt verwijderen van de entiteit op basis van de **huidige** tabel en de entiteit moet toevoegen de **archief** tabel, maar u niet een EGT gebruiken voor het uitvoeren van deze twee bewerkingen. Om te voorkomen dat het risico dat een fout veroorzaakt een entiteit wordt weergegeven in beide of geen van beide tabellen, moet de archiveringsbewerking uiteindelijk consistent. De volgende reeksdiagram geeft een overzicht van de stappen in deze bewerking. Meer informatie is bedoeld voor uitzondering paden in de volgende tekst.  

![Oplossing voor Azure-wachtrijen](media/storage-table-design-guide/storage-table-design-IMAGE12.png)

Een client initieert de archiveringsbewerking door een bericht op een Azure-wachtrij, in dit voorbeeld voor het archiveren van werknemer #456. Een werkrol, peilt de wachtrij voor nieuwe berichten; Wanneer er een wordt gevonden, leest het bericht en een verborgen kopie van de wachtrij verlaat. Vervolgens haalt een kopie van de entiteit op basis van de werkrol de **huidige** tabel, voegt u een kopie in de **archief** tabel en verwijdert u vervolgens de oorspronkelijke uit de **huidige** tabel. Ten slotte, als er geen fouten uit de vorige stap zijn, de werkrol verborgen wordt het bericht verwijderd uit de wachtrij.  

In dit voorbeeld voegt stap 4 de werknemer in het **archief** tabel. Dit kan de werknemer toevoegen aan een blob in de Blob-service of een bestand in een bestandssysteem.  

### <a name="recovering-from-failures"></a>Herstellen van fouten
Het is belangrijk dat de bewerkingen in stappen **4** en **5** moet *idempotent* als de werkrol moet opnieuw worden opgestart de archiveringsbewerking. Als u de Table-service voor stap **4** moet u een bewerking 'invoegen of vervangen door'; voor de stap **5** moet u een ' verwijderen als bestaat "bewerking in de clientbibliotheek die u gebruikt. Als u van een andere opslagsysteem gebruikmaakt, moet u een juiste idempotente bewerking.  

Als de werkrol nooit stap is **6**, en vervolgens na een time-out voor het bericht opnieuw wordt weergegeven in de wachtrij die gereed zijn voor de werkrol om te proberen voor het opnieuw verwerken. De werkrol kunt controleren hoe vaak een bericht in de wachtrij is gelezen en, indien nodig, markering is een 'poison'-bericht voor onderzoek door deze naar een aparte wachtrij worden verzonden. Zie voor meer informatie over het lezen van berichten in wachtrij plaatsen en het aantal uit de wachtrij verwijderen te controleren, [berichten ophalen](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Aantal fouten in de tabel en wachtrij-services zijn tijdelijke fouten en de clienttoepassing moet bevatten geschikt Pogingslogica om deze te verwerken.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Deze oplossing biedt geen voor transactie-isolatie. Bijvoorbeeld, een client kan lezen de **huidige** en **archief** tabellen wanneer de werkrol tussen fasen is **4** en **5**, en een inconsistente weergave van de gegevens. Houd er rekening mee dat de gegevens consistent uiteindelijk is.  
* U moet ervoor dat de stappen 4 en 5 idempotent zijn om ervoor te zorgen van uiteindelijke consistentie.  
* U kunt de oplossing schalen met behulp van meerdere wachtrijen en instanties van de werkrol.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u een garantie voor de uiteindelijke consistentie tussen entiteiten die aanwezig zijn in andere partities of tabellen wilt maken. U kunt dit patroon om ervoor te zorgen uiteindelijke consistentie voor bewerkingen voor de Table-service, de Blob-service en andere Azure Storage-gegevensbronnen, zoals de database of het bestandssysteem kunt uitbreiden.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* Entiteit-groepstransacties  
* [Samenvoegen of vervangen](#merge-or-replace)  

> [!NOTE]
> Als de transactie-isolatieniveau is belangrijk dat uw oplossing, kunt u overwegen uw tabellen om te gebruiken EGTs opnieuw ontwerpen.  
> 
> 

## <a name="index-entities-pattern"></a>Patroon voor index-entiteiten
Onderhouden index entiteiten om in te schakelen van efficiënte zoekopdrachten die een lijst met entiteiten retourneren.  

### <a name="context-and-problem"></a>Context en probleem
De Table-service automatisch geïndexeerd entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing om op te halen van een entiteit efficiënt met een point-query. Bijvoorbeeld, met behulp van de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kan efficiënt een individuele werknemer entiteit ophalen met behulp van de naam van de afdeling en de werknemer-ID (de **PartitionKey** en **RowKey**).  

![Werknemer-entiteit](media/storage-table-design-guide/storage-table-design-IMAGE13.png)

Als u ook wilt kunnen worden opgehaald van een lijst met entiteiten van werknemer op basis van de waarde van een andere niet-unieke eigenschap, zoals hun achternaam, moet u een minder efficiënt partitie scan overeenkomsten in plaats van met behulp van een index om te controleren of ze rechtstreeks te vinden. Dit komt doordat de table-service geen secundaire indexen biedt.  

### <a name="solution"></a>Oplossing
Om in te schakelen lookup gesorteerd op achternaam met de entiteitsstructuur die hierboven wordt weergegeven, moet u lijsten met werknemer-id's onderhouden. Als u wilt ophalen van de werknemer entiteiten met een bepaalde achternaam op, zoals Jones, moet u eerst de lijst van de werknemer-id's vinden voor werknemers met Jones als hun achternaam en vervolgens deze entiteiten werknemer kunt ophalen. Er zijn drie manieren voor het opslaan van de lijsten van de werknemer-id's:  

* Blob-opslag gebruiken.  
* Index entiteiten maken in dezelfde partitie als de werknemer-entiteiten.  
* Index entiteiten maken in een afzonderlijke partitie of een tabel.  

<u>Optie #1: Blob storage gebruiken</u>  

Voor de eerste optie, u een blob maken voor elke unieke achternaam en in elke blob-archief een lijst van de **PartitionKey** (afdeling) en **RowKey** (werknemer-ID) waarden voor werknemers die die laatste naam. Wanneer u toevoegen of verwijderen van een werknemer moet u ervoor zorgen dat de inhoud van de relevante blob uiteindelijk consistent met de werknemer-entiteiten is.  

<u>Optie #2:</u> Index entiteiten maken in dezelfde partitie  

Gebruik voor de tweede optie, index-entiteiten die opslaan van de volgende gegevens:  

![Werknemer index entiteit](media/storage-table-design-guide/storage-table-design-IMAGE14.png)

De **EmployeeIDs** eigenschap bevat een lijst van de werknemer-id's voor werknemers met de achternaam die zijn opgeslagen in de **RowKey**.  

De volgende stappen wordt beschreven hoe die u volgen moet wanneer u een nieuwe werknemer toevoegen wilt als u van de tweede optie gebruikmaakt. In dit voorbeeld voegen we een werknemer met Id 000152 en een achternaam Jones op de afdeling verkoop toe:  

1. Ophalen van de entiteit index met een **PartitionKey** 'Verkoop'-waarde en de **RowKey** waarde "Jones". Sla de ETag van deze entiteit voor gebruik in stap 2.  
2. Maken van een entiteit groep transactie (dat wil zeggen, een batchbewerking) die de nieuwe werknemer-entiteit kan worden ingevoegd (**PartitionKey** 'Verkoop'-waarde en **RowKey** '000152'-waarde), en werkt de index-entiteit (**PartitionKey** 'Verkoop'-waarde en **RowKey** "Jones" waarde) door de nieuwe werknemer-ID toe te voegen aan de lijst in het veld EmployeeIDs. Zie voor meer informatie over de entiteit-groepstransacties entiteit-groepstransacties.  
3. Als de entiteit groep transactie is mislukt vanwege een optimistische gelijktijdigheid-fout (iemand anders heeft alleen de entiteit index gewijzigd), moet u opnieuw beginnen bij stap 1.  

U kunt een soortgelijke benadering als een werknemer verwijderen als u van de tweede optie gebruikmaakt gebruiken. Wijzigen van de achternaam van een werknemer is iets ingewikkelder omdat u moet voor het uitvoeren van een entiteit groep transactie die drie entiteiten worden bijgewerkt: de entiteit werknemer, de entiteit index voor de oude achternaam en de index-entiteit voor de nieuwe achternaam. Voordat u wijzigingen aanbrengt in om op te halen van de ETag-waarden die u vervolgens gebruiken kunt om uit te voeren van de updates met behulp van optimistische gelijktijdigheid, moet u elke entiteit ophalen.  

De volgende stappen wordt beschreven hoe die u volgen moet wanneer u nodig hebt om te controleren of alle werknemers die over een bepaalde achternaam van een afdeling als u van de tweede optie gebruikmaakt. In dit voorbeeld zijn we alle werknemers met achternaam Jones op de afdeling verkoop opzoeken:  

1. Ophalen van de entiteit index met een **PartitionKey** 'Verkoop'-waarde en de **RowKey** waarde "Jones".  
2. De lijst van werknemer-id's in het veld EmployeeIDs parseren.  
3. Als u aanvullende informatie over elk van deze werknemers (zoals hun e-mailadressen), ophalen van elk van de werknemer entiteiten met behulp van **PartitionKey** 'Verkoop'-waarde en **RowKey** waarden van de lijst met werknemers die u hebt verkregen in stap 2.  

<u>Optie #3:</u> Index entiteiten maken in een afzonderlijke partitie of tabel  

Gebruik voor de derde optie, index-entiteiten die opslaan van de volgende gegevens:  

![Werknemer index entiteit in een afzonderlijke partitie](media/storage-table-design-guide/storage-table-design-IMAGE15.png)


De **EmployeeIDs** eigenschap bevat een lijst van de werknemer-id's voor werknemers met de achternaam die zijn opgeslagen in de **RowKey**.  

Met de derde optie, kunt u EGTs niet gebruiken voor het handhaven van de consistentie omdat de index-entiteiten in een afzonderlijke partitie van de werknemer-entiteiten. U moet ervoor zorgen dat de index-entiteiten uiteindelijk consistent zijn met de werknemer-entiteiten.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Deze oplossing vereist ten minste twee query's naar overeenkomende entiteiten ophalen: een query uitvoeren op de index entiteiten voor de lijst van **RowKey** waarden en query's om op te halen van elke entiteit in de lijst.  
* Gezien het feit dat een afzonderlijke entiteit een maximale grootte van 1 MB heeft, optie #2 en de optie #3 in de oplossing wordt ervan uitgegaan dat de lijst van de werknemer-id's voor een bepaalde laatste nooit groter dan 1 MB is. Als de lijst van de werknemer-id's waarschijnlijk moet groter zijn dan 1 MB in grootte, optie #1 gebruiken en de indexgegevens opslaan in blob-opslag.  
* Als u de optie #2 gebruikt moet (met behulp van EGTs om af te handelen toevoegen en verwijderen van werknemers en het wijzigen van de achternaam van een werknemer) u nagaan of het volume van transacties worden de limieten voor schaalbaarheid in een bepaalde partitie benadering. Als dit het geval is, moet u rekening houden met een uiteindelijk consistente oplossing (optie #1 of optie #3) die wachtrijen gebruikt om de updateaanvragen te verwerken en kunt u uw index entiteiten opslaan in een afzonderlijke partitie van de werknemer-entiteiten.  
* Optie #2 in deze oplossing wordt ervan uitgegaan dat u wilt zoeken op achternaam binnen een afdeling: bijvoorbeeld, u wilt ophalen van een lijst van werknemers met een achternaam Jones op de afdeling verkoop. Als u mogelijk om te controleren of alle werknemers die over een achternaam Jones over de hele organisatie wilt, optie #1 of optie #3 gebruiken.
* U kunt een oplossing op basis van een wachtrij die zorgt voor uiteindelijke consistentie implementeren (Zie de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) voor meer informatie).  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u wilt dat voor het opzoeken van een set standaardentiteiten die alle een gemeenschappelijke eigenschapswaarde, zoals alle werknemers die over de achternaam Jones delen.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* Entiteit-groepstransacties  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

## <a name="denormalization-pattern"></a>Denormalisatie patroon
Gerelateerde gegevens samenvoegen in één enkele entiteit waarmee u kunt om op te halen van alle gegevens die u nodig hebt met een single point-query.  

### <a name="context-and-problem"></a>Context en probleem
In een relationele database, moet u doorgaans gegevens als u wilt verwijderen, wat resulteert in query's die gegevens uit meerdere tabellen ophalen kopiëren normaliseren. Als u uw gegevens in Azure-tabellen normaliseren, moet u meerdere retouren van de client naar de server om de gerelateerde gegevens te halen. Bijvoorbeeld, met de structuur van de tabel hieronder u moet twee retouren naar de details voor een afdeling ophalen: één voor het ophalen van de afdeling-entiteit die de manager bevat de ID en vervolgens op een andere aanvraag voor het ophalen van de manager van de details in een entiteit werknemer.  

![Afdeling entiteit en werknemer entiteit](media/storage-table-design-guide/storage-table-design-IMAGE16.png)

### <a name="solution"></a>Oplossing
In plaats van het opslaan van gegevens in twee afzonderlijke entiteiten, de gegevens denormaliseren en bewaar een kopie van de manager van de gegevens in de entiteit afdeling. Bijvoorbeeld:  

![Afdeling entiteit](media/storage-table-design-guide/storage-table-design-IMAGE17.png)

Met de afdeling entiteiten die met deze eigenschappen zijn opgeslagen, kunt u nu alle details die u nodig hebt over een afdeling in die met een point-query ophalen.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Er is enige overhead die is gekoppeld aan het opslaan van sommige gegevens twee keer kosten. De prestatievoordelen (die voortvloeien uit minder aanvragen aan de storage-service) doorgaans belangrijker is dan de marginale toename van de kosten voor opslag (en deze kosten is gedeeltelijk gecompenseerd door een vermindering van het aantal transacties die u nodig hebt om de details van een afdeling ophalen ).  
* U moet de consistentie van de twee entiteiten die opslaan van informatie over managers behouden. U kunt het probleem consistentie verwerken met behulp van EGTs om bij te werken van meerdere entiteiten in één atomische transactie: in dit geval wordt de entiteit afdeling en de werknemer-entiteit voor de afdelingsmanager worden opgeslagen in dezelfde partitie.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u vaak nodig hebt om te controleren of gerelateerde informatie. Dit patroon vermindert het aantal query's die de client aanbrengen moet in de gegevens die moet worden opgehaald.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* Entiteit-groepstransacties  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)

## <a name="compound-key-pattern"></a>Samengestelde sleutel patroon
Gebruik samengestelde **RowKey** waarden om in te schakelen van een client voor het opzoeken van gerelateerde gegevens met een single point-query.  

### <a name="context-and-problem"></a>Context en probleem
In een relationele database is het heel natuurlijke joins in query's gebruiken om te retourneren van gerelateerde onderdelen van de gegevens naar de client in één query. Bijvoorbeeld, kunt u de werknemer-ID om te controleren of een lijst van gerelateerde entiteiten die prestaties bevatten en controleer de gegevens voor die werknemer.  

Wordt ervan uitgegaan dat u bij het opslaan van werknemer entiteiten in de tabel-service met behulp van de volgende structuur:  

![De entiteitsstructuur werknemer](media/storage-table-design-guide/storage-table-design-IMAGE18.png)

U moet ook voor het opslaan van historische gegevens met betrekking tot beoordelingen en prestaties voor elk jaar die de werknemer heeft gewerkt voor uw organisatie en u moet toegang krijgen tot deze informatie per jaar. Een optie is om te maken van een andere tabel waarin entiteiten met de volgende structuur:  

![Alternatieve werknemer entiteitsstructuur](media/storage-table-design-guide/storage-table-design-IMAGE19.png)

U ziet dat met deze methode u besluiten om te dupliceren van bepaalde informatie (zoals de voornaam en achternaam) in de nieuwe entiteit waarmee u kunt uw gegevens met een enkele aanvraag op te halen. U kan geen sterke consistentie echter behouden omdat u niet een EGT gebruiken voor het bijwerken van de twee entiteiten atomisch.  

### <a name="solution"></a>Oplossing
Een nieuwe entiteitstype Store in de oorspronkelijke tabel met behulp van entiteiten met de volgende structuur:  

![Oplossing voor werknemer entiteitsstructuur](media/storage-table-design-guide/storage-table-design-IMAGE20.png)

U ziet hoe de **RowKey** is nu een samengestelde sleutel bestaat uit de werknemer-ID en het jaar van de gegevens waarmee u kunt de prestaties van de werknemer ophalen en gegevens met een enkele aanvraag voor een enkele entiteit bekijken.  

Het volgende voorbeeld bevat een overzicht van hoe u alle controle-gegevens kunt ophalen voor een bepaalde werknemer (zoals werknemer 000123 op de afdeling verkoop):  

$filter = (PartitionKey eq 'Verkoop') en (RowKey ge 'empid_000123') en (RowKey lt 'empid_000124') & $select = RowKey, Manager classificatie, classificatie van de Peer, opmerkingen  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Moet u een geschikte scheidingsteken waarmee u eenvoudig parseren van de **RowKey** waarde: bijvoorbeeld **000123_2012**.  
* U ook opslaat deze entiteit in dezelfde partitie als andere entiteiten die gerelateerde gegevens voor de dezelfde werknemer, wat betekent dat u EGTs kunt gebruiken bevatten dat voor het handhaven van de sterke consistentie.
* U moet rekening houden met hoe vaak u de gegevens om te bepalen of dit patroon geschikt is wordt opvragen.  Bijvoorbeeld, als u toegang de gegevens niet vaak worden en de gegevens van de belangrijkste werknemers vaak tot moet u ze als afzonderlijke entiteiten.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u nodig hebt voor het opslaan van een of meer entiteiten die u ophaalt vaak gerelateerde.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* Entiteit-groepstransacties  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  

## <a name="log-tail-pattern"></a>Logboek tail-patroon
Ophalen van de *n* entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** gesorteerd in omgekeerde datum en de volgorde van tijd-waarde.  

### <a name="context-and-problem"></a>Context en probleem
Een algemene vereiste is kunnen worden opgehaald van de meest recent gemaakte entiteiten, bijvoorbeeld de tien meest recente onkosten-claims verzonden door een werknemer. Tabel ondersteuning vraagt een **$top** tijdens de eerste query *n* entiteiten uit een set: Er is geen equivalent querybewerking om te retourneren van de laatste n entiteiten in een set.  

### <a name="solution"></a>Oplossing
Store de entiteiten met behulp van een **RowKey** dat op een natuurlijke manier sorteert in volgorde van de omgekeerde datum/tijd met behulp van zodat de meest recente vermelding is altijd het eerste item in de tabel.  

Bijvoorbeeld, om te kunnen ophalen van de tien meest recente onkosten claims verzonden door een werknemer, kunt u de waarde van een omgekeerde maatstreepjes afgeleid van de huidige datum en tijd. De volgende C#-codevoorbeeld toont één manier om te maken van een geschikte waarde 'tikken omgedraaid' voor een **RowKey** die van de meest recente worden gesorteerd op de oudste:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

U kunt teruggaan naar de datum en tijd met de volgende code:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

De query uitvoeren op tabel er zo uit:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* U moet de waarde van de omgekeerde maatstreepjes met voorloopnullen om te controleren of dat de tekenreekswaarde sorteert zoals verwacht worden opgevuld.  
* U moet rekening houden met de schaalbaarheidsdoelen op het niveau van een partitie. Wees voorzichtig hotspot partities niet te maken.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u nodig hebt voor toegang tot entiteiten in de volgorde van omgekeerde datum/tijd of wanneer u nodig hebt voor toegang tot de meest recent toegevoegde entiteiten.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Voeg / anti patroon toevoegen](#prepend-append-anti-pattern)  
* [Bij het ophalen van entiteiten](#retrieving-entities)  

## <a name="high-volume-delete-pattern"></a>Patroon van hoge volumes verwijderen
Het verwijderen van een groot aantal entiteiten inschakelen door op te slaan van de entiteiten voor gelijktijdige verwijdering in hun eigen afzonderlijke tabel. u kunt de entiteiten verwijderen door het verwijderen van de tabel.  

### <a name="context-and-problem"></a>Context en probleem
Veel toepassingen verwijderen oude gegevens die niet langer moet beschikbaar zijn voor een clienttoepassing, of die de toepassing naar een ander opslagmedium is gearchiveerd. U doorgaans dergelijke gegevens identificeren door een datum: bijvoorbeeld, u hebt een vereiste voor het verwijderen van records van alle aanmeldingsaanvragen die meer dan 60 dagen oud zijn.  

Een mogelijke ontwerp is het gebruik van de datum en tijd van de aanmeldingsaanvraag in de **RowKey**:  

![Datum en tijd van aanmelden](media/storage-table-design-guide/storage-table-design-IMAGE21.png)

Deze aanpak voorkomt partitie hotspots omdat de toepassing kunt invoegen en verwijderen van aanmelding entiteiten voor elke gebruiker in een afzonderlijke partitie. Deze methode kan echter zijn geld en tijd in beslag nemen als u een groot aantal entiteiten hebt omdat u eerst een tabelscan uitvoert om te achterhalen van de entiteiten moet te verwijderen en vervolgens moet u elke oude entiteit verwijderen. Houd er rekening mee dat u het aantal retouren naar de server die is vereist voor het verwijderen van de oude entiteiten van meerdere delete-aanvragen in EGTs batchverwerking kunt beperken.  

### <a name="solution"></a>Oplossing
Een afzonderlijke tabel gebruiken voor elke dag van aanmeldingspogingen. Kunt u het ontwerp van de entiteit hierboven om te voorkomen dat hotspots wanneer u entiteiten invoegen wilt en verwijderen van oude entiteiten nu gewoon een kwestie is van het verwijderen van één tabel elke dag (een enkel opslagaccount-bewerking) in plaats van zoeken en verwijderen van honderden of duizenden persoon aanmelding entiteiten elke dag.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Uw ontwerp biedt ondersteuning voor andere manieren om de gegevens zoals het opzoeken van bepaalde entiteiten koppelen met andere gegevens of genereren verzamelde gegevens worden gebruikt door uw toepassing?  
* Uw ontwerp dat hotspots voorkomen wanneer u nieuwe entiteiten wilt invoegen?  
* Verwacht een vertraging optreden als u de naam van de dezelfde tabel gebruiken wilt na het verwijderen. Is het beter om altijd te gebruiken unieke tabelnamen.  
* Verwacht dat sommige beperkingen wanneer u eerst een nieuwe tabel gebruiken terwijl de Table-service de toegangspatronen leert herkennen en de partities over knooppunten verdeelt. U moet rekening houden met hoe vaak moet u nieuwe tabellen maken.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer er een groot aantal entiteiten die u op hetzelfde moment moet verwijderen.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* Entiteit-groepstransacties
* [Entiteiten wijzigen](#modifying-entities)  

## <a name="data-series-pattern"></a>Patroon voor gegevens uit de serie
Store volledige gegevensreeks in één enkele entiteit te minimaliseren van het aantal aanvragen die u aanbrengt.  

### <a name="context-and-problem"></a>Context en probleem
Een veelvoorkomend scenario is voor een toepassing voor het opslaan van een reeks gegevens die normaal gesproken nodig is om op te halen in één keer. Uw toepassing kan bijvoorbeeld hoeveel IM-berichten die elke werknemer elk uur verzendt vastleggen en vervolgens deze informatie gebruiken om te tekenen het aantal berichten verzonden via de voorgaande 24 uur van elke gebruiker. Een ontwerp kan voor het opslaan van 24 entiteiten voor elke werknemer zijn:  

![Entiteiten voor elke werknemer Store 24 uur per dag](media/storage-table-design-guide/storage-table-design-IMAGE22.png)

Met dit ontwerp kunt u eenvoudig vinden en bijwerken van de entiteit bij te werken voor elke werknemer wanneer de toepassing moet de waarde voor aantal bijwerken. Als u wilt ophalen van de informatie voor het tekenen van een grafiek van de activiteit in de voorgaande 24 uur, moet u echter 24 entiteiten ophalen.  

### <a name="solution"></a>Oplossing
Gebruik het ontwerp van de volgende met een afzonderlijke eigenschap voor het opslaan van het aantal berichten voor elk uur:  

![Bericht statistieken entiteit](media/storage-table-design-guide/storage-table-design-IMAGE23.png)

Met dit ontwerp kunt u een samenvoeging voor het bijwerken van het aantal berichten voor een werknemer voor een bepaald uur. U kunt nu alle informatie die u nodig hebt om het diagram met behulp van een aanvraag voor een enkele entiteit te tekenen ophalen.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Als de volledige reeks past niet in een enkele entiteit (een entiteit kan maximaal 252 eigenschappen hebben), gebruikt u een alternatieve gegevensarchief, zoals een blob.  
* Als u meerdere clients tegelijkertijd bijwerken van een entiteit hebt, moet u gebruik van de **ETag** voor het implementeren van optimistische gelijktijdigheid. Als u veel clients hebt, kan hoog conflicten optreden.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u nodig hebt om te werken en een gegevensreeks die zijn gekoppeld aan een afzonderlijke entiteit ophalen.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Patroon voor grote instanties](#large-entities-pattern)  
* [Samenvoegen of vervangen](#merge-or-replace)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) (als u opslaan de gegevensreeks in een blob)  

## <a name="wide-entities-pattern"></a>Breed entiteiten patroon
Meerdere fysieke entiteiten gebruiken voor het opslaan van logische entiteiten met meer dan 252 eigenschappen.  

### <a name="context-and-problem"></a>Context en probleem
Een afzonderlijke entiteit kan maximaal 252 eigenschappen (met uitzondering van de verplichte eigenschappen) en niet meer dan 1 MB aan gegevens opslaan in totaal. In een relationele database, meestal krijgt u ronde beperkingen met betrekking tot de grootte van een rij door een nieuwe tabel toegevoegd en afdwingen van een 1-op-1-relatie tussen deze.  

### <a name="solution"></a>Oplossing
Met de Table-service, kunt u meerdere entiteiten om weer te geven van een object één grote bedrijven met meer dan 252 eigenschappen opslaan. Als u wilt voor het opslaan van een telling van het aantal IM-berichten verzonden door elke werknemer voor de afgelopen 365 dagen, kan u bijvoorbeeld het volgende ontwerp dat gebruikmaakt van twee entiteiten met verschillende schema's gebruiken:  

![Meerdere entiteiten](media/storage-table-design-guide/storage-table-design-IMAGE24.png)

Als u wilt een wijziging aanbrengt die is vereist voor het bijwerken van beide entiteiten om te voorkomen dat ze gesynchroniseerd met elkaar kunt u een EGT. Anders kunt u één samenvoegbewerking bijwerken van het aantal berichten voor een specifieke dag. Om op te halen van alle gegevens voor een individuele werknemer haalt u beide entiteiten die u kunt doen met twee efficiënte aanvragen die gebruikmaken van zowel een **PartitionKey** en een **RowKey** waarde.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Bij het ophalen van een volledige logische entiteit bestaat uit ten minste twee opslagtransacties: één voor elke fysieke entiteit ophalen.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer nodig voor het opslaan van entiteiten waarvan grootte of een aantal eigenschappen groter is dan de limieten voor een afzonderlijke entiteit in de Table-service.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* Entiteit-groepstransacties
* [Samenvoegen of vervangen](#merge-or-replace)

## <a name="large-entities-pattern"></a>Patroon voor grote instanties
Blob storage gebruiken voor het opslaan van grote eigenschapswaarden.  

### <a name="context-and-problem"></a>Context en probleem
Een afzonderlijke entiteit kan niet meer dan 1 MB aan gegevens opslaan in totaal. Als een of meer van de eigenschappen voor het opslaan van waarden die ertoe leiden de totale grootte van de entiteit dat moet groter zijn dan deze waarde, kunt u het geheel niet opslaan in de Table-service.  

### <a name="solution"></a>Oplossing
Als uw entiteit heeft, is groter dan 1 MB in grootte, omdat een of meer eigenschappen een grote hoeveelheid gegevens bevatten, kunt u gegevens opslaan in de Blob-service en het adres van de blob op te slaan in een eigenschap in de entiteit. U kunt bijvoorbeeld de foto van een werknemer opslaan in blobopslag en opslaan van een koppeling naar de foto in de **foto** eigenschap van de werknemer entiteit:  

![Foto-eigenschap](media/storage-table-design-guide/storage-table-design-IMAGE25.png)

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Voor het handhaven van de uiteindelijke consistentie tussen de entiteit in de Table-service en de gegevens in de Blob-service, gebruikt u de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) aan uw entiteiten beheren.
* Bij het ophalen van een volledige entiteit bestaat uit ten minste twee opslagtransacties: één om op te halen van de entiteit en één om op te halen van de blob-gegevens.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u nodig hebt voor het opslaan van entiteiten waarvan de grootte groter is dan de limieten voor een afzonderlijke entiteit in de Table-service.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Breed entiteiten patroon](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

## <a name="prependappend-anti-pattern"></a>Anti-patroon toevoegen/toevoegen
Meer schaalbaarheid wanneer er een groot aantal toevoegingen door de invoegingen spreiden over meerdere partities.  

### <a name="context-and-problem"></a>Context en probleem
Begin of u normaal gesproken entiteiten toevoegt aan uw opgeslagen entiteiten resulteert in de toepassing toe te voegen nieuwe entiteiten op de eerste of laatste partitie van een reeks van partities. In dit geval plaatsvinden alle van de invoegingen op een bepaald moment in dezelfde partitie, het maken van een hotspot waarmee wordt voorkomen de tabelservice van voegt taakverdeling over meerdere knooppunten dat, en mogelijk veroorzaakt door uw toepassing aan de schaalbaarheidsdoelen voor bereikt de partitie. Bijvoorbeeld, hebt u een toepassing die de logboeken voor netwerk- en toegang door werknemers, klikt u vervolgens de entiteitsstructuur van een, zoals hieronder weergegeven kan leiden tot het huidige uur partitie een hotspot wordt als het volume van transacties het schaalbaarheidsdoel van bereikt een afzonderlijke partitie:  

![Entiteitsstructuur](media/storage-table-design-guide/storage-table-design-IMAGE26.png)

### <a name="solution"></a>Oplossing
De volgende alternatieve entiteitsstructuur voorkomt een hotspot op een bepaalde partitie als de toepassingsgebeurtenissen worden vastgelegd:  

![Alternatieve entiteitsstructuur](media/storage-table-design-guide/storage-table-design-IMAGE27.png)

U ziet dat met dit voorbeeld van hoe u zowel de **PartitionKey** en **RowKey** samengestelde sleutels. De **PartitionKey** maakt gebruik van de afdeling en de werknemer-ID voor het distribueren van de logboekregistratie voor meerdere partities.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Biedt ondersteuning voor de alternatieve sleutel-structuur die voorkomt het maken van hot-partities op voegt efficiënt query's kunt u uw clienttoepassing?  
* Het verwachte volume van transacties betekent dat u waarschijnlijk de schaalbaarheidsdoelen voor een afzonderlijke partitie bereiken en worden beperkt door de storage-service?  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Vermijd het prepend/append anti-patroon wanneer het volume van transacties is, wat erin resulteert dat in de beperking van de storage-service wanneer u toegang een hot-partitie tot.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Logboek tail-patroon](#log-tail-pattern)  
* [Entiteiten wijzigen](#modifying-entities)  

## <a name="log-data-anti-pattern"></a>Logboek gegevens anti-patroon
Normaal gesproken moet u de Blob-service in plaats van de Table-service voor het opslaan van gegevens aan het logboek.  

### <a name="context-and-problem"></a>Context en probleem
Een algemene use-case voor logboekgegevens is om op te halen van een selectie van logboekvermeldingen voor een specifieke datum-/ tijdbereik: bijvoorbeeld, u wilt zoeken naar alle de fout en kritieke berichten die uw toepassing vastgelegd tussen 15:04 en 15:06 op een specifieke datum. U wilt niet de datum en tijd van het logboekbericht gebruiken om te bepalen van de partitie die opslaan van logboekbestanden entiteiten: die resulteert in een hot partitie omdat op een bepaald moment alle entiteiten in het logboek wordt dezelfde **PartitionKey** waarde (Zie de sectie [anti patroon Prepend/append](#prepend-append-anti-pattern)). De volgende entiteit-schema voor een logboekbericht resulteert bijvoorbeeld in een hot partitie omdat de toepassing alle berichten in het logboek op de partitie voor de huidige datum en uur schrijft:  

![Logboek bericht entiteit](media/storage-table-design-guide/storage-table-design-IMAGE28.png)

In dit voorbeeld wordt de **RowKey** bevat van de datum en tijd van het logboekbericht om ervoor te zorgen dat logboekberichten worden opgeslagen in datum/tijd-volgorde gesorteerd, en bevat een bericht-ID in het geval meerdere logboekberichten de dezelfde datum en tijd delen.  

Een andere benadering is het gebruik van een **PartitionKey** die ervoor zorgt dat de toepassing berichten op tal van partities schrijft. Als de bron van het logboekbericht een manier om berichten te verdelen over meerdere veel partities biedt, kan u bijvoorbeeld de volgende entiteit-schema gebruiken:  

![Alternatieve log bericht entiteit](media/storage-table-design-guide/storage-table-design-IMAGE29.png)

Het probleem met dit schema is echter om op te halen van de logboekberichten voor een bepaalde periode moet u elke partitie zoeken in de tabel.

### <a name="solution"></a>Oplossing
De vorige sectie is het probleem van het gebruik van de Table-service voor het opslaan van vermeldingen in het logboek en voorgestelde twee, ontoereikende, ontwerpen gemarkeerd. Een oplossing heeft geleid tot een hot Partitions tegen het risico van slechte prestaties logboekberichten; schrijven de andere oplossing leidde tot slechte prestaties vanwege de vereisten voor het scannen van elke partitie in de tabel om op te halen berichten in het logboek voor een bepaalde periode. BLOB-opslag biedt een betere oplossing voor dit soort scenario en dit is hoe Azure Storage Analytics winkels de logboekgegevens worden verzameld.  

In deze sectie geeft een overzicht van hoe Storage Analytics logboekgegevens opgeslagen in blob-opslag als een afbeelding van deze aanpak voor het opslaan van gegevens die u normaal gesproken query's met bereik uitvoeren.  

Storage Analytics slaat berichten in het logboek in een door tekens gescheiden indeling in meerdere blobs. De indeling scheidingstekens kunt eenvoudig een clienttoepassing parseren van de gegevens in het logboekbericht.  

Storage Analytics maakt gebruik van een naamgevingsconventie voor blobs waarmee u bij het zoeken van de blob (of -blobs) met de logboekberichten die u zoekt. Een blob met de naam 'queue/2014/07/31/1800/000001.log' bevat bijvoorbeeld logboekberichten die betrekking hebben op de queue-service voor het uur vanaf 18:00 uur op 31 juli 2014. De '000001' geeft aan dat dit het eerste logboekbestand voor deze periode. Storage Analytics registreert ook de tijdstempel van de eerste en laatste logboekberichten opgeslagen in het bestand als onderdeel van de metagegevens van de blob. De API voor blob storage kunt u blobs in een container op basis van een voorvoegsel vinden: als u wilt de blobs die gegevens van een wachtrij voor het starten om 18:00 uur bevatten vinden, kunt u het voorvoegsel "wachtrij/2014/07/31/1800."  

Storage Analytics buffers Meld berichten intern en vervolgens regelmatig updates van de juiste blob of maakt u een nieuw bestand met de laatste batch van vermeldingen in het logboek. Dit vermindert het aantal schrijft die deze naar de blob-service moet uitvoeren.  

Als u een soortgelijke oplossing in uw eigen toepassing implementeert, moet u bedenken hoe voor het beheren van de verhouding tussen de betrouwbaarheid (elke logboekvermelding schrijven naar blob-opslag als dit gebeurt) en de kosten en de schaalbaarheid (buffer-updates in uw toepassing en schrijven ze naar de blob storage in batches).  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen hoe gegevens worden opgeslagen:  

* Als u het ontwerp van een tabel die vermijdt u mogelijke hot-partities maakt, merkt u misschien dat u geen toegang uw logboekgegevens efficiënt tot.  
* Voor het verwerken van logboekgegevens, moet een client vaak veel records laden.  
* Hoewel logboekgegevens is vaak opgebouwd, kan de blob-opslag een betere oplossing zijn.  

## <a name="implementation-considerations"></a>Afwegingen bij de implementatie
In deze sectie worden enkele van de overwegingen op moet letten wanneer u de patronen die worden beschreven in de vorige secties implementeert beschreven. De meeste van deze sectie bevat voorbeelden die zijn geschreven in C# en die gebruikmaken van de Storage-clientbibliotheek (versie 4.3.0 op het moment van schrijven).  

## <a name="retrieving-entities"></a>Bij het ophalen van entiteiten
Zoals beschreven in de sectie ontwerp voor het uitvoeren van query's, is de meest efficiënte query een point-query. In sommige scenario's moet u mogelijk echter meerdere entiteiten ophalen. Deze sectie beschrijft enkele algemene strategieën voor het ophalen van entiteiten met behulp van de Storage-clientbibliotheek.  

### <a name="executing-a-point-query-using-the-storage-client-library"></a>Uitvoeren van een point-query met de Storage-clientbibliotheek
De eenvoudigste manier om het uitvoeren van een point-query is met de **ophalen** bewerking tabel, zoals wordt weergegeven in het volgende C# codefragment die worden opgehaald van een entiteit met een **PartitionKey** van de waarde 'Verkoop' en een  **RowKey** van de waarde '212':  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

U ziet hoe dit voorbeeld wordt verwacht dat de entiteit wordt opgehaald om te worden van het type **EmployeeEntity**.  

### <a name="retrieving-multiple-entities-using-linq"></a>Bij het ophalen van meerdere entiteiten met behulp van LINQ
U kunt meerdere entiteiten ophalen met behulp van LINQ met Storage-clientbibliotheek en op te geven van een query met een **waar** component. Om te voorkomen dat een tabelscan, moet u altijd opnemen de **PartitionKey** waarde in de where-component, en indien mogelijk de **RowKey** waarde om te voorkomen dat de tabel en partitie scans. De tabelservice ondersteunt een beperkt aantal vergelijkingsoperators (groter is dan, groter dan of gelijk aan, minder dan, kleiner dan of gelijk zijn aan, gelijk zijn, en niet gelijk aan) te gebruiken in de where component. De volgende C#-codefragment vindt alle werknemers waarvan de laatste naam begint met "B" (ervan uitgaande dat de **RowKey** bevat de naam van de laatste) van de afdeling verkoop (ervan uitgaande dat de **PartitionKey** slaat de afdelingsnaam):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

U ziet hoe in de query is zowel een **RowKey** en een **PartitionKey** om betere prestaties te garanderen.  

Het volgende codevoorbeeld toont dezelfde functionaliteit met de fluent API (Zie voor meer informatie over de fluent API's in het algemeen [aanbevolen procedures voor het ontwerpen van een Fluent API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> Het voorbeeld worden genest meerdere **CombineFilters** methoden om op te nemen van de drie filtervoorwaarden.  
> 
> 

### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Bij het ophalen van grote aantallen entiteiten op basis van een query
Een optimale query retourneert een afzonderlijke entiteit op basis van een **PartitionKey** waarde en een **RowKey** waarde. In sommige scenario's kan u echter een vereiste om terug te keren van vele entiteiten van dezelfde partitie of zelfs van veel partities hebben.  

U moet de prestaties van uw toepassing altijd volledig hebt getest in dergelijke scenario's.  

Een query op de table-service kan maximaal 1000 entiteiten in één keer worden geretourneerd en kan worden uitgevoerd voor een maximum van vijf seconden. Als de resultatenset bevat meer dan 1000 entiteiten als de query is niet voltooid binnen de vijf seconden, of als de query overschrijdt de grens van de partitie, retourneert de Table-service een vervolgtoken zodat de clienttoepassing om aan te vragen van de volgende set entiteiten. Zie voor meer informatie over hoe voortzetting van tokens voor werk, [time-out voor query's en van paginering](https://msdn.microsoft.com/library/azure/dd135718.aspx).  

Als u van de Storage-clientbibliotheek gebruikmaakt, kan deze voortzetting van tokens automatisch voor u verwerken, zoals het retourneert entiteiten uit de tabelservice. De volgende C# voorbeeldcode met behulp van de Storage-clientbibliotheek automatisch verwerkt voortzetting van tokens als de table-service deze in een antwoord geretourneerd:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

De volgende C#-code wordt expliciet voortzetting van tokens verwerkt:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Met behulp van expliciet voortzetting van tokens, kunt u bepalen wanneer het volgende segment van de gegevens in uw toepassing worden opgehaald. Bijvoorbeeld, als uw clienttoepassing kan gebruikers door de entiteiten die zijn opgeslagen in een tabel, een gebruiker beslissen niet op de pagina met alle entiteiten die zijn opgehaald door de query, zodat uw toepassing een vervolgtoken alleen gebruiken zou om op te halen van de volgende segment wanneer de gebruiker was paging via alle entiteiten in het huidige segment is voltooid. Deze benadering heeft verschillende voordelen:  

* Hiermee kunt u de hoeveelheid gegevens moeten worden opgehaald uit de tabelservice beperken en die u via het netwerk verplaatsen.  
* Hiermee kunt u voor het uitvoeren van asynchrone i/o in .NET.  
* Hiermee kunt u het serialiseren van het vervolgtoken naar de permanente opslag, zodat u in het geval van een toepassing is vastgelopen kunt.  

> [!NOTE]
> Een vervolgtoken retourneert doorgaans een segment met 1000 entiteiten, hoewel het mogelijk minder. Dit is ook het geval als u het aantal vermeldingen dat een query met behulp van retourneert beperken **nemen** op de eerste n entiteiten retourneren die overeenkomen met uw criteria lookup: de table-service kan retourneren een segment met minder dan n entiteiten samen met een het vervolgtoken waarmee u kunt de resterende entiteiten ophalen.  
> 
> 

De volgende C#-code laat zien hoe het aantal entiteiten geretourneerd binnen een segment wijzigen:  

```csharp
employeeQuery.TakeCount = 50;  
```

### <a name="server-side-projection"></a>Server-side-projectie
Een enkele entiteit kan maximaal 255 eigenschappen hebben en maximaal 1 MB in grootte. Wanneer u een query uitvoeren op de tabel en entiteiten ophalen, moet u wellicht niet alle eigenschappen en kunt voorkomen dat het overdragen van gegevens op onnodig (zodat minder latentie en lagere kosten). U kunt de serverzijde projectie gebruiken om over te dragen alleen de eigenschappen die u nodig hebt. Het volgende voorbeeld haalt is alleen de **e** eigenschap (samen met **PartitionKey**, **RowKey**, **tijdstempel**, en **ETag**) van de entiteiten die door de query is geselecteerd.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

U ziet hoe de **RowKey** waarde is beschikbaar, zelfs als deze niet is opgenomen in de lijst met eigenschappen om op te halen.  

## <a name="modifying-entities"></a>Entiteiten wijzigen
De Storage-clientbibliotheek kunt u uw entiteiten die zijn opgeslagen in de table-service door het invoegen, verwijderen en bijwerken van entiteiten wijzigen. EGTs kunt u batch-meerdere insert, update en delete-bewerkingen samen gebruikt om toegang te beperken het aantal retouren zijn vereist en verbeter de prestaties van uw oplossing.  

Houd er rekening mee dat uitzonderingen bij de Storage-clientbibliotheek uitvoeren van een EGT meestal bevatten de index van de entiteit die niet voldoen aan van de batch heeft veroorzaakt. Dit is handig wanneer u de code die gebruikmaakt van EGTs foutopsporing.  

U moet ook overwegen hoe uw ontwerp is van invloed op hoe uw clienttoepassing omgaat met gelijktijdigheid en update-bewerkingen.  

### <a name="managing-concurrency"></a>Gelijktijdigheid beheren
Standaard de table-service implementeert optimistische gelijktijdigheid wordt gecontroleerd op het niveau van afzonderlijke entiteiten voor **invoegen**, **samenvoegen**, en **verwijderen** bewerkingen, hoewel deze is het mogelijk voor een client om af te dwingen de table-service om deze controles over te slaan. Zie voor meer informatie over hoe de tabelservice beheert de gelijktijdigheid [gelijktijdigheid beheren in Microsoft Azure Storage](../../storage/common/storage-concurrency.md).  

### <a name="merge-or-replace"></a>Samenvoegen of vervangen
De **vervangen** -methode van de **TableOperation** klasse altijd vervangen door de volledige entiteit in de Table-service. Als u niet een eigenschap in de aanvraag wanneer deze eigenschap in de opgeslagen entiteit bestaat, wordt de aanvraag die eigenschap verwijderd uit de opgeslagen entiteit. Tenzij u expliciet verwijderen van een eigenschap van een opgeslagen entiteit wilt, moet u elke eigenschap opnemen in de aanvraag.  

U kunt de **samenvoegen** -methode van de **TableOperation** klasse te verminderen de hoeveelheid gegevens die u naar de Table-service verzendt wanneer u wilt bijwerken van een entiteit. De **samenvoegen** methode alle eigenschappen in de opgeslagen entiteit vervangen door waarden van eigenschappen van de entiteit is opgenomen in de aanvraag, maar blijven intact alle eigenschappen in de opgeslagen entiteit die niet zijn opgenomen in de aanvraag. Dit is handig als u beschikt over grote entiteiten en alleen hoeft te werken van een klein aantal eigenschappen in een aanvraag.  

> [!NOTE]
> De **vervangen** en **samenvoegen** twee methoden mislukken als de entiteit niet bestaat. Als alternatief kunt u de **InsertOrReplace** en **InsertOrMerge** methoden die een nieuwe entiteit maken als deze niet bestaat.  
> 
> 

## <a name="working-with-heterogeneous-entity-types"></a>Werken met heterogene Entiteitstypen
De Table-service is een *zonder schema* tabelarchief betekent dit dat één tabel entiteiten van meerdere typen bieden hoge mate van flexibiliteit bij het ontwerpen kunt opslaan. Het volgende voorbeeld wordt een tabel zowel werknemers als afdeling entiteiten opslaan:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Tijdstempel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Naam van de afdeling</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Houd er rekening mee dat elke entiteit moet nog steeds **PartitionKey**, **RowKey**, en **Timestamp** waarden, maar hebben mogelijk een set eigenschappen. Daarnaast wordt is er niets om weer te geven van het type van een entiteit, tenzij u die gegevens ergens op te slaan. Er zijn twee opties voor het identificeren van het entiteitstype:  

* Toevoegen van het entiteitstype in de **RowKey** (of mogelijk de **PartitionKey**). Bijvoorbeeld, **EMPLOYEE_000123** of **DEPARTMENT_SALES** als **RowKey** waarden.  
* Een afzonderlijke eigenschap gebruiken om vast te leggen van het entiteitstype, zoals wordt weergegeven in de onderstaande tabel.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Tijdstempel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>Werknemer</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>Werknemer</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Naam van de afdeling</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Afdeling</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>Werknemer</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

De eerste optie, begin van de entiteit type de **RowKey**, is handig als er een mogelijkheid is die twee entiteiten van verschillende typen met dezelfde sleutelwaarde heeft mogelijk. Deze groepen ook entiteiten van hetzelfde type samen in de partitie.  

De technieken beschreven in deze sectie zijn vooral relevant zijn voor de discussie [relaties voor overname](table-storage-design-modeling.md#inheritance-relationships) eerder in deze handleiding in het artikel [waardoor relaties worden gemaakt](table-storage-design-modeling.md).  

> [!NOTE]
> U moet rekening houden met inbegrip van een uniek versienummer in de waarde van het type entiteit waarmee clienttoepassingen POCO objecten ontwikkelen en werken met verschillende versies.  
> 
> 

De rest van deze sectie beschrijft een aantal van de functies in de Storage-clientbibliotheek werken met meerdere Entiteitstypen in dezelfde tabel kunnen.  

### <a name="retrieving-heterogeneous-entity-types"></a>Bij het ophalen van heterogene Entiteitstypen
Als u van de Storage-clientbibliotheek gebruikmaakt, hebt u drie opties voor het werken met meerdere Entiteitstypen.  

Als u welk type van de entiteit die zijn opgeslagen met een specifieke weet **RowKey** en **PartitionKey** waarden, en vervolgens u het entiteitstype opgeven kunt wanneer u de entiteit ophalen, zoals wordt weergegeven in de vorige twee voorbeelden die entiteiten van het type ophalen **EmployeeEntity**: [Uitvoeren van een point-query met de Storage-clientbibliotheek](#executing-a-point-query-using-the-storage-client-library) en [bij het ophalen van meerdere entiteiten met behulp van LINQ](#retrieving-multiple-entities-using-linq).  

De tweede optie is om het gebruik van de **DynamicTableEntity** type (een eigenschappenverzameling) in plaats van een concreet POCO entiteitstype (deze optie kan ook de prestaties verbeteren omdat niet hoeft te serialiseren en deserialiseren van de entiteit naar .NET-typen). De volgende C#-code mogelijk meerdere entiteiten van verschillende typen opgehaald uit de tabel, maar retourneert alle entiteiten als **DynamicTableEntity** exemplaren. Vervolgens wordt de **EntityType** eigenschap om te bepalen van het type van elke entiteit:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Houd er rekening mee dat als u wilt ophalen van andere eigenschappen moet u de **TryGetValue** methode voor het **eigenschappen** eigenschap van de **DynamicTableEntity** klasse.  

Een derde optie is om te combineren met behulp van de **DynamicTableEntity** type en een **EntityResolver** exemplaar. Hiermee kunt u omzetten in meerdere POCO-typen in dezelfde query. In dit voorbeeld wordt de **EntityResolver** gemachtigde is met behulp van de **EntityType** eigenschap onderscheid maken tussen de twee typen entiteit die de query retourneert. De **oplossen** methode gebruikt de **conflictoplosser** gemachtigde om op te lossen **DynamicTableEntity** exemplaren aan **TableEntity** exemplaren.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

### <a name="modifying-heterogeneous-entity-types"></a>Heterogene Entiteitstypen wijzigen
U hoeft niet te weten van het type van een entiteit te verwijderen en u het type van een entiteit altijd weet wanneer u deze invoegt. U kunt echter **DynamicTableEntity** type voor het bijwerken van een entiteit zonder te weten van het type en zonder een POCO-entiteitsklasse. Het volgende codevoorbeeld haalt een enkele entiteit en controleert de **EmployeeCount** eigenschap bestaat voordat deze wordt bijgewerkt.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

## <a name="controlling-access-with-shared-access-signatures"></a>Beheren van toegang met handtekeningen voor gedeelde toegang
U kunt Shared Access Signature (SAS)-tokens gebruiken om in te schakelen van clienttoepassingen te wijzigen (query) tabelitems zonder de noodzaak om op te nemen van sleutel van uw opslagaccount in uw code. Er zijn drie belangrijke voordelen voor het gebruik van SAS in uw toepassing:  

* U hoeft niet te verdelen van de sleutel van uw opslagaccount naar een niet-beveiligd platform (zoals een mobiel apparaat) als u wilt toestaan dat het apparaat worden geopend en gewijzigd van entiteiten in de tabel-service.  
* U kunt offloaden deel van het werk die web-en werkrollen uitvoeren bij het beheren van uw entiteiten naar clientapparaten, zoals computers van eindgebruikers en mobiele apparaten.  
* U kunt een beperkte en tijd beperkt set machtigingen voor een client (zoals alleen-lezen toegang tot specifieke resources toe te staan).  

Zie voor meer informatie over het gebruik van SAS-tokens met de Table-service [met behulp van Shared Access Signatures (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Echter, moet u nog steeds de SAS-tokens die een clienttoepassing aan de entiteiten in de tabelservice verlenen genereren: u moet dit doen in een omgeving met veilige toegang tot de sleutels van uw storage-account. Meestal gebruikt u een web- of worker-rol voor het genereren van de SAS-tokens en levert u deze aan de clienttoepassingen die toegang nodig tot uw entiteiten. Aangezien er nog steeds een overhead die betrokken zijn bij het genereren en het leveren van SAS-tokens aan clients, kunt u overwegen het beste aan deze overhead, met name in scenario's met hoge volumes verminderen.  

Het is mogelijk om een SAS-token die toegang tot een subset van de entiteiten in een tabel verleent te genereren. U maakt standaard een SAS-token voor een hele tabel, maar het is ook mogelijk om op te geven dat de SAS-token verleent toegang tot een bereik van **PartitionKey** waarden of een bereik van **PartitionKey** en **RowKey** waarden. U kunt selecteren voor het genereren van SAS-tokens voor afzonderlijke gebruikers van uw systeem dat de SAS-token van elke gebruiker alleen kan ze toegang tot hun eigen entiteiten in de table-service.  

## <a name="asynchronous-and-parallel-operations"></a>Asynchrone en parallelle bewerkingen
Indien u uw aanvragen over meerdere partities spreiden bent, kunt u de doorvoer en client reactiesnelheid met behulp van asynchrone of parallelle query's verbeteren.
Bijvoorbeeld, mogelijk hebt u twee of meer instanties van de werkrol toegang krijgen tot uw tabellen parallel. U kan hebben afzonderlijke werkrollen die verantwoordelijk is voor bepaalde sets partities of gewoon hebben meerdere instanties van de werkrol, elk toegang kunnen krijgen tot alle partities in een tabel.  

U kunt binnen een clientexemplaar doorvoer verbeteren door opslagbewerkingen asynchroon uitvoeren. De Storage-clientbibliotheek kunt eenvoudig schrijven asynchrone query's en wijzigingen. U kunt bijvoorbeeld beginnen met de synchrone methode die alle entiteiten in een partitie zoals wordt weergegeven in de volgende C#-code opgehaald:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

Deze code kunt u eenvoudig wijzigen zodat de query wordt asynchroon als volgt uitgevoerd:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

In dit voorbeeld asynchrone ziet u de volgende wijzigingen van de synchrone versie:  

* Handtekening van de methode bevat nu de **asynchrone** modifier en retourneert een **taak** exemplaar.  
* In plaats van aanroepen de **ExecuteSegmented** methode voor het ophalen van resultaten, nu de methode roept de **ExecuteSegmentedAsync** methode en maakt gebruik van de **await** modifier naar resultaten asynchroon ophalen.  

De clienttoepassing kan deze methode niet aanroepen meerdere keren (met verschillende waarden voor de **afdeling** parameter), en elke query wordt uitgevoerd op een afzonderlijke thread.  

Houd er rekening mee dat er geen asynchrone versie van is de **Execute** methode in de **TableQuery** klasse omdat de **IEnumerable** interface biedt geen ondersteuning voor asynchrone opsomming.  

U kunt ook invoegen, bijwerken en verwijderen van entiteiten asynchroon. De volgende C#-voorbeeld ziet u een eenvoudige, synchrone methode voor het invoegen of vervangen van een werknemer entiteit:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Deze code kunt u eenvoudig wijzigen zodat de update wordt asynchroon als volgt uitgevoerd:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

In dit voorbeeld asynchrone ziet u de volgende wijzigingen van de synchrone versie:  

* Handtekening van de methode bevat nu de **asynchrone** modifier en retourneert een **taak** exemplaar.  
* In plaats van aanroepen de **Execute** methode om bij te werken van de entiteit, nu de methode roept de **ExecuteAsync** methode en maakt gebruik van de **await** modifier resultaten ophalen asynchroon.  

De clienttoepassing meerdere asynchrone methoden zoals deze kunt aanroepen, en elke methodeaanroep wordt uitgevoerd op een afzonderlijke thread.  

## <a name="next-steps"></a>Volgende stappen

- [Waardoor relaties worden gemaakt](table-storage-design-modeling.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Versleutelen van gegevens in een tabel](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)
