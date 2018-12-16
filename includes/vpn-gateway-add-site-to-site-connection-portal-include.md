---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 19ad4e39ca4e402c37b2cfa69c7c306b6e5a2766
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444078"
---
1. Navigeer naar en open de pagina voor uw virtuele netwerkgateway. Er zijn meerdere manieren om ernaar te navigeren. U kunt naar de gateway 'VNet1GW' navigeren via: **TestVNet1 -> Overzicht -> Verbonden apparaten -> VNet1GW**.
2. Op de pagina voor VNet1GW klikt u op **Verbindingen**. Klik bovenaan de pagina Verbindingen op **+ Toevoegen** om de pagina **Verbinding toevoegen** te openen.

   ![Site-naar-Site-verbinding maken](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. Op de pagina **Verbinding toevoegen** configureert u de waarden die nodig zijn om verbinding te maken.

   - **Naam:** Naam van de verbinding.
   - **Verbindingstype:** Selecteer **Site-to-site(IPSec)**.
   - **Gateway van virtueel netwerk:** De waarde is opgelost, omdat u verbinding vanaf deze gateway maakt.
   - **Lokale netwerkgateway:** Klik op **een lokale netwerkgateway kiezen** en selecteert u de lokale netwerkgateway die u wilt gebruiken.
   - **Gedeelde sleutel:** de waarde hier moet overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. In het voorbeeld wordt 'abc123' gebruikt, maar u kunt een ingewikkeldere waarde gebruiken (aanbevolen). Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.
   - De resterende waarden voor **Abonnement**, **Resourcegroep**, en **Locatie** zijn vast.

4. Klik op **OK** om uw verbinding te maken. U ziet *Verbinding maken* op het scherm knipperen.
5. U kunt de verbinding bekijken op de pagina **Verbindingen** van de virtuele netwerkgateway. De status verandert van *Onbekend* in *Verbinding maken* en vervolgens in *Voltooid*.
