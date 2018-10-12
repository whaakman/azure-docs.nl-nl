---
title: De huidige Verizon POP-lijst ophalen voor Azure CDN | Microsoft Docs
description: Leer hoe u de huidige Verizon POP-lijst ophalen met behulp van de REST-API.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 8a11f6cb634ca8986b463d815bd75e03b79b5706
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093983"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>De huidige Verizon POP-lijst ophalen voor Azure CDN

U kunt de REST-API gebruiken om de set IP-adressen voor de Verizon aanwezigheid (POP)-servers te halen. Deze servers POP-aanvragen versturen naar bronservers die gekoppeld aan Azure Content Delivery Network (CDN)-eindpunten voor een profiel van Verizon zijn (**Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon**). Houd er rekening mee dat deze set IP-adressen af van de IP-adressen die een client wijkt bij aanvragen voor de POP's zou zien. 

Zie voor de syntaxis van de REST-API-bewerking voor het ophalen van de POP-lijst, [Edge-knooppunten - lijst](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Typische gebruiksscenario’s

Uit veiligheidsoverwegingen kunt u deze lijst IP om af te dwingen dat aanvragen naar uw bronserver bestaan alleen uit een geldige Verizon POP. Bijvoorbeeld, als iemand de hostnaam of IP-adres voor de bronserver een CDN-eindpunt wordt gedetecteerd, kan een aanvragen moet rechtstreeks naar de oorspronkelijke server, dus overslaan van de schaal- en beveiligingsmogelijkheden die worden geleverd door Azure CDN. Door in te stellen de IP-adressen in de lijst met resultaten als de enige toegestane IP-adressen op een bronserver, kan in dit scenario worden voorkomen. Om ervoor te zorgen dat ze hebt u de meest recente POP-lijst, deze ten minste één keer per dag ophalen. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de REST-API [REST-API van Azure CDN](https://docs.microsoft.com/rest/api/cdn/).