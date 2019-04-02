---
title: Oplossen van problemen met activering van Windows-virtuele machine in Azure | Microsoft Docs
description: Biedt de stappen voor problemen oplossen voor het oplossen van problemen met de activering van de Windows virtuele machine in Azure
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/15/2018
ms.author: genli
ms.openlocfilehash: 18cd5a86cc2f52567c5f320719d1a9f21b377ed4
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58791708"
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Problemen met Windows Azure virtuele machine-activering

Als u problemen ondervindt bij het activeren van Windows Azure virtuele machine (VM) die is gemaakt op basis van een aangepaste installatiekopie, kunt u de informatie in dit document om het probleem te verhelpen. 

## <a name="understanding-azure-kms-endpoints-for-windows-product-activation-of-azure-virtual-machines"></a>Wat is Azure KMS-eindpunten voor Windows-productactivering van Azure Virtual Machines?

Azure maakt gebruik van verschillende eindpunten voor KMS-activering, afhankelijk van de cloud-regio waar de virtuele machine zich bevindt. Wanneer u deze handleiding voor probleemoplossing, gebruikt u de juiste KMS-eindpunt dat van toepassing voor uw regio is.

* Azure public cloud regions: kms.core.windows.net:1688
* Azure China 21Vianet national cloud regions: kms.core.chinacloudapi.cn:1688
* Azure Duitsland nationale cloud-regio's: kms.core.cloudapi.de:1688
* Azure US Gov national cloud regions: kms.core.usgovcloudapi.net:1688

## <a name="symptom"></a>Symptoom

Als u probeert te activeren van een Windows Azure VM, krijgt u een fout bericht lijkt op het volgende voorbeeld:

**Fout: 0xC004F074 die de Software-LicensingService gerapporteerd dat de computer niet kan worden geactiveerd. Er is geen sleutel managementservice zijn (KMS) kan worden bereikt. Raadpleeg het toepassingsgebeurtenislogboek voor meer informatie.**

## <a name="cause"></a>Oorzaak

Over het algemeen optreden activeringsproblemen virtuele Azure-machine als de Windows-VM niet is geconfigureerd met behulp van de desbetreffende Installatiecode voor KMS-client of de Windows-VM een verbindingsprobleem met de Azure-KMS-service (kms.core.windows.net, poort 1688 heeft). 

## <a name="solution"></a>Oplossing

