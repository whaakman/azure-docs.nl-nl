---
title: Aanbevelingen voor Azure Advisor hoge beschikbaarheid | Microsoft Docs
description: Azure Advisor gebruiken voor het verbeteren van hoge beschikbaarheid van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: ea8d8d0adbc7cf5a8dfb6e9af51257b9d2ba8db2
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39264079"
---
# <a name="advisor-high-availability-recommendations"></a>Advisor-aanbevelingen voor hoge beschikbaarheid

Azure Advisor helpt u ervoor te zorgen en de continuïteit van uw essentiële toepassingen verbeteren. Krijgt u aanbevelingen voor hoge beschikbaarheid door Advisor uit de **hoge beschikbaarheid** van de Advisor-dashboard.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Zorg ervoor dat de virtuele machine-fouttolerantie gebruikt

U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Advisor identificeert virtuele machines die geen deel uitmaken van een beschikbaarheidsset en de gebruiker wordt aanbevolen dat u ze in een beschikbaarheidsset. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen om te maken van een beschikbaarheidsset voor de virtuele machine of de virtuele machine toevoegen aan een bestaande beschikbaarheidsset.

> [!NOTE]
> Als u ervoor kiest een beschikbaarheidsset maken, moet u ten minste één extra virtuele machine toevoegen aan het. U wordt aangeraden dat u twee of meer virtuele machines in een beschikbaarheidsset om ervoor te zorgen dat ten minste één machine instellen is beschikbaar tijdens een storing.

## <a name="ensure-availability-set-fault-tolerance"></a>Zorg ervoor dat de fouttolerantie van beschikbaarheidsset 

U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Advisor identificeert beschikbaarheidssets met een enkele virtuele machine en beveelt een of meer virtuele machines toe te voegen. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen om te maken van een virtuele machine of een bestaande virtuele machine toevoegen aan de beschikbaarheidsset.  

## <a name="ensure-application-gateway-fault-tolerance"></a>Zorg ervoor dat de fouttolerantie van application gateway
Om ervoor te zorgen de bedrijfscontinuïteit van essentiële toepassingen die worden aangestuurd door de Toepassingsgateways, Advisor identificeert application gateway-instanties die niet zijn geconfigureerd voor fouttolerantie en er wordt verwezen naar herstelacties die u kunt ondernemen. Advisor identificeert middelgrote of grote single instance Toepassingsgateways en dit is de aanbevolen ten minste één meer exemplaar toe te voegen. Ook één of meerdere instance korte Toepassingsgateways identificeert en wordt aanbevolen voor middelgrote of grote SKU's migreren. Advisor raadt aan om deze acties om ervoor te zorgen dat uw application gateway-instanties zijn geconfigureerd om te voldoen aan de huidige SLA-vereisten voor deze resources.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>De prestaties en betrouwbaarheid van virtuele-machineschijven verbeteren

Advisor identificeert virtuele machines met standaardschijven en beveelt een upgrade naar premium-schijven.
 
