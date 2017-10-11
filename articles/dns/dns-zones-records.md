---
title: DNS-Zones en Records overzicht - Azure DNS | Microsoft Docs
description: Overzicht van ondersteuning voor hosting van DNS-zones en -records in Microsoft Azure DNS.
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: jonatul
ms.openlocfilehash: 5818986c939c464a364c52ab31225e15130ab30e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-dns-zones-and-records"></a>Overzicht van DNS-zones en -records

Deze pagina wordt uitgelegd dat de belangrijkste concepten van domeinen, DNS-zones, DNS-records en recordsets en hoe ze worden ondersteund in Azure DNS.

## <a name="domain-names"></a>Domeinnamen

Het Domain Name System is een hiërarchie van domeinen. De hiërarchie start vanaf het hoofddomein. De naam van dit domein is eenvoudigweg '**.**'.  Hieronder komen de topleveldomeinen, zoals com, net, org, uk of nl.  Onder de topleveldomeinen komen de secondleveldomeinen, zoals org.uk of co.jp. De domeinen in de DNS-hiërarchie worden globaal gedistribueerd en gehost door DNS-naamservers over de hele wereld.

Een domeinnaamregistrar is een organisatie waarmee u een domeinnaam, bijvoorbeeld 'contoso.com' kopen.  Het aanschaffen van een domeinnaam, geeft u het recht om te bepalen van de DNS-hiërarchie onder de naam die bijvoorbeeld zodat u de naam 'www.contoso.com' naar de website van uw bedrijf wordt omgeleid. De registrar kan het domein in een eigen naamservers namens jou hosten of kunt u alternatieve naamservers opgeven.

Azure DNS biedt een serverinfrastructuur naam globaal gedistribueerd, hoge beschikbaarheid, kunt u voor het hosten van uw domein. Door het hosten van uw domeinen in Azure DNS kunt u de DNS-records met dezelfde referenties, API's, hulpprogramma's, facturering en ondersteuning als uw andere Azure-services te beheren.

Azure DNS ondersteunt momenteel geen kopen van domeinnamen. Als u een domeinnaam kopen wilt, moet u een domeinnaamregistrar van derden gebruiken. De registrar brengt doorgaans een kleine jaarlijkse rekening. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheer van DNS-records. Zie [een domein aan Azure DNS overdragen](dns-domain-delegation.md) voor meer informatie.

## <a name="dns-zones"></a>DNS-zones

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS-records

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Time to live

De TTL (of Time to Live) geeft aan hoe lang records door clients in de cache worden geplaatst voordat ze opnieuw worden opgevraagd. In het bovenstaande voorbeeld is de TTL 3600 seconden oftewel 1 uur.

De TTL-waarde is opgegeven voor de recordset en niet voor elke record in Azure DNS, zodat dezelfde waarde wordt gebruikt voor alle records in deze recordset.  U kunt geen TTL-waarde tussen 1 en 2.147.483.647 seconden opgeven.

### <a name="wildcard-records"></a>Recordsets met jokertekens

