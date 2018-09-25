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
ms.openlocfilehash: 5d9886e16aa1921963f3d91d0fbd4da9287f7e54
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989138"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>De werking van Azure DNS met andere Azure-services

Azure DNS is een gehoste DNS-beheer en de naam resolution-service. Hiermee kunt u openbare DNS-namen voor de andere toepassingen en services die u hebt geïmplementeerd in Azure maakt. Het maken van een naam op voor een Azure-service in uw aangepaste domein is net zo eenvoudig als een record van het juiste type voor uw service toe te voegen.

* U kunt een DNS CNAME-record die wordt toegewezen aan de DNS-naam die Azure hebt gemaakt voor uw service maken voor dynamisch toegewezen IP-adressen. DNS-standaarden te voorkomen dat u met behulp van een CNAME-record voor het toppunt van de zone, maar u kunt een alias-record in plaats daarvan. Zie voor meer informatie, [zelfstudie: een alias-record om te verwijzen naar een openbare Azure-IP-adres configureren](tutorial-alias-pip.md).
* Voor statisch toegewezen IP-adressen, kunt u een DNS-A-record met behulp van een naam, met inbegrip van een *domein zonder voorvoegsel zijn* naam in het toppunt van de zone.

De volgende tabel geeft een overzicht van de ondersteunde recordtypen die kunnen worden gebruikt voor verschillende Azure-services. Zoals u in deze tabel zien kunt, ondersteunt Azure DNS DNS-records alleen voor internetgerichte-netwerkbronnen. Azure DNS kan niet worden gebruikt voor het omzetten van interne, persoonlijke adressen.

| Azure-service | Netwerkinterface | Beschrijving |
| --- | --- | --- |
| Application Gateway |[Front-end-openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A of CNAME-record maken. |
| Load Balancer |[Front-end-openbaar IP-adres](dns-custom-domain.md#public-ip-address)  |U kunt een DNS-A of CNAME-record maken. Load Balancer kan een openbare IPv6-IP-adres dat wordt dynamisch toegewezen hebben. Daarom moet u een CNAME-record voor een IPv6-adres maken. |
| Traffic Manager |Openbare-naam |U kunt een alias-record die wordt toegewezen aan de trafficmanager.net-naam toegewezen aan uw Traffic Manager-profiel maken. Zie voor meer informatie, [zelfstudie: een alias-record ter ondersteuning van het toppunt van domeinnamen met Traffic Manager configureert](tutorial-alias-tm.md). |
| Cloudservice |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A-record maken voor statisch toegewezen IP-adressen. Voor dynamisch toegewezen IP-adressen, moet u een CNAME-record die wordt toegewezen aan de *cloudapp.net* naam.|
| App Service | [Extern IP-adres](dns-custom-domain.md#app-service-web-apps) |U kunt een DNS-A-record maken voor externe IP-adressen. Anders moet u een CNAME-record die wordt toegewezen aan de naam van de azurewebsites.net maken. Zie voor meer informatie, [een aangepaste domeinnaam toewijzen aan een Azure-app](../app-service/app-service-web-tutorial-custom-domain.md) |
| Resourcemanager-VM 's |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |Resource Manager-virtuele machines kan openbare IP-adressen hebben. Een virtuele machine met een openbaar IP-adres kan ook worden achter een load balancer. U kunt een DNS-A-, CNAME- of alias-record voor het openbare adres maken. Deze aangepaste naam kan worden gebruikt om over te slaan van de VIP-adres op de load balancer. |
| Klassieke VM's |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |Klassieke virtuele machines die zijn gemaakt met behulp van PowerShell of CLI kan worden geconfigureerd met een dynamisch of statisch (gereserveerd) virtuele-adres. U kunt een DNS CNAME of een record, respectievelijk maken. |
