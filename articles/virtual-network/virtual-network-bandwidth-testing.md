---
title: Test virtuele machine van Azure netwerkdoorvoer | Microsoft Docs
description: Informatie over het testen van de netwerkdoorvoer van de virtuele machine van Azure.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 490db57e9f36bed5575b7af5a6e2673fb63af3a8
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bandbreedte/doorvoer (NTTTCP) testen

Bij het testen van prestaties van de netwerkdoorvoer in Azure, is het raadzaam te gebruiken van een hulpprogramma dat gericht is op het netwerk voor het testen en het gebruik van andere bronnen die kan invloed hebben op prestaties geminimaliseerd. NTTTCP wordt aanbevolen.

Kopieer het hulpprogramma naar twee Azure VM's van dezelfde grootte hebben. Een virtuele machine fungeert als de afzender en de andere als ontvanger.

#### <a name="deploying-vms-for-testing"></a>Implementeren van virtuele machines voor testdoeleinden
Voor de doeleinden van deze test moet de twee virtuele machines in dezelfde Cloud Service of in dezelfde Beschikbaarheidsset zodat we kunnen hun interne IP-adressen gebruiken en de Load Balancers van de test uitsluiten. Het is mogelijk om te testen met het VIP, maar dit soort testen is buiten het bereik van dit document.
 
Maak een notitie van IP-adres van de ontvanger. We bellen die IP 'a.b.c.r'

Noteer het aantal kernen op de virtuele machine. Laten we dit noemen '\#num\_kernen '
 
Voert de test NTTTCP voor 300 seconden (of 5 minuten) op de VM-verzender en ontvanger VM.

Tip: Als deze test voor de eerste keer instelt, probeer u mogelijk een kortere testperiode om feedback sneller. Zodra de tool werkt zoals verwacht, breid u de testperiode aan 300 seconden voor de meest nauwkeurige resultaten.

> [!NOTE]
> De afzender **en** ontvanger moet opgeven **dezelfde** testen duur van de parameter (-t).

Voor het testen van één TCP-stroom voor 10 seconden:

Ontvanger parameters: ntttcp - r -t 10 - P 1

Parameters van de afzender: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1

> [!NOTE]
> Het vorige voorbeeld mag alleen worden gebruikt om te bevestigen dat de configuratie. Aantal voorbeelden van testen worden verderop in dit document besproken.

## <a name="testing-vms-running-windows"></a>Testen van virtuele machines waarop WINDOWS wordt uitgevoerd:

#### <a name="get-ntttcp-onto-the-vms"></a>NTTTCP op de virtuele machines worden opgehaald.

Download de nieuwste versie: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Of zoekt u het als verplaatst: <https://www.bing.com/search?q=ntttcp+download> \< --moet eerst worden bereikt

Overweeg NTTTCP plaatsen in een afzonderlijke map, zoals c:\\hulpprogramma's

#### <a name="allow-ntttcp-through-the-windows-firewall"></a>NTTTCP toestaan via de Windows-firewall
Maak een regel voor toestaan op de Windows Firewall zodat het verkeer NTTTCP moet worden uitgevoerd op de ontvanger. Het gemakkelijkst om toe te staan van het volledige NTTTCP programma met de naam in plaats van dat specifieke TCP-poorten inkomend.

Toestaan dat ntttcp via de Windows Firewall als volgt:

Netsh advfirewall firewall regel programma toevoegen =\<pad\>\\ntttcp.exe name = "ntttcp" protocol eventuele dir = in actie = = toestaan inschakelen = yes profile = ANY

Bijvoorbeeld, als u hebt gekopieerd ntttcp.exe aan de ' c:\\extra ' map, zou dit de opdracht: 

Netsh advfirewall firewall regel programma toevoegen = c:\\hulpprogramma's\\ntttcp.exe naam = 'ntttcp' protocol eventuele dir = in actie = = toestaan inschakelen = yes profile = ANY

#### <a name="running-ntttcp-tests"></a>NTTTCP tests die worden uitgevoerd

NTTTCP starten op de ontvanger (**uitvoeren vanaf CMD**, niet vanuit PowerShell):

ntttcp - r-m [2\*\#num\_kernen],\*, 300 a.b.c.r -t

Als de virtuele machine vier kernen en het IP-adres 10.0.0.4 heeft, wordt deze eruit als volgt:

ntttcp - r – m 8,\*, 10.0.0.4 -t 300


NTTTCP starten op de afzender (**uitvoeren vanaf CMD**, niet vanuit PowerShell):

ntttcp -s-m 8,\*, 10.0.0.4 -t 300 

Wacht totdat de resultaten.


## <a name="testing-vms-running-linux"></a>Testen van virtuele machines waarop LINUX wordt uitgevoerd:

Gebruik nttcp voor linux. Deze beschikbaar is via <https://github.com/Microsoft/ntttcp-for-linux>

Op de virtuele Linux-machines (ZENDER en ontvanger), moet u deze opdrachten om voor te bereiden ntttcp voor linux op uw virtuele machines uitvoeren:

CentOS - Git installeren:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu - Git installeren:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Maken en te installeren op beide:
``` bash
 git clone https://github.com/Microsoft/ntttcp-for-linux
 cd ntttcp-for-linux/src
 make && make install
```

Zoals in het Windows-voorbeeld gaan we ervan uit dat IP-adres van de Linux-ontvanger is 10.0.0.4

Start NTTTCP voor Linux op de ontvanger:

``` bash
ntttcp -r -t 300
```

En voer op de afzender:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Test lengte van de standaardwaarde is 60 seconden als er is geen tijdsparameter is gegeven

## <a name="testing-between-vms-running-windows-and-linux"></a>Testen tussen VM's met Windows en LINUX:

Op deze scenario's moeten we de Nee-sync-modus inschakelen zodat de test kunt uitvoeren. Dit wordt gedaan met behulp van de **-N vlag** voor Linux en **-ns vlag** voor Windows.

#### <a name="from-linux-to-windows"></a>Van Linux bij Windows:

Ontvanger <Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Afzender <Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a>Van Windows naar Linux:

Ontvanger <Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Afzender <Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a>Volgende stappen
* Afhankelijk van de resultaten, kunnen er ruimte is om te [netwerk doorvoer machines optimaliseren](virtual-network-optimize-network-bandwidth.md) voor uw scenario.
* Meer informatie over het [bandbreedte is toegewezen aan virtuele machines] (virtuele-machine-netwerk-throughput.md)
* Klik hier als u meer wilt weten met [Azure Virtual Network Veelgestelde vragen (FAQ)](virtual-networks-faq.md)
