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
ms.openlocfilehash: ea616786d69d41435be2a46e90d4973b21270935
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
1. Navigeer naar en open de pagina voor uw virtuele netwerkgateway. Er zijn meerdere manieren om ernaar te navigeren. In het hier gebruikte voorbeeld is naar de gateway VNet1GW genavigeerd via: **TestVNet1 -> Overzicht -> Verbonden apparaten -> VNet1GW**.
2. Op de pagina voor VNet1GW klikt u op **Verbindingen**. Klik bovenaan de pagina Verbindingen op **+ Toevoegen** om de pagina **Verbinding toevoegen** te openen.

    ![Site-naar-Site-verbinding maken](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. Op de pagina **Verbinding toevoegen** vult u de waarden in die nodig zijn om verbinding te maken.

  - **Naam:** de naam van de verbinding. In het voorbeeld wordt gebruikgemaakt van de naam **VNet1toSite2**.
  - **Verbindingstype:** selecteer **Site-naar-site (IPsec)**.
  - **Virtuele netwerkgateway:** hiervoor geldt een vaste waarde, omdat u verbinding maakt vanaf deze gateway.
  - **Lokale netwerkgateway:** klik op **Een lokale netwerkgateway kiezen** en selecteer de lokale netwerkgateway die u wilt gebruiken. In het voorbeeld wordt gebruikgemaakt van **Site2**.
  - **Gedeelde sleutel:** de waarde hier moet overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. In het voorbeeld wordt abc123 gebruikt, maar u kunt een ingewikkeldere waarde gebruiken (aanbevolen). Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.
  - De resterende waarden voor **Abonnement**, **Resourcegroep**, en **Locatie** zijn vast.

4. Klik op **OK** om uw verbinding te maken. U ziet *Verbinding maken* op het scherm knipperen.
5. U kunt de verbinding bekijken op de pagina **Verbindingen** van de virtuele netwerkgateway. De status verandert van *Onbekend* in *Verbinding maken* en vervolgens in *Voltooid*.