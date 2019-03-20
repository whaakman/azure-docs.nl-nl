---
title: SSH-sleutels gebruiken met Windows voor virtuele Linux-machines | Microsoft Docs
description: Informatie over het genereren en het gebruik van SSH-sleutels op een Windows-computer verbinding maken met een virtuele Linux-machine in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: 0ac62a99f5735647f67917d441645e30444b3818
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005675"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>SSH-sleutels gebruiken met Windows op Azure

In dit artikel wordt beschreven hoe om te genereren en gebruiken *secure shell* (SSH)-sleutels op een Windows-computer om te maken en verbinding maken met een Linux virtuele machine (VM) in Azure. Zie voor het gebruik van SSH-sleutels van een client voor Linux of macOS, de [snelle](mac-create-ssh-keys.md) of [gedetailleerde](create-ssh-keys-detailed.md) richtlijnen.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows-pakketten en SSH-clients
U verbinding maken met en beheren van virtuele Linux-machines in Azure met een *SSH-client*. Computers met Linux of macOS meestal hebben een reeks opdrachten SSH te genereren en SSH-sleutels beheren en te maken van SSH-verbindingen. 

Windows-computers altijd geen vergelijkbare SSH opdrachten die zijn geïnstalleerd. Recente versies van Windows 10 bieden [OpenSSH-clientopdrachten](https://blogs.msdn.microsoft.com/commandline/2018/03/07/windows10v1803/) maken en beheren van SSH-sleutels en SSH-verbindingen vanaf een opdrachtprompt. Recente versies van Windows 10 bevat ook de [Windows-subsysteem voor Linux](https://docs.microsoft.com/windows/wsl/about) uitvoeren en toegang tot hulpprogramma's zoals een SSH-client systeemeigen binnen een Bash-shell. 

Andere algemene Windows SSH-clients die kunt u lokaal installeren zijn opgenomen in de volgende pakketten:

* [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [GIT voor Windows](https://git-for-windows.github.io/)
* [MobaXterm](https://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

U kunt ook de SSH-hulpprogramma's beschikbaar in Bash in de [Azure Cloud Shell](../../cloud-shell/overview.md). 

* Toegang tot Cloud Shell in uw webbrowser op [ https://shell.azure.com ](https://shell.azure.com) of in de [Azure-portal](https://portal.azure.com). 
* Toegang krijgen tot Cloud Shell als terminal in Visual Studio Code door het installeren van de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
De volgende secties worden twee opties voor het maken van een SSH-sleutelpaar in Windows. U kunt een shell-opdracht gebruiken (`ssh-keygen`) of een GUI-hulpprogramma (PuTTYgen).

### <a name="create-ssh-keys-with-ssh-keygen"></a>SSH-sleutels maken met ssh-keygen

Als u een opdrachtshell uitvoeren op Windows die ondersteuning biedt voor hulpprogramma's voor SSH-client (of u Azure Cloud Shell gebruiken), maakt u een SSH-sleutelpaar met behulp van de `ssh-keygen` opdracht. Typ de volgende opdracht en de hierom wordt gevraagd. Als een SSH-sleutelpaar in de gekozen locatie bestaat, worden deze bestanden overschreven. 

```bash
ssh-keygen -t rsa -b 2048
```

Zie voor meer achtergrondinformatie en informatie, de [snelle](mac-create-ssh-keys.md) of [gedetailleerde](create-ssh-keys-detailed.md) stappen voor het maken van SSH-sleutels met behulp van `ssh-keygen`.

### <a name="create-ssh-keys-with-puttygen"></a>SSH-sleutels met PuTTYgen maken

Als u liever een GUI-hulpprogramma gebruiken om te maken van SSH-sleutels, kunt u de serversleutelgenerator PuTTYgen, opgenomen in de [PuTTY downloadpakket](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). 

Een SSH-RSA-sleutelpaar maken met PuTTYgen:

1. Start PuTTYgen.

2. Klik op **genereren**. Standaard genereert PuTTYgen een SSH-2 RSA 2048-bits sleutel.

4. Beweeg de muis in de lege ruimte voor aanvraaggrootte voor de sleutel.

5. (Optioneel) Voer nadat de openbare sleutel is gegenereerd, en een wachtwoordzin bevestigen. U wordt gevraagd naar de wachtwoordzin wanneer u met de virtuele machine met uw persoonlijke SSH-sleutel verifieert. Zonder een wachtwoordzin als iemand anders uw privésleutel, verkrijgt deze zich kan aanmelden bij een virtuele machine of service die gebruikmaakt van deze sleutel. U wordt aangeraden dat u een wachtwoordzin maken. Als u de wachtwoordzin vergeet, is er echter geen manier om deze te herstellen.

6. De openbare sleutel wordt aan de bovenkant van het venster weergegeven. U kunt kopiëren van deze volledige openbare sleutel en plak deze in Azure portal of een Azure Resource Manager-sjabloon bij het maken van een Linux-VM. U kunt ook selecteren **openbare sleutel opslaan** een kopie wilt opslaan op uw computer:

    ![PuTTY bestand voor de openbare sleutel opslaan](./media/ssh-from-windows/save-public-key.png)

7. Selecteer desgewenst om op te slaan de persoonlijke sleutel in de PuTTy persoonlijke sleutel indeling (ppk-bestand), **persoonlijke sleutel opslaan**. U moet het ppk-bestand later aan PuTTY gebruiken om een SSH-verbinding maken met de virtuele machine.

    ![PuTTY bestand met persoonlijke sleutel opslaan](./media/ssh-from-windows/save-ppk-file.png)

    Als u wilt de persoonlijke sleutel opslaan in de OpenSSH-indeling, selecteert u de persoonlijke sleutel indeling die wordt gebruikt door veel SSH-clients, **conversies** > **exporteren OpenSSH-sleutel**.

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>Geef een openbare SSH-sleutel bij het implementeren van een virtuele machine

Voor het maken van een Linux-VM die gebruikmaakt van SSH-sleutels voor verificatie, Geef uw openbare SSH-sleutel bij het maken van de virtuele machine met behulp van de Azure-portal of andere methoden.

Het volgende voorbeeld laat zien hoe u wilt kopiëren en plakken van deze openbare sleutel in Azure portal, bij het maken van een Linux-VM. De openbare sleutel wordt doorgaans vervolgens opgeslagen in de map ~/.ssh/authorized_key op uw nieuwe virtuele machine.

   ![Openbare sleutel wordt gebruikt bij het maken van een VM in Azure portal](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Verbinding maken met uw VM

Eén manier om u te maken van een SSH-verbinding met uw Linux-VM van Windows is het gebruik van een SSH-client. Dit is de aanbevolen methode hebt u een SSH-client geïnstalleerd op uw Windows-systeem, of als u de SSH-hulpprogramma's in Bash in Azure Cloud Shell. Als u liever een GUI-hulpprogramma, kunt u verbinding maken met PuTTY.  

### <a name="use-an-ssh-client"></a>Een SSH-client gebruiken
Met de openbare sleutel die is geïmplementeerd op uw Azure-VM en de persoonlijke sleutel op uw lokale systeem, SSH verbinding met uw virtuele machine via het IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht uit met de naam van de beheerder-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt geconfigureerd, voert u de wachtwoordzin wanneer hierom wordt gevraagd tijdens het aanmelden.

Als de virtuele machine wordt met behulp van het beleid voor just-in-time-toegang, moet u om toegang te vragen voordat u verbinding met de virtuele machine maken kunt. Zie voor meer informatie over het beleid voor just-in-time [beheer van de virtuele machine toegang met just in time-beleid](../../security-center/security-center-just-in-time.md).

### <a name="connect-with-putty"></a>Verbinding maken met PuTTY

Als u hebt geïnstalleerd het [PuTTY downloadpakket](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) en eerder gegenereerde een bestand PuTTY persoonlijke sleutel (.ppk), u kunt verbinding maken met een Linux-VM met PuTTY.

1. Start PuTTy.

2. Vul in de hostnaam of IP-adres van uw virtuele machine vanuit Azure portal:

    ![Nieuwe PuTTY verbinding openen](./media/ssh-from-windows/putty-new-connection.png)

3. Selecteer de **verbinding** > **SSH** > **Auth** categorie. Blader naar en selecteer uw PuTTY persoonlijke sleutel (ppk-bestand):

    ![Selecteer uw PuTTY persoonlijke sleutel voor verificatie](./media/ssh-from-windows/putty-auth-dialog.png)

4. Klik op **Open** verbinding maken met uw virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Zie voor gedetailleerde stappen, opties en geavanceerde voorbeelden van het werken met SSH-sleutels, [gedetailleerde stappen voor het maken van SSH-sleutelparen](create-ssh-keys-detailed.md).

* U kunt ook PowerShell gebruiken in Azure Cloud Shell voor het SSH-sleutels genereren en SSH-verbinding maken met een virtuele Linux-machines. Zie de [PowerShell-snelstartgids](../../cloud-shell/quickstart-powershell.md#ssh).

* Als u problemen ondervindt bij het gebruik van SSH verbinding maken met uw virtuele Linux-machines, raadpleeg dan [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
