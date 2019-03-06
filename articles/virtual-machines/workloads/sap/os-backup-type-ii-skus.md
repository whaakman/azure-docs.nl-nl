---
title: Besturingssysteem back-up en herstel van SAP HANA op Azure (grote instanties) Typ II-SKU's | Microsoft Docs
description: Operatign systeemback-up en herstel voor SAP HANA op Azure (grote instanties) Type II-SKU's uitvoeren
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
ms.openlocfilehash: c82c5c74fe13bad99528486be69089df5f477457
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57436337"
---
# <a name="os-backup-and-restore-for-type-ii-skus"></a>OS-back-up en herstel voor SKU's Type II

Dit document beschrijft de stappen voor het uitvoeren van een besturingssysteem op back-up en herstel voor de **SKU's Type II** van de HANA grote instanties. 

>[!NOTE]
>De back-OS-scripts maakt gebruik van de achterzijde-software, die vooraf is geïnstalleerd op de server.  

Nadat het inrichten voltooid door het Microsoft-Service Management-team, standaard is, de server is geconfigureerd met twee back-ups plannen voor back-up van het bestandssysteem niveau back-up van het besturingssysteem. U kunt het schema van de back-uptaak controleren met behulp van de volgende opdracht uit:
```
#crontab –l
```
U kunt de back-upschema op elk gewenst moment met de volgende opdracht:
```
#crontab -e
```
## <a name="how-to-take-a-manual-backup"></a>Hoe kan ik een handmatige back-up maken?

De back-up van besturingssysteem bestand system is gepland met een **cron-taak** al. U kunt echter de besturingssysteem-bestand op back-up ook handmatig uitvoeren. Als u een handmatige back-up, voer de volgende opdracht:

```
#rear -v mkbackup
```
Het volgende scherm ziet toont een voorbeeld voor een handmatige back-up:

![Hoe](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


## <a name="how-to-restore-a-backup"></a>Hoe kunt u een back-up herstellen?

U kunt een volledige back-up of een afzonderlijk bestand herstellen vanuit de back-up. Als u wilt herstellen, gebruikt u de volgende opdracht uit:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
Het bestand is hersteld na het herstel in de huidige werkmap.

De volgende opdracht toont het terugzetten van een bestand */etc/fstabfrom* het back-upbestand *backup.tar.gz*
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>U moet het bestand kopiëren naar de gewenste locatie nadat deze is hersteld van de back-up.

De volgende schermafbeelding ziet u het terugzetten van een volledige back-up:

![HowtoRestoreaBackup.PNG](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

## <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Hoe kunt u het hulpprogramma achterzijde installeren en de configuratie wijzigen? 

De pakketten Relax-en-herstel (achter) zijn **vooraf geïnstalleerd** in de **SKU's Type II** van HANA grote instanties, en er is geen actie te ondernemen. U kunt rechtstreeks starten met behulp van de achterzijde van de back-up van het besturingssysteem.
In de omstandigheden waarin u wilt installeren van de pakketten in uw eigen, kunt u echter de vermelde stappen voor het installeren en configureren van het hulpprogramma achterzijde volgen.

Voor het installeren van de **achterzijde** back-up maken van pakketten, gebruikt u de volgende opdrachten:

Voor **SLES** besturingssysteem, gebruikt u de volgende opdracht:
```
#zypper install <rear rpm package>
```
Voor **RHEL** besturingssysteem, gebruikt u de volgende opdracht: 
```
#yum install rear -y
```
Voor het configureren van het hulpprogramma achter, moet u parameters bijwerken **OUTPUT_URL** en **BACKUP_URL** in de */etc/rear/local.conf bestand*.
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
