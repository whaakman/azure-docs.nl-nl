---
title: Beschikbaarheid van het SAP HANA volledige Azure-regio's | Microsoft Docs
description: Een overzicht van beschikbaarheid overwegingen bij het uitvoeren van SAP HANA op Azure VM's in meerdere Azure-regio's.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: edbd1885dd529e4ccd38f2012d56865a2147f64d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30842268"
---
# <a name="sap-hana-availability-across-azure-regions"></a>Beschikbaarheid van het SAP HANA volledige Azure-regio 's

Dit artikel wordt beschreven scenario's die zijn gerelateerd aan SAP HANA beschikbaarheid over verschillende Azure-regio's. Vanwege de afstand tussen Azure-regio's moet u speciale overwegingen SAP HANA beschikbaarheid instellen in meerdere Azure-regio's.

## <a name="why-deploy-across-multiple-azure-regions"></a>Waarom over meerdere Azure-regio's implementeren

Azure-regio's worden vaak gescheiden door lange afstanden. Afhankelijk van de geopolitieke regio de afstand tussen Azure-regio's mogelijk mijl honderden of zelfs enkele duizenden mijl, zoals in de Verenigde Staten. Vanwege de afstand ervaren netwerkverkeer tussen de assets die zijn geïmplementeerd in twee verschillende Azure-regio's aanzienlijke netwerklatentie voor retour. De latentie is aanzienlijk moeten worden uitgesloten van synchrone gegevensuitwisseling tussen twee SAP HANA-exemplaren typische SAP-werkbelastingen. 

