---
title: SSH-sleutels maken in Linux en Mac | Microsoft Docs
description: SSH-sleutels genereren en gebruiken in Linux en Mac voor het Resource Manager- en het klassieke implementatiemodel in Azure.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/25/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 689445bc656b5cdebc7689f7fec6e2ea2683576e


---
# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>SSH-sleutels maken in Linux en Mac voor virtuele Linux-machines in Azure
Met een SSH-sleutelpaar kunt u virtuele machines in Azure maken die voor verificatie standaard gebruikmaken van SSH-sleutels, waardoor het niet meer nodig is om met een wachtwoord aan te melden.  Wachtwoorden kunnen worden geraden en stellen uw virtuele machines bloot aan voortdurende aanvalspogingen om uw wachtwoord te raden. Virtuele machines die zijn gemaakt met de Azure-sjablonen of de `azure-cli` kunnen uw openbare SSH-sleutel opnemen als onderdeel van de implementatie, waardoor configuratie na implementatie niet meer nodig is.  Raadpleeg [dit document](virtual-machines-linux-ssh-from-windows.md) als u via Windows verbinding maakt met een virtuele Linux-machine.

Het artikel schrijft het volgende als vereiste voor:

* een Azure-account ([probeer een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure-CLI](../xplat-cli-install.md) die is aangemeld bij `azure login`.
* De Azure-CLI *moet* in de Azure Resource Manager-modus `azure config mode arm` staan.

## <a name="quick-commands"></a>Snelle opdrachten
In de volgende opdrachten kunt u de voorbeelden vervangen door uw eigen gegevens.

SSH-sleutels worden standaard opgeslagen in de `.ssh`-directory.  

```bash
cd ~/.ssh/
```

Als u niet beschikt over een `~/.ssh`-directory, wordt deze door de `ssh-keygen`-opdracht voor u gemaakt met de juiste machtigingen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Voer de naam in van het bestand dat in de map `~/.ssh/` wordt opgeslagen:

```bash
id_rsa
```

Geef de wachtwoordzin op voor id_rsa:

```bash
correct horse battery staple
```

Er is nu een `id_rsa`- en `id_rsa.pub`-SSH-sleutelpaar in de `~/.ssh`-directory.

```bash
ls -al ~/.ssh
```

Voeg de nieuwe sleutel toe aan `ssh-agent` in Linux en Mac (ook toegevoegd aan OSX-sleutelketen):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopieer de openbare SSH-sleutel naar de Linux-server:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Test de aanmelding via sleutels in plaats van een wachtwoord:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
Het gebruik van openbare en persoonlijke SSH-sleutels is de eenvoudigste manier om u aan te melden bij uw Linux-servers. [Cryptografie met openbare sleutels](https://en.wikipedia.org/wiki/Public-key_cryptography) biedt een veel veiligere manier van aanmelding bij uw virtuele Linux- of BSD-machines in Azure dan wachtwoorden, die aanzienlijk eenvoudiger met een brute force-aanval kunnen worden aangevallen. Uw openbare sleutel kan worden gedeeld met iedereen, maar alleen u (of uw lokale beveiligingsinfrastructuur) beschikt over uw persoonlijke sleutel.  De persoonlijke SSH-sleutel moet een [zeer veilig wachtwoord ](https://www.xkcd.com/936/) (bron:[xkcd.com](https://xkcd.com)) hebben om het te beschermen.  Dit wachtwoord dient alleen voor toegang tot de persoonlijke SSH-sleutel en **is niet** het wachtwoord voor het gebruikersaccount.  Wanneer u een wachtwoord aan uw SSH-sleutel toevoegt, wordt de persoonlijke sleutel gecodeerd, zodat deze niet kan worden gebruikt zonder het wachtwoord om deze te ontgrendelen.  Als een aanvaller uw persoonlijke sleutel zou stelen en deze geen wachtwoord zou hebben, zou de aanvaller in staat zijn om de persoonlijke sleutel te gebruiken voor aanmelding bij de servers waarop de bijbehorende openbare sleutel is geïnstalleerd.  Als een persoonlijke sleutel is beveiligd met een wachtwoord, kan deze niet worden gebruikt door de aanvaller. Zo beschikt u over een extra beveiligingslaag voor uw infrastructuur in Azure.

In dit artikel worden *ssh-rsa*-sleutelbestanden gemaakt, die worden aanbevolen voor implementaties in Resource Manager.  *ssh-rsa*-sleutels zijn in de [portal](https://portal.azure.com) vereist voor zowel klassieke als Resource Manager-implementaties.

## <a name="create-the-ssh-keys"></a>SSH-sleutels maken
Azure vereist openbare en persoonlijke sleutels van ten minste 2048 bits in ssh-rsa-indeling. Voor het maken van de sleutels gebruiken we `ssh-keygen`, die een reeks vragen stelt en vervolgens een persoonlijke sleutel en een overeenkomende openbare sleutel schrijft. Wanneer een virtuele Azure-machine wordt gemaakt, wordt de openbare sleutel gekopieerd naar `~/.ssh/authorized_keys`.  SSH-sleutels in `~/.ssh/authorized_keys` worden gebruikt om de client te vragen de bijbehorende persoonlijke sleutel te verstrekken bij verbinding via een SSH-aanmelding.

## <a name="using-sshkeygen"></a>ssh-keygen gebruiken
Met deze opdracht wordt een met een wachtwoord beveiligd (versleuteld) SSH-sleutelpaar gemaakt met 2048-bits RSA waaraan opmerkingen worden toegevoegd om het gemakkelijk te kunnen identificeren.  

Begin met het wijzigen van mappen, zodat al uw SSH-sleutels worden gemaakt in die map.

```bash
cd ~/.ssh
```

Als u niet beschikt over een `~/.ssh`-directory, wordt deze door de `ssh-keygen`-opdracht voor u gemaakt met de juiste machtigingen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

*Uitleg van de opdracht*

`ssh-keygen` = het programma dat wordt gebruikt voor het maken van de sleutels

`-t rsa` = type sleutel dat moet worden gemaakt, namelijk de [RSA-indeling](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = aantal bits van de sleutel

`-C "myusername@myserver"` = een opmerking die wordt toegevoegd aan het einde van het openbare sleutelbestand om het gemakkelijk te identificeren.  Normaal gesproken bestaat de opmerking uit een e-mailadres, maar u kunt ook iets anders gebruiken als dat beter aansluit bij uw infrastructuur.

### <a name="using-pem-keys"></a>Met behulp van PEM-sleutels
Als u het klassieke implementatiemodel gebruikt (klassieke Azure Portal of de Azure Service Management CLI `asm`), moet u mogelijk als PEM opgemaakte SSH-sleutels gebruiken voor toegang tot uw virtuele Linux-machines.  U maakt als volgt een PEM-sleutel op basis van een bestaande openbare SSH-sleutel en een bestaand x509-certificaat.

Een PEM-sleutel maken op basis van een bestaande openbare SSH-sleutel:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-sshkeygen"></a>Voorbeeld van ssh-keygen
```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
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

Opgeslagen sleutelbestanden:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

De naam van het sleutelpaar voor dit artikel.  De naam **id_rsa** is de standaardwaarde voor een sleutelpaar. Sommige programma’s verwachten mogelijk **id_rsa** als naam voor het persoonlijke sleutelbestand, dus is het een goed idee als u er een hebt. De map `~/.ssh/` is de standaardlocatie voor SSH-sleutelparen en het SSH-configuratiebestand.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Een vermelding van de `~/.ssh`-map `ssh-keygen` maakt de `~/.ssh`-map als deze niet aanwezig is en stelt ook het juiste eigendom en de juiste bestandsmodi in.

Sleutelwachtwoord:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` verwijst naar een wachtwoord als 'een wachtwoordzin'.  Het wordt *sterk* aanbevolen een wachtwoord toe te voegen aan uw sleutelparen. Zonder een wachtwoord dat het sleutelpaar beveiligt, kan iedereen met een exemplaar van het persoonlijke sleutelbestand zich aanmelden bij elke server die beschikt over de bijbehorende openbare sleutel. Het toevoegen van een wachtwoord biedt veel meer bescherming in het geval iemand in staat is toegang te krijgen tot uw persoonlijke sleutelbestand. In de extra tijd die dit oplevert, kunt u de sleutels wijzigen waarmee u wordt geverifieerd.

## <a name="using-sshagent-to-store-your-private-key-password"></a>ssh-agent gebruiken voor het opslaan van het wachtwoord van uw persoonlijke sleutel
Om te voorkomen dat u het wachtwoord van uw persoonlijke sleutelbestand bij elke SSH-aanmelding moet opgeven, kunt u `ssh-agent` gebruiken om het wachtwoord van uw persoonlijke sleutelbestand in de cache te plaatsen. Als u een Mac gebruikt, worden de wachtwoorden van uw persoonlijke sleutels veilig opgeslagen door Sleutelhangertoegang van OSX wanneer u `ssh-agent` aanroept.

Controleer eerst of `ssh-agent` wordt uitgevoerd.

```bash
eval "$(ssh-agent -s)"
```

Voeg nu de persoonlijke sleutel toe aan `ssh-agent` met de opdracht `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Het wachtwoord voor persoonlijke sleutel wordt nu opgeslagen in `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Een SSH-configuratiebestand maken en configureren
Het wordt aanbevolen om een `~/.ssh/config`-bestand te maken en te configureren om aanmeldingen te versnellen en het gedrag van uw SSH-clientgedrag te optimaliseren.

In het volgende voorbeeld wordt een standaardconfiguratie getoond.

### <a name="create-the-file"></a>Het bestand maken
```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bewerk het bestand om de nieuwe SSH-configuratie toe te voegen:
```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Voorbeeld van `~/.ssh/config`-bestand:
```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Deze SSH-configuratie bevat afzonderlijke secties voor elke server, zodat elke server kan beschikken over een eigen toegewezen sleutelpaar. De standaardinstellingen (`Host *`) zijn voor alle hosts die niet overeenkomen met een van de specifieke hosts die hoger in het configuratiebestand staan.

### <a name="config-file-explained"></a>Uitleg van configuratiebestand
`Host` = de naam van de host die wordt aangeroepen op de terminal.  `ssh fedora22` vertelt `SSH` dat de waarden moeten worden gebruikt in het instellingenblok met het label `Host fedora22` Opmerking: dit kan elk label zijn dat logisch is voor uw gebruik. Het label vertegenwoordigt niet de werkelijke hostnaam van een server.

`Hostname 102.160.203.241` = het IP-adres of de DNS-naam van de server die wordt benaderd.

`User myusername` = het account van de externe gebruiker dat moet worden gebruikt bij het aanmelden bij de server.

`PubKeyAuthentication yes` = vertelt SSH dat u gebruik wilt maken van een SSH-sleutel voor aanmelding.

`IdentityFile /home/myusername/.ssh/id_id_rsa` = de persoonlijke SSH-sleutel en de bijbehorende openbare sleutel te gebruiken voor verificatie.

## <a name="ssh-into-linux-without-a-password"></a>SSH in Linux zonder een wachtwoord
Nu u een SSH-sleutelpaar en een geconfigureerd SSH-configuratiebestand hebt, kunt u zich snel en veilig aanmelden bij uw virtuele Linux-machine. De eerste keer dat u zich met een SSH-sleutel bij een server aanmeldt, wordt u gevraagd naar de wachtwoordzin voor dat sleutelbestand.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Uitleg van de opdracht
Wanneer `ssh fedora22` wordt uitgevoerd, zoekt en laadt SSH eerst instellingen uit het `Host fedora22`-blok en laadt het vervolgens alle overige instellingen uit het laatste blok, `Host *`.

## <a name="next-steps"></a>Volgende stappen
De volgende stap bestaat uit het maken van virtuele Linux-machines in Azure met de nieuwe openbare SSH-sleutel.  Virtuele Azure-machines die zijn gemaakt met een openbare SSH-sleutel voor aanmelding, zijn beter beveiligd dan virtuele machines die zijn gemaakt met een wachtwoord als standaardaanmeldingsmethode.  Virtuele Azure-machines die zijn gemaakt met behulp van SSH-sleutels zijn standaard geconfigureerd met wachtwoorden uitgeschakeld, waarmee brute force-aanvallen om wachtwoorden te raden worden voorkomen.

* [Een beveiligde virtuele Linux-machine maken met een Azure-sjabloon](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Een beveiligde virtuele Linux-machine maken met Azure Portal](virtual-machines-linux-quick-create-portal.md)
* [Een beveiligde virtuele Linux-machine maken met de Azure CLI](virtual-machines-linux-quick-create-cli.md)




<!--HONumber=Nov16_HO2-->


