---
title: DNS-Zones en Records overzicht - Azure DNS | Microsoft Docs
description: Overzicht van ondersteuning voor het hosten van DNS-zones en records in de Microsoft Azure DNS.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: victorh
ms.openlocfilehash: 7f69d77ac7a6c2a17ef2568f0c7edaef2e1ee3f5
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174414"
---
# <a name="overview-of-dns-zones-and-records"></a>Overzicht van DNS-zones en records

Deze pagina leest u de belangrijkste concepten van domeinen, DNS-zones, DNS-records en recordsets en hoe ze worden ondersteund in Azure DNS.

## <a name="domain-names"></a>Domeinnamen

Het Domain Name System is een hiërarchie van domeinen. De hiërarchie start vanaf het hoofddomein. De naam van dit domein is eenvoudigweg '**.**'.  Hieronder komen de topleveldomeinen, zoals com, net, org, uk of nl.  Onder de topleveldomeinen komen de secondleveldomeinen, zoals org.uk of co.jp. De domeinen in de DNS-hiërarchie worden globaal gedistribueerd en gehost door DNS-naamservers over de hele wereld.

Een domeinnaamregistrar is een organisatie waarmee u een domeinnaam, bijvoorbeeld 'contoso.com' aanschaffen.  De naam van een domein aanschaffen, geeft u het recht voor het beheren van de DNS-hiërarchie met die naam, bijvoorbeeld zodat u de naam 'www.contoso.com' naar de website van uw bedrijf. De registrar kan hosten van het domein in een eigen naamservers uit uw naam of kunt u alternatieve naam voor servers opgeven.

Azure DNS biedt een wereldwijd gedistribueerde, hoge beschikbaarheid naam server-infrastructuur, die u gebruiken kunt voor het hosten van uw domein. Door uw domeinen in Azure DNS hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's, facturering en ondersteuning als uw andere Azure-services beheren.

