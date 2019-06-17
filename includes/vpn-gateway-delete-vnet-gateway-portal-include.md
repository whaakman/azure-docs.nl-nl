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
ms.openlocfilehash: d36d2be59010c47348a8e196b28d87e5b967868e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66157419"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Stap 1: Navigeer naar de virtuele netwerkgateway

1. In de [Azure-portal](https://portal.azure.com), gaat u naar **alle resources**. 
2. Als u wilt openen op de pagina virtual network gateway, gaat u naar de virtuele netwerkgateway die u wilt verwijderen en klik erop.

### <a name="step-2-delete-connections"></a>Stap 2: Verbindingen verwijderen

1. Klik op de pagina voor uw virtuele netwerkgateway op **verbindingen** om alle verbindingen met de gateway weer te geven.
2. Klik op de **'...'** op de rij van de naam van de verbinding, selecteert u vervolgens **verwijderen** in de vervolgkeuzelijst.
3. Klik op **Ja** om te bevestigen dat u wilt verwijderen van de verbinding. Als u meerdere verbindingen hebt, verwijdert u elke verbinding.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Stap 3: De virtuele netwerkgateway verwijderen

Er rekening mee dat als u een P2S-configuratie in dit VNet naast de S2S-configuratie hebt, verwijderen van de gateway van virtueel netwerk automatisch de verbinding alle P2S-clients zonder waarschuwing verbroken wordt.

1. Klik op de pagina virtual network gateway **overzicht**.
2. Op de **overzicht** pagina, klikt u op **verwijderen** de gateway niet te verwijderen.
