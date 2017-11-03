---
title: DNS-server in Azure Stack | Microsoft Docs
description: DNS in Azure Stack
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: ac93b5eb4228cef373428b7b69932d5993d54fa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="dns-in-azure-stack"></a>DNS in Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack bevat de volgende DNS-functies:
* Ondersteuning voor DNS-hostnaamomzetting
* DNS-zones en registreert met behulp van API maken en beheren

## <a name="support-for-dns-hostname-resolution"></a>Ondersteuning voor DNS-hostnaamomzetting
U kunt opgeven dat een DNS-domeinnaamlabel voor een openbaar IP-resource, maakt u een toewijzing voor *domainnamelabel.location*. cloudapp.azurestack.external het openbare IP-adres in de Azure-Stack beheerde DNS-servers.  

Bijvoorbeeld, als u een openbaar IP-resource met **contoso** als een domeinnaamlabel in de lokale Azure-Stack-locatie, de volledig gekwalificeerde domeinnaam (FQDN) **contoso.local.cloudapp.azurestack.external**wordt omgezet naar het openbare IP-adres van de resource. U kunt deze FDQN-naam gebruiken voor het maken van een aangepast domein CNAME-record die naar het openbare IP-adres in Azure-Stack verwijst.

> [!IMPORTANT]
> Elke domeinnaamlabel gemaakt moet uniek zijn binnen de Azure-Stack-locatie.

Als u het openbare IP-adres met behulp van de portal maakt, als volgt uitziet:

![Openbare IP-adres maken](media/azure-stack-whats-new-dns/image01.png)

Dit is handig als u wilt koppelen van een openbaar IP-adres aan een resource taakverdeling. Bijvoorbeeld wellicht een load balancer verwerking van aanvragen van een webtoepassing. Achter de load is balancer site een website zich bevinden op een of meer virtuele machines. Nu kunt u de website netwerktaakverdeling openen door een DNS-naam, in plaats van door een IP-adres.

## <a name="create-and-manage-dns-zones-and-records-using-api"></a>DNS-zones en registreert met behulp van API maken en beheren
U kunt maken en beheren van DNS-zones en records in Azure-Stack.  

Azure Stack biedt een DNS-service, zoals Azure API's die consistent met de Azure DNS-API's zijn gebruiken.  U kunt de DNS-records met dezelfde referenties, API's, hulpprogramma's, facturering en ondersteuning door het hosten van uw Azure-Stack DNS-domeinen beheren als uw andere Azure-services. 

De Azure-Stack-DNS-infrastructuur is omwille van de hand liggende compacter dan van Azure. Dus afhankelijk de scope, schaal en prestaties van de schaal van de Azure-Stack-implementatie en de omgeving waarin deze is geïmplementeerd.  Items zoals prestaties, beschikbaarheid, wereldwijde distributie en hoge beschikbaarheid (HA) kunnen dus van implementatie tot implementatie variëren.

## <a name="comparison-with-azure-dns"></a>Vergelijking met Azure DNS
DNS-server in Azure-Stack is vergelijkbaar met DNS-server in Azure, met twee belangrijke uitzonderingen:
* **AAAA-records worden niet ondersteund**

    Azure-Stack biedt geen ondersteuning voor AAAA-records omdat Azure Stack biedt geen ondersteuning voor IPv6-adressen.  Dit is een belangrijk verschil tussen DNS-server in Azure en Azure-Stack.
* **Het is niet meerdere tenants**

    In tegenstelling tot Azure is de DNS-Service in Azure-Stack niet meerdere tenants. Elke tenant kan dus niet dezelfde DNS-zone maken. Alleen het eerste abonnement dat u probeert te maken van de zone is gelukt en volgende aanvragen mislukken.  Het is een bekend probleem en een belangrijk verschil tussen Azure en Azure Stack DNS. Dit probleem worden opgelost in een toekomstige release.

Bovendien zijn er een aantal kleine verschillen in hoe Azure-Stack DNS labels, metagegevens, Etags en limieten implementeert.

De volgende informatie geldt specifiek voor Azure Stack DNS en verschilt enigszins van Azure DNS. Zie voor meer informatie over Azure DNS, [DNS-zones en registreert](../../dns/dns-zones-records.md) op de site van Microsoft Azure-documentatie.

### <a name="tags-metadata-and-etags"></a>Labels, metagegevens en Etags

**Tags**

Azure-Stack DNS ondersteunt het gebruik van Azure Resource Manager-labels op DNS-zone-resources. Het ondersteunt geen labels op DNS-recordsets, hoewel als een alternatief 'metadata' wordt ondersteund op DNS-record zoals uitgelegd vervolgens ingesteld.

**Metagegevens**

Als alternatief voor de recordset labels ondersteunt Azure Stack DNS-recordsets met behulp van 'metadata' Aantekeningen maken. Metagegevens is vergelijkbaar met labels, kunt u naam-waardeparen koppelen aan elke recordset. Dit kan bijvoorbeeld zijn nuttig voor het vastleggen van het doel van elke recordset. In tegenstelling tot tags, metagegevens kan niet worden gebruikt voor een gefilterde weergave van uw Azure-factuur en kan niet worden opgegeven in een Azure Resource Manager-beleid.

**Etags**

Stel dat twee personen of twee processen probeert u een DNS-record op hetzelfde moment wijzigen. Welke wins? En gewonnen weten dat ze wijzigingen die zijn gemaakt door iemand anders hebt overschreven?

Etags Azure Stack DNS gebruikt voor het afhandelen van gelijktijdige wijzigingen in dezelfde resource veilig. Etags zijn gescheiden van Azure Resource Manager 'Labels'. Elke DNS-bronrecords (zone of recordset) heeft een Etag die is gekoppeld. Wanneer een resource wordt opgehaald, wordt ook de Etag opgehaald. Bij het bijwerken van een bron, kunt u door te geven weer de Etag zodat Azure Stack DNS of controleren kunt de Etag op de server komt overeen met. Omdat elke update aan een resource in de Etag opnieuw worden gegenereerd resulteert, geeft een Etag komt niet overeen aan dat een gelijktijdige wijziging is doorgevoerd. Etags kan ook worden gebruikt bij het maken van een nieuwe resource om ervoor te zorgen dat de resource niet al bestaat.

Standaard gebruikt Azure Stack DNS PowerShell Etags blokkeren gelijktijdige wijzigingen in zones en-recordsets. De optionele *-overschrijven* switch kan worden gebruikt voor het onderdrukken van Etag controles, in welk geval een gelijktijdige wijzigingen die zijn opgetreden worden overschreven.

Op het niveau van de REST-API van Azure Stack DNS-Etags opgegeven met behulp van HTTP-headers. Hun gedrag is opgenomen in de volgende tabel:

| Koptekst | Gedrag|
|--------|---------|
| Geen   | PUT altijd is gelukt (geen Etag-controle)|
| If-match| PUT slaagt alleen als de bron bestaat en Etag komt overeen met|
| If-match *| PUT slaagt alleen als de bron bestaat|
| If-none-match *| PUT slaagt alleen als resource niet bestaat|

### <a name="limits"></a>Limieten

De standaardlimiet van het volgende van toepassing wanneer met behulp van Azure Stack DNS:

| Resource| Standaardlimiet|
|---------|--------------|
| Zones per abonnement| 100|
| Recordsets per zone| 5000|
| Records per Recordset| 20|

## <a name="next-steps"></a>Volgende stappen
[Introducing IDN's voor Azure-Stack](azure-stack-understanding-dns.md)
