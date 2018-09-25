---
title: SSH-verbindingsproblemen met een Azure-VM oplossen | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen, zoals 'SSH-verbinding is mislukt' of 'SSH-verbinding is geweigerd' voor een virtuele Azure-machine waarop Linux wordt uitgevoerd.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: b170a675482f69f8188894f8503703c235497503
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982610"
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Problemen met SSH-verbindingen met een Azure Linux VM die is mislukt, fouten, of wordt geweigerd oplossen
Er zijn verschillende redenen dat er Secure Shell (SSH)-fouten, SSH-verbindingsfouten optreden of SSH wordt geweigerd wanneer u probeert verbinding maken met een Linux virtuele machine (VM). Dit artikel helpt u bij het vinden en los de problemen. U kunt Azure portal, Azure CLI of VM-extensie voor toegang voor Linux gebruiken om problemen op te lossen problemen met de verbinding.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](http://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](http://azure.microsoft.com/support/options/) en selecteer **ondersteuning krijgen**. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](http://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Probeer opnieuw verbinding maken met de virtuele machine na elke stap.

1. De SSH-configuratie opnieuw instellen.
2. De referenties voor de gebruiker opnieuw instellen.
3. Controleer of de [netwerkbeveiligingsgroep](../../virtual-network/security-overview.md) SSH-verkeer is toegestaan.
   * Zorg ervoor dat een regel voor Netwerkbeveiligingsgroep bestaat SSH-verkeer toestaan (standaard TCP-poort 22).
   * U kunt geen gebruiken poortomleiding / zonder gebruik van een Azure load balancer toewijzen.
4. Controleer de [VM resourcestatus](../../resource-health/resource-health-overview.md). 
   * Zorg ervoor dat de virtuele machine als in orde wordt gerapporteerd.
   * Als u diagnostische gegevens over opstarten ingeschakeld hebt, controleert u of de virtuele machine is niet die opstartfouten rapporteren, in de logboeken.
5. Start de VM opnieuw.
6. De virtuele machine opnieuw implementeren.

Doorgaan met lezen voor meer gedetailleerde stappen voor probleemoplossing en uitleg.

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Beschikbare methoden voor het oplossen van problemen met SSH-verbinding
U kunt opnieuw instellen van referenties of SSH-configuratie met behulp van een van de volgende methoden:

* [Azure-portal](#use-the-azure-portal) - handig is als u wilt snel opnieuw instellen van de SSH-configuratie of de SSH-sleutel en u de Azure-hulpprogramma's geïnstalleerd hoeft.
* [Azure CLI](#use-the-azure-cli-20) : als u zich al op de opdrachtregel snel opnieuw instellen van de SSH-configuratie of de referenties. U kunt ook de [Azure klassieke CLI](#use-the-azure-cli-10)
* [Azure VMAccessForLinux-extensie](#use-the-vmaccess-extension) - maken en json-definitie-bestanden als u de referenties van de SSH-configuratie of de gebruiker opnieuw wilt gebruiken.

Probeer opnieuw verbinding te maken met uw virtuele machine na elke stap. Als u nog steeds geen verbinding maken, probeert u de volgende stap.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
De Azure portal biedt een snelle manier om het opnieuw instellen van de referenties van de SSH-configuratie of de gebruiker zonder alle hulpprogramma's installeren op uw lokale computer.

Selecteer de virtuele machine in Azure portal. Schuif omlaag naar de **ondersteuning + probleemoplossing** sectie en selecteer **wachtwoord opnieuw instellen** zoals in het volgende voorbeeld:

![SSH-configuratie of de referenties in de Azure-portal opnieuw instellen](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>De SSH-configuratie opnieuw instellen
Selecteer als eerste stap `Reset configuration only` van de **modus** Vervolgkeuzelijst zoals in de vorige schermafbeelding, en klik op de **opnieuw** knop. Zodra deze actie is voltooid, probeert u opnieuw toegang krijgen tot uw virtuele machine.

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als u wilt de referenties van een bestaande gebruiker opnieuw instellen, selecteert u `Reset SSH public key` of `Reset password` uit de **modus** vervolgkeuzelijst zoals in de vorige schermafbeelding. Geef de gebruikersnaam en een SSH-sleutel of het nieuwe wachtwoord en klik op de **opnieuw** knop.

U kunt ook een gebruiker met sudo-machtigingen op de virtuele machine in dit menu maken. Voer een nieuwe gebruikersnaam en het bijbehorende wachtwoord of de SSH-sleutel en klik vervolgens op de **opnieuw** knop.

### <a name="check-security-rules"></a>Beveiligingsregels controleren

Gebruik [IP-stroom controleren](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) om te bevestigen als verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een netwerkbeveiligingsgroep. U kunt ook bekijken beveiligingsgroepsregels om ervoor te zorgen inkomende 'toestaan' NSG-regel bestaat en met prioriteit wordt toegepast voor de SSH-poort (standaard 22). Zie voor meer informatie, [effectieve beveiligingsregels gebruiken om op te lossen VM verkeersstroom](../../virtual-network/diagnose-network-traffic-filter-problem.md).

### <a name="check-routing"></a>Controleer de routering

Gebruik van Network Watcher van [van volgende hop](../../network-watcher/network-watcher-check-next-hop-portal.md) mogelijkheid om te bevestigen dat een route is niet zo wordt voorkomen verkeer dat worden gerouteerd naar of van een virtuele machine. U kunt ook effectieve routes om te zien van alle effectieve routes voor een netwerkinterface bekijken. Zie voor meer informatie, [effectieve routes gebruiken om op te lossen VM verkeersstroom](../../virtual-network/diagnose-network-routing-problem.md).

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken
Als u niet hebt gedaan, installeert u de meest recente [Azure CLI](/cli/azure/install-az-cli2) en aan te melden bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

Als u hebt gemaakt en een aangepaste installatiekopie van het Linux-schijf geüpload, controleert u of de [Microsoft Azure Linux Agent](../extensions/agent-windows.md) versie 2.0.5 of hoger is geïnstalleerd. Deze extensie voor toegang is al voor virtuele machines die zijn gemaakt met behulp van de galerie met installatiekopieën, geïnstalleerd en geconfigureerd voor u.

### <a name="reset-ssh-configuration"></a>SSH-configuratie opnieuw instellen
U kunt in eerste instantie probeer het opnieuw instellen van de SSH-configuratie op de standaardwaarden en de SSH-server op de virtuele machine opnieuw wordt opgestart. Houd er rekening mee dat dit niet de gebruikersnaam, wachtwoord of SSH-sleutels verandert.
Het volgende voorbeeld wordt [az vm gebruiker opnieuw instellen-ssh](/cli/azure/vm/user#az_vm_user_reset_ssh) opnieuw instellen van de SSH-configuratie op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Het volgende voorbeeld wordt [az vm gebruikersupdate](/cli/azure/vm/user#az_vm_user_update) opnieuw instellen van de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Als u verificatie via SSH-sleutel gebruikt, kunt u de SSH-sleutel voor een bepaalde gebruiker opnieuw instellen. Het volgende voorbeeld wordt **az vm toegang instellen-linux-gebruiker** om bij te werken van de SSH-sleutel die zijn opgeslagen in `~/.ssh/id_rsa.pub` voor de gebruiker met de naam `myUsername`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a>Gebruik de VMAccess-extensie
De toegang tot VM-extensie voor Linux wordt gelezen in een json-bestand dat acties definieert uit te voeren. Deze acties omvatten SSHD opnieuw in te stellen, opnieuw instellen van een SSH-sleutel of een gebruiker toe te voegen. U nog steeds de Azure CLI gebruiken om aan te roepen de VMAccess-extensie, maar u kunt de json-bestanden op meerdere virtuele machines gebruiken indien gewenst. Deze aanpak kunt u een opslagplaats voor json-bestanden die vervolgens kan worden aangeroepen voor bepaalde scenario's maken.

### <a name="reset-sshd"></a>Opnieuw instellen van SSHD
Maak een bestand met de naam `settings.json` met de volgende inhoud:

```json
{  
    "reset_ssh":"True"
}
```

Met de Azure CLI kunt u Roep vervolgens de `VMAccessForLinux` -extensie voor het opnieuw instellen van uw verbinding SSHD door uw json-bestand op te geven. Het volgende voorbeeld wordt [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set) opnieuw in te stellen SSHD op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als SSHD lijkt te laten functioneren, kunt u de referenties voor een gebruiker afzender opnieuw instellen. Als u wilt het wachtwoord voor een gebruiker opnieuw instellen, maakt u een bestand met de naam `settings.json`. Het volgende voorbeeld wordt de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`. Voer de volgende regels in uw `settings.json` -bestand, met behulp van uw eigen waarden:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Of als u wilt herstellen van de SSH-sleutel voor een gebruiker, moet u eerst een bestand met de naam maken `settings.json`. Het volgende voorbeeld wordt de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Voer de volgende regels in uw `settings.json` -bestand, met behulp van uw eigen waarden:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Nadat uw json-bestand is gemaakt, kunt u de Azure CLI gebruiken om aan te roepen de `VMAccessForLinux` -extensie voor het opnieuw instellen van de referenties van uw SSH-gebruiker door uw json-bestand op te geven. Het volgende voorbeeld wordt de referenties op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-classic-cli"></a>Gebruik de klassieke Azure-CLI
Als u dat nog niet gedaan hebt, [de klassieke Azure-CLI installeren en verbinding maken met uw Azure-abonnement](../../cli-install-nodejs.md). Zorg ervoor dat u van Resource Manager-modus als volgt gebruikmaakt:

```azurecli
azure config mode arm
```

Als u hebt gemaakt en een aangepaste installatiekopie van het Linux-schijf geüpload, controleert u of de [Microsoft Azure Linux Agent](../extensions/agent-windows.md) versie 2.0.5 of hoger is geïnstalleerd. Deze extensie voor toegang is al voor virtuele machines die zijn gemaakt met behulp van de galerie met installatiekopieën, geïnstalleerd en geconfigureerd voor u.

### <a name="reset-ssh-configuration"></a>SSH-configuratie opnieuw instellen
De configuratie van de SSHD is mogelijk onjuist geconfigureerd of de service is een fout opgetreden. U kunt opnieuw SSHD om te controleren of dat de SSH-configuratie zelf is geldig. Opnieuw instellen van SSHD, moet de eerste probleemoplossing die u ondernemen.

Het volgende voorbeeld wordt SSHD op een virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Gebruik uw eigen namen voor virtuele machine en de resource als volgt:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als SSHD lijkt te laten functioneren, kunt u het wachtwoord voor de gebruiker van een afzender opnieuw instellen. Het volgende voorbeeld wordt de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

Als u verificatie via SSH-sleutel gebruikt, kunt u de SSH-sleutel voor een bepaalde gebruiker opnieuw instellen. Het volgende voorbeeld wordt de SSH-sleutel die zijn opgeslagen in bijgewerkt `~/.ssh/id_rsa.pub` voor de gebruiker met de naam `myUsername`, op de virtuele machine met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Een VM opnieuw opstarten
Als u de SSH-configuratie en de gebruiker referenties opnieuw instellen, of is een fout opgetreden in dat geval, kunt u proberen opnieuw op de virtuele machine naar onderliggende problemen van de compute-adres te starten.

### <a name="azure-portal"></a>Azure Portal
Als u wilt starten op een virtuele machine met behulp van de Azure portal, selecteer de virtuele machine en klik op de **opnieuw** knop zoals in het volgende voorbeeld:

![Start opnieuw op een virtuele machine in Azure portal](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-classic-cli"></a>CLI van Azure classic
Het volgende voorbeeld wordt opnieuw opgestart van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli"></a>Azure-CLI
Het volgende voorbeeld wordt [az vm restart](/cli/azure/vm#az_vm_restart) opnieuw opstarten van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Een virtuele machine opnieuw implementeren
U kunt een virtuele machine naar een ander knooppunt binnen Azure, die mogelijk onderliggende netwerkproblemen opgelost opnieuw implementeren. Zie voor informatie over een virtuele machine opnieuw te implementeren, [opnieuw implementeren van virtuele machine naar de nieuwe Azure-knooppunt](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Nadat deze is voltooid, kortstondige schijfgegevens gaan verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn wordt bijgewerkt.
> 
> 

### <a name="azure-portal"></a>Azure Portal
Als u wilt implementeren op een virtuele machine met behulp van de Azure portal, selecteer de virtuele machine en schuif omlaag naar de **ondersteuning + probleemoplossing** sectie. Klik op de **opnieuw implementeren** knop zoals in het volgende voorbeeld:

![Een virtuele machine in Azure portal opnieuw implementeren](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-classic-cli"></a>CLI van Azure classic
Het volgende voorbeeld implementeert de virtuele machine met de naam opnieuw `myVM` in de resourcegroep met de naam `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli"></a>Azure-CLI
Het volgende voorbeeld van het gebruik [az vm opnieuw implementeren](/cli/azure/vm#az_vm_redeploy) opnieuw implementeren van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel
Probeer de volgende stappen om op te lossen van de meest voorkomende fouten van de SSH-verbinding voor VM's die zijn gemaakt met behulp van het klassieke implementatiemodel. Probeer opnieuw verbinding maken met de virtuele machine na elke stap.

* Opnieuw instellen van externe toegang via de [Azure-portal](https://portal.azure.com). De Azure-portal, selecteer de virtuele machine en klik op de **extern opnieuw instellen...**  knop.
* Start de VM opnieuw. Op de [Azure-portal](https://portal.azure.com), selecteer de virtuele machine en klik op de **opnieuw** knop.
    
* Implementeer de virtuele machine naar een nieuw Azure-knooppunt opnieuw. Zie voor meer informatie over hoe u een virtuele machine opnieuw implementeren [opnieuw implementeren van virtuele machine naar de nieuwe Azure-knooppunt](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    Nadat deze is voltooid, kortstondige schijfgegevens gaan verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn wordt bijgewerkt.
* Volg de instructies in [opnieuw instellen van een wachtwoord of SSH voor virtuele machines op basis van Linux](classic/reset-access-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) aan:
  
  * Het wachtwoord of SSH-sleutel opnieuw instellen.
  * Maak een *sudo* gebruikersaccount.
  * De SSH-configuratie opnieuw instellen.
* Controleer de resourcestatus van de virtuele machine of er problemen zijn platform.<br>
     Selecteer de virtuele machine en schuif naar beneden **instellingen** > **Controleer Health**.

## <a name="additional-resources"></a>Aanvullende resources
* Als u zich nog steeds niet SSH met uw virtuele machine na de volgende stappen na, Zie [meer gedetailleerde stappen voor probleemoplossing](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) om te controleren van aanvullende stappen om uw probleem te verhelpen.
* Zie voor meer informatie over het oplossen van toegang tot toepassingen [problemen oplossen met toegang tot een toepassing die wordt uitgevoerd op een virtuele machine van Azure](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Zie voor meer informatie over het oplossen van virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel [opnieuw instellen van een wachtwoord of SSH voor virtuele machines op basis van Linux](classic/reset-access-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

