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
ms.openlocfilehash: 0dcb591db5f487a103feccf92ffa577a1cbf8b23
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30921424"
---
1. Navigeer naar en open de pagina voor uw virtuele netwerkgateway. Er zijn meerdere manieren om ernaar te navigeren. U kunt naar de gateway 'VNet1GW' navigeren via: **TestVNet1 -> Overzicht -> Verbonden apparaten -> VNet1GW**.
2. Op de pagina voor VNet1GW klikt u op **Verbindingen**. Klik bovenaan de pagina Verbindingen op **+ Toevoegen** om de pagina **Verbinding toevoegen** te openen.

  ![Site-naar-Site-verbinding maken](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. Op de pagina **Verbinding toevoegen** configureert u de waarden die nodig zijn om verbinding te maken.

  - **Naam:** de naam van de verbinding.
  - **Verbindingstype:** selecteer **Site-naar-site (IPsec)**.
  - **Virtuele netwerkgateway:** hiervoor geldt een vaste waarde, omdat u verbinding maakt vanaf deze gateway.
  - **Lokale netwerkgateway:** klik op **Een lokale netwerkgateway kiezen** en selecteer de lokale netwerkgateway die u wilt gebruiken.
  - **Gedeelde sleutel:** de waarde hier moet overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. In het voorbeeld wordt 'abc123' gebruikt, maar u kunt een ingewikkeldere waarde gebruiken (aanbevolen). Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.
  - De resterende waarden voor **Abonnement**, **Resourcegroep**, en **Locatie** zijn vast.

4. Klik op **OK** om uw verbinding te maken. U ziet *Verbinding maken* op het scherm knipperen.
5. U kunt de verbinding bekijken op de pagina **Verbindingen** van de virtuele netwerkgateway. De status verandert van *Onbekend* in *Verbinding maken* en vervolgens in *Voltooid*.