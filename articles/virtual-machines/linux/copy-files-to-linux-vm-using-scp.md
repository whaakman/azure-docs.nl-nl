---
title: Bestanden van en naar Azure Linux VM's met SCP verplaatsen | Microsoft Docs
description: Veilig verplaatsen bestanden naar en van een Linux-VM in Azure met behulp van SCP en een SSH-sleutelpaar.
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
ms.openlocfilehash: 0231e402848e617a46ca70470ba4d3272ace59f7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30904502"
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a>Bestanden van en naar een Linux-VM met SCP verplaatsen

In dit artikel laat zien hoe voor het verplaatsen van bestanden van uw werkstation tot een Azure Linux VM of van een Azure Linux VM naar beneden op uw werkstation met behulp van Secure kopiëren (SCP). Verplaatsen van bestanden tussen uw werkstation en Linux-VM snel en veilig is essentieel voor het beheren van uw Azure-infrastructuur. 

Dit artikel, moet u een Linux-VM is geïmplementeerd in Azure worden verkregen met [SSH openbare en persoonlijke sleutelbestanden](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). U moet ook een SCP-client voor de lokale computer. Het is gebouwd op SSH en opgenomen in de standaard Bash-shell van de meeste Linux en Mac-computers en sommige houders van Windows.

## <a name="quick-commands"></a>Snelle opdrachten

Kopieer een bestand naar de Linux-VM

```bash
scp file azureuser@azurehost:directory/targetfile
```

Kopieer een bestand af van de Linux-VM

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

Als voorbeelden, gaan we een Azure-configuratiebestand naar een Linux-VM en de vervolgkeuzelijst een logboekmap beide SCP en de SSH-sleutels gebruikmaken van.   

## <a name="ssh-key-pair-authentication"></a>SSH-sleutelpaar verificatie

SSH SCP gebruikt voor de transportlaag. SSH verwerkt de verificatie op de doelhost en verplaatst het bestand in een versleutelde standaard meegeleverd met SSH-tunnel. Voor SSH-verificatie, de gebruikersnamen en wachtwoorden kunnen worden gebruikt. SSH openbare en persoonlijke sleutel verificatie worden echter aanbevolen als aanbevolen beveiligingsprocedure. Als SSH heeft de verbinding geverifieerd, begint SCP vervolgens kopiëren van het bestand. Met behulp van een correct geconfigureerde `~/.ssh/config` en SSH-openbare en persoonlijke sleutels, de SCP-verbinding kunnen worden gemaakt met behulp van slechts een servernaam (of IP-adres). Als u slechts één SSH-sleutel hebt, SCP zoekt in de `~/.ssh/` directory, en wordt standaard gebruikt voor aanmelding bij de virtuele machine.

Voor meer informatie over het configureren van uw `~/.ssh/config` en SSH-openbare en persoonlijke sleutels, Zie [maken SSH-sleutels](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-to-a-linux-vm"></a>SCP een bestand met een Linux-VM

Voor het eerste voorbeeld kopieert u een Azure-configuratiebestand naar een Linux-VM die wordt gebruikt voor het implementeren van automation. Beveiliging is belangrijk, omdat dit bestand bevat de Azure-API-referenties, waaronder geheimen. De gecodeerde geleverd door de SSH-tunnel beveiligt de inhoud van het bestand.

De volgende opdracht wordt de lokale *.azure/config* bestand naar een Azure-VM met FQDN-naam *myserver.eastus.cloudapp.azure.com*. De gebruikersnaam van de beheerder op de Azure VM is *azureuser*. Het bestand is gericht op de */home/azureuser/* directory. Vervangt door uw eigen waarden in deze opdracht.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>SCP een map van een Linux-VM

In dit voorbeeld wordt een map met logbestanden van de Linux-VM naar beneden op uw werkstation kopiëren. Een logboekbestand kan of kan geen gevoelige of geheime gegevens. Echter, SCP zorgt u ervoor dat de inhoud van de logboekbestanden worden versleuteld. De bestanden over te brengen met behulp van SCP is de eenvoudigste manier om op te halen van de logboekmap en bestanden naar uw werkstation terwijl tegelijkertijd ook beveiligd.

De volgende opdracht kopieert de bestanden in de *thuis-/azureuser/logboeken/* directory op de virtuele machine van Azure naar de map van de lokale map:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

De `-r` cli vlag opdracht SCP recursief Kopieer de bestanden en mappen vanaf het moment van de directory die worden vermeld in de opdracht.  Let ook op de opdrachtregelsyntaxis is vergelijkbaar met een `cp` -opdracht kopiëren.

## <a name="next-steps"></a>Volgende stappen

* [Beheren van gebruikers, SSH en controleer of het herstellen van schijven op Azure Linux VM's met behulp van de VMAccess-extensie](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)