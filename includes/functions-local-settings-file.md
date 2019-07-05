---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: e319356d555f26354ea29dc7be068bf6168abb17
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455155"
---
## <a name="local-settings-file"></a>Lokale instellingsbestand

Het bestand local.settings.json slaat de app-instellingen, verbindingsreeksen en -instellingen die door de hulpprogramma's voor lokale ontwikkeling. Instellingen in het bestand local.settings.json worden alleen gebruikt wanneer die lokaal wordt uitgevoerd. De lokale instellingen-bestand heeft de volgende structuur:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

De volgende instellingen worden ondersteund bij lokale uitvoering:

| Instelling      | Description                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Als de waarde `true`, alle waarden zijn versleuteld met behulp van de sleutel van een lokale computer. Gebruikt in combinatie met `func settings` opdrachten. Standaardwaarde is `false`. |
| **`Values`** | Matrix van instellingen van toepassingen en verbindingsreeksen gebruikt bij het lokaal worden uitgevoerd. Deze sleutel-waarde (tekenreeks-tekenreeks)-paren overeenkomen met toepassingsinstellingen in uw functie-app in Azure, zoals [ `AzureWebJobsStorage` ]. Veel triggers en bindingen hebben een eigenschap die naar een appinstelling voor de verbindingsreeks, zoals verwijst `Connection` voor de [Blob storage-trigger](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---configuration). Voor deze eigenschappen, moet u een toepassingsinstelling gedefinieerd in de `Values` matrix. <br/>[`AzureWebJobsStorage`] is een vereiste app instellen voor triggers dan HTTP. <br/>Versie 2.x van de Functions-runtime moet de [`FUNCTIONS_WORKER_RUNTIME`]-instelling, die wordt gegenereerd voor uw project door Core Tools. <br/> Wanneer u hebt de [Azure-opslagemulator](../articles/storage/common/storage-use-emulator.md) lokaal is geïnstalleerd, kunt u instellen [ `AzureWebJobsStorage` ] naar `UseDevelopmentStorage=true` en Core Tools maakt gebruik van de emulator. Dit is handig tijdens het ontwikkelen, maar u moet testen met een verbinding van de werkelijke opslag vóór de implementatie.<br/> Waarden moeten tekenreeksen en geen JSON-objecten of matrices zijn. Instellen van namen mogen niet voorkomen een dubbele punt (`:`) of een dubbele onderstreping (`__`); deze zijn gereserveerd door de runtime.  |
| **`Host`** | Instellingen in deze sectie aanpassen hostproces van de functies bij lokale uitvoering. Dit zijn gescheiden van de instellingen host.json, die ook van toepassing wanneer die wordt uitgevoerd in Azure. |
| **`LocalHttpPort`** | Hiermee stelt u de standaardpoort gebruikt bij het uitvoeren van de lokale host van de functies (`func host start` en `func run`). De `--port` opdrachtregeloptie te gebruiken heeft voorrang op deze waarde. |
| **`CORS`** | Definieert de oorsprongen toegestaan voor [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Oorsprongen zijn opgegeven als een door komma's gescheiden lijst zonder spaties. Het jokerteken (\*) wordt ondersteund, waarmee aanvragen van een oorsprong. |
| **`CORSCredentials`** |  Instellen op true opdat `withCredentials` aanvragen. |
| **`ConnectionStrings`** | Gebruik deze verzameling niet voor de verbindingsreeksen die worden gebruikt door uw functiebindingen. Deze verzameling wordt alleen gebruikt door frameworks die doorgaans downloadt verbindingsreeksen uit de `ConnectionStrings` gedeelte van een configuratie-bestand, zoals [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Verbindingsreeksen in dit object zijn toegevoegd aan de omgeving van het providertype [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Items in deze verzameling zijn niet gepubliceerd naar Azure met andere app-instellingen. U moet expliciet toevoegen met deze waarden naar de `Connection strings` verzameling van de instellingen van uw functie-app. Als u maakt een [ `SqlConnection` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) in uw functiecode aan te geven, moet u de connection string-waarde in opslaan **toepassingsinstellingen** in de portal aan met uw andere verbindingen. |

[`AzureWebJobsStorage`]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
