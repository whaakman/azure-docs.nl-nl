---
title: Ontwerp schaalbare en zodat tabellen in de Azure table storage. | Microsoft Docs
description: Ontwerp schaalbare en zodat tabellen in de Azure table storage.
services: storage
documentationcenter: na
author: SnehaGunda
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: ba48283371ac099b162aeb3cbffd6127ccce1676
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660817"
---
# <a name="design-scalable-and-performant-tables"></a>Schaalbare en performante tabellen ontwerpen

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Ontwerp schaalbare en zodat tabellen, moet u rekening houden met factoren zoals prestaties, schaalbaarheid en kosten. Als u eerder schema's voor relationele databases hebt ontworpen, deze overwegingen bekend zijn, maar er zijn een aantal overeenkomsten tussen de Azure Table storage servicemodel en relationele modellen, maar er zijn ook belangrijke verschillen. Deze verschillen worden doorgaans leiden tot verschillende ontwerpen dat mogelijk Zoek vindt of onjuiste naar iemand bekend bent met relationele databases nog zinvol zijn als u voor een NoSQL-sleutel/waarde-archief zoals de service Azure Table ontwerpt. Veel van de verschillen in het ontwerp overeenstemming met het feit dat de tabel-service is ontworpen ter ondersteuning van cloud-toepassingen met miljarden entiteiten (of rijen in een relationele database-terminologie) van gegevens of voor gegevenssets die ondersteuning voor hoge transactie bieden moet volumes. Daarom moet u anders nadenken over hoe u uw gegevens opslaat en inzicht in de werking van de tabelservice. Een goed ontworpen NoSQL-gegevensarchief kunt uw oplossing voor het schalen van veel verder en tegen lagere kosten dan een oplossing die gebruikmaakt van een relationele database inschakelen. Deze handleiding helpt u met deze onderwerpen.  

## <a name="about-the-azure-table-service"></a>Over de Azure Table-service
Deze sectie worden enkele van de belangrijkste functies van de tabel-service die vooral relevant voor ontwerpen voor prestaties en schaalbaarheid zijn. Als u geen ervaring met Azure Storage en de tabelservice, eerst lezen [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md) en [aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) voordat de rest van dit artikel lezen . Hoewel deze handleiding in de tabelservice is, bevat dit onderwerp een van de Azure-wachtrij en de Blob-services, en hoe u ze kunt gebruiken met de tabelservice.  

Wat is de tabel-service? Als u van de naam verwacht, wordt in de tabel-service tabelvorm gebruikt voor het opslaan van gegevens. Elke rij van de tabel een entiteit vertegenwoordigt in de standaard terminologie en de kolommen opslaan van de verschillende eigenschappen van die entiteit. Elke entiteit heeft een paar sleutels om uniek te identificeren en een timestamp-kolom die de tabel-service gebruikt om bij te houden wanneer de entiteit voor het laatst is bijgewerkt. De tijdstempel wordt automatisch toegepast en u kunt handmatig de tijdstempel niet overschrijven met een willekeurige waarde. De tabel-service gebruikt deze tijdstempel laatst is gewijzigd (LMT) voor het beheren van optimistische gelijktijdigheid.  

> [!NOTE]
> De tabel service REST-API-bewerkingen ook retourneren een **ETag** waarde die deze is afgeleid van de LMT. Dit document maakt gebruik van de voorwaarden ETag en LMT door elkaar omdat ze naar de onderliggende gegevens verwijzen.  
> 
> 

Het volgende voorbeeld ziet een eenvoudige tabelontwerp voor het opslaan van entiteiten werknemer en afdeling. Veel van de voorbeelden die verderop in deze handleiding zijn gebaseerd op dit eenvoudige ontwerp.  

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
<th>Voornaam</th>
<th>LastName</th>
<th>Leeftijd</th>
<th>Email</th>
</tr>
<tr>
<td>Jan</td>
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
<th>Voornaam</th>
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
<th>Voornaam</th>
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


