---
title: Overzicht van de reverse DNS-server in Azure | Microsoft Docs
description: Meer informatie over hoe reverse DNS-werkt en hoe deze kan worden gebruikt in Azure
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 70a1ad070e812951fca3d2b19da12c67f0725dd0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Overzicht van de reverse DNS- en biedt ondersteuning in Azure

In dit artikel biedt een overzicht van hoe reverse DNS-werkt en de reverse DNS-scenario's ondersteund in Azure.

## <a name="what-is-reverse-dns"></a>Wat is omgekeerde DNS?

Conventionele DNS-records voor het inschakelen van een toewijzing van een DNS-naam (zoals 'www.contoso.com') naar een IP-adres (zoals 64.4.6.100).  Omgekeerde DNS kunt de vertaling van een IP-adres (64.4.6.100) terug naar een naam (www.contoso.com).

Omgekeerde DNS-records worden gebruikt in verschillende situaties. Omgekeerde DNS-records zijn bijvoorbeeld veel gebruikt in de bestrijding van e-mailbericht door te controleren of de afzender van een e-mailbericht.  De ontvangende e-mailserver haalt de reverse DNS-record van de verzendende server IP-adres en wordt gecontroleerd of die host is gemachtigd om e-mail te verzenden van het oorspronkelijke domein. 

## <a name="how-reverse-dns-works"></a>Hoe reverse DNS-werkt

Omgekeerde DNS-records worden gehost in speciale DNS-zones, bekend als 'ARPA-zones.  Deze zones vormen een afzonderlijke DNS-hiërarchie in combinatie met de normale hiërarchie die als host fungeert voor domeinen, zoals 'contoso.com'.

Bijvoorbeeld, wordt de DNS-record 'www.contoso.com' geïmplementeerd met behulp van een DNS-'A'-record met de naam 'www' in de zone contoso.com.  Deze A-record verwijst naar de bijbehorende IP-adres, in dit geval 64.4.6.100.  De reverse lookup wordt afzonderlijk geïmplementeerd met een 'PTR-record met de naam '100' in de zone '6.4.64.in-addr.arpa' (Let erop dat de IP-adressen in ARPA-zones worden omgekeerd.)  Deze PTR-record, verwijst als deze juist is geconfigureerd naar de naam www.contoso.com.

Wanneer een organisatie een IP-Adresblok toegewezen is, aanschaffen ze ook het recht voor het beheren van de bijbehorende ARPA zone. De ARPA-zones die overeenkomt met de IP-adresblokken gebruikt door Azure worden gehost en beheerd door Microsoft. Uw Internetprovider kan host voor de zone ARPA voor uw eigen IP-adressen voor u mogelijk toe te staan of te hosten van de zone ARPA in een DNS-service van uw keuze, zoals Azure DNS.

> [!NOTE]
> Forward DNS-zoekacties en reverse DNS-zoekacties worden in afzonderlijke, parallelle DNS-hiërarchieën geïmplementeerd. De reverse lookup voor 'www.contoso.com' **niet** gehost in de zone 'contoso.com', in plaats daarvan deze is opgenomen in de zone ARPA voor de bijbehorende IP-Adresblok. Afzonderlijke zones worden gebruikt voor IPv4 en IPv6-adresblokken.

### <a name="ipv4"></a>IPv4

De naam van een IPv4-zone voor reverse lookup moet in de volgende indeling: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Bijvoorbeeld bij het maken van een reverse zone voor hostrecords voor hosts met IP-adressen die zich in het voorvoegsel 192.0.2.0/24, zouden de zonenaam worden gemaakt met het voorvoegsel van het netwerk van het adres (192.0.2) te isoleren en omkeren, volgorde (2.0.192) en het achtervoegsel toevoegen`.in-addr.arpa`.

|Subnet-klasse|Netwerk voorvoegsel  |Omgekeerde netwerk voorvoegsel  |Standaard-achtervoegsel  |Zonenaam van voor reverse |
|-------|----------------|------------|-----------------|---------------------------|
|Klasse A|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|Klasse B|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|Klasse C|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Een IPv4-overdracht

In sommige gevallen kan het IP-adresbereik dat is toegewezen aan een organisatie kleiner is dan een klasse C (/ 24) bereik. Het IP-bereik valt in dit geval niet op de grens van een zone binnen de `.in-addr.arpa` zone hiërarchie en daarom kan niet worden overgedragen als een onderliggende zone.

In plaats daarvan wordt een ander mechanisme gebruikt voor het beheer van record voor afzonderlijke reverse lookup (Pointer) overbrengen naar een specifieke DNS-zone. Dit mechanisme delegeert een onderliggende zone voor elk IP-adresbereik en vervolgens elk IP-adres in het bereik afzonderlijk wordt toegewezen aan die onderliggende zone met behulp van CNAME-records.

