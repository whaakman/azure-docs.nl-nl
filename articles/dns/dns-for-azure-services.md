---
title: Azure DNS gebruiken met andere Azure-services | Microsoft Docs
description: Informatie over hoe u Azure DNS gebruikt voor het omzetten van de naam voor andere Azure-services
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: victorh
ms.openlocfilehash: 39fe23d7289dc78736dd5a85d4100af82b1d7b4a
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398212"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>De werking van Azure DNS met andere Azure-services

Azure DNS is een gehoste DNS-beheer en de naam resolution-service. Hiermee kunt u openbare DNS-namen voor de andere toepassingen en services die u hebt geïmplementeerd in Azure maakt. Het maken van een naam op voor een Azure-service in uw aangepaste domein is net zo eenvoudig als een record van het juiste type voor uw service toe te voegen.

* Voor dynamisch toegewezen IP-adressen, moet u een DNS CNAME-record die wordt toegewezen aan de DNS-naam die Azure hebt gemaakt voor uw service maken. DNS-standaarden te voorkomen dat u met behulp van een CNAME-record voor het toppunt van de zone.
* Voor statisch toegewezen IP-adressen, kunt u een DNS-A-record met behulp van een naam, met inbegrip van een *domein zonder voorvoegsel zijn* naam in het toppunt van de zone.

De volgende tabel geeft een overzicht van de ondersteunde recordtypen die kunnen worden gebruikt voor verschillende Azure-services. Zoals u in deze tabel zien kunt, ondersteunt Azure DNS DNS-records alleen voor internetgerichte-netwerkbronnen. Azure DNS kan niet worden gebruikt voor het omzetten van interne, persoonlijke adressen.

| Azure-service | Netwerkinterface | Beschrijving |
| --- | --- | --- |
| Application Gateway |[Front-end-openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A of CNAME-record maken. |
| Load Balancer |[Front-end-openbaar IP-adres](dns-custom-domain.md#public-ip-address)  |U kunt een DNS-A of CNAME-record maken. Load Balancer kan een openbare IPv6-IP-adres dat wordt dynamisch toegewezen hebben. Daarom moet u een CNAME-record voor een IPv6-adres maken. |
| Traffic Manager |Openbare-naam |U kunt alleen een CNAME die is toegewezen aan de trafficmanager.net-naam toegewezen aan uw Traffic Manager-profiel maken. Zie voor meer informatie, [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md). |
| Cloudservice |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A-record maken voor statisch toegewezen IP-adressen. Voor dynamisch toegewezen IP-adressen, moet u een CNAME-record die wordt toegewezen aan de *cloudapp.net* naam.|
| App Service | [Extern IP-adres](dns-custom-domain.md#app-service-web-apps) |U kunt een DNS-A-record maken voor externe IP-adressen. Anders moet u een CNAME-record die wordt toegewezen aan de naam van de azurewebsites.net maken. Zie voor meer informatie, [een aangepaste domeinnaam toewijzen aan een Azure-app](../app-service/app-service-web-tutorial-custom-domain.md) |
| Resourcemanager-VM 's |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |Resource Manager-virtuele machines kan openbare IP-adressen hebben. Een virtuele machine met een openbaar IP-adres kan ook worden achter een load balancer. U kunt een DNS-A of CNAME-record voor het openbare adres maken. Deze aangepaste naam kan worden gebruikt om over te slaan van de VIP-adres op de load balancer. |
| Klassieke VM's |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |Klassieke virtuele machines die zijn gemaakt met behulp van PowerShell of CLI kan worden geconfigureerd met een dynamisch of statisch (gereserveerd) virtuele-adres. U kunt een DNS CNAME of een record, respectievelijk maken. |
