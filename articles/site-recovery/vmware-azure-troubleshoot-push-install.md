---
title: Azure Site Recovery probleemoplossing van VMware naar Azure | Microsoft Docs
description: Fouten bij het repliceren van virtuele machines van Azure oplossen.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.author: ramamill
ms.date: 07/06/2018
ms.openlocfilehash: 8d5db03eeebb659414ea1f554e5b34c938fd2795
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952906"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Problemen met de Mobility-Service push-installatie

In dit artikel wordt beschreven hoe u oplossen van veelvoorkomende fouten die u tegenkomen kan wanneer u probeert te installeren van Azure Site Recovery Mobility Service op de bronserver beveiliging in te schakelen.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Fout 78007 - de aangevraagde bewerking kan niet worden voltooid.
Deze fout kan worden gegenereerd door de service om verschillende redenen. Kies de bijbehorende providerfout probleem verder oplossen.

* [Fout 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Fout 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Fout 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Fout 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Fout 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Fout 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Fout 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Fout 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Fout 95105 - beveiliging kan niet worden ingeschakeld (EP0856)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95105 </br>**Bericht:** Push-installatie van Mobility Service naar de bronmachine is mislukt met foutcode **EP0856**. <br> Een van beide **bestands- en printerdeling** is niet toegestaan op de bron-machine of er zijn problemen met de netwerkverbinding tussen de processerver en de bron-VM-netwerk.| **Bestands- en printerdeling** is niet ingeschakeld. | Toestaan dat **bestands- en printerdeling** in Windows Firewall op de bronmachine. Op de bronmachine onder **Windows Firewall** > **toestaan een app of functie via Firewall**, selecteer **bestands- en printerdeling voor alle profielen**. </br> Controleer daarnaast de volgende vereisten om de push-installatie is voltooid.<br> Meer informatie over [problemen met het oplossen van WMI](#troubleshoot-wmi-issues).


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Fout 95107 - beveiliging kan niet worden ingeschakeld (EP0858)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95107 </br>**Bericht:** Push-installatie van Mobility Service naar de bronmachine is mislukt met foutcode **EP0858**. <br> De referenties die zijn opgegeven voor het installeren van de Mobility-Service zijn onjuist of het gebruikersaccount heeft onvoldoende bevoegdheden. | De referenties van de gebruiker is opgegeven voor het installeren van de Mobility-Service op de bronmachine zijn onjuist. | Zorg ervoor dat de referenties van de gebruiker opgegeven voor de bronmachine op de configuratieserver juist zijn. <br> Als u wilt toevoegen of bewerken van gebruikersreferenties, gaat u naar de configuratieserver en selecteer **Cspsconfigtool** > **-account beheren**. </br> Controleer daarnaast het volgende [vereisten](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) de push-installatie is voltooid.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Fout 95117 - beveiliging kan niet worden ingeschakeld (EP0865)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95117 </br>**Bericht:** Push-installatie van Mobility Service naar de bronmachine is mislukt met foutcode **EP0865**. <br> Op de broncomputer is niet actief of er zijn problemen met de netwerkverbinding tussen de processerver en de broncomputer. | Problemen met de netwerkverbinding tussen de processerver en de bronserver. | Controleer de connectiviteit tussen de processerver en de bronserver. </br> Controleer daarnaast het volgende [vereisten](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) de push-installatie is voltooid.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Fout 95103 - beveiliging kan niet worden ingeschakeld (EP0854)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95103 </br>**Bericht:** Push-installatie van Mobility Service naar de bronmachine is mislukt met foutcode **EP0854**. <br> Een Windows Management Instrumentation (WMI) is niet toegestaan op de broncomputer of er zijn problemen met de netwerkverbinding tussen de processerver en de broncomputer.| WMI wordt in Windows Firewall geblokkeerd. | WMI in Windows Firewall toestaan. Onder **Windows Firewall** > **toestaan een app of functie via Firewall**, selecteer **WMI voor alle profielen**. </br> Controleer daarnaast het volgende [vereisten](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) de push-installatie is voltooid.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Fout 95213 - beveiliging kan niet worden ingeschakeld (EP0874)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95213 </br>**Bericht:** installatie van Mobility Service op de broncomputer % SourceIP; is mislukt met foutcode **EP0874**. <br> | De versie van het besturingssysteem op de bronmachine wordt niet ondersteund. <br>| Zorg ervoor dat de bron-VM versie van het besturingssysteem wordt ondersteund. Lees de [ondersteuningsmatrix](https://aka.ms/asr-os-support). </br> Controleer daarnaast het volgende [vereisten](https://aka.ms/pushinstallerror) de push-installatie is voltooid.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Fout 95108 - beveiliging kan niet worden ingeschakeld (EP0859)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95108 </br>**Bericht:** Push-installatie van Mobility Service naar de bronmachine is mislukt met foutcode **EP0859**. <br>| De referenties die zijn opgegeven voor het installeren van de Mobility-Service zijn onjuist of het gebruikersaccount heeft onvoldoende bevoegdheden. <br>| Zorg ervoor dat de opgegeven referenties zijn de **hoofdmap** van accountreferenties. Als u wilt toevoegen of bewerken van gebruikersreferenties, gaat u naar de configuratieserver en selecteer de **Cspsconfigtool** pictogram van de snelkoppeling op het bureaublad. Selecteer **-account beheren** toevoegen of bewerken van referenties.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Fout 95265 - beveiliging kan niet worden ingeschakeld (EP0902)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95265 </br>**Bericht:** Push-installatie van Mobility Service op de bronmachine is voltooid, maar de bronmachine moet opnieuw worden opgestart voor bepaalde systeemwijzigingen van kracht te laten worden. <br>| Een oudere versie van Mobility Service is al geïnstalleerd op de server.| Replicatie van de virtuele machine naadloos wordt voortgezet.<br> Start opnieuw op de server tijdens het volgende onderhoudsvenster om de voordelen van de nieuwe uitbreidingen in Mobility-Service.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Fout 95224 - beveiliging kan niet worden ingeschakeld (EP0883)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95224 </br>**Bericht:** Push-installatie van Mobility Service naar de bronmachine % SourceIP; is mislukt met foutcode **EP0883**. Systeem opnieuw wordt opgestart vanuit een eerdere installatie of update is in behandeling.| Het systeem is niet opnieuw opgestart tijdens het verwijderen van een oudere of niet-compatibele versie van Mobility Service.| Zorg ervoor dat er geen versie van Mobility Service op de server bestaat. <br> Start de server opnieuw en voer de taak beveiliging inschakelen opnieuw uit.|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Resource voor het oplossen van problemen met push-installatie

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Bestand problemen op te delen problemen met afdrukken
* [In- of uitschakelen met Groepsbeleid voor het delen van bestanden](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Bestands- en printerdeling via Windows Firewall inschakelen](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>WMI-problemen oplossen
* [WMI-basistest](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-probleemoplossing](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Het oplossen van problemen met WMI-scripts en WMI-services](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](vmware-azure-tutorial.md) het instellen van herstel na noodgevallen voor VMware-VM's.