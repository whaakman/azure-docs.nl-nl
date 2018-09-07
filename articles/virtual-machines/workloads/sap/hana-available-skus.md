---
title: SKU's voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: SKU's voor SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7ed7291333e7baa764b09e66aa5cfaedc77072fb
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028113"
---
# <a name="available-skus-for-hli"></a>Beschikbare SKU's voor HLI

SAP HANA op Azure (grote instanties)-service is beschikbaar in verschillende configuraties in de Azure-regio's VS West en VS-Oost, Australië-Oost, Australië-Zuidoost, West-Europa, Noord-Europa, Japan-Oost en Japan-West.

[SKU's van HANA grote instanties van SAP HANA-gecertificeerde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) lijst, zoals:

| SAP-oplossing | CPU | Geheugen | Storage | Beschikbaarheid |
| --- | --- | --- | --- | --- |
| Geoptimaliseerd voor OLAP: SAP BW, BW/4HANA<br /> of SAP HANA voor werkbelastingen van algemene OLAP-systemen | SAP HANA op Azure S72<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU-kernen en 72 CPU-threads |  768 GB |  3 TB | Beschikbaar |
| --- | SAP HANA op Azure S144<br /> -4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU-kernen en threads 144 CPU |  1,5 TB |  6 TB OPSLAGRUIMTE | Niet beschikbaar meer |
| --- | SAP HANA op Azure S192<br /> -4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en threads 192 CPU |  2.0 TB |  8 TB | Beschikbaar |
| --- | SAP HANA op Azure S384<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  4.0 TB |  16 TB | Beschikbaar |
| Geoptimaliseerd voor OLTP: SAP Business Suite<br /> op SAP HANA of S/4HANA (OLTP)<br /> algemene OLTP-systemen | SAP HANA op Azure S72m<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU-kernen en 72 CPU-threads |  1,5 TB |  6 TB OPSLAGRUIMTE | Beschikbaar |
|---| SAP HANA op Azure S144m<br /> -4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU-kernen en threads 144 CPU |  3.0 TB |  12 TB | Niet beschikbaar meer |
|---| SAP HANA op Azure S192m<br /> -4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en threads 192 CPU  |  4.0 TB |  16 TB | Beschikbaar |
|---| SAP HANA op Azure S384m<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  6.0 TB |  18 TB | Beschikbaar |
|---| SAP HANA op Azure S384xm<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  8.0 TB |  22 TB |  Beschikbaar |
|---| SAP HANA op Azure S576m<br /> tot en met 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU-kernen en 576 CPU-threads |  12.0 TB |  28 TB | Beschikbaar |
|---| SAP HANA op Azure S768m<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-kernen en threads 768 CPU |  16,0 TB |  36 TB | Beschikbaar |
|---| SAP HANA op Azure S960m<br /> -20 x Intel® Xeon® Processor E7-8890 v4<br /> 480 CPU-kernen en threads 960 CPU |  20.0 TB |  46 TB | Beschikbaar |


Onder TDIv5 voor SAP HANA kunt SAP klantspecifieke grootte en klantspecifieke projecten die tot de server-configuraties die niet zijn opgenomen leiden mogelijk gecertificeerde:

- [SAP HANA-gecertificeerde apparaten](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [SAP HANA-gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

In veel gevallen voert u deze serverconfiguraties klantspecifieke meer geheugen dan de server-eenheden met SAP gecertificeerde. Klanten hoeven bij het werken met SAP, de mogelijkheid voor het ophalen van SAP-ondersteuning en certificeren voor hun klant-specifieke grootte server-configuraties. In Azure worden de volgende HANA grote instantie standaard SKU's zijn beschikbaar en in de Microsoft prijslijst voor dergelijke TDIv5 klantspecifieke sizing-projecten.


| Oorspronkelijke SKU die kan worden <br /> uitgebreid in het geheugen | CPU | Geheugen | Storage | Beschikbaarheid |
| --- | --- | --- | --- | --- |
| S192m kan worden uitgebreid naar | SAP HANA op Azure S192xm<br /> -4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU-kernen en threads 192 CPU |  6.0 TB |  16 TB | Beschikbaar |
| S384xm kan worden uitgebreid naar | SAP HANA op Azure S384xxm<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU-kernen en threads 384 CPU |  12.0 TB |  28 TB | Beschikbaar |
| S576m kan worden uitgebreid naar | SAP HANA op Azure S576xm<br /> tot en met 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU-kernen en 576 CPU-threads |  18.0 TB |  41 TB | Beschikbaar |
| S768m kan worden uitgebreid naar | SAP HANA op Azure S768xm<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU-kernen en threads 768 CPU |  24.0 TB |  56 TB | Beschikbaar |

- CPU-kernen = de som van niet-hyper-threaded CPU-kernen van de som van de processors van de server-eenheid.
- CPU-threads = de som van de compute-threads geleverd door hyper-threaded CPU-kernen van de som van de processors van de server-eenheid. De meeste eenheden zijn standaard geconfigureerd voor Hyper-Threading-technologie gebruiken.
- Op basis van de leverancier aanbevelingen S768m, zijn S768xm en S960m niet geconfigureerd voor het gebruik van Hyper-Threading voor het uitvoeren van SAP HANA.


De specifieke configuraties die is gekozen, zijn afhankelijk van de werkbelasting, CPU-resources en gewenste geheugen. Het is mogelijk dat de OLTP-werkbelasting met de SKU's die zijn geoptimaliseerd voor de OLAP-werkbelasting. 

De basis voor de aanbiedingen, met uitzondering van eenheden voor grootte van de klant-specifieke projecten, hardware zijn SAP HANA TDI-gecertificeerde. Twee verschillende soorten hardware deelt de SKU's in:

- S72, S72m S144, S144m, S192, S192m en S192xm, die worden aangeduid als "Type ik klasse" van SKU's.
- S384, S384m, S384xm, S384xxm, S576m, S576xm S768m, S768xm en S960m, die worden aangeduid als de "Type II-klasse" van SKU's.

Een volledige HANA grote instantie stempel is niet uitsluitend voor één klant toegewezen&#39;s gebruiken. Dit geldt voor de rekken van reken- en resources die zijn verbonden via een netwerk-fabric ook geïmplementeerd in Azure. HANA grote instantie infrastructuur, zoals Azure, verschillende klanten implementeert &quot;tenants&quot; die zijn geïsoleerd van elkaar in de volgende drie niveaus:

- **Netwerk**: isolatie door middel van virtuele netwerken in de stempel HANA grote instantie.
- **Opslag**: isolatie door middel van opslag virtuele machines met opslagvolumes die zijn toegewezen en isoleren opslagvolumes tussen tenants.
- **COMPUTE**: toewijzing van server-eenheden toegewezen aan één tenant. Geen vaste of zachte partitioneren van eenheden van de server. Er is geen delen van één server of de host eenheid tussen tenants. 

De implementaties van HANA grote instantie eenheden tussen verschillende tenants zijn niet zichtbaar zijn voor elkaar. HANA grote instantie eenheden die zijn geïmplementeerd in verschillende tenants kunnen niet communiceren rechtstreeks met elkaar op het niveau van de stempel HANA grote instantie. Alleen HANA grote instantie eenheden binnen één tenant kunnen met elkaar communiceren op het niveau van de stempel HANA grote instantie.

Een geïmplementeerde tenant in de stempel grote instantie wordt toegewezen aan één Azure-abonnement voor factureringsdoeleinden. Voor een netwerk, kan deze worden geopend van virtuele netwerken van andere Azure-abonnementen binnen dezelfde Azure-inschrijving. Als u met een andere Azure-abonnement in dezelfde Azure-regio implementeert, kunt ook u vragen om een gescheiden HANA grote instantie-tenant.

Er zijn belangrijke verschillen tussen het uitvoeren van SAP HANA op HANA grote instantie en SAP HANA uitvoeren op virtuele machines die zijn geïmplementeerd in Azure:

- Er is geen virtualisatielaag voor SAP HANA op Azure (grote instanties). Krijgt u de prestaties van de onderliggende hardware van bare-metal.
- In tegenstelling tot Azure, is de SAP HANA op Azure (grote instanties)-server worden toegewezen aan een specifieke klant. Er is geen mogelijkheid is dat een eenheid van de server of de host is hard of zachte gepartitioneerd. Als gevolg hiervan wordt een eenheid HANA grote instantie gebruikt als geheel naar een tenant en die aan u toegewezen. Opnieuw opstarten of afsluiten van de server leiden niet automatisch tot het besturingssysteem en de SAP HANA wordt geïmplementeerd op een andere server. (Voor het Type ik klasse SKU's, de enige uitzondering hierop is als een server problemen detecteert en opnieuw implementeren moet worden uitgevoerd op een andere server.)
- In tegenstelling tot Azure, waarbij host processortypen zijn geselecteerd voor de beste prijs-prestatieverhouding-verhouding, zijn de processortypen voor SAP HANA op Azure (grote instanties) hebt gekozen, de hoogste prestaties van de Intel E7v3 en E7v4 processor-regel.

**Volgende stappen**
- Raadpleeg [HLI grootte](hana-sizing.md)
