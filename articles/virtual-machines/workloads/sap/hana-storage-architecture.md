---
title: Storage-architectuur van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Storage-architectuur van SAP HANA op Azure (grote instanties) implementeren.
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
ms.date: 11/20/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e692cc1fd8670cc14b42e4714d84356d4d4c53a2
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275987"
---
# <a name="sap-hana-large-instances-storage-architecture"></a>Architectuur voor SAP HANA (grote instanties)

De opslagindeling voor SAP HANA op Azure (grote instanties) is geconfigureerd door SAP HANA op het klassieke implementatiemodel per SAP aanbevolen richtlijnen. De richtlijnen worden beschreven in de [opslagvereisten voor SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) technisch document.

De HANA grote instantie van het Type ik klasse wordt geleverd met vier keer het geheugen volume als opslagvolume. De opslag is niet voor de klasse Type II van HANA grote instantie eenheden, vier keer meer. De eenheden worden geleverd met een volume dat is bedoeld voor het opslaan van HANA transactielogboekback-ups. Zie voor meer informatie, [installeren en configureren van SAP HANA (grote instanties) op Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie de volgende tabel in termen van Opslagtoewijzing. De tabel bevat de ruwe capaciteit voor de verschillende volumes die zijn opgegeven met de verschillende HANA grote instantie-eenheden.

| HANA grote instantie SKU | Hana/gegevens | Hana/log | Hana/gedeeld | Hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S96 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S192 | 4.608 GB | 1.024 GB | 1536 GB | 1.024 GB |
| S192m | 11,520 GB | 1536 GB | 1.792 GB | 1536 GB |
| S192xm |  11,520 GB |  1536 GB |  1.792 GB |  1536 GB |
| S384 | 11,520 GB | 1536 GB | 1.792 GB | 1536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2040 GB |
| S384xm | 16.000 GB | 2.050 GB | 2.050 GB | 2040 GB |
| S384xxm |  20.000 GB | 3100 GB | 2.050 GB | 3100 GB |
| S576m | 20.000 GB | 3100 GB | 2.050 GB | 3100 GB |
| S576xm | 31.744 GB | 4096 GB | 2.048 GB | 4096 GB |
| S768m | 28.000 GB | 3100 GB | 2.050 GB | 3100 GB |
| S768xm | 40.960 GB | 6.144 GB | 4096 GB | 6.144 GB |
| S960m | 36.000 GB | 4,100 GB | 2.050 GB | 4,100 GB |


Werkelijke geïmplementeerde volumes kunnen verschillen op basis van de implementatie en het hulpprogramma dat wordt gebruikt om de volumes groter weer te geven.

Als u een HANA grote instantie SKU onderverdelen, enkele voorbeelden van mogelijke deling onderdelen als volgt uitzien:

| Geheugenpartitie in GB | Hana/gegevens | Hana/log | Hana/gedeeld | Hana/log/back-up |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1.792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Deze grootten zijn ruwe volume getallen die kan verschillen enigszins op basis van de implementatie en de hulpprogramma's gebruikt om te kijken naar de volumes. Er zijn ook de grootte van andere partities, zoals 2,5 TB. Deze opslagruimten worden berekend met een formule die vergelijkbaar is met de gebruikt voor de vorige partities. De term 'partities' betekent niet dat het besturingssysteem, geheugen of CPU-resources worden op geen enkele manier gepartitioneerd. Opslag-partities voor de verschillende HANA-instanties die u mogelijk wilt implementeren op één HANA grote instantie eenheid geeft u aan. 

U kunt meer opslag nodig hebt. U kunt opslag toevoegen door het aanschaffen van extra opslagruimte in eenheden van 1 TB. Deze extra opslag kan worden toegevoegd als een extra volume. Er kan ook worden gebruikt om uit te breiden van een of meer van de bestaande volumes. Het is niet mogelijk om te verkleinen van de grootte van de volumes zoals oorspronkelijk geïmplementeerd en voornamelijk door de vorige tabellen worden beschreven. Is het ook niet mogelijk om te wijzigen van de namen van de volumes of koppelen van namen. De opslagvolumes die eerder is beschreven, zijn gekoppeld aan de eenheden HANA grote instantie als NFS4 volumes.

U kunt storage-momentopnamen gebruiken voor back-up en herstel en disaster recovery-doeleinden. Zie voor meer informatie, [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor lay-out opslaggegevens voor uw scenario.

## <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Meerdere exemplaren van de SAP HANA uitvoeren op één HANA grote instantie-eenheid

Het is mogelijk om meer dan één actieve SAP HANA-instantie op HANA grote instantie eenheden te hosten. Een dergelijke configuratie vereist om de mogelijkheden van opslagmomentopnamen en herstel na noodgevallen, een volume per exemplaar instellen. Op dit moment kunnen HANA grote instantie eenheden als volgt worden onderverdeeld:

- **S72, S72m, S96, S144, S192**: In stappen van 256 GB, 256 GB wordt de kleinste vanaf eenheid. Verschillende stappen zoals 256 GB en 512 GB kunnen worden gecombineerd met het maximum van het geheugen van de eenheid.
- **S144m en S192m**: In stappen van 256 GB, 512 GB wordt de kleinste eenheid. Verschillende stappen zoals 512 GB en biedt 768 GB kunnen worden gecombineerd met het maximum van het geheugen van de eenheid.
- **Typ II klasse**: In stappen van 512 GB, waarbij de kleinste vanaf eenheid van 2 TB. Verschillende stappen zoals 512 GB, 1 TB en 1,5 TB kunnen worden gecombineerd met het maximum van het geheugen van de eenheid.

Enkele voorbeelden van het uitvoeren van meerdere SAP HANA-instanties ziet er als volgt uit.

| SKU | Geheugengrootte | Opslaggrootte | Grootten met meerdere databases |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA-instantie<br /> of 1 x 512 GB exemplaar + 1 x 256 GB-exemplaar<br /> of exemplaren van 3 x 256 GB | 
| S72m | 1,5 TB | 6 TB OPSLAGRUIMTE | 3x512GB HANA-instanties<br />of 1 x 512 GB exemplaar + 1 x 1 TB exemplaar<br />of 6 x 256 GB-exemplaren<br />of het exemplaar 1x1.5 TB | 
| S192m | 4 TB | 16 TB | 8 x 512 GB-instanties<br />of exemplaren van 4 x 1 TB<br />of exemplaren van 4 x 512 GB + 2 x 1 TB exemplaren<br />of exemplaren van 4 x 768 GB + 2 x 512 GB-instanties<br />of het exemplaar van 1 x 4 TB |
| S384xm | 8 TB | 22 TB | exemplaren van 4 x 2 TB<br />of 2 x 4 TB-exemplaren<br />of exemplaren van 2 x 3 TB + 1 x 2 TB exemplaren<br />of instanties 2x2.5 TB + 1 x 3 TB exemplaren<br />of het exemplaar van 1 x 8 TB |


Er zijn ook andere verschillen. 

## <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest
De opslag die wordt gebruikt voor HANA grote instantie kunt een transparante versleuteling van de gegevens zoals deze wordt opgeslagen op de schijven. Als een eenheid HANA grote instantie is geïmplementeerd, kunt u dit type versleuteling inschakelen. U kunt ook wijzigen met versleutelde volumes na de implementatie plaatsvindt. De overstap van niet-versleutelde versleutelde volumes is transparant en vereist geen downtime. 

Met het Type ik klasse van SKU's, het volume de LUN wordt opgeslagen, worden opnieuw opgestart, worden versleuteld. Voor het Type II-klasse van SKU's van HANA grote instantie moet u voor het versleutelen van het bestand Boot.ini LUN met besturingssysteem-methoden. Voor meer informatie contact op met het Microsoft-Service Management-team.

**Volgende stappen**
- Raadpleeg [scenario's ondersteund voor HANA grote instanties](hana-supported-scenario.md)