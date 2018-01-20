---
title: Inleiding tot FreeBSD in Azure | Microsoft Docs
description: Meer informatie over het gebruik van FreeBSD virtuele machines in Azure
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/13/2017
ms.author: huishao
ms.openlocfilehash: e47a00a8375659f9112da57f9e7a4747f10f8f16
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-freebsd-on-azure"></a>Inleiding tot FreeBSD op Azure
Dit onderwerp bevat een overzicht van een FreeBSD virtuele machine in Azure wordt uitgevoerd.

## <a name="overview"></a>Overzicht
FreeBSD voor Microsoft Azure is een geavanceerde computerbesturingssysteem gebruikt voor power moderne servers, desktops, en ingesloten platforms.

Microsoft Corporation is afbeeldingen van FreeBSD beschikbaar maken op Azure met de [Azure VM-Gastagent](https://github.com/Azure/WALinuxAgent/) vooraf is geconfigureerd. Op dit moment wordt worden de volgende versies van FreeBSD als afbeeldingen aangeboden door Microsoft:

- FreeBSD 10.3-RELEASE
- FreeBSD 11.0-RELEASE
- FreeBSD 11.1-RELEASE

De agent is verantwoordelijk voor de communicatie tussen de VM FreeBSD en de Azure-infrastructuur voor bewerkingen, zoals de inrichting van de virtuele machine bij het eerste gebruik (gebruikersnaam, wachtwoord of SSH-sleutel, hostnaam, enzovoort) en functionaliteit voor selectieve VM-extensies in te schakelen.

Als voor toekomstige versies van FreeBSD is de strategie Blijf en de meest recente versies beschikbaar kort nadat ze zijn gepubliceerd door het technische team van FreeBSD release.

## <a name="deploying-a-freebsd-virtual-machine"></a>Een FreeBSD virtuele machine implementeren
Een FreeBSD virtuele machine implementeert, is een eenvoudig proces met behulp van een afbeelding uit Azure Marketplace vanuit de Azure-portal:

- [FreeBSD 10.3 op de Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD 11.0 op de Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)
- [FreeBSD 11.1 op de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD111)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a>Een FreeBSD virtuele machine via Azure CLI 2.0 in FreeBSD maken
U moet eerst installeren [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) Hoewel de volgende opdracht op een machine FreeBSD.

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

Als bash niet is geïnstalleerd op uw machine FreeBSD, voert u de volgende opdracht voordat de installatie. 

```bash
sudo pkg install bash
```

Als python niet is geïnstalleerd op uw machine FreeBSD, voert u de volgende opdrachten voordat de installatie. 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Tijdens de installatie wordt u gevraagd `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`. Als u beantwoorden `y` en voer `/etc/rc.conf` als `a path to an rc file to update`, u kunt het probleem wellicht voldoen aan `ERROR: [Errno 13] Permission denied`. U lost dit probleem, moet u de voor schrijven verlenen recht op de huidige gebruiker op basis van het bestand `etc/rc.conf`.

U kunt nu aanmelden met Azure en uw FreeBSD VM maken. Hieronder volgt een voorbeeld van een FreeBSD 11.0 virtuele machine maken. U kunt ook de parameter toevoegen `--public-ip-address-dns-name` met een globaal unieke DNS-naam voor een nieuwe openbare IP-adres. 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Vervolgens kunt u aanmelden bij uw FreeBSD VM via het IP-adres dat in de uitvoer van hierboven implementatie afgedrukt. 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>VM-extensies voor FreeBSD
Hieronder vindt u ondersteunde VM-extensies in FreeBSD.

### <a name="vmaccess"></a>VMAccess
De [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) uitbreiding kunt:

* Het wachtwoord van de oorspronkelijke sudo-gebruiker opnieuw instellen.
* Maak een nieuwe gebruiker met sudo met het wachtwoord opgegeven.
* Stel de sleutel van de openbare host met de sleutel die is opgegeven.
* Instellen van de sleutel van de openbare host is opgegeven tijdens het inrichten van virtuele machine als de sleutel van de host is niet opgegeven.
* Open de SSH-poort (22) en herstel van de sshd_config als reset_ssh is ingesteld op true.
* Verwijder de bestaande gebruiker.
* Controleer de schijven.
* Een extra schijf repareren.

### <a name="customscript"></a>CustomScript
De [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) uitbreiding kunt:

* Indien opgegeven, moet u aangepaste scripts downloaden van Azure Storage of een externe openbare opslag (bijvoorbeeld GitHub).
* Voer het script post-punt.
* Ondersteuning voor inline-opdrachten.
* Windows-stijl nieuwe regel in de schaal en Python-scripts automatisch converteren.
* Verwijder BOM automatisch in de shell- en Python-scripts.
* Gevoelige gegevens in CommandToExecute beveiligd.

> [!NOTE]
> FreeBSD VM ondersteunt alleen CustomScript versie 1.x nu.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Verificatie: gebruikersnamen, wachtwoorden en SSH-sleutels
Wanneer u een FreeBSD virtuele machine met behulp van de Azure-portal maakt, moet u een gebruikersnaam, wachtwoord of openbare SSH-sleutel opgeven.
Gebruikersnamen voor het implementeren van een FreeBSD virtuele machine in Azure mag niet overeenkomen met de namen van systeemaccounts (UID < 100) al aanwezig in de virtuele machine ('root', bijvoorbeeld).
Alleen de RSA SSH-sleutel wordt momenteel ondersteund. Een SSH-sleutel met meerdere regels moet beginnen met `---- BEGIN SSH2 PUBLIC KEY ----` en eindigen met `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Het verkrijgen van supergebruiker bevoegdheden
Het gebruikersaccount dat is opgegeven tijdens de implementatie van de virtuele machine-exemplaar in Azure is een bevoegd account. Het pakket van sudo is geïnstalleerd in de installatiekopie van het gepubliceerde FreeBSD.
Nadat u bent aangemeld via dit gebruikersaccount, kunt u opdrachten uitvoeren als hoofdmap met behulp van de syntaxis van de opdracht.

```
$ sudo <COMMAND>
```

U kunt desgewenst een shell basis verkrijgen via `sudo -s`.

## <a name="known-issues"></a>Bekende problemen
De [Azure VM-Gastagent](https://github.com/Azure/WALinuxAgent/) versie 2.2.2 heeft een [bekend probleem] (https://github.com/Azure/WALinuxAgent/pull/517) die ervoor zorgt dat de fout inrichten voor FreeBSD VM op Azure. De oplossing is vastgelegd door [Azure VM-Gastagent](https://github.com/Azure/WALinuxAgent/) versie 2.2.3 en latere versies. 

## <a name="next-steps"></a>Volgende stappen
* Ga naar [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) een FreeBSD VM's te maken.
