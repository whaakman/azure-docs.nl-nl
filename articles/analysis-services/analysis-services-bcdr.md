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
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: ed2bb2fe159db146ee520fc600c8b11f2dd4f761
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="analysis-services-high-availability"></a>Hoge beschikbaarheid van Analysis Services
Dit artikel wordt beschreven zodat zeker hoge beschikbaarheid voor Azure Analysis Services-servers. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Hoge beschikbaarheid zodat zeker tijdens een service wordt onderbroken
Hoewel dit zelden voorkomt, kan een storing in een Azure-datacenter hebben. Wanneer er een storing optreedt, wordt er een onderbreking van de bedrijven die mogelijk een paar minuten laatste of uur kan duren. Hoge beschikbaarheid wordt vaak bereikt met server redundantie. U kunt met Azure Analysis Services redundantie bereiken door het maken van aanvullende, secundaire servers in een of meer regio's. Bij het maken van redundante servers, om ervoor te zorgen voor de gegevens en metagegevens op die servers in de synchronisatie is met de server in een regio die is geworden offline, kunt u:

* Het implementeren van modellen voor redundante servers in andere regio's. Deze methode moet verwerken van gegevens op uw primaire server en de redundante servers in parallel, zodat alle servers zeker synchroon.

* [Back-up](analysis-services-backup.md) databases van de primaire server en terugzetten op servers met redundante. U kunt bijvoorbeeld elke nacht back-ups naar Azure-opslag automatiseren en herstellen naar andere redundante servers in andere regio's. 

Als een storing optreedt in de primaire server moet u in beide gevallen moet de verbindingsreeksen in reporting clients verbinding maken met de server in een andere regionale datacenter wijzigen. Deze wijziging als een laatste toevlucht moet worden beschouwd en alleen als een onherstelbare regionale datacentrum optreedt. Het is waarschijnlijk een datacentrum die als host fungeert voor de primaire server weer online zou komen voordat verbindingen op alle clients kan worden bijgewerkt. 

Om te voorkomen dat verbindingsreeksen op reporting clients wijzigen, kunt u een server [alias](analysis-services-server-alias.md) voor de primaire server. Als de primaire server uitgeschakeld wordt, kunt u de alias die verwijzen naar een redundante server in een andere regio. U kunt automatiseren alias aan de naam van de statuscontrole van een eindpunt op de primaire server coderen. Als de statuscontrole is mislukt, kan hetzelfde eindpunt rechtstreeks naar een redundante server in een andere regio. 

## <a name="related-information"></a>Gerelateerde informatie
[Back-up en herstel](analysis-services-backup.md)   
[Azure analyseservices beheren](analysis-services-manage.md)   
[Servernamen alias](analysis-services-server-alias.md) 

