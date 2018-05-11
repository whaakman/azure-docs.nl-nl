---
title: Maak een back-up van Azure-Stack-bestanden en toepassingen | Microsoft Docs
description: Azure Backup gebruiken voor back-up en herstellen van Azure-Stack-bestanden en toepassingen naar uw Azure-Stack-omgeving.
services: backup
documentationcenter: ''
author: adiganmsft
manager: shivamg
editor: ''
keyword: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/9/2018
ms.author: adigan,markgal
ms.openlocfilehash: a907335ace1f6ea9ec427327d28ca9be5ce02fcc
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Back-up van bestanden en toepassingen op Azure-Stack
U kunt Azure back-up beveiligen (of back-up) bestanden en toepassingen op Azure-Stack. Als u wilt back-up van bestanden en toepassingen, Microsoft Azure Backup-Server te installeren als een virtuele machine uitgevoerd op Azure-Stack. Eenmaal u Azure Backup-Server hebt geïnstalleerd, voegt Azure-schijven voor een verhoging van de lokale opslag beschikbaar voor back-upgegevens op korte termijn. Azure Backup-Server gebruikmaakt van Azure-opslag op lange termijn.

> [!NOTE]
> Hoewel Azure Backup-Server en System Center Data Protection Manager (DPM) zijn vergelijkbaar, wordt DPM wordt niet ondersteund voor gebruik met Azure-Stack.
>


## <a name="azure-backup-server-protection-matrix"></a>Beveiligingsmatrix voor Azure Backup Server
Azure Backup-Server beveiligt de volgende Stack Azure VM-werkbelastingen.

| Beveiligde gegevensbron | Beveiliging en herstel |
| --------------------- | ----------------------- |
| Windows Server Semi-per jaar kanaal - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| WindowsServer 2016 - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| Windows Server 2012 R2 - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| WindowsServer 2012 - Entprise-Datacenter-standaard | Volumes, bestanden, mappen |
| Windows Server 2008 R2 - Enterprise-Datacenter-standaard | Volumes, bestanden, mappen |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2013 | Farm, database, frontend, webserver |
| SharePoint 2010 | Farm, database, frontend, webserver |
| Systeemstatus | Systeemstatus |
| Bare-metal recovery (BMR) | BMR, systeemstatus, bestanden en mappen | 

## <a name="install-azure-backup-server"></a>Azure Backup-Server installeren
Zie het artikel Azure Backup-Server installeren op een virtuele machine van Azure-Stack [voorbereiden van de back-up van werkbelastingen met Azure Backup-Server](backup-azure-microsoft-azure-backup.md). Voordat u installeren en configureren van Azure Backup-Server, worden rekening met het volgende:

