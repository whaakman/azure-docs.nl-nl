---
title: Ophalen van de huidige lijst van Verizon POP voor Azure CDN | Microsoft Docs
description: Informatie over het ophalen van de huidige lijst van Verizon pop-server met behulp van de REST-API.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: f796d18a03e14fdf652af72366762e1365523f09
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754553"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>De huidige lijst van Verizon POP voor Azure CDN ophalen

U kunt de REST-API voor het ophalen van de reeks IP-adressen voor de punt van Verizon aanwezigheid (POP)-servers. Deze POP-servers aanvragen indienen naar oorsprong-servers die gekoppeld aan Azure Content Delivery Network (CDN) eindpunten voor een profiel van Verizon zijn (**Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon**). Houd er rekening mee dat deze reeks IP-adressen van de IP-adressen die een client verschilt bij aanvragen voor de POP's zou zien. 

Zie voor de syntaxis van de REST-API-bewerking voor het ophalen van de POP-lijst, [Edge-knooppunten - lijst](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Typische gebruiksscenario’s

Uit veiligheidsoverwegingen kunt u deze lijst met IP om af te dwingen dat aanvragen met de oorspronkelijke server afkomstig zijn alleen van een geldige Verizon POP. Als iemand de hostnaam of IP-adres voor de bronserver een CDN-eindpunt wordt gedetecteerd, kan een aanvragen zorg rechtstreeks op de bronserver, dus overslaan van de schaal- en beveiligingsmogelijkheden verstrekt door Azure CDN. Door het instellen van de IP-adressen in de geretourneerde lijst als de enige toegestane IP-adressen op een bronserver, kunt u dit scenario voorkomen. U hebt de meest recente lijst van het pop-server ophalen van ten minste eenmaal per dag om ze te controleren. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de REST-API [REST-API van Azure CDN](https://docs.microsoft.com/en-us/rest/api/cdn/).