---
title: Beperkingen en bekende problemen in Azure API Management-API importeren | Microsoft Docs
description: Details van de bekende problemen en beperkingen voor het importeren in Azure API Management met behulp van de Open API, WSDL of WADL-indelingen.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: bad87931feb11012f23f0ef19bd853b38566c07c
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106821"
---
# <a name="api-import-restrictions-and-known-issues"></a>Importbeperkingen voor API- en bekende problemen
## <a name="about-this-list"></a>Over deze lijst
Bij het importeren van een API, kan u gehoord enkele beperkingen of identificeren van problemen die worden verholpen moeten voordat u met succes kunt importeren. In dit artikel worden deze, onderverdeeld op basis van de indeling van het importeren van de API.

## <a name="open-api"> </a>OpenAPI/Swagger
Als u uw OpenAPI-document importeren fouten ontvangt, zorg ervoor dat u hebt het - met de ontwerpfunctie in Azure portal (ontwerp - Front-End - OpenAPI Specification Editor), gevalideerd of met een derde partij hulpprogramma zoals <a href="https://editor.swagger.io">Swagger Editor</a>.

* Alleen JSON-indeling voor OpenAPI wordt ondersteund.
* Vereiste parameters voor het pad en de query moeten unieke namen hebben. (In de OpenAPI een parameternaam alleen moet uniek zijn binnen een locatie, zoals pad, query, -header.  In API Management kunnen we echter bewerkingen om te worden discriminated op pad en de query-parameters (die biedt geen ondersteuning voor OpenAPI). Daarom moeten parameternamen moeten uniek zijn binnen de volledige URL-sjabloon.)
* Schema's waarnaar wordt verwezen met behulp van **$ref** eigenschappen mag geen andere bevatten **$ref** eigenschappen.
* **$ref** aanwijzers kunnen niet verwijzen naar externe bestanden.
* **x-ms-paden** en **x-servers** zijn de enige ondersteunde extensies.
* Aangepaste extensies zijn worden genegeerd tijdens het importeren en niet opgeslagen of bewaard totdat de export.
* **Recursie** -definities die zijn gedefinieerd recursief (bijvoorbeeld verwijzen naar zichzelf sturen) worden niet ondersteund door APIM.

> [!IMPORTANT]
> Zie dit [document](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) voor belangrijke informatie en tips die betrekking hebben op OpenAPI importeren.

## <a name="wsdl"> </a>WSDL
WSDL-bestanden worden gebruikt om te fungeren als de back-end van een SOAP naar REST-API of SOAP Pass Through-API's genereren.
* **SOAP-bindingen** -alleen SOAP-bindingen van de stijl 'document' en "literal"-codering worden ondersteund. Er is geen ondersteuning voor "rpc" style of SOAP-codering.
* **WSDL: import** -dit kenmerk wordt niet ondersteund. Klanten moeten de invoer samenvoegen in één document.
* **Berichten met meerdere onderdelen** -deze typen berichten worden niet ondersteund.
* **WCF-wsHttpBinding** -SOAP-services die zijn gemaakt met Windows Communication Foundation basicHttpBinding moeten gebruiken - wsHttpBinding wordt niet ondersteund.
* **MTOM** - Services met behulp van MTOM <em>kan</em> werken. Op dit moment is geen officiële ondersteuning geboden.
* **Recursie** -typen die zijn gedefinieerd recursief (bijvoorbeeld verwijzen naar een matrix van zichzelf) worden niet ondersteund door APIM.
* **Meerdere naamruimten** : meerdere naamruimten in een schema kan worden gebruikt, maar alleen de doelnaamruimte kan worden gebruikt voor het delen van een bericht te definiëren. Naamruimten dan de doelgegevensruimte die worden gebruikt voor het definiëren van andere invoer- of -elementen blijven niet behouden. Hoewel dit een WSDL-document kan worden geïmporteerd, bij het exporteren heeft alle delen van een bericht de doelnaamruimte van de WSDL.

## <a name="wadl"> </a>WADL
Er zijn momenteel geen problemen bekend WADL importeren.
