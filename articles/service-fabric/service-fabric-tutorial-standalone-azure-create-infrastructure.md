---
title: Zelfstudie voor het maken van de infrastructuur voor een Service Fabric-cluster op Azure Virtual machines - Azure Service Fabric | Microsoft Docs
description: In deze zelfstudie leert u over het instellen van de virtuele machine van Azure-infrastructuur om uit te voeren van een Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: v-vasuke
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/25/2019
ms.author: v-vasuke
ms.custom: mvc
ms.openlocfilehash: b5f2f77b3caed483aed1736bd510096d44329284
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276004"
---
# <a name="tutorial-create-azure-vm-infrastructure-to-host-a-service-fabric-cluster"></a>Zelfstudie: Azure VM-infrastructuur voor het hosten van een Service Fabric-cluster maken

Zelfstandige Service Fabric-clusters bieden u de mogelijkheid om uw eigen omgeving te kiezen en een cluster te maken als onderdeel van de benadering "Elk besturingssysteem, elke cloud" die we in Service Fabric hanteren. In deze zelfstudie, moet u een zelfstandige cluster die worden gehost op Azure Virtual machines maken en installeren van een toepassing op deze.

Deze zelfstudie is deel één van een serie. In dit artikel hebt u de virtuele machine van Azure-resources die vereist voor het hosten van uw zelfstandige cluster van Service Fabric genereren. In andere artikelen uit deze serie gaat u een zelfstandig Service Fabric-cluster installeren, een voorbeeldtoepassing in het cluster installeren en ten slotte het cluster opschonen.

In deel 1 van de reeks leert u het volgende:

