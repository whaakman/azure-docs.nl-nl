---
title: Overzicht van de SSL-beleid voor Azure Application Gateway | Microsoft Docs
description: Meer informatie over hoe Azure Application Gateway kunt u SSL-beleid configureren
services: application gateway
documentationcenter: na
author: amsriva
manager: ''
editor: ''
tags: azure resource manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure services
ms.date: 08/03/2017
ms.author: amsriva
ms.openlocfilehash: 46a823e4e230656b53a93a97f195d0879fd08bf2
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56731953"
---
# <a name="application-gateway-ssl-policy-overview"></a>Overzicht van Application Gateway SSL-beleid

Azure Application Gateway kunt u SSL-Certificaatbeheer centraliseren en versleuteling en ontsleuteling overhead verminderen vanuit een back-end-server-farm. Deze gecentraliseerde SSL verwerken ook kunt u een centrale SSL-beleid dat geschikt voor de beveiligingsvereisten van uw organisatie opgeven. Dit helpt u bij het voldoen aan nalevingsvereisten, evenals richtlijnen voor beveiliging en aanbevolen procedures.

De SSL-beleid bevat een besturingselement van de versie van het SSL-protocol, evenals de coderingssuites en de volgorde waarin de coderingen die worden gebruikt tijdens een SSL-handshake. Application Gateway biedt twee methoden voor het beheren van SSL-beleid. U kunt een vooraf gedefinieerd beleid of een aangepast beleid gebruiken.

## <a name="predefined-ssl-policy"></a>Vooraf gedefinieerde SSL-beleid

Application Gateway biedt drie vooraf gedefinieerde beveiligingsbeleid. U kunt uw gateway configureren met een van deze beleid voor het ophalen van het juiste niveau van beveiliging. Beleidsnamen die zijn gemarkeerd door het jaar en maand waarin ze zijn geconfigureerd. Elk beleid biedt verschillende SSL-protocolversies en coderingssuites. U wordt aangeraden dat u het nieuwste SSL-beleid gebruiken om te controleren of de beste SSL-beveiliging.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Eigenschap  |Value  |
|---|---|
|Name     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Standaard| De waarde True (als er geen vooraf gedefinieerd beleid is opgegeven) |
|CipherSuites     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Eigenschap  |Value  |
|   ---      |  ---       |
|Name     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_1        |
|Standaard| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Eigenschap  |Value  |
|---|---|
|Name     | AppGwSslPolicy20170401S        |
|MinProtocolVersion     | TLSv1_2        |
|Standaard| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-ssl-policy"></a>Aangepaste SSL-beleid

Als een vooraf gedefinieerde SSL-beleid worden geconfigureerd voor uw vereisten moet, moet u uw eigen aangepaste SSL-beleid definiëren. Met een aangepast SSL-beleid hebt u volledige controle over de minimale SSL-protocolversie te ondersteunen, evenals de ondersteunde coderingssuites en de volgorde van hun prioriteit.
 
### <a name="ssl-protocol-versions"></a>SSL-protocolversies

* SSL 2.0 en 3.0 zijn standaard uitgeschakeld voor alle Toepassingsgateways. Deze protocolversies zijn niet configureerbaar.
* Een aangepast SSL-beleid biedt u de optie voor het selecteren van een van de volgende drie protocollen als de minimale versie van SSL-protocol voor uw gateway: TLSv1_0, TLSv1_1 en TLSv1_2.
* Als er geen SSL-beleid is gedefinieerd, worden alle drie protocollen (TLSv1_0, TLSv1_1 en TLSv1_2) ingeschakeld.

### <a name="cipher-suites"></a>-Coderingssuites

Application Gateway ondersteunt de volgende cipher suites van waaruit u het aangepaste beleid kunt. De volgorde van de coderingssuites, bepaalt de volgorde van prioriteit tijdens het SSL-onderhandeling.


- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over het configureren van een SSL-beleid wilt, Zie [configureren van SSL-beleid in een toepassingsgateway](application-gateway-configure-ssl-policy-powershell.md).
