---
title: Valideren van de VPN-doorvoer met een Microsoft Azure Virtual Network | Microsoft Docs
description: Het doel van dit document is om te helpen een gebruiker de netwerkdoorvoer van hun lokale bronnen om een virtuele machine van Azure te valideren.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: e7e3c641791e7c72f5c2d6f8ecf674d1d7ee7ffa
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a>Het valideren van de VPN-doorvoer met een virtueel netwerk

Een VPN-gatewayverbinding kunt u veilige, cross-premises connectiviteit tussen uw virtuele netwerk in Azure en uw on-premises IT-infrastructuur.

Dit artikel laat zien hoe u controleert de netwerkdoorvoer van de lokale bronnen aan een Azure virtuele machine (VM). Het bevat ook richtlijnen voor probleemoplossing.

>[!NOTE]
>In dit artikel is bedoeld om u te helpen opsporen en oplossen van veelvoorkomende problemen. Als u het probleem oplossen door de volgende informatie kan [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Overzicht

De VPN-gatewayverbinding omvat de volgende onderdelen:

- On-premises VPN-apparaat (een lijst weergeven met [gevalideerde VPN-apparaten)](vpn-gateway-about-vpn-devices.md#devicetable).
- Openbare Internet
- Azure VPN-gateway
- Azure VM

Het volgende diagram toont de logische verbindingen van een on-premises netwerk naar een Azure-netwerk via VPN-verbinding.

![Logische connectiviteit van de Klantnetwerk MSFT netwerk via VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a>De maximale verwachte inkomend en uitgaand berekenen

1.  Bepaal de basisvereisten voor doorvoer van uw toepassing.
2.  Bepaal de doorvoerlimieten van uw Azure VPN-gateway. Help-informatie, Zie de sectie 'Geaggregeerde doorvoer per SKU en VPN-type' [Planning en ontwerp voor VPN-Gateway](vpn-gateway-plan-design.md).
3.  Bepaal de [Azure VM doorvoer richtlijnen](../virtual-machines/virtual-machines-windows-sizes.md) voor uw VM-grootte.
4.  Bepaal uw bandbreedte Internetproviderverbindingen (ISP).
5.  Berekenen van de verwachte doorvoer - minimale bandbreedte van (ISP VM,-Gateway) * 0,8.

Als uw berekende doorvoer voldoet niet aan de basisvereisten doorvoer van uw toepassing, moet u de bandbreedte van de resource die u hebt geïdentificeerd als het knelpunt vergroten. Als u een Azure VPN-Gateway, Zie [wijzigen van een gateway-SKU](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku). Als u een virtuele machine, Zie [vergroten of verkleinen van een virtuele machine](../virtual-machines/virtual-machines-windows-resize-vm.md). Als u niet de verwachte internetbandbreedte ondervindt nog, kunt u ook contact opnemen met uw Internetprovider.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Netwerkdoorvoer valideren met behulp van hulpprogramma's voor prestaties

Deze validatie moet worden uitgevoerd tijdens daluren, zoals VPN-tunnel doorvoer verzadiging tijdens het testen geen nauwkeurige resultaten geeft.

Het hulpprogramma we voor deze test gebruiken is iPerf die werkt op Windows- en Linux- en heeft zowel client als server modi. Het is beperkt tot 3 Gbps voor VM's van Windows.

Dit hulpprogramma voert geen bewerkingen lezen/schrijven naar de schijf. Het resultaat uitsluitend zelfgegenereerd TCP-verkeer van één end naar het andere. Deze statistieken op basis van experimenten die de bandbreedte die beschikbaar zijn tussen client en server knooppunten meet zijn gegenereerd. Bij het testen van tussen twee knooppunten, fungeert een als de server en de andere als een client. Als deze test is voltooid, wordt u aangeraden dat u de functies voor het testen van beide uploaden en downloaden van doorvoer op beide knooppunten ongedaan maken.

### <a name="download-iperf"></a>IPerf downloaden
Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Zie voor meer informatie [iPerf documentatie](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >De producten van derden die in dit artikel wordt beschreven, zijn geproduceerd door bedrijven die onafhankelijk van Microsoft zijn. Microsoft geeft geen garantie, impliciet of anderszins, over de prestaties en betrouwbaarheid van deze producten.
 >
 >

### <a name="run-iperf-iperf3exe"></a>Voer iPerf (iperf3.exe)
1. Een NSG/ACL regel voor het verkeer (voor openbare IP-adres testen op Azure VM) inschakelen.

2. Schakel een firewall-uitzondering voor poort 5001 op beide knooppunten.

    **Windows:** de volgende opdracht uitvoeren als beheerder:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    Als u wilt de regel verwijderen bij het testen is voltooid, voert u deze opdracht:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    **Azure Linux:** Azure Linux afbeeldingen hebt strikte firewalls. Als er een toepassing naar een poort luistert, wordt door het verkeer toegestaan. Aangepaste installatiekopieën die zijn beveiligd wellicht poorten expliciet geopend. Algemene firewalls voor Linux OS-laag omvatten `iptables`, `ufw`, of `firewalld`.

3. Wijzig de map waar iperf3.exe is uitgepakt op de server-knooppunt. Vervolgens iPerf in de servermodus en stel deze luisteren op poort 5001 als de volgende opdrachten:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. Ga op het clientknooppunt naar de map waar iperf hulpprogramma uitgepakt en voer de volgende opdracht:

    ```CMD
    iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
    ```

    De client wordt verkeer op poort 5001 naar de server gedurende 30 seconden veroorzaken. De vlag '-P ' die aangeeft dat we 32 gelijktijdige verbindingen met het serverknooppunt gebruiken.

    De uitvoer van dit voorbeeld ziet u het volgende scherm:

    ![Uitvoer](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. (OPTIONEEL) Als u wilt de testresultaten behouden, moet u deze opdracht uitvoeren:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. Na het voltooien van de vorige stappen uitvoeren u dezelfde stappen aan de rollen teruggedraaid, zodat het serverknooppunt nu de client en vice versa.

## <a name="address-slow-file-copy-issues"></a>Problemen met adres trage bestanden kopiëren
Er kunnen optreden langzaam bestand kopiëren wanneer u Windows Verkenner of slepen en neerzetten via een RDP-sessie. Dit probleem wordt normaal gesproken veroorzaakt door een of beide van de volgende factoren:

- Bestand kopiëren toepassingen, zoals Windows Verkenner en RDP, meerdere threads niet gebruiken tijdens het kopiëren van bestanden. Gebruik voor betere prestaties een kopie-toepassing met meerdere threads bestand, zoals [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) kopiëren van bestanden met behulp van 16 of 32 threads. Het nummer van de thread voor het kopiëren van bestanden in Richcopy, klikt u op **actie** > **opties kopiëren** > **bestanden zijn gekopieerd**.<br><br>
![Problemen met trage bestanden kopiëren](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Onvoldoende VM schijf lezen/schrijven-snelheid. Zie voor meer informatie [Azure Storage probleemoplossing](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>Lokale apparaat extern gerichte interface
Als de on-premises VPN-apparaat internetgerichte IP-adres is opgenomen in de [lokale netwerk](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definitie in Azure, treden onvermogen om de VPN-verbinding, sporadisch verbinding verbreekt omhoog of prestatieproblemen.

## <a name="checking-latency"></a>Latentie controleren
Tracert te traceren naar Microsoft Azure Edge-apparaat gebruiken om te bepalen of er meer dan 100 milliseconden tussen hops vertragingen zijn.

Voer vanuit de on-premises netwerk *tracert* naar het VIP van de Azure-Gateway of VM. Eenmaal u alleen ziet * geretourneerd, u weet dat u de Azure rand hebt bereikt. Wanneer u DNS-namen met 'MSN' geretourneerd ziet, weet u dat u de Microsoft-backbone hebt bereikt.<br><br>
![Latentie controleren](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende koppelingen voor meer informatie en hulp:

- [Netwerkdoorvoer voor Azure virtual machines optimaliseren](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [Microsoft-ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
