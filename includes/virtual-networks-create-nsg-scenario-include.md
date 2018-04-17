---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 09c6871fc5243296da2f2defd594afb80c62ac95
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
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

