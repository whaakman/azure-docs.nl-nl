---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 72e61a36b58c0bc666f3e19b71fb1abe842208f5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111616"
---
1. Aanmelden bij de Azure portal en selecteer **een resource maken**. De **nieuw** pagina wordt geopend.

2. In de **zoeken naar het veld marketplace**, voer *virtuele netwerkgateway*, en selecteer **gateway van virtueel netwerk** uit de lijst. 

3. Op de **gateway van virtueel netwerk** weergeeft, schakelt **maken** openen de **virtuele netwerkgateway maken** pagina.

   ![Velden van de pagina Virtuele netwerkgateway maken](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Velden van de pagina Virtuele netwerkgateway maken")

4. Op de **virtuele netwerkgateway maken** pagina, vult u de waarden voor de gateway van uw virtuele netwerk:

   - **Naam**: Voer een naam voor de gateway-object dat u maakt. Deze naam is anders dan de naam van het gateway-subnet. 

   - **Gateway van het type**: Selecteer **VPN** voor VPN-gateways. 

   - **VPN-type**selecteer het VPN-type dat wordt opgegeven voor uw configuratie. De meeste configuraties vereisen een **op Route gebaseerde** VPN-type.

   - **SKU**: selecteer de gateway-SKU in de vervolgkeuzelijst. Welke SKU's worden weergegeven in de vervolgkeuzelijst, is afhankelijk van het VPN-type dat u selecteert. Zie [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor informatie over gateway-SKU's.

      Selecteer alleen **de modus actief-actief inschakelen** als u een gatewayconfiguratie actief / actief-maakt. Anders selecteert u deze instelling niet.
  
   - **Locatie**: moet u mogelijk scrollen om te zien **locatie**. Stel **locatie** naar de locatie waar uw virtuele netwerk zich bevindt. Bijvoorbeeld, **VS-West**. Als u niet de locatie ingesteld op de regio waar uw virtuele netwerk zich bevindt, niet wordt weergegeven in de vervolgkeuzelijst wanneer u een virtueel netwerk selecteert.

   - **Virtueel netwerk**: kies het virtuele netwerk waaraan u deze gateway wilt toevoegen. Selecteer **virtueel netwerk** openen de **virtueel netwerk kiezen** pagina en selecteer het VNet. Als u uw VNet niet ziet, controleert u of de **locatie** veld is ingesteld op de regio waarin het virtuele netwerk zich bevindt.

   - **Adresbereik gatewaysubnet**: alleen ziet u deze instelling als u een gateway-subnet eerder hebt gemaakt voor het virtuele netwerk. Als u een geldig gatewaysubnet hebt gemaakt, wordt deze instelling wordt niet weergegeven.

   - **Openbare IP-adres**: deze instelling bepaalt u het openbare IP-adres-object, dat is gekoppeld aan de VPN-gateway. Het openbare IP-adres wordt dynamisch toegewezen aan dit object wanneer de VPN-gateway wordt gemaakt. De VPN-gateway ondersteunt momenteel alleen *dynamische* openbare IP-adrestoewijzing. Echter, dynamische toewijzing betekent niet dat het IP-adres verandert nadat deze is toegewezen aan uw VPN-gateway. De enige keer dat het openbare IP-adres verandert is wanneer de gateway wordt verwijderd en opnieuw gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.
    
      - Laat **Nieuwe maken** geselecteerd.

      - Voer een naam voor uw openbare IP-adres in het tekstvak.

   - **ASN van BGP configureren**: laat deze instelling wordt uitgeschakeld, tenzij uw configuratie specifiek vereist. Als u deze instelling nodig hebt, is de standaardinstelling ASN *65515*, die u kunt wijzigen.
     
5. Controleer de instellingen en selecteer **maken** om te beginnen met het maken van de VPN-gateway. De instellingen worden gevalideerd en u ziet de **implementeren van virtuele netwerkgateway** tegel op het dashboard. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

6. Nadat u de gateway maakt, Controleer of het IP-adres dat is toegewezen aan het door het virtuele netwerk in de portal weer te geven. De gateway wordt weergegeven als verbonden apparaat. U kunt het verbonden apparaat (uw virtuele netwerkgateway) voor meer informatie.