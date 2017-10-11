---
title: Problemen met SSH-verbinding naar een Azure-virtuele machine | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen zoals 'SSH-verbinding is mislukt' of 'SSH-verbinding geweigerd' voor een virtuele machine van Azure waarop Linux wordt uitgevoerd.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: 3a282c8b2c2ba2749de6a2d3688bd57d75703b22
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Problemen met SSH-verbindingen met een Azure Linux VM die is mislukt, fouten, of wordt geweigerd
Er zijn diverse redenen dat u op problemen Secure Shell (SSH), SSH verbindingsfouten stuit, of SSH wordt geweigerd wanneer u probeert verbinding maken met virtuele Linux-machine (VM). Dit artikel helpt u bij het vinden en los de problemen. U kunt de Azure-portal, Azure CLI of uitbreiding van de VM-toegang voor Linux kunt oplossen van problemen met de verbinding.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [MSDN Azure en Stack Overflow-forums](http://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](http://azure.microsoft.com/support/options/) en selecteer **ondersteuning krijgen**. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](http://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Probeer opnieuw verbinding te maken met de virtuele machine na elke stap.

1. De SSH-configuratie opnieuw instellen.
2. Stel de referenties voor de gebruiker opnieuw in.
3. Controleer of de [Netwerkbeveiligingsgroep](../../virtual-network/virtual-networks-nsg.md) regels SSH-verkeer toestaan.
   * Zorg ervoor dat een Netwerkbeveiligingsgroep-regel bestaat voor de SSH-verkeer toestaan (standaard TCP-poort 22).
   * U kunt geen poortomleiding gebruiken / toewijzen zonder gebruik van een Azure load balancer.
4. Controleer de [VM resourcestatus](../../resource-health/resource-health-overview.md). 
   * Zorg ervoor dat de virtuele machine als goed rapporteert.
   * Als er diagnostische gegevens over opstarten is ingeschakeld, controleert u of dat de virtuele machine is niet rapportage van fouten in de logboeken.
5. Start opnieuw op de virtuele machine.
6. Implementeer de virtuele machine opnieuw.

Blijven lezen voor meer gedetailleerde stappen voor probleemoplossing en uitleg.

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Beschikbare methoden voor het oplossen van problemen met SSH-verbinding
U kunt opnieuw instellen van referenties of SSH-configuratie met een van de volgende methoden:

* [Azure-portal](#use-the-azure-portal) - handig als u opnieuw wilt instellen snel de SSH-configuratie of de SSH-sleutel en u de Azure-hulpprogramma's geïnstalleerd hoeft.
* [Azure CLI 2.0](#use-the-azure-cli-20) : als u zich al op de opdrachtregel snel opnieuw instellen van de SSH-configuratie of de referenties. U kunt ook de [Azure CLI 1.0](#use-the-azure-cli-10)
* [Azure VMAccessForLinux-extensie](#use-the-vmaccess-extension) - maken en gebruiken als json-definitiebestanden om in te stellen van de SSH-configuratie of gebruikersreferenties.

Probeer opnieuw verbinding te maken met uw virtuele machine na elke stap voor probleemoplossing. Als u nog steeds geen verbinding maken, probeert u de volgende stap.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
De Azure portal biedt een snelle manier om in te stellen van de SSH-configuratie of de gebruiker referenties zonder alle hulpprogramma's te installeren op uw lokale computer.

Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag naar de **ondersteuning + probleemoplossing** sectie en selecteer **wachtwoord opnieuw instellen** zoals in het volgende voorbeeld:

![SSH-configuratie of de referenties in de Azure-portal opnieuw instellen](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>De SSH-configuratie opnieuw instellen
Selecteer als eerste stap `Reset configuration only` van de **modus** Vervolgkeuzelijst als in de vorige schermafbeelding klik vervolgens op de **opnieuw instellen** knop. Zodra deze actie is voltooid, probeert u opnieuw toegang krijgen tot uw virtuele machine.

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als u de referenties van een bestaande gebruiker herstellen, selecteert u `Reset SSH public key` of `Reset password` van de **modus** vervolgkeuzelijst zoals in de vorige schermafbeelding. Geef de gebruikersnaam en een SSH-sleutel of het nieuwe wachtwoord en klik op de **opnieuw** knop.

U kunt ook een gebruiker met sudo-machtigingen op de virtuele machine vanaf dit menu maken. Voer een nieuwe gebruikersnaam en het bijbehorende wachtwoord of de SSH-sleutel en klik vervolgens op de **opnieuw** knop.

## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken
Als u nog niet gedaan hebt, installeert u de nieuwste [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/#login).

Als u hebt gemaakt en de installatiekopie van een aangepaste Linux-schijf geüpload, controleert u of de [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) versie 2.0.5 of hoger is geïnstalleerd. VM's zijn gemaakt met behulp van afbeeldingen, is deze uitbreiding voor toegang tot al geïnstalleerd en geconfigureerd voor u.

### <a name="reset-ssh-configuration"></a>SSH-configuratie opnieuw instellen
U kunt in eerste instantie voor het instellen van de SSH-configuratie op de standaardwaarden en de SSH-server op de virtuele machine opnieuw opstarten. Houd er rekening mee dat dit niet de gebruikersnaam, wachtwoord of SSH-sleutels wijzigt.
Het volgende voorbeeld wordt [az vm gebruiker opnieuw instellen-ssh](/cli/azure/vm/user#reset-ssh) opnieuw instellen van de SSH-configuratie op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Het volgende voorbeeld wordt [az vm gebruikersupdate](/cli/azure/vm/user#update) opnieuw instellen van de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Als u verificatie van SSH-sleutel gebruikt, kunt u de SSH-sleutel voor een bepaalde gebruiker herstellen. Het volgende voorbeeld wordt **az vm set-linux-gebruikers toegang tot** bijwerken van de SSH-sleutel die is opgeslagen in `~/.ssh/id_rsa.pub` voor de gebruiker met de naam `myUsername`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a>Gebruik de VMAccess-extensie
De VM-extensie voor toegang voor Linux wordt gelezen in een json-bestand dat acties definieert uit te voeren. Deze acties omvatten opnieuw instellen van SSHD, opnieuw instellen van een SSH-sleutel of een gebruiker toe te voegen. U nog steeds de Azure CLI gebruiken om aan te roepen de VMAccess-extensie, maar u kunt de json-bestanden hergebruiken over meerdere VM's, indien gewenst. Deze aanpak kunt u een opslagplaats voor json-bestanden die vervolgens kan worden aangeroepen voor bepaalde scenario's maken.

### <a name="reset-sshd"></a>SSHD opnieuw instellen
Maak een bestand met de naam `settings.json` met de volgende inhoud:

```json
{  
    "reset_ssh":"True"
}
```

De Azure CLI, u vervolgens aanroepen de `VMAccessForLinux` uitbreiding opnieuw instellen van uw SSHD-verbinding door te geven van uw json-bestand. Het volgende voorbeeld wordt [az vm-extensie set](/cli/azure/vm/extension#set) opnieuw in te stellen SSHD op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als SSHD lijkt te laten functioneren, kunt u de referenties voor een afzender gebruiker opnieuw instellen. Als u het wachtwoord voor een gebruiker herstellen, maakt u een bestand met de naam `settings.json`. Het volgende voorbeeld worden de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`. Voer de volgende regels in uw `settings.json` -bestand, met behulp van uw eigen waarden:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Of als u de SSH-sleutel voor een gebruiker herstellen, moet u eerst een bestand met de naam maken `settings.json`. Het volgende voorbeeld worden de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Voer de volgende regels in uw `settings.json` -bestand, met behulp van uw eigen waarden:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Nadat uw json-bestand is gemaakt, kunt u de Azure CLI gebruiken om aan te roepen de `VMAccessForLinux` uitbreiding opnieuw instellen van uw SSH-gebruikersreferenties door te geven van uw json-bestand. Het volgende voorbeeld worden de referenties op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-cli-10"></a>Gebruik de Azure CLI 1.0
Als u dat nog niet gedaan hebt, [installeren van de Azure CLI 1.0 en maak verbinding met uw Azure-abonnement](../../cli-install-nodejs.md). Zorg ervoor dat u van Resource Manager-modus als volgt gebruikmaakt:

```azurecli
azure config mode arm
```

Als u hebt gemaakt en de installatiekopie van een aangepaste Linux-schijf geüpload, controleert u of de [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) versie 2.0.5 of hoger is geïnstalleerd. VM's zijn gemaakt met behulp van afbeeldingen, is deze uitbreiding voor toegang tot al geïnstalleerd en geconfigureerd voor u.

### <a name="reset-ssh-configuration"></a>SSH-configuratie opnieuw instellen
De configuratie van de SSHD is mogelijk onjuist geconfigureerd of de service is een fout opgetreden. U kunt herstellen SSHD om ervoor te zorgen dat de SSH-configuratie zelf is geldig. Opnieuw instellen van SSHD, moet de eerste probleemoplossing die u rekening houden.

Het volgende voorbeeld wordt SSHD op een virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Uw eigen namen voor virtuele machine en de resource als volgt gebruiken:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als SSHD lijkt te laten functioneren, kunt u het wachtwoord voor een afzender gebruiker opnieuw instellen. Het volgende voorbeeld worden de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

Als u verificatie van SSH-sleutel gebruikt, kunt u de SSH-sleutel voor een bepaalde gebruiker herstellen. De SSH-sleutel die is opgeslagen in het volgende voorbeeld-updates `~/.ssh/id_rsa.pub` voor de gebruiker met de naam `myUsername`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Een VM opnieuw opstarten
Als u opnieuw instellen van de SSH-configuratie en de gebruikersreferenties, of is een fout opgetreden in dat geval, kunt u proberen opnieuw starten van de virtuele machine naar adres onderliggende compute-problemen.

### <a name="azure-portal"></a>Azure Portal
Als u wilt starten op een virtuele machine met de Azure portal, selecteer uw virtuele machine en klik op de **opnieuw** knop zoals in het volgende voorbeeld:

![Opnieuw opstarten van een virtuele machine in de Azure-portal](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
Het volgende voorbeeld start de virtuele machine met de naam opnieuw `myVM` in de resourcegroep met de naam `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Het volgende voorbeeld wordt [az vm opnieuw](/cli/azure/vm#restart) opnieuw opstarten van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Een virtuele machine opnieuw implementeren
U kunt een virtuele machine naar een ander knooppunt in Azure, die mogelijk onderliggende netwerkproblemen opgelost opnieuw implementeren. Zie voor meer informatie over het opnieuw distribueren van een virtuele machine [opnieuw implementeren van virtuele machine naar een nieuw Azure knooppunt](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Nadat deze is voltooid, kortstondige schijfgegevens niet verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn wordt bijgewerkt.
> 
> 

### <a name="azure-portal"></a>Azure Portal
Als u wilt implementeren op een virtuele machine met de Azure portal, selecteert u de virtuele machine en schuif omlaag naar de **ondersteuning + probleemoplossing** sectie. Klik op de **implementeren** knop zoals in het volgende voorbeeld:

![Implementeren van een virtuele machine in de Azure portal](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
Het volgende voorbeeld de virtuele machine met de naam redeploys `myVM` in de resourcegroep met de naam `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Het volgende Voorbeeldgebruik [az vm opnieuw distribueren](/cli/azure/vm#redeploy) te implementeren van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel
Probeer deze stappen voor het oplossen van de meest voorkomende fouten van de SSH-verbinding voor VM's die zijn gemaakt met behulp van het klassieke implementatiemodel. Probeer opnieuw verbinding te maken met de virtuele machine na elke stap.

* Opnieuw instellen van externe toegang van de [Azure-portal](https://portal.azure.com). De Azure-portal, selecteer uw virtuele machine en klik op de **extern opnieuw instellen...**  knop.
* Start opnieuw op de virtuele machine. Op de [Azure-portal](https://portal.azure.com), selecteert u de virtuele machine en klik op de **opnieuw** knop.
    
* Implementeer de virtuele machine naar een nieuw Azure knooppunt opnieuw. Zie voor meer informatie over het implementeren van een virtuele machine [opnieuw implementeren van virtuele machine naar een nieuw Azure knooppunt](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    Nadat deze is voltooid, kortstondige schijfgegevens niet verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn wordt bijgewerkt.
* Volg de instructies in [opnieuw instellen van een wachtwoord of SSH voor virtuele machines op basis van Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) naar:
  
  * Het wachtwoord of SSH-sleutel opnieuw instellen.
  * Maak een *sudo* gebruikersaccount.
  * De SSH-configuratie opnieuw instellen.
* Controleer de status van de VM-resource voor problemen met het platform.<br>
     Selecteer uw virtuele machine en schuif naar beneden **instellingen** > **Controleer Health**.

## <a name="additional-resources"></a>Aanvullende bronnen
* Als u nog steeds niet SSH met uw virtuele machine na de na stappen te volgen, Zie [meer gedetailleerde stappen voor probleemoplossing](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) om te controleren van extra stappen uitvoeren om het probleem kunt oplossen.
* Zie voor meer informatie over het oplossen van toegang tot toepassingen [toegang tot een toepassing die wordt uitgevoerd op een virtuele machine van Azure oplossen](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Zie voor meer informatie over het oplossen van virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel [opnieuw instellen van een wachtwoord of SSH voor virtuele machines op basis van Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

