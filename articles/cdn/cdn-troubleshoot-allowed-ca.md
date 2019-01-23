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
ms.openlocfilehash: 11651c2721756a4f750a5a5e78f86fdbd363fb9d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462586"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN

Voor een aangepaste domein in Azure Content Delivery Network (CDN) op een **Azure CDN Standard van Microsoft** eindpunt, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), moet u een toegestane certificeringsinstantie (CA) voor het maken van uw SSL-certificaat. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

> [!NOTE]
> De mogelijkheid om uw eigen certificaat gebruikt om aangepaste HTTPS inschakelen is alleen beschikbaar bij **Azure CDN Standard van Microsoft** profielen. 
>

De volgende CA's zijn toegestaan bij het maken van uw eigen certificaat:

- AddTrust External CA Root
- AlphaSSL basis-CA
- AME Infra CA 01
- AME Infra CA 02
- Ameroot
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
- QuoVadis Root CA2 G3
- RapidSSL RSA CA 2018
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


