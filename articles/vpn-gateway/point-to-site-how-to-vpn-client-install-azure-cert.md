---
title: Een clientcertificaat P2S installeren | Azure
description: Een Mac of Windows-client het certificaat voor verificatie via certificaat P2S installeren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: de98201b65f5531f334aded1056f622cecb6e190
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Een clientcertificaat voor punt-naar-Site Azure certificaat verificatie verbindingen installeren

Alle clients die verbinding maken met een virtueel netwerk met Azure certificaatverificatie voor punt-naar-Site is een clientcertificaat vereist. In dit artikel helpt u bij het installeren van een clientcertificaat dat wordt gebruikt voor verificatie bij het verbinden met een VNet met P2S.

## <a name="generate"></a>Genereren en een clientcertificaat exporteren

U kunt een clientcertificaat van een basiscertificaat dat is gegenereerd met behulp van de oplossing van een CA voor ondernemingen of een zelfondertekend basiscertificaat genereren. Zie de [PowerShell](vpn-gateway-certificates-point-to-site.md) of [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) instructies voor het stappen. Na het clientcertificaten worden gegenereerd, door ze te exporteren als pfx-bestanden. Zorg dat u de volledige certificaatketen meegenomen bij het exporteren.

## <a name="installwin"></a>Een certificaat installeren op Windows-clients

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Een certificaat installeren op Mac-clients

Mac-VPN-clients worden ondersteund voor het Resource Manager-implementatiemodel alleen. Ze worden niet ondersteund voor het klassieke implementatiemodel.

> [!NOTE]
>  IKEv2 is momenteel beschikbaar als preview-product.
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Volgende stappen

Ga door met de configuratiestappen voor punt-naar-Site.

* [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure Portal (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)