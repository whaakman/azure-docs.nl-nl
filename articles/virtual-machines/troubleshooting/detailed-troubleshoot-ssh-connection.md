---
title: Gedetailleerde SSH probleemoplossing voor een Azure-VM | Microsoft Docs
description: Meer gedetailleerde stappen voor problemen met het maken van een verbinding met een Azure-machine voor probleemoplossing SSH
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/13/2017
ms.author: genli
ms.openlocfilehash: 1173e4c9ec40e8b8fe9e24587258f7e7aad7c7f8
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269459"
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-to-a-linux-vm-in-azure"></a>Gedetailleerde SSH probleemoplossing voor problemen die verbinding maken met een Linux-VM in Azure
Er zijn veel mogelijke oorzaken waardoor de SSH-client mogelijk niet de SSH-service op de virtuele machine bereiken. Als u hebt gevolgd door de andere meer [algemene SSH stappen voor probleemoplossing](troubleshoot-ssh-connection.md), moet u het probleem verder worden opgelost. In dit artikel begeleidt u bij gedetailleerde stappen voor probleemoplossing om te bepalen waar de SSH-verbinding is mislukt en hoe u deze kunt oplossen.

## <a name="take-preliminary-steps"></a>Voorbereidende stappen uitvoeren
Het volgende diagram ziet u de onderdelen die betrokken zijn.

