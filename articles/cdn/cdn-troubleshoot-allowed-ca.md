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
ms.openlocfilehash: 79b8cfa061bafb43cbcf2ec8a43638fd14c2fcd0
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954029"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN

Voor een aangepaste domein in Azure Content Delivery Network (CDN) op een **Azure CDN Standard van Microsoft** eindpunt, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), moet u een toegestane certificeringsinstantie (CA) voor het maken van uw SSL-certificaat. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

> [!NOTE]
> De mogelijkheid om uw eigen certificaat gebruikt om aangepaste HTTPS inschakelen is alleen beschikbaar bij **Azure CDN Standard van Microsoft** profielen. 
>

De volgende CA's zijn toegestaan bij het maken van uw eigen certificaat:

- AddTrust externe CA-basiscertificaat
- AlphaSSL basis-CA
- AAM Infra CA 01
- AAM Infra CA 02
- Ameroot
- Azië en Stille Oceaan basis-CA
- Azië en Stille Oceaan basiscertificeringsinstantie 2013
- Azië en Stille Oceaan basiscertificeringsinstantie 2014
- APCA DM3P
- AutoPilot-basis-CA
- Baltimore CyberTrust Root
- Klasse 3 openbare primaire certificeringsinstantie
- Certificeringsinstantie (CA) COMODO RSA
- COMODO RSA domein validatie beveiligde Server CA
- D VERTROUWDE basis-CA van klasse 3 2 2009
- DigiCert Cloud Services-CA-1
- DigiCert globale basis-CA
- DigiCert hoge zekerheid CA-3
- DigiCert hoge zekerheid VW basis-CA
- DigiCert SHA2 uitgebreide validatie Server CA
- DigiCert SHA2 hoge zekerheid Server CA
- DigiCert SHA2 beveiligde Server CA
- Zomertijd basis-CA X3
- D vertrouwde basis-CA van klasse 3 2 2009
- Versleuteling overal DV TLS CA
- Entrust basiscertificeringsinstantie
- Basis-CA - G2 Entrust
- Certificeringsinstantie (CA) Entrust.NET (2048)
- Globale CA GeoTrust
- Certificeringsinstantie (CA) GeoTrust primaire
- Primaire GeoTrust certificeringsinstantie - G2
- Geotrust RSA CA 2018
- GlobalSign
- Uitgebreide validatie van CA - SHA256 - G2 GlobalSign
- Validatie van de organisatie GlobalSign CA - G2
- GlobalSign basis-CA
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
- VeriSign International Server CA - klasse 3
- VeriSign tijd tijdstempel Service hoofdmap
- VeriSign Universal basiscertificeringsinstantie


