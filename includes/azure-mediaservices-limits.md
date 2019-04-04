---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: b275a86f8fd35c43865fd920d1bfc9994a796a9c
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58919204"
---
>[!NOTE]
>Open een ondersteuningsticket te vragen voor een verhoging van de quota voor resources die niet zijn opgelost. Maak geen aanvullende Azure Media Services-accounts in een poging om op te halen van hogere limieten.

| Resource | Standaardlimiet | 
| --- | --- | 
| Azure Media Services-accounts in één abonnement | 25 (vast) |
| Gereserveerde Media-eenheden per Media Services-account |25 (S1)<br/>10 (S2, S3)<sup>1</sup> | 
| Taken per Media Services-account | 50.000<sup>2</sup> |
| Gekoppelde taken per taak | 30 (vast) |
| Activa per Media Services-account | 1.000.000|
| Activa per taak | 50 |
| Activa per taak | 100 |
| Unieke locators die aan één activum tegelijk zijn gekoppeld | 5<sup>4</sup> |
| Live kanalen per Media Services-account |5|
| Programma's met de status Gestopt per kanaal |50|
| Programma's met de status Wordt uitgevoerd per kanaal |3|
| Streaming-eindpunten die zijn gestopt of het uitvoeren van per Media Services-account|2|
| Streaming-eenheden per streaming-eindpunt |10 |
| Opslagaccounts | 1.000<sup>5</sup> (vast) |
| Beleidsregels | 1,000,000<sup>6</sup> |
| Bestandsgrootte| In sommige scenario's, moet u er een limiet is op de maximale bestandsgrootte voor verwerking in Media Services wordt ondersteund. <sup>7</sup> |

<sup>1</sup>als u het type wijzigen, bijvoorbeeld van S2 naar S1, de maximale gereserveerde eenheid limieten worden opnieuw ingesteld.

<sup>2</sup>hieronder vallen de in de wachtrij, voltooide, actieve en geannuleerde taken. Deze bevat geen verwijderde taken. U kunt de oude taken verwijderen met behulp van **IJob.Delete** of de **verwijderen** HTTP-aanvraag.

Vanaf 1 April 2017, is elke taakrecord in uw account die ouder is dan 90 dagen automatisch verwijderd, samen met de bijbehorende Taakrecords. Automatische verwijdering gebeurt zelfs als het totale aantal records lager dan het maximale quotum is. Als u wilt archiveren van de taak en gegevens, gebruikt u de code die wordt beschreven in [assets met Media Services .NET SDK beheren](../articles/media-services/previous/media-services-dotnet-manage-entities.md).

<sup>3</sup>wanneer u een aanvraag voor lijst taak entiteiten indienen, maximaal 1000 taken per aanvraag wordt geretourneerd. Bijhouden van alle ingediende taken, de bovenkant of overslaan van query's, zoals beschreven in [OData-queryopties system](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7)).

<sup>4</sup>locators zijn niet bedoeld voor het beheren van toegangsbeheer per gebruiker. Om afzonderlijke gebruikers verschillende toegangsrechten, digital rights management (DRM)-oplossingen te gebruiken. Zie voor meer informatie, [beveiligen van uw inhoud met Azure Media Services](../articles/media-services/previous/media-services-content-protection-overview.md).

<sup>5</sup>de storage-accounts moeten afkomstig zijn van hetzelfde Azure-abonnement.

<sup>6</sup>er geldt een limiet van 1.000.000 beleidsregels voor verschillende beleidsregels voor Media Services. Er is bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy. 

>[!NOTE]
> Als u altijd dezelfde dagen gebruiken en kunt u toegangsmachtigingen, gebruikt u de dezelfde beleids-ID. Zie voor informatie en een voorbeeld [assets met Media Services .NET SDK beheren](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies).

<sup>7</sup>als u inhoud naar een activum in Media Services uploadt te verwerken met een van de mediaprocessors van de service, let dan op de maximale bestandsgrootten die worden ondersteund. Activa zijn encoders zoals Media Encoder Standard en Media Encoder Premium Workflow of analyse-engines als Face Detector.

De maximale grootte die wordt ondersteund voor één blob wordt momenteel maximaal 5 TB in Azure Blob Storage. Aanvullende beperkingen gelden in Media Services op basis van de VM-grootten die worden gebruikt door de service. De volgende tabel bevat de limieten op de gereserveerde media-eenheden S1, S2 en S3. Als de bronbestand groter dan de limieten die zijn gedefinieerd in de tabel is, wordt uw coderingstaak mislukt. Als u 4K resolutie bronnen van lange duur van de codeert, moet u bent S3 gereserveerde media-eenheden gebruik voor de prestaties die nodig zijn. Hebt u 4K-inhoud die groter is dan de limiet van 260-GB op de S3 gereserveerde media-eenheden, contact met ons op amshelp@microsoft.com voor mogelijke oplossingen ter ondersteuning van uw scenario.

| Gereserveerde Media-eenheid | Maximale invoer grootte (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
