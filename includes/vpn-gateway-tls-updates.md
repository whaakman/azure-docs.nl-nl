---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f2d7aba05fc01c5a4dcdb123f25242c4e4a72578
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "43426421"
---
>[!NOTE]
>Vanaf 1 juli 2018 is ondersteuning voor TLS 1.0 en 1.1 uit Azure VPN Gateway verwijderd. VPN Gateway ondersteunt alleen TLS 1.2. Om ondersteuning te behouden, Zie de [updates voor het inschakelen van ondersteuning voor TLS1.2](#tls1).

Bovendien worden de volgende oudere algoritmen ook afgeschaft voor TLS vanaf 1 juli 2018:

* RC4 (Rivest Cipher 4)
* DES (Data Encryption Algorithm)
* 3DES (Triple Data Encryption Algorithm)
* MD5 (Message Digest 5)

### <a name="tls1"></a>Hoe schakel ik ondersteuning voor TLS 1.2 in Windows 7 en Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
