---
title: Tijd synchronisatie voor Windows-VM's in Azure | Microsoft Docs
description: Tijdsynchronisatie voor Windows-machines.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: d7363c3d1cd3aaf6aae8cadbea232c909000f214
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669599"
---
# <a name="time-sync-for-windows-vms-in-azure"></a>Tijdsynchronisatie voor Windows-VM's in Azure

Tijdsynchronisatie is belangrijk voor beveiliging en correlatie tussen gebeurtenissen. Soms wordt gebruikt voor de uitvoering van gedistribueerde transacties. De nauwkeurigheid van de tijd tussen meerdere computersystemen wordt bereikt via synchronisatie. Synchronisatie kan worden beïnvloed door meerdere factoren, met inbegrip van verkeer tussen de tijdbronnen en de computer die de tijd ophalen en opnieuw wordt opgestart. 

Azure wordt nu ondersteund door de infrastructuur met Windows Server 2016. Windows Server 2016 bevat verbeterde algoritmen gebruikt voor het juiste tijd en de lokale klok te synchroniseren met de UTC-voorwaarde.  Windows Server 2016 wordt ook de VMICTimeSync-service die bepaalt hoe virtuele machines worden gesynchroniseerd met de host voor nauwkeurige tijd verbeterd. Verbeteringen zijn onder meer nauwkeurige initiële tijd op de virtuele machine starten of herstellen van de virtuele machine en interrupt latentie correctie voorbeelden die op de Windows-tijd (W32time). 


