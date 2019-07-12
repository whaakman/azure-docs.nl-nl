---
title: Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN | Microsoft Docs
description: Als u uw eigen certificaat gebruikt voor het inschakelen van HTTPS op een aangepast domein, moet u een toegestane certificeringsinstantie (CA) gebruiken om deze te maken.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 754941163ddce9512870f0b76a96207472e5b2aa
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593365"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN

Voor een aangepaste domein in Azure Content Delivery Network (CDN) op een **Azure CDN Standard van Microsoft** eindpunt, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), moet u een toegestane certificeringsinstantie (CA) voor het maken van uw SSL-certificaat. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

> [!NOTE]
> De mogelijkheid om uw eigen certificaat gebruikt om aangepaste HTTPS inschakelen is alleen beschikbaar bij **Azure CDN Standard van Microsoft** profielen. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

