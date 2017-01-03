---
title: Wat is Log Analytics? | Microsoft Docs
description: Log Analytics is een service in Operations Management Suite (OMS) voor het verzamelen en analyseren van operationele gegevens die zijn gegenereerd door resources in uw cloud- en on-premises omgevingen.  Dit artikel geeft een kort overzicht van de verschillende onderdelen van Log Analytics en koppelingen naar gedetailleerde inhoud.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: bf1455cf3975b925d114acc3e1d4cba55f2a17a9
ms.openlocfilehash: 3e7e42e2ac2deedf936ffc4d246553f72ee4dcd3


---
# <a name="what-is-log-analytics"></a>Wat is Log Analytics?
Log Analytics is een service in [Operations Management Suite \(OMS\)](../operations-management-suite/operations-management-suite-overview.md) voor het verzamelen en analyseren van gegevens die zijn gegenereerd door resources in uw cloud- en on-premises omgevingen. Hierdoor kunt u in realtime inzichten verkrijgen met behulp van geïntegreerde zoekdashboard en aangepaste dashboards waarmee u eenvoudig miljoenen records voor al uw workloads en servers kunt analyseren, ongeacht de fysieke locatie.

## <a name="log-analytics-components"></a>Onderdelen van Log Analytics
In het centrum van Log Analytics bevindt zich de OMS-opslagplaats die in de Azure-cloud wordt beheerd.  Gegevens worden vanuit verbonden bronnen verzameld in de opslagplaats door gegevensbronnen te configureren en oplossingen toe te voegen aan uw abonnement.  Gegevensbronnen en oplossingen creëren elk verschillende recordtypen die hun eigen set eigenschappen hebben, maar nog steeds samen kunnen worden geanalyseerd in query's in de opslagplaats.  Hiermee kunt u dezelfde hulpprogramma's en methoden gebruiken om te werken met verschillende soorten gegevens die door verschillende bronnen zijn verzameld.

![OMS-opslagplaats](media/log-analytics-overview/overview.png)

Verbonden bronnen zijn de computers en andere bronnen die gegevens genereren die worden verzameld door Log Analytics.  Dit kunnen agenten zijn die zijn geïnstalleerd op [Windows-](log-analytics-windows-agents.md) en [Linux](log-analytics-linux-agents.md)-computers die rechtstreeks verbinding maken of agenten in een [verbonden beheergroep van System Center Operations Manager](log-analytics-om-agents.md).  Log Analytics kan ook gegevens verzamelen uit [Azure Storage](log-analytics-azure-storage.md).

[Gegevensbronnen](log-analytics-data-sources.md) zijn de verschillende soorten gegevens die uit elke verbonden bron worden verzameld.  Dit omvat gebeurtenissen en [prestatiegegevens](log-analytics-data-sources-performance-counters.md) van [Windows](log-analytics-data-sources-windows-events.md)- en Linux-agenten naast gegevensbronnen zoals [IIS-logboeken](log-analytics-data-sources-iis-logs.md) en [aangepaste tekstlogboeken](log-analytics-data-sources-custom-logs.md).  U configureert elke gegevensbron die u wenst te verzamelen en de configuratie wordt automatisch doorgegeven aan elke verbonden bron.

## <a name="analyzing-log-analytics-data"></a>Log Analytics-gegevens analyseren
Het grootste deel van uw interactie met Log Analytics verloopt via de OMS-portal die in elke browser wordt uitgevoerd en u toegang biedt tot configuratie-instellingen en meerdere hulpprogramma's om de verzamelde gegevens te analyseren en er actie op te ondernemen.  Vanuit de portal kunt u gebruikmaken van [zoekopdrachten in logboeken](log-analytics-log-searches.md) waar u query's samenstelt om verzamelde gegevens te analyseren, [dashboards](log-analytics-dashboards.md) die u kunt aanpassen met grafische views van uw waardevolste zoekopdrachten en [oplossingen](log-analytics-add-solutions.md) die aanvullende functionaliteit en analysehulpprogramma's bieden.

![OMS-portal](media/log-analytics-overview/portal.png)

