---
title: Tijd synchroniseren voor virtuele Linux-machines in Azure | Microsoft Docs
description: Tijdsynchronisatie voor virtuele Linux-machines.
services: virtual-machines-linux
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: zarhoads
ms.openlocfilehash: aec36b8ea0f342b133e6844e5a96314c6348289b
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465456"
---
# <a name="time-sync-for-linux-vms-in-azure"></a>Tijdsynchronisatie voor virtuele Linux-machines in Azure

Tijdsynchronisatie is belangrijk voor beveiliging en correlatie tussen gebeurtenissen. Soms wordt gebruikt voor de uitvoering van gedistribueerde transacties. De nauwkeurigheid van de tijd tussen meerdere computersystemen wordt bereikt via synchronisatie. Synchronisatie kan worden beïnvloed door meerdere factoren, met inbegrip van verkeer tussen de tijdbronnen en de computer die de tijd ophalen en opnieuw wordt opgestart. 

Azure wordt ondersteund door de infrastructuur met Windows Server 2016. Windows Server 2016 bevat verbeterde algoritmen gebruikt voor het juiste tijd en de lokale klok te synchroniseren met de UTC-voorwaarde.  De functie Windows Server 2016 nauwkeurige tijd aanzienlijk verbeterd hoe de VMICTimeSync service die geldt voor virtuele machines met de host voor nauwkeurige tijd. Verbeteringen zijn onder meer nauwkeurige initiële tijd op de virtuele machine starten of herstellen van de virtuele machine en interrupt latentie correctie. 

