---
title: Een SQL Server-database migreren naar SQL Server op een virtuele machine | Microsoft Docs
description: Meer informatie over hoe u een on-premises gebruikersdatabase migreren naar SQL Server in een virtuele machine van Azure.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 08/18/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 95acda60935e82b226a1a0e860b5fa8effb8e47e
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332129"
---
# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Migreer een SQL Server-database naar een SQL Server in een virtuele machine van Azure

Er zijn een aantal methoden voor het migreren van een gebruikersdatabase voor on-premises SQL Server naar SQL Server in een Azure-VM. In dit artikel wordt kort bespreken van verschillende methoden en aanbevolen om de beste methode voor verschillende scenario's.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="what-are-the-primary-migration-methods"></a>Wat zijn de primaire migratiemethoden?
De primaire migratiemethoden zijn:

* On-premises back-up maken met behulp van compressie en kopieert u handmatig het back-upbestand naar de virtuele machine van Azure
* Een back-up naar URL en herstel bij de Azure virtuele machine van de URL
* Loskoppelen en kopieer de bestanden van de gegevens en logboekbestanden naar Azure blob-opslag en vervolgens koppelen aan SQL Server in virtuele Azure-machine via URL
* On-premises fysieke machine converteren naar Hyper-V-VHD, uploaden naar Azure Blob-opslag en vervolgens implementeert als VHD met behulp van nieuwe virtuele machine worden geüpload
* Verzending van harde schijf met behulp van Windows Import/Export-Service
* Als u de implementatie van een AlwaysOn-beschikbaarheidsgroep on-premises, gebruikt u de [Wizard Azure Replica toevoegen](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) voor het maken van een replica in Azure en vervolgens een failover, wijst u gebruikers met het Azure-database-exemplaar
* SQL Server gebruiken [transactionele replicatie](https://msdn.microsoft.com/library/ms151176.aspx) voor het configureren van de Azure SQL Server-exemplaar als een abonnee en schakelt u replicatie uit, die u gebruikers verwijst naar de Azure database-instantie

> [!TIP]
> U kunt ook deze dezelfde technieken gebruiken om databases te verplaatsen tussen SQL Server-VM's in Azure. Bijvoorbeeld, is er geen ondersteunde manier een VM van SQL Server-afbeelding van één versie/editie bijwerken naar een andere. In dit geval moet u een nieuwe SQL Server-VM maken met de nieuwe versie/editie en gebruik een van de migratie-technieken in dit artikel om uw databases te verplaatsen. 

## <a name="choosing-your-migration-method"></a>Uw migratiemethode kiezen
Voor optimale overdrachtprestaties, migreert u bestanden van de database in de Azure-VM met behulp van een gecomprimeerd bestand met back-up.

Om te beperken downtime tijdens het migratieproces van de database, moet u de optie AlwaysOn of de optie voor transactionele replicatie gebruiken.

Als het niet mogelijk om de bovenstaande methoden te gebruiken, moet u handmatig uw database migreren. Met deze methode, wordt u in het algemeen beginnen met een databaseback-up gevolgd door een kopie van de databaseback-up naar Azure en voert u een database terugzetten. U kunt ook de bestanden van de database zelf te kopiëren naar Azure en koppelt u ze. Er zijn verschillende methoden waarmee u deze handmatige verwerking van de migratie van een database in een Azure-VM kunt uitvoeren.

> [!NOTE]
> Wanneer u een upgrade naar SQL Server 2014 of SQL Server 2016 van oudere versies van SQL Server uitvoert, kunt u overwegen of er wijzigingen nodig zijn. Het is raadzaam dat u alle afhankelijkheden op die niet worden ondersteund door de nieuwe versie van SQL Server als onderdeel van uw migratieproject kunt oplossen. Zie voor meer informatie over de ondersteunde versies en scenario's, [upgraden naar SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

De volgende tabel geeft een lijst van elk van de primaire migratie-methoden en wordt beschreven wanneer het gebruik van elke methode is het meest geschikt.

| Methode | De versie van de gegevensbron-database | De versie van de doel-database | Bron-database back-formaat beperking | Opmerkingen |
| --- | --- | --- | --- | --- |
| [On-premises back-up maken met behulp van compressie en kopieert u handmatig het back-upbestand naar de virtuele machine van Azure](#backup-and-restore) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[Azure VM-limiet voor opslag](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Dit is een zeer eenvoudige en goed geteste techniek voor het verplaatsen van databases tussen computers. |
| [Een back-up naar URL en herstel bij de Azure virtuele machine van de URL](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 of hoger |SQL Server 2012 SP1 CU2 of hoger |< 12,8 TB voor SQL Server 2016, anders < 1 TB | Deze methode is gewoon een manier om de back-upbestand naar de virtuele machine met behulp van Azure storage verplaatsen. |
| [Loskoppelen en kopieer de bestanden van de gegevens en logboekbestanden naar Azure blob-opslag en vervolgens koppelen met SQL Server in virtuele Azure-machine via URL](#detach-and-attach-from-url) |SQL Server 2005 of hoger |SQL Server 2014 of hoger |[Azure VM-limiet voor opslag](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruik deze methode wanneer u van plan bent [opslaan van deze bestanden met behulp van de Azure Blob storage-service](https://msdn.microsoft.com/library/dn385720.aspx) en koppel deze aan SQL Server wordt uitgevoerd in een Azure-VM, met name met zeer grote databases |
| [On-premises computer converteren naar Hyper-V-VHD's, uploaden naar Azure Blob-opslag en vervolgens implementeert een nieuwe virtuele machine met de geüploade virtuele harde schijf](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[Azure VM-limiet voor opslag](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruiken wanneer [brengen van uw eigen SQL Server-licentie](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), bij het migreren van een database die u op een oudere versie van SQL Server wordt uitgevoerd of bij het migreren van systeem en gebruikersdatabases samen als onderdeel van de migratie van de database is afhankelijk van andere gebruikersdatabases en/of systeemdatabases. |
| [Verzending van harde schijf met behulp van Windows Import/Export-Service](#ship-hard-drive) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[Azure VM-limiet voor opslag](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruik de [Windows Import/Export-Service](../../../storage/common/storage-import-export-service.md) wanneer de handmatige methode is te langzaam verlopen, zoals met zeer grote databases |
| [Gebruik de Wizard Azure Replica toevoegen](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) |SQL Server 2012 of hoger |SQL Server 2012 of hoger |[Azure VM-limiet voor opslag](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Minimaliseert downtime, gebruik deze sjabloon wanneer u hebt een on-premises implementatie altijd op |
| [SQL Server transactionele replicatie gebruiken](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[Azure VM-limiet voor opslag](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruiken wanneer u nodig hebt om te minimaliseren van downtime en ook geen een Always On on-premises implementatie |

## <a name="backup-and-restore"></a>Back-ups en herstellen
Maak een back-up van uw database met compressie kopiëren van de back-up naar de virtuele machine en de database vervolgens herstellen. Als uw back-upbestand groter dan 1 TB is, moet u het stripe omdat de maximale grootte van een VM-schijf 1 TB is. Gebruik de volgende algemene stappen voor het migreren van een gebruikersdatabase die met deze handmatige methode:

1. Voer een volledige databaseback-up naar een on-premises locatie.
2. Maken of uploaden van een virtuele machine met de versie van SQL Server nodig.
3. Connectiviteit op basis van uw vereisten instellen. Zie [verbinding maken met een SQL Server-Machine in Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
4. Uw back-bestanden kopiëren naar uw virtuele machine via Extern bureaublad, Windows Explorer of de kopieeropdracht uit vanaf de opdrachtprompt.

## <a name="backup-to-url-and-restore"></a>Back-up naar URL's en herstellen
In plaats van de back-ups op een lokaal bestand, kunt u de [back-up naar URL](https://msdn.microsoft.com/library/dn435916.aspx) en herstel van de URL op de virtuele machine. Met SQL Server 2016, worden striped back-upsets worden ondersteund, aanbevolen voor prestaties en moet groter zijn dan de maximale grootte per blob. Voor zeer grote databases, het gebruik van de [Windows Import/Export-Service](../../../storage/common/storage-import-export-service.md) wordt aanbevolen.

## <a name="detach-and-attach-from-url"></a>Loskoppelen en koppelen via URL
Loskoppelen van uw database en logboekbestanden bestanden en deze overbrengen naar [Azure Blob-opslag](https://msdn.microsoft.com/library/dn385720.aspx). Koppel vervolgens de database van de URL van uw VM in Azure. Gebruik deze optie als u wilt dat de bestanden van de fysieke database wilt opslaan in Blob-opslag. Dit kan nuttig zijn voor zeer grote databases zijn. Gebruik de volgende algemene stappen voor het migreren van een gebruikersdatabase die met deze handmatige methode:

1. Loskoppelen van de databasebestanden van de on-premises database-exemplaar.
2. Kopieer de databasebestanden ontkoppelde naar Azure blob storage met behulp van de [AZCopy-opdrachtregelprogramma](../../../storage/common/storage-use-azcopy.md).
3. Bestanden van de database uit de Azure-URL toevoegen aan de SQL Server-exemplaar in de Azure-VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Converteer de SQL Server naar een virtuele machine en upload deze naar de URL. Implementeer de SQL Server vervolgens als nieuwe VM
Gebruik deze methode voor het migreren van alle systeem- en -databases in een on-premises SQL Server-exemplaar met Azure-machine. Gebruik de volgende algemene stappen voor het migreren van een volledige SQL Server-exemplaar met behulp van deze handmatige methode:

1. Fysieke of virtuele machines converteren naar Hyper-V-VHD's.
2. VHD-bestanden uploaden naar Azure Storage met behulp van de [cmdlet Add-AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Een nieuwe virtuele machine implementeren met behulp van de geüploade VHD.

> [!NOTE]
> Als u wilt migreren van een volledige reeks toepassingen, kunt u overwegen [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Verzending van harde schijf
Gebruik de [Windows Import/Export-Service-methode](../../../storage/common/storage-import-export-service.md) om over te dragen van grote hoeveelheden bestandsgegevens over naar Azure Blob storage in situaties waar uploaden via het netwerk bijzonder kostbaar of niet haalbaar is is. Met deze service verzendt u een of meer harde schijven met die gegevens naar een Azure-Datacenter, waar uw gegevens worden geüpload naar uw opslagaccount.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

> [!TIP]
> Als u vragen hebt over virtuele machines met SQL Server, raadpleegt u [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).

Zie voor instructies over het maken van een Azure SQL Server-Machine vanuit een vastgelegd beeld [Tips en trucs voor Azure SQL-virtuele machines van de vastgelegde installatiekopieën te 'klonen'](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) op de CSS-SQL Server-Engineers-blog.

