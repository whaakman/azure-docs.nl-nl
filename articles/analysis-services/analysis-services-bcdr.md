---
title: Azure Analysis Services met hoge beschikbaarheid | Microsoft Docs
description: Ingeschakeld, hoge beschikbaarheid van Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 51a0f560a0e4b6ff791d5ed3f9f221eb2eeb9b4d
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191340"
---
# <a name="analysis-services-high-availability"></a>Hoge beschikbaarheid van Analysis Services

Dit artikel wordt beschreven om hoge beschikbaarheid voor Azure Analysis Services-servers. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Hoge beschikbaarheid ingeschakeld tijdens een onderbreking van de service

Hoewel zeldzaam, kan een Azure-Datacenter een storing hebben. Wanneer een storing optreedt, veroorzaakt deze een bedrijfsonderbreking die mogelijk een paar minuten afgelopen of uren kan duren. Hoge beschikbaarheid wordt vaak bereikt met de server redundantie. Met Azure Analysis Services, kunt u redundantie bereiken door het maken van aanvullende, secundaire servers in een of meer regio's. Wanneer het maken van redundante servers, om ervoor te zorgen voor de gegevens en metagegevens op die servers is gesynchroniseerd met de server in een regio die offline is, kunt u:

* Implementeer modellen op redundante servers in andere regio's. Deze methode moet verwerken van gegevens op uw primaire server en de redundante servers in parallelle, om alle servers worden gesynchroniseerd.

* [Back-up](analysis-services-backup.md) databases van de primaire server en het terugzetten op redundante servers. U kunt bijvoorbeeld 's nachts back-ups naar Azure storage automatiseren en herstellen naar andere redundante servers in andere regio's. 

Als er een storing optreedt in de primaire server moet u in beide gevallen moet de verbindingsreeksen in reporting clients verbinding met de server in een ander regionaal datacenter te wijzigen. Deze wijziging moet worden beschouwd als een laatste toevlucht en alleen als een catastrofale regionale datacenterstoring optreedt. Is het meer waarschijnlijk een storing in het datacenter die als host fungeert voor de primaire server zou weer online komt voordat u verbindingen op alle clients kan bijwerken. 

Om te voorkomen dat tekenreeksen voor databaseverbindingen op rapportage clients wijzigen, kunt u een server [alias](analysis-services-server-alias.md) voor de primaire server. Als de primaire server uitvalt, kunt u de alias om te verwijzen naar een redundante server in een andere regio. U kunt automatiseren alias aan de naam van de statuscontrole van een eindpunt op de primaire server coderen. Als de controle van gatewayservicestatus is mislukt, wordt hetzelfde eindpunt kan sturen naar een redundante server in een andere regio. 

## <a name="related-information"></a>Gerelateerde informatie

[Back-up en herstel](analysis-services-backup.md)   
[Azure analyseservices beheren](analysis-services-manage.md)   
[De namen van de alias](analysis-services-server-alias.md) 

