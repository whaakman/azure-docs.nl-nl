---
title: Een SQL Server-database migreren naar SQL Server op een virtuele machine | Microsoft Docs
description: Meer informatie over het migreren van een on-premises gebruiker-database met SQL Server in Azure een virtuele machine.
services: virtual-machines-windows
documentationcenter: 
author: sabotta
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: carlasab
ms.openlocfilehash: 68767534298783083a441aa295611914d0df9db0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Migreer een SQL Server-database naar een SQL Server in een virtuele machine van Azure

Er zijn een aantal methoden voor het migreren van een on-premises SQL Server gebruiker-database met SQL Server in een Azure VM. Dit artikel wordt kort worden besproken verschillende methoden en de beste methode voor verschillende scenario's het beste.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="what-are-the-primary-migration-methods"></a>Wat zijn de primaire migratiemethoden?
De primaire migratiemethoden zijn:

* Lokale back-up maken gebruik van compressie en kopieert u handmatig het back-upbestand naar de virtuele machine van Azure
* Een back-up naar URL's en terugzetten naar de Azure virtuele machine van de URL
* Ontkoppel en kopieer de bestanden voor gegevens en logboekbestanden naar Azure blob storage en vervolgens koppelen aan SQL Server in Azure VM van URL
* Lokale fysieke machine converteren naar Hyper-V-VHD en vervolgens implementeren als nieuwe virtuele machine via geüpload VHD uploaden naar Azure Blob storage
* Verzenden van de harde schijf met de Windows-Service voor importeren/exporteren
* Zo hebt u een AlwaysOn-implementatie van on-premises, gebruikt u de [Azure Replica Wizard toevoegen](../classic/sql-onprem-availability.md) voor het maken van een replica in Azure en vervolgens failover, wijst u gebruikers aan het Azure-database-exemplaar
* SQL Server gebruiken [transactionele replicatie](https://msdn.microsoft.com/library/ms151176.aspx) voor het configureren van de Azure SQL Server-exemplaar als een abonnee en schakelt u replicatie uit, die gebruikers verwijst naar de Azure-database-exemplaar

> [!TIP]
> U kunt ook dezelfde technieken gebruiken databases verplaatsen tussen VM's van SQL Server in Azure. Bijvoorbeeld: Er is geen ondersteunde manier een SQL Server-afbeelding VM van één versie /-editie bijwerken naar een andere. In dit geval moet u een nieuwe virtuele machine een SQL-Server maken met de nieuwe versie /-editie en gebruik een van de migratie technieken in dit artikel verplaatsen van uw databases. 

## <a name="choosing-your-migration-method"></a>Uw migratiemethode kiezen
Voor optimale prestaties overdracht, door de databasebestanden te migreren naar de Azure-VM met een gecomprimeerde back-upbestand.

De optie AlwaysOn of de optie voor transactionele replicatie gebruiken om de uitvaltijd tijdens het migratieproces van de database.

Als het niet mogelijk om de bovenstaande methoden te gebruiken, handmatig migreren van uw database. Met deze optie kunt u in het algemeen beginnen met een databaseback-up gevolgd door een kopie van de databaseback-up naar Azure en voert u een database terugzetten. U kunt ook de bestanden van de database zelf te kopiëren naar Azure en koppelt u ze. Er zijn verschillende methoden waarmee u deze handmatige verwerking van een database migreren naar een virtuele machine van Azure kunt uitvoeren.

> [!NOTE]
> Wanneer u een upgrade naar SQL Server 2014 of SQL Server 2016 van oudere versies van SQL Server uitvoert, kunt u overwegen of er wijzigingen nodig zijn. Het is raadzaam dat u alle afhankelijkheden op functies die niet wordt ondersteund door de nieuwe versie van SQL Server als onderdeel van uw migratieproject te houden. Zie voor meer informatie over de ondersteunde versies en scenario's [upgraden naar SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

De volgende tabel geeft een lijst van elk van de primaire migratiemethoden en wordt beschreven wanneer het gebruik van elke methode is het meest geschikt.

| Methode | De versie van de gegevensbron-database | De versie van de doel-database | De grootte van de back-beperking op de bron database | Opmerkingen |
| --- | --- | --- | --- | --- |
| [Lokale back-up maken gebruik van compressie en kopieert u handmatig het back-upbestand naar de virtuele machine van Azure](#backup-and-restore) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[De opslaglimiet van de Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Dit is een zeer eenvoudige en uitgebreid geteste techniek voor het verplaatsen van databases op computers. |
| [Een back-up naar URL's en terugzetten naar de Azure virtuele machine van de URL](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 of hoger |SQL Server 2012 SP1 CU2 of hoger |< 12,8 TB voor SQL Server 2016, anders < 1 TB | Deze methode is gewoon een manier om de back-upbestand naar de virtuele machine met behulp van Azure-opslag. |
| [Ontkoppel en kopieer de bestanden voor gegevens en logboekbestanden naar Azure blob storage en vervolgens koppelen aan SQL Server in Azure virtuele machine van de URL](#detach-and-attach-from-url) |SQL Server 2005 of hoger |SQL Server 2014 of hoger |[De opslaglimiet van de Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruik deze methode wanneer u van plan bent om [Bewaar deze bestanden met behulp van de Azure Blob storage-service](https://msdn.microsoft.com/library/dn385720.aspx) en koppel deze aan de SQL Server wordt uitgevoerd in een virtuele machine van Azure, met name met zeer grote databases |
| [Lokale machine converteren naar Hyper-V-VHD's, uploaden naar Azure Blob storage en vervolgens een nieuwe virtuele machine met behulp van geüploade VHD implementeren](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[De opslaglimiet van de Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruiken wanneer [brengen van uw eigen SQL Server-licentie](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), bij het migreren van een database die u in een oudere versie van SQL Server wordt uitgevoerd of bij het systeem- en gebruikersdatabases samen migreren als onderdeel van de migratie van de database is afhankelijk van andere gebruikersdatabases en/of systeemdatabases. |
| [Verzenden van de harde schijf met de Windows-Service voor importeren/exporteren](#ship-hard-drive) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[De opslaglimiet van de Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Gebruik de [Windows Import/Export-Service](../../../storage/common/storage-import-export-service.md) wanneer handmatige copy-methode is te langzaam, zoals met zeer grote databases |
| [Gebruik de Wizard Azure Replica toevoegen](../classic/sql-onprem-availability.md) |SQL Server 2012 of hoger |SQL Server 2012 of hoger |[De opslaglimiet van de Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Minimaliseert uitvaltijd, gebruik wanneer u een AlwaysOn-on-premises-implementatie |
| [Transactionele replicatie van SQL Server gebruiken](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 of hoger |SQL Server 2005 of hoger |[De opslaglimiet van de Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Wanneer u de uitvaltijd te minimaliseren moet gebruiken en geen van de on-premises implementatie van een AlwaysOn |

## <a name="backup-and-restore"></a>Back-ups en herstellen
Maak een back-up van de database met compressie, de back-up naar de virtuele machine en de database vervolgens herstellen. Als uw back-upbestand groter dan 1 TB is, moet u het stripe omdat de maximale grootte van een VM-schijf 1 TB is. Gebruik de volgende algemene stappen voor het migreren van een database voor deze handmatige methode gebruiken:

1. Voer een volledige databaseback-up naar een on-premises locatie.
2. Maken of een virtuele machine met de versie van SQL Server gewenst uploaden.
3. Het instellen van connectiviteit op basis van uw vereisten. Zie [verbinding maken met een virtuele Machine van SQL Server op Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
4. Uw back-bestanden worden gekopieerd naar de virtuele machine via Extern bureaublad, Windows Verkenner of de opdracht kopiëren vanaf een opdrachtprompt.

## <a name="backup-to-url-and-restore"></a>Back-up naar URL's en terugzetten
In plaats van de back-ups op een lokaal bestand, kunt u de [back-up naar URL](https://msdn.microsoft.com/library/dn435916.aspx) en herstel van de URL naar de virtuele machine. Met SQL Server 2016 worden striped back-upsets worden ondersteund, aanbevolen voor prestaties en moet groter zijn dan de maximale grootte per blob. Voor zeer grote databases en het gebruik van de [Windows Import/Export-Service](../../../storage/common/storage-import-export-service.md) wordt aanbevolen.

## <a name="detach-and-attach-from-url"></a>Ontkoppel en koppelen van URL
Ontkoppel de database en logboekbestanden bestanden en deze overbrengen naar [Azure Blob storage](https://msdn.microsoft.com/library/dn385720.aspx). Koppel vervolgens de database van de URL in uw Azure VM. Gebruik deze optie als u wilt dat de bestanden van de fysieke database zich bevinden in de Blob-opslag. Dit kan handig zijn voor zeer grote databases. Gebruik de volgende algemene stappen voor het migreren van een database voor deze handmatige methode gebruiken:

1. Loskoppelen van de databasebestanden van de lokale database-exemplaar.
2. Kopieer de databasebestanden losgekoppelde naar Azure blob storage met behulp van de [AZCopy-opdrachtregelprogramma](../../../storage/common/storage-use-azcopy.md).
3. De databasebestanden vanuit de Azure-URL toevoegen aan de SQL Server-exemplaar in de Azure VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Converteer de SQL Server naar een virtuele machine en upload deze naar de URL. Implementeer de SQL Server vervolgens als nieuwe VM
Gebruik deze methode voor het migreren van alle systeem- en -databases in een lokale SQL Server-exemplaar naar de virtuele machine van Azure. Gebruik de volgende algemene stappen voor het migreren van een volledige SQL Server-exemplaar met behulp van deze handmatige methode:

1. Converteren van fysieke of virtuele machines naar Hyper-V-VHD's met behulp van [Microsoft Virtual Machine Converter](https://technet.microsoft.com/library/dn874008(v=ws.11).aspx).
2. VHD-bestanden uploaden naar Azure Storage met behulp van de [cmdlet Add-AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Een nieuwe virtuele machine implementeren met behulp van de geüploade VHD.

> [!NOTE]
> Als u wilt migreren van een volledige toepassing, kunt u overwegen [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Verzenden van de harde schijf
Gebruik de [Windows Import/Export-Service methode](../../../storage/common/storage-import-export-service.md) grote hoeveelheden gegevens uit een bestand overdragen naar Azure Blob storage in situaties waarbij uploaden via het netwerk beperkend dure of niet haalbaar is is. Met deze service verzendt u een of meer harde schijven met gegevens die u wilt een Azure-Datacenter, waar uw gegevens worden geüpload naar uw opslagaccount.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

Zie voor instructies over het maken van een virtuele Machine van Azure SQL Server vanuit een vastgelegd beeld [Tips & trucs voor SQL Azure virtuele machines van de vastgelegde installatiekopieën klonen](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) op de blog Engineers van CSS SQL Server.

