---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7c7671578dc22926dabfe7735038186ab1c2c2b3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264282"
---
# <a name="standard-ssd-managed-disks-for-azure-virtual-machine-workloads"></a>Standard-SSD Managed Disks voor Azure Virtual machine-werkbelasting

Azure Standard-schijven SSD (Solid State) Managed Disks is een goedkope opslagoptie geoptimaliseerd voor workloads die consistente prestaties op lagere niveaus van IOPS moeten. Standard-SSD biedt een goede op instapniveau voor diegenen die u verplaatsen naar de cloud, wilt met name als u problemen ondervindt met de variantie van workloads die on-premises worden uitgevoerd op uw harde schijf-oplossingen. Standard-SSD's bieden betere beschikbaarheid, consistentie, betrouwbaarheid en latentie in vergelijking met harde schijven, en zijn geschikt voor webservers, toepassingsservers met lage IOPS, weinig wordt gebruikt zakelijke toepassingen en werkbelastingen voor ontwikkelen/testen.

## <a name="standard-ssd-features"></a>Standard-SSD-functies

**Beheerde schijven**: standaard SSD's zijn alleen beschikbaar als beheerde schijven. Niet-beheerde schijven en pagina-Blobs worden niet ondersteund op Standard-SSD. Tijdens het maken van de beheerde schijf, geeft u het schijftype als Standard-SSD en de grootte van de schijf die u nodig hebt en Azure maakt en beheert de schijf voor u.
Standard-SSD's ondersteunen alle classic deployment model bewerkingen die worden aangeboden door beheerde schijven. Bijvoorbeeld, u kunt maken, kopieert of momentopname van Standard-SSD Managed Disks op dezelfde manier als u doen met Managed Disks.

**Virtuele Machines**: standaard SSD's kunnen worden gebruikt met alle Azure-VM's, met inbegrip van de VM-typen die geen ondersteuning voor Premium-schijven. Bijvoorbeeld, als u een A-serie VM, of de VM uit de N-serie of de DS-serie, of elke andere virtuele Azure-machine-serie, kunt u standaard SSD's aan die virtuele machine. Met de introductie van Standard-SSD, zijn we een breed scala aan werkbelastingen die u eerder hebt gebruikt op basis van HDD-schijven voor de overgang naar op basis van SSD-schijven en consistente prestaties, hogere beschikbaarheid, betere latentie en een betere algehele ervaring inschakelen ervaring die afkomstig zijn met SSD's.

**Duurzaam en hoge beschikbaarheid**: standaard SSD's zijn gebouwd op de dezelfde Azure Disks-platform, die consistent is geleverd voor hoge beschikbaarheid en duurzaamheid voor schijven. Azure-schijven zijn ontworpen voor 99,999 procent beschikbaarheid. Net als alle Managed Disks voorzien Standard-SSD's ook van lokaal redundante opslag (LRS). Met LRS, het platform meerdere replica's van gegevens voor elke schijf onderhoudt en zakelijke consistent heeft geleverd duurzaamheid voor IaaS-schijven, met een toonaangevende nul procent op jaarbasis foutpercentage.

**Momentopnamen**: net als alle Managed Disks, Standard-SSD's ook ondersteuning voor het maken van momentopnamen. Type momentopname kan worden standaard (HDD) of Premium (SSD). Voor het opslaan van de kosten, wordt aangeraden type momentopname van de standaard (HDD) voor alle Azure-schijftypes. Dit komt omdat wanneer u een beheerde schijf op basis van een momentopname maken, altijd kunt u een hogere laag, zoals de Standard-SSD- of Premium SSD kiezen.

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen

De volgende tabel bevat de schijfgrootten, die momenteel worden aangeboden voor Standard-SSD. Grootte die is gemarkeerd met een sterretje zijn momenteel in preview.

|Type van de Standard-SSD-schijf  |Schijfgrootte  |IOP's per schijf  |Doorvoer per schijf  |
|---------|---------|---------|---------|
|E10     |128 GiB         |Maximaal 500         |Maximaal 60 MiB per seconde         |
|E15     |256 GiB         |Maximaal 500         |Maximaal 60 MiB per seconde         |
|E20     |512 GiB         |Maximaal 500         |Maximaal 60 MiB per seconde         |
|E30     |1.024 giB       |Maximaal 500         |Maximaal 60 MiB per seconde         |
|E40     |2048 giB       |Maximaal 500         |Maximaal 60 MiB per seconde         |
|E50     |4095 giB       |Maximaal 500         |Maximaal 60 MiB per seconde         |
|E60 *     |8192 giB       |Maximaal 1.300       |Maximaal 300 MiB per seconde        |
|E70 *    |16.384 giB      |Maximaal 2000       |Maximaal 500 MiB per seconde        |
|E80 *    |32.767 giB      |Maximaal 2000       |Maximaal 500 MiB per seconde        |

