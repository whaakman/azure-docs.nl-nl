---
title: Azure App Service-omgeving beheeradressen
description: Geeft een lijst van de beheeradressen die zijn gebruikt voor de opdracht een App Service Environment
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: ccompy
ms.openlocfilehash: 590679daff20f9c469fb8fcfcc0fbbad77f91b5b
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162847"
---
# <a name="app-service-environment-management-addresses"></a>Beheeradressen van App Service-omgeving

De App Service Environment (ASE) is een implementatie van de Azure App Service in een subnet in uw Azure Virtual Network (VNet).  De as-omgeving moet toegankelijk zijn vanaf de beheerlaag gebruikt door de Azure App Service.  Deze as-omgeving beheer van verkeer over de gebruiker beheerde netwerk wordt verzonden. Als dit verkeer wordt geblokkeerd of gerouteerde, wordt de as-omgeving worden onderbroken. Lees voor meer informatie over de afhankelijkheden van ASE-netwerken, [netwerken overwegingen en de App Service Environment][networking]. Voor algemene informatie over de as-omgeving, kunt u beginnen met [Inleiding tot App Service Environment][intro].

Dit document geeft een lijst van de App Service-bron-adressen voor beheer van verkeer naar de as-omgeving en dient twee belangrijke doelen.  

1. U kunt deze adressen gebruiken om te maken van Netwerkbeveiligingsgroepen om binnenkomend verkeer.  
2. U kunt routes maken met deze adressen ter ondersteuning van geforceerde tunnels implementaties. Lees voor meer informatie over het configureren van de as-omgeving werkt in een omgeving waar het uitgaande verkeer on-premises wordt verzonden, [uw ASE met geforceerde tunneling configureren][forcedtunnel]

Alle as-omgevingen hebben een openbare VIP die beheer van verkeer binnenkomt. Het binnenkomende beheerverkeer van deze adressen komen binnen uit poorten 454 en 455 op het openbare VIP-adres van de as-omgeving.  

## <a name="list-of-management-addresses"></a>Lijst met beheeradressen ##

| Regio | Adressen |
|--------|-----------|
| Alle openbare regio 's | 70.37.57.58, 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246 |
| Microsoft Azure Government (Fairfax of MAG) | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="get-your-management-addresses-from-api"></a>Je beheeradressen ophalen uit-API ##

U kunt de beheeradressen die met uw ASE met de volgende API-aanroep overeenkomen kunt weergeven.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

De API retourneert een JSON-document met inbegrip van alle inkomende-adressen voor de as-omgeving. De lijst met adressen bevat de beheeradressen het VIP-adres gebruikt door de as-omgeving en de adresbereik van de ASE-subnet zelf.  

Voor het aanroepen van de API met de [armclient](http://github.com/projectkudu/ARMClient) gebruik de volgende opdrachten, maar vervang in uw abonnements-ID, resourcegroep en de naam van de as-omgeving.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
