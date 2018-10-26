---
title: Azure DNS gebruiken met andere Azure-services | Microsoft Docs
description: Over het gebruik van Azure DNS om op te lossen namen voor andere Azure-services
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
ms.openlocfilehash: dcf209d2036d2686bea0b51380db3cd2473d04a6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094440"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>De werking van Azure DNS met andere Azure-services

Azure DNS is een gehoste DNS-beheer en de naam resolution-service. U kunt deze gebruiken om te maken van openbare DNS-namen voor andere toepassingen en services die u implementeert in Azure. Het maken van een naam op voor een Azure-service in uw aangepaste domein is eenvoudig. U kunt alleen een record van het juiste type toevoegen voor uw service.

* U kunt een DNS CNAME-record die wordt toegewezen aan de DNS-naam die Azure hebt gemaakt voor uw service maken voor dynamisch toegewezen IP-adressen. DNS-standaarden te voorkomen dat u met behulp van een CNAME-record voor het toppunt van de zone. U kunt een alias-record in plaats daarvan gebruiken. Zie voor meer informatie, [zelfstudie: een alias-record om te verwijzen naar een openbare Azure-IP-adres configureren](tutorial-alias-pip.md).
* Voor statisch toegewezen IP-adressen, kunt u een DNS-A-record maken met behulp van een willekeurige naam, waaronder een *domein zonder voorvoegsel zijn* naam in het toppunt van de zone.

De volgende tabel geeft een overzicht van de ondersteunde recordtypen die u voor verschillende Azure-services gebruiken kunt. Zoals in de tabel wordt weergegeven, ondersteunt Azure DNS DNS-records voor internetgerichte-netwerkbronnen. Azure DNS kan niet worden gebruikt voor het omzetten van interne, persoonlijke adressen.

| Azure-service | Netwerkinterface | Beschrijving |
| --- | --- | --- |
| Azure Application Gateway |[Front-end-openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A of CNAME-record maken. |
| Azure Load Balancer |[Front-end-openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A of CNAME-record maken. Load Balancer kan een IPv6-openbare IP-adres wordt dynamisch toegewezen hebben. Maak een CNAME-record voor een IPv6-adres. |
| Azure Traffic Manager |Openbare-naam |U kunt een alias-record die wordt toegewezen aan de trafficmanager.net-naam toegewezen aan uw Traffic Manager-profiel maken. Zie voor meer informatie, [zelfstudie: een alias-record ter ondersteuning van het toppunt van domeinnamen met Traffic Manager configureert](tutorial-alias-tm.md). |
| Azure Cloud Services |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |U kunt een DNS-A-record maken voor statisch toegewezen IP-adressen. Voor dynamisch toegewezen IP-adressen, moet u een CNAME-record die wordt toegewezen aan de *cloudapp.net* naam.|
| Azure App Service | [Extern IP-adres](dns-custom-domain.md#app-service-web-apps) |U kunt een DNS-A-record maken voor externe IP-adressen. Anders moet u een CNAME-record die wordt toegewezen aan de naam van de azurewebsites.net maken. Zie voor meer informatie, [een aangepaste domeinnaam toewijzen aan een Azure-app](../app-service/app-service-web-tutorial-custom-domain.md). |
| Virtuele machines in Azure Resource Manager |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |Resource Manager-virtuele machines kan openbare IP-adressen hebben. Een virtuele machine met een openbaar IP-adres kan ook achter een load balancer zijn. U kunt een DNS-A-, CNAME- of alias-record voor het openbare adres maken. U kunt deze aangepaste naam gebruiken om over te slaan van de VIP-adres op de load balancer. |
| Klassieke VM's |[Openbaar IP-adres](dns-custom-domain.md#public-ip-address) |Klassieke virtuele machines die zijn gemaakt met behulp van PowerShell of CLI kunnen worden geconfigureerd met een dynamisch of statisch (gereserveerd) virtuele-adres. U kunt een DNS CNAME of een A-record, respectievelijk maken. |
