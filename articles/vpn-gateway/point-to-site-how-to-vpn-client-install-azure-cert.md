---
title: Een clientcertificaat P2S installeren | Azure
description: Een Mac of Windows-client het certificaat voor verificatie via certificaat P2S installeren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager, azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: cherylmc
ms.openlocfilehash: bf2788fff64ab8b3a5ccf75b8a80f2bd5aba5151
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30317990"
---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Een clientcertificaat voor punt-naar-Site Azure certificaat verificatie verbindingen installeren

Alle clients die verbinding maken met een virtueel netwerk met Azure certificaatverificatie voor punt-naar-Site is een clientcertificaat vereist. In dit artikel helpt u bij het installeren van een clientcertificaat dat wordt gebruikt voor verificatie bij het verbinden met een VNet met P2S.

## <a name="generate"></a>Genereren en een clientcertificaat exporteren

U kunt een clientcertificaat van een basiscertificaat dat is gegenereerd met behulp van de oplossing van een CA voor ondernemingen of een zelfondertekend basiscertificaat genereren. Zie de [PowerShell](vpn-gateway-certificates-point-to-site.md) of [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) instructies voor het stappen. Na het clientcertificaten worden gegenereerd, door ze te exporteren als pfx-bestanden. Zorg dat u de volledige certificaatketen meegenomen bij het exporteren.

## <a name="installwin"></a>Certificaat - Windows installeren

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Certificaat - Mac installeren

Mac-VPN-clients worden ondersteund voor het Resource Manager-implementatiemodel alleen. Ze worden niet ondersteund voor het klassieke implementatiemodel.

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Volgende stappen

Ga door met de configuratiestappen voor punt-naar-Site.

* [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure Portal (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
