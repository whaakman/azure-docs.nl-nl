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
ms.openlocfilehash: d9825ea41937dc9436fe8b465b48b378e13407c1
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444073"
---
1. In de portal klikt u via **Alle resources** op **+Toevoegen**.
2. In de **Alles** pagina zoekvak, type **lokale netwerkgateway**, klik vervolgens om te retourneren van een lijst met resources. Klik op **Lokale netwerkgateway** om de pagina te openen en klik op **Maken** om de pagina **Lokale netwerkgateway maken** te openen.

   ![maak een lokale netwerkgateway](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)
3. Op de **pagina Lokale netwerkgateway maken** geeft u de waarden voor uw lokale netwerkgateway op.

   - **Naam:** Geef een naam voor uw lokale netwerkgateway-object. Gebruik indien mogelijk iets intuïtieve, zoals **ClassicVNetLocal** of **TestVNet1Local**. Dit maakt het eenvoudiger voor u bij het identificeren van de lokale netwerkgateway in de portal.
   - **IP-adres:** Geef een geldig openbaar **IP-adres** voor de VPN-apparaat of de virtuele netwerkgateway waarmee u verbinding wilt maken.

     * **Als dit lokale netwerk een on-premises locatie vertegenwoordigt:** Geef het openbare IP-adres van het VPN-apparaat waarmee u verbinding wilt maken. Het mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure.
     * **Als dit lokale netwerk een ander VNet vertegenwoordigt:** Geef het openbare IP-adres dat is toegewezen aan de virtuele netwerkgateway voor dit VNet.
     * **Als u nog het IP-adres hebt:** U kunt gezamenlijk een geldige tijdelijke aanduiding voor IP-adres, en vervolgens keert u terug en wijzig deze instelling voordat u verbinding maakt.
   - **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat het hier opgegeven bereiken niet overlappen met bereiken van andere netwerken waarmee u verbinding maken.
   - **BGP-instellingen configureren:** Gebruik alleen bij het configureren van BGP. Selecteer deze anders niet.
   - **Abonnement:** Controleer of dat het juiste abonnement wordt weergegeven.
   - **Resourcegroep:** Selecteer de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
   - **Locatie:** Selecteer de locatie waarin dit object wordt gemaakt. U kunt dezelfde locatie selecteren als die van uw VNet, maar dat is niet vereist.

4. Klik op **Maken** om de lokale netwerkgateway te maken.
