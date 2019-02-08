---
title: Veelgestelde vragen (FAQ) over FHIR-services in Azure - API van Azure voor FHIR
description: In dit artikel Veelgestelde vragen over vindt u antwoorden op veelgestelde vragen over Azure-API voor FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: e3889ed9f758ce2c374eae106674930ba67f7620
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878779"
---
# <a name="frequently-asked-questions-about-azure-api-for-fhir"></a>Veelgestelde vragen over Azure-API voor FHIR

## <a name="storage-location"></a>Opslaglocatie

**Is van de gegevens achter de FHIR&reg; API's die zijn opgeslagen in Azure?** Ja, de gegevens worden opgeslagen in beheerde databases in Azure. De API van Azure voor FHIR biedt geen directe toegang tot het onderliggende gegevensarchief.

## <a name="identity-providers"></a>Id-providers

Op dit moment ondersteunen we Microsoft Azure Active Directory als id-provider.

## <a name="supported-fhir-version"></a>Ondersteunde FHIR-versie

We ondersteunen momenteel versie 3.0.1. Zie [ondersteunde functies](fhir-features-supported.md) voor meer informatie.

## <a name="oss-and-azure-api-for-fhir"></a>OSS- en Azure-API voor FHIR

Wat is het verschil tussen de Open Source Microsoft FHIR-server voor Azure en Azure-API voor FHIR? De API van Azure voor FHIR is een gehoste en beheerde versie van het OSS Microsoft FHIR-Server voor Azure. Microsoft biedt in de beheerde service, onderhoud, updates, enzovoort. Wanneer de OSS FHIR-Server voor Azure wordt uitgevoerd, hebt u directe toegang tot de onderliggende services, maar ook bent u verantwoordelijk voor het onderhouden, bijwerken van de server en alle vereiste naleving werken als het opslaan van PHI-gegevens.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u enkele van de veelgestelde vragen over Azure-API voor FHIR gelezen. Lees over de ondersteunde API-functies in Microsoft FHIR-server voor Azure.
 
>[!div class="nextstepaction"]
>[Ondersteunde FHIR-functies](fhir-features-supported.md)