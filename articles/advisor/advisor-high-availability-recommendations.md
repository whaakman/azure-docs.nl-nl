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
ms.openlocfilehash: 42627649145b568b2b25411d182e5a36cdb025b0
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881185"
---
# <a name="improve-availability-of-your-application-with-azure-advisor"></a>Verbeter de beschikbaarheid van uw toepassing met Azure Advisor

Azure Advisor helpt u ervoor te zorgen en de continuïteit van uw essentiële toepassingen verbeteren. Krijgt u aanbevelingen voor hoge beschikbaarheid door Advisor uit de **hoge beschikbaarheid** van de Advisor-dashboard.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Zorg ervoor dat de virtuele machine-fouttolerantie gebruikt

U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Advisor identificeert virtuele machines die geen deel uitmaken van een beschikbaarheidsset en de gebruiker wordt aanbevolen dat u ze in een beschikbaarheidsset. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen om te maken van een beschikbaarheidsset voor de virtuele machine of de virtuele machine toevoegen aan een bestaande beschikbaarheidsset.

> [!NOTE]
> Als u ervoor kiest een beschikbaarheidsset maken, moet u ten minste één extra virtuele machine toevoegen aan het. U wordt aangeraden dat u twee of meer virtuele machines in een beschikbaarheidsset om ervoor te zorgen dat ten minste één machine instellen is beschikbaar tijdens een storing.

## <a name="ensure-availability-set-fault-tolerance"></a>Zorg ervoor dat de fouttolerantie van beschikbaarheidsset

U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Advisor identificeert beschikbaarheidssets met een enkele virtuele machine en beveelt een of meer virtuele machines toe te voegen. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen om te maken van een virtuele machine of een bestaande virtuele machine toevoegen aan de beschikbaarheidsset.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Beheerde schijven gebruiken voor verbeterde betrouwbaarheid van gegevens

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

Schakel [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) op uw storage-account zodat de blobs overgang naar een voorlopig verwijderde status in plaats van definitief verwijderd verwijderd. Wanneer gegevens worden overschreven, wordt een voorlopig verwijderde momentopname is gegenereerd om op te slaan de status van de gegevens overschreven. Voorlopig verwijderen gebruiken, kunt u om te herstellen als er onbedoeld verwijderen of worden overschreven. Advisor identificeert Azure Storage-accounts waarvoor geen voorlopig verwijderen zijn ingeschakeld en stelt dat u dit inschakelen.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>De actief / actief VPN-gateway configureren voor verbindingstolerantie

In de actief / actief-configuratie, beide exemplaren van een VPN-gateway S2S VPN-tunnels naar uw on-premises VPN-apparaat tot stand brengen. Als een gepland onderhoud of niet-geplande gebeurtenis op een gatewayexemplaar gebeurt, wordt verkeer omgeleid naar de andere actieve IPsec-tunnel automatisch. Azure Advisor worden VPN-gateways die niet zijn geconfigureerd als actief / actief-geïdentificeerd en voorstellen dat u deze voor hoge beschikbaarheid configureert.

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

