---
title: Azure aanbevelingen voor hoge beschikbaarheid van Advisor | Microsoft Docs
description: Gebruik Azure Advisor voor het verbeteren van hoge beschikbaarheid van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 7b63de2453180e562596c211d40cebe85b95bd54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-high-availability-recommendations"></a>Hoge beschikbaarheid aanbevelingen Advisor te ontvangen

Azure Advisor kunt u ervoor te zorgen en de continuïteit van uw bedrijfskritieke toepassingen te verbeteren. U krijgt aanbevelingen voor hoge beschikbaarheid door Advisor van de **hoge beschikbaarheid** van de Advisor-dashboard.

![Hoge beschikbaarheid knop op de Advisor-dashboard](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Zorg ervoor dat de virtuele machine fouttolerantie

Advisor identificeert virtuele machines die geen deel uitmaken van een beschikbaarheidsset en raadt in een beschikbaarheidsset. U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens beide gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt kiezen voor het maken van een beschikbaarheidsset voor de virtuele machine of de virtuele machine toevoegen aan een bestaande beschikbaarheidsset.

> [!NOTE]
> Als u een beschikbaarheidsset maakt, moet u ten minste één meer virtuele machine in de App toevoegen. U wordt aangeraden die groep u twee of meer virtuele machines in een beschikbaarheidsset instellen om ervoor te zorgen dat ten minste één van de machine is beschikbaar tijdens een storing.

![Advisor aanbeveling: voor de redundantie van de virtuele machine, gebruik beschikbaarheidssets](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Zorg ervoor dat de beschikbaarheidsset fouttolerantie 

Advisor identificeert beschikbaarheidssets met een enkele virtuele machine en raadt aan om een of meer virtuele machines toe te voegen. U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens beide gepland of ongepland onderhoud, beschikbaar is en voldoet aan de virtuele machine van Azure SLA. U kunt een virtuele machine maken of gebruiken van een bestaande virtuele machine en toe te voegen aan de beschikbaarheidsset.  

![Advisor aanbeveling: een of meer virtuele machines toevoegen aan deze beschikbaarheidsset](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Zorg ervoor dat fouttolerantie voor application gateway
De bedrijfscontinuïteit van bedrijfskritieke toepassingen die gemaakt, via Toepassingsgateways aangeboden zijn Advisor identificeert toepassingsexemplaren gateway die niet zijn geconfigureerd voor fouttolerantie en er wordt verwezen naar herstelacties die u kunt ondernemen. Advisor identificeert grote of middelgrote single instance Toepassingsgateways en raadt aan om ten minste één meer exemplaar toe te voegen. Ook één of meerdere instance kleine Toepassingsgateways identificeert en raadt aan om te migreren naar grote of middelgrote SKU's. Advisor raadt aan om deze acties om ervoor te zorgen dat uw toepassing gateway-exemplaren zijn geconfigureerd om te voldoen aan de huidige SLA-vereisten voor deze resources.

![Advisor aanbeveling: twee of meer grote of middelgrote grootte gatewayexemplaren van een toepassing implementeren](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>De prestaties en betrouwbaarheid van de virtuele-machineschijven verbeteren

Advisor identificeert virtuele machines met standaardschijven en raadt aan om een upgrade naar de premium-schijven.
 
Azure Premium-opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines die I/O-intensieve werkbelastingen worden uitgevoerd. Virtuele machine-schijven die gebruikmaken van premium storage-accounts opslaan gegevens op de SSD-schijven (SSD's). Voor de beste prestaties voor uw toepassing, wordt u aangeraden een hoge IOPS naar premium-opslag vereisen schijven op virtuele machine te migreren. 

Als uw schijven geen hoge IOPS vereisen, kunt u de kosten beperken door het onderhoud ervan in standard-opslag. Standard-opslag slaat schijfgegevens voor virtuele machine op de harde schijven (HDD's) in plaats van SSD's. U kunt kiezen voor het migreren van uw schijven op virtuele machine naar de premium-schijven. Premium-schijven worden op de meeste virtuele machine SKU's ondersteund. In sommige gevallen, als u wilt gebruiken van premium-schijven, hoeft u wellicht uw virtuele machine SKU's ook te upgraden.

![Advisor aanbeveling: de betrouwbaarheid van uw virtuele-machineschijven verbeteren door te upgraden naar de premium-schijven](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>De gegevens van uw virtuele machines te beschermen tegen onopzettelijk verwijderen
Advisor identificeert virtuele machines waar back-up is niet ingeschakeld en het beveelt back-up inschakelen. Instellen van de virtuele machine back-up zorgt ervoor dat de beschikbaarheid van uw bedrijfskritieke gegevens en biedt bescherming tegen onbedoeld worden verwijderd of beschadigd.

![Advisor aanbeveling: back-up van virtuele machine voor het beveiligen van uw bedrijfskritieke gegevens configureren](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Toegang tot hoge beschikbaarheid aanbevelingen in Advisor

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik in het linkerdeelvenster op **meer services**.

3. Klik in het deelvenster service menu onder **bewaking en beheer**, klikt u op **Azure Advisor**.  
 De Advisor-dashboard wordt weergegeven.

4. Klik op het dashboard Advisor de **hoge beschikbaarheid** tabblad en selecteer vervolgens het abonnement waarvoor u wilt ontvangen van aanbevelingen.

> [!NOTE]
> Voor toegang tot de aanbevelingen Advisor te ontvangen, moet u eerst *registreren van uw abonnement* met Advisor. Een abonnement is geregistreerd als een *abonnement eigenaar* start van de Advisor-dashboard en klikt op de **aanbevelingen krijgen** knop. Dit is een *eenmalige bewerking*. Nadat het abonnement is geregistreerd, kunt u de aanbevelingen van Advisor als openen *eigenaar*, *Inzender*, of *lezer* voor een abonnement, resourcegroep of een specifieke bron.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor aanbevelingen:
* [Inleiding tot Azure Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Advisor kosten aanbevelingen](advisor-performance-recommendations.md)
* [Aanbevelingen van Advisor-prestaties](advisor-performance-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-security-recommendations.md)

