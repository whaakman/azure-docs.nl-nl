---
title: Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure-Service voordeur | Microsoft Docs
description: Als u uw eigen certificaat gebruikt voor het inschakelen van HTTPS op een aangepast domein, moet u een toegestane certificeringsinstantie (CA) gebruiken om deze te maken.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: 5cf94079dcd68887d9725ffbe9124f9b6c897dd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736153"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS op Azure voordeur Service

Voor een aangepast Azure voordeur Service-domein, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), moet u een toegestane certificeringsinstantie (CA) gebruiken om uw SSL-certificaat te maken. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

De volgende CA's zijn toegestaan bij het maken van uw eigen certificaat:

- AddTrust External CA Root
- AlphaSSL basis-CA
- AME Infra CA 01
- AME Infra CA 02
- Ameroot
- AP Root CA
- Azië en Stille Oceaan basiscertificeringsinstantie 2013
- Azië en Stille Oceaan basiscertificeringsinstantie 2014
- APCA-DM3P
- Autopilot Root CA
- Baltimore CyberTrust Root
- Klasse 3 openbare primaire certificeringsinstantie
- Certificeringsinstantie (CA) COMODO RSA
- COMODO RSA Domain Validation Secure Server CA
- D VERTROUWDE basis-CA van klasse 3 2 2009
- DigiCert Cloud Services CA-1
- DigiCert Global Root CA
- DigiCert hoge zekerheid CA-3
- DigiCert hoge zekerheid VW basis-CA
- DigiCert SHA2 uitgebreide validatie Server CA
- DigiCert SHA2 hoge zekerheid Server CA
- DigiCert SHA2 Secure Server CA
- DST Root CA X3
- D vertrouwde basis-CA van klasse 3 2 2009
- Versleuteling overal DV TLS CA
- Entrust basiscertificeringsinstantie
- Basis-CA - G2 Entrust
- Entrust.net Certification Authority (2048)
- GeoTrust Global CA
- Certificeringsinstantie (CA) GeoTrust primaire
- Primaire GeoTrust certificeringsinstantie - G2
- Geotrust RSA CA 2018
- GlobalSign
- Uitgebreide validatie van CA - SHA256 - G2 GlobalSign
- GlobalSign Organization Validation CA - G2
- GlobalSign Root CA
- Go Daddy-basiscertificeringsinstantie - G2
- Go Daddy beveiligde certificeringsinstantie - G2
- RapidSSL RSA CA 2018
- Basis-instantie
- Symantec-klasse 3 VW SSL CA - G3
- Symantec-klasse 3 beveiligde Server CA - G4
- Symantec Enterprise Mobile Root voor Microsoft
- Primaire Thawte basis-CA
- Primaire Thawte basis-CA - G2
- Primaire Thawte basis-CA - G3
- Thawte RSA CA 2018
- Thawte Timestamping CA
- TrustAsia TLS RSA CA
- VeriSign klasse 3 uitgebreide validatie SSL CA
- VeriSign klasse 3 uitgebreide validatie SSL SGC CA
- VeriSign klasse 3 openbare primaire certificeringsinstantie - G5
- VeriSign International Server CA - Class 3
- VeriSign Time Stamping Service Root
- VeriSign Universal basiscertificeringsinstantie
