---
title: Behouden bestanden voor Bash in Azure Cloud Shell | Microsoft Docs
description: Overzicht van hoe bestanden in Bash in Azure Cloud Shell blijft bestaan.
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: juluk
ms.openlocfilehash: 4671287d78dafce9b662a167ba99a680d2fa70d2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734072"
---
[!INCLUDE [PersistingStorage-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-storage-works"></a>De werking van Cloud Shell-storage 
Cloudshell zich blijft voordoen bestanden via beide van de volgende methoden: 
* Het maken van de installatiekopie van een schijf van uw `$Home` directory om vast te leggen van alle inhoud in de map. De installatiekopie van de schijf wordt opgeslagen in de opgegeven bestandsshare als `acc_<User>.img` op `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, en deze wijzigingen automatisch gesynchroniseerd. 
* Koppelen van de opgegeven bestandsshare als `clouddrive` in uw `$Home` Active directory voor interactie met de rechtstreekse bestandsshare. `/Home/<User>/clouddrive` is toegewezen aan `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Alle bestanden in uw `$Home` directory, zoals SSH-sleutels zijn opgeslagen in de schijfinstallatiekopie van de gebruiker die is opgeslagen in de gekoppelde bestandsshare. Aanbevolen procedures van toepassing als u informatie in uw `$Home` directory en gekoppelde-bestandsshare.

## <a name="bash-specific-commands"></a>Bash-specifieke opdrachten

### <a name="use-the-clouddrive-command"></a>Gebruik de `clouddrive` opdracht
Met Bash in Cloud Shell, kunt u een opdracht uitvoeren `clouddrive`, waarmee u kunt handmatig bijwerken van de bestandsshare die is gekoppeld aan Cloud Shell.
![De opdracht "clouddrive"](media/persisting-shell-storage/clouddrive-h.png)

### <a name="mount-a-new-clouddrive"></a>Een nieuwe clouddrive koppelen

#### <a name="prerequisites-for-manual-mounting"></a>Vereisten voor handmatige koppelen
U kunt de bestandsshare die is gekoppeld aan Cloud Shell met behulp van bijwerken de `clouddrive mount` opdracht.

Als u een bestaande bestandsshare koppelt, moeten u de storage-accounts zich in uw Selecteer Cloud Shell-regio. De locatie ophalen door het uitvoeren van `env` van Bash en controleren van de `ACC_LOCATION`.

#### <a name="the-clouddrive-mount-command"></a>De `clouddrive mount` opdracht

> [!NOTE]
> Als u een nieuwe bestandsshare koppelen wilt, een nieuwe gebruikersinstallatiekopie is gemaakt voor uw `$Home` directory. Uw vorige `$Home` installatiekopie wordt opgeslagen in de vorige bestandsshare.

Voer de `clouddrive mount` opdracht met de volgende parameters:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Uitvoeren als u meer details, `clouddrive mount -h`, zoals hier wordt weergegeven:

![Met de ' clouddrive mount'command](media/persisting-shell-storage/mount-h.png)

### <a name="unmount-clouddrive"></a>Clouddrive ontkoppelen
U kunt een bestandsshare die gekoppeld aan Cloud Shell op elk gewenst moment ontkoppelen. Nadat u Cloud Shell is vereist voor een gekoppelde bestandsshare die moet worden gebruikt, wordt u gevraagd om te maken en koppelen van een andere bestandsshare in de volgende sessie.

1. Voer `clouddrive unmount` uit.
2. Bevestigen en bevestigt u wordt gevraagd.

De bestandsshare blijft bestaan, tenzij u deze handmatig verwijderen. Cloudshell wordt niet meer voor deze bestandsshare zoeken op de volgende sessies. Uitvoeren als u meer details, `clouddrive unmount -h`, zoals hier wordt weergegeven:

![Met de ' clouddrive unmount'command](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Hoewel deze opdracht uitvoert, alle resources niet verwijderen wordt, gewist handmatig verwijderen van een resourcegroep, de storage-account of de bestandsshare die toegewezen aan Cloud Shell de `$Home` schijfinstallatiekopie van de map en alle bestanden in de bestandsshare. Deze actie kan niet ongedaan worden gemaakt.

### <a name="list-clouddrive"></a>Lijst met `clouddrive`
Om te ontdekken welke-bestandsshare is gekoppeld als `clouddrive`, voert de `df` opdracht. 

Het pad naar clouddrive ziet u dat de storage-accountnaam en het bestand delen in de URL. Bijvoorbeeld: `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                          1K-blocks   Used  Available Use% Mounted on
overlay                                             29711408 5577940   24117084  19% /
tmpfs                                                 986716       0     986716   0% /dev
tmpfs                                                 986716       0     986716   0% /sys/fs/cgroup
/dev/sda1                                           29711408 5577940   24117084  19% /etc/hosts
shm                                                    65536       0      65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName 5368709120    64 5368709056   1% /home/justin/clouddrive
justin@Azure:~$
```
## <a name="powershell-specific-commands"></a>PowerShell-specifieke opdrachten

### <a name="list-clouddrive-azure-file-shares"></a>Lijst met `clouddrive` Azure-bestandsshares
De `Get-CloudDrive` cmdlet haalt de gegevens voor het delen van Azure-bestand dat momenteel is gekoppeld met de `clouddrive` in de Cloud Shell. <br>
![Get-CloudDrive uitgevoerd](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

### <a name="unmount-clouddrive"></a>Ontkoppelen `clouddrive`
U kunt een Azure-bestandsshare die gekoppeld aan Cloud Shell op elk gewenst moment ontkoppelen. Als de Azure-bestandsshare is verwijderd, wordt u gevraagd om te maken en een nieuwe Azure-bestandsshare koppelen op de volgende sessie.

De `Dismount-CloudDrive` cmdlet wordt een Azure-bestandsshare van het huidige opslagaccount. Ontkoppelen van de `clouddrive` de huidige sessie wordt beëindigd. De gebruiker wordt gevraagd om te maken en een nieuwe Azure-bestandsshare koppelen tijdens de volgende sessie.
![Actieve Dismount-CloudDrive](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [PersistingStorage-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Volgende stappen
[Bash in Cloud Shell-snelstartgids](quickstart.md) <br>
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md) <br>
[Meer informatie over Microsoft Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction) <br>
[Meer informatie over de storage-tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
