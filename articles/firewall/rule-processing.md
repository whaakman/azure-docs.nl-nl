---
title: Azure Firewall-regel verwerkingslogica
description: Meer informatie over Azure firewallregel verwerking van logica
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/27/2018
ms.author: victorh
ms.openlocfilehash: 12d86793c0d75413559aad77c558c4adb7ac91af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64681594"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Firewall-regel verwerkingslogica
Firewall van Azure heeft NAT-regels, netwerkregels en regels voor toepassingen. De regels worden verwerkt op basis van het regeltype.


## <a name="network-rules-and-applications-rules"></a>Netwerkregels voor en toepassingen 
Netwerkregels zijn toegepaste regels voor eerste en vervolgens de toepassing. De regels worden beëindigd. Dus als een overeenkomst wordt gevonden in netwerkregels, zijn klikt u vervolgens regels van toepassing niet verwerkt.  Als er geen overeenkomst van de regel netwerk, en als het protocol pakketten HTTP/HTTPS is, wordt het pakket wordt vervolgens geëvalueerd door de regels van toepassing. Als u nog steeds geen overeenkomst wordt gevonden, en vervolgens het pakket is geëvalueerd op basis van de infrastructuur-regelverzameling. Als er dan nog steeds geen overeenkomende regel is, wordt het pakket standaard afgewezen.

## <a name="nat-rules"></a>NAT-regels
Binnenkomende verbindingen kan worden ingeschakeld door het configureren van bestemming Network Address Translation (DNAT), zoals beschreven in [zelfstudie: Binnenkomend verkeer filteren met de Azure-portal met behulp van Azure Firewall DNAT](tutorial-firewall-dnat.md). DNAT regels worden eerst toegepast. Als een overeenkomst wordt gevonden, wordt een impliciete bijbehorende netwerk-regel waarmee de vertaalde verkeer toegevoegd. U kunt dit gedrag overschrijven door expliciet een verzameling netwerkregels toe te voegen met regels voor weigeren die overeenkomen met het omgezette verkeer. Er is geen toepassing regels worden toegepast voor deze verbindingen.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren en configureren van een Azure-Firewall](tutorial-firewall-deploy-portal.md).