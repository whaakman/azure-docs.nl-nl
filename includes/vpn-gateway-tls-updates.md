---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 383c4a53913333a3e25006980dd7533b9e243a4a
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236672"
---
Vanaf 1 juli 2018, wordt ondersteuning voor TLS 1.0 en 1.1 van Azure VPN-Gateway verwijderd. VPN-Gateway ondersteunt alleen TLS 1.2. Als u wilt behouden TLS-ondersteuning en connectiviteit voor uw Windows 7 en Windows 8 punt-naar-site-clients die gebruik van TLS, raden we aan dat u de volgende updates installeren:

• [Update voor Microsoft-EAP-implementatie het gebruik van TLS maakt](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

• [Update waarmee TLS 1.1 en TLS 1.2 als beveiligde standaardprotocollen in WinHTTP](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

De volgende oudere algoritmen wordt ook afgeschaft voor TLS op 1 juli 2018:

* RC4 (Rivest Cipher 4)
* DES (algoritme voor gegevensversleuteling)
* 3DES (Triple algoritme voor gegevensversleuteling)
* MD5 (Message Digest 5)
* SHA-1 (Secure Hash Algorithm 1)