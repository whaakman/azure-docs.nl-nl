---
title: Documentatie voor Azure Operations Management (OMS) - zelfstudies | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) is een cloudoplossing voor IT-beheer van Microsoft waarmee u uw on-premises en cloudinfrastructuur kunt beheren en beveiligen. In dit artikel worden de verschillende services van OMS beschreven en vindt u koppelingen naar gedetailleerde inhoud.
services: operations-management-suite
author: czeumault
manager: carolz
layout: LandingPage
ms.assetid: 
ms.service: operations-management-suite
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 01/23/2017
ms.author: carolz
ms.openlocfilehash: 12f959376d4923e4e2481e37108ade632ac14902
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Wat is Operations Management Suite (OMS)?
Microsoft Operations Management Suite (OMS) is een cloudoplossing voor IT-beheer van Microsoft waarmee u uw on-premises en cloudinfrastructuur kunt beheren en beveiligen.  Aangezien OMS wordt geïmplementeerd als een cloudservice, kunt u er al snel mee aan de slag, en dat met minimale investeringen in infrastructuurservices.  Nieuwe functies worden automatisch geleverd, zodat u bespaart op kosten voor lopend onderhoud en upgrades.

Naast het feit dat OMS afzonderlijk waardevolle services heeft te bieden, kan de suite ook worden geïntegreerd met onderdelen van System Center, zoals System Center Operations Manager, als u uw huidige investeringen in beheerfuncties in de cloud wilt uitbreiden.  Door de samenwerking van System Center en OMS krijgt u de beschikking over een volledige hybride beheeroplossing.

De volgende secties bevatten een algemene beschrijving van de diverse waardegebieden van OMS en de services die voor de implementatie ervan zorgen.  U kunt de secties over de OMS-architectuur raadplegen voor een overzicht van de verschillende onderdelen van OMS, voordat u de gedetailleerde documentatie voor elk van de onderdelen raadpleegt.

## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Inzicht en analyses](media/operations-management-suite-overview/icon-insight-analytics.png) Inzicht en analyses
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) helpt u om logboek- en prestatiegegevens die worden gegenereerd door besturingssystemen en toepassingen, te verzamelen, aan elkaar de correleren, te vinden en er actie op te ondernemen. Log Analytics biedt u operationeel inzicht in realtime omdat er gebruik wordt gemaakt van geïntegreerde zoek- en aangepaste dashboards. Hiermee kunnen zonder problemen miljoenen records voor al uw werkbelastingen en servers, ongeacht hun fysieke locatie, worden geanalyseerd.

U kunt eenvoudig oplossingen toevoegen aan Log Analytics die bepalen welke gegevens er moeten worden verzameld en welke logica er moet worden gebruikt om ze te analyseren.  Deze oplossingen bieden mogelijk aanvullende functionaliteit die automatisch wordt geleverd aan agents met een minimale of geen configuratie.  U kunt naast het gebruik van analysehulpprogramma's die deel uitmaken van afzonderlijke oplossingen, aangepaste zoekopdrachten uitvoeren in de volledige gegevensset om zo correlaties te vinden tussen gegevens van systemen en van toepassingen.  

## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automation en besturing](media/operations-management-suite-overview/icon-automation-control.png) Automation en besturing
Met Azure Automation automatiseert u administratieve processen door middel van [runbooks](../automation/automation-runbook-types.md) die zijn gebaseerd op PowerShell en in de Azure-cloud worden uitgevoerd.  Runbooks hebben toegang tot andere producten of services die kunnen worden beheerd met PowerShell, met inbegrip van resources in andere clouds zoals Amazon Web Services (AWS).  Runbooks kunnen ook worden uitgevoerd op een server in uw lokale datacentrum om lokale resources te beheren.

Azure Automation biedt configuratiebeheer met [PowerShell DSC](../automation/automation-dsc-overview.md).  U kunt DSC-resources maken en beheren die in Azure worden gehost. U kunt deze vervolgens toepassen op systemen in de cloud of op on-premises systemen om zo hun configuratie te definiëren en deze automatisch af te dwingen.

## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Beveiliging en herstel](media/operations-management-suite-overview/icon-protection-recovery.png) Beveiliging en herstel na noodgevallen
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) beschermt uw toepassingsgegevens en bewaart deze jarenlang, zonder dat u grote investeringen hoeft te doen en met een minimum aan operationele kosten.  U kunt er back-ups van gegevens mee maken afkomstig van fysieke en virtuele Windows-servers, naast back-ups van werkbelastingen zoals SQL Server en SharePoint.  Azure Backup kan ook worden gebruikt door System Center Data Protection Manager (DPM) om beveiligde gegevens te repliceren naar Azure voor redundantie en opslag voor de lange termijn.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) draagt bij aan de bedrijfscontinuïteit en de strategie voor herstel na noodgevallen (BCDR) door de replicatie, failover en het herstel van virtuele on-premises Hyper-V-machines, virtuele VMware-machines en fysieke Windows/Linux-servers in te delen. U kunt machines naar een secundair datacenter repliceren of uw datacentrum uitbreiden door ze naar Azure te repliceren. Site Recovery biedt ook eenvoudige functionaliteit voor failover en herstel voor werkbelastingen. Site Recovery is geïntegreerd met mechanismen voor herstel na noodgevallen, zoals SQL Server AlwaysOn, en biedt plannen voor herstel voor een eenvoudige failover van werkbelastingen die zich gelaagd op meerdere computers bevinden.

## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![Beveiliging en naleving in OMS](media/operations-management-suite-overview/icon-security-compliance.png) Beveiliging en naleving
Beveiliging en naleving helpt u beveiligingsrisico's voor uw infrastructuur te identificeren, te beoordelen en te verhelpen.  Deze functies van OMS worden geïmplementeerd via meerdere oplossingen in Log Analytics. Deze analyseren logboekgegevens en configuraties van agentsystemen en helpen u zo om uw omgeving permanent te beveiligen.

* De [oplossing Beveiliging en controle](oms-security-getting-started.md) verzamelt en analyseert beveiligingsgebeurtenissen op beheerde systemen om verdachte activiteiten te identificeren.
* De [antimalwareoplossing](../log-analytics/log-analytics-malware.md) rapporteert over de status van de antimalwarebeveiliging op beheerde systemen.  
* De oplossing Systeemupdates analyseert de beveiligingsupdates en andere updates op uw beheerde systemen, zodat u eenvoudig kunt zien op welke systemen een patch moet worden toegepast.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Meer informatie over [Azure Automation](../automation/automation-intro.md).
* Meer informatie over [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Meer informatie over [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).