>[!NOTE]
>Voor een snel overzicht van Windows Time-service, Bekijk dit [overzicht op hoog niveau video](https://aka.ms/WS2016TimeVideo).
>
> Zie voor meer informatie, [nauwkeurige tijd voor Windows Server 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Overzicht

Nauwkeurigheid van de klok van een computer is af te meten hoe dicht de klok van de computer is de standaard van de tijd Coordinated Universal Time (UTC). UTC wordt gedefinieerd door een multinationale voorbeeld van nauwkeurige atomic klokken die alleen uitgeschakeld door een tweede 300 jaren worden kan. Maar rechtstreeks lezen van UTC speciale hardware vereist. In plaats daarvan tijdservers worden gesynchroniseerd naar UTC en toegankelijk zijn vanaf andere computers om schaalbaarheid en stabiliteit te bieden. Elke computer heeft time-synchronisatieservice uitvoert die weet welke tijdservers u te gebruiken en controleert regelmatig of als de computerklok moet worden gecorrigeerd en past de tijd indien nodig. 

Azure-hosts worden gesynchroniseerd met de interne Microsoft-tijdservers die hun tijd van Stratum 1 eigendom van Microsoft-apparaten, met GPS antennes in beslag nemen. Virtuele machines in Azure kunnen ofwel afhankelijk zijn van de host om door te geven van de nauwkeurige tijd (*tijd host*) u aan bij de virtuele machine of de virtuele machine rechtstreeks krijgt tijd vanuit een tijdserver of een combinatie van beide. 

Op zelfstandige hardware, leest de Linux-besturingssysteem alleen de hosthardware klok bij het opstarten. Daarna wordt de klok onderhouden met behulp van de timer interrupt in de Linux-kernel. In deze configuratie wordt de klok drift na verloop van tijd. In nieuwere distributies van Linux op Azure, virtuele machines de VMICTimeSync-provider, opgenomen in de Linux-integratieservices (LIS) gebruiken om op te vragen van de host vaker klok updates.

Interactie van de virtuele machine met de host kunnen ook invloed op de klok. Tijdens de [geheugen onderhoud met statusbehoud](maintenance-and-updates.md#memory-preserving-maintenance), virtuele machines zijn onderbroken voor maximaal 30 seconden. Bijvoorbeeld voordat onderhoud begint de klok van de virtuele machine bevat 10:00:00 uur en 28 seconden duurt. Nadat de virtuele machine wordt hervat, de klok op de virtuele machine nog steeds 10:00:00 uur, wat 28 seconden weergeven uit. Op juiste hiervoor de VMICTimeSync service bewaakt wat er gebeurt op de host en wordt u gevraagd om wijzigingen op de virtuele machines om te compenseren.

Zonder tijd synchronisatie werkt, zou de klok op de virtuele machine fouten oplopen. Wanneer er slechts één VM, het effect mogelijk niet significant, tenzij de werkbelasting is zeer nauwkeurige vereist. Maar in de meeste gevallen we hebben meerdere, met elkaar verbonden virtuele machines die gebruikmaken van de tijd voor het bijhouden van transacties en wat de behoeften van de tijd consistent is gedurende de volledige implementatie. Wanneer de tijd tussen virtuele machines anders is, ziet u kan de volgende gevolgen:

- Verificatie mislukt. Beveiligingsprotocollen, zoals Kerberos of technologie afhankelijk zijn van het certificaat zijn afhankelijk van de tijd consistent wordt in de systemen.
- Het is moeilijk te achterhalen wat zich in een systeem hebben voorgedaan als Logboeken (of andere gegevens) niet op tijd akkoord gaat. Dezelfde gebeurtenis wilt bekijken, zoals het op verschillende tijdstippen, maken de correlatie moeilijk is opgetreden.
- Als de klok is uitgeschakeld, kan de facturering onjuist worden berekend.



## <a name="configuration-options"></a>Configuratie-opties

Er zijn in het algemeen drie manieren om de tijdsynchronisatie configureren voor uw virtuele Linux-machines die worden gehost in Azure:

- De standaardconfiguratie voor Azure Marketplace-installatiekopieën maakt gebruik van zowel de NTP-tijd en de VMICTimeSync host-tijd. 
- Host-alleen met behulp van VMICTimeSync.
- Gebruik een andere, externe tijdserver met of zonder VMICTimeSync host-tijd.


### <a name="use-the-default"></a>De standaardwaarde gebruiken

Standaard de meeste Azure Marketplace-installatiekopieën voor Linux worden geconfigureerd om te synchroniseren van twee bronnen: 

- NTP als primaire Hiermee haalt u tijd van een NTP-server. Bijvoorbeeld, Ubuntu 16.04 LTS Marketplace-installatiekopieën gebruiken **ntp.ubuntu.com**.
- De service VMICTimeSync als secundaire gebruikt om te communiceren, de tijd van de host aan de virtuele machines en correcties nadat de virtuele machine is onderbroken voor onderhoud. Apparaten die eigendom Stratum 1 Azure hosts gebruiken om nauwkeurige tijd.

In nieuwere distributies van Linux, de VMICTimeSync-service gebruikt de precisie tijd protocol (PTP), maar eerdere distributies bieden mogelijk geen ondersteuning voor PTP en zal dalen-terug naar NTP voor het ophalen van de tijd van de host.

Om te bevestigen NTP correct wordt gesynchroniseerd, voer de `ntpq -p` opdracht.

### <a name="host-only"></a>Alleen-host 

Omdat de NTP-servers, zoals time.windows.com en ntp.ubuntu.com openbaar zijn, ze tijd synchroniseren is vereist die verkeer verzenden via internet. Pakket vertragingen te variëren, kan de kwaliteit van de tijdsynchronisatie negatief beïnvloeden. NTP verwijderen door over te schakelen om te synchroniseren met alleen-host, kan uw tijd soms verbeteren resultaten synchroniseren.

Overschakelen naar alleen-host tijd synchronisatie kan zinvol als u tijdsynchronisatie ondervindt problemen met behulp van de standaardconfiguratie. Probeer nu de alleen-host synchronisatie om te zien of er die de tijdsynchronisatie op de virtuele machine wilt verbeteren. 

### <a name="external-time-server"></a>Externe tijdserver

Als u specifieke tijd synchronisatie vereisten hebt, is er ook een optie van het gebruik van externe tijdservers. Externe tijdservers kunnen specifieke tijd nuttig voor Testscenario's zijn kan, ervoor te zorgen dat de tijd uniformiteit met machines die worden gehost in niet-Microsoft-datacenters of verwerking leap seconden in een speciale manier bieden.

U kunt een externe tijdserver combineren met de service VMICTimeSync om resultaten die vergelijkbaar is met de standaardconfiguratie te bieden. Een externe tijdserver combineren met VMICTimeSync is de beste optie voor het omgaan met problemen die veroorzaakt worden kunnen wanneer de virtuele machines zijn onderbroken voor onderhoud. 

## <a name="tools-and-resources"></a>Hulpprogramma's en resources

Er zijn enkele eenvoudige opdrachten voor het controleren van de configuratie van uw tijd synchronisatie. Documentatie voor Linux-distributie, meer informatie heeft op de beste manier om tijdssynchronisatie configureren voor dit distributiepunt.

### <a name="integration-services"></a>Integratieservices

Controleer of de service voor integratie met (hv_utils) is geladen.

```bash
lsmod | grep hv_utils
```
U ziet er ongeveer als volgt uit:

```
hv_utils               24418  0
hv_vmbus              397185  7 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
```

Zie als de Hyper-V integration services-daemon wordt uitgevoerd.

```bash
ps -ef | grep hv
```

U ziet er ongeveer als volgt uit:

```
root        229      2  0 17:52 ?        00:00:00 [hv_vmbus_con]
root        391      2  0 17:52 ?        00:00:00 [hv_balloon]
```


### <a name="check-for-ptp"></a>PTP controleren

Met een nieuwere versie van Linux, bron clock precisie tijd Protocol (PTP) is beschikbaar als onderdeel van de provider VMICTimeSync. Bij oudere versies van Red Hat Enterprise Linux of CentOS 7.x de [Linux Integration Services](https://github.com/LIS/lis-next) kunnen worden gedownload en kan worden gebruikt om de bijgewerkte stuurprogramma te installeren. Wanneer u PTP, het Linux-apparaat is van het formulier/dev/ptp*x*. 

Zie welke bronnen van de klok PTP beschikbaar zijn.

```bash
ls /sys/class/ptp
```

In dit voorbeeld wordt de geretourneerde waarde is *ptp0*, zodat we die gebruiken om te controleren of de naam van de klok. Als u wilt controleren of het apparaat, Controleer de naam van de klok.

```bash
cat /sys/class/ptp/ptp0/clock_name
```

Dit moet retourneren **Hyper-v**.

### <a name="chrony"></a>chrony

Op Red Hat Enterprise Linux- en CentOS 7.x, [chrony](https://chrony.tuxfamily.org/) geconfigureerd voor het gebruik van een PTP bron klok. De daemon Network Time Protocol (ntpd) biedt geen ondersteuning voor PTP bronnen, met behulp van **chronyd** wordt aanbevolen. Bijwerken zodat PTP **chrony.conf**.

```bash
refclock PHC /dev/ptp0 poll 3 dpoll -2 offset 0
```

Zie voor meer informatie over Red Hat en NTP [configureren NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-configure_ntp). 

Zie voor meer informatie over chrony [met behulp van chrony](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-using_chrony).

Als zowel chrony en TimeSync bronnen tegelijk zijn ingeschakeld, kunt u markeren als één **liever** die de andere bron wordt ingesteld als een back-up. Omdat de klok voor de grote laat behalve na een lange periode, NTP services niet bijwerken, herstelt de VMICTimeSync de klok van gebeurtenissen in onderbroken virtuele machine veel sneller dan de NTP-gebaseerde hulpprogramma's alleen.


### <a name="systemd"></a>systemd 

Op Ubuntu en SUSE tijd synchroniseren is geconfigureerd met behulp van [systemd](https://www.freedesktop.org/wiki/Software/systemd/). Zie voor meer informatie over Ubuntu [tijdsynchronisatie](https://help.ubuntu.com/lts/serverguide/NTP.html). Zie voor meer informatie over SUSE sectie 4.5.8 in [SUSE Linux Enterprise Server 12 SP3 opmerkingen bij de Release](https://www.suse.com/releasenotes/x86_64/SUSE-SLES/12-SP3/#InfraPackArch.ArchIndependent.SystemsManagement).



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [nauwkeurige tijd voor Windows Server 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time).