> [!div class="checklist"]
> * Een set van exemplaren van de installatiekopieën van virtuele machines maken
> * De beveiligingsgroep wijzigen
> * Aanmelden bij een van de exemplaren
> * Het exemplaar voorbereiden voor Service Fabric

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-abonnement nodig om deze zelfstudie te voltooien.  Als u nog een account hebt, gaat u naar de [Azure-portal](https://portal.azure.com) een te maken.

## <a name="create-azure-virtual-machine-instances"></a>Virtuele Azure-Machine-instanties maken

1. Aanmelden bij de Azure portal en selecteer **virtuele machines** (geen virtuele Machines (klassiek)).

   ![Virtuele machine van Azure portal][az-console]

2. Selecteer de **toevoegen** knop, die wordt geopend de **maken van een virtuele machine** formulier.

3. In de **basisbeginselen** tabblad, zorg ervoor dat u selecteert u de abonnement en de resourcegroep waarin u wilt (met behulp van een nieuwe resourcegroep wordt aanbevolen).

4. Wijzig de **installatiekopie** type **Windows Server 2016 Datacenter**. 
 
5. Wijzigen van het exemplaar **grootte** naar **Standard DS2 versie 2**. Stel een beheerdersnaam **gebruikersnaam** en **wachtwoord**, wat ze zijn waard.

6. Laat de **regels voor binnenkomende poort** geblokkeerd voorlopig; configureren we die in de volgende sectie.

7. In de **netwerken** tabblad, het maken van een nieuwe **Virtueelnetwerk** en noteer de naam ervan.

8. Vervolgens stelt de **NIC netwerkbeveiligingsgroep** naar **Geavanceerd**. Maak een nieuwe beveiligingsgroep, waarbij de naam en maak de volgende regels voor TCP-verkeer van elke bron:

   ![sf-inbound][sf-inbound]

   * Poort `3389`voor RDP en ICMP (basisconnectiviteit).
   * Poorten `19000-19003`, voor Service Fabric.
   * Poorten `19080-19081`, voor Service Fabric.
   * Poort `8080`, voor web browseraanvragen.

   > [!TIP]
   > Om uw virtuele machines te verbinden in Service Fabric, moeten de VM's die uw infrastructuur hosten dezelfde referenties hebben.  Er zijn twee manieren om consistente referenties te hanteren: alle machines toevoegen aan hetzelfde domein of op elke VM hetzelfde beheerderswachtwoord instellen. Gelukkig Azure kan alle virtuele machines op dezelfde **virtueel netwerk** eenvoudig verbinding maken, zodat we ervoor dat u hebt alle onze exemplaren op hetzelfde netwerk.

9. Een andere regel toevoegen. Instellen van de bron worden **servicetag** en stelt u de bronservicetag op **VirtualNetwork**. Service Fabric is vereist voor de volgende poorten zijn geopend voor communicatie binnen het cluster: 135,137-139,445,20001-20031,20606-20861.

   ![vnet-inbound][vnet-inbound]

10. De rest van de opties worden geaccepteerd in de standaardstatus heeft. Bekijk deze als u dat wilt, en start vervolgens de virtuele machine.

## <a name="creating-more-instances-for-your-service-fabric-cluster"></a>Meer instanties voor uw Service Fabric-cluster maken

Start twee **virtuele Machines**, moet u dezelfde instellingen die worden beschreven in de vorige sectie onderhouden. In het bijzonder onderhouden hetzelfde administrator-gebruikersnaam en wachtwoord. De **Virtueelnetwerk** en **NIC netwerkbeveiligingsgroep** niet opnieuw moet worden gemaakt; Selecteer de items die u al hebt gemaakt in de vervolgkeuzelijst. Het duurt een paar minuten voor elk van uw instanties worden geïmplementeerd.

## <a name="connect-to-your-instances"></a>Verbinding maken met uw exemplaren

1. Selecteer een van uw instanties uit de **virtuele Machine** sectie.

2. In de **overzicht** tabblad, noteer de *persoonlijke* IP-adres. Klik vervolgens op **Connect**.

3. In de **RDP** tabblad, houd er rekening mee dat we gebruiken het openbare IP-adres en poort 3389, waarmee we specifiek eerder hebt geopend. Download het RDP-bestand.
 
4. Open het RDP-bestand, en wanneer u hierom wordt gevraagd invoeren de gebruikersnaam en het wachtwoord die u hebt opgegeven in de instellingen van de virtuele machine.

5. Nadat u met een exemplaar verbonden bent, moet u controleren of extern register werd uitgevoerd en de vereiste poorten openen voor SMB en remote registry inschakelen van SMB.

   Inschakelen van SMB, is dit de PowerShell-opdracht:

   ```powershell
   netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
   ```

6. Dit is de PowerShell-opdracht voor het openen van de poorten in de firewall:

   ```powershell
   New-NetFirewallRule -DisplayName "Service Fabric Ports" -Direction Inbound -Action Allow -RemoteAddress LocalSubnet -Protocol TCP -LocalPort 135, 137-139, 445
   ```

7. Herhaal dit proces voor de andere exemplaren, opnieuw waard privé IP-adressen.

## <a name="verify-your-settings"></a>Uw instellingen verifiëren

1. Voor het valideren van de basisconnectiviteit, verbinding maken met een van de virtuele machines met behulp van RDP.

2. Open de **opdrachtprompt** uit binnen die virtuele machine, gebruik vervolgens de ping-opdracht verbinding maken tussen één virtuele machine en een ander, vervangen de onderstaande IP-adres met een van de privé-IP-adressen u eerder hebt genoteerd (niet het IP-adres van de virtuele machine waarmee u bent verbonden al).

   ```
   ping 172.31.20.163
   ```

   Als de uitvoer er gedurende vier pogingen uitziet zoals `Reply from 172.31.20.163: bytes=32 time<1ms TTL=128`, werkt de verbinding tussen de exemplaren.

3. Gebruik nu de volgende opdracht om te controleren of delen via SMB werkt:

   ```
   net use * \\172.31.20.163\c$
   ```

   De uitvoer moet gelijk zijn aan `Drive Z: is now connected to \\172.31.20.163\c$.`.


   De exemplaren worden nu correct voorbereid voor Service Fabric.

## <a name="next-steps"></a>Volgende stappen

In deel één van de serie, hebt u geleerd hoe u drie Azure VM-exemplaren te starten en ze geconfigureerd voor de Service Fabric-installatie:

> [!div class="checklist"]
> * Een set van Azure VM-exemplaren maken
> * De beveiligingsgroep wijzigen
> * Aanmelden bij een van de exemplaren
> * Het exemplaar voorbereiden voor Service Fabric

Ga naar deel twee van de reeks om Service Fabric te configureren in uw cluster.

> [!div class="nextstepaction"]
> [Service Fabric installeren](service-fabric-tutorial-standalone-create-service-fabric-cluster.md)

<!-- IMAGES -->
[az-console]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/az-console.png
[sf-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/sf-inbound.png
[vnet-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/vnet-inbound.png
