---
title: Opnieuw instellen van wachtwoord van de Linux-VM en SSH-sleutel van de CLI | Microsoft Docs
description: Het gebruik van de VMAccess-extensie van de Azure-opdrachtregelinterface (CLI) op een Linux-VM wachtwoord of SSH-sleutel opnieuw instellen en controleren van de consistentie van de schijf los van de SSH-configuratie
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: d975eb70-5ff1-40d1-a634-8dd2646dcd17
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cynthn
ms.openlocfilehash: c36498d2f4fef506dc7047fe91666aceec73c13d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Het opnieuw instellen van een Linux-VM wachtwoord of SSH-sleutel, los van de SSH-configuratie en consistentie van de schijf met behulp van de VMAccess-extensie controleren
Als u geen verbinding met een virtuele Linux-machine in Azure vanwege een vergeten wachtwoord, een onjuiste Secure Shell (SSH)-sleutel, of een probleem met de SSH-configuratie gebruiken de VMAccessForLinux-extensie met de Azure CLI opnieuw instellen van het wachtwoord of SSH-sleutel, los van de SSH-configuratie en controleren van de consistentie van de schijf. 

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Met de Azure CLI, gebruikt u de **azure vm-extensie set** opdracht van de opdrachtregelinterface (Bash, Terminal-opdrachtprompt) om opdrachten te openen. Voer **help azure vm-extensie set** voor het gebruik van gedetailleerde extensie.

Met de Azure CLI kunt u de volgende taken uitvoeren:

