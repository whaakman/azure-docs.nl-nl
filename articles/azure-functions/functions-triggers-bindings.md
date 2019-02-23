---
title: Triggers en bindingen in Azure Functions
description: Leer hoe u met triggers en bindingen verbinding maken met uw Azure-functie online evenementen en cloud-gebaseerde services.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 3865f748a9ca2fe09660d6454542d64f73a8e3c1
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56736954"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions-triggers en bindingen concepten

In dit artikel leert u de geavanceerde concepten rond functions-triggers en bindingen.

Triggers zijn wat ertoe leiden dat een functie om uit te voeren. Een trigger bepaalt hoe een functie is aangeroepen en een functie moet exact één trigger hebben. Triggers aan gegevens, die vaak wordt geleverd als de nettolading van de functie zijn gekoppeld. 

Binding aan een functie is een manier om een andere resource declaratief verbinding te maken met de functie. Bindingen kunnen worden verbonden als *invoer bindingen*, *uitvoerbindingen*, of beide. Gegevens van bindingen is opgegeven voor de functie als parameters.

U kunt combineren en overeenkomen met andere bindingen aan uw behoeften. Bindingen zijn optioneel en een functie kan één of meerdere invoer hebt en/of uitvoerbindingen.

Triggers en bindingen kunnen u voorkomen dat hardcoderen toegang tot andere services. De functie ontvangt gegevens (bijvoorbeeld de inhoud van een wachtrijbericht) in de parameters van de functie. U kunt gegevens (bijvoorbeeld om een wachtrijbericht te maken) verzenden met behulp van de geretourneerde waarde van de functie. 

Houd rekening met de volgende voorbeelden van hoe u verschillende functies kan implementeren.

| Voorbeeldscenario | Trigger | Invoerbinding | Storage-Uitvoerbinding |
|-------------|---------|---------------|----------------|
| Een nieuwe wachtrijbericht wordt ontvangen die wordt uitgevoerd met een functie voor het schrijven naar een andere wachtrij. | wachtrij<sup>*</sup> | *Geen* | wachtrij<sup>*</sup> |
|Een geplande taak leest de inhoud van de Blob-opslag en maakt u een nieuwe Cosmos DB-document. | Timer | Blob Storage | Cosmos DB |
|De Event Grid wordt gebruikt om een afbeelding niet lezen uit de Blob-opslag en een document van Cosmos DB om een e-mail te verzenden. | Event Grid | BLOB-opslag en Cosmos DB | SendGrid |
| Een webhook die gebruikmaakt van Microsoft Graph om bij te werken van een Excel-werkblad. | HTTP | *Geen* | Microsoft Graph |

<sup>\*</sup> Hiermee geeft u verschillende wachtrijen

Deze voorbeelden zijn niet bedoeld om te worden volledig, maar om te laten zien hoe u kunt triggers en bindingen samen worden opgegeven.

###  <a name="trigger-and-binding-definitions"></a>Trigger-en-binding

Triggers en bindingen zijn verschillend, afhankelijk van de aanpak van ontwikkeling gedefinieerd.

| Platform | Triggers en bindingen zijn geconfigureerd door... |
|-------------|--------------------------------------------|
| C#klassebibliotheek | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;met het inrichten van methoden en parameters met C# kenmerken |
| Alle andere (met inbegrip van Azure portal) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bijwerken van [function.json](./functions-reference.md) ([schema](http://json.schemastore.org/function)) |

De portal biedt een gebruikersinterface voor deze configuratie, maar u kunt het bestand bewerken rechtstreeks door het openen van de **geavanceerde editor** beschikbaar via de **integreren** tabblad van uw functie.

In .NET definieert het parametertype het gegevenstype voor de invoergegevens. Gebruik bijvoorbeeld `string` verbinding maken met de tekst van een wachtrijtrigger, een matrix van bytes lezen als binaire bestanden en een aangepaste-type naar een object niet deserialiseren.

Voor de talen die dynamisch worden getypt, zoals JavaScript, gebruikt u de `dataType` eigenschap in de *function.json* bestand. Bijvoorbeeld, als u wilt de inhoud van een HTTP-aanvraag in binaire indeling lezen, ingesteld `dataType` naar `binary`:

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

Andere opties voor `dataType` zijn `stream` en `string`.

## <a name="binding-direction"></a>Richting van de binding

Alle triggers en bindingen hebben een `direction` eigenschap in de [function.json](./functions-reference.md) bestand:

- Triggers is de richting altijd `in`
- Gebruik van de invoer- en uitvoerbindingen `in` en `out`
- Sommige bindingen ondersteunt een speciale richting `inout`. Als u `inout`, alleen de **geavanceerde editor** is beschikbaar via de **integreren** tabblad in de portal.

Bij het gebruik [kenmerken in een klassebibliotheek](functions-dotnet-class-library.md) voor het configureren van triggers en bindingen, de richting is opgegeven in een kenmerkconstructie of afgeleid van het parametertype.

## <a name="supported-bindings"></a>Ondersteunde bindingen

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Zie voor meer informatie over welke bindingen zijn beschikbaar als preview of zijn goedgekeurd voor gebruik in productieomgevingen, [ondersteunde talen](supported-languages.md).

## <a name="resources"></a>Resources
- [Binding-expressies en patronen](./functions-bindings-expressions-patterns.md)
- [Met behulp van de geretourneerde waarde van de Azure-functie](./functions-bindings-return-value.md)
- [Informatie over het registreren van een bindingexpressie](./functions-bindings-register.md)
- Testen:
  - [Strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md)
  - [Handmatig uitvoeren van een niet HTTP-geactiveerde functie](functions-manually-run-non-http.md)
- [Afhandeling van fouten-binding](./functions-bindings-errors.md)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Azure Functions-binding extensies registreren](./functions-bindings-register.md)
