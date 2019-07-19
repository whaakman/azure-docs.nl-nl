---
title: Selecteer de juiste VM-SKU voor uw Azure Data Explorer-cluster
description: In dit artikel wordt beschreven hoe u de optimale SKU-grootte voor Azure Data Explorer cluster selecteert.
author: avnera
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 0239111ca56dfe431a00eee83c79eedccc66c927
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226153"
---
# <a name="select-the-correct-vm-sku-for-your-azure-data-explorer-cluster"></a>Selecteer de juiste VM-SKU voor uw Azure Data Explorer-cluster 

Azure Data Explorer heeft meerdere VM-Sku's waaruit u kunt kiezen wanneer u een nieuw cluster maakt of het cluster optimaliseert voor een wijzigings werk belasting. De virtuele machines zijn zorgvuldig gekozen om de meest optimale kosten voor elke werk belasting toe te staan. 

De grootte en VM-SKU van het gegevens beheer cluster worden volledig beheerd door de Azure Data Explorer-service. Dit wordt bepaald door factoren zoals de VM-grootte van de engine en de opname werk belasting. 

De VM-SKU voor het engine-cluster kan op elk gewenst moment worden gewijzigd door [het cluster omhoog te schalen](manage-cluster-vertical-scaling.md). Daarom kunt u het beste beginnen met de minimale SKU-grootte die past bij het eerste scenario. Wanneer het cluster wordt geschaald, wordt de uitval tijd van Maxi maal 30 minuten verg Roten, terwijl de nieuwe VM-SKU opnieuw wordt gemaakt.