Log Analytics biedt een querysyntaxis voor het snel ophalen en samenvoegen van gegevens in de opslagplaats.  U kunt [zoekopdrachten in logboeken](log-analytics-log-searches.md) maken en opslaan om gegevens direct in de OMS-portal te analyseren of automatisch zoekopdrachten in logboeken uitvoeren om een waarschuwing te genereren als de resultaten van de query een belangrijke toestand aangeven.

![Zoekopdrachten in logboeken](media/log-analytics-overview/log-search.png)

Om een snelle grafische weergave van de status van uw algehele omgeving te krijgen, kunt u visualisaties voor opgeslagen zoekopdrachten in logboeken toevoegen aan uw [dashboard](log-analytics-dashboards.md).   

![Dashboard](media/log-analytics-overview/dashboard.png)

Als u gegevens buiten Log Analytics wilt analyseren, kunt u de gegevens uit de OMS-opslagplaats OMS exporteren in hulpprogramma's zoals [Power BI](log-analytics-powerbi.md) of Excel.  U kunt ook gebruikmaken van de [API voor de zoekopdrachten in logboeken](log-analytics-log-search-api.md) om aangepaste oplossingen te bouwen die gebruikmaken van Log Analytics-gegevens of om te integreren met andere systemen.

## <a name="solutions"></a>Oplossingen
Oplossingen voegen functionaliteit toe aan Log Analytics.  Deze worden voornamelijk uitgevoerd in de cloud en analyseren gegevens die in de OMS-opslagplaats zijn verzameld. Ze kunnen ook nieuwe recordtypen definiëren om te verzamelen die kunnen worden geanalyseerd met zoekopdrachten in logboeken of door een aanvullende gebruikersinterface geleverd door de oplossing in het OMS-dashboard.  

![Traceringsoplossingen wijzigen](media/log-analytics-overview/change-tracking.png)

Oplossingen zijn beschikbaar voor verschillende functies en u kunt eenvoudig bladeren door beschikbare oplossingen en [deze toevoegen aan uw OMS-werkruimte](log-analytics-add-solutions.md) vanuit de Galerie van oplossingen.  Vele worden automatisch geïmplementeerd en werken onmiddellijk terwijl voor andere mogelijk enige configuratie vereist is.

![Galerie van oplossingen](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Architectuur van Log Analytics
De implementatievereisten van Log Analytics zijn minimaal omdat de centrale onderdelen in de Azure-cloud worden gehost.  Dit omvat de opslagplaats naast de services die u in staat stellen verzamelde gegevens te correleren en te analyseren.  De portal is toegankelijk vanuit elke browser. Er is dus geen vereiste voor de clientsoftware.

U moet agenten installeren op [Windows](log-analytics-windows-agents.md)- en [Linux](log-analytics-linux-agents.md)-computers, maar er is geen aanvullende agent vereist voor computers die al lid van zijn een [verbonden SCOM-beheergroep](log-analytics-om-agents.md).  SCOM-agenten blijven communiceren met beheerservers die hun gegevens doorsturen naar Log Analytics.  Een aantal oplossingen vereist echter agenten om rechtstreeks te communiceren met Log Analytics.  In de documentatie voor elke oplossing worden de communicatievereisten gespecificeerd.

Wanneer u [zich aanmeldt voor Log Analytics](log-analytics-get-started.md), maakt u een OMS-werkruimte.  U kunt de werkruimte zien als een unieke OMS-omgeving met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. U kunt meerdere werkruimten maken in uw abonnement om meerdere omgevingen zoals een productieomgeving en testomgeving te ondersteunen.

![Architectuur van Log Analytics](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Volgende stappen
* [Meld u aan voor een gratis Log Analytics-account](log-analytics-get-started.md) om in uw eigen omgeving te testen.
* Bekijk de verschillende [gegevensbronnen](log-analytics-data-sources.md) die beschikbaar voor het verzamelen van gegevens in de OMS-opslagplaats.
* [Blader door de beschikbare oplossingen in de Galerie van oplossingen](log-analytics-add-solutions.md) om functionaliteit toe te voegen aan Log Analytics.




<!--HONumber=Dec16_HO2-->


