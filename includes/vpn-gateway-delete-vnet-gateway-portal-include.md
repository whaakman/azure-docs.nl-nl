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
ms.openlocfilehash: 5b2aa7fedbc203c50796a0e0c8d9cdb3895ae6c1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Stap 1: Ga naar de virtuele netwerkgateway

1. In de [Azure-portal](https://portal.azure.com), gaat u naar **alle resources**. 
2. Open de pagina van de gateway virtuele netwerk, gaat u naar de virtuele netwerkgateway die u wilt verwijderen en klik erop.

### <a name="step-2-delete-connections"></a>Stap 2: Delete-verbindingen

1. Klik op de pagina voor uw virtuele netwerkgateway op **verbindingen** om weer te geven van alle verbindingen met de gateway.
2. Klik op de **'...'** op de rij van de naam van de verbinding, schakelt u **verwijderen** uit de vervolgkeuzelijst.
3. Klik op **Ja** om te bevestigen dat u wilt verwijderen van de verbinding. Als er meerdere verbindingen, verwijdert u elke verbinding.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Stap 3: De virtuele netwerkgateway verwijderen

Let wel dat als u een P2S-configuratie in dit VNet naast de S2S-configuratie hebt, verwijderen van de virtuele netwerkgateway automatisch alle P2S-clients zonder waarschuwing verbreekt.

1. Klik op de pagina van de gateway virtuele netwerk **overzicht**.
2. Op de **overzicht** pagina, klikt u op **verwijderen** de gateway wilt verwijderen.
