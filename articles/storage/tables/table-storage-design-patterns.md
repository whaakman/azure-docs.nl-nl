---
title: Azure-opslag tabel ontwerppatronen | Microsoft Docs
description: Patronen voor oplossingen voor Azure table-service gebruiken.
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
ms.openlocfilehash: 0d098b7befe5426db4aff503e9633623b1249dbf
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660830"
---
# <a name="table-design-patterns"></a>Ontwerppatronen voor tabel
In dit artikel beschrijft alle patronen geschikt voor gebruik met oplossingen voor tabel-service. Ook ziet u hoe u enkele van de problemen en de verschillen besproken in andere tabelartikelen voor opslag ontwerpen vrijwel kunt oplossen. Het volgende diagram geeft een overzicht van de relaties tussen de verschillende patronen:  

![om gerelateerde gegevens te zoeken](media/storage-table-design-guide/storage-table-design-IMAGE05.png)


De kaart patroon hierboven licht sommige relaties tussen patronen (blauw) en anti patronen (oranje) die in deze handleiding worden beschreven. Er zijn veel andere patronen die moeten overwogen worden. Bijvoorbeeld, een van de belangrijkste scenario's voor tabel-Service is met de [gematerialiseerd weergave patroon](https://msdn.microsoft.com/library/azure/dn589782.aspx) van de [opdracht Query verantwoordelijkheid scheiding (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) patroon.  

## <a name="intra-partition-secondary-index-pattern"></a>Intra-partitie secundaire index patroon
Opslaan van meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden. Updates tussen kopieën consistent kunnen worden gehouden met behulp van EGT.  

### <a name="context-and-problem"></a>Context en probleem
De tabelservice indexeert automatisch entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing voor het ophalen van een entiteit efficiënt gebruik maakt van deze waarden. Bijvoorbeeld, met de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kunt gebruiken een punt-query voor een afzonderlijke werknemer entiteit ophalen met de afdelingsnaam van de en de werknemer-ID (de **PartitionKey** en **RowKey**  waarden). Een client kan ook gesorteerd op werknemer-ID binnen elke afdeling entiteiten ophalen.

![Image06](media/storage-table-design-guide/storage-table-design-IMAGE06.png)

Als u ook kunnen wilt vinden van een werknemer entiteit op basis van de waarde van een andere eigenschap, zoals e-mailadres, moet u een minder efficiënte partitie scan te vinden. Dit is omdat de tabelservice geen secundaire indexen biedt. Er is bovendien geen optie voor het aanvragen van een lijst met werknemers gesorteerd in een andere volgorde dan **RowKey** volgorde.  

### <a name="solution"></a>Oplossing
U kunt het ontbreken van een secundaire indexen omzeilen, kunt u meerdere exemplaren van elke entiteit bij elk exemplaar met een andere opslaan **RowKey** waarde. Als u een entiteit met de onderstaande structuren opslaat, kunt u efficiënt ophalen werknemer entiteiten op basis van e-mailadres of de werknemer-ID. Het voorvoegsel waarden voor de **RowKey**, 'empid_' en 'email_', kunt u zoeken naar één werknemer of een bereik van werknemers met behulp van een bereik van e-mailadressen of werknemer-id's.  

![Werknemer-entiteiten](media/storage-table-design-guide/storage-table-design-IMAGE07.png)

De volgende twee filtercriteria (één opzoeken op basis van de werknemer-ID en een opzoeken op basis van e-mailadres) Geef beide punt query's:  

* $filter = (PartitionKey eq 'Sales') en (RowKey eq 'empid_000223')  
* $filter = (PartitionKey eq 'Sales') en (RowKey eq 'email_jonesj@contoso.com')  

Als u een query voor een bereik van de werknemer entiteiten uitvoert, kunt u een bereik gesorteerd in volgorde van de werknemer-ID of een bereik in e-mailadres volgorde gesorteerd op basis van een query uitvoert voor entiteiten met het juiste voorvoegsel op in de **RowKey**.  

