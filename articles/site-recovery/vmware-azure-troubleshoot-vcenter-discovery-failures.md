---
title: Failback naar on-premises oplossen tijdens het herstel van VMware-VM na noodgevallen naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel worden manieren voor het oplossen van problemen met failback en opnieuw beveiligen tijdens het herstel van VMware-VM na noodgevallen naar Azure met Azure Site Recovery beschreven.
author: vDonGlover
manager: JarrettRenshaw
ms.service: site-recovery
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: v-doglov
ms.openlocfilehash: c598c5e238458c010500579c5371622b85e71de0
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540802"
---
# <a name="troubleshoot-vcenter-discovery-failures"></a>vCenter-detectiefouten oplossen

In dit artikel helpt u bij het oplossen van problemen die vanwege VMware vCenter-detectie van problemen optreden.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>Niet-numerieke waarden in de eigenschap maxSnapShots

In eerdere versies 9.20, vCenter wordt verbroken bij het ophalen van een niet-numerieke waarde voor de eigenschap `snapshot.maxSnapShots` eigenschap op een virtuele machine.

Dit probleem wordt aangeduid met fout-ID 95126.

    ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
    System.FormatException: Input string was not in a correct format.
       at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
       at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
       at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
    
Het probleem kunt oplossen:

- Identificeren van de virtuele machine en de waarde ingesteld op een numerieke waarde (bewerken van de virtuele machine-instellingen in vCenter).

of

- Uw configuratieserver bijwerken naar versie 9.20 of hoger.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>Problemen met de proxyconfiguratie voor verbinding met vCenter

vCenter detectie zich houdt aan het systeem standaard proxy-instellingen die door de gebruiker van het systeem. De service DRA zich houdt aan de proxy-instellingen opgegeven door de gebruiker tijdens de installatie van configuratieserver met behulp van de installatie van unified installer of een OVA-sjabloon. 

In het algemeen wordt de proxy gebruikt om te communiceren met een openbaar netwerk; zoals het communiceren met Azure. Als de proxy is geconfigureerd en vCenter in een lokale omgeving is, niet het mogelijk zijn om te communiceren met DRA.

De volgende situaties zich voordoen wanneer dit probleem is opgetreden:

- De vCenter-server \<vCenter > is niet bereikbaar vanwege de volgende fout: De externe server heeft een fout geretourneerd: (503) Server Unavailable
- De vCenter-server \<vCenter > is niet bereikbaar vanwege de volgende fout: De externe server heeft een fout geretourneerd: Kan geen verbinding maken met de externe server.
- Kan geen verbinding maken met vCenter-/ ESXi-server.

Het probleem kunt oplossen:

Download de [PsExec-hulpprogramma](https://aka.ms/PsExec). 

Het PsExec-hulpprogramma gebruiken voor toegang tot de systeemcontext van de gebruiker en bepalen of de proxy-adres is geconfigureerd. Vervolgens kunt u vCenter toevoegen aan de bypass-lijst met de volgende procedures.

Voor detectie van proxy-configuratie:

1. Open Internet Explorer in de systeemcontext van gebruiker met het PsExec-hulpprogramma.
    
    psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"

2. De proxy-instellingen in Internet Explorer om over te slaan van de vCenter-IP-adres wijzigen.
3. Start de service tmanssvc.

Voor de configuratie van DRA-proxy:

1. Open een opdrachtprompt en open de map Microsoft Azure Site Recovery Provider.
 
    **cd C:\Program Files\Microsoft Azure Site Recovery Provider**

3. Voer de volgende opdracht vanaf de opdrachtprompt.
   
   **DRCONFIGURATOR. EXE / /AddBypassUrls configureren [IP-adres/de FQDN van vCenter-Server die worden geleverd op het moment van de vCenter toevoegen]**

4. Het DRA-provider-service opnieuw starten.

## <a name="next-steps"></a>Volgende stappen

[De configuratieserver voor VMware-VM-noodherstel beheren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-manage-configuration-server#refresh-configuration-server) 
