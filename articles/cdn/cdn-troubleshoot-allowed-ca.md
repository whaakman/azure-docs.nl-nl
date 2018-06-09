---
title: Certificeringsinstanties toegestaan voor het aangepaste HTTPS op Azure CDN inschakelen | Microsoft Docs
description: Als u uw eigen certificaat gebruikt voor HTTPS op een aangepast domein inschakelen, moet u een toegestane certificeringsinstantie (CA) gebruiken om deze te maken.
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
ms.date: 06/06/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 3c41ca7e375324ff784bf7bee347bb56400ddfbd
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238268"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Certificeringsinstanties toegestaan voor het aangepaste HTTPS op Azure CDN inschakelen

Voor een aangepast domein Azure Content Delivery Network (CDN) op een **Azure CDN Standard van Microsoft** eindpunt, wanneer u [de HTTPS-functie inschakelen met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), moet u een toegestane certificeringsinstantie (CA) voor het maken van uw SSL-certificaat. Als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt, kunnen uw aanvraag anders zal worden geweigerd.

> [!NOTE]
> De optie van uw eigen certificaat gebruikt om aangepaste HTTPS inschakelen is alleen beschikbaar bij **Azure CDN Standard van Microsoft** profielen. 
>

De volgende CA's zijn toegestaan bij het maken van uw eigen certificaat:

- AddTrust externe CA-basiscertificaat
- Azië basis-CA
- Azië hoofdcertificeringsinstantie 2013
- Azië hoofdcertificeringsinstantie 2014
- APCA DM3P
- Automatische piloot basis-CA
- Baltimore CyberTrust Root
- Klasse 3 openbare primaire certificeringsinstantie
- COMODO RSA-certificeringsinstantie
- COMODO RSA domein validatie beveiligde Server CA
- D VERTROUWDE basis-CA van klasse 3 2 2009
- DigiCert Cloud Services-CA-1
- DigiCert globale basis-CA
- DigiCert hoge zekerheid CA-3
- DigiCert hoge zekerheid EV basis-CA
- DigiCert SHA2 hoge zekerheid Server CA
- DigiCert SHA2 beveiligde Server CA
- GlobalSign
- Validatie van CA - SHA256 - G2 uitgebreid GlobalSign
- Validatie van de organisatie GlobalSign CA - G2
- GlobalSign basis-CA
- Microsoft Authenticode (TM) basisinstantie
- Microsoft Exchange-Services CA 2015
- Microsoft interne zakelijke basis
- Microsoft IT KREGEL SSL CA 1
- Microsoft IT SSL SHA1
- Microsoft IT SSL SHA2
- Microsoft IT TLS CA 1
- Microsoft IT TLS CA 2
- Microsoft IT TLS CA 4
- Microsoft IT TLS CA 5
- Microsoft basisinstantie
- Microsoft basiscertificeringsinstantie
- Microsoft hoofdcertificeringsinstantie 2010
- Microsoft hoofdcertificeringsinstantie 2011
- Microsoft beveiligde Server 2011 CA
- Microsoft-Services Partner hoofdmap
- Microsoft Time Stamping Service Root
- Compatibiliteit van hardwareapparaten voor Microsoft Windows
- MSIT CA Z2
- MSIT ondernemings-CA 1
- MSIT ondernemings-CA 3
- Hoofdmap Agency
- Symantec klasse 3 EV SSL-CA - G3
- Symantec klasse 3 beveiligde Server CA - G4
- Symantec Enterprise Mobile Root voor Microsoft
- Thawte Timestamping CA
- UTN-USERFirst-Object
- VeriSign klasse 3 uitgebreide validatie SSL-Certificeringsinstantie
- VeriSign klasse 3 uitgebreide validatie SSL SGC CA
- VeriSign klasse 3 openbare primaire certificeringsinstantie - G5
- VeriSign International Server CA - klasse 3
- VeriSign Time Stamping Service Root
- VeriSign Universal basiscertificeringsinstantie
- WMSvc-SHA2-DALEDGE1008