### <a name="determining-size-of-virtual-machine"></a>Grootte van virtuele machine vaststellen
Azure Backup-Server op een virtuele machine van Azure-Stack uitgevoerd, gebruikt u de grootte A2 of hoger. Download voor hulp bij het kiezen van de grootte van een virtuele machine, de [Azure Stack VM-grootte Rekenmachine](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Virtuele netwerken op de Stack Azure virtuele machines
Alle virtuele machines die worden gebruikt in een Azure-Stack-werkbelasting moet behoren tot de dezelfde Azure-netwerk en Azure-abonnement.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Opslaan van back-upgegevens op lokale schijf en in Azure
Azure Backup-Server slaat de back-upgegevens op Azure schijven zijn gekoppeld aan de virtuele machine voor operationeel herstel. Zodra de schijven en de opslagruimte die zijn gekoppeld aan de virtuele machine, beheert back-upserver van Azure storage voor u. De hoeveelheid back-upgegevens opslag is afhankelijk van het aantal en grootte van de schijven die zijn gekoppeld aan elk [Stack Azure virtuele machine](../azure-stack/user/azure-stack-storage-overview.md). Elke grootte van de Stack van virtuele machine in Azure heeft het maximale aantal schijven dat kan worden gekoppeld aan de virtuele machine. A2 is bijvoorbeeld vier schijven. A3 is acht schijven. A4 is 16 schijven. Bepaalt het totale aantal back-upopslag opnieuw, de grootte en het aantal schijven.

> [!IMPORTANT]
> U moet **niet** operationele herstelgegevens (back-up) op schijven met Azure Backup-Server gekoppeld voor meer dan vijf dagen bewaren.
>

Back-upgegevens opslaan in Azure, vermindert back-upinfrastructuur op Azure-Stack. Als gegevens niet meer dan vijf dagen, moet worden opgeslagen in Azure.

Als u wilt back-gegevens opslaat in Azure, maken of gebruiken van een Recovery Services-kluis. Bij de voorbereiding op de back-up van de werkbelasting van de Azure Backup-Server, wordt u [configureren van de Recovery Services-kluis](backup-azure-microsoft-azure-backup.md#recovery-services-vault). Na de configuratie, elke keer die een back-uptaak wordt uitgevoerd, wordt er een herstelpunt gemaakt in de kluis. Elke Recovery Services-kluis bevat tot 9999 herstelpunten. Afhankelijk van het aantal herstelpunten die zijn gemaakt en hoe lang ze blijven behouden, kunt u back-upgegevens jaren behouden. U kan bijvoorbeeld maandelijks herstelpunten maken en deze gedurende vijf jaar te handhaven.
 
### <a name="using-sql-server"></a>Met behulp van SQL Server
Als u een externe SQL Server gebruikt voor de Azure Backup-Server-database wilt, selecteert u alleen een Stack van virtuele machine van Azure met SQL Server.

### <a name="azure-backup-server-vm-performance"></a>Azure virtuele machine back-up van Server-prestaties
Als gedeeld met andere virtuele machines, kunnen de grootte van de opslagruimte en IOPS-limieten invloed op de prestaties van de virtuele machine Azure Backup-Server. Daarom moet u een afzonderlijke opslagaccount voor de virtuele machine van Azure Backup-Server. De Azure Backup-agent op de Azure Backup-Server wordt uitgevoerd, heeft tijdelijke opslag voor:
    - eigen gebruik nodig (een cachelocatie)
    - gegevens die worden hersteld vanuit de cloud (lokaal faseringsgebied)
  
### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup tijdelijke schijfopslag configureren
Elke virtuele machine van Azure-Stack wordt geleverd met tijdelijke schijfruimte die beschikbaar is voor de gebruiker als volume D:`\`. Het lokale faseringsgebied dat Azure Backup nodig kan worden geconfigureerd dat het zich in D:`\`, en de cachelocatie kan worden geplaatst op C:`\`. Er is geen opslag moet verzinken weg van de gegevensschijven gekoppeld aan de virtuele machine van Azure Backup-Server op deze manier.

### <a name="scaling-deployment"></a>Implementatie schalen
Als u schalen van uw implementatie wilt, hebt u de volgende opties:
  - Opschalen: verhoog de grootte van de virtuele machine Azure Backup-Server op D-reeks uit een reeks en verhogen van de lokale opslag [per de instructies van de virtuele machine Azure Stack](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Gegevens overdragen: verzend oudere gegevens naar Azure Backup-Server en bewaar alleen de nieuwste gegevens op de opslag die is gekoppeld aan de Azure Backup-Server.
  - Uitschalen - toevoegen van meer back-up van Azure-Servers om de werkbelastingen te beveiligen.


## <a name="bare-metal-recovery-for-azure-stack-vm"></a>Bare Metal Recovery voor Azure Stack VM

Een bare-metal recovery (BMR) back-up beveiligt besturingssysteembestanden en alle essentiële volumegegevens, behalve gebruikersgegevens. Een BMR back-up bevat een systeemstatusback-up. De volgende procedures wordt uitgelegd hoe de BMR-gegevens herstellen.

### <a name="run-recovery-on-the-azure-backup-server"></a>Herstel uitvoeren op de Azure Backup-Server

Open de console van de Azure Backup-Server.

1. Klik in de console **herstel**, de computer die u wilt herstellen en klik op Zoeken **Bare Metal Recovery**.
2. Beschikbare herstelpunten worden weergegeven in vet weergegeven in de kalender. Selecteer de datum en tijd voor het herstelpunt dat u wilt gebruiken.
3. In **Type herstelbewerking selecteren**, selecteer **kopiëren naar een netwerkmap**.
4. In **bestemming opgeven**, selecteer waar u wilt kopiëren van de gegevens. Vergeet niet dat de geselecteerde bestemming voldoende ruimte voor het herstelpunt moet hebben. Het wordt aangeraden dat u een nieuwe map maken.
5. In **herstelopties opgeven**, selecteer de beveiligingsinstellingen wilt toepassen en kies of u hardwaremomentopnamen op basis van SAN te gebruiken voor sneller herstel.     Momentopnamen van hardware op basis van SAN zijn een optie alleen als u een SAN met deze functionaliteit ingeschakeld en de mogelijkheid om te maken en een kloon hebt om deze beschrijfbaar te splitsen. Ook voor hardwaremomentopnamen van op basis van SAN te werken, moet de beveiligde computer en de Azure Backup-Server worden verbonden met hetzelfde netwerk.
6. Meldingsopties instellen en klik op **herstellen** op de **samenvatting** pagina.

### <a name="set-up-the-share-location"></a>De sharelocatie instellen
In de console Azure Backup-Server:
1. Navigeer naar de map met de back-up op de locatie herstellen.
2. Deel de map boven WindowsImageBackup, zodat de hoofdmap van de gedeelde map de map WindowsImageBackup is. Als de map WindowsImageBackup wordt niet gedeeld, kan het terugzetten van de back-up niet gevonden. Als u wilt verbinding maken met behulp van WinRE, moet u een WinRE toegankelijke share en de juiste IP-adres en referenties.

### <a name="restore-the-machine"></a>De machine herstellen

1. Op de virtuele machine waar u BMR herstellen, open een verhoogde opdrachtprompt en typ de volgende opdrachten. **/boottore** geeft aan dat Windows HERSTELOMGEVING wordt automatisch zodra het opstarten van het systeem gestart.
```
Reagentc /boottore
shutdown /r /t 0
```

2. Selecteer de taal en landinstellingen-instellingen in het dialoogvenster openen. Op de **installeren** Schakel in het scherm **uw computer herstellen**.
3. Op de **opties voor Systeemherstel** pagina **herstellen van de computer met de systeeminstallatiekopie van een dat u eerder hebt gemaakt**.
4. Op de **Selecteer een systeemkopieback-up** pagina **een systeemkopie selecteren** > **Geavanceerd** > **een systeemkopie zoeken op het netwerk**. Als u een waarschuwing wordt weergegeven, selecteert u **Ja**. Om de installatiekopie te selecteren, gaat u naar de netwerkshare, voer de referenties in en selecteer het herstelpunt. Hiermee wordt er gescand voor beschikbaar is in dat herstelpunt specifieke back-ups. Selecteer het herstelpunt.
5. In **kiezen hoe de back-up terugzetten**, selecteer **schijven formatteren en opnieuw partitioneren**. Controleer de instellingen in het volgende scherm en klikt u op **voltooien** om te beginnen met de hersteltaak. Opnieuw opstarten als vereist.

## <a name="see-also"></a>Zie ook
Zie voor informatie over andere werkbelastingen beveiligen met behulp van Azure Backup-Server een van de volgende artikelen:
- [Back-up van SharePoint-farm](backup-azure-backup-sharepoint-mabs.md)
- [Back-up van SQL server](backup-azure-sql-mabs.md)
