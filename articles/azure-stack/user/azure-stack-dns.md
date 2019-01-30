---
title: DNS-server in Azure Stack | Microsoft Docs
description: Met behulp van DNS in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 822fd40b2fc707efe34d4081c3c49e5f71621cdb
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245538"
---
# <a name="using-dns-in-azure-stack"></a>Met behulp van DNS in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack ondersteunt de volgende functies van de Domain Name System (DNS):

* DNS-hostnaamomzetting
* DNS-zones en records met behulp van de API maken en beheren

## <a name="support-for-dns-hostname-resolution"></a>Ondersteuning voor DNS-hostnaamomzetting

U kunt opgeven dat een DNS-domeinnaamlabel voor openbare IP-resources. Maakt gebruik van Azure Stack **domainnamelabel.location.cloudapp.azurestack.external** voor de labelnaam en maps naar het openbare IP-adres in Azure Stack DNS-servers worden beheerd.

Bijvoorbeeld, als u een openbare IP-resource maakt met **contoso** als een domeinnaamlabel op de lokale Azure Stack-locatie, de [volledig gekwalificeerde domeinnaam (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)  **Contoso.local.cloudapp.azurestack.external** wordt omgezet naar het openbare IP-adres van de resource. U kunt deze FDQN gebruiken voor het maken van een aangepast domein-CNAME-record die naar het openbare IP-adres in Azure Stack verwijst.

Zie voor meer informatie over naamomzetting, de [DNS-omzetting](../../dns/dns-for-azure-services.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) artikel.

> [!IMPORTANT]
> Elk domeinnaamlabel dat u maakt moet uniek zijn binnen de Azure Stack-locatie.

De volgende schermafbeelding ziet u de **openbare IP-adres maken** dialoogvenster voor het maken van een openbaar IP-adres met behulp van de portal:

![Openbare IP-adres maken](media/azure-stack-whats-new-dns/image01.png)

### <a name="example-scenario"></a>Voorbeeldscenario

Hebt u een load balancer voor het verwerken van aanvragen van een web-App. Achter de load is balancer site een website die wordt uitgevoerd op een of meer virtuele machines. U kunt toegang tot de website met load balancing met behulp van een DNS-naam in plaats van een IP-adres.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>DNS-zones en records met behulp van de API maken en beheren

U kunt maken en beheren van DNS-zones en records in Azure Stack.

Azure Stack biedt een DNS-service die vergelijkbaar is met Azure, met behulp van API's die consistent met de Azure DNS-API's zijn.  Door uw domeinen in Azure Stack DNS hosten, kunt u uw DNS-records met dezelfde referenties, API's en hulpprogramma's kunt beheren. U kunt ook gebruik van de dezelfde facturering en ondersteuning als uw andere Azure-services.

De Azure Stack-DNS-infrastructuur is compacter dan Azure. De grootte en locatie van een Azure Stack-implementatie is van invloed op DNS-scope, schaal en prestaties. Dit betekent ook dat er prestaties, beschikbaarheid, wereldwijde distributie en hoge beschikbaarheid van implementatie tot implementatie variëren kunnen.

## <a name="comparison-with-azure-dns"></a>Vergelijking met Azure DNS

DNS-server in Azure Stack is vergelijkbaar met de DNS-server in Azure, maar er zijn een paar belangrijke uitzonderingen:

* **Biedt geen ondersteuning voor AAAA-records**: Azure Stack biedt geen ondersteuning voor AAAA-records omdat Azure Stack biedt geen ondersteuning voor IPv6-adressen. Dit is een belangrijk verschil tussen de DNS-server in Azure en Azure Stack.

* **Is niet meerdere tenants**: De DNS-Service in Azure Stack is niet meerdere tenants. Elke tenant kan niet dezelfde DNS-zone maken. Alleen het eerste abonnement waarmee wordt geprobeerd om de zone te maken is gelukt en mislukt de volgende aanvragen. Dit is een belangrijk verschil tussen Azure en Azure Stack DNS.

* **Tags, metagegevens en Etags**: Er zijn kleine verschillen in hoe Azure Stack omgaat met tags, metagegevens, Etags en limieten.

Zie voor meer informatie over Azure DNS, [DNS-zones en records](../../dns/dns-zones-records.md).

### <a name="tags"></a>Tags

Azure Stack DNS ondersteunt het gebruik van Azure Resource Manager-tags op DNS-zone-resources. Het ondersteunt geen tags op DNS-recordsets, hoewel als alternatief kunt **metagegevens** wordt ondersteund op DNS-recordsets, zoals wordt beschreven in de volgende sectie.

### <a name="metadata"></a>Metagegevens

Als alternatief voor de recordset tags, ondersteunt Azure Stack DNS aantekeningen te maken met behulp van recordsets *metagegevens*. Metagegevens is vergelijkbaar met tags, kunt u de naam / waarde-paren koppelen aan elke recordset. Dit kan bijvoorbeeld nuttig zijn om vast te leggen van het doel van elke recordset zijn. In tegenstelling tot tags, u kunt metagegevens niet gebruiken om te voorzien in een gefilterde weergave van uw Azure-factuur en metagegevens kan niet worden opgegeven in een Azure Resource Manager-beleid.

### <a name="etags"></a>Etags

Stel dat twee personen of op twee manieren kunt u proberen te wijzigen van een DNS-record op hetzelfde moment. Welke wins? En de winnaar weten dat ze wijzigingen die zijn gemaakt door iemand anders hebt overschreven?

Azure Stack DNS maakt gebruik van *Etags* veilig gelijktijdige om wijzigingen te verwerken naar dezelfde resource. Etags zijn verschillend van Azure Resource Manager *Tags*. Elke DNS-bronrecords (zone of recordset) heeft een Etag die is gekoppeld. Wanneer een resource wordt opgehaald, wordt ook de ETag die is opgehaald. Wanneer een resource wordt bijgewerkt, kunt u kiezen om door te geven weer de Etag, zodat Azure Stack DNS of controleren kunt de Etag op de server komt overeen met. Omdat elke update voor een resource in de ETag die is opnieuw gegenereerd resulteert, een Etag komt niet overeen geeft aan dat een gelijktijdige wijziging is doorgevoerd. Etags kan ook worden gebruikt bij het maken van een nieuwe resource om ervoor te zorgen dat de resource al bestaat niet.

Azure Stack DNS PowerShell-cmdlets gebruiken Etags standaard, om te voorkomen dat gelijktijdige wijzigingen in zones en -recordsets. U kunt de optionele `-Overwrite` schakelen als u wilt onderdrukken Etag controles, waardoor alle gelijktijdige wijzigingen die hebben plaatsgevonden om te worden overschreven.

Op het niveau van de REST-API van Azure Stack DNS, zijn Etags opgegeven met behulp van HTTP-headers. Hun gedrag wordt in de volgende tabel beschreven:

| Header | Gedrag|
|--------|---------|
| Geen   | PUT altijd is gelukt (geen Etag-controle)|
| If-None-match| PUT slaagt alleen als de resource bestaat en Etag komt overeen met|
| If-None-match *| PUT slaagt alleen als de resource bestaat|
| If-none-match *| PUT slaagt alleen als de resource bestaat niet|

### <a name="limits"></a>Limieten

De volgende standaard beperkingen gelden bij het gebruik van Azure Stack-DNS:

| Resource| Standaardlimiet|
|---------|--------------|
| Zones per abonnement| 100|
| Recordsets per zone| 5000|
| Records per Recordset| 20|

## <a name="next-steps"></a>Volgende stappen

- [Maak kennis met IDN's voor Azure Stack](azure-stack-understanding-dns.md)
