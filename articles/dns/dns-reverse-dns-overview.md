---
title: Overzicht van de omgekeerde DNS-server in Azure | Microsoft Docs
description: Meer informatie over hoe omgekeerde DNS werkt en hoe deze kan worden gebruikt in Azure
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: victorh
ms.openlocfilehash: fa3798a35804998936e0ac166fceff02b01231a0
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171507"
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Overzicht omgekeerde DNS-en ondersteuning in Azure

In dit artikel biedt een overzicht van hoe omgekeerde DNS werkt en de omgekeerde DNS-scenario's die worden ondersteund in Azure.

## <a name="what-is-reverse-dns"></a>Wat is omgekeerde DNS?

Conventionele DNS-records voor het inschakelen van een toewijzing van een DNS-naam (zoals 'www.contoso.com') naar een IP-adres (zoals 64.4.6.100).  Reverse-DNS kunt de vertaling van een IP-adres (64.4.6.100) terug naar een naam ('www.contoso.com').

Omgekeerde DNS-records worden gebruikt in diverse situaties. Omgekeerde DNS-records worden bijvoorbeeld veel gebruikt in de strijd tegen e-mailbericht door te controleren of de afzender van een e-mailbericht.  De ontvangende e-mailserver de omgekeerde DNS-record van de verzendende server IP-adres opgehaald en wordt gecontroleerd of die als host fungeren voor het verzenden van e-mail van het oorspronkelijke domein is gemachtigd. 

## <a name="how-reverse-dns-works"></a>Hoe omgekeerde DNS werkt

Omgekeerde DNS-records worden gehost in een speciale DNS-zones, ook wel 'ARPA-zones.  Deze zones vormen een afzonderlijke DNS-hiërarchie in combinatie met de normale hiërarchie die als host fungeert domeinen, zoals 'contoso.com'.

Bijvoorbeeld, wordt de DNS-record 'www.contoso.com' geïmplementeerd met behulp van een DNS-'A'-record met de naam 'www' in de zone 'contoso.com'.  Deze A-record verwijst naar het bijbehorende IP-adres, in dit geval 64.4.6.100.  De reverse lookup wordt afzonderlijk geïmplementeerd met behulp van een 'PTR-record met de naam '100' in de zone '6.4.64.in-addr.arpa' (Houd er rekening mee dat de IP-adressen in ARPA-zones worden omgekeerd.)  Deze PTR-record, verwijst als het goed is geconfigureerd naar de naam 'www.contoso.com'.

Wanneer een organisatie een IP-Adresblok toegewezen is, verkrijgen ze ook het recht voor het beheren van de bijbehorende ARPA-zone. De ARPA-zones die overeenkomt met de IP-adresblokken die worden gebruikt door Azure worden gehost en beheerd door Microsoft. Uw Internetprovider de ARPA-zone voor uw eigen IP-adressen voor u kan hosten of kunt u de ARPA-zone in een DNS-service van uw keuze, zoals Azure DNS hosten.

> [!NOTE]
> Forward DNS-zoekacties en omgekeerde DNS-zoekacties worden in afzonderlijke, parallelle DNS-hiërarchieën geïmplementeerd. De reverse lookup voor 'www.contoso.com' **niet** gehost in de zone 'contoso.com', in plaats daarvan wordt gehost in de ARPA-zone voor het bijbehorende IP-Adresblok. Afzonderlijke zones worden gebruikt voor IPv4 en IPv6-adresblokken.

### <a name="ipv4"></a>IPv4

De naam van een IPv4-zone voor reverse lookup moet zich in de volgende indeling: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Bijvoorbeeld bij het maken van een zone voor reverse op host-records voor hosts met IP-adressen die zich in het voorvoegsel 192.0.2.0/24, zouden de zonenaam worden gemaakt met het voorvoegsel van het adres (192.0.2) isoleren en omkeren van de order (2.0.192) en het achtervoegsel toetevoegen`.in-addr.arpa`.

|Klasse van subnet|-Voorvoegsel  |Omgekeerde-voorvoegsel  |Standard-achtervoegsel  |Zonenaam van voor reverse |
|-------|----------------|------------|-----------------|---------------------------|
|Klasse A|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|Klasse B|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|Klasse C|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Klasseloze IPv4-overdracht

In sommige gevallen, het IP-adresbereik dat is toegewezen aan een organisatie is kleiner dan een klasse C (/ 24) bereik. De IP-adresbereik valt in dit geval niet op een grens van een zone in de `.in-addr.arpa` zone van de hiërarchie en kan daarom niet als een onderliggende zone kan niet worden gedelegeerd.