Azure Premium Storage voorziet in ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines die I/O-intensieve workloads uitvoeren. VM-schijven die gebruikmaken van premium storage-accounts worden gegevens opgeslagen op SSD-schijven (SSD's). Voor de beste prestaties voor uw toepassing, wordt u aangeraden dat u alle schijven van virtuele machines waarvoor hoge IOPS naar premium storage migreert. 

Als uw schijven geen hoge IOPS vereisen, kunt u de kosten beperken door het onderhoud ervan in de standard-opslag. Standard-opslag opgeslagen gegevens van de virtuele machine-schijf op hardeschijfstations (HDD's) in plaats van SSD's. U kunt de schijven van uw virtuele machines migreren naar premium-schijven. Premium-schijven worden ondersteund op de meeste VM-SKU's. In sommige gevallen, als u wilt gebruikmaken van premium-schijven, hoeft u wellicht uw virtuele machine SKU's ook bijwerken.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>De gegevens van uw virtuele machines beschermen tegen onopzettelijk verwijderen

Instellen van de virtuele machine back-up zorgt ervoor dat de beschikbaarheid van uw bedrijfskritieke gegevens en biedt beveiliging tegen per ongeluk verwijderen of beschadiging.  Advisor identificeert virtuele machines waarbij back-up is niet ingeschakeld, en dit is de aanbevolen back-up inschakelen. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Zorg ervoor dat u toegang tot experts van Azure-cloud hebt u nodig hebt

Wanneer een essentiële werkbelasting uitvoert, is het belangrijk dat u hebt toegang tot technische ondersteuning wanneer dat nodig is. Advisor identificeert mogelijke bedrijfskritische abonnementen waarvoor geen technische ondersteuning in hun support-plan opgenomen en wordt aanbevolen een upgrade naar een optie met technische ondersteuning.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Waarschuwingen wilt worden gewaarschuwd bij problemen met Azure met Azure Service Health maken

Het wordt aangeraden de instellen van Azure Service Health waarschuwingen wilt worden gewaarschuwd bij problemen met Azure-service. [Azure Service Health](https://azure.microsoft.com/features/service-health/) is een gratis service die biedt gepersonaliseerde begeleiding en ondersteuning wanneer u wordt beïnvloed door een probleem met Azure-service. Advisor identificeert abonnementen waarvoor geen waarschuwingen geconfigureerd en raadt aan om het maken van een.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Traffic Manager-eindpunten voor tolerantie configureren

Traffic Manager-profielen met meer dan één eindpunt optreden hogere mate van beschikbaarheid als een bepaald eindpunt is mislukt. Eindpunten in verschillende regio's verder plaatsen verbetert de betrouwbaarheid van de service. Advisor identificeert Traffic Manager-profielen, waarbij er slechts één eindpunt en de gebruiker wordt aanbevolen ten minste één meer eindpunt toe te voegen in een andere regio.

Als alle eindpunten in een Traffic Manager-profiel dat is geconfigureerd voor de routering van de service zich in dezelfde regio bevinden, kunnen gebruikers uit andere regio's verbindingsvertragingen optreden. Toe te voegen of het verplaatsen van een eindpunt naar een andere regio wordt de algehele prestaties verbeteren en een betere beschikbaarheid als alle eindpunten in één regio mislukken. Advisor identificeert Traffic Manager-profielen die zijn geconfigureerd voor de service Routering waar alle eindpunten die zich in dezelfde regio bevinden bevinden en raadt aan om toe te voegen of het verplaatsen van een eindpunt naar een andere Azure-regio.

Als een Traffic Manager-profiel is geconfigureerd voor de geografische routering, wordt verkeer doorgestuurd naar eindpunten op basis van gedefinieerde regio's. Als een regio uitvalt, is er geen vooraf gedefinieerde failover. Met een eindpunt waar de regionale groepering is geconfigureerd voor 'Alle (wereld)' te voorkomen dat verkeer wordt verwijderd en de servicebeschikbaarheid te verbeteren. Advisor identificeert Traffic Manager-profielen die zijn geconfigureerd voor de geografische routering waarbij er geen eindpunt is geconfigureerd voor de regionale groepering als 'Alle (wereld)' hebben en deze wijziging configuratie aanbevolen.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Gegevensverschil op uw SQL datawarehouse-tabel te verhogen van de prestaties van query's verwijderen

Gegevensverschil kan leiden tot onnodige gegevens knelpunten in verkeer of resource biedt bij het uitvoeren van uw workload. Advisor detecteert distributiegegevens scheeftrekken groter is dan 15% is en aangeraden dat u uw gegevens distribueren en terugkeren naar uw tabel distributie sleutel selecties. Zie voor meer informatie over het identificeren en verwijderen van scheeftrekken [probleemoplossing scheeftrekken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Maken of bijwerken van verouderde tabelstatistieken op uw SQL datawarehouse-tabel voor betere queryprestaties

Advisor identificeert de tabellen die u geen recente hebt [tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) en de gebruiker wordt aanbevolen maken of bijwerken van statistieken tabel. Query's optimaliseren up-to-date statische waarden gebruikt om te schatten van de kardinaliteit of het aantal rijen in het queryresultaat waarmee het queryoptimalisatieprogramma om een hoge kwaliteit queryplan voor de snelste prestaties te maken voor de SQL data warehouse.

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Toegang tot de aanbevelingen voor hoge beschikbaarheid in Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op de Advisor-dashboard op de **hoge beschikbaarheid** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:
* [Inleiding tot Azure Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Aanbevelingen van Advisor-kosten](advisor-performance-recommendations.md)
* [Advisor-aanbevelingen voor prestaties](advisor-performance-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)

