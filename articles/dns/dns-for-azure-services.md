---
title: Met andere Azure-services met behulp van Azure DNS | Microsoft Docs
description: Begrijpen hoe u Azure DNS gebruiken voor het omzetten van de naam van een andere Azure-services
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: gwallace
ms.openlocfilehash: a286508fe445208b6bb348d07434b5722cc3f11e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>De werking van Azure DNS met andere Azure-services

Azure DNS is een gehoste DNS-beheer en name resolution-service. Hiermee kunt u het maken van openbare DNS-namen voor de andere toepassingen en services die u hebt geïmplementeerd in Azure. Maken van een naam voor een Azure-service in uw aangepaste domein is net zo eenvoudig als het toevoegen van een record van het juiste type voor uw service.

* Voor dynamisch toegewezen IP-adressen, moet u een DNS CNAME-record dat is toegewezen aan de DNS-naam die Azure voor uw service gemaakt maken. DNS-standaarden voorkomen dat u een CNAME-record gebruiken voor het toppunt van de zone.
* Voor de statisch toegewezen IP-adressen, kunt u een DNS A-record met behulp van een naam, met inbegrip van een *Open domein* naam in het toppunt van de zone.

De volgende tabel bevat een overzicht van de ondersteunde typen die kunnen worden gebruikt voor verschillende Azure-services. Als u in deze tabel zien kunt, ondersteunt Azure DNS alleen DNS-records voor internetgerichte netwerkbronnen. Azure DNS, kan niet worden gebruikt voor naamomzetting van interne, particuliere adressen.

| Azure-service | Netwerkinterface | Beschrijving |
| --- | --- | --- |
| Application Gateway |[Front-openbare IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS A of CNAME-record maken. |
| Load Balancer |[Front-openbare IP-adres](dns-custom-domain.md#public-ip-address)  |U kunt een DNS A of CNAME-record maken. Load Balancer kan een IPv6-openbare IP-adres wordt dynamisch toegewezen hebben. Daarom moet u een CNAME-record voor een IPv6-adres maken. |
| Traffic Manager |Openbare naam |U kunt alleen een CNAME dat is toegewezen aan de trafficmanager.net-naam toegewezen aan uw Traffic Manager-profiel maken. Zie voor meer informatie [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-overview.md#traffic-manager-example). |
| Cloudservice |[Openbare IP-adres](dns-custom-domain.md#public-ip-address) |Voor de statisch toegewezen IP-adressen, kunt u een DNS A-record maken. Voor dynamisch toegewezen IP-adressen, moet u een CNAME-record dat is toegewezen aan de *cloudapp.net* naam. Deze regel geldt voor virtuele machines in de klassieke portal worden gemaakt omdat ze zijn geïmplementeerd als een cloudservice. Zie voor meer informatie [een aangepaste domeinnaam configureren in Cloudservices](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App Service | [Extern IP-adres](dns-custom-domain.md#app-service-web-apps) |U kunt een DNS A-record maken voor het externe IP-adressen. Anders moet u een CNAME-record dat is toegewezen aan de naam van de azurewebsites.net maken. Zie voor meer informatie [een aangepaste domeinnaam toewijzen aan een Azure-app](../app-service/app-service-web-tutorial-custom-domain.md) |
| Resource Manager virtuele machines |[Openbare IP-adres](dns-custom-domain.md#public-ip-address) |Resource Manager virtuele machines kunnen openbare IP-adressen hebben. Een virtuele machine met een openbare IP-adres kan ook worden achter een load balancer. U kunt een DNS A of CNAME-record maken voor het openbare adres. Deze aangepaste naam kan worden gebruikt voor het overslaan van de VIP op de load balancer. |
| Klassieke VM's |[Openbare IP-adres](dns-custom-domain.md#public-ip-address) |Klassieke virtuele machines gemaakt met behulp van PowerShell of CLI kan worden geconfigureerd met een dynamisch of statisch (gereserveerd) virtueel adres. U kunt een DNS CNAME of een record respectievelijk maken. |