Deze gegevens weergegeven tot nu toe lijkt op een tabel in een relationele database met de belangrijkste verschillen, wordt de verplichte kolommen en de mogelijkheid voor het opslaan van meerdere Entiteitstypen in dezelfde tabel. Bovendien elk van de gebruiker gedefinieerde eigenschappen zoals **FirstName** of **leeftijd** heeft een gegevenstype, zoals geheel getal of tekenreeks, net zoals een kolom in een relationele database. Hoewel in tegenstelling tot in een relationele database, de schema-minder aard van de tabel-service betekent dat een eigenschap moet niet hetzelfde gegevenstype voor elke entiteit. Complexe gegevenstypen opslaan in één eigenschap, moet u een zoals JSON of XML-serialisatie-indeling. Zie voor meer informatie over de tabel-service, zoals ondersteunde gegevenstypen, ondersteunde datumbereiken, naamgevingsregels en beperkingen voor [inzicht in de tabel Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Uw keuze van **PartitionKey** en **RowKey** is van cruciaal belang goede tabelontwerp. Elke entiteit die is opgeslagen in een tabel moet een unieke combinatie van **PartitionKey** en **RowKey**. Als u met de sleutels in een relationele database, de **PartitionKey** en **RowKey** waarden worden geïndexeerd voor het maken van een geclusterde index zodat snel te zoeken. Echter, de tabel-service maakt geen secundaire indexen, dus **PartitionKey** en **RowKey** zijn de enige geïndexeerde eigenschappen. Sommige van de patronen die zijn beschreven in [ontwerppatronen voor tabel](table-storage-design-patterns.md) laten zien hoe u deze duidelijk beperking kunt omzeilen.  

Een tabel bestaat uit een of meer partities en veel van de ontwerpbeslissingen die u aanbrengt worden rond het kiezen van een geschikte **PartitionKey** en **RowKey** optimaliseren van uw oplossing. Een oplossing kan bestaan uit één tabel die de entiteiten die zijn onderverdeeld in partities bevat, maar een oplossing heeft doorgaans meerdere tabellen. Tabellen kunt u logisch ordenen van uw entiteiten, helpen bij het beheren van toegang tot de gegevens met behulp van toegangsbeheerlijsten en u kunt een hele tabel met een één opslag-bewerking neerzetten.  

## <a name="table-partitions"></a>Tabelpartities
De accountnaam, de tabelnaam van de en **PartitionKey** samen bepalen de partitie in de storage-service waar de entiteit in de tabelservice worden opgeslagen. Onderdeel van het adresseringsschema voor entiteiten zijn, partities een bereik voor transacties definiëren (Zie [entiteit groepstransacties](#entity-group-transactions) hieronder), en de basis vormen van de manier waarop de tabelservice schaalt. Zie voor meer informatie over partities [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md).  

Een afzonderlijke knooppunten services in de tabel-service, een of meer partities en de service kan worden geschaald voltooien door dynamisch taakverdeling partities over knooppunten. Als een knooppunt belast wordt, de tabelservice kunt *splitsen* het bereik van de partities onderhouden door dat knooppunt op verschillende knooppunten; wanneer verkeer subsidies, de service kan *samenvoegen* de partitie kan variëren van stille knooppunten back naar één knooppunt.  

Zie het artikel voor meer informatie over de interne details van de tabel-service en met name hoe de service partities beheert, [Microsoft Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

## <a name="entity-group-transactions"></a>Entiteit groep transacties
Entiteit groepstransacties (EGTs) zijn in de tabel-service de enige ingebouwde mechanisme voor het uitvoeren van atomaire updates tussen meerdere entiteiten. EGTs worden soms ook aangeduid als *batch transacties*. EGTs werkt alleen voor entiteiten die zijn opgeslagen in dezelfde partitie (dat wil zeggen, delen dezelfde partitiesleutel in een bepaalde tabel). Dus telkens wanneer u atomic transactionele gedrag tussen meerdere entiteiten vereist, u ervoor zorgen moet dat deze entiteiten in dezelfde partitie zijn. Dit is vaak een reden voor meerdere tabellen niet gebruiken voor andere entiteitstypen en meerdere Entiteitstypen houden in dezelfde tabel (en de partitie). Een enkele EGT kan werken op maximaal 100 entiteiten.  Als u meerdere gelijktijdige EGTs voor verwerking indienen, is het belangrijk om ervoor te zorgen dat die EGTs worden niet uitgevoerd op de entiteiten die gemeenschappelijk voor EGTs zijn; anders wordt de verwerking kan worden uitgesteld.

EGTs ook veroorzaken een mogelijke waarde om te beoordelen in uw ontwerp. Dat wil zeggen, verhoogt met behulp van meer partities de schaalbaarheid van uw toepassing omdat Azure heeft meer mogelijkheden voor aanvragen voor taakverdeling over de knooppunten. Maar meer partities gebruikt, kan de mogelijkheid van uw toepassing uit te voeren atomische transacties sterke consistentie voor uw gegevens beperken. Er zijn bovendien specifieke schaalbaarheidsdoelen op het niveau van een partitie die de doorvoer van transacties die u voor één knooppunt verwachten kunt kan beperken. Zie voor meer informatie over de schaalbaarheidsdoelen voor Azure storage-accounts en de tabelservice [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md).   

## <a name="capacity-considerations"></a>Overwegingen voor capaciteitsplanning
De volgende tabel worden enkele van de belangrijkste waarden moet denken bij het ontwerpen van een tabel-service-oplossing beschreven:  

| Totale capaciteit van een Azure storage-account | 500 TB |
| --- | --- |
| Aantal tabellen in een Azure storage-account |Alleen beperkt door de capaciteit van het storage-account |
| Aantal partities in een tabel |Alleen beperkt door de capaciteit van het storage-account |
| Aantal entiteiten in een partitie |Alleen beperkt door de capaciteit van het storage-account |
| Grootte van een afzonderlijke entiteit |Maximaal 1 MB van maximaal 255 eigenschappen (inclusief de **PartitionKey**, **RowKey**, en **tijdstempel**) |
| Grootte van de **PartitionKey** |Een tekenreeks maximaal 1 KB groot |
| Grootte van de **RowKey** |Een tekenreeks maximaal 1 KB groot |
| Grootte van een entiteit groep-transactie |Een transactie kan maximaal 100 entiteiten bevatten en de lading moet minder dan 4 MB groot. Een EGT kan slechts eenmaal bijwerken een entiteit. |

Zie voor meer informatie [inzicht in de tabel Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

## <a name="cost-considerations"></a>Kosten overwegingen
Tabelopslag relatief goedkope is, maar moet u kosten maakt een schatting voor capaciteitsgebruik en het aantal transacties opnemen als onderdeel van de evaluatie van een tabel-service-oplossing. In veel scenario's is gedenormaliseerd of dubbele gegevens op te slaan om te verbeteren de prestaties of de schaalbaarheid van uw oplossing echter een geldige benadering. Zie voor meer informatie over prijzen [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Relaties te modelleren](table-storage-design-modeling.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Versleuteling van tabelgegevens](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)