---
title: Hoge beschikbaarheid van Azure Analysis Services | Microsoft Docs
description: Zodat zeker Azure Analysis Services met hoge beschikbaarheid.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 554c5e6e3e3cfa2742ef27a3c1510176184b6bd0
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="analysis-services-high-availability"></a>Hoge beschikbaarheid van Analysis Services
Dit artikel wordt beschreven zodat zeker hoge beschikbaarheid voor Azure Analysis Services-servers. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Hoge beschikbaarheid zodat zeker tijdens een service wordt onderbroken
Hoewel dit zelden voorkomt, kan een storing in een Azure-datacenter hebben. Wanneer er een storing optreedt, wordt er een onderbreking van de bedrijven die mogelijk een paar minuten laatste of uur kan duren. Hoge beschikbaarheid wordt vaak bereikt met server redundantie. U kunt met Azure Analysis Services redundantie bereiken door het maken van aanvullende, secundaire servers in een of meer regio's. Bij het maken van redundante servers, om ervoor te zorgen voor de gegevens en metagegevens op die servers in de synchronisatie is met de server in een regio die is geworden offline, kunt u:

* Het implementeren van modellen voor redundante servers in andere regio's. Deze methode moet verwerken van gegevens op uw primaire server en de redundante servers in parallel, zodat alle servers zeker synchroon.

* Back-up databases uit de primaire server en terugzetten op servers met redundante. U kunt bijvoorbeeld elke nacht back-ups naar Azure-opslag automatiseren en herstellen naar andere redundante servers in andere regio's. 

Als een storing optreedt in de primaire server moet u in beide gevallen moet de verbindingsreeksen in reporting clients verbinding maken met de server in een andere regionale datacenter wijzigen. Deze wijziging als een laatste toevlucht moet worden beschouwd en alleen als een onherstelbare regionale datacentrum optreedt. Het is waarschijnlijk een datacentrum die als host fungeert voor de primaire server weer online zou komen voordat verbindingen op alle clients kan worden bijgewerkt. 



## <a name="related-information"></a>Gerelateerde informatie
[Back-up en herstel](analysis-services-backup.md)   
[Azure analyseservices beheren](analysis-services-manage.md) 