In plaats daarvan wordt een ander mechanisme gebruikt voor het beheer van afzonderlijke reverse lookup (Pointer) records overbrengen naar een toegewezen DNS-zone. Dit mechanisme delegeert een onderliggende zone voor elk IP-adresbereik en vervolgens elk IP-adres binnen het bereik afzonderlijk toegewezen aan die onderliggende zone met behulp van CNAME-records.

Stel bijvoorbeeld dat een organisatie is het IP-bereik 192.0.2.128/26 verleend door de Internet-provider. Dit vertegenwoordigt 64 IP-adressen van 192.0.2.128 naar 192.0.2.191. Reverse-DNS voor dit bereik is als volgt uitgevoerd:
- De organisatie maakt u een zone voor reverse lookup 128-26.2.0.192.in-addr.arpa genoemd. Het voorvoegsel ' 128-26' vertegenwoordigt het netwerksegment dat is toegewezen aan de organisatie binnen de klasse C (/ 24) bereik.
- De provider maakt NS-records voor het instellen van de DNS-delegering voor de bovenstaande zone van de zone van de bovenliggende klasse C. Het maakt ook CNAME-records in de bovenliggende (klasse C) reverse lookup zone, elk IP-adres in de IP-adresbereik toe te wijzen aan de nieuwe zone is gemaakt door de organisatie:

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
- De organisatie beheert de afzonderlijke PTR-records in de onderliggende zone.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
Een reverse lookup voor de IP-adres '192.0.2.129'-query's voor een PTR-record met de naam '129.2.0.192.in-addr.arpa'. Deze query wordt omgezet via de CNAME in de bovenliggende zone naar de PTR-record in de onderliggende zone.

### <a name="ipv6"></a>IPv6

De naam van een IPv6-zone voor reverse lookup moet de volgende notatie: `<IPv6 network prefix in reverse order>.ip6.arpa`

Bijvoorbeeld:. Wanneer het maken van een zone voor reverse op host-records voor hosts met IP-adressen die zich in de 2001:db8:1000:abdc:: / 64 voorvoegsel, naam van de zone moet worden gemaakt door te isoleren van het voorvoegsel van het adres (2001:db8:abdc::). Vouw vervolgens de IPv6-voorvoegsel verwijderen [nul compressie](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), als deze is gebruikt om te verkorten van de IPv6-adresvoorvoegsel (2001:0db8:abdc:0000::). De volgorde, met een punt als scheidingsteken tussen elke hexadecimaal nummer in het voorvoegsel, om te maken van de omgekeerde-voorvoegsel (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) en het achtervoegsel toevoegen `.ip6.arpa`.


|-Voorvoegsel  |Uitgevouwen en omgekeerde-voorvoegsel |Standard-achtervoegsel |Zonenaam van voor reverse  |
|---------|---------|---------|---------|
|2001:db8:abdc:: / 64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | . ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102:: / 64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | . ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Azure-ondersteuning voor omgekeerde DNS

Azure ondersteunt twee afzonderlijke scenario's met betrekking tot de reverse-DNS:

**De zone voor reverse lookup overeenkomt met uw IP-Adresblok hosten.**
Azure DNS kunnen worden gebruikt om [uw zones voor reverse lookup hosten en beheren van de PTR-records voor elke reverse DNS-lookup](dns-reverse-dns-hosting.md), voor zowel IPv4 als IPv6.  Het proces voor het maken van de zone voor reverse lookup (ARPA), het instellen van de delegatie en het configureren van PTR-records is hetzelfde als voor gewone DNS-zones.  De enige verschillen zijn dat de overdracht via uw Internetprovider in plaats van uw DNS-registratieservice moet worden geconfigureerd en alleen de PTR-record type moet worden gebruikt.

**Configureer de omgekeerde DNS-record voor het IP-adres toegewezen aan uw Azure-service.** Azure kunt u [configureren van de reverse lookup voor de IP-adressen toegewezen aan uw Azure-service](dns-reverse-dns-for-azure-services.md).  Deze reverse lookup is geconfigureerd door Azure als een PTR-record in de bijbehorende ARPA-zone.  Deze ARPA-zones, overeenkomt met alle IP-adresbereiken die worden gebruikt door Azure worden gehost door Microsoft

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over omgekeerde DNS [omgekeerde DNS-zoekactie voor Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Meer informatie over het [host voor de zone voor reverse lookup voor uw Internet-provider toegewezen IP-adresbereik in Azure DNS](dns-reverse-dns-for-azure-services.md).
<br>
Meer informatie over het [omgekeerde DNS-records voor uw Azure-services beheren](dns-reverse-dns-for-azure-services.md).

