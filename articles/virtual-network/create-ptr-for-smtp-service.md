---
title: Zones voor reverse lookup voor een controle van de SMTP-koptekst configureren in Azure
titlesuffix: Azure Virtual Network
description: Beschrijft hoe u zones voor reverse lookup voor een controle van de SMTP-koptekst configureert in Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 203c3c5f371af7de891f0949a35378294bb50a0e
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652379"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Zones voor reverse lookup voor een controle van de SMTP-koptekst configureren

In dit artikel wordt beschreven hoe u een reverse zone in Azure DNS gebruiken en maken van een omgekeerde DNS PTR-record voor het controleren van de SMTP-koptekst.

## <a name="symptom"></a>Symptoom

Als u een SMTP-server in Microsoft Azure hosten, ontvangt u mogelijk het volgende foutbericht weergegeven wanneer verzenden of ontvangen van externe e-mailservers:

**554: Er is geen PTR-Record**

## <a name="solution"></a>Oplossing

Voor een virtueel IP-adres in Azure, worden de reverse-records gemaakt in domein zones, geen aangepast domein zones eigendom zijn van Microsoft.

PTR-records configureren in zones eigendom zijn van Microsoft, gebruiken de ReverseFqdn - eigenschap op de bron van het openbare IP-adres. Zie voor meer informatie, [configureren reverse-DNS voor services die worden gehost in Azure](../dns/dns-reverse-dns-for-azure-services.md).

Wanneer u de PTR-records configureert, zorg ervoor dat het IP-adres en de omgekeerde FQDN-naam zijn eigendom van het abonnement. Als u probeert om in te stellen van een omgekeerde FQDN-naam die geen deel van het abonnement uitmaken, ontvangt u de volgende strekking weergegeven:

    Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                        
    1) ReverseFqdn komt overeen met de FQDN-naam van een openbaar IP-resource onder het abonnement;
    2) ReverseFqdn wordt omgezet naar de FQDN-naam (via de keten van CName-records) van een openbaar IP-adres bron onder het abonnement;
    3) Het wordt omgezet naar het ip-adres (via CNAME- en A-records) van een statisch openbaar ip-resource onder het abonnement.

Als u handmatig wijzigen uw SMTP-koptekst zodat deze overeenkomt met onze standaard reverse FQDN-naam, de externe e-mailserver kan nog steeds mislukken omdat deze de SMTP-koptekst host zodat deze overeenkomen met de MX-record voor het domein kan verwacht.