![Diagram met onderdelen van de SSH-service](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

De volgende stappen helpen u bij het isoleren van de bron van de fout en oplossingen of tijdelijke oplossingen te achterhalen.

1. Controleer de status van de virtuele machine in de portal.
   In de [Azure-portal](https://portal.azure.com), selecteer **virtuele machines** > *VM-naam*.

   Het deelvenster status voor de virtuele machine moet worden weergegeven **met**. Schuif omlaag naar de recente activiteit weergeven voor berekening, opslag en netwerkresources.

2. Selecteer **instellingen** om te controleren van eindpunten, IP-adressen, netwerkbeveiligingsgroepen en andere instellingen.

   De virtuele machine moet een eindpunt dat is gedefinieerd voor SSH-verkeer die u kunt bekijken in **eindpunten** of  **[netwerkbeveiligingsgroep](../../virtual-network/security-overview.md)**. Eindpunten in virtuele machines die zijn gemaakt met behulp van Resource Manager worden opgeslagen in een netwerkbeveiligingsgroep. Controleer of de regels aan de netwerkbeveiligingsgroep zijn toegepast en wordt verwezen in het subnet.

Als u wilt controleren of verbinding met het netwerk, Controleer de geconfigureerde eindpunten en zien als u verbinding met de virtuele machine via een ander protocol, zoals HTTP of een andere service maken kunt.

Probeer de SSH-verbinding opnieuw na deze stappen.

## <a name="find-the-source-of-the-issue"></a>De bron van het probleem vinden
De SSH-client op uw computer kan geen verbinding maken met de SSH-service op de Azure-VM vanwege problemen of configuratiefouten in de volgende gebieden:

* [SSH-clientcomputer](#source-1-ssh-client-computer)
* [Organisatie-edge-apparaat](#source-2-organization-edge-device)
* [Service-eindpunt in de cloud en toegangsbeheerlijsten (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Netwerkbeveiligingsgroepen](#source-4-network-security-groups)
* [Azure-VM op basis van Linux](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>1 bron: SSH client-computer
Om te voorkomen de computer als de bron van de fout, Controleer of dat het SSH-verbindingen naar een andere on-premises, op basis van Linux-computer kunt maken.

![Diagram waarin computeronderdelen van SSH-client](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Als de verbinding is mislukt, controleert de volgende problemen op uw computer:

* Een lokale firewall-instelling die wordt geblokkeerd door binnenkomende of uitgaande SSH-verkeer (TCP 22)
* Lokaal geïnstalleerde clientsoftware proxy dat voorkomt SSH-verbindingen dat
* Lokaal geïnstalleerde software die voorkomt SSH-verbindingen dat voor netwerkbeheer
* Andere typen beveiligingssoftware om verkeer te controleren of bepaalde typen verkeer toestaan/weigeren

Als een van deze voorwaarden van toepassing, tijdelijk uitschakelen van de software en probeert u een SSH-verbinding met een on-premises computer om te achterhalen waarom die de verbinding wordt geblokkeerd op uw computer. Werk vervolgens verder met de netwerkbeheerder om op te lossen van de software-instellingen voor SSH-verbindingen.

Als u verificatie via certificaat gebruikt, controleert u of deze machtigingen te hebben tot de SSH-map in de basismap:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (of andere bestanden met persoonlijke sleutels die zijn opgeslagen in deze)
* Chmod 644 ~/.ssh/known_hosts (bevat hosts die u via SSH verbinding hebt gemaakt)

## <a name="source-2-organization-edge-device"></a>Bron 2: Organisatie edge-apparaat
Om te voorkomen dat uw edge-apparaat organisatie als de bron van de fout, Controleer of dat een computer die rechtstreeks zijn verbonden met Internet SSH-verbindingen met uw Azure-VM maken kunt. Als u de virtuele machine via een site-naar-site-VPN of een Azure ExpressRoute-verbinding opent, gaat u naar [bron 4: Netwerkbeveiligingsgroepen](#nsg).

![Diagram waarin organisatie edge-apparaat](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Als u hebt een computer die rechtstreeks is verbonden met Internet, een nieuwe Azure-VM maken in een eigen resourcegroep of cloudservice en die nieuwe virtuele machine gebruiken. Zie voor meer informatie, [maken van een virtuele machine waarop Linux wordt uitgevoerd in Azure](../linux/quick-create-cli.md). Verwijder de resourcegroep of de virtuele machine en cloud-service wanneer u klaar bent met het testen.

Als u een SSH-verbinding met een computer die rechtstreeks verbonden met Internet maken kunt, controleert u uw organisatie edge-apparaat voor:

* Een interne firewall die SSH-verkeer met het Internet wordt geblokkeerd door
* Een proxyserver die voorkomt SSH-verbindingen dat
* Indringers detecteren of software die wordt uitgevoerd op apparaten in uw edge-netwerk dat voorkomt SSH-verbindingen dat voor netwerkbeheer

Werken met uw netwerkbeheerder om op te lossen van de instellingen van uw organisatie edge-apparaten zodat SSH-verkeer met het Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: De Cloud service-eindpunt en de ACL
> [!NOTE]
> Deze bron geldt alleen voor virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel. Voor virtuele machines die zijn gemaakt met behulp van Resource Manager, gaat u naar [4 van bron: Netwerkbeveiligingsgroepen](#nsg).

Om te voorkomen de cloud service-eindpunt en de ACL als de bron van de fout, Controleer of dat een andere Azure-virtuele machine in hetzelfde virtuele netwerk verbinding kunnen maken met behulp van SSH.

![Het diagram cloud service-eindpunt en ACL illustreert](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Als u geen een andere virtuele machine in hetzelfde virtuele netwerk hebt, kunt u een eenvoudig maken. Zie voor meer informatie, [maakt een Linux-VM in Azure met behulp van de CLI](../linux/quick-create-cli.md). De extra virtuele machine verwijderen wanneer u klaar bent met het testen.

Als u een SSH-verbinding met een virtuele machine in hetzelfde virtuele netwerk maken kunt, controleert u de volgende gebieden:

* **De endpoint-configuratie voor SSH-verkeer op de doel-VM.** De persoonlijke TCP-poort van het eindpunt moet overeenkomen met de TCP-poort waarop de SSH-service op de virtuele machine luistert. (De standaardpoort is 22). Controleer of de SSH TCP-poortnummer in Azure portal door te selecteren **virtuele machines** > *VM-naam* > **instellingen**  >   **Eindpunten**.
* **De ACL voor het eindpunt van de SSH-verkeer op de virtuele doelmachine.** Een ACL kunt u opgeven toegestaan of geweigerd inkomend verkeer van Internet, op basis van de bron-IP-adres. Onjuist geconfigureerde ACL's kunnen voorkomen dat binnenkomende SSH-verkeer naar het eindpunt. Controleer de ACL's om ervoor te zorgen dat inkomend verkeer van het openbare IP-adressen van uw proxy of andere edge-server is toegestaan. Zie voor meer informatie, [over toegang tot het netwerk toegangsbeheerlijsten (ACL's)](../../virtual-network/virtual-networks-acl.md).

Om te voorkomen het eindpunt als een bron van het probleem, het eindpunt van de huidige verwijderen, een ander eindpunt maken en de SSH-naam (TCP-poort 22 voor de openbare en particuliere poortnummer) opgeven. Zie voor meer informatie, [eindpunten instellen op een virtuele machine in Azure](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>Bron 4: Netwerkbeveiligingsgroepen
Netwerkbeveiligingsgroepen kunnen u meer gedetailleerde controle van toegestaan binnenkomend en uitgaand verkeer. U kunt regels maken die subnetten omvatten en cloudservices in een Azure-netwerk. Controleer uw regels voor netwerkbeveiligingsgroepen om ervoor te zorgen dat de SSH-verkeer naar en van Internet is toegestaan.
Zie voor meer informatie, [over netwerkbeveiligingsgroepen](../../virtual-network/security-overview.md).

U kunt ook een IP-controleren gebruiken om de NSG-configuratie te valideren. Zie voor meer informatie, [Azure-netwerk bewakingsoverzicht](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>: 5 op basis van Linux Azure virtuele bronmachine
De laatste bron van mogelijke problemen is de Azure virtuele machine zelf.

![Diagram waarin wordt uitgelegd op basis van Linux virtuele machine van Azure](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Als u dit nog niet hebt gedaan, volgt u de instructies [opnieuw instellen van een wachtwoord op basis van Linux virtuele machines](../linux/reset-password.md).

Probeer opnieuw verbinding te maken van uw computer. Als het nog steeds mislukt, volgen hier enkele van de mogelijke problemen:

* De SSH-service wordt niet uitgevoerd op de virtuele doelmachine.
* De SSH-service luistert niet op TCP-poort 22. Om te testen, een Telnet-client installeren op uw lokale computer en voer ' telnet *cloudServiceName*. cloudapp.net 22 '. Deze stap bepaalt als de virtuele machine kan binnenkomende en uitgaande communicatie met de SSH-eindpunt.
* De lokale firewall op de virtuele doelmachine heeft regels die verhinderen binnenkomende of uitgaande SSH-verkeer dat.
* Indringers detecteren of het netwerk bewaken die wordt uitgevoerd op virtuele machine van Azure voorkomt dat SSH-verbindingen.

## <a name="additional-resources"></a>Aanvullende resources
Zie voor meer informatie over het oplossen van toegang tot toepassingen [problemen oplossen met toegang tot een toepassing die wordt uitgevoerd op een virtuele machine van Azure](../linux/troubleshoot-app-connection.md)
