---
title: Inleiding tot FreeBSD op Azure | Microsoft Docs
description: Meer informatie over het gebruik van FreeBSD virtuele machines op Azure
services: virtual-machines-linux
documentationcenter: ''
author: thomas1206
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/13/2017
ms.author: huishao
ms.openlocfilehash: e6f284b2a3483b21901dc4ad26bff83b8fcd8848
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843016"
---
# <a name="introduction-to-freebsd-on-azure"></a>Inleiding tot FreeBSD op Azure
Dit artikel bevat een overzicht van het uitvoeren van FreeBSD virtuele machines in Azure.

## <a name="overview"></a>Overzicht
FreeBSD voor Microsoft Azure is een geavanceerde computerbesturingssysteem gebruikt met power moderne servers, desktops, en ingesloten platforms.

Microsoft Corporation is afbeeldingen van FreeBSD het beschikbaar maken op Azure met de [Azure VM-Gastagent](https://github.com/Azure/WALinuxAgent/) vooraf is geconfigureerd. De volgende versies van FreeBSD worden momenteel aangeboden als installatiekopieën door Microsoft:

- FreeBSD 10.3-RELEASE
- FreeBSD 10.4-RELEASE
- FreeBSD 11.1-RELEASE

De agent is verantwoordelijk voor de communicatie tussen de FreeBSD-VM en de Azure-infrastructuur voor bewerkingen zoals het inrichten van de virtuele machine op de eerste keer wordt gebruikt (gebruikersnaam, wachtwoord of SSH-sleutel, hostnaam, enzovoort) en functionaliteit voor selectief VM-extensies in te schakelen.

De strategie bestaat als voor toekomstige versies van FreeBSD op de hoogte blijven en de meest recente versies beschikbaar kort nadat ze zijn gepubliceerd door het engineeringteam van FreeBSD release.

## <a name="deploying-a-freebsd-virtual-machine"></a>Een FreeBSD virtuele machines implementeren
Een FreeBSD virtuele machine implementeert, is een eenvoudig proces met een installatiekopie uit de Azure Marketplace in Azure portal:

- [FreeBSD 10.4 op Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD104)
- [FreeBSD 11.1 op Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD111)
- [FreeBSD 11.2 op Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD112)

### <a name="create-a-freebsd-vm-through-azure-cli-on-freebsd"></a>Een FreeBSD virtuele machine via Azure CLI op FreeBSD maken
U moet eerst installeren [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) al de volgende opdracht op een machine FreeBSD.

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

Als bash is niet geïnstalleerd op uw computer toegang tot FreeBSD, voert u de volgende opdracht voordat de installatie. 

```bash
sudo pkg install bash
```

Als python is niet geïnstalleerd op uw computer toegang tot FreeBSD, voert u de volgende opdrachten voordat de installatie. 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Tijdens de installatie wordt u gevraagd `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`. Als u beantwoorden `y` en voer `/etc/rc.conf` als `a path to an rc file to update`, u mogelijk voldoet aan het probleem `ERROR: [Errno 13] Permission denied`. U lost dit probleem, moet u de schrijven verlenen recht op de huidige gebruiker op basis van het bestand `etc/rc.conf`.

U kunt nu aanmelden bij Azure en maak uw FreeBSD-VM. Hieronder volgt een voorbeeld om een FreeBSD 11.0 VM te maken. U kunt ook de parameter toevoegen `--public-ip-address-dns-name` met een unieke DNS-naam voor een nieuwe openbare IP-adres. 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Vervolgens kunt u aanmelden met uw VM FreeBSD via de ip-adres dat in de uitvoer van bovenstaande implementatie afgedrukt. 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>VM-extensies voor FreeBSD
Hieronder vindt u ondersteunde VM-extensies in FreeBSD.

### <a name="vmaccess"></a>VMAccess
De [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extensie kunt:

* Het wachtwoord van de oorspronkelijke sudo-gebruiker opnieuw instellen.
* Maak een nieuwe gebruiker met sudo met het opgegeven wachtwoord.
* Stel de sleutel van de openbare host met de sleutel die is opgegeven.
* Opnieuw instellen van de openbare hostsleutel die is opgegeven tijdens het inrichten van VM's als de hostsleutel niet is opgegeven.
* Open de SSH-poort (22) en deze herstellen van de sshd_config als reset_ssh is ingesteld op true.
* De bestaande gebruiker te verwijderen.
* Schijven controleren.
* Herstellen van een extra schijf.

### <a name="customscript"></a>CustomScript
De [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extensie kunt:

* Indien opgegeven, moet u de aangepaste scripts downloaden via Azure Storage of externe openbare opslag (bijvoorbeeld GitHub).
* Het invoerpunt-script uitvoeren.
* Ondersteuning voor inline-opdrachten.
* Windows-stijl newline in shell- en Python-scripts automatisch converteren.
* Stuklijst automatisch in shell- en Python-scripts verwijderen.
* Gevoelige gegevens in een CommandToExecute beveiligen.

> [!NOTE]
> FreeBSD VM biedt alleen ondersteuning voor CustomScript versie 1.x nu.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Verificatie: gebruikersnamen, wachtwoorden en SSH-sleutels
Wanneer u een FreeBSD virtuele machines met behulp van Azure portal maakt, moet u een gebruikersnaam, wachtwoord of openbare SSH-sleutel opgeven.
Gebruikersnamen voor het implementeren van een FreeBSD virtuele machines in Azure mag niet overeenkomen met de namen van systeemaccounts (UID < 100) al aanwezig in de virtuele machine ('root', bijvoorbeeld).
Op dit moment wordt alleen de RSA SSH-sleutel ondersteund. Een SSH-sleutel met meerdere regels moet beginnen met `---- BEGIN SSH2 PUBLIC KEY ----` en eindigen met `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Het ophalen van supergebruikersbevoegdheden
Het gebruikersaccount dat is opgegeven tijdens de implementatie van de virtuele machine-exemplaar op Azure is een bevoegd account. Het pakket van sudo is geïnstalleerd in een installatiekopie van het gepubliceerde FreeBSD.
Nadat u bent aangemeld via dit gebruikersaccount, kunt u opdrachten als root uitvoeren met behulp van de opdrachtsyntaxis.

```
$ sudo <COMMAND>
```

U kunt eventueel een shell hoofdmap verkrijgen met behulp van `sudo -s`.

## <a name="known-issues"></a>Bekende problemen
De [Azure VM-Gastagent](https://github.com/Azure/WALinuxAgent/) versie 2.2.2 heeft een [bekende probleem](https://github.com/Azure/WALinuxAgent/pull/517) dat ervoor zorgt dat de fout inrichten voor FreeBSD VM op Azure. De oplossing zijn vastgelegd door [Azure VM-Gastagent](https://github.com/Azure/WALinuxAgent/) versie 2.2.3 en latere versies. 

## <a name="next-steps"></a>Volgende stappen
* Ga naar [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD111) om een FreeBSD-VM te maken.
