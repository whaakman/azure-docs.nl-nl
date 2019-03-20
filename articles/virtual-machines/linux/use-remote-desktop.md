---
title: Met een virtuele Linux-machine via Extern bureaublad gebruiken in Azure | Microsoft Docs
description: Meer informatie over het installeren en configureren van extern bureaublad (xrdp) verbinding maken met een Linux-VM in Azure met behulp van de grafische hulpprogramma 's
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: dec0b8bcd5a0e0d20ead5b149405e3716589ba53
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57995726"
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a>Installeren en configureren van extern bureaublad verbinding maken met een Linux-VM in Azure
Linux virtuele machines (VM's) in Azure worden meestal beheerd vanaf de opdrachtregel met behulp van een secure shell (SSH)-verbinding. Wanneer er nieuwe voor Linux, of om snel oplossen van problemen met scenario's, kan het gebruik van extern bureaublad eenvoudiger zijn. Dit artikel wordt uitgelegd hoe u kunt installeren en configureren van een bureaublad-omgeving ([xfce](https://www.xfce.org)) en extern bureaublad ([xrdp](http://www.xrdp.org)) voor uw Linux-VM met het Resource Manager-implementatiemodel.


## <a name="prerequisites"></a>Vereisten
In dit artikel gebruikmaken van een bestaande Ubuntu 16.04 LTS-VM in Azure. Als u een virtuele machine maken wilt, gebruikt u een van de volgende methoden:

- De [Azure CLI](quick-create-cli.md)
- [Azure Portal](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Een desktopomgeving installeren op uw Linux-VM
De meeste Linux-VM's in Azure beschikt niet over een desktopomgeving standaard geïnstalleerd. Linux-VM's worden meestal beheerd met behulp van SSH-verbindingen in plaats van een bureaublad-omgeving. Er zijn verschillende desktopomgevingen in Linux die u kunt kiezen. Afhankelijk van uw keuze van desktopomgeving, kan het één tot en met 2 GB aan schijfruimte in beslag nemen en 5 tot 10 minuten installeren en configureren van de vereiste pakketten.

Het volgende voorbeeld installeert de lightweight [xfce4](https://www.xfce.org/) desktopomgeving op een Ubuntu 16.04 LTS-VM. Opdrachten voor andere distributies enigszins verschillen (Gebruik `yum` op Red Hat Enterprise Linux installeren en configureren juiste `selinux` regels, of gebruik `zypper` te installeren op SUSE, bijvoorbeeld).

Eerste, SSH verbinding met uw virtuele machine. Het volgende voorbeeld maakt verbinding met de virtuele machine met de naam *myvm.westus.cloudapp.azure.com* met de gebruikersnaam van *azureuser*. Gebruik uw eigen waarden:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u van Windows gebruikmaakt en meer informatie over het gebruik van SSH nodig hebt, raadpleegt u [over het gebruik van SSH-sleutels met Windows](ssh-from-windows.md).

Vervolgens installeert u met behulp van xfce `apt` als volgt:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Installeren en configureren van een extern bureaublad-server
Nu dat u een desktop-omgeving is geïnstalleerd hebt, configureert u een extern bureaublad-services om te luisteren naar binnenkomende verbindingen. [xrdp](http://xrdp.org) is een open-source Remote Desktop Protocol (RDP)-server die is beschikbaar in de meeste Linux-distributies en werkt goed met xfce. Installeer xrdp op uw Ubuntu-VM als volgt:

```bash
sudo apt-get install xrdp
sudo systemctl enable xrdp
```

Vertel xrdp welke desktopomgeving moet worden gebruikt wanneer u uw sessie starten. Xrdp voor het gebruik van xfce als uw desktopomgeving als volgt configureren:

```bash
echo xfce4-session >~/.xsession
```

Start de service xrdp voor de wijzigingen worden doorgevoerd als volgt:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Stel het wachtwoord van een lokale gebruikersaccount
Als u een wachtwoord voor uw gebruikersaccount gemaakt tijdens het maken van uw virtuele machine, moet u deze stap overslaan. Als u alleen gebruik van verificatie van SSH-sleutel en het wachtwoord van een lokaal account instellen, een wachtwoord opgeven voordat u xrdp gebruiken om aan te melden bij uw virtuele machine niet hebt. xrdp kan geen SSH-sleutels voor verificatie accepteren. Het volgende voorbeeld geeft u een wachtwoord voor het gebruikersaccount *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Een wachtwoord op te geven, wordt de configuratie van uw SSHD zodanig aanmelding met een wachtwoord als die op dit moment niet bestaat niet bijgewerkt. Vanuit het oogpunt van veiligheid kunt u wilt verbinden met uw virtuele machine met een SSH-tunnel met behulp van verificatie op basis van een sleutel en maak verbinding met xrdp. Als dit het geval is, slaat u de volgende stap voor het maken van een regel voor de netwerkbeveiligingsgroep voor het toestaan van verkeer van extern bureaublad.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Maak een regel voor Netwerkbeveiligingsgroep voor verkeer van extern bureaublad
Voor extern bureaublad-verkeer tot uw Linux-VM, een netwerkbeveiligingsgroep kunt groep regel moet worden gemaakt die TCP op poort 3389 uw virtuele machine kan bereiken. Zie voor meer informatie over de regels voor netwerkbeveiligingsgroepen [wat is er een netwerkbeveiligingsgroep?](../../virtual-network/security-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) U kunt ook [de Azure portal gebruiken voor het maken van een regel voor netwerkbeveiligingsgroep](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Het volgende voorbeeld wordt een netwerkbeveiligingsgroepregel met [az vm open-port](/cli/azure/vm#az-vm-open-port) op poort *3389*. Vanuit de Azure CLI niet de SSH-sessie met uw virtuele machine, opent u de volgende regel voor netwerkbeveiligingsgroep:

```azurecli
az vm open-port --resource-group myResourceGroup --name myVM --port 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Verbind uw Linux-VM met een extern bureaublad-client
Open uw lokale extern-bureaubladclient en verbinding maken met het IP-adres of de DNS-naam van uw Linux-VM. Voer de gebruikersnaam en wachtwoord voor het gebruikersaccount op de virtuele machine als volgt:

![Verbinding maken met xrdp met behulp van de extern bureaublad-client](./media/use-remote-desktop/remote-desktop-client.png)

Na verificatie, wordt de desktopomgeving xfce laden en het volgende voorbeeld als volgt uitzien:

![xfce desktopomgeving via xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)

Als uw lokale RDP-client gebruikmaakt van verificatie op netwerkniveau (NLA), moet u mogelijk om uit te schakelen die instelling. XRDP biedt momenteel geen ondersteuning voor NLA. U kunt ook zoeken op alternatieve RDP-oplossingen die ondersteuning voor NLA, zoals bieden [FreeRDP](https://www.freerdp.com).


## <a name="troubleshoot"></a>Problemen oplossen
Als u geen verbinding met uw Linux-VM met behulp van een extern bureaublad-client maken, gebruikt u `netstat` op uw Linux-VM om te controleren of dat uw virtuele machine voor RDP-verbindingen als volgt luistert:

```bash
sudo netstat -plnt | grep rdp
```

Het volgende voorbeeld ziet u de virtuele machine die luistert op TCP-poort 3389 zoals verwacht:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Als de *xrdp sesman* service niet wordt geluisterd, op een Ubuntu-VM Start de service als volgt:

```bash
sudo service xrdp restart
```

Beoordeling aanmeldt */var/log* op uw Ubuntu-VM aanwijzingen over waarom de service reageert niet. U kunt ook de syslog controleren tijdens een poging verbinding met extern bureaublad om eventuele fouten weer te geven:

```bash
tail -f /var/log/syslog
```

Andere Linux-distributies zoals Red Hat Enterprise Linux en SUSE mogelijk verschillende manieren voor het opnieuw opstarten van services en de locatie van alternatieve logboekbestanden om te controleren.

Als u geen een reactie in uw extern bureaublad-client ontvangt en alle gebeurtenissen in het systeemlogboek niet ziet, wordt dit gedrag geeft aan dat de virtuele machine niet verkeer van extern bureaublad bereiken kan. Controleer uw regels voor netwerkbeveiligingsgroepen om ervoor te zorgen dat u een regel hebt om TCP op poort 3389. Zie voor meer informatie, [oplossen van problemen met de netwerkverbinding van de toepassing](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het maken en gebruiken van SSH-sleutels met Linux-VM's, [SSH-sleutels maken voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

Zie voor meer informatie over het gebruik van SSH van Windows [over het gebruik van SSH-sleutels met Windows](ssh-from-windows.md).

