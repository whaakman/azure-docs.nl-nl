---
title: Gebruik van SSH-sleutels met Windows voor virtuele Linux-machines | Microsoft Docs
description: Informatie over het genereren en gebruiken van SSH-sleutels op een Windows-computer verbinding maken met een virtuele Linux-machine in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: danlep
ms.openlocfilehash: d0762f80267fa927681344a3e0de78b0800c8306
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Het gebruik van SSH-sleutels met Windows in Azure

Dit artikel bevat manieren om te genereren en het gebruik van secure shell (SSH) sleutels op een Windows-computer maken en verbinding maken met een Linux virtuele machine (VM) in Azure. Voor het gebruik van SSH-sleutels van een Linux- of Mac OS-client, Zie de [snelle](mac-create-ssh-keys.md) of [gedetailleerde](create-ssh-keys-detailed.md) richtlijnen.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows-pakketten en SSH-clients
U verbinding maken met en beheren van virtuele Linux-machines in Azure worden verkregen met een *SSH client*. Computers met Linux- of Mac OS meestal is een suite met SSH opdrachten te genereren en SSH-sleutels beheren en te maken van SSH-verbindingen. 

Windows-computers altijd geen vergelijkbare SSH opdrachten geïnstalleerd. Versies van Windows 10 met de [Windows-subsysteem voor Linux](https://docs.microsoft.com/windows/wsl/about) kunt u uitvoeren en toegang tot hulpprogramma's zoals een SSH-client systeemeigen binnen een Bash-shell. 

Als u gebruikmaken van iets anders dan wilt voor Windows Bash, algemene Windows SSH clients die kunt u lokaal installeren zijn opgenomen in de volgende pakketten:

* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [GIT voor Windows](https://git-for-windows.github.io/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Een andere optie is het gebruik van de SSH-hulpprogramma's beschikbaar zijn in Bash in de [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Cloud-Shell openen in uw webbrowser op [ https://shell.azure.com ](https://shell.azure.com) of in de [Azure-portal](https://portal.azure.com). 
* Toegang krijgen tot Cloud-Shell als terminal uit in Visual Studio Code door het installeren van de [Azure-Account extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
Deze sectie vindt u twee opties voor het maken van een SSH-sleutelpaar in Windows.

### <a name="create-ssh-keys-with-ssh-keygen"></a>SSH-sleutels maken met de ssh-keygen

Als u een opdrachtshell zoals Bash voor Windows of GitBash (of Bash in de Azure-Cloud-Shell) uitvoeren kunt, maakt u een SSH-sleutelpaar met behulp van de `ssh-keygen` opdracht. Typ de volgende opdracht en de vragen te beantwoorden. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven. 

```bash
ssh-keygen -t rsa -b 2048
```

Zie voor meer informatie over de achtergrond en informatie de [snelle](mac-create-ssh-keys.md) of [gedetailleerde](create-ssh-keys-detailed.md) stappen voor het maken van de sleutels met `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>SSH-sleutels met PuTTYgen maken

Als u liever een GUI-hulpprogramma gebruiken om te maken van SSH-sleutels, kunt u de codegenerator PuTTYgen, die deel uitmaakt van de [PuTTY downloadpakket](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Een SSH-RSA-sleutelpaar met PuTTYgen maken:

1. PuTTYgen starten.

2. Klik op **genereren**. Standaard genereert PuTTYgen een SSH-2 RSA 2048-bits sleutel.

4. De muis over de leeg gebied sommige aanvraaggrootte voor de sleutel te genereren.

5. Nadat de openbare sleutel wordt gegenereerd, eventueel invoeren en een wachtwoordzin bevestigen. U wordt gevraagd de wachtwoordzin voor wanneer u naar de virtuele machine met uw SSH-sleutel verifiëren. Zonder een wachtwoordzin als iemand anders uw persoonlijke sleutel, krijgt kunnen deze aanmelden bij een virtuele machine of service die gebruikmaakt van die sleutel. U wordt aangeraden maken van een wachtwoordzin. Als u de wachtwoordzin vergeet, is er echter geen manier om deze te herstellen.

6. De openbare sleutel wordt aan de bovenkant van het venster weergegeven. U kopieert en plakt u de openbare sleutel van deze indeling van één regel in de Azure-portal of een Azure Resource Manager-sjabloon bij het maken van een Linux-VM. U kunt ook klikken op **openbare sleutel opslaan** een kopie wilt opslaan op uw computer:

    ![PuTTY bestand voor de openbare sleutel opslaan](./media/ssh-from-windows/save-public-key.png)

7. Klik desgewenst op voor het opslaan van de persoonlijke sleutel in de PuTTy persoonlijke sleutel-indeling (ppk-bestand), **persoonlijke sleutel opslaan**. U moet het ppk-bestand van u PuTTY later gebruiken om een SSH-verbinding maken met de virtuele machine wilt maken.

    ![PuTTY persoonlijke sleutelbestand opslaan](./media/ssh-from-windows/save-ppk-file.png)

    Als u wilt dat de persoonlijke sleutel opslaan in de OpenSSH-indeling, de persoonlijke sleutel indeling die wordt gebruikt door veel clients SSH, klikt u op **conversies** > **exporteren OpenSSH-sleutel**.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Openbare SSH-sleutel bieden bij het implementeren van een virtuele machine

Voor het maken van een Linux VM die gebruikmaakt van SSH-sleutels voor verificatie, Geef uw openbare SSH-sleutel bij het maken van de virtuele machine via de Azure portal of andere methoden.

Het volgende voorbeeld ziet hoe u Kopieer en plak deze openbare sleutel in de Azure-portal bij het maken van een Linux-VM. De openbare sleutel vervolgens doorgaans wordt opgeslagen in `~/.ssh/authorized_keys` op de nieuwe virtuele machine.

   ![Openbare sleutel wordt gebruikt bij het maken van een virtuele machine in de Azure portal](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Verbinding maken met uw virtuele machine

Als u een SSH-verbinding voor uw Linux-VM van Windows is het gebruik van een SSH-client. Dit is de aanbevolen methode als u een SSH-client op uw Windows-systeem is geïnstalleerd, of u SSH-hulpprogramma's in Bash in de Azure-Cloud-Shell gebruiken. Als u liever een GUI-hulpprogramma, kunt u met PuTTY verbinding maken.  

### <a name="use-an-ssh-client"></a>Een SSH-client gebruiken
Met de openbare sleutel geïmplementeerd op de virtuele machine in Azure, en de persoonlijke sleutel op het lokale systeem, SSH met uw virtuele machine met behulp van de IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht met de naam van de administrator-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u een wachtwoordzin geconfigureerd wanneer u uw sleutelpaar gemaakt, voert u de wachtwoordzin in wanneer u wordt gevraagd tijdens het aanmeldingsproces.

### <a name="connect-with-putty"></a>Verbinding maken met PuTTY

Als u hebt geïnstalleerd het [PuTTY downloadpakket](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) en eerder gegenereerde PuTTY persoonlijke sleutel (ppk-bestand), u kunt verbinding maken met de Linux-VM met PuTTY.

1. PuTTy starten.

2. Vul de hostnaam of IP-adres van uw virtuele machine van de Azure-portal:

    ![Nieuwe PuTTY verbinding openen](./media/ssh-from-windows/putty-new-connection.png)

3. Voordat u selecteert **Open**, klikt u op **verbinding** > **SSH** > **Auth** tabblad. Blader naar en selecteer uw PuTTY persoonlijke sleutel (ppk-bestand):

    ![Selecteer uw PuTTY persoonlijke sleutel voor verificatie](./media/ssh-from-windows/putty-auth-dialog.png)

4. Klik op **Open** verbinding maken met uw virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Zie voor gedetailleerde stappen, opties en geavanceerde voorbeelden van het werken met SSH-sleutels, [gedetailleerde stappen voor het maken van SSH-sleutel paren](create-ssh-keys-detailed.md).

* U kunt ook PowerShell gebruiken in de Azure-Cloud-Shell SSH-sleutels genereren en SSH-verbinding maken met een virtuele Linux-machines. Zie de [Quick Start PowerShell](../../cloud-shell/quickstart-powershell.md#ssh).

* Als u problemen ondervindt bij het gebruik van SSH verbinding maken met uw virtuele Linux-machines, Zie [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