* [Het wachtwoord opnieuw instellen](#pwresetcli)
* [De SSH-sleutel opnieuw instellen](#sshkeyresetcli)
* [Het wachtwoord en de SSH-sleutel opnieuw instellen](#resetbothcli)
* [Een nieuw sudo-gebruikersaccount maken](#createnewsudocli)
* [De SSH-configuratie opnieuw instellen](#sshconfigresetcli)
* [Een gebruiker verwijderen](#deletecli)
* [De status van de VMAccess-extensie weergeven](#statuscli)
* [Controleer de consistentie van toegevoegde schijven](#checkdisk)
* [Herstellen van toegevoegde schijven op uw Linux-VM](#repairdisk)

## <a name="prerequisites"></a>Vereisten
U moet het volgende doen:

* U moet [Azure CLI installeren](../../../cli-install-nodejs.md) en [verbinding maken met uw abonnement](/cli/azure/authenticate-azure-cli) Azure resources die zijn gekoppeld aan uw account te gebruiken.
* Stel de juiste modus voor het klassieke implementatiemodel door de volgende opdracht bij de opdrachtprompt te typen:
    ``` 
        azure config mode asm
    ```
* Een nieuw wachtwoord of SSH-sleutels hebben als u wilt instellen, ofwel een. U kunt deze niet nodig als u wilt de SSH-configuratie opnieuw instellen.

## <a name="pwresetcli"></a>Het wachtwoord opnieuw instellen
1. Maak een bestand op uw lokale computer met de naam PrivateConf.json met deze regels. Vervang **gebruik met Windows** en **myP@ssW0rd** met uw eigen gebruikersnaam en wachtwoord en stel uw eigen datum voor verlopen.

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. Voer deze opdracht uit, vervangen door de naam van uw virtuele machine voor **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <a name="sshkeyresetcli"></a>De SSH-sleutel opnieuw instellen
1. Maak een bestand met de naam PrivateConf.json met deze inhoud. Vervang de **gebruik met Windows** en **mySSHKey** waarden door uw eigen waarden.

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. Voer deze opdracht uit, vervangen door de naam van uw virtuele machine voor **myVM**.
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Zowel het wachtwoord en de SSH-sleutel opnieuw instellen
1. Maak een bestand met de naam PrivateConf.json met deze inhoud. Vervang de **gebruik met Windows**, **mySSHKey** en **myP@ssW0rd** waarden door uw eigen waarden.

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. Voer deze opdracht uit, vervangen door de naam van uw virtuele machine voor **myVM**.

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="createnewsudocli"></a>Een nieuw sudo-gebruikersaccount maken

Als u uw gebruikersnaam vergeet, kunt u vmaccess gebruikt voor het maken van een nieuw bestand met de sudo-instantie. In dit geval wordt worden de bestaande gebruikersnaam en wachtwoord niet gewijzigd.

Maakt een nieuwe sudo-gebruiker met wachtwoord toegang kunnen krijgen, gebruikt u het script in [wachtwoordherstel](#pwresetcli) en geef de naam van de nieuwe gebruiker.

Maakt een nieuwe sudo-gebruiker met toegang tot de sleutel van de SSH, gebruikt u het script in [opnieuw instellen van de SSH-sleutel](#sshkeyresetcli) en geef de naam van de nieuwe gebruiker.

U kunt ook [opnieuw instellen van het wachtwoord en de SSH-sleutel](#resetbothcli) voor het maken van een nieuwe gebruiker met wachtwoord en toegang tot de sleutel van SSH.

## <a name="sshconfigresetcli"></a>De SSH-configuratie opnieuw instellen
Als de SSH-configuratie in een ongewenste status, kunt u ook toegang verliezen naar de virtuele machine. De configuratie opnieuw instellen op de standaardstatus kunt u de VMAccess-extensie. Om dit te doen, moet u alleen de sleutel 'reset_ssh' op 'True' ingesteld. De uitbreiding wordt de SSH-server opnieuw opstarten, opent u de SSH-poort op de virtuele machine en de SSH-configuratie opnieuw instellen naar standaardwaarden. Het gebruikersaccount (naam, wachtwoord of SSH-sleutels) worden niet gewijzigd.

> [!NOTE]
> /Etc/ssh/sshd_config bevindt het SSH-configuratiebestand die opnieuw wordt ingesteld.
> 
> 

1. Maak een bestand met de naam PrivateConf.json met deze inhoud.

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. Voer deze opdracht uit, vervangen door de naam van uw virtuele machine voor **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="deletecli"></a>Een gebruiker verwijderen
Als u verwijderen van een gebruikersaccount zonder aan te melden bij naar de virtuele machine rechtstreeks wilt, kunt u dit script gebruiken.

1. Maak een bestand met de naam PrivateConf.json met deze inhoud, vervangen door de naam van de gebruiker om te verwijderen voor **removeUserName**. 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. Voer deze opdracht uit, vervangen door de naam van uw virtuele machine voor **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="statuscli"></a>De status van de VMAccess-extensie weergeven
Als u wilt weergeven van de status van de VMAccess-extensie, moet u deze opdracht uitvoeren.

```
        azure vm extension get
```

## <a name='checkdisk'></a>Controleer de consistentie van toegevoegde schijven
Fsck op alle schijven in uw virtuele Linux-machine uitgevoerd, moet u het volgende doen:

1. Maak een bestand met de naam PublicConf.json met deze inhoud. Controleer de dat schijf wordt een Booleaanse waarde of om te controleren van de schijven die aan de virtuele machine is gekoppeld of niet. 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. Voer deze opdracht worden uitgevoerd, vervangen door de naam van uw virtuele machine voor **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <a name='repairdisk'></a>Herstellen van schijven
Als u wilt herstellen op schijven die u niet wilt koppelen of configuratiefouten van de mount hebben, met de VMAccess-extensie opnieuw instellen van de mount-configuratie op uw virtuele Linux-machine. Vervangen door de naam van de schijf voor **myDisk**.

1. Maak een bestand met de naam PublicConf.json met deze inhoud. 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. Voer deze opdracht worden uitgevoerd, vervangen door de naam van uw virtuele machine voor **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a>Volgende stappen
* Als u wilt opnieuw instellen van het wachtwoord of SSH-sleutel met Azure PowerShell-cmdlets of Azure Resource Manager-sjablonen, los het SSH-configuratie en controle van schijf consistentie, Zie de [VMAccess-extensie-documentatie op GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 
* U kunt ook de [Azure-portal](https://portal.azure.com) opnieuw instellen van het wachtwoord of SSH-sleutel van een Linux-VM die is geïmplementeerd in het klassieke implementatiemodel. U kunt de portal komen momenteel niet gebruiken voor dit voor een Linux-VM die is geïmplementeerd in het Resource Manager-implementatiemodel.
* Zie [over functies en uitbreidingen van de virtuele machine](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over het gebruik van VM-extensies voor Azure virtual machines.

