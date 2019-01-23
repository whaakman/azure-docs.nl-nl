---
title: Verplaats bestanden naar en van Azure Linux VM's met SCP | Microsoft Docs
description: Veilig verplaatsen bestanden naar en van een Linux-VM in Azure met behulp van de SCP en een SSH-sleutelpaar.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.component: disks
ms.openlocfilehash: 1f186e410718ce30f48602ce907afb6206597638
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465765"
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a>Verplaatsen van bestanden en naar een Linux-VM met SCP

Dit artikel leest hoe u bestanden verplaatst vanuit uw werkstation tot een virtuele Azure Linux-machine of vanuit een Azure Linux-VM naar uw werkstation met behulp van Secure Copy (SCP). Verplaatsen van bestanden tussen uw werkstation en een Linux-VM, snel en veilig, is van essentieel belang voor het beheren van uw Azure-infrastructuur. 

Voor dit artikel, moet u een Linux VM die is geïmplementeerd in Azure met [SSH openbare en persoonlijke sleutelbestanden](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). U moet ook een SCP-client voor de lokale computer. Het is gebaseerd op SSH en opgenomen in de standaard-Bash-shell met de meeste Linux en Mac-computers en sommige shells Windows.

## <a name="quick-commands"></a>Snelle opdrachten

Een bestand naar de Linux-VM kopiëren

```bash
scp file azureuser@azurehost:directory/targetfile
```

Een bestand af van de Linux-VM kopiëren

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

Als voorbeelden, gaan we een Azure-configuratiebestand maximaal een Linux-VM en de vervolgkeuzelijst een logboekmap, die beide gebruikmaken SCP en SSH-sleutels.   

## <a name="ssh-key-pair-authentication"></a>SSH-sleutelpaar-verificatie

SCP wordt SSH gebruikt voor de transportlaag is opgehaald. SSH verwerkt de verificatie op de doelhost en wordt het bestand in een gecodeerde tunnel geboden door standaard met SSH verplaatst. Voor de SSH-verificatie, de gebruikersnamen en wachtwoorden kunnen worden gebruikt. SSH openbare en persoonlijke sleutel verificatie worden echter wel aanbevolen als aanbevolen beveiligingsprocedure. Als SSH heeft de verbinding geverifieerd, begint SCP vervolgens kopiëren van het bestand. Met behulp van een correct geconfigureerde `~/.ssh/config` en SSH-openbare en persoonlijke sleutels, de SCP-verbinding kunnen worden gemaakt met behulp van alleen een servernaam (of IP-adres). Als u slechts één SSH-sleutel hebt, SCP zoekt in de `~/.ssh/` directory, en gebruikt voor het standaard aanmelden bij de virtuele machine.

Voor meer informatie over het configureren van uw `~/.ssh/config` en SSH-openbare en persoonlijke sleutels, Zie [SSH-sleutels maken](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-to-a-linux-vm"></a>SCP een bestand met een Linux VM

Voor het eerste voorbeeld kopieert u een Azure-configuratie-bestand naar een Linux-VM die wordt gebruikt voor het implementeren van automation. Beveiliging is belangrijk, omdat dit bestand bevat API van Azure-referenties, waaronder geheimen. De gecodeerde geleverd door de SSH-tunnel beveiligt de inhoud van het bestand.

Met de volgende opdracht wordt de lokale *.azure/config* bestand met een Azure-VM met FQDN-naam *myserver.eastus.cloudapp.azure.com*. De gebruikersnaam van beheerder op de Azure VM is *azureuser*. Het bestand is gericht op de */home/azureuser/* directory. Vervangen door uw eigen waarden in deze opdracht.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>SCP een directory vanaf een Linux-VM

In dit voorbeeld wordt een map met logboekbestanden van de Linux-VM naar uw werkstation kopiëren. Een logboekbestand kan of kan geen vertrouwelijke of geheime gegevens bevatten. Echter met behulp van SCP zorgt ervoor dat de inhoud van de logboekbestanden worden versleuteld. SCP gebruik voor het overbrengen van de bestanden is de eenvoudigste manier om op te halen van de logboekmap en bestanden op uw werkstation terwijl u ook beveiligd.

De volgende opdracht kopieert bestanden in de */home/azureuser/logboeken/* map op de Azure-VM naar de lokale map/TMP-map:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

De `-r` cli-vlag opdracht SCP recursief Kopieer de bestanden en mappen vanaf het moment van de directory die worden vermeld in de opdracht.  Ook ziet u dat de syntaxis van de opdrachtregel vergelijkbaar met is een `cp` -opdracht kopiëren.

## <a name="next-steps"></a>Volgende stappen

* [Beheren van gebruikers, SSH en controleer of het herstellen van schijven op virtuele Azure Linux-machines met behulp van de VMAccess-extensie](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)