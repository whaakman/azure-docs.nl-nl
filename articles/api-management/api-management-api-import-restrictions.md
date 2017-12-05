---
title: Beperkingen en bekende problemen in Azure API Management-API importeren | Microsoft Docs
description: Details van de bekende problemen en beperkingen voor het importeren in Azure API Management met de Open API, WSDL of WADL indelingen.
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: 758babce3ed387ed4864f1934650cf701bda788f
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>API importeren beperkingen en bekende problemen
## <a name="about-this-list"></a>Over deze lijst
Bij het importeren van een API kunt u tegenkomt enkele beperkingen of geven aan welke problemen die worden verholpen moeten voordat u met succes kunt importeren. Deze documenten artikel, ingedeeld door de importindeling van de API.

## <a name="open-api"></a>API/Swagger openen
Als u uw document Open API importeren fouten ontvangt, controleert u of u hebt deze - met de ontwerpfunctie voor in de Azure portal (ontwerp - Front-End - API specificatie Editor openen), gevalideerd of met een derde hulpprogramma zoals <a href="http://www.swagger.io">Swagger Editor</a>.

* **Hostnaam** APIM vereist een host name-kenmerk.
* **Pad baseren** APIM vereist een kenmerk basispad.
* **Schema's** APIM is een matrix schema vereist. 

## <a name="wsdl"></a>WSDL
WSDL-bestanden worden gebruikt voor het genereren van SOAP Pass Through-API's of fungeren als de back-end van een SOAP-REST-API.

* **WSDL: import** -APIM biedt momenteel geen ondersteuning voor API's met behulp van dit kenmerk. Klanten moeten de geïmporteerde elementen samenvoegen in één document.
* **Berichten met meerdere onderdelen** APIM biedt momenteel geen ondersteuning voor deze typen berichten.
* **WCF-wsHttpBinding** SOAP-services die zijn gemaakt met Windows Communication Foundation basicHttpBinding moeten gebruiken - wsHttpBinding wordt niet ondersteund.
* **MTOM** MTOM-Services <em>kan</em> werken. Op dit moment is geen officiële ondersteuning geboden.
* **Recursie** typen die zijn gedefinieerd recursief (bijvoorbeeld verwijzen naar een matrix van zichzelf) worden niet ondersteund door APIM.

## <a name="wadl"></a>WADL
Er zijn momenteel geen bekende problemen van WADL importeren.
