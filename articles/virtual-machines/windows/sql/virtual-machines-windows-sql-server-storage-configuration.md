---
title: Configuratie van de opslag voor virtuele machines van SQL Server | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe Azure storage voor VM's van SQL Server tijdens het inrichten (Resource Manager-implementatiemodel) configureert. Ook wordt uitgelegd hoe u opslag kunt configureren voor uw bestaande SQL Server-VM's.
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/05/2017
ms.author: ninarn
ms.openlocfilehash: 7d076b970481b68d9c352d54f3452b8e222f5c64
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="storage-configuration-for-sql-server-vms"></a>Configuratie van de opslag voor virtuele machines van SQL Server
Wanneer u de installatiekopie van een SQL Server-virtuele machine in Azure configureert, kunt u de Portal de opslagconfiguratie automatiseren. Dit omvat het toevoegen van opslag aan de virtuele machine, zodat de opslag die toegankelijk is voor SQL Server en te configureren om te optimaliseren voor uw specifieke vereisten.

Dit onderwerp wordt uitgelegd hoe Azure configureert opslag voor uw VM's van SQL Server tijdens het inrichten, en ook voor bestaande virtuele machines. Deze configuratie is gebaseerd op de [best practices prestaties](virtual-machines-windows-sql-performance.md) voor Azure virtuele machines waarop SQL Server wordt uitgevoerd.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van de configuratie-instellingen voor automatische opslag, moet de virtuele machine de volgende kenmerken:

