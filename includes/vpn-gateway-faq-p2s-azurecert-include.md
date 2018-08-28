---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 97d33bfcc8251b10ba121b7fb013800904450563
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "30197070"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basis-CA voor een punt-naar-site-verbinding gebruiken?

Ja. Voorheen konen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Welke hulpprogramma's kan ik gebruiken om certificaten te maken?

U kunt uw Enterprise PKI-oplossing (uw interne PKI), Azure PowerShell, MakeCert en OpenSSL gebruiken.

### <a name="certsettings"></a>Zijn er instructies voor het instellen van het certificaat en de parameters?

* **Interne PKI/Enterprise PKI-oplossing:** zie de stappen om [certificaten te genereren](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** zie het [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md)-artikel voor een stappenplan.

* **MakeCert:** zie het [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md)-artikel voor een stappenplan.

* **OpenSSL:** 

    * Bij het exporteren van certificaten, moet u het basiscertificaat naar Base64 converteren.

    * Voor het clientcertificaat:

      * Bij het maken van de persoonlijke sleutel, moet u de lengte 4096 opgeven.
      * Bij het maken van het certificaat moet u voor de paramter *-extensies* de waarde *usr_cert* opgeven.