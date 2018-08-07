---
title: Ontwerp schaalbare en performante tabellen in Azure-tabelopslag. | Microsoft Docs
description: Ontwerp schaalbare en performante tabellen in Azure-tabelopslag.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.component: tables
ms.openlocfilehash: 783522997a752c4eac575316983bc6ef853c3f43
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526909"
---
# <a name="design-scalable-and-performant-tables"></a>Schaalbare en beter bruikbare tabellen ontwerpen

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Ontwerp schaalbare en performante tabellen, moet u rekening houden met factoren zoals prestaties, schaalbaarheid en kosten. Als u eerder schema's voor relationele databases hebt ontworpen, deze overwegingen bekend bent, maar er zijn enkele overeenkomsten tussen de Azure Table storage servicemodel en relationele modellen, maar er zijn ook belangrijke verschillen. Deze verschillen leidt doorgaans tot verschillende ontwerpen. deze kunnen erg intuïtief of verkeerd aan iemand kent relationele databases, maar zinvol zijn als u voor een NoSQL-sleutel/waarde-archief, zoals de Azure Table-service ontwerpt. Veel van de verschillen in het ontwerp overeenstemming met het feit dat de Table-service is ontworpen ter ondersteuning van-toepassingen op cloudschaal met miljarden entiteiten (of rijen in de relationele database-terminologie) van de gegevens of voor gegevenssets die ondersteuning voor hoge transactie bieden moet volumes. Daarom moet u denkt dat anders over hoe u uw gegevens opslaat en te begrijpen hoe de Table-service werkt. Een goed ontworpen NoSQL-gegevensarchief kunt uw oplossing voor het schalen van veel verder en tegen lagere kosten dan een oplossing die gebruikmaakt van een relationele database inschakelen. Deze handleiding helpt u bij deze onderwerpen.  

## <a name="about-the-azure-table-service"></a>Over de Azure Table-service
Deze sectie worden enkele van de belangrijkste functies van de Table-service die vooral relevant zijn voor het ontwerpen voor prestaties en schaalbaarheid. Als u geen ervaring met Azure Storage en de tabelservice, eerst lezen [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md) en [aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) voordat de rest van dit artikel lezen . Hoewel de focus van deze handleiding in de tabelservice is, bevat deze bespreking van de Azure-wachtrij en Blob-services en hoe u ze kunt gebruiken met de Table-service.  

Wat is de Table-service? Als u van de naam verwacht, de tabelservice tabelvorm gebruikt voor het opslaan van gegevens. Staat elke rij van de tabel voor een entiteit in de standard-terminologie en de kolommen opslaan van de verschillende eigenschappen van die entiteit. Elke entiteit heeft een combinatie van sleutel voor het aanduiden van, en een timestamp-kolom die de tabel-service gebruikt om bij te houden wanneer de entiteit voor het laatst is bijgewerkt. De tijdstempel wordt automatisch toegepast en u kunt handmatig de tijdstempel niet overschrijven met een willekeurige waarde. De Table-service maakt gebruik van deze timestamp laatst gewijzigd (LMT) voor het beheren van optimistische gelijktijdigheid.  

> [!NOTE]
> De tabel-service REST API-bewerkingen retourneren ook een **ETag** waarde die deze is afgeleid van de LMT. Dit document maakt gebruik van de voorwaarden ETag en LMT door elkaar omdat ze naar dezelfde onderliggende gegevens verwijzen.  
> 
> 

