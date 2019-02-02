---
title: 'Genereren en exporteren van certificaten voor punt-naar-Site: Linux: CLI: Azure | Microsoft Docs'
description: Een zelfondertekend basiscertificaat maken, de openbare sleutel exporteren en clientcertificaten genereren via de Linux (strongSwan) CLI.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/31/2019
ms.author: cherylmc
ms.openlocfilehash: b673be47d4951adab08f04efc56410095f549356
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564784"
---
# <a name="generate-and-export-certificates"></a>Certificaten genereren en exporteren

Punt-naar-Site-verbindingen gebruiken certificaten om te verifiëren. Dit artikel ziet u hoe u een zelfondertekend basiscertificaat maken en clientcertificaten genereren via de Linux-CLI en strongSwan. Als u naar ander certificaat-instructies zoekt, Zie de [Powershell](vpn-gateway-certificates-point-to-site.md) of [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) artikelen. Zie voor informatie over het installeren van strongSwan met behulp van de gebruikersinterface in plaats van de CLI de stappen in de [clientconfiguratie](point-to-site-vpn-client-configuration-azure-cert.md#install) artikel.

## <a name="generate-and-export"></a>Genereren en exporteren
[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Volgende stappen

Ga door met de punt-naar-Site-configuratie met [Create and install VPN-clientconfiguratiebestanden](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
