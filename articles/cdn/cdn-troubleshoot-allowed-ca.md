---
title: Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN | Microsoft Docs
description: Als u uw eigen certificaat gebruikt voor het inschakelen van HTTPS op een aangepast domein, moet u een toegestane certificeringsinstantie (CA) gebruiken om deze te maken.
services: cdn
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 28d6d24266c11b1295c57c8ec46c2bd5ec690b28
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005914"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Certificeringsinstanties toegestaan voor het inschakelen van de aangepaste HTTPS voor Azure CDN

Voor een aangepaste domein in Azure Content Delivery Network (CDN) op een **Azure CDN Standard van Microsoft** eindpunt, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), moet u een toegestane certificeringsinstantie (CA) voor het maken van uw SSL-certificaat. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

> [!NOTE]
> De mogelijkheid om uw eigen certificaat gebruikt om aangepaste HTTPS inschakelen is alleen beschikbaar bij **Azure CDN Standard van Microsoft** profielen. 
>

De volgende CA's zijn toegestaan bij het maken van uw eigen certificaat:

- AddTrust externe CA-basiscertificaat
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
- DigiCert SHA2 hoge zekerheid Server CA
- DigiCert SHA2 beveiligde Server CA
- Globale CA GeoTrust
- Certificeringsinstantie (CA) GeoTrust primaire
- Primaire GeoTrust certificeringsinstantie - G2
- GlobalSign
- Uitgebreide validatie van CA - SHA256 - G2 GlobalSign
- Validatie van de organisatie GlobalSign CA - G2
- GlobalSign basis-CA
- Go Daddy-basiscertificeringsinstantie - G2
- Microsoft Authenticode (TM) basis-CA
- Microsoft Exchange-Services CA 2015
- Microsoft interne zakelijke basis
- Microsoft IT Tevredenheid SSL CA 1
- Microsoft IT SSL SHA1
- Microsoft IT SSL SHA2
- Microsoft IT TLS CA 1
- Microsoft IT TLS CA 2
- Microsoft IT TLS CA 4
- Microsoft IT TLS CA 5
- Microsoft-basis-CA
- Microsoft basiscertificeringsinstantie
- Microsoft Root Certificate Authority 2010
- Microsoft Root Certificate Authority 2011
- Microsoft-beveiligde Server CA 2011
- Hoofdmap voor Services van Microsoft-Partner
- Microsoft Time tijdstempel Service hoofdmap
- Microsoft Windows hardwarecompatibiliteit
- MSIT CA Z2
- MSIT CA voor ondernemingen 1
- MSIT CA voor ondernemingen 3
- Basis-instantie
- Symantec-klasse 3 VW SSL CA - G3
- Symantec-klasse 3 beveiligde Server CA - G4
- Symantec Enterprise Mobile Root voor Microsoft
- Primaire Thawte basis-CA
- Primaire Thawte basis-CA - G2
- Primaire Thawte basis-CA - G3
- Thawte Timestamping CA
- UTN-USERFirst-Object
- VeriSign klasse 3 uitgebreide validatie SSL CA
- VeriSign klasse 3 uitgebreide validatie SSL SGC CA
- VeriSign klasse 3 openbare primaire certificeringsinstantie - G5
- VeriSign International Server CA - klasse 3
- VeriSign tijd tijdstempel Service hoofdmap
- VeriSign Universal basiscertificeringsinstantie
- WMSvc-SHA2-DALEDGE1008

