---
title: Gedetailleerde SSH voor probleemoplossing voor een virtuele machine in Azure | Microsoft Docs
description: Gedetailleerde probleemoplossing voor problemen die verbinding maken met een virtuele machine van Azure SSH
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: support-article
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 9ccdb3fbca21264065eeb1c4e46314c62af4c2e8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-to-a-linux-vm-in-azure"></a>Gedetailleerde SSH probleemoplossing voor problemen die verbinding maken met een Linux VM in Azure
Er zijn veel mogelijke oorzaken waardoor de SSH-client mogelijk niet bereiken van de SSH-service op de virtuele machine. Als u hebt gevolgd door de meer [algemene SSH stappen voor probleemoplossing](troubleshoot-ssh-connection.md), moet u het probleem verder op te lossen. In dit artikel begeleidt u bij de gedetailleerde stappen voor probleemoplossing om te bepalen waar de SSH-verbinding is mislukt en het probleem te verhelpen.

## <a name="take-preliminary-steps"></a>Voorbereidende stappen uitvoeren
Het volgende diagram toont de onderdelen die betrokken zijn.

![Diagram waarin de onderdelen van SSH-service](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

De volgende stappen helpen u de bron van de fout te isoleren en oplossingen of tijdelijke oplossingen te achterhalen.

1. Controleer de status van de virtuele machine in de portal.
   In de [Azure-portal](https://portal.azure.com), selecteer **virtuele machines** > *VM-naam*.

   Het deelvenster status voor de virtuele machine moet worden weergegeven **met**. Schuif naar beneden recente activiteit weergeven voor de berekenings-, opslag- en netwerkbronnen.

2. Selecteer **instellingen** onderzoeken eindpunten, IP-adressen, beveiligingsgroepen en andere instellingen.

   De virtuele machine moet een eindpunt dat is gedefinieerd voor de SSH-verkeer dat u kunt bekijken in **eindpunten** of  **[netwerkbeveiligingsgroep](../../virtual-network/virtual-networks-nsg.md)**. Eindpunten in virtuele machines die zijn gemaakt met Resource Manager worden opgeslagen in een netwerkbeveiligingsgroep. Controleer ook of de regels zijn toegepast op de netwerkbeveiligingsgroep en er naar wordt verwezen in het subnet.

Om te controleren of verbinding met het netwerk, de geconfigureerde eindpunten te zien als u de virtuele machine via een ander protocol, zoals HTTP- of een andere service kan bereiken.

Probeer opnieuw de SSH-verbinding na deze stappen.

## <a name="find-the-source-of-the-issue"></a>De bron van het probleem vinden
De SSH-client op uw computer mogelijk niet kan bereiken van de SSH-service op de Azure VM vanwege problemen of configuratiefouten in de volgende gebieden:

* [SSH-clientcomputer](#source-1-ssh-client-computer)
* [Randapparaat organisatie](#source-2-organization-edge-device)
* [De cloud service-eindpunt en toegangsbeheerlijsten (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Netwerkbeveiligingsgroepen](#source-4-network-security-groups)
* [Azure virtuele machine op basis van Linux](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Bron 1: SSH clientcomputer
Om te voorkomen de computer als de bron van de fout, Controleer of dat deze SSH-verbindingen met een andere on-premises, op basis van Linux-computer kan maken.

![Diagram illustreert de onderdelen voor clientcomputers SSH](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Als de verbinding mislukt, controleert u de volgende problemen op uw computer:

* Een instelling van lokale firewall binnenkomende of uitgaande SSH-verkeer (TCP 22 blokkeert)
* Lokaal geïnstalleerde clientsoftware proxy waardoor SSH-verbindingen
* Lokaal geïnstalleerde software die wordt verhinderd door een SSH-verbindingen voor netwerkbeheer
* Andere typen beveiligingssoftware die om verkeer te controleren of in een specifieke soorten verkeer toestaan/weigeren

Tijdelijk uitschakelen van de software als een van deze voorwaarden van toepassing, en probeer het een SSH-verbinding met een on-premises computer om erachter te komen de reden dat de verbinding wordt geblokkeerd op uw computer. Vervolgens kunt u werken met de netwerkbeheerder om het corrigeren van de software-instellingen voor SSH-verbindingen.

Als u verificatie via certificaat gebruikt, controleert u of deze machtigingen te hebben tot de SSH-map in de basismap:

* Type chmod 700 ~/.ssh
* Type chmod 644 ~/.ssh/\*.pub
* Type chmod 600 ~/.ssh/id_rsa (of andere bestanden die opgeslagen in deze persoonlijke sleutels zijn)
* Type chmod 644 ~/.ssh/known_hosts (bevat hosts die u via SSH verbinding hebt gemaakt)

## <a name="source-2-organization-edge-device"></a>Bron 2: Organisatie randapparaat
Om te voorkomen de randapparaat van uw organisatie als de bron van de fout, Controleer of dat een computer die direct is verbonden met Internet SSH-verbindingen in uw Azure VM aanbrengen kunt. Als u de virtuele machine via een VPN site-naar-site of een Azure ExpressRoute-verbinding gebruiken, gaat u naar [bron 4: Netwerkbeveiligingsgroepen](#nsg).

![Diagram die organisatie randapparaat markeert](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Als u een computer die direct niet hebt verbonden met Internet, een nieuwe Azure-virtuele machine maken in een eigen resourcegroep of cloudservice en worden gebruikt. Zie voor meer informatie [maken van een virtuele machine waarop Linux wordt uitgevoerd in Azure](quick-create-cli.md). De resourcegroep of de virtuele machine en cloud service verwijderen wanneer u met het testen bent klaar.

Als u een SSH-verbinding met een computer die direct is verbonden met Internet maken kunt, controleert u het apparaat van rand voor uw organisatie voor:

* Een interne firewall verkeer van SSH met het Internet blokkeert
* Een proxyserver die wordt verhinderd door een SSH-verbindingen
* Inbraakdetectie of software die wordt uitgevoerd op apparaten in uw netwerk rand waardoor SSH-verbindingen voor netwerkbeheer

Werken met de netwerkbeheerder om het corrigeren van de instellingen van uw organisatie randapparaten voor SSH-verkeer met Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: De Cloud service-eindpunt en ACL
> [!NOTE]
> Deze bron geldt alleen voor virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel. Voor virtuele machines die zijn gemaakt met Resource Manager, gaat u verder met [4 bron: Netwerkbeveiligingsgroepen](#nsg).

Om te voorkomen de cloud service-eindpunt en ACL als de bron van de fout, Controleer of dat een andere Azure-virtuele machine in hetzelfde virtuele netwerk SSH-verbindingen met uw virtuele machine maken kunt.

![Diagram cloud service-eindpunt en ACL illustreert](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Als u geen een andere virtuele machine in hetzelfde virtuele netwerk hebt, kunt u eenvoudig een maken. Zie voor meer informatie [maken van een Linux-VM op Azure met behulp van de CLI](quick-create-cli.md). Verwijder de extra VM wanneer u klaar bent met het testen.

Als u een SSH-verbinding met een virtuele machine in hetzelfde virtuele netwerk maken kunt, controleert u de volgende gebieden:

* **De eindpuntconfiguratie voor SSH-verkeer op de doel-virtuele machine.** De particuliere TCP-poort van het eindpunt moet overeenkomen met de TCP-poort waarop de SSH-service op de virtuele machine luistert. (De standaardpoort is 22). Controleer het nummer van de SSH TCP-poort in de Azure portal door te kiezen **virtuele machines** > *VM-naam* > **instellingen**  >   **Eindpunten**.
* **De ACL voor het eindpunt van het SSH-verkeer op de virtuele doelmachine.** Een ACL kunt u opgeven toegestaan of geweigerd binnenkomend verkeer vanaf Internet, op basis van de bron-IP-adres. Onjuist geconfigureerde ACL's kunnen voorkomen dat binnenkomend verkeer van SSH met het eindpunt. Controleer de ACL's om ervoor te zorgen dat binnenkomend verkeer van het openbare IP-adressen van uw proxy of andere edge-server is toegestaan. Zie voor meer informatie [over toegang tot het netwerk toegangsbeheerlijsten (ACL's)](../../virtual-network/virtual-networks-acl.md).

Het eindpunt als bron van het probleem te voorkomen, verwijder het eindpunt van de huidige, maken van een ander eindpunt en geef de naam van SSH (TCP-poort 22 voor de openbare en persoonlijke poortnummer). Zie voor meer informatie [-eindpunten op een virtuele machine in Azure instellen](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>Bron 4: Netwerkbeveiligingsgroepen
Netwerkbeveiligingsgroepen kunnen u meer gedetailleerde controle over de toegestane binnenkomend en uitgaand verkeer. U kunt regels die subnetten omvatten en cloudservices in een Azure-netwerk maken. Controleer uw netwerkbeveiligingsgroepen om ervoor te zorgen dat SSH verkeer van en naar Internet is toegestaan.
Zie voor meer informatie [over netwerkbeveiligingsgroepen](../../virtual-network/virtual-networks-nsg.md).

U kunt IP-controleren ook gebruiken om het NSG-configuratie te valideren. Zie voor meer informatie [Azure-netwerk bewakingsoverzicht](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>Bron 5: Op basis van Linux virtuele machine van Azure
De laatste bron van mogelijke problemen met is de virtuele machine van Azure zelf.

![Diagram die worden gemarkeerd op basis van Linux virtuele machine van Azure](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Als u dit nog niet hebt gedaan, volgt u de instructies [opnieuw instellen van een wachtwoord of SSH voor virtuele machines op basis van Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

Probeer opnieuw verbinding te maken van uw computer. Als het nog steeds mislukt, Hier volgen enkele mogelijke problemen:

* De SSH-service wordt niet uitgevoerd op de virtuele doelmachine.
* De SSH-service luistert niet op TCP-poort 22. Om te testen, een Telnet-client installeren op uw lokale computer en voer ' telnet *cloudServiceName*. cloudapp.net 22 '. Deze stap bepaalt als de virtuele machine binnenkomende en uitgaande communicatie met het SSH-eindpunt kunt.
* De lokale firewall op de virtuele doelmachine heeft regels die verhinderen binnenkomend of uitgaand verkeer van SSH dat.
* Inbraakdetectie of software die wordt uitgevoerd op virtuele machine van Azure voor netwerkbeheer houdt SSH-verbindingen.

## <a name="additional-resources"></a>Aanvullende bronnen
Zie voor meer informatie over het oplossen van toegang tot toepassingen [toegang tot een toepassing die wordt uitgevoerd op een virtuele machine van Azure oplossen](troubleshoot-app-connection.md)
