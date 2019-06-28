---
title: Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN | Microsoft Docs
description: Als u uw eigen certificaat gebruikt voor het inschakelen van HTTPS op een aangepast domein, moet u een toegestane certificeringsinstantie (CA) gebruiken om deze te maken.
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
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 8ce141fbf3d767159d16495bff5a93d791224c17
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331884"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN

Voor een aangepaste domein in Azure Content Delivery Network (CDN) op een **Azure CDN Standard van Microsoft** eindpunt, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), moet u een toegestane certificeringsinstantie (CA) voor het maken van uw SSL-certificaat. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

> [!NOTE]
> De mogelijkheid om uw eigen certificaat gebruikt om aangepaste HTTPS inschakelen is alleen beschikbaar bij **Azure CDN Standard van Microsoft** profielen. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

