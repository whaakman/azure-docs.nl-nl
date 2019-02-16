---
title: De opslagconfiguratie voor SQL Server-VM's | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe Azure storage configureren voor SQL Server-VM's tijdens het inrichten (Resource Manager-implementatiemodel). Hierin wordt ook uitgelegd hoe u opslag kunt configureren voor uw bestaande SQL Server-VM's.
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: craigg
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/05/2017
ms.author: ninarn
ms.openlocfilehash: da850b8ff9174fa310c5247cd7e99af69db28a8b
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328432"
---
# <a name="storage-configuration-for-sql-server-vms"></a>De opslagconfiguratie voor SQL Server-VM 's

Wanneer u een virtuele machine-installatiekopie van SQL Server in Azure configureert, kunt u de Portal de opslagconfiguratie automatiseren. Dit omvat het toevoegen van opslag met de VM, dat toegankelijk is voor SQL Server maken en deze configureren om te optimaliseren voor uw specifieke vereisten.

In dit onderwerp wordt uitgelegd hoe configureert Azure storage voor uw SQL Server-VM's tijdens het inrichten van zowel voor bestaande VM's. Deze configuratie is gebaseerd op de [aanbevolen procedures voor prestaties](virtual-machines-windows-sql-performance.md) voor virtuele Azure-machines waarop SQL Server wordt uitgevoerd.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de configuratie-instellingen voor automatische opslag, is uw virtuele machine vereist de volgende kenmerken:

