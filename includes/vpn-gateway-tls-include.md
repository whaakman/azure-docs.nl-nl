---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/27/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1bbb56f819bccaa032f25e43b197e31a6a0730b7
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "43426426"
---
1. Open een opdrachtprompt met verhoogde bevoegdheden met de rechtermuisknop op **opdrachtprompt** en selecteren **als administrator uitvoeren**.
2. Voer de volgende opdrachten in de opdrachtprompt:

  ```
  reg add HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13 /v TlsVersion /t REG_DWORD /d 0xfc0
  reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" /v DefaultSecureProtocols /t REG_DWORD /d 0xaa0
  if %PROCESSOR_ARCHITECTURE% EQU AMD64 reg add "HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" /v DefaultSecureProtocols /t REG_DWORD /d 0xaa0
  ```

3. Installeer de volgende updates:
  
  * [KB3140245](https://www.catalog.update.microsoft.com/search.aspx?q=kb3140245)
  * [KB2977292](https://www.microsoft.com/en-us/download/details.aspx?id=44342)

4. Start de computer opnieuw op.
5. Verbinding maken met de VPN-verbinding.