Het volgende voorbeeld ziet het ontwerp van een eenvoudige tabel om op te slaan en de werknemers en afdelingen entiteiten. Veel van de voorbeelden verderop in deze handleiding zijn gebaseerd op dit eenvoudige ontwerp.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>Don</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>jun</td>
<td>CaO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Afdeling</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>Naam van de afdeling</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Verkoop</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Tot nu toe, deze gegevens worden weergegeven die vergelijkbaar is met een tabel in een relationele database met de belangrijkste verschillen, wordt de verplichte kolommen en de mogelijkheid voor het opslaan van meerdere Entiteitstypen in dezelfde tabel. Bovendien elk van de gebruiker gedefinieerde eigenschappen zoals **FirstName** of **leeftijd** heeft een gegevenstype, zoals geheel getal of tekenreeks, net zoals een kolom in een relationele database. Hoewel in tegenstelling tot in een relationele database, de zonder schema aard van de Table-service betekent dat een eigenschap niet hetzelfde gegevenstype voor elke entiteit moet hebben. Voor het opslaan van complexe gegevenstypen in één eigenschap, moet u een geserialiseerde indeling zoals JSON of XML. Zie voor meer informatie over de tabel-service, zoals ondersteunde gegevenstypen, ondersteunde datumbereiken, naamgevingsregels en beperkingen [inzicht in het Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Uw eigen keuze aan **PartitionKey** en **RowKey** is fundamenteel voor een goede tabelontwerp. Elke entiteit die zijn opgeslagen in een tabel moet een unieke combinatie van **PartitionKey** en **RowKey**. Net als bij de sleutels in een relationele database-tabel de **PartitionKey** en **RowKey** waarden worden geïndexeerd voor het maken van een geclusterde index om in te schakelen snel op te zoeken. Echter, de Table-service maakt geen secundaire indexen, dus **PartitionKey** en **RowKey** zijn de enige geïndexeerde eigenschappen. Sommige van de patronen die worden beschreven in [tabel ontwerppatronen](table-storage-design-patterns.md) laten zien hoe u deze zichtbaar beperking kunt omzeilen.  

Een tabel bestaat uit een of meer partities en zijn veel van de ontwerpbeslissingen die u ervoor kiezen een geschikt **PartitionKey** en **RowKey** het optimaliseren van uw oplossing. Een oplossing kan bestaan uit één tabel met de entiteiten die zijn onderverdeeld in partities, maar een oplossing heeft doorgaans meerdere tabellen. Tabellen kunt u logisch ordenen van uw entiteiten, helpen bij het beheren van toegang tot de gegevens met behulp van toegangsbeheerlijsten en u kunt een hele tabel met behulp van een enkele opslagbewerking neerzetten.  

## <a name="table-partitions"></a>Tabelpartities
De accountnaam, de tabelnaam, en **PartitionKey** samen bepalen de partitie in de storage-service waar de entiteit in de table-service worden opgeslagen. Als u deelneemt aan het adresschema gebruiken voor entiteiten, partities een bereik voor transacties definiëren (Zie [entiteit-groepstransacties](#entity-group-transactions) hieronder), en vormen de basis van hoe de table-service kan worden geschaald. Zie voor meer informatie over partities [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md).  

Een knooppunt van de afzonderlijke services in de tabel-service een of meer partities en de service schalen voltooien door dynamische taakverdeling partities over knooppunten. Als een knooppunt belast wordt, de table-service kunt *splitsen* het bereik van partities afgehandeld door dat knooppunt aan andere knooppunten; wanneer netwerkverkeer afneemt, de service kunt *samenvoegen* de partitie kan variëren van stille knooppunten terug op een enkel knooppunt.  

Raadpleeg het artikel voor meer informatie over de interne details van de Table-service, en met name hoe partities worden beheerd door de service, [Microsoft Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

## <a name="entity-group-transactions"></a>Entiteit-groepstransacties
Entiteit-groepstransacties (EGTs) zijn in de tabel-service, het enige ingebouwde mechanisme voor het uitvoeren van atomic updates voor meerdere entiteiten. EGTs worden soms ook aangeduid als *batch transacties*. EGTs kan alleen worden uitgevoerd op entiteiten die zijn opgeslagen in dezelfde partitie (dat wil zeggen, delen dezelfde partitiesleutel in een bepaalde tabel). Dus telkens wanneer u atomische transactionele gedrag voor meerdere entiteiten vereist, u ervoor zorgen moet dat deze entiteiten in dezelfde partitie zijn. Dit is vaak een reden voor het bewaren van meerdere Entiteitstypen in dezelfde tabel (en de partitie) en meerdere tabellen voor verschillende Entiteitstypen niet gebruiken. Een enkele EGT kan worden uitgevoerd op maximaal 100 entiteiten.  Als u meerdere gelijktijdige EGTs voor verwerking indient, is het belangrijk om ervoor te zorgen dat die EGTs worden niet uitgevoerd op entiteiten die betrekking hebben op EGTs; anders wordt de verwerking kan worden uitgesteld.

EGTs introduceert ook een mogelijke compromis voor u om te evalueren in uw ontwerp. Dat wil zeggen, verhoogt met behulp van meer partities de schaalbaarheid van uw toepassing, omdat Azure heeft meer mogelijkheden om aanvragen te verdelen over meerdere knooppunten. Maar het vermogen van uw toepassing atomische transacties uitvoeren en onderhouden van sterke consistentie voor uw gegevens met behulp van meer partities mogelijk beperken. Er zijn bovendien specifieke schaalbaarheidsdoelen op het niveau van een partitie die de doorvoer van transacties die u voor een enkel knooppunt verwachten kunt mogelijk beperken. Zie voor meer informatie over de schaalbaarheidsdoelen voor Azure storage-accounts en de tabelservice [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md).   

## <a name="capacity-considerations"></a>Overwegingen voor capaciteit
De volgende tabel worden enkele van de sleutelwaarden rekening mee moet houden bij het ontwerpen van een tabel-service-oplossing beschreven:  

| Totale capaciteit van een Azure storage-account | 500 TB |
| --- | --- |
| Aantal tabellen in een Azure storage-account |Alleen beperkt door de capaciteit van het storage-account |
| Aantal partities in een tabel |Alleen beperkt door de capaciteit van het storage-account |
| Aantal entiteiten in een partitie |Alleen beperkt door de capaciteit van het storage-account |
| Grootte van een afzonderlijke entiteit |Maximaal 1 MB met een maximum van 255 eigenschappen (met inbegrip van de **PartitionKey**, **RowKey**, en **Timestamp**) |
| Grootte van de **PartitionKey** |Een tekenreeks van 1 KB groot |
| Grootte van de **RowKey** |Een tekenreeks van 1 KB groot |
| Grootte van een transactie entiteitsgroep |Een transactie mag maximaal 100 entiteiten en de payload moet minder dan 4 MB groot zijn. Een entiteit kunt slechts één keer bijwerken door een EGT. |

Zie [Het gegevensmodel van de tabelservice](http://msdn.microsoft.com/library/azure/dd179338.aspx) voor meer informatie.  

## <a name="cost-considerations"></a>Kostenoverwegingen
Tabelopslag is relatief goedkope, maar u moet de geraamde kosten voor zowel het capaciteitsgebruik van en het aantal transacties opnemen als onderdeel van de evaluatie van een tabel-service-oplossing. In veel scenario's is gedenormaliseerde of dubbele gegevens opslaan om te verbeteren de prestaties of de schaalbaarheid van uw oplossing echter een geldig benadering. Zie voor meer informatie over prijzen [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Waardoor relaties worden gemaakt](table-storage-design-modeling.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Versleutelen van gegevens in een tabel](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)