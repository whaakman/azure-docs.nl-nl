---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 873549442284ede2e9f020bd90879f721b9c1a18
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31805201"
---
## <a name="scenario"></a>Scenario
Dit document gebruikt om beter te laten zien hoe u kunt nsg's maken, het volgende scenario:

![VNet-scenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In dit scenario maakt u een NSG voor elk subnet in de **TestVNet** virtueel netwerk, als volgt: 

* **NSG-FrontEnd**. De front-NSG wordt toegepast op de *FrontEnd* subnet, en bevat twee regels:    
  * **RDP-regel**. Hiermee kunt u RDP-verkeer naar de *FrontEnd* subnet.
  * **Web-regel**. Hiermee kunt u HTTP-verkeer naar de *FrontEnd* subnet.
* **NSG-back-end**. De back-end-NSG wordt toegepast op de *back-end* subnet, en bevat twee regels:    
  * **SQL-regel**. Kan SQL-verkeer alleen via de *FrontEnd* subnet.
  * **Web-regel**. Weigert u alle internet gebonden verkeer van de *back-end* subnet.

De combinatie van deze regels een DMZ-achtige scenario maken, waarbij het subnet voor back-end kan alleen inkomend verkeer ontvangen voor SQL van de front-end-subnet, en heeft geen toegang tot het Internet, terwijl het front-end-subnet kan met het Internet communiceren en ontvangen binnenkomende HTTP-aanvragen alleen.

