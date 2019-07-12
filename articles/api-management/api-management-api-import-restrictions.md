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
ms.date: 06/26/2019
ms.author: apimpm
ms.openlocfilehash: af550d3cdf359fc79b3cc2c799e531e5ec491c4e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67613635"
---
# <a name="api-import-restrictions-and-known-issues"></a>Importbeperkingen voor API- en bekende problemen

## <a name="about-this-list"></a>Over deze lijst

Bij het importeren van een API, kan u gehoord enkele beperkingen of identificeren van problemen die worden verholpen moeten voordat u met succes kunt importeren. In dit artikel worden deze, onderverdeeld op basis van de indeling van het importeren van de API.

## <a name="open-api"> </a>OpenAPI/Swagger

Als u fouten importeren van uw OpenAPI-document ontvangt, zorg er dan voor dat u deze vooraf hebt gevalideerd. U kunt doen die beide met de ontwerpfunctie in Azure portal (ontwerp - Front-End - OpenAPI-specificatie-Editor) of met een hulpprogramma van derden zoals <a href="https://editor.swagger.io">Swagger Editor</a>.

### <a name="open-api-general"> </a>Algemene

-   Vereiste parameters voor het pad en de query moeten unieke namen hebben. (In de OpenAPI een parameternaam alleen moet uniek zijn binnen een locatie, bijvoorbeeld pad, query, -header. In API Management kunnen we echter bewerkingen om te worden discriminated op pad en de query-parameters (die biedt geen ondersteuning voor OpenAPI). That's waarom parameternamen moeten uniek zijn binnen de volledige URL-sjabloon is vereist.)
-   **\$REF** aanwijzers kunnen niet verwijzen naar externe bestanden.
-   **x-ms-paden** en **x-servers** zijn de enige ondersteunde extensies.
-   Aangepaste extensies worden niet worden genegeerd tijdens het importeren en opgeslagen of bewaard totdat de export.
-   **Recursie** -definities gedefinieerd recursief (bijvoorbeeld schema's die verwijzen naar zichzelf) biedt geen ondersteuning voor API Management.
-   Bestand bron-URL (indien beschikbaar) wordt toegepast op de relatieve URL's.

### <a name="open-api-v2"> </a>OpenAPI versie 2

-   Alleen JSON-indeling wordt ondersteund.

### <a name="open-api-v3"> </a>OpenAPI versie 3

-   Als veel **servers** zijn opgegeven, API Management wordt geprobeerd om te selecteren van de eerste HTTPs-URL. Als er een HTTPs-URL - de eerste HTTP-URL. Als er geen HTTP-URL's - is de URL van de server niet leeg zijn.
-   **Voorbeelden** wordt niet ondersteund, maar **voorbeeld** is.
-   **Multipart/formuliergegevens** wordt niet ondersteund.

> [!IMPORTANT]
> Zie dit [document](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) voor belangrijke informatie en tips die betrekking hebben op OpenAPI importeren.

## <a name="wsdl"> </a>WSDL

WSDL-bestanden worden gebruikt om SOAP Pass Through-query en SOAP naar REST-API's te maken.

-   **SOAP-bindingen** -alleen SOAP-bindingen van de stijl 'document' en "literal"-codering worden ondersteund. Er is geen ondersteuning voor "rpc" style of SOAP-codering.
-   **WSDL: import** -dit kenmerk wordt niet ondersteund. Klanten moeten de invoer samenvoegen in één document.
-   **Berichten met meerdere onderdelen** -deze typen berichten worden niet ondersteund.
-   **WCF-wsHttpBinding** -SOAP-services die zijn gemaakt met Windows Communication Foundation basicHttpBinding moeten gebruiken - wsHttpBinding wordt niet ondersteund.
-   **MTOM** - Services met behulp van MTOM <em>kan</em> werken. Officiële ondersteuning niet wordt op dit moment aangeboden.
-   **Recursie** -typen die zijn gedefinieerd recursief (bijvoorbeeld verwijzen naar een matrix van zichzelf) worden niet ondersteund door APIM.
-   **Meerdere naamruimten** : meerdere naamruimten in een schema kan worden gebruikt, maar alleen de doelnaamruimte kan worden gebruikt voor het delen van een bericht te definiëren. Naamruimten dan de doelgegevensruimte die worden gebruikt voor het definiëren van andere invoer- of -elementen blijven niet behouden. Hoewel dit een WSDL-document kan worden geïmporteerd, bij het exporteren heeft alle delen van een bericht de doelnaamruimte van de WSDL.
-   **Matrices** - SOAP naar REST transformatie ondersteunt alleen verpakt matrices wordt weergegeven in het voorbeeld hieronder:

```xml
    <complexType name="arrayTypeName">
        <sequence>
            <element name="arrayElementValue" type="arrayElementType" minOccurs="0" maxOccurs="unbounded"/>
        </sequence>
    </complexType>
    <complexType name="typeName">
        <sequence>
            <element name="element1" type="someTypeName" minOccurs="1" maxOccurs="1"/>
            <element name="element2" type="someOtherTypeName" minOccurs="0" maxOccurs="1" nillable="true"/>
            <element name="arrayElement" type="arrayTypeName" minOccurs="1" maxOccurs="1"/>
        </sequence>
    </complexType>
```

## <a name="wadl"> </a>WADL

Er zijn momenteel geen problemen bekend WADL importeren.
