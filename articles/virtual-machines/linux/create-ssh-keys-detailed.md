---
title: Gedetailleerde stappen - SSH sleutelpaar voor Azure Linux VM's | Microsoft Docs
description: Meer gedetailleerde stappen voor het maken en beheren van een SSH openbare en persoonlijke sleutelpaar voor virtuele Linux-machines in Azure.
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
ms.openlocfilehash: 827c80a70047fd0f1ad67e4f19cb2300e45b2c6b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="detailed-steps-create-and-manage-ssh-keys-for-authentication-to-a-linux-vm-in-azure"></a>Gedetailleerde stappen: maken en beheren van de SSH-sleutels voor verificatie van een Linux VM in Azure 
Met de combinatie van een secure shell (SSH), kunt u een virtuele Linux-machine maken in Azure die standaard SSH-sleutels gebruiken voor verificatie, hoeft u de wachtwoorden aan te melden. Virtuele machines gemaakt met de Azure portal, Azure CLI Resource Manager-sjablonen of andere hulpprogramma's voor uw openbare SSH-sleutel kunnen opnemen als onderdeel van de implementatie, u de verificatie van SSH-sleutel voor SSH-verbindingen stelt. 

In dit artikel vindt u gedetailleerde achtergrond en stappen voor het maken en beheren van een paar SSH RSA bestand met openbare en persoonlijke sleutel voor SSH-verbindingen voor clients. Als u wilt dat snelle opdrachten, Zie [maken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

Zie voor meer manieren om te genereren en gebruiken van SSH-sleutels op een Windows-computer [het gebruik van SSH-sleutels met Windows in Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

### <a name="private-key-passphrase"></a>Persoonlijke sleutel wachtwoordzin
De persoonlijke SSH-sleutel moet een zeer beveiligde wachtwoordzin te beschermen. Deze wachtwoordzin is alleen voor toegang tot de persoonlijke SSH-sleutelbestand en *is niet* wachtwoord voor het gebruikersaccount. Wanneer u een wachtwoordzin aan uw SSH-sleutel toevoegt, versleutelt de persoonlijke sleutel met AES 128-bits, zodat de persoonlijke sleutel onbruikbaar zonder de wachtwoordzin is te ontsleutelen. Als een aanvaller uw persoonlijke sleutel stelen en deze sleutel is geen een wachtwoordzin, is ze zou kunnen persoonlijke sleutel gebruiken voor aanmelding bij andere servers waarop de bijbehorende openbare sleutel. Als een persoonlijke sleutel wordt beveiligd door een wachtwoordzin, wordt deze niet gebruikt door de aanvaller, door een extra laag van beveiliging voor uw infrastructuur in Azure.

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="ssh-keys-use-and-benefits"></a>Gebruik en voordelen van SSH-sleutels

Wanneer u een virtuele machine in Azure maakt door te geven van de openbare sleutel, Azure kopieert u de openbare sleutel (in de `.pub` indeling) naar de `~/.ssh/authorized_keys` map op de virtuele machine. SSH-sleutels in `~/.ssh/authorized_keys` worden gebruikt om de client te vragen de bijbehorende persoonlijke sleutel te verstrekken bij verbinding via een SSH-aanmelding. Azure configureert in een Azure Linux VM die gebruikmaakt van SSH-sleutels voor verificatie, de server SSHD wachtwoord aanmeldingen, SSH-sleutels niet zijn toegestaan. Daarom door het maken van een Azure Linux-VM met SSH-sleutels, kunt u de VM-implementatie te beveiligen en sla zelf de stap standaardconfiguratie na de implementatie van het uitschakelen van wachtwoorden in de `sshd_config` bestand.

Als u niet gebruiken van SSH-sleutels wilt, kunt u uw Linux-VM instellen wachtwoordverificatie te gebruiken. Als uw virtuele machine geen toegang heeft tot Internet, met wachtwoorden mogelijk niet voldoende. U moet nog steeds wachtwoorden voor elke Linux-VM te beheren en onderhouden in orde wachtwoordbeleid en procedures, zoals minimale wachtwoordlengte en regelmatig updates. Met behulp van SSH-sleutels, vermindert de complexiteit van het beheren van afzonderlijke referenties in meerdere virtuele machines.

## <a name="generate-keys-with-ssh-keygen"></a>Genereren van sleutels met ssh-keygen

Voor het maken van de sleutels een voorkeurs-opdracht is `ssh-keygen`, die beschikbaar zijn met de OpenSSH-hulpprogramma's in de Azure-Cloud-Shell, een Mac OS- of Linux-host, is de [Windows-subsysteem voor Linux](https://docs.microsoft.com/windows/wsl/about), en andere hulpprogramma's. `ssh-keygen` een reeks vragen en schrijft u een persoonlijke sleutel en een overeenkomende openbare sleutel. 

SSH-sleutels worden standaard opgeslagen in de `~/.ssh`-directory.  Als u niet beschikt over de map `~/.ssh`, wordt deze door de opdracht `ssh-keygen` voor u gemaakt met de juiste machtigingen.

### <a name="basic-example"></a>Basic-voorbeeld

De volgende `ssh-keygen` opdracht 2048-bits SSH RSA openbare en persoonlijke sleutelbestanden genereert standaard in de `~/.ssh` directory. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven.

```bash
ssh-keygen -t rsa -b 2048
```

### <a name="detailed-example"></a>Gedetailleerd voorbeeld
Het volgende voorbeeld ziet extra opdrachtopties voor het maken van een SSH-RSA-sleutelpaar. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven. 

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

`-t rsa` = type sleutel dat moet worden gemaakt in dit geval de RSA-indeling

`-b 4096` = het aantal bits in de sleutel in dit geval 4096

`-C "azureuser@myserver"` = een opmerking die wordt toegevoegd aan het einde van het openbare sleutelbestand om het gemakkelijk te identificeren. Normaal gesproken een e-mailadres wordt gebruikt als de opmerking, maar wat het beste werkt voor uw infrastructuur gebruiken.

`-f ~/.ssh/mykeys/myprivatekey` de bestandsnaam van het persoonlijke sleutelbestand = als u niet de standaardnaam gebruiken. Een bestand met bijbehorende openbare sleutel wordt toegevoegd aan de `.pub` wordt gegenereerd in dezelfde map. De map moet bestaan.

`-N mypassphrase` = een extra wachtwoordzin gebruikt voor toegang tot het bestand met de persoonlijke sleutel. 

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

De naam van het sleutelpaar voor dit artikel. Een sleutelpaar met de naam `id_rsa` is de standaardinstelling; sommige programma's verwachten mogelijk de `id_rsa` naam van de persoonlijke sleutelbestand, dus is het een goed idee. De map `~/.ssh/` is de standaardlocatie voor SSH-sleutelparen en het SSH-configuratiebestand. Als `ssh-keygen` niet wordt opgegeven met een volledig pad, worden de sleutels in de huidige werkmap gemaakt in plaats van in de standaardmap `~/.ssh`.

#### <a name="list-of-the-ssh-directory"></a>Lijst van de `~/.ssh` directory

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

#### <a name="key-passphrase"></a>Sleutel wachtwoordzin

`Enter passphrase (empty for no passphrase):`

Het is *sterk* aanbevolen een wachtwoordzin toevoegen aan uw persoonlijke sleutel. Zonder een wachtwoordzin voor het beveiligen van het sleutelbestand kunt iedereen met het bestand aanmelden bij elke server waarop de bijbehorende openbare sleutel. Toevoegen van een wachtwoordzin biedt meer beveiliging geval iemand in staat is toegang te krijgen tot uw persoonlijke sleutelbestand is geeft u de sleutels wijzigen tijd.

## <a name="generate-keys-automatically-during-deployment"></a>Het automatisch genereren van sleutels tijdens de implementatie

Als u de [Azure CLI 2.0](/cli/azure) voor het maken van uw virtuele machine, kunt u eventueel SSH openbare en persoonlijke sleutelbestanden genereren door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--generate-ssh-keys` optie. De sleutels worden opgeslagen in de map ~/.ssh. Houd er rekening mee dat deze optie niet sleutels overschreven als ze al aanwezig zijn op die locatie.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Openbare SSH-sleutel bieden bij het implementeren van een virtuele machine

Geef uw openbare SSH-sleutel voor het maken van een Linux VM die gebruikmaakt van SSH-sleutels voor verificatie bij het maken van de virtuele machine met behulp van de Azure CLI-portal Resource Manager-sjablonen of andere methoden. Wanneer u de portal gebruikt, voert u de openbare sleutel zelf. Als u de [Azure CLI 2.0](/cli/azure) als uw virtuele machine maken met een bestaande openbare sleutel, geeft u de waarde of de locatie van deze openbare sleutel door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--ssh-key-value` optie. 

Als u niet bekend met de indeling van een openbare SSH-sleutel bent, ziet u de openbare sleutel door te voeren `cat` als volgt vervangen `~/.ssh/id_rsa.pub` met uw eigen locatie openbaar-sleutelbestand:

```bash
cat ~/.ssh/id_rsa.pub
```

Uitvoer is vergelijkbaar met de volgende (hier geredigeerd):

```
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXG+/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Als u kopieert en plakt u de inhoud van een bestand met de openbare sleutel in de Azure-portal of de Resource Manager-sjabloon, moet u niet alle aanvullende witruimte kopiëren of extra linebreaks introduceren. Als u Mac OS gebruikt, kunt u bijvoorbeeld een bestand met de openbare sleutel doorsluizen (standaard `~/.ssh/id_rsa.pub`) naar **pbcopy** om de inhoud te kopiëren (Er zijn andere Linux-programma's die dezelfde dingen, zoals doen **xclip**).

Als u liever een openbare sleutel in een indeling met meerdere regels gebruikt, kunt u een RFC4716 opgemaakte sleutel genereren in een pem-container uit de openbare sleutel die u eerder hebt gemaakt.

Een RFC4716-sleutel maken van een bestaande openbare SSH-sleutel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="ssh-to-your-vm-with-an-ssh-client"></a>SSH met uw virtuele machine met een SSH-client
Met de openbare sleutel geïmplementeerd op de virtuele machine in Azure, en de persoonlijke sleutel op het lokale systeem, SSH met uw virtuele machine met behulp van de IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht met de naam van de administrator-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt opgegeven, moet u deze tijdens het aanmeldingsproces desgevraagd invoeren. (De server is toegevoegd aan uw map `~/.ssh/known_hosts` en u wordt pas weer gevraagd om verbinding te maken nadat de openbare sleutel op uw virtuele Azure-machine is gewijzigd of de naam van de server is verwijderd uit `~/.ssh/known_hosts`.)

## <a name="use-ssh-agent-to-store-your-private-key-passphrase"></a>Ssh-agent gebruiken voor het opslaan van uw persoonlijke sleutel wachtwoordzin

Om te voorkomen dat uw wachtwoordzin persoonlijke sleutelbestand bij elke SSH-aanmelding te typen, kunt u `ssh-agent` om uw persoonlijke sleutelbestand wachtwoordzin in cache. Als u een Mac worden gebruikt, het Mac OS-sleutelhanger de wachtwoordzin voor persoonlijke sleutels veilig opgeslagen wanneer u aanroept `ssh-agent`.

Controleer en gebruik `ssh-agent` en `ssh-add` het SSH-systeem informeren over de belangrijkste bestanden, zodat u niet wilt dat de wachtwoordzin interactief gebruiken.

```bash
eval "$(ssh-agent -s)"
```

Voeg nu de persoonlijke sleutel toe aan `ssh-agent` met de opdracht `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

De wachtwoordzin voor persoonlijke sleutels worden nu opgeslagen `ssh-agent`.

## <a name="use-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>Ssh-exemplaar-id gebruiken om te kopiëren van de sleutel naar een bestaande virtuele machine
Als u al een virtuele machine hebt gemaakt, kunt u de nieuwe openbare SSH-sleutel voor uw Linux-VM met een vergelijkbaar met de volgende opdracht installeren:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Een SSH-configuratiebestand maken en configureren

U kunt maken en configureren van een SSH-configuratiebestand (`~/.ssh/config`) om te versnellen logboek-modules en optimaliseren het gedrag van uw SSH-client. 

Het volgende voorbeeld toont een eenvoudige configuratie waarmee u kunt snel aanmelden als een gebruiker voor een specifieke virtuele machine met behulp van de standaard persoonlijke SSH-sleutel. 

### <a name="create-the-file"></a>Het bestand maken

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bewerk het bestand om toe te voegen van de nieuwe SSH-configuratie

```bash
vim ~/.ssh/config
```

### <a name="example-configuration"></a>Voorbeeldconfiguratie

Configuratie-instellingen die geschikt is voor uw VM-host toevoegen.

```bash
# Azure Keys
Host myvm
  Hostname 102.160.203.241
  User azureuser
# ./Azure Keys
```

U kunt configuraties voor extra hosts elk een eigen toegewezen sleutelpaar gebruiken toevoegen. Zie [SSH-configuratiebestand](https://www.ssh.com/ssh/config/) voor meer geavanceerde configuratieopties.

Nu u een SSH-sleutelpaar en een geconfigureerd SSH-configuratiebestand hebt, kunt u zich snel en veilig aanmelden bij uw virtuele Linux-machine. Wanneer u de volgende opdracht uitvoert, SSH zoekt en laadt van instellingen uit de `Host myvm` blokkeren in de SSH-configuratiebestand.

```bash
ssh myvm
```

De eerste keer dat u zich aanmelden met een server met een SSH-sleutel, de opdrachtprompts u naar de wachtwoordzin voor dat sleutelbestand.

## <a name="next-steps"></a>Volgende stappen

De volgende stap bestaat uit het maken van virtuele Linux-machines in Azure met de nieuwe openbare SSH-sleutel. Virtuele Azure-machines die zijn gemaakt met een openbare SSH-sleutel voor aanmelding, zijn beter beveiligd dan virtuele machines die zijn gemaakt met een wachtwoord als standaardaanmeldingsmethode.

* [Een virtuele Linux-machine maken met de Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een virtuele Linux-machine maken met de Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Maak een Linux-VM met een Azure-sjabloon](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