Azure DNS ondersteunt [recordsets met jokertekens](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Recordsets met jokertekens worden geretourneerd in antwoord op een query met een overeenkomende naam (tenzij er een geschikter is van een recordset van niet-jokertekens). Azure DNS ondersteunt recordsets met jokertekens voor alle recordtypen behalve NS en SOA.

Voor het maken van een recordset met jokertekens gebruikt u de recordnaam '\*'. U kunt ook ook gebruiken een naam met '\*'als de meest linkse label, bijvoorbeeld'\*.foo'.

### <a name="cname-records"></a>CNAME-records

CNAME-recordsets kunnen niet worden gecombineerd met andere recordsets met dezelfde naam. U kan bijvoorbeeld een CNAME-recordset met de relatieve naam 'www' en een A-record maken met de relatieve naam 'www' op hetzelfde moment.

Omdat het toppunt van de zone (naam = ' @') bevat altijd NS en SOA-recordsets die zijn gemaakt toen de zone werd gemaakt, kunt u een CNAME-recordset in het toppunt van de zone maken.

Deze beperkingen worden veroorzaakt door de DNS-standaarden en geen beperkingen van Azure DNS.

### <a name="ns-records"></a>NS-records

De NS-recordset in het toppunt van de zone (naam ' @') automatisch met elke DNS-zone wordt gemaakt en wordt automatisch verwijderd wanneer de zone wordt verwijderd (deze kan niet worden verwijderd afzonderlijk).

Deze recordset bevat de namen van de Azure DNS-naamservers toegewezen aan de zone. U kunt extra naam ingesteld van servers aan deze NS-record, ter ondersteuning van collega hosting domeinen met meer dan één DNS-provider toevoegen. U kunt ook de TTL-waarde en de metagegevens voor deze recordset wijzigen. U kan echter verwijderen of wijzigen van de vooraf ingestelde Azure DNS-naamservers. 

Houd er rekening mee dat dit alleen voor de NS-recordset in het toppunt van de zone geldt. Andere NS-recordsets in de zone (zoals gebruikt voor het delegeren van onderliggende zones) kunnen worden gemaakt, gewijzigd en verwijderd zonder beperking.

### <a name="soa-records"></a>SOA-records

Een recordset SOA wordt automatisch gemaakt in de apex van elke zone (naam = ' @'), en wordt automatisch verwijderd wanneer de zone wordt verwijderd.  SOA-records kunnen niet worden gemaakt of afzonderlijk worden verwijderd.

U kunt alle eigenschappen van de SOA-record, met uitzondering van de eigenschap 'host', die is vooraf geconfigureerd om te verwijzen naar de primaire naam servernaam verstrekt door Azure DNS kunt wijzigen.

### <a name="spf-records"></a>SPF-records

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV-records

[SRV-records](https://en.wikipedia.org/wiki/SRV_record) worden gebruikt door verschillende services serverlocaties op te geven. Bij het opgeven van een SRV-record in Azure DNS:

* De *service* en *protocol* moet worden opgegeven als deel van de naam van de recordset, voorafgegaan door onderstrepingstekens bevatten.  Bijvoorbeeld '\_sip.\_ TCP.name'.  Voor een record in het toppunt van de zone, is niet nodig om op te geven ' @' in de recordnaam gewoon gebruiken de service en het protocol, bijvoorbeeld '\_sip.\_ TCP'.
* De *prioriteit*, *gewicht*, *poort*, en *doel* zijn opgegeven als parameters van elke record in de recordset.

### <a name="txt-records"></a>TXT-records

TXT-records worden gebruikt voor het toewijzen van domeinnamen aan tekenreeksen van willekeurige tekst. Ze worden gebruikt in meerdere toepassingen, met name met betrekking tot e-mailconfiguratie, zoals de [afzender beleid Framework (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) en [DomainKeys geïdentificeerd Mail (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

De DNS-standaarden toe dat een enkele TXT-record bevat meerdere tekenreeksen, die elk kan maximaal 254 tekens lang zijn. Indien meerdere tekenreeksen worden gebruikt, zijn ze door clients worden samengevoegd en behandeld als één tekenreeks.

Wanneer u de Azure DNS-REST-API aanroept, moet u elke tekenreeks TXT afzonderlijk opgeven.  Wanneer u de Azure-portal, PowerShell of CLI interfaces moet u één tekenreeks per record, waarmee automatisch is onderverdeeld in segmenten 254 tekens indien nodig.

Verwar de meerdere tekenreeksen in een DNS-record niet met de verschillende TXT-records in een TXT-Recordset.  Een TXT-recordset kan meerdere records bevatten *die elk* kunnen meerdere tekenreeksen bevatten.  Azure DNS ondersteunt een totaal tekenreekslengte van maximaal 1024 tekens in elke TXT-recordset (voor alle records gecombineerd).

## <a name="tags-and-metadata"></a>Tags en metagegevens

### <a name="tags"></a>Tags

Tags zijn van een lijst met naam / waarde-paren en worden door Azure Resource Manager gebruikt om resources te labelen.  Azure Resource Manager maakt gebruik van labels zodat gefilterde weergaven van uw Azure-factuur, en ook kunt u instellen van een beleid dat labels vereist zijn. Zie [Tags gebruiken om uw Azure-resources te organiseren](../azure-resource-manager/resource-group-using-tags.md) voor meer informatie over tags.

Azure DNS ondersteunt het gebruik van Azure Resource Manager-labels op DNS-zone-resources.  Het ondersteunt geen labels op DNS-recordsets, hoewel als een alternatief 'metadata' wordt ondersteund op DNS-recordsets zoals hieronder wordt uitgelegd.

### <a name="metadata"></a>Metagegevens

Als alternatief voor de recordset labels Azure DNS ondersteunt recordsets met behulp van 'metadata' Aantekeningen maken.  Metagegevens is vergelijkbaar met labels, kunt u naam-waardeparen koppelen aan elke recordset.  Dit kan handig zijn, bijvoorbeeld naar een record voor het doel van elke recordset zijn.  In tegenstelling tot tags, metagegevens kan niet worden gebruikt voor een gefilterde weergave van uw Azure-factuur en kan niet worden opgegeven in een Azure Resource Manager-beleid.

## <a name="etags"></a>Etags

Stel dat twee personen of twee processen probeert u een DNS-record op hetzelfde moment wijzigen. Welke wins? En gewonnen weten dat ze wijzigingen die zijn gemaakt door iemand anders hebt overschreven?

Azure DNS maakt gebruik van Etags gelijktijdige wijzigingen in dezelfde resource veilig afhandelen. Etags zijn gescheiden van [Azure Resource Manager 'Labels'](#tags). Elke DNS-bronrecords (zone of recordset) heeft een Etag die is gekoppeld. Wanneer een resource wordt opgehaald, wordt ook de Etag opgehaald. Bij het bijwerken van een bron, kunt u door te geven weer de Etag zodat Azure DNS of controleren kunt de Etag op de server komt overeen met. Omdat elke update aan een resource in de Etag opnieuw worden gegenereerd resulteert, geeft een Etag komt niet overeen aan dat een gelijktijdige wijziging is doorgevoerd. Etags kan ook worden gebruikt bij het maken van een nieuwe resource om ervoor te zorgen dat de resource niet al bestaat.

Azure DNS PowerShell gebruikt standaard Etags blokkeren gelijktijdige wijzigingen in zones en-recordsets. De optionele *-overschrijven* switch kan worden gebruikt voor het onderdrukken van Etag controles, in welk geval een gelijktijdige wijzigingen die zijn opgetreden worden overschreven.

Op het niveau van de REST-API van Azure DNS-Etags opgegeven met behulp van HTTP-headers.  Hun gedrag is opgenomen in de volgende tabel:

| Koptekst | Gedrag |
| --- | --- |
| Geen |PUT altijd is gelukt (geen Etag-controle) |
| If-match<etag> |PUT slaagt alleen als de bron bestaat en Etag komt overeen met |
| If-match * |PUT slaagt alleen als de bron bestaat |
| If-none-match * |PUT slaagt alleen als resource niet bestaat |


## <a name="limits"></a>Limieten

De standaardlimiet van het volgende van toepassing wanneer u Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Volgende stappen

* Om te starten met behulp van Azure DNS, informatie over hoe [maken van een DNS-zone](dns-getstarted-create-dnszone-portal.md) en [DNS-records maken](dns-getstarted-create-recordset-portal.md).
* Voor het migreren van een bestaande DNS-zone meer informatie over hoe [importeren en exporteren van een DNS-zonebestand](dns-import-export.md).
