---
title: Bestanden voor Bash in de Azure-Cloud-Shell behouden | Microsoft Docs
description: Overzicht van hoe Bash in de Azure-Cloud-Shell zich blijft bestanden voordoen.
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: a04342f77126afe1e0a2707f1d213e0a8eb5825d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-bash-in-cloud-shell-storage-works"></a>De werking van Bash in de Cloud Shell-opslag 
Bash in de Cloud-Shell aanhoudt bestanden via beide van de volgende methoden: 
* Maken van de installatiekopie van een schijf van uw `$Home` directory voor het persistent maken van alle inhoud in de map. Installatiekopie van de schijf wordt opgeslagen in de opgegeven bestandsshare als `acc_<User>.img` op `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, en worden wijzigingen automatisch gesynchroniseerd. 
* Koppelen van de opgegeven bestandsshare als `clouddrive` in uw `$Home` map voor directe bestandsshare interactie. `/Home/<User>/clouddrive`is toegewezen aan `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Alle bestanden in uw `$Home` directory, zoals SSH-sleutels worden doorgevoerd in uw schijfimage gebruiker die is opgeslagen in de gekoppelde bestandsshare. Aanbevolen procedures van toepassing wanneer u deze persistent maken informatie in uw `$Home` directory en gekoppelde bestandsshare.

## <a name="use-the-clouddrive-command"></a>Gebruik de `clouddrive` opdracht
Met Bash in de Cloud-Shell, kunt u een opdracht uitvoeren `clouddrive`, waarmee u handmatig bijwerken van de bestandsshare die is gekoppeld aan Cloud Shell.
![De opdracht 'clouddrive' uit te voeren](media/persisting-shell-storage/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Een nieuwe clouddrive koppelen

### <a name="prerequisites-for-manual-mounting"></a>Vereisten voor de handmatige koppelen
U kunt de bestandsshare die is gekoppeld aan Cloud-Shell met behulp van bijwerken de `clouddrive mount` opdracht.

Als u een bestaande bestandsshare koppelt, moet de storage-accounts:
* Lokaal redundante opslag of geografisch redundante opslag voor de ondersteuning van bestandsshares.
* Bevindt zich in uw regio toegewezen. Als u de voorbereiding, de regio die u zijn toegewezen aan wordt vermeld in de naam van de resourcegroep `cloud-shell-storage-<region>`.

### <a name="the-clouddrive-mount-command"></a>De opdracht clouddrive koppelen

> [!NOTE]
> Als u een nieuwe bestandsshare koppelen wilt, een nieuwe gebruikersinstallatiekopie is gemaakt voor uw `$Home` directory. Uw vorige `$Home` installatiekopie wordt opgeslagen in de vorige bestandsshare.

Voer de `clouddrive mount` opdracht met de volgende parameters:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Uitvoeren als u meer details, `clouddrive mount -h`, zoals hier wordt weergegeven:

![Met de ' clouddrive mount'command](media/persisting-shell-storage/mount-h.png)

## <a name="unmount-clouddrive"></a>Clouddrive ontkoppelen
U kunt een bestandsshare die gekoppeld aan Cloud-Shell op elk gewenst moment ontkoppelen. Omdat Cloud Shell een gekoppelde bestandsshare die vereist moet worden gebruikt, wordt u gevraagd om te maken en koppelen van een andere bestandsshare op de volgende sessie.

1. Voer `clouddrive unmount` uit.
2. Erkent en wordt u gevraagd te bevestigen.

De bestandsshare blijft bestaan, tenzij u deze handmatig verwijderen. Cloud-Shell zoekt op de volgende sessies niet meer naar deze bestandsshare. Uitvoeren als u meer details, `clouddrive unmount -h`, zoals hier wordt weergegeven:

![Met de ' clouddrive unmount'command](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Hoewel deze opdracht uit te voeren wordt niet alle resources moet verwijderen, handmatig verwijderen van een resourcegroep, de storage-account of bestandsshare die toegewezen aan Cloud Shell worden gewist door uw `$Home` directory schijfimage en alle bestanden in de bestandsshare. Deze actie kan niet ongedaan worden gemaakt.

## <a name="list-clouddrive"></a>Lijst`clouddrive`
Om te ontdekken welke bestandsshare is gekoppeld als `clouddrive`, voert de `df` opdracht. 

Het pad naar clouddrive ziet dat de naam van het opslagaccount en bestandsshare in de URL. Bijvoorbeeld: `//storageaccountname.file.core.windows.net/filesharename`

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

[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Volgende stappen
[In de Cloud Shell snel Bash](quickstart.md) <br>
[Meer informatie over Microsoft Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Meer informatie over opslag labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
