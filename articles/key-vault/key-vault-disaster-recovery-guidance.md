---
title: Wat te doen in het geval van een Azure-service wordt onderbroken die gevolgen heeft voor Azure Key Vault | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die gevolgen heeft voor Azure Key Vault.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
editor: ''
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: barclayn
ms.openlocfilehash: 5ab6fc207d8a7732062a51a6215c25a4a0fac006
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465840"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault beschikbaarheid en redundantie
Azure Key Vault bevat meerdere lagen van redundantie om ervoor te zorgen dat uw sleutels en geheimen voor uw toepassing beschikbaar blijven, zelfs als afzonderlijke onderdelen van de service mislukt.

De inhoud van uw key vault worden gerepliceerd binnen de regio en naar een secundaire regio ten minste 150 mijl opgeslagen maar binnen dezelfde Geografie. Hierdoor blijven de duurzaamheid van uw sleutels en geheimen. Zie de [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) document voor meer informatie over specifieke regioparen.

Als afzonderlijke onderdelen in de sleutelkluis-service is mislukt, stap alternatieve onderdelen binnen de regio om uw aanvraag om ervoor te zorgen dat er geen afname van de functionaliteit is. U hoeft niet te doen voor het activeren van. Het gebeurt automatisch en transparant voor u.

In het zeldzame geval dat een hele Azure-regio niet beschikbaar is, worden de aanvragen die u van Azure Key Vault in die regio maakt automatisch doorgestuurd (*failover*) naar een secundaire regio. Als de primaire regio weer beschikbaar is, aanvragen weer worden gerouteerd (*failback*) naar de primaire regio. Nogmaals, hoeft u niet geen actie te ondernemen omdat dit automatisch gebeurt.

Er zijn enkele aanvullende opmerkingen rekening mee moet houden:

* In het geval van een failover regio duurt het enkele minuten voor de service failover wilt uitvoeren. Aanvragen die afkomstig zijn gedurende deze tijd kunnen mislukken totdat de failover is voltooid.
* Na een failover voltooid is, wordt uw key vault in de modus alleen-lezen is. Aanvragen die worden ondersteund in deze modus zijn:
  * Lijst met sleutelkluizen
  * Hiermee worden eigenschappen van sleutelkluizen
  * Geheimen vermelden
  * Ophalen van geheimen
  * Een lijst met sleutels
  * (Eigenschappen van) sleutels ophalen
  * Versleutelen
  * Ontsleutelen
  * Tekstterugloop
  * Uitpakken
  * Verifiëren
  * Ondertekenen
  * Backup
* Na een failover is failback, alle aanvraagtypen (waaronder lezen *en* schrijfaanvragen) beschikbaar zijn.

