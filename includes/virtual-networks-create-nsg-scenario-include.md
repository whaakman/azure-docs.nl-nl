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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38760353"
---
## <a name="scenario"></a>Scenario
Dit document wordt u beter laten zien hoe u nsg's maken, het volgende scenario:

![VNet-scenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In dit scenario maakt u een NSG voor elk subnet in de **TestVNet** virtueel netwerk, als volgt te werk: 

* **NSG-FrontEnd**. De front-end-NSG wordt toegepast op de *FrontEnd* subnet, en bevat twee regels:    
  * **RDP-regel**. RDP-verkeer naar de *FrontEnd* subnet.
  * **Web-regel**. Hiermee kunt u HTTP-verkeer naar de *FrontEnd* subnet.
* **NSG-back-end**. De back-end-NSG wordt toegepast op de *back-end* subnet, en bevat twee regels:    
  * **SQL-regel**. SQL-verkeer alleen vanaf de *FrontEnd* subnet.
  * **Web-regel**. Alle internetverkeer van weigert de *back-end* subnet.

De combinatie van deze regels maken een DMZ-achtige-scenario, waarin de back-end-subnet kan alleen inkomend verkeer voor SQL ontvangen van het front-end-subnet, en heeft geen toegang tot het Internet, terwijl het front-end-subnet kan met Internet communiceren en ontvangen binnenkomende HTTP-aanvragen alleen.

