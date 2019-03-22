---
title: Wat is Azure DNS?
description: Overzicht van DNS-hostingservice op Microsoft Azure. Host uw domein op Microsoft Azure.
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 3/21/2019
ms.author: victorh
ms.openlocfilehash: cea19ad2ba03d3e71df32912a1d7ee9e6171689a
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339450"
---
# <a name="what-is-azure-dns"></a>Wat is Azure DNS?

Azure DNS is een service voor het hosten van DNS-domeinen die naamomzetting verzorgt met behulp van de Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services.

U kunt Azure DNS niet gebruiken om een ​​domeinnaam te kopen. Tegen een jaarlijkse vergoeding kunt u een domeinnaam kopen met behulp van [App Service-domeinen](https://docs.microsoft.com/azure/app-service/manage-custom-dns-buy-domain#buy-the-domain) of bij een externe domeinnaamregistrar. Uw domeinen kunnen vervolgens worden gehost in Azure DNS voor recordbeheer. Zie [Delegate a domain to Azure DNS](dns-domain-delegation.md) (Een domein aan Azure DNS overdragen) voor meer informatie.

De volgende functies zijn opgenomen in Azure DNS.

## <a name="reliability-and-performance"></a>Betrouwbaarheid en prestaties

DNS-domeinen in Azure DNS worden gehost op het wereldwijde netwerk van Azure DNS-naamservers. Azure DNS maakt gebruik van anycast-netwerken. Elke DNS-query wordt beantwoord door de dichtstbijzijnde DNS-server voor het leveren van snelle prestaties en hoge beschikbaarheid voor uw domein.

## <a name="security"></a>Beveiliging

 Azure DNS is gebaseerd op Azure Resource Manager, die onder meer de volgende functies biedt:

* [op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) om te bepalen wie er toegang heeft tot bepaalde acties voor uw organisatie.

* [activiteitenlogboeken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) om te controleren hoe een gebruiker in uw organisatie een resource heeft gewijzigd, of te gebruiken bij het opsporen van fouten.

* [vergrendelen van resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) om een abonnement, een resourcegroep of een resource te vergrendelen. Met vergrendeling voorkomt u dat andere gebruikers in uw organisatie per ongeluk kritieke bronnen wijzigen of verwijderen.

Zie voor meer informatie [DNS-zones en -records beschermen](dns-protect-zones-recordsets.md). 

## <a name="dnssec"></a>DNSSEC

Azure DNS biedt momenteel geen ondersteuning voor DNSSEC. U kunt in de meeste gevallen de noodzaak van DNSSEC beperken door consistent gebruik van HTTPS/TLS in uw toepassingen. Als DNSSEC een essentiële vereiste is voor uw DNS-zones is, kunt u deze zones hosten met host DNS-providers van derden.

## <a name="ease-of-use"></a>Gebruiksgemak

 Azure DNS kan DNS-records voor uw Azure-services beheren en ook DNS leveren aan uw externe resources. Azure DNS is geïntegreerd in Azure Portal en gebruikt dezelfde referenties, hetzelfde ondersteuningscontract en dezelfde facturering als uw andere Azure-services. 

De facturering van DNS vindt plaats op basis van het aantal DNS-zones dat in Azure wordt gehost en het aantal DNS-query's dat wordt ontvangen. Zie [Prijzen voor Azure DNS](https://azure.microsoft.com/pricing/details/dns/) voor meer informatie over prijzen.

Uw domeinen en records kunnen worden beheerd met behulp van de Azure-portal, Azure PowerShell-cmdlets en de platformoverschrijdende Azure CLI. Toepassingen waarvoor automatisch DNS-beheer is vereist, kunnen met de service worden geïntegreerd met behulp van de REST-API en SDK's.

## <a name="customizable-virtual-networks-with-private-domains"></a>Aanpasbare virtuele netwerken met privédomeinen

Azure DNS ondersteunt ook privé-DNS-domeinen, door middel van een functie die nu in openbare preview is. Met deze functie kunt u uw eigen aangepaste domeinnamen gebruiken in uw virtuele privénetwerken in plaats van de door Azure geleverde namen die momenteel beschikbaar zijn.

Zie [Azure DNS gebruiken voor privédomeinen](private-dns-overview.md) voor meer informatie.

## <a name="alias-records"></a>Aliasrecords

Azure DNS ondersteunt alias-recordsets. U kunt een recordset alias gebruiken om te verwijzen naar een Azure-resource, zoals een openbaar IP-adres van Azure, een Azure Traffic Manager-profiel of een eindpunt Azure Content Delivery Network (CDN). Als het IP-adres van de onderliggende resource verandert, wordt de alias-recordset automatisch bijgewerkt tijdens DNS-omzetting. De alias-record set verwijst naar het service-exemplaar en het service-exemplaar is gekoppeld aan een IP-adres.

Bovendien kunt u nu uw apex of een domein zonder voorvoegsel zijn verwijzen naar een Traffic Manager-profiel of een CDN-eindpunt met behulp van een alias-record. Bijvoorbeeld: contoso.com.

Zie [Overzicht van Azure DNS-aliasrecords](dns-alias.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van DNS-zones en -records](dns-zones-records.md) voor meer informatie over DNS-zones en records.

* Zie [Een DNS-zone maken](./dns-getstarted-create-dnszone-portal.md) voor meer informatie over het maken van een zone in Azure DNS.

* Zie voor veelgestelde vragen over Azure DNS de [Veelgestelde vragen over Azure DNS](dns-faq.md).

