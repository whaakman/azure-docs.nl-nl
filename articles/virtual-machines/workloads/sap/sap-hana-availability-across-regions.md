---
title: Beschikbaarheid van de SAP HANA volledige Azure-regio's | Microsoft Docs
description: Overzicht van beschikbaarheid overwegingen beschrijft wanneer SPA HANA wordt uitgevoerd op Azure Virtual machines
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 052394884f85d527caa88ff6c9464af669f47bb5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-across-azure-regions"></a>Beschikbaarheid van de SAP HANA volledige Azure-regio 's
In dit artikel worden de scenario's SAP HANA-beschikbaarheid over verschillende Azure-regio's beschreven en besproken. Gezien het feit dat afzonderlijke Azure-regio's groter afstand tussen deze twee hebben, wordt er speciale overwegingen die worden vermeld in dit artikel zijn.

## <a name="motivation-to-deploy-across-multiple-azure-regions"></a>Doel voor het implementeren van over meerdere Azure-regio 's
Andere Azure-regio's worden gescheiden door een grotere afstand. Afhankelijk van de geopolitieke regio, dit kan zijn mijl honderden of zelfs enkele duizenden mijl, zoals in de Verenigde Staten. Vanwege de afstand tussen de verschillende Azure-regio's, netwerkverkeer tussen de activa in twee verschillende Azure-regio's ervaringen aanzienlijke RTT netwerklatentie geïmplementeerd. Aanzienlijke genoeg synchrone gegevensuitwisseling tussen twee SAP HANA-exemplaren onder normale SAP werklast uitsluiten. Aan de andere kant u vaak worden momenteel geconfronteerd met het feit dat er gedefinieerde vereiste op afstand tussen uw primaire datacenter en een secundair datacenter om beschikbaarheid in geval van een natuurramp, kunt u door een ruimere gebied is. Zoals orkanen druk die op het gebied caraïbische en Florida in September en oktober 2017. Of ten minste een vereiste minimale afstand. In de meeste van de aanvragen van de klant, moet u voor een ontwerp voor beschikbaarheid van de hele met deze definitie voor de minimale afstand [Azure-gebieden](https://azure.microsoft.com/regions/). Aangezien de afstand te groot tussen is twee Azure-regio's gebruikt die synchrone replicatiemodus van HANA, de vereisten van RTO en RPO forceert mogelijk u configuraties van de beschikbaarheid binnen één regio implementeren en vervolgens vullen met aanvullende implementaties in een tweede de regio.

Een ander aspect worden overwogen in deze configuraties is de failover en de omleidings-client. De veronderstelling is dat een failover tussen SAP HANA-exemplaren in twee verschillende Azure-regio's altijd een handmatige failover. Omdat de replicatiemodus voor van SAP HANA System Replication is ingesteld op asynchrone, is het mogelijk dat gegevens die zijn vastgelegd in het primaire HANA-exemplaar is niet het geworden nog naar de secundaire HANA-exemplaar. Automatische failover kan daarom niet worden geaccepteerd voor configuraties waarin de replicatie asynchroon is. Zelfs bij handmatige failover gecontroleerde, zoals in een oefening failover moet u maatregelen om ervoor te zorgen dat alle vastgelegde gegevens op de primaire kant met het exemplaar van de secundaire gemaakt voordat handmatig naar de andere Azure-regio te verplaatsen.
 
Aangezien u met een ander IP-adresbereik in de Azure VNets die zijn geïmplementeerd in de tweede Azure-regio, de SAP HANA-clients die beide moeten worden gewijzigd in de configuratie of een betere manier moet u de stappen voor het wijzigen van de naamomzetting plaatsen werken. Ja, dat de clients worden ophalen omgeleid naar de nieuwe secundaire de IP-adres. Het artikel SAP op [herstel van de Client verbinding na overname](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html) probeert meer informatie.   

