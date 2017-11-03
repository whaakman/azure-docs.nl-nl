---
title: Gebruik van gegevens en lookup verwijzingsdimensies in Stream Analytics | Microsoft Docs
description: Referentiegegevens gebruiken in een Stream Analytics-query
keywords: referentiegegevens opzoektabel
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 438ec565f3c6e06ab7ec92cf1bbfbdde88f99b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Met behulp van verwijzingsdimensies of opzoeken van gegevens in een Stream Analytics-invoerstroom
Referentiegegevens (ook wel bekend als een opzoektabel) is een beperkte verzameling die is statisch of vertraging wijzigen van aard gebruikt om een zoekopdracht uitvoert of correleren met de gegevensstroom. Om ervoor te gebruiken van referentiegegevens in uw Azure Stream Analytics-taak, gebruikt u doorgaans een [verwijzing gegevens Join](https://msdn.microsoft.com/library/azure/dn949258.aspx) in uw Query. Stream Analytics maakt gebruik van Azure Blob storage als de storage-laag voor referentiegegevens en met Azure Data Factory-verwijzing gegevens kunnen worden omgezet en/of gekopieerd naar Azure Blob-opslag voor gebruik als referentie-gegevens van [alle cloud-gebaseerde aantal en de on-premises gegevensopslagexemplaren](../data-factory/copy-activity-overview.md). Referentiegegevens is gemodelleerd als een reeks blobs (gedefinieerd in de configuratie van de invoer) in oplopende volgorde van de datum/tijd opgegeven in de blob-naam. Deze **alleen** ondersteunt toe te voegen aan het einde van de reeks met behulp van een datum/tijd **groter** dan die is opgegeven door de laatste blob in de reeks.

Stream Analytics is een **limiet van 100 MB per blob** maar taken kunnen meerdere verwijzing blobs verwerken met behulp van de **pad patroon** eigenschap.


## <a name="configuring-reference-data"></a>Referentiegegevens configureren
Als u wilt uw referentiegegevens configureren, moet u eerst voor het maken van een invoer van het type **referentiegegevens**. De onderstaande tabel wordt elke eigenschap die u opgeven moet tijdens het maken van de referentiegegevens invoer met de beschrijving uitgelegd:


<table>
<tbody>
<tr>
<td>De naam van eigenschap</td>
<td>Beschrijving</td>
</tr>
<tr>
<td>Invoeralias</td>
<td>Een beschrijvende naam die wordt gebruikt in de taak query om te verwijzen naar deze invoer.</td>
</tr>
<tr>
<td>Opslagaccount</td>
<td>De naam van het opslagaccount waar uw BLOB's zich bevinden. Als deze zich binnen hetzelfde abonnement als uw Stream Analytics-taak, kunt u het selecteren in de vervolgkeuzelijst.</td>
</tr>
<tr>
<td>De sleutel van opslagaccount</td>
<td>De geheime sleutel die is gekoppeld aan het opslagaccount. Dit wordt automatisch ingevuld als het opslagaccount zich in hetzelfde abonnement als uw Stream Analytics-taak.</td>
</tr>
<tr>
<td>Storage-Container</td>
<td>Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob naar de Blob-service uploaden, moet u een blob-container opgeven.</td>
</tr>
<tr>
<td>Het pad</td>
<td>Het pad dat wordt gebruikt om uw blobs in de opgegeven container te vinden. U kunt kiezen binnen het pad naar een of meer exemplaren van de volgende 2 variabelen opgeven:<BR>{date} {time}<BR>Voorbeeld 1: products/{date}/{time}/product-list.csv<BR>Voorbeeld 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[Optioneel] datumnotatie</td>
<td>Als u {date} binnen het pad dat u hebt opgegeven gebruikt hebt, kunt u de datumnotatie waarin uw blobs zijn ingedeeld in de vervolgkeuzelijst met ondersteunde indelingen selecteren.<BR>Voorbeeld: Jjjj/MM/DD, DD-MM-jjjj, enzovoort.</td>
</tr>
<tr>
<td>[Optioneel] tijdnotatie</td>
<td>Als u {time} binnen het pad dat u hebt opgegeven gebruikt hebt, kunt u de tijdnotatie waarin uw blobs zijn ingedeeld in de vervolgkeuzelijst met ondersteunde indelingen selecteren.<BR>Bijvoorbeeld: HH uu/mm of uu mm</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Om ervoor te zorgen dat uw query's werken zoals verwacht, Stream Analytics moet weten welke serialisatie-indeling gebruikt u voor inkomende gegevensstromen. De ondersteunde indelingen zijn voor referentiegegevens, CSV en JSON.</td>
</tr>
<tr>
<td>Encoding</td>
<td>UTF-8 is de enige ondersteunde coderingsindeling op dit moment</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Referentiegegevens volgens een schema genereren
Als uw referentiegegevens een langzaam veranderende gegevensset is, wordt ondersteund voor het vernieuwen van verwijzing gegevens wordt ingeschakeld door het opgeven van een patroon pad in de configuratie van de invoer met de {date} en {time} vervanging tokens. Stream Analytics neemt over de bijgewerkte referentiegegevens gegevensdefinities op basis van dit patroon pad. Bijvoorbeeld, een patroon van `sample/{date}/{time}/products.csv` met een datumnotatie van **'Jjjj-MM-DD'** en tijdnotatie **'Uu mm'** Hiermee geeft u Stream Analytics om op te halen de bijgewerkte blob `sample/2015-04-16/17-30/products.csv` om 5:30 uur 16de April 2015 UTC-tijdzone.

> [!NOTE]
> Momenteel er Stream Analytics-taken voor het vernieuwen van de blob alleen wanneer de machinetijd wordt verplaatst naar de tijd die is gecodeerd in de blob-naam. Bijvoorbeeld: de taak zoekt `sample/2015-04-16/17-30/products.csv` zo snel mogelijk, maar geen eerder dan 5:30 PM 16de April 2015 UTC tijdzone. Het *nooit* zoekt u naar een blob met een gecodeerde tijd eerder dan de laatste versie die is gedetecteerd.
> 
> Bijvoorbeeld Zodra de taak de blob vindt `sample/2015-04-16/17-30/products.csv` deze bestanden met een gecodeerde datum ouder is dan 5:30 PM 16de April 2015 wordt genegeerd als een laat binnenkomen `sample/2015-04-16/17-25/products.csv` blob wordt gemaakt in dezelfde container de taak wordt niet gebruikt.
> 
> Op dezelfde manier als `sample/2015-04-16/17-30/products.csv` alleen op 10:03 PM 16de April 2015 wordt geproduceerd, maar geen blob met een eerdere datum is aanwezig in de container, de taak wordt dit bestand begint bij 10:03 PM 16de April 2015 gebruiken en gebruik van de vorige referentiegegevens tot die tijd.
> 
> Een uitzondering hierop is wanneer de taak opnieuw verwerken gegevens terug in tijd moet of wanneer de taak is het eerste gestart. Tijdens het starten is de taak op zoek naar de meest recente blob geproduceerd voordat de taak begintijd opgegeven. Dit wordt gedaan om ervoor te zorgen dat er een **niet-lege** verwijst naar gegevensset wanneer de taak wordt gestart. Als er een kan niet worden gevonden, de taak wordt weergegeven met de volgende diagnose: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kan worden gebruikt voor het indelen van de taak voor het maken van de bijgewerkte blobs vereist door Stream Analytics verwijzing gegevensdefinities bijwerken. Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd. Data Factory ondersteunt [verbinding te maken met een groot aantal cloud gebaseerd en on-premises gegevensopslagexemplaren](../data-factory/copy-activity-overview.md) en gemakkelijk verplaatsen van gegevens op een vaste planning die u opgeeft. Voor meer informatie en stapsgewijze instructies voor het instellen van een Data Factory-pijplijn voor het genereren van referentiegegevens voor Stream Analytics die wordt vernieuwd op basis van een vooraf gedefinieerde planning, Bekijk dit [GitHub voorbeeld](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tips voor het vernieuwen van uw verwijzingsgegevens
1. Stream Analytics om opnieuw te laden van de blob leidt niet tot verwijzing gegevensblobs overschrijven en in sommige gevallen kan dit leiden tot de taak is mislukt. De aanbevolen manier om te wijzigen van referentiegegevens is een nieuwe blob met behulp van de container en het pad patroon gedefinieerd in de invoer van de taak toevoegen en gebruiken van een datum/tijd **groter** dan die is opgegeven door de laatste blob in de reeks.
2. Verwijzing naar gegevensblobs zijn **niet** besteld door het tijdstip van de blob 'Laatst gewijzigd', maar alleen door de tijd en datum is opgegeven in de blob naam weergeven met de {date} en {time} vervangingen.
3. In enkele gevallen kan een taak moet teruggaan in tijd, daarom verwijzing gegevensblobs mag niet worden gewijzigd of verwijderd.

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
U hebt kennisgemaakt met Stream Analytics, een beheerde service voor streaminganalyse van gegevens afkomstig van het Internet of Things. Raadpleeg de volgende onderwerpen voor meer informatie over deze service:

* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
