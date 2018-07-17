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
ms.openlocfilehash: 9a17f34333503436d3da340670abdde154e45ef6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38727523"
---
Vanaf 1 juli 2018 is ondersteuning voor TLS 1.0 en 1.1 uit Azure VPN Gateway verwijderd. VPN Gateway ondersteunt alleen TLS 1.2. Om TLS-ondersteuning en connectiviteit te behouden voor uw punt-naar-site-clients met Windows 7 en Windows 8 die gebruikmaken van TLS, raden we u aan om de volgende updates te installeren:

•  [Update voor Microsoft EAP-implementatie die gebruikmaakt van TLS](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

•  [Update om TLS 1.1 en TLS 1.2 in te schakelen als standaardbeveiligingsprotocollen in WinHTTP](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

De volgende verouderde algoritmen worden ook vanaf 1 juli 2018 afgeschaft voor TLS:

* RC4 (Rivest Cipher 4)
* DES (Data Encryption Algorithm)
* 3DES (Triple Data Encryption Algorithm)
* MD5 (Message Digest 5)
