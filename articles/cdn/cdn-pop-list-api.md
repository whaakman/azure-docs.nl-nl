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
ms.openlocfilehash: 2b34f6bcbf37a48fb49a2a64d727fc9330b0d735
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882248"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>De huidige Verizon POP-lijst ophalen voor Azure CDN

U kunt de REST-API gebruiken om de set IP-adressen voor de Verizon aanwezigheid (POP)-servers te halen. Deze servers POP-aanvragen versturen naar bronservers die gekoppeld aan Azure Content Delivery Network (CDN)-eindpunten voor een profiel van Verizon zijn (**Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon**). Houd er rekening mee dat deze set IP-adressen af van de IP-adressen die een client wijkt bij aanvragen voor de POP's zou zien. 

Zie voor de syntaxis van de REST-API-bewerking voor het ophalen van de POP-lijst, [Edge-knooppunten - lijst](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Typische gebruiksscenario’s

Uit veiligheidsoverwegingen kunt u deze lijst IP om af te dwingen dat aanvragen naar uw bronserver bestaan alleen uit een geldige Verizon POP. Bijvoorbeeld, als iemand de hostnaam of IP-adres voor de bronserver een CDN-eindpunt wordt gedetecteerd, kan een aanvragen moet rechtstreeks naar de oorspronkelijke server, dus overslaan van de schaal- en beveiligingsmogelijkheden die worden geleverd door Azure CDN. Door in te stellen de IP-adressen in de lijst met resultaten als de enige toegestane IP-adressen op een bronserver, kan in dit scenario worden voorkomen. Om ervoor te zorgen dat u de meest recente POP-lijst hebt, deze ophalen ten minste één keer per dag. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de REST-API [REST-API van Azure CDN](https://docs.microsoft.com/rest/api/cdn/).
