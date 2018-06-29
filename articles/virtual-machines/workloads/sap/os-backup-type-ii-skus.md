---
title: Besturingssysteem back-up en herstel van SAP HANA in Azure (grote exemplaren) typt u II SKU's | Microsoft Docs
description: Operatign systeemback-up en herstel voor SAP HANA op Azure (grote exemplaren) Type II SKU's uitvoeren
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f01a32612b335003856a372ece15ef300b9d93db
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063271"
---
# <a name="os-backup-and-restore-for-type-ii-skus"></a>OS-back-up en herstel voor Type II SKU 's

Dit document beschrijft de stappen voor het uitvoeren van een besturingssysteem niveau back-up en herstel voor de **Type II SKU's** van de grote HANA-exemplaren. 

>[!NOTE]
>De back-OS-scripts maakt gebruik van de software achterzijde, die vooraf worden geïnstalleerd op de server.  

Nadat het inrichten voltooid door het beheer van de Service Microsoft-team standaard is de server is geconfigureerd met twee back-ups plannen voor back-up van het bestandssysteem niveau back-up van het besturingssysteem. U kunt de planning van de back-uptaak controleren met behulp van de volgende opdracht:
```
#crontab –l
```
U kunt het back-upschema op elk gewenst moment met de volgende opdracht wijzigen:
```
#crontab -e
```
## <a name="how-to-take-a-manual-backup"></a>Hoe kan ik een handmatige back-up maken?

Het besturingssysteem bestandssysteemback-up is gepland met een **cron taak** al. U kunt echter de besturingssysteem bestand niveau back-up ook handmatig uitvoeren. Als u een handmatige back-up, voer de volgende opdracht:

```
#rear -v mkbackup
```
Voorbeeld voor een handmatige back-up ziet u het volgende scherm weergeven:

![hoe](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


## <a name="how-to-restore-a-backup"></a>Hoe kan ik een back-up terugzetten?

U kunt een volledige back-up of een afzonderlijk bestand terugzetten vanaf de back-up. Als u wilt herstellen, moet u de volgende opdracht gebruiken:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
Het bestand is hersteld na het terugzetten in de huidige werkmap.

De volgende opdracht toont het terugzetten van een bestand */etc/fstabfrom* het back-upbestand *backup.tar.gz*
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>U moet het bestand kopiëren naar de gewenste locatie nadat deze is hersteld van de back-up.

De volgende schermafbeelding ziet u het terugzetten van een volledige back-up:

![HowtoRestoreaBackup.PNG](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

## <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Het installeren van het hulpprogramma achterzijde en de configuratie wijzigen? 

De pakketten Relax-en-herstel (achter) zijn **vooraf geïnstalleerde** in de **Type II SKU's** van grote instanties HANA en er is geen actie te ondernemen. U kunt rechtstreeks starten met de achterzijde voor back-up van het besturingssysteem.
In de omstandigheden waarin u wilt de pakketten in uw eigen worden geïnstalleerd, kunt u echter de vermelde stappen om te installeren en configureren van het hulpprogramma achterzijde volgen.

Voor het installeren van de **achterzijde** back-up maken van pakketten, gebruikt u de volgende opdrachten:

Voor **SLES** besturingssysteem, gebruik de volgende opdracht:
```
#zypper install <rear rpm package>
```
Voor **RHEL** besturingssysteem, gebruik de volgende opdracht: 
```
#yum install rear -y
```
Voor het configureren van het hulpprogramma achterzijde, moet u parameters bijwerken **OUTPUT_URL** en **BACKUP_URL** in de */etc/rear/local.conf bestand*.
```
OUTPUT=ISO
ISO_MKISOFS_BIN=/usr/bin/ebiso
BACKUP=NETFS
OUTPUT_URL="nfs://nfsip/nfspath/"
BACKUP_URL="nfs://nfsip/nfspath/"
BACKUP_OPTIONS="nfsvers=4,nolock"
NETFS_KEEP_OLD_BACKUP_COPY=
EXCLUDE_VG=( vgHANA-data-HC2 vgHANA-data-HC3 vgHANA-log-HC2 vgHANA-log-HC3 vgHANA-shared-HC2 vgHANA-shared-HC3 )
BACKUP_PROG_EXCLUDE=("${BACKUP_PROG_EXCLUDE[@]}" '/media' '/var/tmp/*' '/var/crash' '/hana' '/usr/sap'  ‘/proc’)
```

De volgende schermafbeelding ziet u het terugzetten van een volledige back-up: ![RearToolConfiguration.PNG](media/HowToHLI/OSBackupTypeIISKUs/RearToolConfiguration.PNG)