> [!TIP]
> COMPUTE [RI (gereserveerde instanties)](https://docs.microsoft.com/azure/virtual-machines/windows/prepay-reserved-vm-instances) is van toepassing op Azure Data Explorer cluster.  

In dit artikel worden de verschillende opties voor de VM-SKU beschreven en vindt u technische details die u kunnen helpen bij het maken van een optimale keuze.

## <a name="select-the-cluster-type"></a>Het cluster type selecteren

Azure Data Explorer biedt twee typen clusters:

* **Productie**: Productie clusters bevatten twee knoop punten voor Engine-en gegevens beheer clusters en worden beheerd onder de [Sla](https://azure.microsoft.com/support/legal/sla/data-explorer/v1_0/)van Azure Data Explorer.

* **Dev/test (geen sla)** : Dev/test-clusters hebben één D11_v2-knoop punt voor het engine-cluster en één D1-knoop punt voor het gegevens beheer cluster. Dit cluster type is de laagste kosten configuratie vanwege het aantal lage instanties en er worden geen kosten voor Engine markeringen in rekening gebracht. Er is geen SLA voor deze cluster configuratie, omdat deze geen redundantie heeft.

## <a name="sku-types"></a>SKU-typen

Wanneer u een Azure Data Explorer-cluster maakt, selecteert u de *optimale* VM-SKU voor de geplande werk belasting. Azure Data Explorer heeft twee SKU-families waaruit u kunt kiezen:

* **D_V2**: De **D** SKU is geoptimaliseerd voor reken kracht en is voorzien van twee soorten.
    * VM zelf
    * VM gebundeld met Premium-opslag schijven

* **LS**: De **L** -SKU is geoptimaliseerd voor opslag. Het heeft een veel groter SSD-grootte dan de soort gelijke prijs- **D** -SKU.

De volgende tabel bevat de belangrijkste verschillen tussen de beschik bare SKU-typen:
 
|**Geschreven** | **D SKU** | **L-SKU**
|---|---|---
|**Kleine Sku's**|De minimale grootte is 11 ' met twee kernen|Minimale grootte is ' N4 ' met vier kernen
|**Beschikbaarheid**|Beschikbaar in alle regio's (de DS + PS-versie heeft een meer beperkte Beschik baarheid)|Beschikbaar in een paar regio's
|**Kosten per GB cache per kern**|Hoog met de D-SKU, laag met de DS + PS-versie|Goedkoopste met de optie *betalen per* gebruik
|**Prijzen van RI (gereserveerde instanties)**|Hoge korting (meer dan 55% voor een toezeg ging van drie jaar)|Lagere korting (20% voor een toezeg ging van drie jaar)  

## <a name="select-your-cluster-vm"></a>De virtuele machine van het cluster selecteren 

Als u uw cluster-VM wilt selecteren, [configureert u verticaal schalen](manage-cluster-vertical-scaling.md#configure-vertical-scaling). 

Met de verschillende opties voor de VM-SKU kunt u de kosten voor de benodigde prestatie-en Hot-cache vereisten voor het gewenste scenario optimaliseren. Als het scenario de meest optimale prestaties voor een hoog query volume vereist, moet de ideale SKU worden geoptimaliseerd voor berekening. Als in het scenario daarentegen query's moeten worden doorzocht op grote hoeveel heden gegevens met een relatief lagere query belasting, worden de kosten door de door opslag geoptimaliseerde SKU verminderd en bieden ze uitstekende prestaties.

Het aantal exemplaren per cluster voor de kleine Sku's is beperkt zodat het de voor keur heeft om grotere Vm's te gebruiken met meer RAM-geheugen. De grootte van het RAM-geheugen is vereist voor sommige query typen waarmee meer eisen worden gesteld aan de RAM-bron, `joins`zoals die query's die gebruikmaken van. Daarom wordt bij het overwegen van schaal opties aanbevolen om te schalen naar een grotere SKU dan uitschalen door meer instanties toe te voegen.

## <a name="vm-options"></a>VM-opties

De volgende tabel bevat de technische specificaties voor de Azure Data Explorer-cluster-Vm's:

|**Name**| **Categorie** | **SSD-grootte** | **Kernen** | **NODIG** | **Premium-opslag schijven (1 TB)**| **Minimum aantal exemplaren per cluster** | **Maximum aantal exemplaren per cluster**
|---|---|---|---|---|---|---|---
|D11_v2| geoptimaliseerd voor berekenen | 75 GB    | 2 | 14 GB | 0 | 1 | 8 (behalve voor de SKU dev/test, waar deze 1 is)
|D12_v2| geoptimaliseerd voor berekenen | 150 GB   | 4 | 28 GB | 0 | 2 | 16
|D13_v2| geoptimaliseerd voor berekenen | 307 GB   | 8 | 56 GB | 0 | 2 | 1000
|D14_v2| geoptimaliseerd voor berekenen | 614 GB   | 16| 112 GB | 0 | 2 | 1000
|DS13_v2 + 1 TB PS| opslag geoptimaliseerd | 1 TB | 8 | 56 GB | 1 | 2 | 1000
|DS13_v2 + 2 TB PS| opslag geoptimaliseerd | 2 TB | 8 | 56 GB | 2 | 2 | 1000
|DS14_v2 + 3 TB PS| opslag geoptimaliseerd | 3 TB | 16 | 112 GB | 2 | 2 | 1000
|DS14_v2 + 4 TB PS| opslag geoptimaliseerd | 4 TB | 16 | 112 GB | 4 | 2 | 1000
|L4s_v1| opslag geoptimaliseerd | 650 GB | 4 | 32 GB | 0 | 2 | 16
|L8s_v1| opslag geoptimaliseerd | 1,3 TB | 8 | 64 GB | 0 | 2 | 1000
|L16s_1| opslag geoptimaliseerd | 2,6 TB | 16| 128 GB | 0 | 2 | 1000

* Bekijk de bijgewerkte SKU-lijst met VM'S per regio met behulp van de Azure Data Explorer [LISTSKUS API](/dotnet/api/microsoft.azure.management.kusto.clustersoperationsextensions.listskus?view=azure-dotnet). 
* Meer informatie over de [verschillende Compute-sku's](/azure/virtual-machines/windows/sizes-compute). 

## <a name="next-steps"></a>Volgende stappen

* Het engine-cluster kan op elk gewenst moment [omhoog of omlaag](manage-cluster-vertical-scaling.md) worden geschaald door de VM-SKU te wijzigen voor verschillende behoeften. 

* De grootte van het engine-cluster kan worden geschaald [in en uit](manage-cluster-horizontal-scaling.md) om capaciteit te wijzigen met veranderende vereisten.

