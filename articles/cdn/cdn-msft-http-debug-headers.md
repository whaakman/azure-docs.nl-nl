---
title: Fout opsporing voor HTTP-headers voor Azure CDN van micro soft | Microsoft Docs
description: Debug cache-aanvraag headers bieden aanvullende informatie over het cache beleid dat wordt toegepast op de aangevraagde Asset. Deze headers zijn specifiek voor Azure CDN van micro soft.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: magattus
ms.openlocfilehash: 297c65c1cd89163b8663819f844dc6c2a83fd1bf
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814068"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Fout opsporing voor de HTTP-header voor Azure CDN van micro soft
De antwoord header `X-Cache`voor fout opsporing bevat details over de laag van de CDN-stack waaruit de inhoud afkomstig is. Deze header is specifiek voor Azure CDN van micro soft.

### <a name="response-header-format"></a>Indeling van de antwoord header

Header | Description
-------|------------
X-cache: TCP_HIT | Deze header wordt geretourneerd wanneer de inhoud wordt geleverd vanuit de cache van de CDN-rand. 
X-cache: TCP_REMOTE_HIT | Deze header wordt geretourneerd wanneer de inhoud wordt geleverd vanuit de regionale cache van de CDN (oorspronkelijke afschermings laag)
X-cache: TCP_MISS | Deze header wordt geretourneerd als er een cache-Missing is en de inhoud van de oorsprong wordt geleverd. 


