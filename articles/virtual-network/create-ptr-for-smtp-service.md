---
title: Zones voor reverse lookup voor een SMTP-koptekst controle configureren in Azure | Microsoft Docs
description: Hierin wordt beschreven hoe u zones voor reverse lookup voor een SMTP-koptekst controle configureren in Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: WillChen
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 02/06/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 1e95b00ea08105238a860265e46275c24ed7bfbd
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2018
ms.locfileid: "29151871"
---
#  <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Zones voor reverse lookup voor een SMTP-koptekst controle configureren

Dit artikel wordt beschreven hoe u een zone voor reverse in Azure DNS en een omgekeerde DNS PTR-record maken voor het controleren van SMTP-koptekst. 

## <a name="symptom"></a>Symptoom

Als u een SMTP-server in Microsoft Azure host, verschijnt het volgende foutbericht weergegeven wanneer verzenden of ontvangen van een bericht van de externe e-mailservers:

**554: geen PTR-Record** 

## <a name="solution"></a>Oplossing

Voor een virtueel IP-adres in Azure, worden de omgekeerde records gemaakt in Microsoft die eigendom zijn van de domein-zones, niet aangepast domein zones.

Wilt configureren PTR-records in Microsoft die eigendom zijn van zones, gebruikt u de ReverseFqdn - eigenschap op de PublicIpAddress-resource. Zie voor meer informatie [configureren omgekeerde DNS voor services die worden gehost in Azure](../dns/dns-reverse-dns-for-azure-services.md). 

Wanneer u de PTR-records configureert, zorg dat het IP-adres en de omgekeerde FQDN-naam zijn eigendom van het abonnement. Als u een omgekeerde FQDN-naam die geen deel van het abonnement uitmaken wordt ingesteld probeert, wordt het volgende foutbericht weergegeven:

    Set-AzureRmPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership: 
                        
    1) ReverseFqdn komt overeen met de FQDN-naam van een openbaar IP-resource onder het abonnement; 
    2) ReverseFqdn wordt omgezet naar de fqdn (via CNAME-records) van een openbaar IP-bron op onder hoort bij het abonnement; 
    3) Dit wordt omgezet naar het IP-adres (via CName en A-records) van een statische openbare IP-bron op onder het abonnement. 

Als u handmatig wijzigen uw SMTP-koptekst overeenkomt met onze reverse FQDN-naam, de externe e-mailserver kan nog steeds mislukt omdat het de banner SMTP-host overeenkomen met de MX-record voor het domein kan verwachten.