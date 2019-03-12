---
title: Informatie over IDN's in Azure Stack | Microsoft Docs
description: Informatie over IDN-functies en mogelijkheden in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 32d00a1c0daf383d41d6bbc7c9fe105b3a2a6ab1
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57774912"
---
# <a name="introducing-idns-for-azure-stack"></a>Maak kennis met IDN's voor Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

IDN's is een Azure Stack-netwerken functie waarmee u externe DNS-namen worden omgezet (bijvoorbeeld https://www.bing.com.) ook kunt u intern virtueel netwerknamen te registreren. Op deze manier kunt u virtuele machines in hetzelfde virtuele netwerk oplossen door de naam in plaats van IP-adres. Deze aanpak wordt de noodzaak voor aangepaste DNS-serververmeldingen verwijderd. Zie voor meer informatie over DNS de [Azure DNS-overzicht](https://docs.microsoft.com/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>Wat doet IDN's?

Met IDN's in Azure Stack krijgt u de volgende mogelijkheden, zonder te geven van aangepaste DNS-servergegevens:

- DNS-services voor naamomzetting tenantwerkbelastingen gedeeld.
- Gezaghebbende DNS-service voor naamomzetting en DNS-registratie binnen het virtuele tenantnetwerk.
- Recursive DNS-service voor het omzetten van de Internet-namen van de tenant-VM's. Tenants niet langer nodig hebt om op te geven van aangepaste DNS-vermeldingen om op te lossen Internet-namen (bijvoorbeeld www.bing.com.)

U kunt nog steeds doen om uw eigen DNS en gebruiken van aangepaste DNS-servers. Echter met behulp van IDN's, kunt u Internet-DNS-namen omzetten en maak verbinding met andere virtuele machines in hetzelfde virtuele netwerk, u hoeft te maken van aangepaste DNS-vermeldingen.

## <a name="what-doesnt-idns-do"></a>Wat biedt geen IDN's?

Welke IDN's staat niet toe dat u dit doet, is het maken van een DNS-record voor een naam die omgezet van buiten het virtuele netwerk worden kan.

In Azure hebt u de optie voor het opgeven van een DNS-naamlabel die is gekoppeld aan een openbaar IP-adres. U kunt het label (voorvoegsel), maar Azure kiest het achtervoegsel, die is gebaseerd op de regio waarin u het openbare IP-adres maakt.

![Voorbeeld van een DNS-naamlabel](media/azure-stack-understanding-dns-in-tp2/image3.png)

Zoals in de vorige afbeelding wordt weergegeven, Azure maakt een 'A'-record in DNS voor de DNS-naamlabel opgegeven onder de zone **westus.cloudapp.azure.com**. Het voorvoegsel en het achtervoegsel worden gecombineerd om op te stellen een [volledig gekwalificeerde domeinnaam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) die kan worden omgezet vanaf elke locatie op het openbare Internet.

Azure Stack ondersteunt alleen IDN's voor registratie van de interne naam, zodat het volgende niet doen:

- Maak een DNS-record in een bestaande gehoste DNS-zone (bijvoorbeeld local.azurestack.external.)
- Maken van een DNS-zone (zoals Contoso.com).
- Een record bij uw eigen aangepaste DNS-zone maken.
- Ondersteuning voor de aanschaf van domeinnamen.

## <a name="next-steps"></a>Volgende stappen

[Met behulp van DNS in Azure Stack](azure-stack-dns.md)