* Om te zoeken naar alle werknemers van de afdeling verkoop, met een werknemer-ID in het bereik 000100-000199 gebruik: $filter = (PartitionKey eq 'Sales') en (RowKey ge 'empid_000100') en (RowKey RP 'empid_000199')  
* Alle medewerkers van de verkoopafdeling vinden met een e-mailadres begint met de letter "a" gebruik: $filter = (PartitionKey eq 'Sales') en (RowKey ge 'email_a') en (RowKey lt 'email_b')  
  
  Opmerking: de filtersyntaxis gebruikt in de bovenstaande voorbeelden uit de tabelservice REST API, Zie voor meer informatie is [Query entiteiten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Table storage is relatief goedkope gebruiken zodat de overhead van de kosten voor het opslaan van dubbele gegevens mag geen groot belang. U moet echter altijd de kosten van het ontwerp op basis van de verwachte opslagvereisten geëvalueerd en alleen toevoegen dubbele entiteiten ter ondersteuning van de query's die de clienttoepassing wordt uitgevoerd.  
* Omdat de secundaire index entiteiten worden opgeslagen in dezelfde partitie als de oorspronkelijke entiteiten, moet u ervoor zorgen dat u de schaalbaarheidsdoelen voor een afzonderlijke partitie niet overschrijden.  
* U kunt uw dubbele entiteiten consistent zijn met elkaar houden met behulp van EGTs moment bijwerken van de twee kopieën van de entiteit. Dit betekent dat alle exemplaren van een entiteit in dezelfde partitie moeten worden opgeslagen. Zie voor meer informatie de sectie [entiteitstransacties met behulp van](table-storage-design.md#entity-group-transactions).  
* De waarde voor de **RowKey** moet uniek zijn voor elke entiteit. Overweeg het gebruik van de samengestelde sleutel komt.  
* Opvulling numerieke waarden in de **RowKey** (bijvoorbeeld de werknemer-ID 000223), kunt sorteren en filteren op basis van hoofdletters en de ondergrenzen corrigeren.  
* U hoeft hoeft niet te dupliceren van de eigenschappen van de entiteit. Bijvoorbeeld, als de query's die lookup de entiteiten die u het e-adres in de **RowKey** nooit moet de leeftijd van de werknemer, deze entiteiten kunnen de volgende structuur:

   ![Werknemer entiteit structuur](media/storage-table-design-guide/storage-table-design-IMAGE08.png)


* Is het doorgaans beter dubbele gegevens opslaan en ervoor te zorgen dat u alle gegevens die u nodig hebt met een enkele query kunt ophalen dan het gebruik van een query naar een entiteit en een andere voor het opzoeken van de vereiste gegevens.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer uw clienttoepassing moet met een aantal verschillende sleutels wanneer de client hoeft op te halen van entiteiten in een andere sorteervolgorde entiteiten ophalen en waar u elke entiteit met een aantal unieke waarden kunt identificeren. U moet echter zeker van te zijn dat de limieten voor schaalbaarheid van partitie niet te overschrijden wanneer u de entiteit zoekacties met behulp van de verschillende uitvoert **RowKey** waarden.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Secundaire tussen partitioneren van index patroon](#inter-partition-secondary-index-pattern)
* [Samengestelde sleutel patroon](#compound-key-pattern)
* [Entiteit groep transacties](#entity-group-transactions)
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)

## <a name="inter-partition-secondary-index-pattern"></a>Secundaire tussen partitioneren van index patroon
Opslaan van meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden in afzonderlijke partities of in afzonderlijke tabellen inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  

### <a name="context-and-problem"></a>Context en probleem
De tabelservice indexeert automatisch entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing voor het ophalen van een entiteit efficiënt gebruik maakt van deze waarden. Bijvoorbeeld, met de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kunt gebruiken een punt-query voor een afzonderlijke werknemer entiteit ophalen met de afdelingsnaam van de en de werknemer-ID (de **PartitionKey** en **RowKey**  waarden). Een client kan ook gesorteerd op werknemer-ID binnen elke afdeling entiteiten ophalen.  

![Werknemer-id](media/storage-table-design-guide/storage-table-design-IMAGE09.png)

Als u ook kunnen wilt vinden van een werknemer entiteit op basis van de waarde van een andere eigenschap, zoals e-mailadres, moet u een minder efficiënte partitie scan te vinden. Dit is omdat de tabelservice geen secundaire indexen biedt. Er is bovendien geen optie voor het aanvragen van een lijst met werknemers gesorteerd in een andere volgorde dan **RowKey** volgorde.  

U bent anticiperen op een zeer groot aantal transacties tegen deze entiteiten en wilt de risico's van de tabelservice beperking van de client.  

### <a name="solution"></a>Oplossing
U kunt het ontbreken van een secundaire indexen omzeilen, kunt u meerdere exemplaren van elke entiteit met het gebruik van elk exemplaar andere opslaan **PartitionKey** en **RowKey** waarden. Als u een entiteit met de onderstaande structuren opslaat, kunt u efficiënt ophalen werknemer entiteiten op basis van e-mailadres of de werknemer-ID. Het voorvoegsel waarden voor de **PartitionKey**, 'empid_' en 'email_' kunt u bepalen welke index die u wilt gebruiken voor een query.  

![Index van primaire en secundaire index](media/storage-table-design-guide/storage-table-design-IMAGE10.png)


De volgende twee filtercriteria (één opzoeken op basis van de werknemer-ID en een opzoeken op basis van e-mailadres) Geef beide punt query's:  

* $filter = (PartitionKey eq ' empid_Sales') en (RowKey eq '000223')
* $filter = (PartitionKey eq ' email_Sales') en (RowKey eq 'jonesj@contoso.com')  

Als u een query voor een bereik van de werknemer entiteiten uitvoert, kunt u een bereik gesorteerd in volgorde van de werknemer-ID of een bereik in e-mailadres volgorde gesorteerd op basis van een query uitvoert voor entiteiten met het juiste voorvoegsel op in de **RowKey**.  

* Alle medewerkers vinden in de afdeling verkoop met een werknemer-ID in het bereik **000100** naar **000199** in gebruik door werknemers-ID volgorde gesorteerd: $filter = (PartitionKey eq ' empid_Sales') en (RowKey ge '000100') en (RowKey RP '000199')  
* Alle medewerkers vinden in de afdeling verkoop met een e-mailadres dat met "a" gesorteerde e-mailadres volgorde gebruikt begint: $filter = (PartitionKey eq ' email_Sales') en (RowKey ge "a") en (RowKey lt "b")  

Opmerking: de filtersyntaxis gebruikt in de bovenstaande voorbeelden uit de tabelservice REST API, Zie voor meer informatie is [Query entiteiten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* U uw dubbele entiteiten uiteindelijk consistent is met elkaar kunt houden met behulp van de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) onderhouden van de primaire en secundaire index-entiteiten.  
* Table storage is relatief goedkope gebruiken zodat de overhead van de kosten voor het opslaan van dubbele gegevens mag geen groot belang. U moet echter altijd de kosten van het ontwerp op basis van de verwachte opslagvereisten geëvalueerd en alleen toevoegen dubbele entiteiten ter ondersteuning van de query's die de clienttoepassing wordt uitgevoerd.  
* De waarde voor de **RowKey** moet uniek zijn voor elke entiteit. Overweeg het gebruik van de samengestelde sleutel komt.  
* Opvulling numerieke waarden in de **RowKey** (bijvoorbeeld de werknemer-ID 000223), kunt sorteren en filteren op basis van hoofdletters en de ondergrenzen corrigeren.  
* U hoeft hoeft niet te dupliceren van de eigenschappen van de entiteit. Bijvoorbeeld, als de query's die lookup de entiteiten die u het e-adres in de **RowKey** nooit moet de leeftijd van de werknemer, deze entiteiten kunnen de volgende structuur:
  
   ![Werknemer entiteit (secundaire index)](media/storage-table-design-guide/storage-table-design-IMAGE11.png)

* Is het doorgaans beter dubbele gegevens opslaan en ervoor te zorgen dat u alle gegevens die u nodig hebt met één query dan te gebruiken één query vinden van een entiteit met behulp van de secundaire index en een andere lookup de vereiste gegevens in de primaire index kunt ophalen.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer uw clienttoepassing moet met een aantal verschillende sleutels wanneer de client hoeft op te halen van entiteiten in een andere sorteervolgorde entiteiten ophalen en waar u elke entiteit met een aantal unieke waarden kunt identificeren. Dit patroon gebruiken wanneer u voorkomen dat de partitie-limieten voor schaalbaarheid dan wilt wanneer u de entiteit zoekacties met behulp van de verschillende uitvoert **RowKey** waarden.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Intra-partitie secundaire index patroon](#intra-partition-secondary-index-pattern)  
* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

## <a name="eventually-consistent-transactions-pattern"></a>Uiteindelijk consistent transacties patroon
Uiteindelijk consistent gedrag over grenzen van partities of opslag system grenzen inschakelen met behulp van Azure wachtrijen.  

### <a name="context-and-problem"></a>Context en probleem
EGTs inschakelen atomische transacties tussen meerdere entiteiten die dezelfde partitiesleutel delen. Voor betere prestaties en schaalbaarheid u besluiten voor het opslaan van entiteiten met vereisten voor consistentie in afzonderlijke partities of in een afzonderlijke opslagsysteem: in een dergelijk scenario u EGTs niet gebruiken voor het handhaven van de consistentie. Zo mogelijk een vereiste uiteindelijke consistentie tussen onderhouden:  

* Entiteiten in opgeslagen in twee verschillende partities in dezelfde tabel in verschillende tabellen in verschillende opslagaccounts.  
* Een entiteit die is opgeslagen in de tabel-service en een blob die is opgeslagen in de Blob-service.  
* Een entiteit in de tabel-service en een bestand opgeslagen in een bestandssysteem.  
* Een entiteit store in de tabel-service nog geïndexeerd met de Azure Search-service.  

### <a name="solution"></a>Oplossing
U kunt een oplossing die zorgt voor uiteindelijke consistentie tussen twee of meer partities of opslagsystemen implementeren met behulp van Azure wachtrijen.
Ter illustratie van deze benadering wordt ervan uitgegaan dat u hebt een vereiste kunnen archiveren oude werknemer entiteiten. Oude werknemer entiteiten zelden worden opgevraagd en van alle activiteiten die betrekking op de huidige werknemers hebben moeten worden uitgesloten. Voor het implementeren van deze vereiste actieve werknemers op te slaan de **huidige** tabel en oude werknemers in de **archief** tabel. Een werknemer archiveren, moet u verwijderen van de entiteit van de **huidige** tabel en het toevoegen van de entiteit de **archief** tabel, maar u een EGT niet gebruiken voor het uitvoeren van deze twee bewerkingen. Om te voorkomen dat het risico dat een fout een entiteit veroorzaakt moet worden weergegeven in beide of geen van beide tabellen, moet de archiveringsbewerking uiteindelijk consistent is. Het volgende diagram van de takenreeks bevat de stappen in deze bewerking. Meer details wordt uitzondering paden in de volgende tekst geleverd.  

![Azure wachtrijen oplossing](media/storage-table-design-guide/storage-table-design-IMAGE12.png)

Een client initieert de archiveringsbewerking door het plaatsen van een bericht op een Azure-wachtrij in dit voorbeeld voor het archiveren van de werknemer #456. Een werkrol, peilt de wachtrij voor nieuwe berichten; Wanneer er een is gevonden, leest het bericht en wordt een verborgen kopie in de wachtrij. De werkrol een kopie van de entiteit van de volgende haalt de **huidige** tabel, voegt u een kopie in het **archief** tabel en verwijdert u vervolgens de oorspronkelijke uit de **huidige** tabel. Ten slotte, als er geen fouten uit de vorige stap zijn, de werkrol verborgen wordt het bericht verwijderd uit de wachtrij.  

In dit voorbeeld voegt stap 4 van de werknemer in het **archief** tabel. Dit kan de werknemer toevoegen aan een blob in de Blob-service of een bestand in een bestandssysteem.  

### <a name="recovering-from-failures"></a>Herstellen van fouten
Het is belangrijk dat de bewerkingen in stappen **4** en **5** moet *idempotent* voor het geval de werkrol opnieuw opstarten van de archiveringsbewerking. Als u de tabelservice voor stap **4** moet u een bewerking "invoegen of vervangen"; voor de stap **5** moet u een ' verwijderen als bestaat ' bewerking in de clientbibliotheek die u gebruikt. Als u een andere opslagsysteem gebruikt, moet u een geschikte idempotent-bewerking.  

Als de werkrol nooit stap voltooit **6**, en vervolgens verschijnt het bericht opnieuw na een time-out voor de wachtrij gereed is voor de werkrol proberen te deze opnieuw te verwerken. De werkrol kunt controleren hoe vaak een bericht in de wachtrij is gelezen en, indien nodig, markeren is een 'poison'-bericht voor onderzoek door ze naar een afzonderlijke wachtrij. Zie voor meer informatie over het lezen van berichten in wachtrij plaatsen en het controleren van de wachtrij halen telling [berichten ophalen](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Er zijn fouten van de tabel en Queue-services zijn tijdelijke fouten en uw clienttoepassing bevatten geschikte Pogingslogica om deze te verwerken.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Deze oplossing biedt geen voor het isoleren van de transactie. Bijvoorbeeld, een client kan lezen de **huidige** en **archief** tabellen wanneer de werkrol werd tussen stappen **4** en **5**, en een inconsistente weergave van de gegevens ziet. Houd er rekening mee dat de gegevens zal consistent uiteindelijk.  
* U moet ervoor dat de stappen 4 en 5 idempotent zijn om de uiteindelijke consistentie te garanderen.  
* U kunt de oplossing met behulp van meerdere wachtrijen en rolinstanties worker schalen.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon als u wilt garanderen uiteindelijke consistentie tussen entiteiten die aanwezig zijn in verschillende partities of tabellen. U kunt dit patroon om ervoor te zorgen uiteindelijke consistentie voor bewerkingen in de tabel-service en de Blob-service en andere Azure Storage gegevensbronnen zoals database of het bestandssysteem uitbreiden.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)  
* [Samenvoegen of vervangen](#merge-or-replace)  

> [!NOTE]
> Als de transactie-isolatieniveau is belangrijk dat uw oplossing, kunt u overwegen opnieuw ontwerpen van uw tabellen zodat u EGTs gebruiken.  
> 
> 

## <a name="index-entities-pattern"></a>Index entiteiten patroon
Onderhouden index entiteiten zodat efficiënte zoekopdrachten die lijsten van entiteiten retourneren.  

### <a name="context-and-problem"></a>Context en probleem
De tabelservice indexeert automatisch entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Hierdoor wordt een clienttoepassing voor het ophalen van een entiteit efficiënt gebruik maakt van een punt-query. Bijvoorbeeld, met behulp van de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kunt efficiënt een afzonderlijke werknemer entiteit ophalen met behulp van de naam van de afdeling en de werknemer-ID (de **PartitionKey** en **RowKey**).  

![werknemer-entiteit](media/storage-table-design-guide/storage-table-design-IMAGE13.png)

Als u ook kunnen wilt ophalen van een lijst van de werknemer entiteiten op basis van de waarde van een andere niet-unieke eigenschap, zoals hun achternaam, moet u een minder efficiënte partitie scan om te zoeken naar overeenkomsten in plaats van een index met ze rechtstreeks opzoeken. Dit is omdat de tabelservice geen secundaire indexen biedt.  

### <a name="solution"></a>Oplossing
Als u wilt zoeken op naam inschakelen met de structuur van de entiteit hierboven weergegeven, moet u een lijst met werknemer-id's onderhouden. Als u wilt ophalen van de werknemer entiteiten met een bepaalde achternaam, zoals Peeters, moet u eerst de lijst met werknemer-id vinden voor werknemers met Jones als hun achternaam en vervolgens deze werknemer entiteiten worden opgehaald. Er zijn drie manieren voor het opslaan van de lijsten met werknemer-id's:  

* Blob storage gebruiken.  
* Index entiteiten in dezelfde partitie als de werknemer-entiteiten maken.  
* Maak index entiteiten in een afzonderlijke partitie of de tabel.  

<u>Optie #1: Gebruik blob storage</u>  

Voor de eerste optie, u een blob voor elke unieke achternaam en in elke blob-store een lijst maken van de **PartitionKey** (afdeling) en **RowKey** (werknemer-ID) waarden voor werknemers die die achternaam hebben. Bij het toevoegen of verwijderen van een werknemer moet u ervoor zorgen dat de inhoud van de relevante blob uiteindelijk consistent is met de werknemer entiteiten is.  

<u>Optie #2:</u> index entiteiten in dezelfde partitie maken  

Gebruik voor de tweede optie, index-entiteiten die de volgende gegevens worden opgeslagen:  

![Werknemer index entiteit](media/storage-table-design-guide/storage-table-design-IMAGE14.png)

De **EmployeeIDs** eigenschap bevat een lijst van de werknemer-id's voor werknemers met de achternaam opgeslagen in de **RowKey**.  

De volgende stappen wordt beschreven hoe die u volgen moet wanneer u een nieuwe werknemer toevoegen wilt als u de tweede optie. In dit voorbeeld toevoegen we een werknemer met Id 000152 en een achternaam Jones van de verkoopafdeling:  

1. Ophalen van de entiteit index met een **PartitionKey** waarde 'Verkoop' en de **RowKey** waarde "Jones." Sla de ETag van deze entiteit moet worden gebruikt in stap 2.  
2. Maken van een entiteit groep transactie (dat wil zeggen, een batchbewerking) die de nieuwe werknemer entiteit ingevoegd (**PartitionKey** waarde 'Verkoop' en **RowKey** waarde '000152'), en updates van de entiteit index (**PartitionKey** waarde 'Verkoop' en **RowKey** waarde 'Jones') door de nieuwe werknemers-ID toe te voegen aan de lijst in het veld EmployeeIDs. Zie voor meer informatie over entiteit groepstransacties [entiteit groepstransacties](#entity-group-transactions).  
3. Als de entiteit groep transactie is mislukt vanwege een optimistische gelijktijdigheid-fout (iemand anders heeft alleen de entiteit index gewijzigd), moet u opnieuw beginnen bij stap 1.  

U kunt een soortgelijke benadering van een werknemer verwijderen als u de tweede optie gebruiken. Wijzigen van de achternaam van een werknemer is iets ingewikkelder omdat u moet uitvoeren van een entiteit groep transactie die drie entiteiten worden bijgewerkt: de entiteit van de werknemer, de entiteit van de index voor de achternaam van de oude en de entiteit van de index voor de nieuwe achternaam. Voordat u wijzigingen aanbrengt in om op te halen van de ETag-waarden die u vervolgens gebruiken kunt voor het uitvoeren van de updates gebruikt optimistische gelijktijdigheid, moet u elke entiteit ophalen.  

De volgende stappen wordt beschreven hoe die u volgen moet wanneer u alle werknemers met een bepaalde achternaam in een afdeling opzoeken moet, als u de tweede optie. In dit voorbeeld zijn er opzoeken van alle werknemers met een achternaam Jones in de afdeling verkoop:  

1. Ophalen van de entiteit index met een **PartitionKey** waarde 'Verkoop' en de **RowKey** waarde "Jones."  
2. De lijst met werknemer-id's in het veld EmployeeIDs parseren.  
3. Als u meer informatie over elk van deze werknemers (zoals hun e-mailadressen), ophalen van elk van de werknemer entiteiten met **PartitionKey** waarde 'Verkoop' en **RowKey** waarden uit de lijst met werknemers die u hebt verkregen in stap 2.  

<u>Optie #3:</u> index entiteiten in een afzonderlijke partitie of een tabel maken  

Gebruik voor de derde optie, index-entiteiten die opslaan van de volgende gegevens:  

![Werknemer index entiteit in een afzonderlijke partitie](media/storage-table-design-guide/storage-table-design-IMAGE15.png)


De **EmployeeIDs** eigenschap bevat een lijst van de werknemer-id's voor werknemers met de achternaam opgeslagen in de **RowKey**.  

Met de derde optie, kunt u EGTs niet gebruiken om consistentie te behouden omdat de index-entiteiten in een afzonderlijke partitie van de werknemer entiteiten. U moet ervoor zorgen dat de index entiteiten uiteindelijk consistent is met de werknemer entiteiten.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Deze oplossing vereist ten minste twee query's naar het overeenkomende entiteiten ophalen: een query uitvoeren op de index-entiteiten voor de lijst van **RowKey** waarden en query's voor het ophalen van elke entiteit in de lijst.  
* Gezien het feit dat een afzonderlijke entiteit een maximale grootte van 1 MB heeft, optie #2 en de optie #3 in de oplossing wordt ervan uitgegaan dat de lijst met werknemer-id's voor een bepaalde achternaam nooit groter dan 1 MB is. Als de lijst met werknemer-id's kunnen niet groter zijn dan 1 MB groot is, gebruik van optie #1 en opslaan van de indexgegevens in de blob-opslag.  
* Als u de optie #2 gebruikt moet (met EGTs om af te handelen toevoegen en verwijderen van werknemers en het wijzigen van de achternaam van een werknemer) u nagaan of het volume van transacties worden de limieten voor schaalbaarheid in een bepaalde partitie benaderen. Als dit het geval is, moet u rekening houden met een uiteindelijk consistent oplossing (optie 1 # of optie #3) die gebruikmaakt van wachtrijen om de updateaanvragen te verwerken en kunt u uw index entiteiten opslaan op een afzonderlijke partitie van de werknemer entiteiten.  
* Optie #2 in deze oplossing wordt ervan uitgegaan dat u wilt zoeken op achternaam binnen een afdeling: bijvoorbeeld, u wilt ophalen van een lijst met werknemers met een achternaam Jones van de verkoopafdeling. Als u zoeken naar alle werknemers met een achternaam Jones over de hele organisatie wilt, optie #1 of optie #3 gebruiken.
* U kunt een oplossing op basis van wachtrijen die zorgt voor uiteindelijke consistentie implementeren (Zie de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) voor meer informatie).  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon als u wilt voor het opzoeken van een set van entiteiten met algemene waarde van een eigenschap, zoals alle werknemers die over de achternaam Jones dezelfde.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

## <a name="denormalization-pattern"></a>Denormalization patroon
Verwante gegevens samen combineren in één entiteit waarmee u kunt alle gegevens die u nodig hebt met een query één punt ophalen.  

### <a name="context-and-problem"></a>Context en probleem
In een relationele database, moet u doorgaans gegevens als u wilt verwijderen, wat resulteert in query's die gegevens uit meerdere tabellen ophalen worden gedupliceerd normaliseren. Als u uw gegevens in Azure-tabellen normaliseren, moet u meerdere retouren van de client naar de server voor het ophalen van de bijbehorende gegevens. Bijvoorbeeld met de structuur van de tabel hieronder u moet twee retouren naar de details voor een afdeling ophalen: één voor het ophalen van de afdeling entiteit die de manager bevat de ID en vervolgens een andere aanvraag voor het ophalen van de manager details in een entiteit van de werknemer.  

![Afdeling entiteit en werknemer entiteit](media/storage-table-design-guide/storage-table-design-IMAGE16.png)

### <a name="solution"></a>Oplossing
In plaats van de gegevens worden opgeslagen in twee afzonderlijke entiteiten, denormalize van de gegevens en een kopie van de details van de manager in de entiteit afdeling houden. Bijvoorbeeld:  

![Afdeling entiteit](media/storage-table-design-guide/storage-table-design-IMAGE17.png)

Met de afdeling entiteiten met deze eigenschappen worden opgeslagen, kunt u nu alle gegevens die u moet over een afdeling in een query punt ophalen.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Er is enige kosten overhead die is gekoppeld aan twee keer sommige gegevens op te slaan. De prestatievoordelen (die voortvloeien uit minder aanvragen aan de storage-service) doorgaans belangrijker is dan de marginale toename van de kosten voor opslag (en deze kosten is gedeeltelijk gecompenseerd door een vermindering van het aantal transacties die u nodig hebt voor het ophalen van de details van een afdeling).  
* U moet de consistentie van de twee entiteiten die voor het opslaan van informatie over beheerders onderhouden. U kunt het probleem consistentiecontrole verwerkt met behulp van EGTs meerdere entiteiten in één transactie atomic bijwerken: in dit geval wordt de entiteit afdeling en de werknemer entiteit voor de afdelingsmanager worden opgeslagen in dezelfde partitie.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Dit patroon gebruikt als u regelmatig om verwante informatie te zoeken. Dit patroon vermindert het aantal query's die de client aanbrengen moet in de gegevens die vereist worden opgehaald.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)

## <a name="compound-key-pattern"></a>Samengestelde sleutel patroon
Gebruik samengestelde **RowKey** waarden om in te schakelen van een client voor het opzoeken van gerelateerde gegevens met een query één punt.  

### <a name="context-and-problem"></a>Context en probleem
In een relationele database is het erg natuurlijke joins in query's gebruiken om te retourneren van gerelateerde delen van gegevens naar de client in één query op. Bijvoorbeeld, kunt u de werknemer-ID te zoeken om een overzicht van gerelateerde entiteiten die prestaties bevatten en gegevens voor die werknemer bekijken.  

Stel dat u bij het opslaan van entiteiten van de werknemer in de tabel-service met behulp van de volgende structuur:  

![Werknemer entiteit structuur](media/storage-table-design-guide/storage-table-design-IMAGE18.png)

U moet er ook voor het opslaan van historische gegevens met betrekking tot beoordelingen en prestaties voor elk jaar die de werknemer heeft gewerkt voor uw organisatie en moet u toegang tot deze informatie per jaar. Een mogelijkheid is het maken van een andere tabel die wordt opgeslagen entiteiten met de volgende structuur:  

![Alternatieve werknemer entiteit structuur](media/storage-table-design-guide/storage-table-design-IMAGE19.png)

U ziet dat met deze methode wilt u misschien dubbele bepaalde gegevens (zoals de voornaam en achternaam) in de nieuwe entiteit waarmee u uw gegevens met één aanvraag ophalen. U moet echter sterke consistentie kan niet onderhouden omdat u niet een EGT gebruiken voor het bijwerken van de twee entiteiten moment.  

### <a name="solution"></a>Oplossing
Een nieuwe entiteitstype opslaan in de oorspronkelijke tabel met behulp van de entiteiten met de volgende structuur:  

![Oplossing voor werknemer entiteit structuur](media/storage-table-design-guide/storage-table-design-IMAGE20.png)

U ziet hoe de **RowKey** is nu een samengestelde sleutel bestaat uit de werknemer-ID en het jaar van de evaluatie-gegevens waarmee u de prestaties van de werknemer ophalen en gegevens met één aanvraag voor een enkele entiteit te controleren.  

Het volgende voorbeeld ziet hoe u alle controle-gegevens kunt ophalen voor een bepaalde werknemer (zoals werknemer 000123 van de verkoopafdeling):  

$filter = (PartitionKey eq 'Sales') en (RowKey ge 'empid_000123') en (RowKey lt 'empid_000124') & $select = RowKey, beoordeling door Manager, beoordeling van de Peer, opmerkingen  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* U moet een geschikte scheidingsteken waarmee u gemakkelijk parseren gebruiken de **RowKey** waarde: bijvoorbeeld **000123_2012**.  
* U ook opslaat deze entiteit in dezelfde partitie als andere entiteiten die gerelateerde gegevens bevatten voor dezelfde werknemer, wat betekent dat u kunt EGTs sterke consistentie.
* U moet overwegen hoe vaak u een query uit op de gegevens om te bepalen of dit patroon geschikt is.  Bijvoorbeeld, als u toegang hebben tot de gegevens van revisie zelden en vaak de belangrijkste werknemersgegevens bewaar ze als afzonderlijke entiteiten.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer u wilt opslaan op een of meer entiteiten die query u vaak gerelateerde.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  

## <a name="log-tail-pattern"></a>Patroon voor logboekbestand staart
Ophalen van de *n* entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** waarde die in omgekeerde datum en tijd volgorde worden gesorteerd.  

### <a name="context-and-problem"></a>Context en probleem
Een algemene vereiste is mogelijk het meest recent gemaakte entiteiten ophalen, bijvoorbeeld de meest recente tien claims die zijn ingediend door een werknemer onkosten. Tabel ondersteuning vraagt een **$top** querybewerking te retourneren van de eerste *n* entiteiten uit een set: Er is geen equivalent querybewerking te retourneren van de laatste n entiteiten in een set.  

### <a name="solution"></a>Oplossing
Opslaan van de entiteiten met een **RowKey** dat natuurlijk sorteren in volgorde van de omgekeerde datum/tijd met behulp van zodat de meest recente vermelding is altijd de eerste rij in de tabel.  

Bijvoorbeeld, om te kunnen ophalen van de tien meest recente onkosten claims ingediend door een werknemer, kunt u een omgekeerde maatstreepjes-waarde die is afgeleid van de huidige datum en tijd. De volgende C#-codevoorbeeld ziet u een manier om u te maken van een geschikte waarde 'omgekeerde ticks' voor een **RowKey** die sorteren van de meest recente naar oud:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

U kunt teruggaan naar de datum-tijdwaarde met de volgende code:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

De tabelquery ziet er als volgt:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* U moet de waarde van de omgekeerde maatstreepjes met nullen om te controleren of dat de tekenreekswaarde sorteert zoals verwacht toonaangevende opgevuld.  
* U moet rekening houden met de schaalbaarheidsdoelen op het niveau van een partitie. Wees voorzichtig hotspot partities niet maken.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Dit patroon gebruikt als u toegang tot entiteiten in volgorde van de omgekeerde datum/tijd- of wanneer u toegang wilt tot de meest recent toegevoegde entiteiten.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Toevoegen / antivirusprogramma patroon toevoegen](#prepend-append-anti-pattern)  
* [Entiteiten ophalen](#retrieving-entities)  

## <a name="high-volume-delete-pattern"></a>Patroon voor grote volumes verwijderen
Het verwijderen van een groot aantal entiteiten inschakelen door het opslaan van alle entiteiten voor gelijktijdige verwijdering in hun eigen afzonderlijke tabel. u kunt de entiteiten verwijderen door de tabel verwijderen.  

### <a name="context-and-problem"></a>Context en probleem
Veel toepassingen verwijderen oude gegevens die niet langer beschikbaar zijn voor een clienttoepassing of de toepassing naar een ander opslagmedium is gearchiveerd. U doorgaans dergelijke gegevens identificeren met een datum: bijvoorbeeld: u hebt een vereiste om records van alle aanmeldingsaanvragen voor die meer dan 60 dagen te verwijderen.  

Een mogelijke ontwerp is het gebruik van de datum en tijd van de aanmeldingsaanvraag in de **RowKey**:  

![Datum en tijd van aanmeldingspoging](media/storage-table-design-guide/storage-table-design-IMAGE21.png)

Deze aanpak voorkomt partitie hotspots omdat de toepassing kunt invoegen en verwijderen van aanmelding entiteiten voor elke gebruiker in een afzonderlijke partitie. Deze aanpak kan echter kostbaar en tijd in beslag nemen als u een groot aantal entiteiten hebt omdat u eerst een tabelscan uitvoeren om te kunnen identificeren van de entiteiten moet te verwijderen en vervolgens moet u elke oude entiteit verwijderen zijn. Houd er rekening mee dat u het aantal retouren naar de server vereist voor het verwijderen van de oude entiteiten met meerdere delete-aanvragen in EGTs batchverwerking kunt verkleinen.  

### <a name="solution"></a>Oplossing
Een afzonderlijke tabel gebruiken voor elke dag van aanmeldingspogingen. U kunt het ontwerp van de entiteit bovenstaande hotspots voorkomen wanneer entiteiten invoegen en verwijderen van oude entiteiten nu gewoon een vraag is van het verwijderen van één tabel elke dag (een enkele opslagbewerking) in plaats van zoeken en verwijderen van honderden en duizenden afzonderlijke aanmelding entiteiten elke dag.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Uw ontwerp biedt ondersteuning voor andere manieren de gegevens zoals het opzoeken van specifieke entiteiten, koppelen aan andere gegevens of genereren verzamelde gegevens zullen worden gebruikt door uw toepassing?  
* Uw ontwerp hotspots voorkomen tijdens het invoegen van nieuwe entiteiten  
* Een vertraging verwachten als u gebruiken als de naam van de dezelfde tabel wilt na het verwijderen. Het is beter gebruik altijd uniek tabelnamen.  
* Verwachten dat sommige beperking wanneer u eerst een nieuwe tabel terwijl de tabelservice de toegangspatronen leert en distributie van de partities over knooppunten. U moet rekening houden hoe vaak moet u nieuwe tabellen maken.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Dit patroon gebruiken wanneer u een groot aantal entiteiten die op hetzelfde moment moet u verwijderen.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)
* [Entiteiten wijzigen](#modifying-entities)  

## <a name="data-series-pattern"></a>Patroon van de reeks gegevens
Store voltooid gegevensreeksen in één entiteit om te beperken het aantal aanvragen die u aanbrengt.  

### <a name="context-and-problem"></a>Context en probleem
Er is een veelvoorkomend scenario voor een toepassing voor het opslaan van een reeks met gegevens die normaal gesproken nodig is om op te halen in één keer. Uw toepassing kan bijvoorbeeld opname hoeveel IM-berichten elke werknemer elk uur en vervolgens deze informatie gebruiken om te tekenen hoeveel berichten elke gebruiker die is verzonden via de voorgaande 24 uur. Een ontwerp kan voor het opslaan van 24 entiteiten voor elke werknemer zijn:  

![24 entiteiten voor elke werknemer opslaan](media/storage-table-design-guide/storage-table-design-IMAGE22.png)

Bij dit ontwerp kunt u eenvoudig zoeken en bijwerken van de entiteit voor elke werknemer bijwerken wanneer de toepassing moet de waarde voor aantal bijwerken. Voor het ophalen van de informatie voor het tekenen van een grafiek van de activiteit voor de voorgaande 24 uur, moet u echter 24 entiteiten ophalen.  

### <a name="solution"></a>Oplossing
Het ontwerp van de volgende voor het opslaan van het aantal berichten voor elk uur een afzonderlijke eigenschap gebruiken:  

![Bericht statistieken entiteit](media/storage-table-design-guide/storage-table-design-IMAGE23.png)

Bij dit ontwerp kunt u een samenvoegbewerking voor het aantal berichten voor een werknemer bijwerken voor een specifiek uur. U kunt nu alle informatie die u nodig hebt om het gebruik van een aanvraag voor een enkele entiteit diagram te tekenen ophalen.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Als de volledige reeks past niet in één entiteit (een entiteit kan maximaal 252 eigenschappen hebben), gebruikt u een alternatieve gegevensarchief, zoals een blob.  
* Als u meerdere clients tegelijkertijd bijwerken van een entiteit hebt, moet u gebruik van de **ETag** optimistische gelijktijdigheid implementeren. Als u veel clients hebt, kunt u hoge conflicten ondervinden.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Dit patroon gebruiken als u wilt bijwerken en ophalen van een reeks die is gekoppeld aan een afzonderlijke entiteit.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Grote entiteiten patroon](#large-entities-pattern)  
* [Samenvoegen of vervangen](#merge-or-replace)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) (als u de gegevensreeks in een blob opslaat)  

## <a name="wide-entities-pattern"></a>Wide entiteiten patroon
Gebruik van meerdere fysieke entiteiten voor het opslaan van logische entiteiten met meer dan 252 eigenschappen.  

### <a name="context-and-problem"></a>Context en probleem
Een afzonderlijke entiteit kan maximaal 252 eigenschappen (met uitzondering van de verplichte eigenschappen) en kan niet meer dan 1 MB aan gegevens opslaan in totaal. In een relationele database, doorgaans krijgt u ronde beperkingen met betrekking tot de grootte van een rij bij het toevoegen van een nieuwe tabel en een 1-op-1-relatie tussen deze twee afdwingen.  

### <a name="solution"></a>Oplossing
De tabel-service gebruikt, kunt u meerdere entiteiten ter vertegenwoordiging van een object één grote bedrijven met meer dan 252 eigenschappen opslaan. Bijvoorbeeld, als u een telling van het aantal IM-berichten is verzonden door elke werknemer voor de afgelopen 365 dagen opslaan wilt, kunt u het ontwerp van de volgende die gebruikmaakt van twee entiteiten met verschillende schema's:  

![Meerdere entiteiten](media/storage-table-design-guide/storage-table-design-IMAGE24.png)

U kunt een EGT gebruiken als u een wijziging aanbrengt die is vereist voor het bijwerken van beide entiteiten wilt zodat ze zijn gesynchroniseerd met elkaar. Anders kunt u één samenvoegbewerking bijwerken van het aantal berichten voor een specifieke dag. Alle gegevens ophalen voor een afzonderlijke werknemer moet u beide entiteiten, kunt u doen met twee efficiënte aanvragen die gebruikmaken van beide ophalen een **PartitionKey** en een **RowKey** waarde.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Bij het ophalen van een volledige logische entiteit ten minste twee opslagtransacties omvat: een voor elke fysieke entiteit ophalen.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Gebruik dit patroon wanneer nodig voor het opslaan van entiteiten waarvan de grootte van of het aantal eigenschappen de grenzen voor een afzonderlijke entiteit in de tabel-service overschrijdt.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)
* [Samenvoegen of vervangen](#merge-or-replace)

## <a name="large-entities-pattern"></a>Grote entiteiten patroon
Blob storage gebruiken voor het opslaan van grote eigenschapswaarden.  

### <a name="context-and-problem"></a>Context en probleem
Een afzonderlijke entiteit kan niet meer dan 1 MB aan gegevens opslaan in totaal. Als een of meer van de eigenschappen van uw waarden die ertoe leiden dat de totale grootte van uw entiteit zijn dan deze waarde kunt opslaan, kunt u het geheel niet opslaan in de tabel-service.  

### <a name="solution"></a>Oplossing
Als uw entiteit 1 MB groot overschrijdt omdat een of meer eigenschappen een grote hoeveelheid gegevens bevatten, kunt u gegevens opslaan in de Blob-service en het adres van de blob op te slaan in een eigenschap in de entiteit. Bijvoorbeeld, u kunt de foto van een werknemer opslaan in blob-opslag en opslaan van een koppeling naar de foto in de **Photo** eigenschap van de werknemer entiteit:  

![Foto-eigenschap](media/storage-table-design-guide/storage-table-design-IMAGE25.png)

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Gebruiken om te blijven uiteindelijke consistentie tussen de entiteit in de tabel-service en de gegevens in de Blob-service, de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) uw entiteiten onderhouden.
* Bij het ophalen van een volledige entiteit ten minste twee opslagtransacties omvat: één voor het ophalen van de entiteit en één voor het ophalen van de blob-gegevens.  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Dit patroon gebruikt als u voor het opslaan van entiteiten waarvan de grootte de grenzen voor een afzonderlijke entiteit in de tabel-service overschrijdt.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Wide entiteiten patroon](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

## <a name="prependappend-anti-pattern"></a>Antivirusprogramma patroon toevoegen/toevoegen
Schaalbaarheid vergroten wanneer u een groot aantal invoegingen hebt met de invoegt verspreid over meerdere partities.  

### <a name="context-and-problem"></a>Context en probleem
Voorafgaand of entiteiten doorgaans in te voegen aan uw opgeslagen entiteiten resulteert in de toepassing nieuwe entiteiten toe te voegen aan de eerste of laatste partitie van een reeks van partities. In dit geval plaatsvinden alle van de invoegt op elk moment in dezelfde partitie maken van een hotspot waarmee wordt voorkomen de tabelservice van load voegt te verdelen over meerdere knooppunten en mogelijk veroorzaakt door uw toepassing dat om de schaalbaarheidsdoelen voor partitie. Bijvoorbeeld, als u een toepassing hebt die netwerk logboeken en toegang tot bedrijfsbronnen door werknemers, klikt u vervolgens een entiteit structuur zoals hieronder wordt weergegeven in het huidige uur partitie een hotspot is als het volume van transacties het doel van de schaalbaarheid van een afzonderlijke partitie bereikt kan leiden:  

![Structuur van de entiteit](media/storage-table-design-guide/storage-table-design-IMAGE26.png)

### <a name="solution"></a>Oplossing
De volgende alternatieve entiteit structuur voorkomt een hotspot op een bepaalde partitie als de toepassing Logboeken gebeurtenissen:  

![Alternatieve entiteit structuur](media/storage-table-design-guide/storage-table-design-IMAGE27.png)

Kennisgeving met dit voorbeeld van hoe beide de **PartitionKey** en **RowKey** samengestelde sleutels. De **PartitionKey** gebruikt de afdeling en de werknemer-ID om de logboekregistratie verdelen over meerdere partities.  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Beschouw de volgende punten als u besluit hoe u dit patroon wilt implementeren:  

* Ondersteunt de alternatieve sleutelstructuur die voorkomt efficiënt hot partities maken op voegt de query's kunt u de clienttoepassing?  
* Het verwachte volume van transacties betekent dat u waarschijnlijk de schaalbaarheidsdoelen voor een afzonderlijke partitie bereiken en worden beperkt door de storage-service?  

### <a name="when-to-use-this-pattern"></a>Wanneer dit patroon gebruiken
Vermijd het prepend/append anti-patroon wanneer het volume van transacties kunnen ertoe leiden dat door de storage-service beperken wanneer u een hot partitie is.  

### <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen
De volgende patronen en richtlijnen zijn mogelijk ook relevant bij de implementatie van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Patroon voor logboekbestand staart](#log-tail-pattern)  
* [Entiteiten wijzigen](#modifying-entities)  

## <a name="log-data-anti-pattern"></a>Antivirusprogramma patroon voor logboekbestand gegevens
Normaal gesproken moet u de Blob-service in plaats van de tabel-service voor het opslaan van gegevens aan het logboek.  

### <a name="context-and-problem"></a>Context en probleem
Als u een algemeen gebruiksvoorbeeld voor logboekgegevens is voor het ophalen van een selectie van vermeldingen in het logboek voor een specifieke datum/tijd-bereik: bijvoorbeeld wordt gezocht naar alle fout en kritieke berichten die uw toepassing vastgelegd tussen 15:04 en 15:06 op een bepaalde datum. U niet wilt dat de datum en tijd van het logboekbericht gebruiken om te bepalen van de partitie opslaan van entiteiten logboek: die resulteert in een hot partitie omdat er op elk gewenst alle entiteiten in het logboek wordt delen dezelfde **PartitionKey** waarde (Zie de sectie [antivirusprogramma patroon Prepend/append](#prepend-append-anti-pattern)). Het volgende schema van de entiteit voor een logboekbericht resulteert bijvoorbeeld in een hot partitie omdat de toepassing alle berichten in het logboek voor de partitie voor de huidige datum en het uur schrijft:  

![Logboek bericht entiteit](media/storage-table-design-guide/storage-table-design-IMAGE28.png)

In dit voorbeeld wordt de **RowKey** bevat de datum en tijd van het logboekbericht om ervoor te zorgen dat logboekberichten worden opgeslagen in datum/tijd-volgorde gesorteerd en bevat een bericht-ID als meerdere logboekberichten de dezelfde datum en tijd delen.  

Een andere manier is het gebruik van een **PartitionKey** die ervoor zorgt dat de toepassing berichten over een reeks partities schrijft. Als de bron van het logboekbericht een manier biedt voor de distributie van berichten over veel partities, kan u bijvoorbeeld het volgende schema voor de entiteit gebruiken:  

![Alternatieve logboek bericht entiteit](media/storage-table-design-guide/storage-table-design-IMAGE29.png)

Het probleem met dit schema is echter voor het ophalen van de logboekberichten voor een bepaalde periode moet u elke partitie zoeken in de tabel.

### <a name="solution"></a>Oplossing
De vorige sectie het probleem van de tabel-service gebruiken voor het opslaan van logboekvermeldingen en voorgestelde twee onvoldoende, ontwerpen probeert gemarkeerd. Een oplossing heeft geleid tot een hot partitie met het risico van slechte prestaties schrijven logboekberichten; de andere oplossing heeft geresulteerd in slechte queryprestaties vanwege de vereisten voor het scannen van elke partitie in de tabel voor het ophalen van berichten in het logboek voor een bepaalde periode. BLOB storage biedt een betere oplossing voor dit soort scenario en dit is hoe Azure Storage Analytics slaat de logboekgegevens worden verzameld.  

Deze sectie geeft een overzicht van hoe opslag Analytics logboekgegevens opslaat in de blob-opslag ter illustratie van deze benadering voor het opslaan van gegevens die u doorgaans een query voor het bereik.  

Storage Analytics slaat logboekberichten in een indeling met scheidingstekens in meerdere blobs. De indeling gescheiden eenvoudig een clienttoepassing parseren van de gegevens in het logboekbericht.  

Storage Analytics maakt gebruik van een naamgevingsconventie voor blobs die kunt u de blob vinden (of BLOB's) die de berichten in het logboek waarnaar u zoekt bevatten. Een blob met de naam 'queue/2014/07/31/1800/000001.log' bevat bijvoorbeeld logboekberichten die betrekking hebben op de queue-service voor het uur om 18:00 uur op 31 juli 2014 wordt gestart. De '000001' geeft aan dat dit het eerste logboekbestand voor deze periode. Storage Analytics registreert ook de tijdstempel van de eerste en laatste logboekberichten opgeslagen in het bestand als onderdeel van de blob-metagegevens. De API voor blob-opslag kunt u blobs niet in een container op basis van een voorvoegsel vinden: om te zoeken in alle blobs die wachtrij logboekgegevens bevatten voor het beginnen bij 18:00 uur, kunt u het voorvoegsel "wachtrij/2014/07/31/1800."  

Storage Analytics buffers Meld berichten intern en vervolgens regelmatig updates van de juiste blob of maakt u een nieuw bestand met de meest recente batch van logboekvermeldingen. Dit vermindert het aantal schrijft die deze naar de blob-service moet uitvoeren.  

Als u een vergelijkbare oplossing in uw eigen toepassing implementeert, moet u rekening houden met het beheren van de verhouding tussen betrouwbaarheid (elke logboekvermelding schrijven naar de blob-opslag als dit gebeurt) en de kosten en schaalbaarheid (buffer updates in uw toepassing en worden geschreven naar blob storage in batches).  

### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten wanneer u beslist het opslaan van gegevens aan het logboek:  

* Als u een tabelontwerp dat potentiële hot partities voorkomt maakt, merkt u dat u geen toegang uw logboekgegevens efficiënt tot.  
* Voor het verwerken van gegevens aan het logboek moet een client vaak veel records laden.  
* Hoewel logboekgegevens is vaak opgebouwd, kan de blob-opslag een betere oplossing zijn.  

## <a name="implementation-considerations"></a>Afwegingen bij de implementatie
Deze sectie worden enkele van de overwegingen op moet letten wanneer u de patronen die zijn beschreven in de vorige secties implementeert beschreven. De meeste van deze sectie bevat voorbeelden geschreven in C# en die gebruikmaken van de Storage-clientbibliotheek (versie 4.3.0 op het moment van schrijven).  

## <a name="retrieving-entities"></a>Entiteiten ophalen
Zoals beschreven in de sectie [ontwerp voor het uitvoeren van query's](#design-for-querying), de meest efficiënte query is een punt-query. In sommige scenario's moet u mogelijk echter meerdere entiteiten ophalen. Deze sectie beschrijft een aantal algemene benaderingen bij het ophalen van entiteiten met behulp van de Storage-clientbibliotheek.  

### <a name="executing-a-point-query-using-the-storage-client-library"></a>Uitvoeren van een punt-query met behulp van de Storage-clientbibliotheek
De eenvoudigste manier om het uitvoeren van een punt-query is met de **ophalen** bewerking tabel, zoals wordt weergegeven in het volgende C# codefragment die ophaalt van een entity met een **PartitionKey** van de waarde 'Verkoop' en een **RowKey** van de waarde '212':  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

U ziet hoe de entiteit in dit voorbeeld worden verwacht opgehaald om te worden van het type **EmployeeEntity**.  

### <a name="retrieving-multiple-entities-using-linq"></a>Bij het ophalen van meerdere entiteiten met behulp van LINQ
U kunt meerdere entiteiten ophalen met behulp van LINQ met Storage-clientbibliotheek en het opgeven van een query met een **waar** component. Om te voorkomen dat een tabelscan, moet u altijd opnemen de **PartitionKey** waarde in de where-component, en indien mogelijk de **RowKey** waarde om te voorkomen dat de tabel en partitie scans. De tabelservice ondersteunt een beperkte set vergelijkingsoperators (groter dan groter dan of gelijk is, minder dan, kleiner dan of gelijk zijn, gelijk en niet gelijk) te gebruiken in de where component. De volgende C#-codefragment vindt alle werknemers waarvan de laatste naam begint met "B" (ervan uitgaande dat de **RowKey** slaat de achternaam) in de afdeling verkoop (ervan uitgaande dat de **PartitionKey** slaat de afdelingsnaam):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

U ziet hoe de query is zowel een **RowKey** en een **PartitionKey** betere prestaties te garanderen.  

Het volgende codevoorbeeld toont dezelfde functionaliteit beheersen API gebruiken (Zie voor meer informatie over beheersen API's in het algemeen [aanbevolen procedures voor het ontwerpen van een beheersen API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

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
> Het voorbeeld worden genest meerdere **CombineFilters** methoden voor het opnemen van de drie filtervoorwaarden.  
> 
> 

### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Groot aantal entiteiten ophalen uit een query
Een optimale query retourneert een afzonderlijke entiteit op basis van een **PartitionKey** waarde en een **RowKey** waarde. In sommige scenario's hebben u echter een vereiste is te groot aantal entiteiten retourneren vanuit dezelfde partitie of zelfs via veel partities.  

In dergelijke gevallen moet u altijd volledig de prestaties van uw toepassing testen.  

Een query op de tabelservice kan maximaal 1000 entiteiten in één keer worden geretourneerd en wordt uitgevoerd voor een maximum van vijf seconden. Als de resultatenset bevat meer dan 1000 entiteiten, als de query is niet voltooid binnen vijf seconden, of als de query overschrijdt de grens van de partitie is, retourneert de tabel-service een vervolgtoken zodat de clienttoepassing om aan te vragen van de volgende set van entiteiten. Zie voor meer informatie over hoe voortzetting werk tokens [querytime-out en paginering](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Als u de Storage-clientbibliotheek gebruikt, kan deze automatisch voortzetting tokens voor u verwerken als deze entiteiten uit de tabelservice retourneert. De volgende C# voorbeeldcode met behulp van de Storage-clientbibliotheek automatisch verwerkt voortzetting tokens als de tabelservice die in een antwoord retourneert:  

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

De volgende C#-code voortzetting tokens expliciet worden verwerkt:  

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

Voortzetting tokens expliciet gebruikt, kunt u bepalen wanneer de toepassing wordt het volgende segment van gegevens wordt opgehaald. Bijvoorbeeld, als uw clienttoepassing kan gebruikers op de pagina via de entiteiten die zijn opgeslagen in een tabel, een gebruiker kan niet meer wilt pagina via de entiteiten die zijn opgehaald door de query zodat uw toepassing een vervolgtoken alleen gebruiken zou voor het ophalen van het volgende segment wanneer de gebruiker had voltooid paging via alle entiteiten in het huidige segment. Deze methode biedt verschillende voordelen:  

* Hiermee kunt u de hoeveelheid gegevens op te halen uit de tabel-service beperken en die u via het netwerk verplaatst.  
* Hiermee kunt u het uitvoeren van asynchrone IO in .NET.  
* Hiermee kunt u voor het serialiseren van het vervolgtoken naar de permanente opslag zodat u kunt doorgaan in het geval van een toepassing is vastgelopen.  

> [!NOTE]
> Een vervolgtoken retourneert doorgaans een segment met 1000 entiteiten, hoewel deze mogelijk minder. Dit is ook het geval als u het aantal vermeldingen met behulp van een query retourneert beperken **nemen** te retourneren van de eerste n entiteiten die voldoen aan uw criteria lookup: de tabelservice kan een segment met minder dan n entiteiten samen met een vervolgtoken waarmee u kunt de resterende entiteiten ophalen geretourneerd.  
> 
> 

De volgende C#-code wordt getoond hoe u het aantal entiteiten die zijn geretourneerd binnen een segment wijzigen:  

```csharp
employeeQuery.TakeCount = 50;  
```

### <a name="server-side-projection"></a>Projectie-serverzijde
Één entiteit kan maximaal 255 eigenschappen hebben en maximaal 1 MB groot zijn. Wanneer u een query uitvoeren op de tabel en entiteiten ophalen, moet u wellicht niet alle eigenschappen en ervaren onnodig (als u wilt verminderen en de kosten van latentie) kunt voorkomen. U kunt serverzijde projectie gebruiken om over te dragen alleen de eigenschappen die u nodig hebt. Het volgende voorbeeld is haalt alleen de **e** eigenschap (samen met **PartitionKey**, **RowKey**, **tijdstempel**, en **ETag**) van de entiteiten die zijn geselecteerd door de query.  

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
De Storage-clientbibliotheek kunt u de entiteiten die zijn opgeslagen in de tabelservice door invoegen, verwijderen en bijwerken van entiteiten wijzigen. U kunt EGTs batch meerdere insert, update en delete-bewerkingen samen in Verminder het aantal retouren vereist en de prestaties van uw oplossing verbeteren.  

Rekening mee dat uitzonderingen die worden gegenereerd wanneer de Opslagclientbibliotheek wordt uitgevoerd een EGT doorgaans de index van de entiteit die de batch mislukken veroorzaakt. Dit is handig wanneer u code die gebruikmaakt van EGTs foutopsporing.  

U moet ook overwegen hoe uw ontwerp is van invloed op hoe de clienttoepassing gelijktijdigheid van taken en updatebewerkingen verwerkt.  

### <a name="managing-concurrency"></a>Gelijktijdigheid beheren
Standaard implementeert de tabelservice optimistische gelijktijdigheid controleert op het niveau van afzonderlijke entiteiten voor **invoegen**, **samenvoegen**, en **verwijderen** bewerkingen, hoewel het mogelijk voor een client om af te dwingen van de tabelservice voor het overslaan van deze controles. Zie voor meer informatie over hoe de tabelservice gelijktijdigheid beheert [gelijktijdigheid beheren in Microsoft Azure Storage](../../storage/common/storage-concurrency.md).  

### <a name="merge-or-replace"></a>Samenvoegen of vervangen
De **vervangen** methode van de **TableOperation** klasse altijd vervangen door de volledige entiteit in de tabel-service. Als u geen een eigenschap in de aanvraag wanneer deze eigenschap in de opgeslagen entiteit bestaat, wordt in de aanvraag die eigenschap verwijdert uit de opgeslagen entiteit. Tenzij u een eigenschap expliciet verwijderen uit een opgeslagen entiteit wilt, moet u elke eigenschap opnemen in de aanvraag.  

U kunt de **samenvoegen** methode van de **TableOperation** klasse om te verminderen de hoeveelheid gegevens die u naar de tabel-service verzendt wanneer u wilt bijwerken van een entiteit. De **samenvoegen** methode alle eigenschappen in de opgeslagen entiteit vervangen door waarden van eigenschappen van de entiteit die is opgenomen in de aanvraag, maar blijft behouden alle eigenschappen in de opgeslagen entiteit die niet zijn opgenomen in de aanvraag. Dit is handig als u grote entiteiten en hoeft slechts een klein aantal eigenschappen in een aanvraag bijwerken.  

> [!NOTE]
> De **vervangen** en **samenvoegen** twee methoden mislukken als de entiteit niet bestaat. Als alternatief kunt u de **InsertOrReplace** en **InsertOrMerge** methoden die nieuwe entiteit maken als deze niet bestaat.  
> 
> 

## <a name="working-with-heterogeneous-entity-types"></a>Werken met heterogene Entiteitstypen
De tabel-service is een *schema minder* tabel archief dat betekent dat één tabel entiteiten van meerdere typen bieden geweldige flexibiliteit in uw ontwerp kunt opslaan. Het volgende voorbeeld wordt een tabel die het opslaan van zowel werknemer en entiteiten van de afdeling:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Voornaam</th>
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
<th>Voornaam</th>
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
<th>Voornaam</th>
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

Denk eraan dat elke entiteit moet nog steeds **PartitionKey**, **RowKey**, en **tijdstempel** waarden, maar mogelijk elke gewenste set eigenschappen. Bovendien, er is niets om aan te geven van het type van een entiteit, tenzij u ervoor kiest die gegevens ergens op te slaan. Er zijn twee opties voor het identificeren van het entiteitstype:  

* Toevoegen van het entiteitstype voor de **RowKey** (of mogelijk de **PartitionKey**). Bijvoorbeeld: **EMPLOYEE_000123** of **DEPARTMENT_SALES** als **RowKey** waarden.  
* Gebruik een afzonderlijke eigenschap voor het vastleggen van het entiteitstype, zoals wordt weergegeven in de onderstaande tabel.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
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
<th>Voornaam</th>
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
<th>Voornaam</th>
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
<th>Voornaam</th>
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

De eerste optie, de entiteit prepending type de **RowKey**, is handig als er een kans bestaat dat twee entiteiten met verschillende typen wellicht dezelfde sleutelwaarde. Deze groepen ook entiteiten van hetzelfde type samen in de partitie.  

De technieken beschreven in deze sectie zijn vooral relevant zijn voor de bespreking van de [relaties voor overname](table-storage-design-modeling.md#inheritance-relationships) eerder in deze handleiding in het artikel [relaties te modelleren](table-storage-design-modeling.md).  

> [!NOTE]
> U moet rekening houden met inbegrip van een uniek versienummer op in de waarde van het type entiteit zodat clienttoepassingen ontwikkelen POCO-objecten en werken met verschillende versies.  
> 
> 

De rest van deze sectie beschrijft een aantal van de functies in de Storage-clientbibliotheek die werken met meerdere Entiteitstypen in dezelfde tabel vergemakkelijken.  

### <a name="retrieving-heterogeneous-entity-types"></a>Heterogene Entiteitstypen ophalen
Als u van de Storage-clientbibliotheek gebruikmaakt, hebt u drie opties voor het werken met meerdere Entiteitstypen.  

Als u welk type van de entiteit die is opgeslagen met een specifieke weet **RowKey** en **PartitionKey** waarden, daarna u het entiteitstype opgeven kunt wanneer u de entiteit ophalen, zoals wordt weergegeven in de vorige twee voorbeelden die entiteiten van het type ophalen **EmployeeEntity**: [uitvoeren van een punt-query met behulp van de Storage-clientbibliotheek](#executing-a-point-query-using-the-storage-client-library) en [bij het ophalen van meerdere entiteiten met behulp van LINQ](#retrieving-multiple-entities-using-linq).  

De tweede optie is met de **DynamicTableEntity** type (een eigenschappenverzameling) in plaats van een concreet POCO entiteitstype (deze optie ook de prestaties mogelijk verbeterd omdat het is niet nodig voor het serialiseren en deserialiseren van de entiteit .NET-typen). De volgende C#-code mogelijk meerdere entiteiten met verschillende typen opgehaald uit de tabel, maar retourneert alle entiteiten als **DynamicTableEntity** exemplaren. Vervolgens wordt de **EntityType** eigenschap om te bepalen van het type van elke entiteit:  

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

Houd er rekening mee dat voor het ophalen van andere eigenschappen moet u de **TryGetValue** methode op de **eigenschappen** eigenschap van de **DynamicTableEntity** klasse.  

Een derde optie is om te combineren met behulp van de **DynamicTableEntity** type en een **EntityResolver** exemplaar. Hiermee kunt u omzetten in meerdere POCO-typen in dezelfde query. In dit voorbeeld wordt de **EntityResolver** gemachtigde maakt gebruik van de **EntityType** eigenschap onderscheid maken tussen de twee typen entiteit die de query retourneert. De **los** methode gebruikt de **resolver** gemachtigde om op te lossen **DynamicTableEntity** exemplaren te **TableEntity** exemplaren.  

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
U hoeft niet te weten van het type van een entiteit wilt verwijderen en u het type van een entiteit altijd weet wanneer u het invoegen. U kunt echter **DynamicTableEntity** type zonder te weten van het type en zonder gebruik van een POCO-entiteitsklasse bijwerken van een entiteit. Het volgende codevoorbeeld één entiteit worden opgehaald en controleert de **EmployeeCount** eigenschap bestaat voordat u het bijwerkt.  

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
Shared Access Signature (SAS)-tokens kunt u toepassingen die client te wijzigen (en een query) tabelentiteiten rechtstreeks zonder de noodzaak om te verifiëren rechtstreeks met de tabelservice. Er zijn in principe drie belangrijke voordelen bij SAS gebruiken in uw toepassing:  

* U hoeft niet te distribueren naar een onbeveiligde platform (zoals een mobiel apparaat) sleutel van uw opslagaccount als u wilt toestaan dat het apparaat te openen en te wijzigen van de entiteiten in de tabel-service.  
* U kunt offload deel van het werk die web-en werkrollen uitvoeren bij het beheren van uw entiteiten op clientapparaten zoals eindgebruikers, computers en mobiele apparaten.  
* U kunt een beperkte toewijzen en tijd beperkt set machtigingen voor een client (zoals alleen-lezen toegang tot specifieke bronnen toe te staan).  

Zie voor meer informatie over het gebruik van SAS-tokens met de tabelservice [met behulp van Shared Access Signatures (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Echter, moet u nog steeds de SAS-tokens die de entiteiten in de tabelservice een clienttoepassing verlenen genereren: u moet dit doen in een omgeving met veilige toegang tot uw toegangscodes voor opslag. Normaal, gebruikt u een web- of worker-rol voor het genereren van de SAS-tokens en ervoor zorgen dat ze de clienttoepassingen die toegang nodig tot de entiteiten. Omdat er nog steeds een overhead voor het genereren en leveren van SAS-tokens op clients, moet u het beste aan deze overhead, met name in grootschalige scenario's verminderen.  

Het is mogelijk om een SAS-token die toegang tot een subset van de entiteiten in een tabel verleent te genereren. U maakt standaard een SAS-token voor een hele tabel, maar het is ook mogelijk om op te geven dat het SAS-token toegang verlenen tot een bereik van **PartitionKey** waarden of een bereik van **PartitionKey** en **RowKey** waarden. U kunt selecteren voor het genereren van SAS-tokens voor afzonderlijke gebruikers van uw systeem dat de SAS-token van elke gebruiker kan alleen ze toegang tot hun eigen entiteiten in de tabelservice.  

## <a name="asynchronous-and-parallel-operations"></a>Asynchrone en parallelle bewerkingen
Mits u kunt uw verzoeken om te worden verspreid over meerdere partities, kunt u de doorvoer en client reactiesnelheid verbeteren met behulp van asynchrone of parallelle query's.
U wellicht bijvoorbeeld twee of meer werkprocessen rolinstanties toegang krijgen tot uw tabellen parallel. U kunt afzonderlijke werkrollen die verantwoordelijk zijn voor bepaalde sets van partities hebben of gewoon hebben meerdere worker rolinstanties, elke toegang kunnen krijgen tot alle partities in een tabel.  

U kunt binnen een clientexemplaar doorvoer verbeteren door opslagbewerkingen asynchroon uitgevoerd. De Storage-clientbibliotheek kunt eenvoudig schrijven van asynchrone query's en wijzigingen. Bijvoorbeeld, kunt u bijvoorbeeld starten met de synchrone methode die alle entiteiten in een partitie haalt zoals weergegeven in de volgende C#-code:  

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

U kunt deze code eenvoudig wijzigen zodat de query wordt asynchroon als volgt uitgevoerd:  

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

* Handtekening van de methode bevat nu de **asynchrone** aanpassingsfunctie en retourneert een **taak** exemplaar.  
* In plaats van aanroepen de **ExecuteSegmented** methode voor het ophalen van de resultaten van de methode nu roept de **ExecuteSegmentedAsync** methode en gebruikt de **await** aanpassingsfunctie resultaten asynchroon ophalen.  

De clienttoepassing kan deze methode niet aanroepen meerdere keren (met verschillende waarden voor de **afdeling** parameter), en elke query wordt uitgevoerd op een afzonderlijke thread.  

Houd er rekening mee dat er geen asynchrone versie van is de **Execute** methode in de **TableQuery** klasse omdat de **IEnumerable** interface ondersteunt geen asynchrone opsomming.  

U kunt invoegen, bijwerken en verwijderen van entiteiten asynchroon. De volgende C#-voorbeeld ziet u een eenvoudige, synchrone methode invoegen of vervangen van een werknemer entiteit:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

U kunt deze code eenvoudig wijzigen zodat de update wordt asynchroon als volgt uitgevoerd:  

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

* Handtekening van de methode bevat nu de **asynchrone** aanpassingsfunctie en retourneert een **taak** exemplaar.  
* In plaats van aanroepen de **Execute** methode voor het bijwerken van de entiteit, de methode nu roept de **ExecuteAsync** methode en gebruikt de **await** aanpassingsfunctie resultaten asynchroon ophalen.  

De clienttoepassing kan meerdere asynchrone methoden zoals deze aanroepen en elke methodeaanroep wordt uitgevoerd op een afzonderlijke thread.  

## <a name="next-steps"></a>Volgende stappen

- [Relaties te modelleren](table-storage-design-modeling.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Versleuteling van tabelgegevens](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)