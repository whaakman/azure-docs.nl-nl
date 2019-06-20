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
ms.openlocfilehash: 1199819d274590cc81d0234680f8765f9cc36c0a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175871"
---
### <a name="noconnection"></a>IP-adresvoorvoegsels wijzigen voor de gateway van een lokaal netwerk - geen gatewayverbinding

#### <a name="to-add-additional-address-prefixes"></a>Ga als volgt te werk om aanvullende voorvoegsels toe te voegen:

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **configuratie**.
2. Voeg de IP-adresruimte in de *aanvullend adresbereik toevoegen* vak.
3. Klik op **opslaan** uw instellingen op te slaan.

#### <a name="to-remove-address-prefixes"></a>Ga als volgt te werk om adresvoorvoegsels te verwijderen:

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **configuratie**.
2. Klik op de **'...'** op de regel met het voorvoegsel dat u wilt verwijderen.
3. Klik op **Verwijderen**.
4. Klik op **opslaan** uw instellingen op te slaan.

### <a name="withconnection"></a>IP-adresvoorvoegsels wijzigen voor de gateway van een lokaal netwerk - bestaande gatewayverbinding

Als u een gatewayverbinding hebt en u IP-adresvoorvoegsels wilt toevoegen aan of verwijderen uit uw lokale netwerkgateway, moet u de volgende stappen uitvoeren in de volgorde waarin ze staan vermeld. Dit veroorzaakt enige downtime in uw VPN-verbinding. Als u IP-adresvoorvoegsels wijzigt, hoeft u de VPN-gateway niet te verwijderen. U hoeft alleen de verbinding te verwijderen.

#### <a name="1-remove-the-connection"></a>1. Verwijder de verbinding.

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **verbindingen**.
2. Klik op de **...**  op de regel voor elke verbinding en klik vervolgens op **verwijderen**.
3. Klik op **opslaan** uw instellingen op te slaan.

#### <a name="2-modify-the-address-prefixes"></a>2. De adresvoorvoegsels wijzigen.

Ga als volgt te werk om aanvullende voorvoegsels toe te voegen:

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **configuratie**.
2. De IP-adresruimte toevoegen.
3. Klik op **opslaan** uw instellingen op te slaan.

Ga als volgt te werk om adresvoorvoegsels te verwijderen:

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **configuratie**.
2. Klik op de **...**  op de regel met het voorvoegsel dat u wilt verwijderen.
3. Klik op **Verwijderen**.
4. Klik op **opslaan** uw instellingen op te slaan.

#### <a name="3-recreate-the-connection"></a>3. Maak opnieuw verbinding.

1. Navigeer naar de virtuele netwerkgateway voor uw VNet. (Niet de lokale netwerkgateway.)
2. Op de Gateway van het virtuele netwerk, in de **instellingen** sectie, klikt u op **verbindingen**.
3. Klik op de **+ toevoegen** openen de **verbinding toevoegen** blade.
4. Uw verbinding opnieuw maakt.
5. Klik op **OK** om de verbinding te maken.