* Ingericht met een [image in SQL Server-galerie](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).
* Maakt gebruik van de [Resource Manager-implementatiemodel](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Maakt gebruik van [Premium-opslag](../premium-storage.md).

## <a name="new-vms"></a>Nieuwe virtuele machines
De volgende secties wordt beschreven hoe u opslag voor de nieuwe virtuele machines van de SQL Server configureert.

### <a name="azure-portal"></a>Azure Portal
Bij het inrichten van een virtuele machine met de installatiekopie van een SQL Server-galerie van Azure, kunt u automatisch configureren van de opslag voor uw nieuwe virtuele machine. U opgeven de grootte van de opslagruimte, prestatielimieten en type werkbelasting. De volgende schermafbeelding ziet de opslag configuratie blade die wordt gebruikt tijdens SQL VM-inrichting.

![SQL Server VM-opslag-configuratie tijdens het inrichten](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Azure voert op basis van uw keuze, de volgende opslagconfiguratietaken na het maken van de virtuele machine:

* Maakt en gegevensschijven premium-opslag is gekoppeld aan de virtuele machine.
* Hiermee configureert u de gegevensschijven zodat ze toegankelijk zijn voor SQL Server.
* Hiermee configureert u de gegevensschijven in een opslaggroep op basis van de opgegeven grootte en de prestaties (IOPS en doorvoerlimieten) vereisten.
* De opslaggroep koppelt aan een nieuw station op de virtuele machine.
* Deze nieuwe station op basis van het type van de opgegeven werkbelasting (datawarehousing, transactionele verwerking of algemeen) worden geoptimaliseerd.

Zie voor meer informatie over hoe Azure storage-instellingen configureert de [opslag configuratiesectie](#storage-configuration). Zie voor een volledig overzicht van het maken van een virtuele machine van SQL Server in de Azure Portal [de zelfstudie voor inrichting](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Sjablonen voor resource beheren
Als u de volgende Resource Manager-sjablonen gebruikt, worden twee premium gegevensschijven met geen opslaggroepconfiguratie wordt standaard gekoppeld. U kunt echter deze sjablonen om het aantal premium-schijven voor gegevens die zijn gekoppeld aan de virtuele machine te wijzigen.

* [Virtuele machine met automatische back-up maken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Virtuele machine maken met automatisch patchen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Virtuele machine maken met Azure Sleutelkluis-integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Bestaande virtuele machines
U kunt een aantal instellingen voor de opslag in de Azure portal wijzigen voor bestaande SQL Server-VM's. Selecteert u de virtuele machine, gaat u naar het gedeelte instellingen en selecteer vervolgens SQL Server-configuratiebestand. De configuratie van SQL Server-blade ziet u het huidige opslaggebruik van uw virtuele machine. Alle stations die bestaan op de virtuele machine worden weergegeven in deze grafiek. Voor elk station, wordt de opslagruimte die wordt weergegeven in vier secties:

* SQL-gegevens
* SQL-logboek
* Andere (niet-SQL-opslag)
* Beschikbaar

![Opslag configureren voor een bestaande SQL Server VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Klik op de koppeling bewerken boven het diagram voor het configureren van de opslag voor het toevoegen van een nieuwe schijf of een bestaand station uitbreiden.

De configuratieopties die u ziet varieert, afhankelijk van of u hebt deze functie voordat gebruikt. Wanneer u voor het eerst gebruikt, kunt u uw opslagvereisten voor een nieuw station opgeven. Als u deze functie eerder hebt gebruikt om een station te maken, kunt u de opslag van het station uitbreiden.

### <a name="use-for-the-first-time"></a>Gebruik voor de eerste keer
Als de eerste keer dat u deze functie is, kunt u de opslag en prestaties van de grootte van de limieten voor een nieuw station opgeven. Deze ervaring is vergelijkbaar met wat u ziet op tijd inrichten. Het belangrijkste verschil is dat u mag niet het type werkbelasting opgeven. Deze beperking wordt voorkomen dat een bestaande SQL Server-configuraties op de virtuele machine onderbreken.

![SQL Server-opslag schuifregelaars configureren](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure maakt een nieuw station op basis van uw specificaties. In dit scenario voert Azure de volgende configuratietaken voor opslag:

* Maakt en gegevensschijven premium-opslag is gekoppeld aan de virtuele machine.
* Hiermee configureert u de gegevensschijven zodat ze toegankelijk zijn voor SQL Server.
* Hiermee configureert u de gegevensschijven in een opslaggroep op basis van de opgegeven grootte en de prestaties (IOPS en doorvoerlimieten) vereisten.
* De opslaggroep koppelt aan een nieuw station op de virtuele machine.

Zie voor meer informatie over hoe Azure storage-instellingen configureert de [opslag configuratiesectie](#storage-configuration).

### <a name="add-a-new-drive"></a>Voeg een nieuw station
Als u al opslag hebt geconfigureerd op uw SQL Server VM, wordt het uitbreiden van de opslag twee nieuwe opties. De eerste optie is het toevoegen van een nieuwe schijf, waarvoor het prestatieniveau van uw virtuele machine kunt verhogen.

![Een nieuwe schijf toevoegen aan een SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Na het toevoegen van het station, moet u echter een aantal extra handmatige configuratie zodat de toename van de prestaties uitvoeren.

### <a name="extend-the-drive"></a>De schijf uitbreiden
De andere optie voor het uitbreiden van de opslag is de bestaande schijf uitbreiden. Deze optie verhoogt de beschikbare opslag voor de schijf, maar niet wordt verhoogd tot prestaties. Met opslaggroepen wijzigen u het aantal kolommen niet nadat de opslaggroep is gemaakt. Het aantal kolommen bepaalt het aantal parallelle schrijfbewerkingen, die kan worden striped worden verdeeld over de gegevensschijven. Eventuele extra gegevensschijven kunnen daarom niet prestaties verhogen. Ze kunnen alleen meer opslag bieden voor de gegevens worden geschreven. Deze beperking betekent ook dat wanneer u de schijf uitbreidt, het aantal kolommen bepaalt u het minimum aantal gegevensschijven die u kunt toevoegen. Dus als u een opslaggroep met vier gegevensschijven maakt, is het aantal kolommen ook vier. Elk gewenst moment uitbreiden van de opslag, moet u ten minste vier gegevensschijven toevoegen.

![Uitbreiden van een station voor een SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Opslagconfiguratie
Deze sectie bevat een verwijzing voor de opslag configuratiewijzigingen die Azure automatisch uitgevoerd tijdens de configuratie in de Azure Portal of SQL VM-inrichting.

* Als u minder dan twee TBs van opslag voor uw virtuele machine hebt geselecteerd, wordt in Azure een opslaggroep niet maken.
* Als u ten minste twee TBs van opslag voor uw virtuele machine hebt geselecteerd, configureert Azure een opslaggroep. De volgende sectie van dit onderwerp bevat de details van de opslaggroepconfiguratie.
* Maakt gebruik van automatische opslagconfiguratie altijd [premium-opslag](../premium-storage.md) P30 gegevensschijven. Er is daarom een 1:1-toewijzing tussen het geselecteerde aantal terabyte en het aantal gegevensschijven gekoppeld aan uw virtuele machine.

Zie voor informatie over prijzen, de [prijzen Storage](https://azure.microsoft.com/pricing/details/storage) pagina op de **schijfopslag** tabblad.

### <a name="creation-of-the-storage-pool"></a>Het maken van de opslaggroep
Azure maakt gebruik van de volgende instellingen voor het maken van de opslaggroep op VM's van SQL Server.

| Instelling | Waarde |
| --- | --- |
| Stripe-grootte |256 KB (gegevensopslag); 64 KB (transactionele) |
| Schijfformaten |1 TB |
| Cache |Lezen |
| Toegewezen grootte |De clustergrootte van 64 KB NTFS |
| Initialisatie van de directe bestanden |Ingeschakeld |
| Pagina's in het geheugen vergrendelen |Ingeschakeld |
| Herstel |Eenvoudig herstel (geen tolerantie) |
| Aantal kolommen |Aantal gegevensschijven<sup>1</sup> |
| TempDB-locatie |Opgeslagen op gegevensschijven<sup>2</sup> |

<sup>1</sup> nadat de opslaggroep is gemaakt, het aantal kolommen in de opslaggroep kan niet worden gewijzigd.

<sup>2</sup> deze instelling is alleen van toepassing op de eerste schijf die u maakt met de functie van de configuratie opslag.

## <a name="workload-optimization-settings"></a>Werkbelasting optimalisatie-instellingen
De volgende tabel beschrijft de werkbelasting van de drie opties beschikbaar en hun bijbehorende optimalisaties:

| Type werkbelasting | Beschrijving | Optimalisaties |
| --- | --- | --- |
| **Algemeen** |Standaardinstelling die ondersteuning biedt voor de meeste werkbelastingen |Geen |
| **Transactionele verwerking** |Optimaliseert de opslag voor OLTP-workloads van traditionele databases |Traceringsvlag 1117<br/>Traceringsvlag 1118 |
| **Gegevensopslag** |Optimaliseert de opslag voor analyse- en rapportageworkloads |Traceringsvlag 610<br/>Traceringsvlag 1117 |

> [!NOTE]
> U kunt alleen het type werkbelasting opgeven wanneer u een virtuele SQL-machine inrichten door deze te selecteren in de configuratiestap opslag.
>
>

## <a name="next-steps"></a>Volgende stappen
Zie voor andere onderwerpen die betrekking hebben op SQL Server wordt uitgevoerd in Azure VM's [SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
