---
title: Informatie over IDN's in Azure-Stack | Microsoft Docs
description: Informatie over IDN's functies en mogelijkheden in Azure-Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: scottnap
ms.openlocfilehash: 9123160f42adea57c28dff265bd5b5dbbcbb7918
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724255"
---
# <a name="introducing-idns-for-azure-stack"></a>Introducing IDN's voor Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

IDN's is een Azure-Stack netwerken functie waarmee u bij het oplossen van externe DNS-namen (bijvoorbeeld http://www.bing.com.) ook kunt u intern virtueel netwerknamen te registreren. Op deze manier kunt u virtuele machines in hetzelfde virtuele netwerk oplossen door de naam in plaats van IP-adres. Deze aanpak verwijdert de behoefte om aangepaste DNS-serveringangen. Zie voor meer informatie over DNS de [Azure DNS-overzicht](https://docs.microsoft.com/en-us/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>Wat doet de IDN's?

Met IDN's in Azure-Stack beschikt u over de volgende mogelijkheden zonder aangepaste DNS-serveringangen opgeven:

- DNS-services voor naamomzetting tenantwerkbelastingen gedeeld.
- Gezaghebbende DNS-service voor naamomzetting en DNS-registratie in het virtuele netwerk van de tenant.
- Recursive DNS-service voor de omzetting van de Internet-namen van de tenant-VM's. Tenants niet langer nodig hebt om op te geven van aangepaste DNS-vermeldingen voor naamomzetting voor Internet (bijvoorbeeld www.bing.com.)

U kunt nog steeds brengt uw eigen DNS en aangepaste DNS-servers gebruiken. Echter met behulp van IDN's, kunt u Internet-DNS-namen omzetten en verbinding maken met andere virtuele machines in hetzelfde virtuele netwerk, u hoeft niet te maken van aangepaste DNS-vermeldingen.

## <a name="what-doesnt-idns-do"></a>Wat niet IDN's?

Welke IDN's kunt u niet te doen, is een DNS-record maken voor een naam die omgezet van buiten het virtuele netwerk worden kan.

In Azure hebt u de optie voor het opgeven van een label van DNS-naam die is gekoppeld aan een openbaar IP-adres. U kunt het label (voorvoegsel), maar Azure kiest het achtervoegsel, die is gebaseerd op de regio waarin u het openbare IP-adres maken.

![Voorbeeld van een label van DNS-naam](media/azure-stack-understanding-dns-in-tp2/image3.png)

Als de vorige afbeelding toont, Azure maakt een 'A'-record in DNS voor het label van DNS-naam opgegeven onder de zone **westus.cloudapp.azure.com**. Het voorvoegsel en het achtervoegsel worden gecombineerd om op te stellen een [volledig gekwalificeerde domeinnaam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) die kan worden omgezet vanaf elke locatie op het openbare Internet.

Azure-Stack ondersteunt alleen IDN's voor registratie van de interne naam, zodat het volgende niet doen:

- Maak een DNS-record onder een bestaande gehoste DNS-zone (bijvoorbeeld local.azurestack.external.)
- Maken van een DNS-zone (zoals Contoso.com).
- Een record bij uw eigen aangepaste DNS-zone maken.
- Ondersteuning voor de aankoop van domeinnamen.

## <a name="next-steps"></a>Volgende stappen

[Met behulp van DNS in Azure-Stack](azure-stack-dns.md)
