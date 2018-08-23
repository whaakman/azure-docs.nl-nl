---
title: Problemen met het netwerk virtueel apparaat in Azure | Microsoft Docs
description: Informatie over het oplossen van het virtuele apparaat netwerkproblemen in Azure.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2018
ms.author: genli
ms.openlocfilehash: d16f106fa1776b0d11a97f71ffe16cf89fb9584a
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42056898"
---
#  <a name="network-virtual-appliance-issues-in-azure"></a>Virtueel apparaat netwerkproblemen in Azure

Mogelijk is uw virtuele machine of problemen met VPN-connectiviteit en fouten bij het gebruik van een derde partij Network Virtual Appliance (NVA) in Microsoft Azure. In dit artikel biedt eenvoudige stappen om te kunnen valideren basisvereisten voor Azure-Platform voor de NVA-configuraties.

Technische ondersteuning voor NVA's van derden en de integratie met de Azure-platform wordt geleverd door de leverancier van de NVA. Als u een verbinding of een routeringsprobleem dat betrekking heeft op een NVA hebt, moet u [Neem contact op met de leverancier van de NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines) rechtstreeks.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="checklist-for-troubleshooting-with-nva-vendor"></a>Controlelijst voor het oplossen van problemen met de leverancier van de NVA

- Software-updates voor software van de NVA-VM
- Service-Account instellen en de functionaliteit
- Gebruiker gedefinieerde routes (udr's) op virtuele subnetten waarmee verkeer naar de NVA leiden
- Udr's op subnetten van het virtuele netwerk dat verkeer van de NVA leiden
- Routering tabellen en regels binnen het NVA (bijvoorbeeld van NIC1 naar NIC2)
- Tracering op NVA-NIC's om te controleren of ontvangen en verzenden van netwerkverkeer

## <a name="basic-troubleshooting-steps"></a>Basisstappen voor het oplossen van problemen

- Controleer de configuratie van de basic
- NVA-prestaties controleren
- Geavanceerde netwerkproblemen

## <a name="check-the-minimum-configuration-requirements-for-nvas-on-azure"></a>Controleer de minimale configuratievereisten voor NVA's in Azure

Elke NVA heeft basisconfiguratie vereisten goed te laten functioneren op Azure. De volgende sectie bevat de stappen om te controleren of deze basisconfiguraties. Voor meer informatie, [Neem contact op met de leverancier van de NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

**Controleer of doorsturen via IP is ingeschakeld op NVA**

Azure Portal gebruiken

1.  Zoek de NVA-resource in de [Azure-portal](https://portal.azure.com), selecteert u netwerken, en selecteer vervolgens de netwerkinterface.
2.  Selecteer op de pagina Network interface-IP-configuratie.
3.  Zorg ervoor dat doorsturen via IP is ingeschakeld.

PowerShell gebruiken

1. Open PowerShell en meldt u zich aan bij uw Azure-account.
2. Voer de volgende opdracht uit (Vervang de tussen haakjes waarden door uw gegevens):

        Get-AzureRmNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NicName>  

3. Controleer de **EnableIPForwarding** eigenschap.
 
4. Als doorsturen via IP is niet ingeschakeld, moet u de volgende opdrachten te kunnen uitvoeren:

          $nic2 = Get-AzureRmNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NicName>
          $nic2.EnableIPForwarding = 1
          Set-AzureRmNetworkInterface -NetworkInterface $nic2
          Execute: $nic2 #and check for an expected output:
          EnableIPForwarding   : True
          NetworkSecurityGroup : null

**Controleer of het verkeer kan worden gerouteerd naar de NVA**

1. Op [Azure-portal](https://portal.azure.com)Open **Network Watcher**, selecteer **' volgende hop '**.
2. Geef een virtuele machine die u wilt dat de NVA de volgende hop en een doel-IP-adres waarop om de volgende hop weer te geven. 
3. Als de NVA niet wordt vermeld als de **volgende hop**, controleren en bijwerken van de Azure-routetabellen.

**Controleer of het verkeer de NVA kan bereiken**

1.  In [Azure-portal](https://portal.azure.com)Open **Network Watcher**, en selecteer vervolgens **IP-stroom controleren**. 
2.  Geef een virtuele machine en het IP-adres van de NVA en controleer vervolgens of het verkeer wordt geblokkeerd door een netwerkbeveiligingsgroepen (NSG).
3.  Als er een NSG-regel waarmee het verkeer wordt geblokkeerd, zoek de NSG in **effectieve** regels en werk vervolgens het toestaan van verkeer om door te geven. Voer **IP-stroom controleren** opnieuw en gebruik **Connectiviteitscontrole** voor het testen van TCP-communicatie van VM naar uw intern of extern IP-adres.

**Controleer of NVA en virtuele machines voor het verwachte verkeer luisteren**

1.  Verbinding maken met de NVA met behulp van RDP of SSH en voer de volgende opdracht:

    Voor Windows:

        netstat -an

    Voor Linux:

        netstat -an | grep -i listen
2.  Als u de TCP-poort die wordt gebruikt door de NVA-software die wordt vermeld in de resultaten niet ziet, moet u de toepassing configureren op de NVA en de virtuele machine om te luisteren naar en reageren op verkeer die geschikt is voor deze poorten. [Neem contact op met de leverancier van de NVA behoefte](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

## <a name="check-nva-performance"></a>NVA-prestaties controleren

### <a name="validate-vm-cpu"></a>Valideren van de VM-CPU

Als het CPU-gebruik afkomstig bijna 100 procent is, kunt u probleem die invloed hebben op network pakket val kan optreden. Uw rapporten VM het gemiddelde CPU-voor een bepaalde periode in de Azure-portal. Tijdens een piek CPU onderzoek welk proces op de Gast-VM wordt veroorzaakt door de hoge CPU en, indien mogelijk beperken. U mogelijk ook om de grootte van de virtuele machine naar een grotere SKU-grootte of, voor virtuele-machineschaalset, het aantal instanties verhogen of ingesteld op automatisch schalen op CPU-gebruik. Voor een van deze problemen [voor hulp contact op met de leverancier van de NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines), indien nodig.

### <a name="validate-vm-network-statistics"></a>Statistieken van de VM-netwerk valideren 

Als het VM-netwerk met pieken of perioden met een hoog gebruik, ziet dat u mogelijk ook om de grootte van de SKU van de virtuele machine voor hogere doorvoer die kenmerkend zijn. U kunt ook de virtuele machine opnieuw implementeren door te laten versnelde netwerken ingeschakeld. Om te controleren of de NVA ondersteuning biedt voor versnelde netwerken functie [voor hulp contact op met de leverancier van de NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines), indien nodig.

## <a name="advanced-network-administrator-troubleshooting"></a>Geavanceerde netwerkbeheerder oplossen van problemen

### <a name="capture-network-trace"></a>Netwerktracering vastleggen
Een gelijktijdige netwerktracering op de bron-VM, de NVA en de doel-VM vastleggen tijdens het uitvoeren van **[PsPing](https://docs.microsoft.com/sysinternals/downloads/psping)** of **Nmap**, en vervolgens de tracering stoppen.

1. Voor het vastleggen van een gelijktijdige netwerktracering, voer de volgende opdracht:

    Voor Windows:

        netsh trace start capture=yes tracefile=c:\server_IP.etl scenario=netconnection

    Voor Linux:

        sudo tcpdump -s0 -i eth0 -X -w vmtrace.cap

2. Gebruik **PsPing** of **Nmap** uit de bron-VM naar de bestemming VM (bijvoorbeeld: `PsPing 10.0.0.4:80` of `Nmap -p 80 10.0.0.4`).

3. Open de netwerktracering van de doel-VM met behulp van [Network Monitor](https://www.microsoft.com/download/details.aspx?id=4865) of tcpdump. Pas een weergavefilter toe voor de IP-adres van de bron-VM die u hebt uitgevoerd **PsPing** of **Nmap** uit, zoals `IPv4.address==10.0.0.4 (Windows netmon)` of `tcpdump -nn -r vmtrace.cap src or dst host 10.0.0.4` (Linux).

### <a name="analyze-traces"></a>Traceringen analyseren

Als u de inkomende pakketten van de tracering van de back-end-VM niet ziet, er is waarschijnlijk een NSG of UDR onderbreekt of de NVA-routeringstabellen zijn onjuist.

Als de inkomende pakketten wel worden weergegeven, maar er geen antwoord is, moet u mogelijk een probleem met een VM-toepassing of firewall oplossen. Voor een van deze problemen [Neem contact op met de leverancier van de NVA behoefte](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

