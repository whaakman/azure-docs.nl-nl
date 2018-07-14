---
title: Valideren van de VPN-doorvoer naar een Microsoft Azure Virtual Network | Microsoft Docs
description: Het doel van dit document is om u te helpen bij het valideren van de netwerkdoorvoer van hun on-premises bronnen met een Azure-machine van een gebruiker.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2018
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 7e6b3e7496c4a063156ff3b8feae1f5096efe55f
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035615"
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a>VPN-doorvoer naar een virtueel netwerk valideren

Een VPN-gatewayverbinding kunt u veilige, cross-premises connectiviteit tussen uw virtuele netwerk in Azure en uw on-premises IT-infrastructuur.

In dit artikel laat zien hoe om te valideren van de netwerkdoorvoer van de on-premises bronnen met een Azure-machine (VM). Het bevat ook richtlijnen voor probleemoplossing.

>[!NOTE]
>In dit artikel is bedoeld om u te helpen vaststellen en oplossen van veelvoorkomende problemen. Als u niet het probleem kunt oplossen met behulp van de volgende informatie, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Overzicht

De VPN-gatewayverbinding omvat de volgende onderdelen:

- On-premises VPN-apparaat (een lijst weergeven met [gevalideerde VPN-apparaten)](vpn-gateway-about-vpn-devices.md#devicetable).
- Openbare Internet
- Azure VPN-gateway
- Azure VM

Het volgende diagram toont de logische verbinding van een on-premises netwerk met een Azure-netwerk via VPN-verbinding.

![Logische verbinding van de Klantnetwerk MSFT-netwerk met behulp van VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a>Berekenen van de maximale verwachte Inkomend/uitgaand verkeer

1.  Bepaal de basisvereisten voor doorvoer van uw toepassing.
2.  Bepaal de doorvoerlimieten van uw Azure VPN-gateway. Voor Help-informatie, Zie de sectie "Geaggregeerde doorvoer per SKU en VPN-type" van [Planning en ontwerp voor VPN-Gateway](vpn-gateway-plan-design.md).
3.  Bepaal de [doorvoerrichtlijnen voor virtuele Azure-machine](../virtual-machines/virtual-machines-windows-sizes.md) voor uw VM-grootte.
4.  Bepaal de bandbreedte Internetproviderverbindingen (ISP).
5.  Bereken uw verwachte doorvoer - minimale bandbreedte van de (ISP VM, Gateway) * 0,8.

Als de berekende doorvoer voldoet niet aan de basisvereisten voor doorvoer van uw toepassing, moet u de bandbreedte van de resource die u hebt geïdentificeerd als het knelpunt vergroten. Als u wilt het formaat van een Azure VPN-Gateway, Zie [wijzigen van een gateway-SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku). Als u wilt het formaat van een virtuele machine, Zie [vergroten of verkleinen van een virtuele machine](../virtual-machines/virtual-machines-windows-resize-vm.md). Als u niet verwachte internetbandbreedte voordoet zich, kunt u kunt ook contact opnemen met uw Internetprovider.

## <a name="validate-network-throughput-by-using-performance-tools"></a>De netwerkdoorvoer valideren met behulp van hulpprogramma's voor prestaties

Deze validatie moet worden uitgevoerd tijdens niet-piekuren, zoals VPN-tunnel doorvoer verzadiging tijdens het testen geen nauwkeurige resultaten geeft.

Het hulpprogramma dat worden gebruikt voor deze test is iPerf deze werkt op zowel Windows als Linux en beschikt over zowel client als server modi. Het is beperkt tot 3 Gbps voor Windows VM's.

Dit hulpprogramma voert geen bewerkingen voor lezen/schrijven naar de schijf. Het genereert uitsluitend zelfgegenereerd TCP-verkeer van het ene naar het andere. Deze statistieken op basis van experimenten die de bandbreedte die beschikbaar zijn tussen client en server-knooppunten meet zijn gegenereerd. Wanneer u test tussen twee knooppunten, fungeert een als de server en de andere als een client. Zodra deze test is voltooid, wordt u aangeraden dat u de rollen voor het testen van beide uploaden en downloaden van doorvoer op beide knooppunten ongedaan maken.

### <a name="download-iperf"></a>IPerf downloaden
Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Zie voor meer informatie, [iPerf documentatie](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >De producten van derden die in dit artikel worden besproken die worden geproduceerd door bedrijven die onafhankelijk van Microsoft zijn. Microsoft biedt geen enkele garantie, impliciet of anderszins, over de prestaties of betrouwbaarheid van deze producten.
 >
 >

### <a name="run-iperf-iperf3exe"></a>IPerf (iperf3.exe) worden uitgevoerd
1. Een NSG/ACL-regel waardoor het verkeer (voor openbare IP-adres testen op Azure VM) inschakelen.

2. Schakel een firewall-uitzondering voor poort 5001 op beide knooppunten.

    **Windows:** uitvoeren van de volgende opdracht uit als beheerder:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    Als u wilt verwijderen van de regel bij het testen is voltooid, voert u deze opdracht uit:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
     
    **Azure Linux:** Azure Linux-installatiekopieën hebben ruime firewalls. Als er een toepassing die luistert op een andere poort, wordt het verkeer is toegestaan via. Aangepaste installatiekopieën die zijn beveiligd, moet mogelijk expliciet geopende poorten. Algemene Linux OS-laag firewalls opnemen `iptables`, `ufw`, of `firewalld`.

3. Wijzig de map waar iperf3.exe is uitgepakt op de server-knooppunt. IPerf uitvoeren in de servermodus en stelt u deze om te luisteren op poort 5001 als de volgende opdrachten:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. Ga naar de map waar iperf hulpprogramma is uitgepakt en voer de volgende opdracht op de client-knooppunt:

    ```CMD
    iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
    ```

    De client is verkeer op poort 5001 naar de server veroorzaken gedurende 30 seconden. De vlag '-P ' die aangeeft dat we van 32 gelijktijdige verbindingen met de server-knooppunt.

    Het volgende scherm ziet u de uitvoer van dit voorbeeld:

    ![Uitvoer](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. (OPTIONEEL) Als u wilt behouden de testresultaten, kunt u deze opdracht uitvoeren:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. Na het voltooien van de vorige stappen, uitvoeren van dezelfde stappen met behulp van de teruggedraaid, zodat het serverknooppunt nu de client en vice versa is.

## <a name="address-slow-file-copy-issues"></a>Oplossen van problemen met kopie langzaam bestand
Kan optreden langzaam bestand kopiëren wanneer met behulp van Windows Explorer of slepen en neer te zetten via een RDP-sessie. Dit probleem wordt gewoonlijk veroorzaakt door een of beide van de volgende factoren:

- Kopiëren-toepassingen, zoals Windows Explorer en RDP-bestand, moet u meerdere threads niet gebruiken bij het kopiëren van bestanden. Voor betere prestaties, zoals een toepassing met meerdere threads bestand kopiëren gebruiken [Richcopy](https://technet.microsoft.com/magazine/2009.04.utilityspotlight.aspx) om bestanden te kopiëren met behulp van 16 of 32 threads. Het nummer van de thread voor het kopiëren van bestanden in Richcopy, klikt u op **actie** > **opties voor kopiëren** > **bestand kopiëren**.<br><br>
![Problemen met trage bestanden kopiëren](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Er is onvoldoende VM schijf lezen/schrijven-snelheid. Zie voor meer informatie, [probleemoplossing van Azure Storage](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>On-premises apparaat extern gerichte interface
Als de on-premises VPN-apparaat internetgerichte IP-adres is opgenomen in de [lokale netwerk](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definitie in Azure, treden onvermogen om aan te brengen tot de VPN-verbinding, sporadisch verbinding verbreekt, of prestatieproblemen.

## <a name="checking-latency"></a>Latentie controleren
Tracert te traceren op Microsoft Azure Edge-apparaat gebruiken om te bepalen of er een vertraging van meer dan 100 ms tussen hops.

Voer vanuit de on-premises netwerk, *tracert* naar het VIP van de Azure-Gateway of de virtuele machine. Als u alleen ziet * wordt geretourneerd, u weet dat u hebt bereikt van de rand van het Azure. Wanneer u DNS-namen die 'MSN zijn' geretourneerd ziet, weet u dat u de Microsoft-backbone hebt bereikt.<br><br>
![Latentie controleren](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie en hulp:

- [Netwerkdoorvoer van virtuele machines van Azure optimaliseren](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [Microsoft-ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
