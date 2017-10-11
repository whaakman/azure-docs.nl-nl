---
title: Gebruik van SSH-sleutels met Windows voor virtuele Linux-machines | Microsoft Docs
description: Informatie over het genereren en gebruiken van SSH-sleutels op een Windows-computer verbinding maken met een virtuele Linux-machine in Azure.
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 66837a3a153cda041f5351c52c8ccb1f8ccfea50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Hoe gebruik SSH-sleutels met Windows in Azure
> [!div class="op_single_selector"]
> * [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

Als u verbinding met virtuele Linux-machines (VM's) in Azure, moet u [cryptografie met openbare sleutels](https://wikipedia.org/wiki/Public-key_cryptography) op een veiliger manier zich aanmelden bij uw Linux-VM. Dit proces omvat een openbare en persoonlijke sleuteluitwisseling zelf in plaats van een gebruikersnaam en het wachtwoord verifiëren met de shellopdracht secure (SSH). Wachtwoorden zijn kwetsbaar voor aanvallen, vooral op Internet gerichte VMs zoals webservers met brute kracht. Dit artikel bevat een overzicht van SSH-sleutels en het genereren van de juiste sleutels op een Windows-computer.

## <a name="overview-of-ssh-and-keys"></a>Overzicht van SSH en sleutels
U kunt veilig aanmelden bij uw Linux-VM met behulp van openbare en persoonlijke sleutels:

* De **openbare sleutel** uw Linux-VM of een andere service die u wilt gebruiken met cryptografie met openbare sleutels is geplaatst.
* De **persoonlijke sleutel** is wat u aanwezig zijn voor uw Linux-VM wanneer u zich aanmeldt, om uw identiteit te verifiëren. Houd deze privésleutel geheim. Deel deze niet.

Deze openbare en persoonlijke sleutels kunnen worden gebruikt voor meerdere virtuele machines en services. U hoeft niet een sleutelpaar voor elke virtuele machine of service die u wilt openen. Zie voor een gedetailleerd overzicht [cryptografie met openbare sleutels](https://wikipedia.org/wiki/Public-key_cryptography).

SSH is een versleutelde verbindingsprotocol waarmee beveiligde aanmeldingen via niet-beveiligde verbindingen. Het is het standaardprotocol voor de verbinding voor virtuele Linux-machines gehost in Azure. Maar SSH zelf een versleutelde verbinding biedt, wordt met wachtwoorden met SSH-verbindingen nog steeds de virtuele machine kwetsbaar voor brute-force-aanvallen of te raden van wachtwoorden. Een veiliger en gewenste methode voor het verbinding maken met een virtuele machine via SSH is met behulp van deze openbare en persoonlijke sleutels, ook wel bekend als SSH-sleutels.

Als u niet gebruiken van SSH-sleutels wilt, kunt u nog steeds aanmelden bij uw virtuele Linux-machines met een wachtwoord. Als uw virtuele machine geen toegang heeft tot Internet, met wachtwoorden mogelijk niet voldoende. U moet nog steeds wachtwoorden voor elke Linux-VM te beheren en onderhouden in orde wachtwoordbeleid en procedures, zoals minimale wachtwoordlengte en deze regelmatig bij te werken. Het gebruik van SSH-sleutels vermindert de complexiteit van het beheren van afzonderlijke referenties in meerdere virtuele machines.

## <a name="windows-packages-and-ssh-clients"></a>Windows-pakketten en SSH-clients
U verbinding maken met en beheren van virtuele Linux-machines in Azure worden verkregen met een **SSH client**. Windows-computers hebben doorgaans geen een SSH-client is geïnstalleerd. De Update voor Windows 10 Verjaardag toegevoegd Bash voor Windows en de meest recente Update voor Windows 10 gebruikers biedt aanvullende updates. Deze Windows-subsysteem voor Linux kunt u uitvoeren en toegang tot hulpprogramma's zoals een SSH-client systeemeigen binnen een Bash-shell. Voer vervolgens een van de Linux-documenten, zoals [het genereren van SSH-sleutelparen voor Linux](mac-create-ssh-keys.md). Bash voor Windows, nog in ontwikkeling is en wordt beschouwd als een bètaversie. Zie voor meer informatie over Windows Bash [Bash op Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about).

Als u gebruikmaken van iets anders dan wilt voor Windows Bash, algemene Windows SSH clients die kunt u installeren zijn opgenomen in de volgende pakketten:

* [GIT voor Windows](https://git-for-windows.github.io/)
* [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)


## <a name="which-key-files-do-you-need-to-create"></a>Welke sleutelbestanden moet u maken?
Azure vereist ten minste 2048 bits **ssh-rsa** geformatteerd openbare en persoonlijke sleutels. Als u Azure-resources met behulp van het klassieke implementatiemodel beheert, moet u ook een PEM genereren (`.pem` bestand).

Hier worden de implementatiescenario's en de typen bestanden die u in elk gebruikt:

1. **SSH-rsa** sleutels zijn vereist voor een implementatie met behulp van de [Azure-portal](https://portal.azure.com), en implementaties van Resource Manager met behulp van de [Azure CLI](../../cli-install-nodejs.md).
   * Deze sleutels worden doorgaans alle de meeste mensen hebben.
2. Een `.pem` bestand is vereist voor het maken van virtuele machines met de klassieke implementatie. Deze sleutels worden ondersteund in de klassieke implementaties wanneer de [Azure-portal](https://portal.azure.com) of [Azure CLI](../../cli-install-nodejs.md).
   * U hoeft alleen te maken van deze aanvullende sleutels en certificaten, als u de resources die zijn gemaakt met behulp van het klassieke implementatiemodel beheert.

## <a name="install-git-for-windows"></a>Installeer Git voor Windows
De voorgaande sectie verschillende pakketten die zijn vermeld de `openssl` hulpprogramma voor Windows. Dit hulpprogramma is nodig om openbare en persoonlijke sleutels te maken. De volgende voorbeelden worden het installeren en gebruiken in detail beschreven **Git voor Windows**, maar u kunt kiezen welke u liever pakket. **GIT voor Windows** hebt u toegang tot een aantal extra open source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) hulpprogramma's en hulpmiddelen die nuttig zijn kunnen bij het werken met virtuele Linux-machines.

1. Download en installeer **Git voor Windows** van de volgende locatie: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).
2. De standaardopties te gebruiken tijdens het installatieproces accepteren, tenzij u moet in het bijzonder te wijzigen.
3. Voer **Git Bash** van de **Menu Start** > **Git** > **Git Bash**. De console lijkt op het volgende voorbeeld:

    ![GIT voor Windows Bash-shell](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a>Een persoonlijke sleutel maken
1. In uw **Git Bash** venster gebruik `openssl.exe` een persoonlijke sleutel te maken. Het volgende voorbeeld wordt een sleutel met de naam `myPrivateKey` en het certificaat met de naam `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    De uitvoer lijkt op het volgende voorbeeld:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   Als bash is een fout rapporteert, probeert te openen van een nieuwe **Git Bash** venster met verhoogde bevoegdheden. Voer de `openssl` opdracht.

2. Beantwoord de aanwijzingen voor het landnaam, locatie, de organisatienaam van de, enzovoort.
3. Uw nieuwe persoonlijke sleutel en het certificaat worden in de huidige werkmap gemaakt. Uit veiligheidsoogpunt, moet u de machtigingen instellen voor uw persoonlijke sleutel zodat alleen u toegang hebt tot het:

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. De [volgende sectie](#create-a-private-key-for-putty) details PuTTYgen met zowel weergeven en gebruiken van de openbare sleutel en een persoonlijke sleutel maken dat specifiek is voor het gebruik van PuTTY om SSH aan virtuele Linux-machines. De volgende opdracht genereert u een bestand met openbare sleutel met de naam `myPublicKey.key` die u meteen kunt gebruiken:

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. Als u ook klassieke resources beheren moet, converteert u de `myCert.pem` naar `myCert.cer` (DER encoded X509 certificaat). Deze optionele stap alleen uitvoeren als u moet specifiek oudere klassieke om resources te beheren.

    Converteren van het certificaat met de volgende opdracht:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Maken van een persoonlijke sleutel voor PuTTY
PuTTY is een algemene SSH-client voor Windows. U bent kunt u een SSH-client die u wilt gebruiken. Voor het gebruik van PuTTY, moet u een extra type sleutel - een PuTTY persoonlijke sleutel (PPK) maken. Als u niet PuTTY gebruiken wilt, moet u deze sectie overslaan.

Het volgende voorbeeld maakt deze extra persoonlijke sleutel specifiek voor PuTTY gebruiken:

1. Gebruik **Git Bash** uw persoonlijke sleutel converteren naar een persoonlijke RSA-sleutel die PuTTYgen kunt begrijpen. Het volgende voorbeeld wordt een sleutel met de naam `myPrivateKey_rsa` uit de bestaande sleutel met de naam `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Uit veiligheidsoogpunt, moet u de machtigingen instellen voor uw persoonlijke sleutel zodat alleen u toegang hebt tot het:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. Downloaden en uitvoeren van PuTTYgen van de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
3. Klik op het menu: **bestand** > **Load persoonlijke sleutel**
4. Zoek uw persoonlijke sleutel (`myPrivateKey_rsa` in het vorige voorbeeld). De standaardmap bij het starten van **Git Bash** is `C:\Users\%username%`. Wijzig het filter om weer te geven **alle bestanden (\*.\*)** :

    ![Laden van de bestaande persoonlijke sleutel in PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. Klik op **Open**. Een prompt geeft aan dat de sleutel geïmporteerd zijn:

    ![Sleutel naar PuTTYgen met succes geïmporteerd](./media/ssh-from-windows/successfully-imported-key.png)
6. Klik op **OK** achter de opdrachtprompt sluiten.
7. De openbare sleutel wordt weergegeven boven aan de **PuTTYgen** venster. U kopieert en plakt u deze openbare sleutel in de Azure-portal of de Azure Resource Manager-sjabloon bij het maken van een Linux-VM. U kunt ook klikken op **openbare sleutel opslaan** een kopie wilt opslaan op uw computer:

    ![PuTTY bestand voor de openbare sleutel opslaan](./media/ssh-from-windows/save-public-key.png)

    Het volgende voorbeeld ziet hoe u Kopieer en plak deze openbare sleutel in de Azure-portal bij het maken van een Linux-VM. De openbare sleutel vervolgens doorgaans wordt opgeslagen in `~/.ssh/authorized_keys` op de nieuwe virtuele machine.

    ![Openbare sleutel wordt gebruikt bij het maken van een virtuele machine in de Azure portal](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. Terug in de **PuTTYgen**, klikt u op **persoonlijke sleutel opslaan**:

    ![PuTTY persoonlijke sleutel opslaan](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > Gevraagd als u doorgaan wilt zonder een wachtwoordzin invoeren voor uw sleutel. Een wachtwoordzin is vergelijkbaar met een wachtwoord dat is gekoppeld aan uw persoonlijke sleutel. Zelfs als iemand het verkrijgen van uw persoonlijke sleutel was, ze nog steeds niet kan worden geverifieerd met alleen de sleutel. Ze moet tevens de wachtwoordzin. Zonder een wachtwoordzin als iemand anders uw persoonlijke sleutel, krijgt kunnen deze aanmelden bij een virtuele machine of service die gebruikmaakt van die sleutel. U wordt aangeraden maken van een wachtwoordzin. Als u de wachtwoordzin vergeet, is er echter geen manier om deze te herstellen.
   >
   >

    Als u een wachtwoordzin invoeren wilt, klikt u op **Nee**, voer een wachtwoordzin in het hoofdvenster PuTTYgen en klik vervolgens op **persoonlijke sleutel opslaan** opnieuw. Klik anders op **Ja** om door te gaan zonder op te geven van de optionele wachtwoordzin.
9. Voer een naam en locatie voor het opslaan van het PPK-bestand.

## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Putty gebruiken om SSH kunt uitvoeren naar een Linux-Machine
PuTTY is opnieuw een algemene SSH-client voor Windows. U bent kunt u een SSH-client die u wilt gebruiken. De volgende stappen wordt beschreven hoe u uw persoonlijke sleutel gebruiken om te verifiëren met uw Azure-virtuele machine via SSH. De stappen zijn vergelijkbaar in andere SSH sleutel clients in termen van hoeven laden van uw persoonlijke sleutel om de SSH-verbinding te verifiëren.

1. Downloaden en uitvoeren putty van de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Vul de hostnaam of IP-adres van uw virtuele machine van de Azure-portal:

    ![Nieuwe PuTTY verbinding openen](./media/ssh-from-windows/putty-new-connection.png)
3. Voordat u selecteert **Open**, klikt u op **verbinding** > **SSH** > **Auth** tabblad. Blader naar en selecteer uw persoonlijke sleutel:

    ![Selecteer uw PuTTY persoonlijke sleutel voor verificatie](./media/ssh-from-windows/putty-auth-dialog.png)
4. Klik op **Open** verbinding maken met uw virtuele machine

## <a name="next-steps"></a>Volgende stappen
U kunt ook de openbare en persoonlijke sleutels genereren [met OS X- en Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie voor meer informatie over Bash voor Windows en de voordelen van een OSS-hulpprogramma's direct beschikbaar op uw Windows-computer [Bash op Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about).

Als u problemen ondervindt bij het gebruik van SSH verbinding maken met uw virtuele Linux-machines, Zie [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
