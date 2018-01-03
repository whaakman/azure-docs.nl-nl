---
title: Overzicht van Azure DNS | Microsoft Docs
description: Overzicht van DNS-hosting-service op Microsoft Azure. Host uw domein in Microsoft Azure.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: f255fd9621ff90bfbb3ad193faa64495acf7ecd7
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="azure-dns-overview"></a>Azure DNS-overzicht

Het Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service het IP-adres. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. Azure DNS ondersteunt nu ook persoonlijke DNS-domeinen. Zie voor meer informatie [Azure DNS gebruiken voor persoonlijke domeinen](private-dns-overview.md).

![DNS-overzicht](./media/dns-overview/scenario.png)

## <a name="features"></a>Functies

* **Betrouwbaarheid en prestaties** -DNS-domeinen in Azure DNS worden gehost op Azure wereldwijd netwerk van DNS-naamservers. Azure DNS maakt gebruik van Anycast networking zodat elke DNS-query wordt beantwoord door de dichtstbijzijnde beschikbare DNS-server. Dit biedt zowel snelle prestaties en hoge beschikbaarheid voor uw domein.

* **Naadloze integratie** -de Azure DNS-service kan worden gebruikt voor het beheren van DNS-records voor uw Azure-services en kan worden gebruikt voor DNS voor ook uw externe bronnen. Azure DNS is geïntegreerd in de Azure-portal en de dezelfde referenties, facturering en support-contract gebruikt als uw andere Azure-services.

* **Beveiliging** -de Azure DNS-service is gebaseerd op Azure Resource Manager. Als zodanig voordelen van Resource Manager-functies zoals rollen gebaseerd toegangsbeheer controlelogboeken en resource vergrendelen. Uw domeinen en records kunnen worden beheerd via de Azure-portal, Azure PowerShell-cmdlets en de platformoverschrijdende Azure CLI. Toepassingen waarbij automatisch DNS-beheer kunnen integreren met de service via de REST-API en SDK's.

Azure DNS ondersteunt momenteel geen kopen van domeinnamen. Als u aanschaffen domeinen wilt, moet u een domeinnaamregistrar van derden gebruiken. De registrar brengt doorgaans een kleine jaarlijkse rekening. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheer van DNS-records. Zie [een domein aan Azure DNS overdragen](dns-domain-delegation.md) voor meer informatie.

## <a name="pricing"></a>Prijzen

DNS-facturering is gebaseerd op het aantal DNS-zones die worden gehost in Azure en door het aantal DNS-query's. Voor meer informatie over prijzen Bezoek [prijzen van Azure DNS-](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>Veelgestelde vragen

Zie voor veelgestelde vragen over Azure DNS de [DNS-Veelgestelde vragen over Azure](dns-faq.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over DNS-zones en records in via: [DNS-zones en registreert overzicht](dns-zones-records.md).

Meer informatie over hoe [maken van een DNS-zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

