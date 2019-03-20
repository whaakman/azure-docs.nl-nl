---
title: Logboekregistratie en diagnostische gegevens in Azure ruimtelijke ankers | Microsoft Docs
description: Gedetailleerde uitleg over hoe u het genereren en ophalen van logboekregistratie en diagnostische gegevens in Azure ruimtelijke ankers.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 134023c0884ce3a402b99806f1bf19dcb59ecc32
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57882825"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Logboekregistratie en diagnostische gegevens in Azure ruimtelijke ankers

Azure ruimtelijke ankers biedt een mechanisme voor standaard logboekregistratie die handig is voor de ontwikkeling van apps. De ruimtelijke ankers Diagnostische logboekregistratie modus is handig als u meer informatie nodig voor het opsporen van fouten hebt. Logboekregistratie van diagnostische gegevens worden opgeslagen afbeeldingen van de omgeving.

## <a name="standard-logging"></a>Standaard logboekregistratie
In de ruimtelijke ankers-API, kunt u zich abonneert op het mechanisme voor logboekregistratie handige logboeken ophalen voor de ontwikkeling van toepassingen en foutopsporing. De standaard API-logboekregistratie niet foto's van de omgeving opslaan op de schijf van het apparaat. De SDK biedt deze logboeken als callbacks van de gebeurtenis. Het is aan u deze logboeken integreren in mechanisme voor logboekregistratie van de toepassing.

### <a name="configuration-of-log-messages"></a>Configuratie van logboekberichten
Er zijn twee callbacks van belang zijn voor de gebruiker. Het volgende voorbeeld laat zien hoe het configureren van de sessie.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events-and-properties"></a>Gebeurtenissen en eigenschappen

Deze gebeurtenis retouraanroepen zijn beschikbaar voor het verwerken van Logboeken en fouten van de sessie:

- [LogLevel](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): Hiermee geeft u het detailniveau voor de gebeurtenissen voor het ontvangen van de runtime.
- [OnLogDebug](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): Biedt standaard foutopsporing logboekgebeurtenissen.
- [Fout](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): Biedt gebeurtenissen die de runtime acht fouten.

## <a name="diagnostics-logging"></a>Logboekregistratie van diagnostische gegevens

Naast de standaardmodus voor logboekregistratie heeft ruimtelijke ankers ook een diagnostische modus. Diagnostische modus van installatiekopieën van de omgeving vastgelegd en registreert ze naar de schijf. In deze modus kunt u fouten opsporen in bepaalde soorten problemen, zoals een anker zoals verwacht te vinden is mislukt. Schakel Diagnostische logboekregistratie alleen voor het reproduceren van een specifiek probleem. Vervolgens uitschakelt. Geen diagnostische gegevens niet inschakelen wanneer u nu uw apps worden normaal uitgevoerd.

Tijdens een interactie met de ondersteuning van Microsoft, kan een Microsoft-vertegenwoordiger vragen als u bereid bent te verzenden van een bundel diagnostische gegevens voor verder onderzoek. U kunt in dit geval diagnostische gegevens inschakelen en het probleem te reproduceren, zodat u de diagnostische bundel kunt indienen. 

Als u een logboek met diagnostische gegevens naar Microsoft zonder voorafgaande bevestiging van een Microsoft-vertegenwoordiger verzenden, gaat de inzending onbeantwoorde.

De volgende secties tonen hoe u diagnostische gegevens en voor informatie over diagnostische logboeken naar Microsoft verzenden.

### <a name="enable-diagnostics-logging"></a>Diagnostische logboekregistratie inschakelen

Wanneer u een sessie voor logboekregistratie van diagnostische gegevens inschakelt, zijn alle bewerkingen in de sessie bijbehorende Diagnostische logboekregistratie in het lokale bestandssysteem. Afbeeldingen van de omgeving worden opgeslagen tijdens de registratie naar de schijf.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // opt in to diagnostics logging of environment images
    // if this is enabled, the diagnostics bundle includes images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submit-the-diagnostics-bundle"></a>De bundel diagnostische gegevens verzenden

Het volgende codefragment toont hoe u kunt een bundel diagnostische gegevens naar Microsoft verzenden. Deze bundel omvat installatiekopieën van de omgeving die is opgenomen door de sessie nadat u diagnostische gegevens inschakelen. 

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="parts-of-a-diagnostics-bundle"></a>Onderdelen van een bundel diagnostische gegevens
De bundel diagnostische gegevens kan bevatten de volgende informatie:

- **Sleutelframes installatiekopieën**: Installatiekopieën van de omgeving die is vastgelegd tijdens de sessie terwijl de diagnostische gegevens zijn ingeschakeld.
- **Logs**: Logboekgebeurtenissen vastgelegd door de runtime.
- **De metagegevens van de sessie**: De metagegevens die de sessie identificeert.
