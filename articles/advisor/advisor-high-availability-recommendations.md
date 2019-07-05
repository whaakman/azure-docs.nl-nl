---
title: Verbeter de beschikbaarheid van uw toepassing met Azure Advisor | Microsoft Docs
description: Azure Advisor gebruiken voor het verbeteren van hoge beschikbaarheid van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: kasparks
ms.author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: d78d8a689b860162a742e85c155205f072a3667c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446781"
---
# <a name="improve-availability-of-your-application-with-azure-advisor"></a>Verbeter de beschikbaarheid van uw toepassing met Azure Advisor

Azure Advisor helpt u ervoor te zorgen en de continuïteit van uw essentiële toepassingen verbeteren. Krijgt u aanbevelingen voor hoge beschikbaarheid door Advisor uit de **hoge beschikbaarheid** van de Advisor-dashboard.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Zorg ervoor dat de virtuele machine-fouttolerantie gebruikt

U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Advisor identificeert virtuele machines die geen deel uitmaken van een beschikbaarheidsset en de gebruiker wordt aanbevolen dat u ze in een beschikbaarheidsset. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen om te maken van een beschikbaarheidsset voor de virtuele machine of de virtuele machine toevoegen aan een bestaande beschikbaarheidsset.

> [!NOTE]
> Als u ervoor kiest een beschikbaarheidsset maken, moet u ten minste één extra virtuele machine toevoegen aan het. U wordt aangeraden dat u twee of meer virtuele machines in een beschikbaarheidsset om ervoor te zorgen dat ten minste één machine instellen is beschikbaar tijdens een storing.

## <a name="ensure-availability-set-fault-tolerance"></a>Zorg ervoor dat de fouttolerantie van beschikbaarheidsset

U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Advisor identificeert beschikbaarheidssets met een enkele virtuele machine en beveelt een of meer virtuele machines toe te voegen. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen om te maken van een virtuele machine of een bestaande virtuele machine toevoegen aan de beschikbaarheidsset.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Managed Disks gebruiken voor verbeterde betrouwbaarheid van gegevens

Virtuele machines die zich in een beschikbaarheidsset met schijven die opslagaccounts of opslagschaaleenheden delen zijn niet ongevoelig voor één eenheid schalingsfouten tijdens onderbrekingen. Advisor worden geïdentificeerd deze beschikbaarheidssets, en het beste migreren naar Azure Managed Disks. Dit zorgt ervoor dat de schijven van de verschillende virtuele machines in de beschikbaarheidsset voldoende geïsoleerd zijn om te voorkomen dat een single point of failure. 

## <a name="ensure-application-gateway-fault-tolerance"></a>Zorg ervoor dat de fouttolerantie van application gateway

Deze aanbeveling zorgt ervoor dat de zakelijke continuïteit van uw essentiële toepassingen die worden aangestuurd door de Toepassingsgateways. Advisor identificeert application gateway-instanties die niet zijn geconfigureerd voor fouttolerantie en er wordt verwezen naar herstelacties die u kunt ondernemen. Advisor identificeert middelgrote of grote single instance Toepassingsgateways en dit is de aanbevolen ten minste één meer exemplaar toe te voegen. Ook één of meerdere instance korte Toepassingsgateways identificeert en wordt aanbevolen voor middelgrote of grote SKU's migreren. Advisor raadt aan om deze acties om ervoor te zorgen dat uw application gateway-instanties zijn geconfigureerd om te voldoen aan de huidige SLA-vereisten voor deze resources.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>De gegevens van uw virtuele machines beschermen tegen onopzettelijk verwijderen

Instellen van de virtuele machine back-up zorgt ervoor dat de beschikbaarheid van uw bedrijfskritieke gegevens en biedt beveiliging tegen per ongeluk verwijderen of beschadiging. Advisor identificeert virtuele machines waarbij back-up is niet ingeschakeld, en dit is de aanbevolen back-up inschakelen. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Zorg ervoor dat u toegang tot experts van Azure-cloud hebt u nodig hebt

Wanneer een essentiële werkbelasting uitvoert, is het belangrijk dat u hebt toegang tot technische ondersteuning wanneer dat nodig is. Advisor identificeert mogelijke bedrijfskritische abonnementen waarvoor geen technische ondersteuning in hun support-plan opgenomen en wordt aanbevolen een upgrade naar een optie met technische ondersteuning.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Waarschuwingen wilt worden gewaarschuwd bij problemen met Azure met Azure Service Health maken