* Ingericht met een [installatiekopie voor SQL Server-galerie](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).
* Maakt gebruik van de [Resource Manager-implementatiemodel](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Maakt gebruik van [premium SSD's](../disks-types.md).

## <a name="new-vms"></a>Nieuwe virtuele machines

De volgende secties wordt beschreven hoe u opslag voor nieuwe virtuele machines van de SQL Server configureren.

### <a name="azure-portal"></a>Azure-portal

Bij het inrichten van een Azure-VM met behulp van een installatiekopie van de galerie met SQL Server, kunt u de opslag automatisch te configureren voor uw nieuwe virtuele machine. U geeft de maximale grootte, prestatielimieten en type werkbelasting. De volgende schermafbeelding ziet u de configuratieblade van de opslag die wordt gebruikt tijdens de SQL-VM inrichten.

![Configuratie van SQL Server VM-opslag tijdens het inrichten](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Azure voert op basis van uw keuze, de volgende configuratietaken voor de opslag na het maken van de virtuele machine:

* Hiermee maakt en koppelt premium SSD's aan de virtuele machine.
* Hiermee configureert u de gegevensschijven zodat ze toegankelijk zijn met SQL Server.
* Hiermee configureert u de gegevensschijven in een opslaggroep op basis van de opgegeven grootte en prestaties (IOPS en doorvoer).
* Koppelt de opslaggroep aan een nieuw station op de virtuele machine.
* Optimaliseert deze nieuwe schijf op basis van het type van de opgegeven werkbelasting (datawarehousing, transactionele verwerking of algemeen).

Zie voor meer informatie over hoe Azure storage-instellingen configureert, de [opslag configuratiesectie](#storage-configuration). Zie voor een overzicht van het maken van een SQL Server-VM in Azure Portal, [de zelfstudie voor inrichting](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Resource beheren-sjablonen

Als u de volgende Resource Manager-sjablonen gebruikt, worden twee schijven voor premium-gegevens met geen configuratie van opslaggroep wordt standaard gekoppeld. U kunt echter aanpassen dat deze sjablonen om het aantal premium-gegevensschijven die zijn gekoppeld aan de virtuele machine te wijzigen.

* [Virtuele machine met automatische back-up maken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Virtuele machine maken met automatische patches](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Virtuele machine maken met Azure Sleutelkluis-integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Bestaande VM 's

Voor bestaande SQL Server-VM's, kunt u bepaalde Opslaginstellingen voor in Azure portal. Selecteer de virtuele machine, gaat u naar het gebied Instellingen en selecteer vervolgens SQL Server-configuratie. De blade SQL Server-configuratie wordt het huidige opslaggebruik van uw virtuele machine. Alle schijven die aanwezig zijn op de virtuele machine worden weergegeven in deze grafiek. Voor elk station wordt de opslagruimte in vier secties:

* SQL-gegevens
* SQL-logboek
* Overige (niet-SQL-opslag)
* Beschikbaar

![Opslag configureren voor bestaande SQL Server-machine](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Voor het configureren van de opslag voor het toevoegen van een nieuw station of een bestaand station uitbreiden, klikt u op de koppeling bewerken boven de grafiek.

De configuratieopties die u ziet, is afhankelijk van of u hebt deze functie voordat gebruikt. Wanneer u voor de eerste keer gebruikt, kunt u uw opslagvereisten voor een nieuw station opgeven. Als u deze functie eerder hebt gebruikt om een station te maken, kunt u de opslag van dat station uitbreiden.

### <a name="use-for-the-first-time"></a>Gebruik voor de eerste keer

Als de eerste keer dat u deze functie is, kunt u de opslaglimieten voor grootte en prestaties voor een nieuw station opgeven. Deze ervaring is vergelijkbaar met wat u bij het inrichten van de tijd zou zien. Het belangrijkste verschil is dat u niet toegestaan om op te geven van het type werkbelasting. Deze beperking voorkomt dat onderbreken van een bestaande SQL Server-configuraties op de virtuele machine.

![SQL Server-opslag schuifregelaars configureren](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure maakt een nieuw station op basis van uw specificaties. In dit scenario voert Azure uit de volgende configuratietaken voor opslag:

* Hiermee maakt en koppelt premium-opslagschijven gegevens aan de virtuele machine.
* Hiermee configureert u de gegevensschijven zodat ze toegankelijk zijn met SQL Server.
* Hiermee configureert u de gegevensschijven in een opslaggroep op basis van de opgegeven grootte en prestaties (IOPS en doorvoer).
* Koppelt de opslaggroep aan een nieuw station op de virtuele machine.

Zie voor meer informatie over hoe Azure storage-instellingen configureert, de [opslag configuratiesectie](#storage-configuration).

### <a name="add-a-new-drive"></a>Een nieuw station toevoegen

Als u opslag al hebt geconfigureerd op de SQL Server-VM, biedt uitbreiden van opslag nu twee nieuwe opties. De eerste optie is het toevoegen van een nieuwe schijf, waarvoor het prestatieniveau van uw virtuele machine kunt verhogen.

![Een nieuw station toevoegen aan een SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Na het toevoegen van het station, moet u enkele extra handmatige configuratie voor het bereiken van de toename van de prestaties echter uitvoeren.

### <a name="extend-the-drive"></a>Het station uitbreiden

De andere optie voor het uitbreiden van opslag is om uit te breiden het bestaande station. Deze optie verhoogt de beschikbare opslag voor de schijf, maar de prestaties niet wordt verhoogd. Met opslaggroepen, u kunt het aantal kolommen niet wijzigen nadat de opslaggroep wordt gemaakt. Het aantal kolommen, bepaalt het aantal parallelle schrijfbewerkingen, die worden striped op verschillende schijven opgeslagen kan. Eventuele extra gegevensschijven kunnen daarom niet prestaties verhogen. Ze kunnen alleen meer opslag opgeven voor de gegevens die worden geschreven. Deze beperking betekent ook dat bij het uitbreiden van het station, het aantal kolommen bepaalt u het minimum aantal gegevensschijven die u kunt toevoegen. Dus als u een opslaggroep met vier gegevensschijven maakt, is het aantal kolommen ook vier. Elk gewenst moment uitbreiden van de opslag, moet u ten minste vier gegevensschijven toevoegen.

![Een station uitbreiden voor een SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Opslagconfiguratie

Deze sectie bevat een verwijzing voor de opslag-configuratiewijzigingen die Azure automatisch uitgevoerd tijdens het inrichten van SQL-VM of configuratie in de Azure-Portal.

* Als u minder dan twee TB aan opslag hebt geselecteerd voor uw virtuele machine, maakt Azure een opslaggroep geen.
* Als u ten minste twee TB aan opslag voor uw virtuele machine hebt geselecteerd, configureert Azure een opslaggroep. De volgende sectie van dit onderwerp bevat de details van de configuratie van opslaggroep.
* Maakt gebruik van automatische opslagconfiguratie altijd [premium SSD's](../disks-types.md) P30-gegevensschijven. Als gevolg hiervan is er een 1:1 toewijzing tussen het geselecteerde aantal Terabytes en het aantal gegevensschijven die zijn gekoppeld aan uw virtuele machine.

Zie voor informatie over de prijzen, de [prijzen voor Storage](https://azure.microsoft.com/pricing/details/storage) pagina op de **schijfopslag** tabblad.

### <a name="creation-of-the-storage-pool"></a>Het maken van de opslaggroep

Azure maakt gebruik van de volgende instellingen om te maken van de opslaggroep op SQL Server-VM's.

| Instelling | Value |
| --- | --- |
| Streepgrootte |256 KB (datawarehousing); 64 KB (transactioneel) |
| Schijfformaten |1 TB |
| Cache |Lezen |
| Grootte van bestandssysteemtoewijzing |De clustergrootte van 64 KB NTFS |
| Initialisatie van de directe bestanden |Ingeschakeld |
| Pagina's in het geheugen vergrendelen |Ingeschakeld |
| Herstel |Eenvoudig herstel (geen tolerantie) |
| Aantal kolommen |Aantal gegevensschijven<sup>1</sup> |
| TempDB-locatie |Opgeslagen op gegevensschijven<sup>2</sup> |

<sup>1</sup> nadat de opslaggroep is gemaakt, kunt u het aantal kolommen in de opslaggroep niet wijzigen.

<sup>2</sup> deze instelling is alleen van toepassing op het eerste station dat u maakt met de functie voor het configureren van opslag.

## <a name="workload-optimization-settings"></a>Workload optimalisatie-instellingen

De volgende tabel beschrijft de drie werkbelasting opties beschikbaar en hun bijbehorende optimalisatie:

| Type werkbelasting | Description | Optimalisaties |
| --- | --- | --- |
| **Algemeen** |Standaardinstelling die ondersteuning biedt voor de meeste workloads |Geen |
| **Transactieverwerking** |Wordt de opslag voor OLTP-workloads van traditionele databases geoptimaliseerd |Traceringsvlag 1117<br/>Traceringsvlag 1118 |
| **Gegevensopslag** |Optimaliseert de opslag voor analyse- en rapportageworkloads |Trace Flag 610<br/>Traceringsvlag 1117 |

> [!NOTE]
> U kunt alleen het type werkbelasting opgeven wanneer u een virtuele SQL-machine inrichten door deze te selecteren in de stap voor het configureren van opslag.

## <a name="next-steps"></a>Volgende stappen

Zie voor andere onderwerpen met betrekking tot SQL Server wordt uitgevoerd in Azure VM's, [SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
