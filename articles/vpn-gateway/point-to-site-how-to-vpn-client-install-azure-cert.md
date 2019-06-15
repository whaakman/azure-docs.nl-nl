---
title: 'Een certificaat voor punt-naar-Site installeren: Azure | Microsoft Docs'
description: Clientcertificaat voor P2S - verificatie van Windows, Mac en Linux installeren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: c278c1c85961fbeb0779cad98f8ac16d4961ba75
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60679955"
---
# <a name="install-client-certificates-for-p2s-certificate-authentication-connections"></a>Installeer de clientcertificaten voor P2S verbindingen met verificatie via certificaat

Alle clients die verbinding maken met een virtueel netwerk met behulp van Azure-certificaatverificatie van punt-naar-Site is een clientcertificaat vereist. Dit artikel helpt u bij het installeren van een clientcertificaat dat wordt gebruikt voor verificatie bij het verbinden met een VNet maken via P2S.

## <a name="generate"></a>Een certificaat verkrijgen

Ongeacht welke clientbesturingssysteem dat u verbinding wilt maken, moet u altijd een clientcertificaat hebben. U kunt een clientcertificaat genereren uit een basiscertificaat dat is gegenereerd met behulp van de oplossing van een CA voor ondernemingen, of een zelfondertekend basiscertificaat. Zie de [PowerShell](vpn-gateway-certificates-point-to-site.md), [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md), of [Linux](vpn-gateway-certificates-point-to-site-linux.md) instructies voor de stappen om een clientcertificaat te genereren. 

## <a name="installwin"></a>Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Mac

>[!NOTE]
>Mac-VPN-clients worden ondersteund voor het Resource Manager-implementatiemodel alleen. Ze worden niet ondersteund voor het klassieke implementatiemodel.
>
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="installlinux"></a>Linux

Het certificaat van de Linux-client is geïnstalleerd op de client als onderdeel van de configuratie van de client. Zie [clientconfiguratie - Linux](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli) voor instructies.

## <a name="next-steps"></a>Volgende stappen

Ga door met de stappen van de punt-naar-Site-configuratie voor het [Create and install VPN-clientconfiguratiebestanden](point-to-site-vpn-client-configuration-azure-cert.md).