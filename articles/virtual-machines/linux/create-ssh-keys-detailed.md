---
title: Gedetailleerde stappen om in Azure een SSH-sleutelpaar te maken voor virtuele Linux-machines | Microsoft Docs
description: Informatie over extra stappen om in Azure een openbaar en persoonlijk SSH-sleutelpaar voor virtuele Linux-machines te maken, samen met specifieke certificaten voor verschillende gebruiksvoorbeelden.
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.openlocfilehash: d4548c6f21d04effd57ea36e4fc0d15f77568903
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="detailed-walk-through-to-create-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a>Gedetailleerd stappenplan voor het maken van een SSH-sleutelpaar en extra certificaten voor een virtuele Linux-machine in Azure
Met een SSH-sleutelpaar kunt u virtuele machines in Azure maken die voor verificatie standaard gebruikmaken van SSH-sleutels, waardoor aanmelding met een wachtwoord niet meer nodig is. Wachtwoorden kunnen worden geraden en stellen uw virtuele machines bloot aan voortdurende aanvalspogingen om uw wachtwoord te raden. Virtuele machines die zijn gemaakt met de Azure CLI- of Resource Manager-sjablonen, kunnen uw openbare SSH-sleutel opnemen als onderdeel van de implementatie, waardoor configuratie na implementatie niet meer nodig is voor het uitschakelen van aanmelding met een wachtwoord voor SSH. Dit artikel vindt gedetailleerde stappen en aanvullende voorbeelden gegeven van certificaten genereren, zoals voor gebruik met de virtuele Linux-machines. Als u snel een SSH-sleutelpaar wilt maken en gebruiken, raadpleegt u [Een openbaar en persoonlijk SSH-sleutelpaar maken voor Linux-VM's in Azure](mac-create-ssh-keys.md).

## <a name="understanding-ssh-keys"></a>Inzicht in SSH-sleutels

Het gebruik van openbare en persoonlijke SSH-sleutels is de eenvoudigste manier om u aan te melden bij uw Linux-servers. [Cryptografie met openbare sleutels](https://en.wikipedia.org/wiki/Public-key_cryptography) biedt een veel veiligere manier van aanmelding bij uw virtuele Linux- of BSD-machines in Azure dan wachtwoorden, die aanzienlijk eenvoudiger met een brute force-aanval kunnen worden aangevallen.

Uw openbare sleutel kan worden gedeeld met iedereen, maar alleen u (of uw lokale beveiligingsinfrastructuur) beschikt over uw persoonlijke sleutel.  De privé-SSH-sleutel moet ter beveiliging een [zeer veilig wachtwoord](https://www.xkcd.com/936/) hebben (bron:[xkcd.com](https://xkcd.com)).  Dit wachtwoord dient alleen voor toegang tot het persoonlijke SSH-sleutelbestand en is **niet** het wachtwoord voor het gebruikersaccount.  Wanneer u een wachtwoord aan de SSH-sleutel toevoegt, wordt de privésleutel versleuteld met behulp van 128-bits AES, zodat deze niet kan worden gebruikt zonder het wachtwoord om de privésleutel te ontsleutelen.  Als een aanvaller uw persoonlijke sleutel zou stelen en deze geen wachtwoord zou hebben, zou de aanvaller in staat zijn om de persoonlijke sleutel te gebruiken voor aanmelding bij de servers waarop de bijbehorende openbare sleutel is geïnstalleerd.  Als een persoonlijke sleutel is beveiligd met een wachtwoord, kan deze niet worden gebruikt door de aanvaller. Zo beschikt u over een extra beveiligingslaag voor uw infrastructuur in Azure.

In dit artikel wordt een openbaar en persoonlijk sleutelpaarbestand met SSH-protocolversie 2 RSA gemaakt (ook wel 'SSH-RSA-sleutels' genoemd), dat wordt aanbevolen voor implementaties met Azure Resource Manager. *ssh-rsa*-sleutels zijn vereist in de [portal](https://portal.azure.com) voor zowel de klassieke implementatie als de implementatie voor resourcebeheer.

## <a name="ssh-keys-use-and-benefits"></a>Gebruik en voordelen van SSH-sleutels

Voor Azure zijn openbare en persoonlijke sleutels van minimaal 2048-bits, met SSH-protocol versie 2 en met RSA-indeling vereist. Het openbare-sleutelbestand heeft de `.pub`-containerindeling. Voor het maken van de sleutels gebruiken we `ssh-keygen`, die een reeks vragen stelt en vervolgens een persoonlijke sleutel en een overeenkomende openbare sleutel schrijft. Wanneer een virtuele Azure-machine wordt gemaakt, kopieert Azure de openbare sleutel naar de map `~/.ssh/authorized_keys` op de virtuele machine. SSH-sleutels in `~/.ssh/authorized_keys` worden gebruikt om de client te vragen de bijbehorende persoonlijke sleutel te verstrekken bij verbinding via een SSH-aanmelding.  Wanneer er in Azure een virtuele Linux-machine wordt gemaakt met SSH-sleutels voor verificatie, configureert Azure de SSHD-server zodanig dat aanmelding met een wachtwoord niet wordt toegestaan en alleen SSH-sleutels worden toegestaan.  Daarom kunt u helpen de VM-implementatie te beveiligen door virtuele Azure-machines in Linux met SSH-sleutels te maken. Ook kunt u uzelf de typische configuratiestap besparen die moet worden uitgevoerd na de implementatie: wachtwoorden uitschakelen in het bestand **sshd_config**.

## <a name="using-ssh-keygen"></a>ssh-keygen gebruiken

Met deze opdracht wordt een met een wachtwoord beveiligd (versleuteld) SSH-sleutelpaar gemaakt met 2048-bits RSA waaraan opmerkingen worden toegevoegd om het gemakkelijk te kunnen identificeren.  

SSH-sleutels worden standaard opgeslagen in de `~/.ssh`-directory.  Als u niet beschikt over de map `~/.ssh`, wordt deze door de opdracht `ssh-keygen` voor u gemaakt met de juiste machtigingen.

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

*Uitleg van de opdracht*

`ssh-keygen` = het programma dat wordt gebruikt voor het maken van de sleutels

`-t rsa`= type sleutel maken die is de RSA-indeling [wikipedia][parens aan einde](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` = aantal bits van de sleutel

`-C "azureuser@myserver"` = een opmerking die wordt toegevoegd aan het einde van het openbare sleutelbestand om het gemakkelijk te identificeren.  Normaal gesproken bestaat de opmerking uit een e-mailadres, maar u kunt ook iets anders gebruiken als dat beter aansluit bij uw infrastructuur.

## <a name="classic-deploy-using-asm"></a>Klassieke implementatie met behulp van`asm`

Als u van het klassieke implementatiemodel gebruikmaakt (`asm` modus in de CLI), kunt u een openbare SSH-RSA-sleutel of een RFC4716 geformatteerd sleutel in een pem-container.  De openbare SSH-RSA-sleutel is de sleutel die eerder in dit artikel is gemaakt met behulp van `ssh-keygen`.

Een RFC4716-sleutel maken van een bestaande openbare SSH-sleutel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Voorbeeld van ssh-keygen

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

Opgeslagen sleutelbestanden:

`Enter file in which to save the key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

De naam van het sleutelpaar voor dit artikel.  De naam **id_rsa** is de standaardwaarde voor een sleutelpaar. Sommige programma’s verwachten mogelijk **id_rsa** als naam voor het persoonlijke sleutelbestand, dus is het een goed idee als u er een hebt. De map `~/.ssh/` is de standaardlocatie voor SSH-sleutelparen en het SSH-configuratiebestand.  Als `ssh-keygen` niet wordt opgegeven met een volledig pad, worden de sleutels in de huidige werkmap gemaakt in plaats van in de standaardmap `~/.ssh`.

Een vermelding van de `~/.ssh`-map

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

Sleutelwachtwoord:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` verwijst naar een wachtwoord voor de privésleutel (een zogenaamde 'wachtwoordzin').  Het wordt *sterk* aanbevolen een wachtwoord toe te voegen aan uw privésleutel. Zonder een wachtwoord dat het sleutelbestand beveiligt, kan iedereen met een exemplaar van het bestand zich aanmelden bij elke server die beschikt over de bijbehorende openbare sleutel. Het toevoegen van een wachtwoord (wachtwoordzin) biedt veel meer bescherming in het geval iemand in staat is toegang te krijgen tot uw persoonlijke sleutelbestand. In de extra tijd die dit oplevert, kunt u de sleutels wijzigen waarmee u wordt geverifieerd.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>ssh-agent gebruiken voor het opslaan van het wachtwoord van uw persoonlijke sleutel

Om te voorkomen dat u het wachtwoord van uw persoonlijke sleutelbestand bij elke SSH-aanmelding moet opgeven, kunt u `ssh-agent` gebruiken om het wachtwoord van uw persoonlijke sleutelbestand in de cache te plaatsen. Als u een Mac gebruikt, worden de wachtwoorden van uw persoonlijke sleutels veilig opgeslagen door Sleutelhangertoegang van OSX wanneer u `ssh-agent` aanroept.

Controleer en gebruik ssh-agent en ssh-add om het SSH-systeem te informeren over de sleutelbestanden, zodat de wachtwoordzin niet interactief hoeft te worden gebruikt.

```bash
eval "$(ssh-agent -s)"
```

Voeg nu de persoonlijke sleutel toe aan `ssh-agent` met de opdracht `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Het wachtwoord voor persoonlijke sleutel wordt nu opgeslagen in `ssh-agent`.

## <a name="using-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>`ssh-copy-id` gebruiken om de sleutel te kopiëren naar een bestaande virtuele machine
Als u al een virtuele machine hebt gemaakt, kunt u de nieuwe openbare SSH-sleutel installeren op uw virtuele Linux-machine met:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

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
  User ahmet
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

`Host` = de naam van de host die wordt aangeroepen op de terminal.  `ssh fedora22` vertelt `SSH` dat de waarden moeten worden gebruikt in het instellingenblok met het label `Host fedora22` OPMERKING: de host kan elk label zijn dat logisch is voor uw gebruik. Het label vertegenwoordigt niet de werkelijke hostnaam van een server.

`Hostname 102.160.203.241` = het IP-adres of de DNS-naam van de server die wordt benaderd.

`User ahmet` = het account van de externe gebruiker dat moet worden gebruikt bij het aanmelden bij de server.

`PubKeyAuthentication yes` = vertelt SSH dat u gebruik wilt maken van een SSH-sleutel voor aanmelding.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` = de persoonlijke SSH-sleutel en de bijbehorende openbare sleutel te gebruiken voor verificatie.

## <a name="ssh-into-linux-without-a-password"></a>SSH in Linux zonder een wachtwoord

Nu u een SSH-sleutelpaar en een geconfigureerd SSH-configuratiebestand hebt, kunt u zich snel en veilig aanmelden bij uw virtuele Linux-machine. De eerste keer dat u zich met een SSH-sleutel bij een server aanmeldt, wordt u gevraagd naar de wachtwoordzin voor dat sleutelbestand.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Uitleg van de opdracht

Wanneer `ssh fedora22` wordt uitgevoerd, zoekt en laadt SSH eerst instellingen uit het `Host fedora22`-blok en laadt het vervolgens alle overige instellingen uit het laatste blok, `Host *`.

## <a name="next-steps"></a>Volgende stappen

De volgende stap bestaat uit het maken van virtuele Linux-machines in Azure met de nieuwe openbare SSH-sleutel.  Virtuele Azure-machines die zijn gemaakt met een openbare SSH-sleutel voor aanmelding, zijn beter beveiligd dan virtuele machines die zijn gemaakt met een wachtwoord als standaardaanmeldingsmethode.  Virtuele Azure-machines die zijn gemaakt met behulp van SSH-sleutels zijn standaard geconfigureerd met wachtwoorden uitgeschakeld, waarmee brute force-aanvallen om wachtwoorden te raden worden voorkomen.

* [Een beveiligde virtuele Linux-machine maken met een Azure-sjabloon](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een beveiligde virtuele Linux-machine maken met Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een beveiligde virtuele Linux-machine maken met de Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
