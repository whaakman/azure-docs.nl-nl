---
title: Wat te doen in het geval van een Azure-service wordt onderbroken die gevolgen heeft voor Azure Sleutelkluis | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die gevolgen heeft voor Azure Sleutelkluis.
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 1f19c4bba2152eb021831dedda734fd54ee78d75
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Sleutelkluis beschikbaarheid en redundantie
Azure Sleutelkluis is uitgerust met meerdere lagen van redundantie om ervoor te zorgen dat uw sleutels en geheimen beschikbaar voor uw toepassing blijven zelfs als afzonderlijke onderdelen van de service is mislukt.

De inhoud van de sleutelkluis worden gerepliceerd binnen de regio en een secundaire regio ten minste 150 mijl verwijderd, maar binnen de dezelfde Geografie. Dit onderhoudt gebied van duurzaamheid van uw sleutels en geheimen. Zie de [Azure regio's gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) document voor meer informatie over specifieke regio-paren.

Als afzonderlijke onderdelen in de sleutelkluis-service is mislukt, stap alternatieve componenten binnen de regio in voor het uitvoeren van uw aanvraag om ervoor te zorgen dat er zonder enige vermindering van de functionaliteit is. U hoeft niet te doen dit activeren. Dit gebeurt automatisch en transparant voor u.

In het zeldzame geval dat een volledige Azure-regio niet beschikbaar is, de aanvragen die u van Azure Sleutelkluis in deze regio aanbrengt automatisch doorgestuurd (*failover*) naar een secundaire regio. Wanneer de primaire regio weer beschikbaar is, aanvragen terug doorgestuurd (*kan niet terug*) naar de primaire regio. Opnieuw hoeft niet geen actie te ondernemen omdat dit automatisch gebeurt.

Er zijn enkele aanvullende opmerkingen voor Let op:

* In het geval van een failover regio duurt enkele minuten duren voordat de service failover. Aanvragen die afkomstig zijn gedurende deze tijd mislukken totdat de failover is voltooid.
* Nadat een failover voltooid is, wordt de sleutelkluis in de modus alleen-lezen is. Aanvragen die worden ondersteund in deze modus zijn:
  * Lijst sleutelkluizen
  * Eigenschappen van sleutelkluizen opgehaald
  * Lijst met geheimen
  * Ophalen van geheimen
  * Lijst met sleutels
  * (Eigenschappen van)-sleutels ophalen
  * Versleutelen
  * Ontsleutelen
  * Tekstterugloop
  * Uitpakken
  * Verifiëren
  * Ondertekenen
  * Back-up
* Na een failover terug mislukte is, alle aanvraagtypen (inclusief lezen *en* schrijfaanvragen) beschikbaar zijn.

