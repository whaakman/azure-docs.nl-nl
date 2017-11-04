---
title: Extern bureaublad voor een virtuele Linux-machine in Azure gebruiken | Microsoft Docs
description: Meer informatie over het installeren en configureren van extern bureaublad (xrdp) verbinding maken met een Linux VM in Azure met grafische hulpprogramma 's
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: iainfou
ms.openlocfilehash: d8d6130a270285c84c1dd057a3512cdeb39287f6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a>Installeren en configureren van extern bureaublad verbinding maken met een Linux VM in Azure
Linux virtuele machines (VM's) in Azure worden meestal beheerd vanaf de opdrachtregel met behulp van een verbinding met secure shell (SSH). Wanneer er nieuwe voor Linux, of om snel scenario's voor het oplossen van problemen, kan het gebruik van extern bureaublad eenvoudiger zijn. Dit artikel wordt uitgelegd hoe u kunt installeren en configureren van een bureaublad-omgeving ([xfce](https://www.xfce.org)) en extern bureaublad ([xrdp](http://www.xrdp.org)) voor uw Linux-VM met het implementatiemodel van Resource Manager.


## <a name="prerequisites"></a>Vereisten
In dit artikel is vereist voor een bestaande Linux VM in Azure. Als u maken van een virtuele machine wilt, een van de volgende methoden gebruiken:

- De [Azure CLI 2.0](quick-create-cli.md)
- De [Azure-portal](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Een bureaubladomgeving installeren op uw Linux-VM
De meeste Linux virtuele machines in Azure beschikt niet over een bureaubladomgeving standaard geïnstalleerd. Linux VM's worden meestal beheerd met behulp van SSH-verbindingen in plaats van een bureaublad-omgeving. Er zijn verschillende bureaubladomgevingen in Linux die u kunt kiezen. Afhankelijk van uw keuze van bureaubladomgeving van het één tot 2 GB aan schijfruimte in beslag nemen en 5 tot 10 minuten installeren en configureren van de vereiste pakketten.

Het volgende voorbeeld installeert de lightweight [xfce4](https://www.xfce.org/) bureaubladomgeving op een Ubuntu VM. Opdrachten voor andere distributies enigszins verschillen (Gebruik `yum` op Red Hat Enterprise Linux installeren en configureren van juiste `selinux` regels of gebruik `zypper` installeren op SUSE, bijvoorbeeld).

Eerste, SSH met uw virtuele machine. Het volgende voorbeeld maakt verbinding met de virtuele machine met de naam *myvm.westus.cloudapp.azure.com* aan de gebruikersnaam van *azureuser*:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u van Windows gebruikmaakt en meer informatie over het gebruik van SSH nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows](ssh-from-windows.md).

Vervolgens installeert met behulp van xfce `apt` als volgt:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Installeren en configureren van een extern bureaublad-server
Nu dat u een bureaubladomgeving geïnstalleerd hebt, configureert u een extern bureaublad-services om te luisteren naar binnenkomende verbindingen. [xrdp](http://xrdp.org) is een open-source Remote Desktop Protocol (RDP)-server die beschikbaar is op de meeste Linux-distributies en werkt goed samen met xfce. Installeer xrdp op uw Ubuntu VM als volgt:

```bash
sudo apt-get install xrdp
```

Vertel xrdp welke bureaubladomgeving moet worden gebruikt wanneer u uw sessie starten. Xrdp voor het gebruik van xfce als de bureaubladomgeving als volgt configureren:

```bash
echo xfce4-session >~/.xsession
```

Start de service xrdp om de wijzigingen van kracht als volgt opnieuw:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Wachtwoord voor een lokale gebruikersaccount instellen
Als u een wachtwoord voor uw gebruikersaccount gemaakt toen u uw virtuele machine hebt gemaakt, kunt u deze stap overslaan. Als u alleen verificatie van SSH-sleutel en niet het wachtwoord van een lokale account instellen hoeft, een wachtwoord opgeven voordat u xrdp gebruiken voor aanmelding bij uw virtuele machine. xrdp accepteren niet SSH-sleutels voor verificatie. Het volgende voorbeeld wordt een wachtwoord voor het gebruikersaccount *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Een wachtwoord opgeeft, wordt uw configuratie SSHD wachtwoord aanmeldingen toestaan als dit momenteel niet niet bijgewerkt. U kunt vanuit het beveiligingsoogpunt van wilt verbinden met uw virtuele machine met een SSH-tunnel met verificatie op basis van sleutels en maak verbinding met xrdp. Als dit het geval is, moet u de volgende stap overslaan over het maken van een groep van de netwerkbeveiligingsregel voor extern bureaublad-verkeer.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Een Netwerkbeveiligingsgroep regel maken voor extern bureaublad-verkeer
Voor extern bureaublad-verkeer te bereiken van uw Linux-VM een netwerkbeveiliging kunt groep regel moet worden gemaakt die TCP op poort 3389 bereiken van uw virtuele machine. Zie voor meer informatie over netwerkbeveiligingsgroepen [wat is er een Netwerkbeveiligingsgroep?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) U kunt ook [Azure portal gebruiken voor het maken van een groep voor de netwerkbeveiligingsregel](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

De volgende voorbeelden maakt een groep van de netwerkbeveiligingsregel met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) met de naam *myNetworkSecurityGroupRule* naar *toestaan* verkeer op *tcp* poort *3389*.

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Verbinding maken met uw Linux-VM met een extern bureaublad-client
Open uw lokale extern-bureaubladclient en verbinding maken met de IP-adres of de DNS-naam van uw Linux-VM. Geef de gebruikersnaam en wachtwoord voor het gebruikersaccount op de virtuele machine als volgt:

![Verbinding maken met xrdp met behulp van extern bureaublad-client](./media/use-remote-desktop/remote-desktop-client.png)

Als de verificatie is gelukt, wordt de bureaubladomgeving xfce geladen en is vergelijkbaar met het volgende voorbeeld:

![xfce bureaubladomgeving via xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a>Problemen oplossen
Als u verbinding maken met uw Linux-VM met een extern bureaublad-client, `netstat` op uw Linux-VM om te controleren dat de virtuele machine voor RDP-verbindingen als volgt luistert:

```bash
sudo netstat -plnt | grep rdp
```

Het volgende voorbeeld ziet u de virtuele machine luisteren op TCP-poort 3389 zoals verwacht:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Als de xrdp-service niet wordt geluisterd, op een VM Ubuntu de service opnieuw starten als volgt:

```bash
sudo service xrdp restart
```

Bekijk wordt geregistreerd in */var/log*Thug op uw VM Ubuntu voor aanwijzingen over waarom de service niet reageert. U kunt ook de syslog bewaken tijdens een poging verbinding met extern bureaublad om eventuele fouten weer te geven:

```bash
tail -f /var/log/syslog
```

Andere zoals Red Hat Enterprise Linux en SUSE Linux-distributies mogelijk verschillende manieren opnieuw starten van services en locaties alternatieve logboekbestand om te controleren.

Als u geen antwoord niet in uw extern-bureaubladclient krijgt en alle gebeurtenissen in het systeemlogboek niet ziet, wordt de reden hiervoor is dat het externe bureaublad verkeer de virtuele machine kan niet bereiken. De netwerkbeveiligingsgroepen om ervoor te zorgen dat u hebt een regel om TCP op poort 3389 toe te controleren. Zie voor meer informatie [problemen met toepassing](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het maken en gebruiken van SSH-sleutels met Linux VM's [maken SSH-sleutels voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

Zie voor meer informatie over het gebruik van SSH op Windows [het gebruik van SSH-sleutels met Windows](ssh-from-windows.md).