## <a name="simple-availability-between-two-azure-regions"></a>Eenvoudige beschikbaarheid tussen twee Azure-regio 's
In dit scenario besloten u niet voor het opzetten van een willekeurige beschikbaarheidsconfiguratie binnen één regio. Maar u hebt de vraag om de werkbelasting die wordt geleverd als er een ramp optreedt. Typische gevallen voor systemen zoals die zijn niet-productieve systemen. Hoewel u tolereren kan omlaag om het systeem voor een halve of zelfs een dag, kunt u het systeem niet beschikbaar te zijn 48 uur of langer kan niet toestaan. Als doel het maken van de instellingen minder dure, voeren een andere die nog minder belangrijk bewaartermijn is de virtuele machine die als een doel fungeert of u het formaat van de virtuele machine in de secundaire regio kleinere en kies niet vooraf de gegevens te laden. Aangezien de failover is het verstandig om handmatige en veel meer stappen voor het failover de volledige toepassing-stack houdt, wordt de extra tijd buiten de virtuele machine, het formaat en start de virtuele machine opnieuw is acceptabel.

> [!NOTE]
> Zelfs zonder vooraf laden van gegevens in de doel-HANA System Replication, moet u minimaal 64 GB geheugen en voorbij dat voldoende geheugen de rowstore om gegevens te behouden in het geheugen van de doelinstantie.

![Twee virtuele machines via twee regio 's](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> In deze configuratie kunt u een RPO niet opgeven = 0, omdat de replicatiemodus HANA System asynchroon is. Als u wilt bieden een RPO = 0, deze configuratie is niet de configuratie van de keuze.

Een kleine wijziging van de configuratie kan worden vooraf laden van gegevens configureren. Gezien echter de handmatige aard van failover- en het feit dat toepassingslagen hoeft te verplaatsen naar de tweede regio, evenals het wellicht niet verstandig vooraf laden van gegevens. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Combineren van beschikbaarheid binnen één regio en in verschillende regio 's 
Een combinatie van beschikbaarheid binnen en tussen regio's kan worden bepaald door:

- Vereiste voor RPO = 0 binnen een Azure-regio.
- Niet bereid of beschikken over de algemene bewerkingen van het bedrijf dat wordt beïnvloed door een primaire natuurlijke ramp die van invloed op een grotere regio. Als de aanvraag door een aantal orkanen die de caraïbische treffers in de afgelopen jaren.
- Voorschriften die vraag afstanden tussen de primaire en secundaire site die duidelijk buiten de zones van welke Azure-beschikbaarheid kunnen bieden.

 
In dergelijke gevallen configureert u welke aanroepen SAP een [replicatieconfiguratie SAP HANA Multitier System](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) met HANA System replicatie. De architectuur eruit als:

![drie virtuele machines via twee regio 's](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Deze configuratie biedt een RPO = 0 met kleine RTO tijden binnen de primaire regio en bovendien biedt dalen RPO in geval van een verplaatsing tot de tweede regio. De RTO tijdstippen in de tweede regio zijn afhankelijk van of u vooraf laden van gegevens of niet configureren. In veel gevallen van de klant, wordt de virtuele machine in de secundaire regio worden gebruikt voor het uitvoeren van een testsysteem. Als gevolg van die gebruik van de gegevens niet vooraf laden.

> [!NOTE]
> Nadat u de bewerkingsmodus logreplay gebruikt voor replicatie van HANA System die van het niveau van Tier 1 naar Tier 2 (synchrone replicatie in de primaire regio), kan de replicatie tussen Tier 2 en laag 3 (replicatie naar secundaire site) niet in delta_datashipping bewerking modus. Details van bewerkingsmodi en er zijn enkele beperkingen worden gedocumenteerd volgens SAP in [bewerkingsmodi voor SAP HANA System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Volgende stappen
Als u stapsgewijze instructies voor het instellen van een dergelijke configuratie in Azure nodig hebt, leest u de artikelen:

- [SAP HANA System replicatie in virtuele machines in Azure instellen](sap-hana-high-availability.md)
- [Uw SAP op Azure – deel 4: hoge beschikbaarheid voor SAP HANA met behulp van replicatie van systeem](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