Standard-SSD's zijn ontworpen om latentie in milliseconden voor de meeste i/o-bewerkingen, en om u te bieden de IOPS en doorvoer van maximaal de limieten die worden beschreven in de bovenstaande tabel. Werkelijke IOPS en doorvoer kunnen soms variëren, afhankelijk van de patronen in het netwerkverkeer. Standard-SSD's biedt consistente prestaties dan de HDD-schijven met een lagere latentie.

Premium SSD's uitvoeren aan de andere kant beter dan standaard SSD met lage latentie, hoge IOPS/doorvoer en nog beter consistentie met de prestaties van de ingerichte schijf. Het is het aanbevolen schijftype voor kritieke productieworkloads. Als uw workload schijfondersteuning met hoge prestaties en lage latentie, moet u overwegen Premium-opslag.

Standard-SSD's, zoals de Premium-SSD's, ook i/o-grootte van 256 KiB gebruiken. Als de gegevens worden overgebracht minder dan 256 KiB is, wordt deze beschouwd als 1 i/o-eenheid. I/o-grotere worden geteld als meerdere i/o's van de grootte van 256 KiB. Bijvoorbeeld, wordt een 1100 KiB i/o geteld als vijf i/o-eenheden.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u de Standard-SSD's, zijn de volgende factureringsvoorwaarden van toepassing:

- Beheerde schijfgrootte
- Momentopnamen
- Uitgaande gegevensoverdracht
- Transacties

**Beheerde schijfgrootte**: beheerde schijven worden in rekening gebracht voor de ingerichte grootte. Azure wijst de ingerichte grootte (omhoog afgerond) toe aan de aanbieding voor schrijfgrootte die het dichtst in de buurt komt. Zie de tabel in Scalability and Performance Targets sectie hierboven voor meer informatie van de grootte van de schijven die worden aangeboden. Elke schijf wordt toegewezen aan een grootte van de ondersteunde ingerichte schijf en dienovereenkomstig kosten in rekening gebracht. Bijvoorbeeld, als u een 200 GiB Standard-SSD ingericht, deze wordt toegewezen aan de aanbieding van de grootte van schijf van E15 (256 GB). Facturering voor ingerichte schijven is per uur Pro rata met behulp van de maandelijkse prijs voor de Premium-opslag-aanbieding. Bijvoorbeeld, als u een schijf E10 ingericht en deze na 20 uur verwijderd, wordt u gefactureerd voor de aanbieding E10 20 uur Pro rata berekend. Dit is, ongeacht de hoeveelheid feitelijke gegevens geschreven naar de schijf.

**Momentopnamen**: momentopnamen van Managed Disks worden kosten in rekening gebracht voor de capaciteit die wordt gebruikt door de momentopnamen, op het doel en op de bron, indien van toepassing. Zie voor meer informatie over momentopnamen [momentopnamen van de beheerde schijf](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#managed-disk-snapshots).

**Uitgaande gegevensoverdracht**: [uitgaande gegevensoverdrachten](https://azure.microsoft.com/pricing/details/bandwidth/) (gegevens die vanuit Azure-datacenters) worden gefactureerd voor bandbreedtegebruik.

**Transacties**: net als bij standaard harde schijven, transacties voor Standard-SSD's worden gefactureerd. Transacties bestaan zowel uit lees- en schrijfbewerkingen op de schijf. Grootte van i/o die wordt gebruikt voor de transacties voor Standard-SSD accounting is 256 KiB. I/o-grotere worden geteld als meerdere i/o's van de grootte van 256 KiB.

Zie voor meer informatie over prijzen voor virtuele Machines en beheerde schijven:

- [Prijzen voor Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)
- [Prijzen van Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie over het maken van de standard-SSD's in het voorbeeld dat laat zien hoe u een virtuele machine maken met meerdere standard-SSD's.

> [!div class="nextstepaction"]
> [Een virtuele machine maken met meerdere standard-SSD 's](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)