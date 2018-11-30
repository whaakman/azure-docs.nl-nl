---
title: Wat is er nieuw in Microsoft Azure Backup Server
description: Microsoft Azure Backup-Server biedt u verbeterde back-mogelijkheden voor het beveiligen van virtuele machines, bestanden en mappen en werkbelastingen. Informatie over het installeren of upgraden naar Azure Backup Server V3.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adigan; kasinh
ms.openlocfilehash: e963f675dfe2a57fc19f3169beeb1d7d4066af93
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315593"
---
# <a name="whats-new-in-microsoft-azure-backup-server"></a>Wat is er nieuw in Microsoft Azure Backup Server

Microsoft Azure Backup Server versie 3 (V3 MABS) is de meest recente upgrade en bevat belangrijke bugfixes, ondersteuning voor Windows Server 2019, ondersteuning voor SQL-2017 en andere functies en verbeteringen. Om weer te geven van de lijst met fouten die zijn verholpen en de installatie-instructies voor MABS V3, Zie KB-artikel [4457852](https://support.microsoft.com/en-us/help/4457852/microsoft-azure-backup-server-v3).

De volgende functies zijn opgenomen in MABS V3:

## <a name="volume-to-volume-migration"></a>Volume-doelvolume van migratie
Met Modern Backup Storage (MBS) in MABS V2, hebben we aangekondigd werkbelastingbewuste opslag, waarbij u bepaalde workloads naar een back-up voor specifieke opslag te configureren op basis van eigenschappen van opslag. Echter, na de configuratie merkt u misschien een hoeft te verplaatsen van back-ups van bepaalde gegevensbronnen naar andere opslag voor geoptimaliseerde Resourcegebruik. MABS V3 biedt u de mogelijkheid voor het migreren van uw back-ups en configureer ze zodanig worden opgeslagen naar een ander volume in [in 3 stappen](https://blogs.technet.microsoft.com/dpm/2017/10/24/storage-migration-with-dpm-2016-mbs/).

## <a name="prevent-unexpected-data-loss"></a>Voorkom het verlies van onverwachte
MABS wordt binnen ondernemingen beheerd door een team van beheerders. Zijn er richtlijnen voor opslag die moet worden gebruikt voor back-ups, worden een onjuiste volume gegeven aan MABS als back-upopslag kan leiden tot verlies van kritieke gegevens. Met MABS V3, kunt u voorkomen dat dergelijke scenario's door het configureren van de volumes die niet beschikbaar voor het gebruik van opslag zijn [deze PowerShell-cmdlets](https://docs.microsoft.com/system-center/dpm/add-storage#volume-exclusion).

## <a name="custom-size-allocation"></a>Toewijzing van de aangepaste grootte
Modern Backup Storage (MBS) verbruikt opslag dun, zo nodig. Om dit te doen, berekent MABS de grootte van de gegevens back-up wanneer deze is geconfigureerd voor beveiliging. Echter, als veel bestanden en mappen wordt een back-samen, zoals in het geval van een bestandsserver grootteberekening kan lang duren. Met MABS V3, kunt u MABS voor het accepteren van de volumegrootte als standaard, in plaats van het berekenen van de grootte van elk bestand configureren daarom tijd te besparen.

## <a name="optimized-cc-for-rct-vms"></a>Geoptimaliseerde CC voor RCT VM 's
MABS maakt gebruik van RCT (het systeemeigen bijhouden in Hyper-V), welke vermindert de noodzaak voor tijdrovende consistentiecontroles controleert op in scenario's zoals VM vastloopt. RCT biedt betere tolerantie dan de bijhouden geleverd door de VSS-momentopname gebaseerde back-ups. MABS V3 optimaliseert de netwerk- en opslagverbruik verder door over te dragen alleen de gewijzigde gegevens tijdens een consistentiecontrole.

## <a name="support-to-tls-12"></a>Ondersteuning van TLS 1.2
TLS 1.2 is de veilige manier van communicatie voorgesteld door Microsoft met beste versleuteling. MABS biedt nu ondersteuning voor TLS 1.2-communicatie tussen MABS en de beveiligde servers, voor verificatie op basis van certificaten, en voor cloudback-ups.

## <a name="vmware-vm-protection-support"></a>Ondersteuning voor VMware-VM
VMware-VM back-up wordt nu ondersteund voor productie-implementaties. MABS V3 biedt de volgende onderwerpen voor VMware-VM-beveiliging:

-   Ondersteuning voor vCenter en ESXi 6.5, samen met de ondersteuning voor 5.5 en 6.0.
- Automatische beveiliging van virtuele VMware-machines naar de cloud. Als nieuwe VMware-VM's naar een beveiligde map worden toegevoegd, worden ze automatisch beveiligd naar schijf en de cloud.
- Herstel efficiëntieverbeteringen voor het herstel van VMware alternatieve locatie.

## <a name="sql-2017-support"></a>Ondersteuning voor SQL-2017
MABS V3 kan worden geïnstalleerd met SQL 2017 als de MAB-database. U kunt de SQL server vanuit SQL 2016 upgraden naar SQL 2017 of opnieuw worden geïnstalleerd. U kunt ook back-up van SQL 2017 werkbelasting, zowel in geclusterde en niet-geclusterde omgeving met MABS V3.

## <a name="windows-server-2019-support"></a>Ondersteuning voor Windows Server 2019
MABS V3 kan worden geïnstalleerd op Windows Server 2019. Voor het gebruik van MABS V3 met WS2019, kunt u een upgrade uitvoeren voor uw besturingssysteem WS2019 voordat het besturingssysteem installeren/upgraden naar MABS V3 of u kunt upgraden V3 op WS2016 installeren boeken/bijwerken.

MABS V3 betreft een volledige release, en kan worden geïnstalleerd op Windows Server 2016, Windows Server 2019, rechtstreeks of kunnen worden bijgewerkt vanuit MABS V2. Voordat u een upgrade naar uitvoert of V3 voor back-up-Server installeert, kunt u lezen over de installatievereisten.
Zoeken naar meer informatie over de installatie/upgrade stappen voor MABS [hier](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#software-package).


> [!NOTE]

> MABS heeft dezelfde codebasis als System Center Data Protection Manager. MABS v3 is gelijk aan Data Protection Manager 1807.

## <a name="next-steps"></a>Volgende stappen

Informatie over het voorbereiden van de server of beginnen met een werkbelasting beveiligen:
- [Bekende problemen in MABS V3](backup-mabs-release-notes-v3.md)
- [Back-up-Server-workloads voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Backup Server gebruiken voor back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Backup Server gebruiken voor back-up van SQL Server](backup-azure-sql-mabs.md)
- [Modern Backup Storage gebruiken met back-upserver](backup-mabs-add-storage.md)
