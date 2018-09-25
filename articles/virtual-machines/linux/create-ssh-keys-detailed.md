---
title: Gedetailleerde stappen - SSH-sleutel sleutelpaar voor virtuele Azure Linux-machines | Microsoft Docs
description: Meer informatie over gedetailleerde stappen voor het maken en beheren van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: danlep
ms.openlocfilehash: 804b7c0ff31575e6d62497fd5166e1a38a273076
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965573"
---
# <a name="detailed-steps-create-and-manage-ssh-keys-for-authentication-to-a-linux-vm-in-azure"></a>Gedetailleerde stappen: maken en beheren van SSH-sleutels voor verificatie met een Linux-VM in Azure 
Met de combinatie van een secure shell (SSH), kunt u een virtuele Linux-machine maken in Azure die standaard gebruik van SSH-sleutels voor verificatie, hoeft u het wachtwoord aan te melden. VM's gemaakt met de Azure portal, Azure CLI, Resource Manager-sjablonen of andere hulpprogramma's kunnen uw openbare SSH-sleutel opnemen als onderdeel van de implementatie, u verificatie met SSH-sleutel voor SSH-verbindingen stelt. 

In dit artikel vindt u gedetailleerde achtergrond en stappen voor het maken en beheren van een SSH-RSA openbaar / persoonlijk sleutelpaarbestand voor SSH-client-verbindingen. Als u wilt dat snelle opdrachten, Zie [over het maken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

Zie voor aanvullende manieren om te genereren en het gebruik van SSH-sleutels op een Windows-computer, [over het gebruik van SSH-sleutels met Windows op Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

### <a name="private-key-passphrase"></a>Wachtwoordzin persoonlijke-sleutel
De persoonlijke SSH-sleutel moet een zeer veilig wachtwoordzin om deze te beschermen. Deze wachtwoordzin is alleen voor toegang tot de persoonlijke SSH-sleutelbestand en *is niet* wachtwoord voor het gebruikersaccount. Wanneer u een wachtwoordzin aan uw SSH-sleutel toevoegt, worden de persoonlijke sleutel met behulp van 128-bits AES, versleuteld zodat de persoonlijke sleutel niet kan worden gebruikt zonder de wachtwoordzin is te ontsleutelen. Als een aanvaller zou uw persoonlijke sleutel en deze sleutel is geen een wachtwoordzin, is ze zou kunnen persoonlijke sleutel gebruiken voor het aanmelden bij de servers waarop de bijbehorende openbare sleutel. Als een persoonlijke sleutel wordt beveiligd door een wachtwoordzin, wordt deze niet gebruikt door de aanvaller, bieden een extra beveiligingslaag voor uw infrastructuur in Azure.

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="ssh-keys-use-and-benefits"></a>Gebruik en voordelen van SSH-sleutels

Wanneer u een Azure-VM maken door de openbare sleutel op te geven, kopieert Azure de openbare sleutel (in de `.pub` indeling) naar de `~/.ssh/authorized_keys` map op de virtuele machine. SSH-sleutels in `~/.ssh/authorized_keys` worden gebruikt om de client te vragen de bijbehorende persoonlijke sleutel te verstrekken bij verbinding via een SSH-aanmelding. In een Azure Linux VM die gebruikmaakt van SSH-sleutels voor verificatie, configureert Azure de SSHD-server niet aanmelding met een wachtwoord, alleen SSH-sleutels zijn toegestaan. Daarom door het maken van een virtuele Azure Linux-machine met SSH-sleutels, kunt u de VM-implementatie beveiligen en uzelf de typische na de implementatie configuratiestap besparen wachtwoorden uitschakelen in de `sshd_config` bestand.

Als u niet gebruiken van SSH-sleutels wilt, kunt u uw Linux-VM instellen om wachtwoordverificatie te gebruiken. Als uw virtuele machine geen toegang heeft tot Internet, met behulp van wachtwoorden mogelijk niet voldoende. U moet nog steeds uw wachtwoorden beheren voor elke Linux-VM en onderhouden in orde wachtwoordbeleid en -procedures, zoals minimale wachtwoordlengte en regelmatige updates. Met behulp van SSH-sleutels vermindert de complexiteit van het beheer van afzonderlijke referenties op meerdere virtuele machines.

## <a name="generate-keys-with-ssh-keygen"></a>Genereren van sleutels met ssh-keygen

De sleutels wilt maken, een gewenste opdracht is `ssh-keygen`, die beschikbaar zijn met de OpenSSH-hulpprogramma's in de Azure Cloud Shell, een macOS- of Linux-host, is de [Windows-subsysteem voor Linux](https://docs.microsoft.com/windows/wsl/about), en andere hulpprogramma's. `ssh-keygen` een reeks vragen stelt en vervolgens een persoonlijke sleutel en een overeenkomende openbare sleutel schrijft. 

SSH-sleutels worden standaard opgeslagen in de `~/.ssh`-directory.  Als u niet beschikt over de map `~/.ssh`, wordt deze door de opdracht `ssh-keygen` voor u gemaakt met de juiste machtigingen.

### <a name="basic-example"></a>Eenvoudige voorbeeld

De volgende `ssh-keygen` opdracht 2048-bits SSH RSA openbare en persoonlijke sleutelbestanden genereert standaard in de `~/.ssh` directory. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven.

```bash
ssh-keygen -t rsa -b 2048
```

### <a name="detailed-example"></a>Gedetailleerd voorbeeld
Het volgende voorbeeld ziet aanvullende opdrachtopties voor het maken van een SSH-RSA-sleutelpaar. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven. 

```bash
ssh-keygen \
    -t rsa \
    -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N mypassphrase
```

**Uitleg van de opdracht**

`ssh-keygen` = het programma dat wordt gebruikt voor het maken van de sleutels

`-t rsa` = type sleutel voor het maken, in dit geval de RSA-indeling

`-b 4096` = het aantal bits in de sleutel in dit geval 4096

`-C "azureuser@myserver"` = een opmerking die wordt toegevoegd aan het einde van het openbare sleutelbestand om het gemakkelijk te identificeren. Normaal gesproken een e-mailadres wordt gebruikt als de opmerking, maar gebruiken aansluit bij uw infrastructuur.

`-f ~/.ssh/mykeys/myprivatekey` = de bestandsnaam van het bestand met persoonlijke sleutel als u niet de standaardnaam gebruiken. Een bijbehorende openbare-sleutelbestand met het achtervoegsel `.pub` wordt gegenereerd in dezelfde map. De map moet bestaan.

`-N mypassphrase` = een extra wachtwoordzin die wordt gebruikt voor toegang tot het persoonlijke sleutelbestand. 

### <a name="example-of-ssh-keygen"></a>Voorbeeld van ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
The keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

#### <a name="saved-key-files"></a>Opgeslagen sleutelbestanden

`Enter file in which to save the key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

De naam van het sleutelpaar voor dit artikel. Een sleutelpaar met de naam `id_rsa` is de standaardinstelling; sommige programma's verwachten mogelijk de `id_rsa` naam van bestand met persoonlijke sleutel, zodat met een een goed idee is. De map `~/.ssh/` is de standaardlocatie voor SSH-sleutelparen en het SSH-configuratiebestand. Als `ssh-keygen` niet wordt opgegeven met een volledig pad, worden de sleutels in de huidige werkmap gemaakt in plaats van in de standaardmap `~/.ssh`.

#### <a name="list-of-the-ssh-directory"></a>Lijst van de `~/.ssh` directory

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

#### <a name="key-passphrase"></a>Sleutelwachtwoordzin

`Enter passphrase (empty for no passphrase):`

Het is *sterk* aanbevolen een wachtwoordzin toevoegen aan uw persoonlijke sleutel. Zonder een wachtwoordzin op om de belangrijkste bestand te beveiligen, kunt iedereen met het bestand dat deze aan te melden bij elke server die beschikt over de bijbehorende openbare sleutel gebruiken. Toevoegen van een wachtwoordzin biedt veel meer bescherming in het geval iemand in staat is toegang te krijgen tot uw persoonlijke sleutelbestand is zodat u tijd de sleutels wijzigen.

## <a name="generate-keys-automatically-during-deployment"></a>Het automatisch genereren van sleutels tijdens de implementatie

Als u de [Azure CLI](/cli/azure) voor het maken van uw virtuele machine, kunt u eventueel SSH openbare en persoonlijke sleutelbestanden genereren door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--generate-ssh-keys` optie. De sleutels worden opgeslagen in de map ~/.ssh. Houd er rekening mee dat deze optie niet sleutels overschreven als deze al bestaan op die locatie.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Openbare SSH-sleutel opgeven bij het implementeren van een virtuele machine

Geef uw openbare SSH-sleutel voor het maken van een Linux-VM die gebruikmaakt van SSH-sleutels voor verificatie bij het maken van de virtuele machine met behulp van Azure portal, CLI, Resource Manager-sjablonen of andere methoden. Wanneer u de portal gebruikt, voert u de openbare sleutel zelf. Als u de [Azure CLI](/cli/azure) opgeven voor het maken van uw virtuele machine met een bestaande openbare sleutel, de waarde of de locatie van deze openbare sleutel door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--ssh-key-value` optie. 

Als u niet bekend met de indeling van een openbare SSH-sleutel bent, kunt u de openbare sleutel bekijken door te voeren `cat` als volgt vervangen `~/.ssh/id_rsa.pub` met de locatie van uw eigen openbare-sleutelbestand:

```bash
cat ~/.ssh/id_rsa.pub
```

Uitvoer is vergelijkbaar met de volgende (hier geredigeerd):

```
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXG+/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Als u kopieert en plakt u de inhoud van het openbare sleutelbestand in de Azure-portal of een Resource Manager-sjabloon, zorg ervoor dat u geen geen extra witruimte kopieert of extra linebreaks introduceren. Als u Mac OS, kunt u bijvoorbeeld een bestand met de openbare sleutel doorgeven (standaard `~/.ssh/id_rsa.pub`) naar **pbcopy** om de inhoud te kopiëren (Er zijn andere Linux-programma's die hetzelfde, zoals doen **xclip**).

Als u liever een openbare sleutel die zich in een indeling met meerdere regels te gebruiken, kunt u een opgemaakte RFC4716-sleutel genereren in een pem-container vanuit de openbare sleutel die u eerder hebt gemaakt.

Een RFC4716-sleutel maken van een bestaande openbare SSH-sleutel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="ssh-to-your-vm-with-an-ssh-client"></a>SSH naar uw virtuele machine met een SSH-client
Met de openbare sleutel die is geïmplementeerd op uw Azure-VM en de persoonlijke sleutel op uw lokale systeem, SSH verbinding met uw virtuele machine via het IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht uit met de naam van de beheerder-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt opgegeven, moet u deze tijdens het aanmeldingsproces desgevraagd invoeren. (De server is toegevoegd aan uw map `~/.ssh/known_hosts` en u wordt pas weer gevraagd om verbinding te maken nadat de openbare sleutel op uw virtuele Azure-machine is gewijzigd of de naam van de server is verwijderd uit `~/.ssh/known_hosts`.)

## <a name="use-ssh-agent-to-store-your-private-key-passphrase"></a>Gebruik van ssh-agent voor het opslaan van de wachtwoordzin van uw persoonlijke sleutel

Teneinde te voorkomen dat de wachtwoordzin van uw persoonlijke sleutelbestand bij elke SSH-aanmelding, kunt u `ssh-agent` de wachtwoordzin van uw persoonlijke sleutelbestand in de cache. Als u een Mac gebruikt, het Mac OS Keychain de wachtwoordzin van de persoonlijke sleutel veilig opslaat wanneer u aanroepen `ssh-agent`.

Controleer en gebruik `ssh-agent` en `ssh-add` naar het SSH-systeem informeren over de sleutelbestanden, zodat u niet hoeft de wachtwoordzin interactief gebruiken.

```bash
eval "$(ssh-agent -s)"
```

Voeg nu de persoonlijke sleutel toe aan `ssh-agent` met de opdracht `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

De wachtwoordzin van de persoonlijke sleutel wordt nu opgeslagen in `ssh-agent`.

## <a name="use-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>Ssh-exemplaar-id gebruiken om te kopiëren van de sleutel aan een bestaande virtuele machine
Als u al een virtuele machine hebt gemaakt, kunt u de nieuwe openbare SSH-sleutel installeren op uw Linux-VM met een vergelijkbaar met de volgende opdracht:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Een SSH-configuratiebestand maken en configureren

U kunt maken en configureren van een SSH-configuratiebestand (`~/.ssh/config`) om log-modules te versnellen en uw SSH-clientgedrag te optimaliseren. 

Het volgende voorbeeld toont een eenvoudige configuratie waarmee u kunt snel aanmelden als een gebruiker naar een specifieke virtuele machine met behulp van de standaard persoonlijke SSH-sleutel. 

### <a name="create-the-file"></a>Het bestand maken

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bewerk het bestand om toe te voegen van de nieuwe SSH-configuratie

```bash
vim ~/.ssh/config
```

### <a name="example-configuration"></a>Voorbeeld van een configuratie

Configuratie-instellingen die geschikt is voor uw VM-host toevoegen.

```bash
# Azure Keys
Host myvm
  Hostname 102.160.203.241
  User azureuser
# ./Azure Keys
```

U kunt configuraties voor extra hosts zodat u een eigen toegewezen sleutelpaar toevoegen. Zie [SSH-configuratiebestand](https://www.ssh.com/ssh/config/) voor meer geavanceerde configuratieopties.

Nu u een SSH-sleutelpaar en een geconfigureerd SSH-configuratiebestand hebt, kunt u zich snel en veilig aanmelden bij uw virtuele Linux-machine. Wanneer u de volgende opdracht uitvoert, SSH zoekt en laadt van de instellingen van de `Host myvm` blokkeren in de SSH-configuratiebestand.

```bash
ssh myvm
```

De eerste keer dat u zich aanmeldt bij een server met behulp van een SSH-sleutel gevraagd u naar de wachtwoordzin voor dat sleutelbestand.

## <a name="next-steps"></a>Volgende stappen

De volgende stap bestaat uit het maken van virtuele Linux-machines in Azure met de nieuwe openbare SSH-sleutel. Virtuele Azure-machines die zijn gemaakt met een openbare SSH-sleutel voor aanmelding, zijn beter beveiligd dan virtuele machines die zijn gemaakt met een wachtwoord als standaardaanmeldingsmethode.

* [Een virtuele Linux-machine maken met de Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-machine maken met de Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-VM met behulp van een Azure-sjabloon maken](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
