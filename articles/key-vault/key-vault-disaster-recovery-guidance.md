---
title: Wat te doen in het geval van een Azure-service wordt onderbroken die gevolgen heeft voor Azure Key Vault - Azure Key Vault | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die gevolgen heeft voor Azure Key Vault.
services: key-vault
author: barclayn
manager: barbkess
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: barclayn
ms.openlocfilehash: dba1fe91a635f467f4a3aeeaa048897065822869
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66236642"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault beschikbaarheid en redundantie

Azure Key Vault bevat meerdere lagen van redundantie om ervoor te zorgen dat uw sleutels en geheimen voor uw toepassing beschikbaar blijven, zelfs als afzonderlijke onderdelen van de service mislukt.

De inhoud van uw key vault worden gerepliceerd binnen de regio en naar een secundaire regio ten minste 150 mijl opgeslagen maar binnen dezelfde Geografie. Hierdoor blijven de duurzaamheid van uw sleutels en geheimen. Zie de [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) document voor meer informatie over specifieke regioparen.

Als afzonderlijke onderdelen in de sleutelkluis-service is mislukt, stap alternatieve onderdelen binnen de regio om uw aanvraag om ervoor te zorgen dat er geen afname van de functionaliteit is. U hoeft niet te doen voor het activeren van. Het gebeurt automatisch en transparant voor u.

In het zeldzame geval dat een hele Azure-regio niet beschikbaar is, worden de aanvragen die u van Azure Key Vault in die regio maakt automatisch doorgestuurd (*failover*) naar een secundaire regio. Als de primaire regio weer beschikbaar is, aanvragen weer worden gerouteerd (*failback*) naar de primaire regio. Nogmaals, hoeft u niet geen actie te ondernemen omdat dit automatisch gebeurt.

Via dit ontwerp voor hoge beschikbaarheid vereist Azure Key Vault zonder uitvaltijd voor onderhoudswerkzaamheden.

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
  * Unwrap
  * Verifiëren
  * Meld u
  * Backup
* Na een failover is failback, alle aanvraagtypen (waaronder lezen *en* schrijfaanvragen) beschikbaar zijn.

