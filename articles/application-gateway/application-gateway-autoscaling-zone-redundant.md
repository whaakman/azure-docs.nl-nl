---
title: Automatisch schalen en Zone-redundante Application Gateway in Azure (openbare Preview) | Microsoft Docs
description: In dit artikel bevat informatie over web application firewall-aanvraaglimieten grootte en uitsluitingslijsten in Application Gateway met Azure portal.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: victorh
ms.openlocfilehash: 8fb3dce108b59b8df0d330ec642365d2487eae35
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085458"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Automatisch schalen en zoneredundante toepassingsgateway (openbare Preview)

Application Gateway en Web Application Firewall (WAF) zijn nu beschikbaar in openbare preview-versie onder een nieuwe v2-SKU die biedt verbeterde prestaties en voegt ondersteuning toe voor belangrijke nieuwe functies zoals automatisch schalen, zoneredundantie en ondersteuning voor statische VIP's. Bestaande functies onder de algemeen beschikbare SKU's worden nog steeds ondersteund in de nieuwe v2-SKU, met een paar uitzonderingen die worden vermeld in de sectie bekende beperkingen. De nieuwe v2-SKU's zijn onder andere de volgende verbeteringen:

- **Automatisch schalen**: Application Gateway of WAF implementaties voor de automatische schaalaanpassing SKU kan omhoog of omlaag op basis schalen van het wijzigen van verkeer patronen laden. Automatisch schalen heft ook de vereiste op om tijdens het inrichten een implementatiegrootte of het aantal instanties te kiezen. Daarom kan biedt de SKU flexibiliteit van de waarde true. Application Gateway kan in de nieuwe SKU werken zowel in vaste capaciteit (automatisch schalen uitgeschakeld), evenals in de modus voor automatisch schalen ingeschakeld. Modus vaste capaciteit is handig voor scenario's met consistente en voorspelbare workloads. Modus voor automatisch schalen is het nuttig zijn in toepassingen die veel afwijkingen in het toepassingsverkeer te zien.
   
   > [!NOTE]
   > Automatisch schalen is momenteel niet beschikbaar voor de WAF-SKU. WAF configureren met vaste capaciteit modus, in plaats van de modus voor automatisch schalen.
- **Zone redundantie**: An Application Gateway- of WAF-implementatie kan meerdere Beschikbaarheidszones, hoeft u te richten en te draaien afzonderlijke Application Gateway-instanties in elke zone met een Traffic Manager omvatten. U kunt een enkele zone of meerdere zones waarop Application Gateway-instanties zijn geïmplementeerd, duidt dit erop toezien dat fout zoneflexibiliteit. De back-endpool voor toepassingen kan op dezelfde manier worden verdeeld in meerdere beschikbaarheidszones.
- **Prestatieverbeteringen**: de automatische schaalaanpassing SKU biedt maximaal 5 X betere SSL-offload voor prestaties in vergelijking met de algemeen beschikbare SKU's.
- **Snellere implementatie en update** de automatische schaalaanpassing SKU biedt snellere implementatie en update in vergelijking met de algemeen beschikbare SKU's.
- **Statische VIP**: de toepassingsgateway VIP biedt nu ondersteuning voor de statische VIP-type exclusief. Dit zorgt ervoor dat het VIP-adres dat aan Application Gateway is gekoppeld niet verandert, zelfs niet nadat het systeem opnieuw is opgestart.

> [!IMPORTANT]
> De SKU met automatisch schalen en zoneredundantie voor toepassingsgateways is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Ondersteunde regio’s
AutoScaling SKU is beschikbaar in VS-Oost 2, VS centraal, West vs2, Frankrijk-centraal, West-Europa en Zuidoost-Azië.

## <a name="pricing"></a>Prijzen
Er zijn geen kosten tijdens Preview-versie. U wordt gefactureerd voor andere resources dan application-gateway, zoals Key Vault, virtuele machines, enzovoort. 

## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen

|Probleem|Details|
|--|--|
|Certificaat voor clientverificatie|Wordt niet ondersteund.<br>Zie voor meer informatie, [overzicht van end-to-end SSL met Application Gateway](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Met een combinatie van Standard_v2 en Standard Application Gateway in hetzelfde subnet|Wordt niet ondersteund.<br>Als automatisch schalen is ingeschakeld, kan één subnet bovendien slechts één application gateway hebben.|
|Gebruiker gedefinieerde Route (UDR) op Application Gateway-subnet|Niet ondersteund|
|NSG voor binnenkomende poortbereik| -65200 tot en met 65535 voor Standard_v2 SKU<br>-65503 tot 65534 voor standaard-SKU.<br>Zie voor meer informatie de [Veelgestelde vragen over](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Prestatielogboeken in Azure diagnostics|Wordt niet ondersteund.<br>Metrische gegevens van Azure moet worden gebruikt.|
|Billing|Er is momenteel geen facturering.|
|FIPS-modus, WebSocket|Deze worden momenteel niet ondersteund.|
|Alleen ILB-modus|Dit wordt momenteel niet ondersteund. Openbare en samen ILB-modus wordt ondersteund.|
|Web application firewall automatisch schalen|WAF biedt geen ondersteuning voor automatisch schalen-modus. Modus vaste capaciteit wordt ondersteund.|

## <a name="next-steps"></a>Volgende stappen
- [Een automatisch schalen, zoneredundante toepassingsgateway maken met een gereserveerd virtueel IP-adres met behulp van Azure PowerShell](tutorial-autoscale-ps.md)
- Meer informatie over [Application Gateway](overview.md).
- Meer informatie over [Azure Firewall](../firewall/overview.md). 