Het wordt aangeraden de instellen van Azure Service Health waarschuwingen wilt worden gewaarschuwd bij problemen met Azure-service. [Azure Service Health](https://azure.microsoft.com/features/service-health/) is een gratis service die biedt gepersonaliseerde begeleiding en ondersteuning wanneer u wordt beïnvloed door een probleem met Azure-service. Advisor identificeert abonnementen waarvoor geen waarschuwingen geconfigureerd en raadt aan om het maken van een.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Traffic Manager-eindpunten voor tolerantie configureren

Traffic Manager-profielen met meer dan één eindpunt optreden hogere mate van beschikbaarheid als een bepaald eindpunt is mislukt. Eindpunten in verschillende regio's verder plaatsen verbetert de betrouwbaarheid van de service. Advisor identificeert Traffic Manager-profielen, waarbij er slechts één eindpunt en de gebruiker wordt aanbevolen ten minste één meer eindpunt toe te voegen in een andere regio.

Als alle eindpunten in een Traffic Manager-profiel dat is geconfigureerd voor de routering van de service zich in dezelfde regio bevinden, kunnen gebruikers uit andere regio's verbindingsvertragingen optreden. Toe te voegen of het verplaatsen van een eindpunt naar een andere regio wordt de algehele prestaties verbeteren en een betere beschikbaarheid als alle eindpunten in één regio mislukken. Advisor identificeert Traffic Manager-profielen die zijn geconfigureerd voor de service Routering waar alle eindpunten die zich in dezelfde regio bevinden bevinden. Dit is de aanbevolen toe te voegen of het verplaatsen van een eindpunt naar een andere Azure-regio.

Als een Traffic Manager-profiel is geconfigureerd voor de geografische routering, wordt verkeer doorgestuurd naar eindpunten op basis van gedefinieerde regio's. Als een regio uitvalt, is er geen vooraf gedefinieerde failover. Met een eindpunt waar de regionale groepering is geconfigureerd voor 'Alle (wereld)' te voorkomen dat verkeer wordt verwijderd en de servicebeschikbaarheid te verbeteren. Advisor identificeert Traffic Manager-profielen die zijn geconfigureerd voor de geografische routering waarbij er geen eindpunt is geconfigureerd voor de regionale groepering als 'Alle (wereld)' hebben. Dit is de aanbevolen wijzigen van de configuratie voor het maken van een eindpunt 'alle (wereld).

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-after-accidental-overwrite-or-deletion"></a>Voorlopig verwijderen gebruiken in uw Azure Storage-Account te bewaren en herstellen van gegevens na het per ongeluk overschrijven of verwijderen

Schakel [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) op uw storage-account zodat de blobs overgang naar een voorlopig verwijderde status in plaats van definitief verwijderd verwijderd. Als gegevens worden overschreven, wordt er een voorlopig verwijderde momentopname gegenereerd waarin de status van de overschreven gegevens wordt opgeslagen. Voorlopig verwijderen gebruiken, kunt u om te herstellen als er onbedoeld verwijderen of worden overschreven. Advisor identificeert Azure Storage-accounts waarvoor geen voorlopig verwijderen zijn ingeschakeld en stelt dat u dit inschakelen.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>De actief / actief VPN-gateway configureren voor verbindingstolerantie

In de actief / actief-configuratie, beide exemplaren van een VPN-gateway S2S VPN-tunnels naar uw on-premises VPN-apparaat tot stand brengen. Als een gepland onderhoud of niet-geplande gebeurtenis op een gatewayexemplaar gebeurt, wordt verkeer omgeleid naar de andere actieve IPsec-tunnel automatisch. Azure Advisor worden VPN-gateways die niet zijn geconfigureerd als actief / actief-geïdentificeerd en voorstellen dat u deze voor hoge beschikbaarheid configureert.

## <a name="use-production-vpn-gateways-to-run-your-production-workloads"></a>Productie-VPN-gateways gebruiken om uit te voeren van uw werkbelastingen voor productie

Azure Advisor controleert alle VPN-gateways die zijn van een basis-SKU en kunt het beste een productie-SKU in plaats daarvan te gebruiken. De basis-SKU is bedoeld voor ontwikkelings- en testdoeleinden. Productie-SKU's bieden een groter aantal tunnels, BGP-ondersteuning, actief / actief-configuratie-opties, aangepast Ipsec/IKE-beleid, en hogere stabiliteit en beschikbaarheid.

## <a name="repair-invalid-log-alert-rules"></a>Ongeldige logboekwaarschuwingsregels herstellen

Azure Advisor detecteert waarschuwingsregels die ongeldige query's die zijn opgegeven in de sectie van de voorwaarde. De logboekwaarschuwingsregels worden gemaakt in Azure Monitor en worden gebruikt om analytische query's met bepaalde intervallen uit te voeren. De resultaten van de query bepalen of er een waarschuwing moet worden gegeven. Analytics-query’s kunnen ongeldig worden als gevolg van wijzigingen in bronnen, tabellen of opdrachten waarnaar wordt verwezen. Advisor wordt aanbevolen dat u de query in de waarschuwingsregel om te voorkomen dat het ophalen van automatisch uitgeschakeld en zorg ervoor dat controle dekking van uw resources in Azure oplossen. [Meer informatie over het oplossen van regels voor waarschuwingen](https://aka.ms/aa_logalerts_queryrepair)

## <a name="configure-consistent-indexing-mode-on-your-cosmos-db-collection"></a>Consistente indexering modus op uw Cosmos DB-verzameling configureren

Azure Cosmos DB-containers geconfigureerd met de vertraagde indexering modus mogelijk van invloed op de actualiteit van de resultaten van query. Advisor detecteert containers op deze manier geconfigureerd en kunt het beste overschakelen naar consistente modus. [Meer informatie over het beleid in Cosmos DB indexeren](https://aka.ms/cosmosdb/how-to-manage-indexing-policy)

## <a name="configure-your-azure-cosmos-db-containers-with-a-partition-key"></a>Uw Azure Cosmos DB-containers met een partitiesleutel configureren

Azure Advisor identificeert Azure Cosmos DB niet-gepartitioneerde verzamelingen die hun quota ingerichte opslag nadert. Het wordt aangeraden deze verzamelingen migreren naar nieuwe verzamelingen met de definitie van een partitie, zodat ze kunnen automatisch worden uitgebreid door de service. [Meer informatie over het kiezen van een partitiesleutel](https://aka.ms/cosmosdb/choose-partitionkey)

## <a name="upgrade-your-azure-cosmos-db-net-sdk-to-the-latest-version-from-nuget"></a>Uw Azure Cosmos DB .NET SDK bijwerken naar de nieuwste versie van Nuget

Azure Advisor identificeert Azure Cosmos DB-accounts die met een oude versie van de .NET SDK en aan te bevelen upgraden naar de nieuwste versie van Nuget voor de nieuwste oplossingen, verbeterde prestaties en nieuwe mogelijkheden. [Meer informatie over Cosmos DB .NET SDK](https://aka.ms/cosmosdb/sql-api-sdk-dotnet)

## <a name="upgrade-your-azure-cosmos-db-java-sdk-to-the-latest-version-from-maven"></a>Uw Azure Cosmos DB Java SDK bijwerken naar de nieuwste versie van Maven

Azure Advisor identificeert Azure Cosmos DB-accounts die met een oude versie van de Java-SDK en aan te bevelen upgraden naar de meest recente versie van Maven voor de nieuwste oplossingen, verbeterde prestaties en nieuwe mogelijkheden. [Meer informatie over Cosmos DB Java SDK](https://aka.ms/cosmosdb/sql-api-sdk-dotnet)

## <a name="upgrade-your-azure-cosmos-db-spark-connector-to-the-latest-version-from-maven"></a>Uw Azure Cosmos DB Spark-connector bijwerken naar de nieuwste versie van Maven

Azure Advisor identificeert Azure Cosmos DB-accounts die met een oude versie van de Cosmos DB Spark-connector en kunt het beste een upgrade naar de meest recente versie van Maven voor de nieuwste oplossingen, verbeterde prestaties en nieuwe mogelijkheden. [Meer informatie over Cosmos DB Spark-connector](https://aka.ms/cosmosdb/spark-connector)

## <a name="enable-virtual-machine-replication"></a>Replicatie van virtuele machines inschakelen
Virtuele machines die geen replicatie is ingeschakeld op een andere regio zijn niet ongevoelig voor regionale storingen. Replicatie van virtuele machines, vermindert de gevolgen van eventuele nadelige business tijdens het ophalen van een storing van de Azure-regio. Advisor detecteert VM's waarvoor geen replicatie ingeschakeld en kunt het beste inschakelen van replicatie, zodat na een storing, u snel van uw virtuele machines in een externe Azure-regio brengen kunt. [Meer informatie over replicatie voor virtuele machines](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Toegang tot de aanbevelingen voor hoge beschikbaarheid in Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op de Advisor-dashboard op de **hoge beschikbaarheid** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:
* [Inleiding tot Azure Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Aanbevelingen van Advisor-kosten](advisor-cost-recommendations.md)
* [Advisor-aanbevelingen voor prestaties](advisor-performance-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)