>[!NOTE]
>Voor een snel overzicht van Windows Time-service, Bekijk dit [overzicht op hoog niveau video](https://aka.ms/WS2016TimeVideo).
>
> Zie voor meer informatie, [nauwkeurige tijd voor Windows Server 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Overzicht

Nauwkeurigheid van de klok van een computer is af te meten hoe dicht de klok van de computer is de standaard van de tijd Coordinated Universal Time (UTC). UTC wordt gedefinieerd door een multinationale voorbeeld van nauwkeurige atomic klokken die alleen uitgeschakeld door een tweede 300 jaren worden kan. Maar rechtstreeks lezen van UTC speciale hardware vereist. In plaats daarvan tijdservers worden gesynchroniseerd naar UTC en toegankelijk zijn vanaf andere computers om schaalbaarheid en stabiliteit te bieden. Elke computer heeft time-synchronisatieservice uitvoert die weet welke tijdservers u te gebruiken en controleert regelmatig of als de computerklok moet worden gecorrigeerd en past de tijd indien nodig. 

Azure-hosts worden gesynchroniseerd met de interne Microsoft-tijdservers die hun tijd van Stratum 1 eigendom van Microsoft-apparaten, met GPS antennes in beslag nemen. Virtuele machines in Azure kunnen ofwel afhankelijk zijn van de host om door te geven van de nauwkeurige tijd (*tijd host*) u aan bij de virtuele machine of de virtuele machine rechtstreeks krijgt tijd vanuit een tijdserver of een combinatie van beide. 

Interactie van de virtuele machine met de host kunnen ook invloed op de klok. Tijdens de [geheugen onderhoud met statusbehoud](maintenance-and-updates.md#memory-preserving-maintenance), virtuele machines zijn onderbroken voor maximaal 30 seconden. Bijvoorbeeld voordat onderhoud begint de klok van de virtuele machine bevat 10:00:00 uur en 28 seconden duurt. Nadat de virtuele machine wordt hervat, de klok op de virtuele machine nog steeds 10:00:00 uur, wat 28 seconden weergeven uit. Op juiste hiervoor de VMICTimeSync service bewaakt wat er gebeurt op de host en wordt u gevraagd om wijzigingen op de virtuele machines om te compenseren.

De service VMICTimeSync in voorbeeld of sync modus uitgevoerd en alleen van invloed zijn op de klok doorsturen. In de voorbeeld-modus, waarvoor is vereist W32time worden uitgevoerd, de service VMICTimeSync pollt de host om de vijf seconden en biedt W32time time-voorbeelden. Ongeveer elke 30 seconden, de W32time-service heeft de meest recente tijd steekproef en gebruikt voor het invloed hebben op de klok van de Gast. Synchronisatiemodus wordt geactiveerd als een Gast is hervat of als de klok van een gast drifts meer dan 5 seconden achter de klok van de host. Het laatste geval mag niet voorkomen in gevallen waar de W32time-service juist wordt uitgevoerd.

Zonder tijd synchronisatie werkt, zou de klok op de virtuele machine fouten oplopen. Wanneer er slechts één VM, het effect mogelijk niet significant, tenzij de werkbelasting is zeer nauwkeurige vereist. Maar in de meeste gevallen we hebben meerdere, met elkaar verbonden virtuele machines die gebruikmaken van de tijd voor het bijhouden van transacties en wat de behoeften van de tijd consistent is gedurende de volledige implementatie. Wanneer de tijd tussen virtuele machines anders is, ziet u kan de volgende gevolgen:

- Verificatie mislukt. Beveiligingsprotocollen, zoals Kerberos of technologie afhankelijk zijn van het certificaat zijn afhankelijk van de tijd consistent wordt in de systemen. 
- Het is moeilijk te achterhalen wat zich in een systeem hebben voorgedaan als Logboeken (of andere gegevens) niet op tijd akkoord gaat. Dezelfde gebeurtenis wilt bekijken, zoals het op verschillende tijdstippen, maken de correlatie moeilijk is opgetreden.
- Als de klok is uitgeschakeld, kan de facturering onjuist worden berekend.

De beste resultaten voor Windows-implementaties worden bereikt met behulp van Windows Server 2016 als het gastbesturingssysteem, waardoor dat u kunt de laatste verbeteringen in tijdsynchronisatie.

## <a name="configuration-options"></a>Configuratie-opties

Er zijn drie opties voor het configureren van tijdsynchronisatie voor uw Windows-VM's die worden gehost in Azure:

- Host en time.windows.com. Dit is de standaardconfiguratie gebruikt in Azure Marketplace-installatiekopieën.
- Host alleen-lezen.
- Gebruik een andere, externe tijdserver met of zonder met behulp van de tijd van de host.


### <a name="use-the-default"></a>De standaardwaarde gebruiken

Windows-besturingssysteem VM-installatiekopieën zijn standaard geconfigureerd voor w32time om te synchroniseren van twee bronnen: 

- De provider NtpClient die informatie wordt verkregen uit time.windows.com.
- De service VMICTimeSync, die wordt gebruikt om te communiceren, de tijd van de host aan de virtuele machines en correcties nadat de virtuele machine is onderbroken voor onderhoud. Apparaten die eigendom Stratum 1 Azure hosts gebruiken om nauwkeurige tijd.

W32Time liever wilt dat de time-provider in de volgende volgorde van prioriteit: stratum niveau, hoofdmap vertraging, spreiding van de hoofdmap, time-offset. In de meeste gevallen liever w32time time.windows.com naar de host omdat time.windows.com lagere stratum rapporteert. 

Voor machines die worden toegevoegd aan een domein het domein zelf wordt tot stand gebracht tijd synchronisatie-hiërarchie, maar de hoofdforest nog steeds moet rekening houden met tijd ergens en de volgende overwegingen zou nog steeds houdt u de waarde true.


### <a name="host-only"></a>Alleen-host 

Omdat time.windows.com is een openbare NTP-server, tijd synchroniseren, moet u verkeer verzenden via internet, verschillende pakket vertragingen kunnen de kwaliteit van de tijdsynchronisatie negatief beïnvloeden. Verwijderen van time.windows.com door over te schakelen om te synchroniseren met alleen-host, kan uw tijd soms verbeteren resultaten synchroniseren.

Overschakelen naar alleen-host tijd synchronisatie kan zinvol als u tijdsynchronisatie ondervindt problemen met behulp van de standaardconfiguratie. Probeer nu de alleen-host synchronisatie om te zien of er die de tijdsynchronisatie op virtuele machine wilt verbeteren. 

### <a name="external-time-server"></a>Externe tijdserver

Als u specifieke tijd synchronisatie vereisten hebt, is er ook een optie van het gebruik van externe tijdservers. Externe tijdservers kunnen specifieke tijd nuttig voor Testscenario's zijn kan, ervoor te zorgen dat de tijd uniformiteit met machines die worden gehost in niet-Microsoft-datacenters of verwerking leap seconden in een speciale manier bieden.

U kunt externe servers combineren met de VMICTimeSync service en VMICTimeProvider om resultaten die vergelijkbaar is met de standaardconfiguratie te bieden. 

## <a name="check-your-configuration"></a>Controleer de configuratie


Controleer of de provider van de tijd NtpClient is geconfigureerd voor het gebruik van expliciete NTP-servers (NAP) of tijdsynchronisatie domein (NT5DS).

```
w32tm /dumpreg /subkey:Parameters | findstr /i "type"
```

Als de virtuele machine van NTP gebruikmaakt, ziet u de volgende uitvoer:

```
Value Name                 Value Type          Value Data
Type                       REG_SZ              NTP
```

Om te zien welke tijdserver de tijdprovider NtpClient gebruikt, bij een opdrachtprompt met verhoogde bevoegdheid type:

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

Als de virtuele machine standaard gebruikt wordt, wordt de uitvoer ziet er als volgt:

```
NtpServer                  REG_SZ              time.windows.com,0x8
```


Om te zien welke tijdprovider momenteel wordt gebruikt.

```
w32tm /query /source
```


Hier volgt de uitvoer die u kunt zien en wat het betekent:
    
- **Time.Windows.com** -in de standaardconfiguratie w32time time van time.windows.com zou krijgen. De tijd synchronisatie-kwaliteit is afhankelijk van de verbinding met internet toe en wordt beïnvloed door pakket vertragingen. Dit is de gebruikelijke uitvoer van de standaardinstellingen.
- **Virtuele machine IC-tijdprovider synchronisatie** -tijd van de host wordt gesynchroniseerd door de virtuele machine. Dit is meestal het resultaat als u zich aanmelden voor alleen-host tijdsynchronisatie of de NtpServer op dit moment niet beschikbaar is. 
- *Uw domeinserver* : de huidige computer zich in een domein en het domein definieert de tijd synchronisatie-hiërarchie.
- *Een andere server* -w32time expliciet is geconfigureerd voor het ophalen van de tijd van die een andere server. Tijd synchronisatie-kwaliteit is afhankelijk van deze server tijd kwaliteit.
- **Lokale CMOS-klok** -klok is gesynchroniseerd. U kunt deze uitvoer als w32time nog niet over voldoende tijd om te starten na opnieuw opstarten of beschikt wanneer alle de geconfigureerde time-bronnen niet beschikbaar zijn.


## <a name="opt-in-for-host-only-time-sync"></a>Aanmelden voor alleen-host tijdsynchronisatie

Azure wordt voortdurend gewerkt aan het verbeteren van tijdsynchronisatie op hosts en kan garanderen dat de infrastructuur voor de synchronisatie van tijd worden samengevoegd in eigendom van Microsoft-datacenters. Als u problemen met de standaardinstellingen die de voorkeur geniet time.windows.com gebruiken als de primaire tijdbronnen synchroniseren tijd hebt, kunt u de volgende opdrachten voor aanmelden voor alleen-host tijdsynchronisatie.

De provider VMIC markeren als ingeschakeld. 

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\VMICTimeProvider /v Enabled /t REG_DWORD /d 1 /f
```

De provider NTPClient markeren als uitgeschakeld.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f
```

De w32time-Service opnieuw starten.

```
net stop w32time && net start w32time
```


## <a name="windows-server-2012-and-r2-vms"></a>WindowsServer 2012 en R2 VM 's 

Windows Server 2012 en Windows Server 2012 R2 hebben verschillende instellingen voor tijdsynchronisatie. De w32time standaard is geconfigureerd op een manier die weinig overhead van de service de voorkeur boven op de precieze tijd krijgt. 

Als u verplaatsen van uw Windows Server 2012 en 2012 R2 implementaties wilt naar de nieuwere standaardwaarden die liever nauwkeurige tijd, kunt u de volgende instellingen toepassen.

Werk de w32time-poll en update-intervallen om instellingen voor Windows Server 2016.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MinPollInterval /t REG_DWORD /d 6 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MaxPollInterval /t REG_DWORD /d 10 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UpdateInterval /t REG_DWORD /d 100 /f
w32tm /config /update
```

Voor w32time willen gebruiken de nieuwe poll-interval, de NtpServers worden gemarkeerd als het gebruik ervan. Als de servers zijn gemarkeerd met 0x1 bitvlaggen masker, die dit mechanisme wilt overschrijven en w32time in plaats daarvan SpecialPollInterval zou gebruiken. Zorg ervoor dat opgegeven NTP-servers zijn met behulp van 0x8 vlag of er geen vlag helemaal:

Controleer welke vlaggen worden gebruikt voor de gebruikte NTP-servers.

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

## <a name="next-steps"></a>Volgende stappen

Hieronder vindt u koppelingen naar meer informatie over de tijdsynchronisatie:

- [Windows Time-Service-hulpprogramma's en instellingen](https://docs.microsoft.com/windows-server/networking/windows-time-service/Windows-Time-Service-Tools-and-Settings)
- [Verbeteringen in Windows Server 2016 ](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-server-2016-improvements)
- [Nauwkeurige tijd voor WindowsServer 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)
- [Ondersteuning grens voor het configureren van de Windows Time-service voor hoge nauwkeurigheid omgevingen](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary)