Stel bijvoorbeeld dat een organisatie is het IP-bereik 192.0.2.128/26 verleend door de Internetprovider. Dit vertegenwoordigt 64 IP-adressen van 192.0.2.128 naar 192.0.2.191. Omgekeerde DNS voor dit bereik wordt als volgt geïmplementeerd:
- De organisatie maakt aangeroepen 128-26.2.0.192.in-addr.arpa-zone voor reverse lookup. Het voorvoegsel ' 128-26' vertegenwoordigt het netwerksegment dat wordt toegewezen aan de organisatie binnen de klasse C (/ 24) bereik.
- De Internetprovider maakt NS-records voor het instellen van de DNS-delegering voor de bovenstaande zone van de bovenliggende klasse C zone. Dit leidt ook tot CNAME-records in de bovenliggende (klasse C) reverse lookup zone, elk IP-adres in de IP-adresbereik toewijzen aan de nieuwe zone gemaakt door de organisatie:

```
$ORIGIN 2.0.192.in-addr.arpa
; Delegate child zone
128-26    NS       <name server 1 for 128-26.2.0.192.in-addr.arpa>
128-26    NS       <name server 2 for 128-26.2.0.192.in-addr.arpa>
; CNAME records for each IP address
129       CNAME    129.128-26.2.0.192.in-addr.arpa
130       CNAME    130.128-26.2.0.192.in-addr.arpa
131       CNAME    131.128-26.2.0.192.in-addr.arpa
; etc
```
- De organisatie beheert de afzonderlijke PTR-records binnen hun onderliggende zone.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
Een reverse lookup voor de IP-adres '192.0.2.129' query's voor een PTR-record met de naam '129.2.0.192.in-addr.arpa'. Deze query wordt omgezet via de CNAME in de bovenliggende zone naar de PTR-record in de onderliggende zone.

### <a name="ipv6"></a>IPv6

De naam van een IPv6-zone voor reverse lookup worden de volgende notatie:`<IPv6 network prefix in reverse order>.ip6.arpa`

Bijvoorbeeld:. Wanneer maken van een reverse zone voor hostrecords voor hosts met IP-adressen die zich in de 2001:db8:1000:abdc:: / 64-voorvoegsel, de zonenaam zouden worden gemaakt met het voorvoegsel van het netwerk van het adres te isoleren (2001:db8:abdc::). Vouw vervolgens het IPv6-netwerk voorvoegsel verwijderen [nul compressie](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), als deze is gebruikt om te verkorten van de IPv6-adresvoorvoegsel (2001:0db8:abdc:0000::). De volgorde, met een punt als scheidingsteken tussen elke hexadecimaal getal in het voorvoegsel voor het bouwen van de omgekeerde netwerk voorvoegsel (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) en voeg het achtervoegsel `.ip6.arpa`.


|Netwerk voorvoegsel  |Uitgebreide en omgekeerde netwerk voorvoegsel |Standaard-achtervoegsel |Zonenaam van voor reverse  |
|---------|---------|---------|---------|
|2001:db8:abdc:: / 64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | . ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102:: / 64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | . ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Ondersteuning van Azure voor omgekeerde DNS

Azure ondersteunt twee afzonderlijke scenario's met betrekking tot de reverse DNS:

**De zone voor reverse lookup overeenkomt met uw IP-Adresblok hosten.**
Azure DNS kan worden gebruikt voor het [hosten van uw zones voor reverse lookup en PTR-records voor elke reverse DNS-lookup beheren](dns-reverse-dns-hosting.md), voor zowel IPv4 als IPv6.  Het proces van het maken van de zone voor reverse lookup (ARPA) en PTR-records configureren instellen van de overdracht is hetzelfde als voor gewone DNS-zones.  De enige verschillen zijn dat de overdracht moet worden geconfigureerd via uw Internetprovider in plaats van uw registrar DNS- en alleen het PTR-record type moet worden gebruikt.

**Configureer de reverse DNS-record voor de IP-adres dat is toegewezen aan uw Azure-service.** U kunt Azure [de reverse lookup voor de IP-adressen toegewezen aan uw Azure-service configureren](dns-reverse-dns-for-azure-services.md).  Deze reverse lookup is geconfigureerd door Azure als een PTR-record in de bijbehorende ARPA-zone.  Deze ARPA-zones, overeenkomt met alle IP-adresbereiken die worden gebruikt door Azure worden gehost door Microsoft

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over omgekeerde DNS [achterwaartse DNS-zoekopdracht op Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Meer informatie over hoe [zone voor reverse lookup voor uw Internetprovider toegewezen IP-adresbereik in Azure DNS hosten](dns-reverse-dns-for-azure-services.md).
<br>
Meer informatie over hoe [omgekeerde DNS-records voor uw Azure-services beheren](dns-reverse-dns-for-azure-services.md).

