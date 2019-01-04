---
title: Netwerkdoorvoer van virtuele Azure-machine testen
titlesuffix: Azure Virtual Network
description: Informatie over het testen van de netwerkdoorvoer van virtuele machine van Azure.
services: virtual-network
documentationcenter: na
author: steveesp
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: b02f5011b9482481ca3fbe439061c160b3c73a1e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016610"
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bandbreedte/doorvoer testen (NTTTCP)

Wanneer u test de prestaties van de netwerkdoorvoer in Azure, is het raadzaam te gebruiken van een hulpprogramma dat is gericht op het netwerk voor het testen en minimaliseert het gebruik van andere bronnen die invloed kunnen hebben op prestaties. NTTTCP wordt aanbevolen.

Het hulpprogramma voor kopiëren naar twee Azure-VM's van dezelfde grootte. Een virtuele machine fungeert als de afzender en de andere als ontvanger.

#### <a name="deploying-vms-for-testing"></a>Implementatie van VM's voor het testen
Voor de toepassing van deze test moet de twee virtuele machines in dezelfde Cloudservice of in dezelfde Beschikbaarheidsset zodat we kunnen hun interne IP-adressen gebruiken en de Load Balancers van de test uitsluiten. Het is mogelijk om te testen met de VIP-adres, maar dit soort testen is buiten het bereik van dit document.

Noteer de IP-adres van de ontvanger. Noemen we dat IP-adres "a.b.c.r"

Noteer het aantal kernen op de virtuele machine. Noemen we dit "\#num\_kernen"

De test NTTTCP voor 300 seconden (of 5 minuten) worden uitgevoerd op de afzender VM en de ontvanger VM.

Tip: Bij het instellen van deze test voor de eerste keer, kunt u een kortere testperiode om feedback sneller wilt proberen. Nadat het hulpprogramma werkt zoals verwacht, breid de testperiode aan 300 seconden voor de meest nauwkeurige resultaten.

> [!NOTE]
> De afzender **en** ontvanger moet opgeven **hetzelfde** duur van de parameter testen (-t).

Voor het testen van één TCP-stroom voor 10 seconden:

Ontvanger parameters: ntttcp - r -t 10 - P 1

Afzender parameters: ntttcp-s10.27.33.7 -t 10 - n 1 P - 1

> [!NOTE]
> Het voorgaande voorbeeld mag alleen worden gebruikt om uw configuratie te bevestigen. Geldige voorbeelden van testen worden verderop in dit document besproken.

## <a name="testing-vms-running-windows"></a>Testen van virtuele machines waarop WINDOWS wordt uitgevoerd:

#### <a name="get-ntttcp-onto-the-vms"></a>Get-NTTTCP naar de virtuele machines.

Download de nieuwste versie: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Of zoek het als verplaatst: <https://www.bing.com/search?q=ntttcp+download> \< --moet eerst worden bereikt

Houd rekening met NTTTCP plaatsen in een afzonderlijke map, zoals c:\\hulpprogramma's

#### <a name="allow-ntttcp-through-the-windows-firewall"></a>NTTTCP toestaan via de Windows-firewall
Maak een regel voor toestaan op de Windows Firewall zodat het verkeer NTTTCP aankomt op de ontvanger. Het gemakkelijkst om toe te staan van de gehele NTTTCP programma met de naam in plaats van dat specifieke TCP-poorten inkomend.

Toestaan dat ntttcp via de Windows Firewall als volgt:

Netsh advfirewall firewall regel programma toevoegen =\<pad\>\\ntttcp.exe naam = "ntttcp" protocol = alle dir = in actie = inschakelen toestaan = yes profile = ANY

Bijvoorbeeld, als u hebt gekopieerd ntttcp.exe naar de "c:\\extra" map, zou dit de opdracht: 

Netsh advfirewall firewall regel programma toevoegen = c:\\hulpprogramma's\\ntttcp.exe naam = "ntttcp" protocol = alle dir = in actie = inschakelen toestaan = yes profile = ANY

#### <a name="running-ntttcp-tests"></a>Actieve NTTTCP tests

NTTTCP starten op de ontvanger (**uitvoeren vanaf CMD**, niet vanuit PowerShell):

ntttcp - r – m [2\*\#num\_kernen],\*, 300 a.b.c.r -t

Als de virtuele machine vier kernen en het IP-adres 10.0.0.4 heeft, zou deze er als volgt:

ntttcp - r – m 8,\*, 10.0.0.4 -t 300


NTTTCP starten op de afzender (**uitvoeren vanaf CMD**, niet vanuit PowerShell):

ntttcp -s – m 8,\*, 10.0.0.4 -t 300 

Wacht tot de resultaten.


## <a name="testing-vms-running-linux"></a>Testen van virtuele machines waarop LINUX wordt uitgevoerd:

Gebruik nttcp voor linux. Het is beschikbaar via <https://github.com/Microsoft/ntttcp-for-linux>

Voer deze opdrachten om voor te bereiden ntttcp voor linux op uw virtuele machines op de virtuele Linux-machines (afzender en ontvanger):

CentOS - Installeer Git:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu - Installeer Git:
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

Zoals in het Windows-voorbeeld gaan we ervan uit dat van de ontvanger van de Linux-IP-adres 10.0.0.4 is

Start NTTTCP voor Linux op de ontvanger:

``` bash
ntttcp -r -t 300
```

En op de afzender, worden uitgevoerd:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Test lengte standaard ingesteld op 60 seconden als u geen tijdsparameter is opgegeven

## <a name="testing-between-vms-running-windows-and-linux"></a>Testen tussen virtuele machines met Windows en LINUX:

Op deze scenario's moet we het nosync-modus inschakelen, zodat de test kunt uitvoeren. Dit wordt gedaan met behulp van de **-N vlag** voor Linux en **-ns vlag** voor Windows.

#### <a name="from-linux-to-windows"></a>Van Linux en Windows:

Ontvanger <Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Afzender <Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a>Van Windows tot Linux:

Ontvanger <Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Afzender <Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```
## <a name="testing-cloud-service-instances"></a>Testen van Cloud Service-exemplaren:
U moet toevoegen de volgende sectie in uw ServiceDefinition.csdef
```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" />
</Endpoints> 
```

## <a name="next-steps"></a>Volgende stappen
* Afhankelijk van resultaten, kunnen er ruimte [optimaliseren van doorvoer netwerkmachines](virtual-network-optimize-network-bandwidth.md) voor uw scenario.
* Meer informatie over hoe u [bandbreedte is toegewezen aan virtuele machines](virtual-machine-network-throughput.md)
* Leer meer met [Azure Virtual Network Veelgestelde vragen (FAQ)](virtual-networks-faq.md)
