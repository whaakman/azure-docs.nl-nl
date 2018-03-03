---
title: Wat is DNS in Azure-Stack | Microsoft Docs
description: Inzicht in de DNS-functies en mogelijkheden in Azure-Stack
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 86ed2805e93bd147841e22a773b52d1451f8c353
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="introducing-idns-for-azure-stack"></a>Introducing IDN's voor Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

IDN's is een functie in Azure-Stack waarmee u bij het oplossen van externe DNS-namen (zoals http://www.bing.com).
U kunt er ook intern virtueel netwerknamen te registreren. Op deze manier kunt u virtuele machines in hetzelfde virtuele netwerk door de naam in plaats van IP-adres, oplossen zonder aangepaste DNS-serveringangen bieden.

Er is iets wat er in Azure is, maar is beschikbaar in Windows Server 2016 en Azure-Stack te.

## <a name="what-does-idns-do"></a>Wat doet de IDN's?
Met IDN's in Azure-Stack beschikt u over de volgende mogelijkheden zonder aangepaste DNS-serveringangen opgeven:

* DNS-services voor naamomzetting tenantwerkbelastingen gedeeld.
* Gezaghebbende DNS-service voor naamomzetting en DNS-registratie in het virtuele netwerk van de tenant.
* Recursive DNS-service voor de omzetting van de Internet-namen van de tenant-VM's. Tenants wordt niet langer hoeft te aangepaste DNS-vermeldingen voor naamomzetting voor Internet (bijvoorbeeld www.bing.com) opgeven.

U kunt nog steeds brengt uw eigen DNS en aangepaste DNS-servers gebruiken als u wilt. Maar nu, als u wilt kunnen Internet-DNS-namen en andere virtuele machines in hetzelfde virtuele netwerk verbinding kunnen maken, moet u niet opgeven iets en werkt alleen op te lossen.

## <a name="what-does-idns-not-do"></a>Wat doet IDN's niet?
Welke IDN's kunt u niet te doen, is een DNS-record maken voor een naam die omgezet van buiten het virtuele netwerk worden kan.

In Azure hebt u de optie voor het opgeven van een label van DNS-naam die gekoppeld aan een openbaar IP-adres worden kan. U kunt het label (voorvoegsel), maar Azure kiest het achtervoegsel, die is gebaseerd op de regio waarin u het openbare IP-adres maken.

![Schermopname van DNS-naamlabel](media/azure-stack-understanding-dns-in-tp2/image3.png)

In de afbeelding hierboven, maakt Azure een "A" record in DNS voor het label van DNS-naam opgegeven onder de zone **westus.cloudapp.azure.com**. Het voorvoegsel en het achtervoegsel samen vormen een Fully Qualified Domain Name (FQDN) die kan worden omgezet vanaf elke locatie op het openbare Internet.

Azure-Stack ondersteunt alleen IDN's voor registratie van de interne naam, zodat het volgende niet doen:

* Maak een DNS-record onder een bestaande gehoste DNS-zone (bijvoorbeeld local.azurestack.external).
* Maak een DNS-zone (zoals Contoso.com).
* Een record bij uw eigen aangepaste DNS-zone maken.
* Ondersteuning voor de aankoop van domeinnamen.

