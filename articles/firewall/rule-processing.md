---
title: Azure Firewall-regel verwerkingslogica
description: Meer informatie over Azure firewallregel verwerking van logica
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/27/2018
ms.author: victorh
ms.openlocfilehash: 542682037a932a2e3b08c71b38b64b2694ad40f3
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228330"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Firewall-regel verwerkingslogica
Firewall van Azure heeft NAT-regels, netwerkregels en regels voor toepassingen. De regels worden verwerkt op basis van het regeltype.


## <a name="network-rules-and-applications-rules"></a>Netwerkregels voor en toepassingen 
Netwerkregels zijn toegepaste regels voor eerste en vervolgens de toepassing. De regels worden beëindigd. Dus als een overeenkomst wordt gevonden in netwerkregels, zijn klikt u vervolgens regels van toepassing niet verwerkt.  Als er geen overeenkomst van de regel netwerk, en als het protocol pakketten HTTP/HTTPS is, wordt het pakket wordt vervolgens geëvalueerd door de regels van toepassing. Als u nog steeds geen overeenkomst wordt gevonden, en vervolgens het pakket is geëvalueerd op basis van de infrastructuur-regelverzameling. Als er nog steeds geen overeenkomst, klikt u vervolgens het pakket is standaard niet toegestaan.

## <a name="nat-rules"></a>NAT-regels
Binnenkomende verbindingen kan worden ingeschakeld door het configureren van bestemming Network Address Translation (DNAT), zoals beschreven in [zelfstudie: binnenkomend verkeer filteren met de Azure-portal met behulp van Azure Firewall DNAT](tutorial-firewall-dnat.md). DNAT regels worden eerst toegepast. Als een overeenkomst wordt gevonden, wordt een impliciete bijbehorende netwerk-regel waarmee de vertaalde verkeer toegevoegd. U kunt dit gedrag negeren door expliciet toe te voegen een regelverzameling netwerk met regels voor weigeren die overeenkomen met de vertaalde verkeer. Er is geen toepassing regels worden toegepast voor deze verbindingen.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren en configureren van een Azure-Firewall](tutorial-firewall-deploy-portal.md).