Organisaties hebben aan de andere kant vaak een vereiste voor de afstand tussen de locatie van het primaire datacenter en een secundair datacenter. Een vereiste afstand biedt beschikbaarheid als een natuurlijke ramp in een grotere geografische locatie optreedt. Voorbeelden hiervan zijn de orkanen die de caraïbische en Florida in September en oktober 2017 bereikt. Uw organisatie mogelijk heeft ten minste een vereiste minimale afstand. Voor de meeste Azure-klanten, moet u voor een ontwerp voor beschikbaarheid van de hele met een definitie van de minimale afstand [Azure-regio's](https://azure.microsoft.com/regions/). Omdat de afstand tussen twee Azure-regio's te groot is om de HANA synchrone replicatie gebruikt, vereisten RTO en RPO forceert mogelijk u configuraties beschikbaarheid in een regio te implementeren en vervolgens vullen met aanvullende implementaties in een tweede de regio.

Een ander aspect in overweging moet nemen in dit scenario is een failover en client omleiden. De veronderstelling is dat een failover tussen SAP HANA-exemplaren in twee verschillende Azure-regio's altijd een handmatige failover. Omdat de replicatiemodus van replicatie voor SAP HANA-systeem is ingesteld op asynchrone, is het mogelijk dat gegevens die zijn vastgelegd in het primaire HANA exemplaar nog niet nog met het secundaire HANA-exemplaar gemaakt. Daarom geen automatische failover een optie voor configuraties waarin de replicatie is asynchroon. Zelfs met handmatig beheerde failover, zoals in een oefening failover moet u maatregelen om ervoor te zorgen dat alle vastgelegde gegevens op de primaire kant met het exemplaar van de secundaire gemaakt voordat u handmatig naar de andere Azure-regio overstappen.
 
Virtuele Azure-netwerk maakt gebruik van een ander IP-adresbereik. De IP-adressen zijn in de tweede Azure-regio geïmplementeerd. Dus ofwel moet u de configuratie van de SAP HANA-client te wijzigen of bij voorkeur moet u stappen voor het wijzigen van de naamomzetting maken. Op deze manier worden de clients omgeleid naar de nieuwe secundaire site server-IP-adres. Zie voor meer informatie het artikel SAP [herstel van de Client verbinding na overname](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html).   

## <a name="simple-availability-between-two-azure-regions"></a>Eenvoudige beschikbaarheid tussen twee Azure-regio 's

U kunt niet een willekeurige beschikbaarheidsconfiguratie in plaatsvinden binnen één regio worden geplaatst, maar nog steeds de vraag om de werkbelasting die wordt geleverd als een noodsituatie voordoet. Typische gevallen voor systemen als volgt zijn productieve systemen. Hoewel dat het systeem niet actief voor een halve of zelfs een dag duurzame is, kunt u het systeem niet beschikbaar is 48 uur of langer kan niet toestaan. Als u de instellingen minder dure, een ander systeem die nog minder belangrijk is in de virtuele machine worden uitgevoerd. Het andere systeem fungeert als een doel. U kunt ook het formaat van de virtuele machine in de secundaire regio kleiner en u niet op de gegevens worden geladen. Omdat de failover handmatig is en veel meer stappen op de stack volledige toepassing failover houdt, wordt de extra tijd voor het afsluiten van de virtuele machine, het formaat en start vervolgens opnieuw de virtuele machine is acceptabel.

> [!NOTE]
> Zelfs als u niet vooraf laden van gegevens in de doel-HANA system replicatie gebruikt, moet u minimaal 64 GB aan geheugen. U moet ook voldoende geheugen naast 64 GB de rowstore om gegevens te behouden in het geheugen van de doelinstantie.

![Diagram van twee virtuele machines via twee regio 's](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> In deze configuratie kunt u een RPO niet opgeven = 0, omdat de modus voor de replicatie van uw HANA asynchroon is. Als u wilt bieden een RPO = 0, deze configuratie wordt de configuratie van de keuze niet.

Een kleine wijziging die u kunt in de configuratie is mogelijk om gegevens te configureren als vooraf laden. Echter, vanwege de handmatige aard van failover- en het feit dat toepassingslagen moeten ook verplaatsen naar de tweede regio, kan het niet zinvol vooraf laden van gegevens. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Beschikbaarheid binnen één regio en in verschillende regio's combineren 

Een combinatie van beschikbaarheid binnen en tussen regio's kan worden veroorzaakt door deze factoren:

- Een vereiste voor RPO = 0 binnen een Azure-regio.
- De organisatie is niet bereid of beschikken over de algemene bewerkingen van invloed op een door een primaire natuurlijke ramp die van invloed op een grotere regio. Dit is het geval is bij sommige orkanen die de caraïbische de afgelopen jaren heeft.
- Voorschriften die vraag afstanden tussen primaire en secundaire sites die zijn duidelijk buiten welke Azure beschikbaarheid zones kunnen bieden.

In dergelijke gevallen kunt u welke SAP-aanroepen instellen een [replicatieconfiguratie voor SAP HANA-systeem met meerdere lagen](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) met behulp van HANA system replicatie. De architectuur eruit als volgt:

![Diagram van drie virtuele machines via twee regio 's](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Deze configuratie biedt een RPO = 0, met een laag RTO, de primaire regio. De configuratie biedt ook goede RPO als een verplaatsing naar de tweede regio is betrokken. De RTO tijdstippen in de tweede regio zijn afhankelijk van of gegevens vooraf geladen. Veel klanten voor het gebruik van de virtuele machine in de secundaire regio een testsysteem uitvoeren. Gebruik in dat geval, de gegevens kan niet worden geladen.

> [!NOTE]
> Omdat u **logreplay** Werkingsmodus voor HANA system replicatie gaat van laag 1 aan tier 2 (synchrone replicatie in de primaire regio) van de replicatie tussen laag 2 of 3 (replicatie naar de secundaire site) kan niet in **delta_datashipping** werkingsmodus. Voor meer informatie over bewerkingsmodi en er zijn enkele beperkingen, Zie het artikel SAP [bewerkingsmodi voor SAP HANA system replicatie](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Volgende stappen

Zie voor stapsgewijze instructies over het instellen van deze configuraties in Azure:

- [De replicatie voor SAP HANA-systeem in Azure VM's instellen](sap-hana-high-availability.md)
- [Hoge beschikbaarheid voor SAP HANA met behulp van replicatie van systeem](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
