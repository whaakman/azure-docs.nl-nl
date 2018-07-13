---
title: SSH-sleutels gebruiken met Windows voor virtuele Linux-machines | Microsoft Docs
description: Informatie over het genereren en het gebruik van SSH-sleutels op een Windows-computer verbinding maken met een virtuele Linux-machine in Azure.
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38630208"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>SSH-sleutels gebruiken met Windows op Azure

Dit artikel bevat manieren om te genereren en gebruiken van SSH (secure shell) sleutels op een Windows-computer om te maken en verbinding maken met een Linux virtuele machine (VM) in Azure. Zie voor het gebruik van SSH-sleutels van een client voor Linux of macOS, de [snelle](mac-create-ssh-keys.md) of [gedetailleerde](create-ssh-keys-detailed.md) richtlijnen.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows-pakketten en SSH-clients
U verbinding maken met en beheren van virtuele Linux-machines in Azure met een *SSH-client*. Computers met Linux of macOS meestal hebben een reeks opdrachten SSH te genereren en SSH-sleutels beheren en te maken van SSH-verbindingen. 

Windows-computers altijd geen vergelijkbare SSH opdrachten die zijn geïnstalleerd. Windows 10-versies met de [Windows-subsysteem voor Linux](https://docs.microsoft.com/windows/wsl/about) kunt u uitvoeren en toegang tot hulpprogramma's zoals een SSH-client systeemeigen binnen een Bash-shell. 

Als u gebruiken dan Bash wilt voor Windows, algemene Windows SSH clients die kunt u lokaal installeren zijn opgenomen in de volgende pakketten:

* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [GIT voor Windows](https://git-for-windows.github.io/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Een andere optie is het gebruik van de SSH-hulpprogramma's beschikbaar in Bash in de [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Toegang tot Cloud Shell in uw webbrowser op [ https://shell.azure.com ](https://shell.azure.com) of in de [Azure-portal](https://portal.azure.com). 
* Toegang krijgen tot Cloud Shell als terminal in Visual Studio Code door het installeren van de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
Deze sectie ziet u twee opties voor het maken van een SSH-sleutelpaar in Windows.

### <a name="create-ssh-keys-with-ssh-keygen"></a>SSH-sleutels maken met ssh-keygen

Als u een opdrachtshell zoals Bash voor Windows of GitBash (of Bash in Azure Cloud Shell) uitvoeren kunt, maakt u een SSH-sleutelpaar gebruiken de `ssh-keygen` opdracht. Typ de volgende opdracht en de hierom wordt gevraagd. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven. 

```bash
ssh-keygen -t rsa -b 2048
```

Zie voor meer achtergrondinformatie en informatie, de [snelle](mac-create-ssh-keys.md) of [gedetailleerde](create-ssh-keys-detailed.md) stappen voor het maken van de sleutels met `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>SSH-sleutels met PuTTYgen maken

Als u liever een GUI-hulpprogramma gebruiken om te maken van SSH-sleutels, kunt u de serversleutelgenerator PuTTYgen, opgenomen in de [PuTTY downloadpakket](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Een SSH-RSA-sleutelpaar maken met PuTTYgen:

1. Start PuTTYgen.

2. Klik op **genereren**. Standaard genereert PuTTYgen een SSH-2 RSA 2048-bits sleutel.

4. Met de muisaanwijzer de lege ruimte voor het genereren van sommige aanvraaggrootte voor de sleutel.

5. (Optioneel) Voer nadat de openbare sleutel is gegenereerd, en een wachtwoordzin bevestigen. U wordt gevraagd naar de wachtwoordzin wanneer u met de virtuele machine met uw SSH-sleutel verifieert. Zonder een wachtwoordzin als iemand anders uw privésleutel verkrijgt kunnen ze aanmelden bij een virtuele machine of service die gebruikmaakt van deze sleutel. U wordt aangeraden dat u een wachtwoordzin maken. Als u de wachtwoordzin vergeet, is er echter geen manier om deze te herstellen.

6. De openbare sleutel wordt aan de bovenkant van het venster weergegeven. U kopiëren en plakken van de openbare sleutel van deze indeling met één regel in de Azure-portal of een Azure Resource Manager-sjabloon bij het maken van een Linux-VM. U kunt ook klikken op **openbare sleutel opslaan** een kopie wilt opslaan op uw computer:

    ![PuTTY bestand voor de openbare sleutel opslaan](./media/ssh-from-windows/save-public-key.png)

7. (Optioneel) om de persoonlijke sleutel in de PuTTy persoonlijke sleutel indeling (ppk-bestand) hebt opgeslagen, klikt u op **persoonlijke sleutel opslaan**. U moet het ppk-bestand van u PuTTY later gebruiken om een SSH-verbinding maken met de virtuele machine wilt maken.

    ![PuTTY bestand met persoonlijke sleutel opslaan](./media/ssh-from-windows/save-ppk-file.png)

    Als u wilt de persoonlijke sleutel opslaan in de OpenSSH-indeling, de persoonlijke sleutel indeling die wordt gebruikt door veel SSH-clients, klikt u op **conversies** > **exporteren OpenSSH-sleutel**.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Openbare SSH-sleutel opgeven bij het implementeren van een virtuele machine

Voor het maken van een Linux-VM die gebruikmaakt van SSH-sleutels voor verificatie, Geef uw openbare SSH-sleutel bij het maken van de virtuele machine met behulp van de Azure-portal of andere methoden.

Het volgende voorbeeld laat zien hoe u wilt kopiëren en plakken van deze openbare sleutel in Azure portal, bij het maken van een Linux-VM. De openbare sleutel worden meestal vervolgens opgeslagen in `~/.ssh/authorized_keys` op uw nieuwe virtuele machine.

   ![Openbare sleutel wordt gebruikt bij het maken van een VM in Azure portal](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Verbinding maken met uw virtuele machine

Eén manier om u te maken van een SSH-verbinding met uw Linux-VM van Windows is het gebruik van een SSH-client. Dit is de aanbevolen methode als u een SSH-client geïnstalleerd op uw Windows-systeem, of u bij het gebruik van SSH-hulpprogramma's in Bash in Azure Cloud Shell. Als u liever een GUI-hulpprogramma, kunt u verbinding maken met PuTTY.  

### <a name="use-an-ssh-client"></a>Een SSH-client gebruiken
Met de openbare sleutel die is geïmplementeerd op uw Azure-VM en de persoonlijke sleutel op uw lokale systeem, SSH verbinding met uw virtuele machine via het IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht uit met de naam van de beheerder-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt geconfigureerd, moet u de wachtwoordzin tijdens het aanmeldingsproces desgevraagd invoeren.

### <a name="connect-with-putty"></a>Verbinding maken met PuTTY

Als u hebt geïnstalleerd het [PuTTY downloadpakket](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) en eerder gegenereerde PuTTY persoonlijke sleutel (ppk-bestand), u kunt verbinding maken met de Linux-VM met PuTTY.

1. Start PuTTy.

2. Vul in de hostnaam of IP-adres van uw virtuele machine vanuit Azure portal:

    ![Nieuwe PuTTY verbinding openen](./media/ssh-from-windows/putty-new-connection.png)

3. Voordat u selecteert **Open**, klikt u op **verbinding** > **SSH** > **Auth** tabblad. Blader naar en selecteer uw PuTTY persoonlijke sleutel (ppk-bestand):

    ![Selecteer uw PuTTY persoonlijke sleutel voor verificatie](./media/ssh-from-windows/putty-auth-dialog.png)

4. Klik op **Open** verbinding maken met uw virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Zie voor gedetailleerde stappen, opties en geavanceerde voorbeelden van het werken met SSH-sleutels, [gedetailleerde stappen voor het maken van SSH-sleutelparen](create-ssh-keys-detailed.md).

* U kunt ook PowerShell gebruiken in Azure Cloud Shell voor het SSH-sleutels genereren en SSH-verbinding maken met een virtuele Linux-machines. Zie de [PowerShell-snelstartgids](../../cloud-shell/quickstart-powershell.md#ssh).

* Als u problemen ondervindt bij het gebruik van SSH verbinding maken met uw virtuele Linux-machines, raadpleeg dan [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