Azure DNS ondersteunt momenteel geen domeinnamen aanschaffen. Als u een domeinnaam aanschaffen wilt, moet u het gebruik van een externe domeinnaamregistrar. De registrar kosten doorgaans jaarlijks een klein rekening. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheren van DNS-records. Zie [een domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

## <a name="dns-zones"></a>DNS-zones

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS-records

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Time-to-live

De time to live van of de TTL-waarde, geeft u aan hoe lang elke record is opgeslagen in de cache door clients voordat opnieuw een query wordt uitgevoerd. In het bovenstaande voorbeeld wordt de TTL is 3600 seconden oftewel 1 uur.

De TTL-waarde is opgegeven voor de recordset en niet voor elke record in Azure DNS, zodat dezelfde waarde wordt gebruikt voor alle records in deze recordset.  U kunt een TTL-waarde tussen 1 en 2.147.483.647 seconden opgeven.

### <a name="wildcard-records"></a>Recordsets met jokertekens

Azure DNS ondersteunt [recordsets met jokertekens](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Recordsets met jokertekens worden geretourneerd in antwoord op een query met een overeenkomende naam (tenzij er een geschikter is van een recordset zonder jokertekens). Azure DNS ondersteunt recordsets met jokertekens voor alle recordtypen behalve NS en SOA.

Voor het maken van een recordset met jokertekens, gebruikt u de recordnaam '\*'. U kunt ook ook gebruiken een naam met '\*'als het meest linkse label, bijvoorbeeld'\*.foo'.

### <a name="caa-records"></a>CAA-records

CAA-records kunnen domeineigenaars om op te geven welke certificeringsinstanties (CA's) zijn gemachtigd voor het uitgeven van certificaten voor hun domein. Hierdoor kan er CA's om te voorkomen dat niet uitgeven van certificaten in sommige gevallen. CAA-records hebben drie eigenschappen:
* **Vlaggen**: dit is een geheel getal tussen 0 en 255, de kritieke vlag waarvoor speciale betekenis per vertegenwoordigt de [RFC](https://tools.ietf.org/html/rfc6844#section-3)
* **Tag**: een ASCII-tekenreeks die van de volgende een:
    * **probleem**: Gebruik deze optie als u opgeven van CA's die zijn toegestaan wilt voor het uitgeven van certificaten (alle typen)
    * **issuewild**: Gebruik deze optie als u opgeven van CA's die zijn toegestaan wilt voor het uitgeven van certificaten (alleen voor jokertekens-certificaten)
    * **iodef**: Geef een e-mailadres of de hostnaam op die CA's voor niet-geautoriseerde cert probleem aanvragen kan melden
* **Waarde**: de waarde voor de specifieke Tag gekozen

### <a name="cname-records"></a>CNAME-records

CNAME-recordsets kunnen niet worden gecombineerd met andere recordsets met dezelfde naam. U niet kan bijvoorbeeld een CNAME-recordset met de relatieve naam 'www' en een A-record maken met de relatieve naam 'www' op hetzelfde moment.

Omdat het toppunt van de zone (naam = '\@') altijd NS en SOA-recordsets die zijn gemaakt toen de zone werd gemaakt, kunt u een CNAME-recordset in de apex van de zone kan niet maken.

Deze beperkingen optreden als gevolg van de DNS-standaarden en zijn geen beperkingen van Azure DNS.

### <a name="ns-records"></a>NS-records

De NS-recordset in de apex van de zone (naam '\@') wordt automatisch gemaakt met elke DNS-zone en wordt automatisch verwijderd wanneer de zone wordt verwijderd (deze kan niet worden verwijderd afzonderlijk).

Deze recordset bevat de namen van de Azure DNS-naamservers toegewezen aan de zone. U kunt de naam van aanvullende servers aan deze NS-record ingesteld voor de ondersteuning van collega hosting-domeinen met meer dan één DNS-provider toevoegen. U kunt ook de TTL-waarde en de metagegevens voor deze recordset wijzigen. U kan echter verwijderen of wijzigen van de vooraf ingestelde Azure DNS-naamservers. 

Dit geldt alleen voor de NS-recordset in de apex van de zone. Andere NS-recordsets in de zone (zoals die wordt gebruikt om te delegeren onderliggende zones) kunnen worden gemaakt, gewijzigd en verwijderd zonder beperking.

### <a name="soa-records"></a>SOA-records

Een set van SOA-record wordt automatisch gemaakt in de apex van elke zone (naam = '\@'), en wordt automatisch verwijderd wanneer de zone wordt verwijderd.  SOA-records kunnen niet worden gemaakt of afzonderlijk worden verwijderd.

U kunt alle eigenschappen van de SOA-record, met uitzondering van de eigenschap 'host', dit is vooraf geconfigureerd om te verwijzen naar de naam van de primaire naam verstrekt door Azure DNS kunt wijzigen.

### <a name="spf-records"></a>SPF-records

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV-records

[SRV-records](https://en.wikipedia.org/wiki/SRV_record) worden gebruikt door verschillende services serverlocaties op te geven. Bij het opgeven van een SRV-record in Azure DNS:

* De *service* en *protocol* moet worden opgegeven als onderdeel van de naam van de recordset, voorafgegaan door onderstrepingstekens bevatten.  Bijvoorbeeld '\_sip.\_ TCP.name'.  Voor een record in de apex van de zone, is het niet nodig om op te geven '\@'in de recordnaam gewoon gebruiken de service en het protocol, bijvoorbeeld'\_sip.\_ TCP'.
* De *prioriteit*, *gewicht*, *poort*, en *doel* zijn opgegeven als parameters van elke record in de recordset.

### <a name="txt-records"></a>TXT-records

TXT-records worden gebruikt voor het toewijzen van domeinnamen aan willekeurige teksttekenreeksen. Ze worden gebruikt in meerdere toepassingen, met name met betrekking tot de configuratie van e-mail, zoals de [afzender beleid Framework (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) en [DomainKeys geïdentificeerd e-Mail (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

De DNS-standaarden staan een enkel TXT-record bevat meerdere tekenreeksen, die elk kan maximaal 254 tekens lang zijn. Indien meerdere tekenreeksen worden gebruikt, zijn ze samengevoegd door clients en behandeld als een enkele tekenreeks.

Bij het aanroepen van de REST-API van Azure DNS, moet u elke tekenreeks TXT afzonderlijk opgeven.  Wanneer u de Azure-portal, PowerShell of CLI interfaces moet u één tekenreeks per record, waarmee automatisch is onderverdeeld in segmenten 254 tekens indien nodig.

De meerdere tekenreeksen in een DNS-record moeten niet worden verward met de meerdere TXT-records in een TXT-Recordset.  Een TXT-recordset kan meerdere records bevatten *die elk* meerdere tekenreeksen kan bevatten.  Azure DNS ondersteunt een totaal tekenreekslengte van maximaal 1024 tekens in elke TXT-recordset (voor alle records gecombineerd).

## <a name="tags-and-metadata"></a>-Tags en metagegevens

### <a name="tags"></a>Tags

Tags zijn een lijst met naam / waarde-paren en worden door Azure Resource Manager gebruikt om resources te labelen.  Azure Resource Manager maakt gebruik van tags om in te schakelen gefilterde weergaven van uw Azure-factuur en ook kunt u instellen van een beleid labels vereist zijn. Zie [Tags gebruiken om uw Azure-resources te organiseren](../azure-resource-manager/resource-group-using-tags.md) voor meer informatie over tags.

Azure DNS ondersteunt het gebruik van Azure Resource Manager-tags op DNS-zone-resources.  Het ondersteunt geen tags op DNS-recordsets, maar als een alternatief 'metagegevens' wordt ondersteund op DNS-record wordt ingesteld, zoals hieronder wordt beschreven.

### <a name="metadata"></a>Metagegevens

Als alternatief voor de recordset tags Azure DNS ondersteunt aantekeningen te maken met behulp van 'metagegevens' recordsets.  Metagegevens is vergelijkbaar met tags, kunt u de naam / waarde-paren koppelen aan elke recordset.  Dit kan handig zijn, bijvoorbeeld voor het doel van elke recordset record zijn.  In tegenstelling tot tags, metagegevens kan niet worden gebruikt om te voorzien in een gefilterde weergave van uw Azure-factuur en kan niet worden opgegeven in een Azure Resource Manager-beleid.

## <a name="etags"></a>Etags

Stel dat twee personen of op twee manieren kunt u proberen te wijzigen van een DNS-record op hetzelfde moment. Welke wins? En de winnaar weten dat ze wijzigingen die zijn gemaakt door iemand anders hebt overschreven?

Azure DNS maakt gebruik van Etags voor het afhandelen van gelijktijdige wijzigingen in de dezelfde resource veilig. Etags staan los van [Azure Resource Manager-Tags'](#tags). Elke DNS-bronrecords (zone of recordset) heeft een Etag die is gekoppeld. Wanneer een resource wordt opgehaald, wordt ook de ETag die is opgehaald. Wanneer een resource wordt bijgewerkt, kunt u kiezen om door te geven weer de Etag, zodat Azure DNS of controleren kunt de Etag op de server komt overeen met. Omdat elke update voor een resource in de ETag die is opnieuw gegenereerd resulteert, een Etag komt niet overeen geeft aan dat een gelijktijdige wijziging is doorgevoerd. Etags kan ook worden gebruikt bij het maken van een nieuwe resource om ervoor te zorgen dat de resource al bestaat niet.

Azure DNS PowerShell gebruikt standaard Etags voorkomen dat gelijktijdige wijzigingen in zones en-recordsets. De optionele *-overschrijven* switch kan worden gebruikt om te onderdrukken Etag controles, in welk geval een gelijktijdige wijzigingen die zijn opgetreden worden overschreven.

Etags zijn opgegeven met behulp van HTTP-headers op het niveau van de REST-API van Azure DNS.  Hun gedrag is opgenomen in de volgende tabel:

| Koptekst | Gedrag |
| --- | --- |
| Geen |PUT altijd is gelukt (geen Etag-controle) |
| If-None-match <etag> |PUT slaagt alleen als de resource bestaat en Etag komt overeen met |
| If-None-match * |PUT slaagt alleen als de resource bestaat |
| If-none-match * |PUT slaagt alleen als de resource bestaat niet |


## <a name="limits"></a>Limieten

De volgende standaard beperkingen gelden bij het gebruik van Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Volgende stappen

* Als u wilt gaan met behulp van Azure DNS, informatie over hoe u [maken van een DNS-zone](dns-getstarted-create-dnszone-portal.md) en [DNS-records maken](dns-getstarted-create-recordset-portal.md).
* Als u wilt migreren van een bestaande DNS-zone, informatie over hoe u [importeren en exporteren van een DNS-zonebestand](dns-import-export.md).
