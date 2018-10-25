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
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984763"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS op Azure voordeur Service

Voor een aangepast Azure voordeur Service-domein, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), moet u een toegestane certificeringsinstantie (CA) gebruiken om uw SSL-certificaat te maken. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

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