>[!NOTE]
>Als u een site-naar-site-VPN en geforceerde tunneling, Zie [aangepaste routes gebruiken Azure om in te schakelen van KMS-activering met geforceerde tunneling](https://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>Als u ExpressRoute gebruikt en u hebt een standaard-route die zijn gepubliceerd, Zie [virtuele Azure-machine kan niet worden geactiveerd via ExpressRoute](https://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).

### <a name="step-1-configure-the-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a>Stap 1 de desbetreffende Installatiecode voor KMS-client configureren (voor Windows Server 2016 en Windows Server 2012 R2)

Voor de virtuele machine die is gemaakt op basis van een aangepaste installatiekopie van Windows Server 2016 of Windows Server 2012 R2, moet u de desbetreffende Installatiecode voor KMS-client configureren voor de virtuele machine.

Deze stap is niet van toepassing op Windows 2012 of Windows 2008 R2. De functie activering automatisering van virtuele Machine (AVMA), die wordt alleen ondersteund door Windows Server 2016 en Windows Server 2012 R2 wordt gebruikt.

1. Voer **slmgr.vbs/dlv** bij een opdrachtprompt met verhoogde bevoegdheid. Controleer de waarde van de beschrijving in de uitvoer en vervolgens kunt u bepalen of deze is gemaakt van retail (detailhandel) of (VOLUME_KMSCLIENT) volume license-media:
  

    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. Als in **slmgr.vbs /dlv** het kanaal RETAIL wordt weergegeven, voert u de volgende opdrachten uit om de [installatiecode voor de KMS-client](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) in te stellen voor de gebruikte versie van Windows Server, en dwingt u af dat de activering opnieuw wordt uitgevoerd: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Bijvoorbeeld, voor Windows Server 2016 Datacenter, moet u de volgende opdracht uitvoeren:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>Stap 2 de connectiviteit tussen de virtuele machine en Azure KMS-service controleren

1. Downloaden en uitpakken van de [PSping](http:/technet.microsoft.com/sysinternals/jj729731.aspx) hulpprogramma naar een lokale map op de virtuele machine die niet worden geactiveerd. 

2. Naar begin gaan, zoeken op Windows PowerShell, met de rechtermuisknop op Windows PowerShell en selecteer vervolgens als administrator uitvoeren.

3. Zorg ervoor dat de VM is geconfigureerd om de juiste Azure KMS-server te gebruiken. U doet dit door de volgende opdracht uitvoeren:
  

    ```powershell
    Invoke-Expression "$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms kms.core.windows.net:1688"
    ```

    Met deze opdracht wordt dit geretourneerd: Computernaam van de Key Management Service is ingesteld op kms.core.windows.net:1688.

4. Controleer of met behulp van Psping dat u verbinding met de KMS-server hebben. Ga naar de map waarin u het gedownloade bestand Pstools.zip hebt uitgepakt en voer vervolgens het volgende uit:
  

    ```
    \psping.exe kms.core.windows.net:1688
    ```

  
   Controleer of u in de een-na-laatste regel van de uitvoer het volgende ziet: Verzonden = 4, ontvangen = 4, verloren = 0 (0% verlies).

   Als verloren is groter dan 0 (nul), is de virtuele machine heeft geen verbinding met de KMS-server. In dit geval is als de virtuele machine zich in een virtueel netwerk en heeft een aangepaste DNS-server opgegeven, moet u ervoor zorgen dat DNS-server kunnen omzetten van kms.core.windows.net. Of de DNS-server wijzigt in een kms.core.windows.net is opgelost.

   U ziet dat als u alle DNS-servers van een virtueel netwerk verwijderen, VM's gebruiken de interne DNS-service van Azure. This service can resolve kms.core.windows.net.
  
Controleer ook of de Gast-firewall niet is geconfigureerd op een manier die activering wordt blokkeren.

1. Nadat u geslaagde verbinding met kms.core.windows.net verifieert, voer de volgende opdracht bij die met verhoogde bevoegdheid Windows PowerShell-prompt. Met deze opdracht wordt meerdere keren geprobeerd te activeren.

    ```powershell
    1..12 | ForEach-Object { Invoke-Expression “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

Na een geslaagde activering worden gegevens geretourneerd die er ongeveer als volgt uitzien:

**Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) activeren... Product is geactiveerd.**

## <a name="faq"></a>Veelgestelde vragen 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Ik heb de Windows Server 2016 op Azure Marketplace hebt gemaakt. Heb ik nodig voor het configureren van KMS-sleutel voor het activeren van de Windows Server 2016? 

 
Nee. De installatiekopie in Azure Marketplace is de juiste KMS-clientinstallatiecode al geconfigureerd. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>Windows-activering werkt hetzelfde ongeacht als de virtuele machine van Azure Hybrid Use Benefit (HUB) of niet gebruikmaakt? 

 
Ja. 
 

### <a name="what-happens-if-windows-activation-period-expires"></a>Wat gebeurt er als Windows is verstreken? 

 
Wanneer de respijtperiode is verlopen en Windows is nog niet geactiveerd, wordt Windows Server 2008 R2 en latere versies van Windows aanvullende meldingen over het activeren van weergeven. Achtergrond van het bureaublad blijft zwart en Windows Update wordt geïnstalleerd, beveiliging en alleen essentiële updates, maar niet optionele updates. Zie de sectie meldingen aan de onderkant van de [licentieverlening voorwaarden](https://technet.microsoft.com/library/ff793403.aspx) pagina.   

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.