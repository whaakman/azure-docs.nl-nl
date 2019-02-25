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
ms.openlocfilehash: d9377e2b5b66a7d426373a8a85e4880dafeaeee6
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753065"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Logboekregistratie en diagnostische gegevens in Azure ruimtelijke ankers

Azure ruimtelijke ankers biedt een mechanisme standaard logboekregistratie nuttig voor het app-ontwikkeling. Bovendien, is er een diagnostische logboekregistratie modus nuttig wanneer nog meer informatie vereist voor het opsporen van fouten is. Logboekregistratie van diagnostische gegevens omvat het opslaan van installatiekopieën van de omgeving.

## <a name="standard-logging-in-azure-spatial-anchors"></a>Standaard logboekregistratie in Azure ruimtelijke ankers
De ruimtelijke ankers-API van Azure biedt een mechanisme voor logboekregistratie die toepassingen zich abonneren kunnen op voor het ontvangen van handige logboeken voor de ontwikkeling van toepassingen en foutopsporing. De standaard API-logboekregistratie niet foto's van de omgeving naar de schijf van het apparaat behouden. De SDK biedt deze logboeken als callbacks van de gebeurtenis. Het is aan u deze logboeken integreren in mechanisme voor logboekregistratie van de toepassing.

### <a name="how-to-configure-the-log-messages"></a>Het configureren van de logboekberichten
Er zijn twee callbacks van belang zijn voor de gebruiker. In het onderstaande voorbeeld ziet u hoe u de sessie configureren.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // setup the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events--properties"></a>& Eigenschappen van gebeurtenissen

De callbacks gebeurtenis is opgegeven voor het verwerken van Logboeken en fouten van de sessie.

- [LogLevel](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): Hiermee geeft u het detailniveau voor de gebeurtenissen voor het ontvangen van de runtime.
- [OnLogDebug](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): De callback van deze gebeurtenis biedt standaard foutopsporing logboekgebeurtenissen.
- [Fout](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): De callback van deze gebeurtenis biedt logboekgebeurtenissen fouten beschouwd door de runtime.

## <a name="diagnostics-logging-in-azure-spatial-anchors"></a>Diagnostische logboekregistratie in Azure ruimtelijke ankers

Naast de standaardmodus voor logboekregistratie die hierboven wordt beschreven, heeft Azure ruimtelijke ankers ook een diagnostische modus van die ontwikkelaars zich kunnen aanmelden. Diagnostische gegevens vastleggen van installatiekopieën van de omgeving en vastgelegd in de schijf. In deze modus is handig voor foutopsporing van bepaalde soorten problemen, zoals wanneer u niet kunnen een anker zoals verwacht te vinden. Diagnostische logboekregistratie te registreren voor een specifiek probleem te reproduceren en schakelt u deze alleen inschakelen. Voer niet uit uw apps normaal gesproken met diagnostische gegevens zijn ingeschakeld.

Tijdens een interactie met de ondersteuning van Microsoft vraag een Microsoft-vertegenwoordiger als u bereid bent te verzenden van een bundel van diagnostische gegevens naar Microsoft voor meer informatie. In dit geval wilt u mogelijk diagnostische gegevens inschakelen, het probleem te reproduceren en het verzenden van de diagnostische bundel naar Microsoft voor meer informatie. Logboeken met diagnostische gegevens naar Microsoft verzonden zonder voorafgaande bevestiging door een Microsoft-vertegenwoordiger gaat onbeantwoorde.

De volgende codefragmenten laten u diagnostische modus inschakelen en kunt u ook hoe u diagnostische logboeken naar Microsoft kunt verzenden.

### <a name="enabling-diagnostics-logging"></a>Diagnostische logboekregistratie inschakelen

Tijdens een sessie voor logboekregistratie van diagnostische gegevens is ingeschakeld, hebben alle bewerkingen op de sessie bijbehorende diagnostische gegevens die zich aanmelden op het lokale bestandssysteem. Logboekregistratie omvat het opslaan van installatiekopieën van de omgeving naar de schijf.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // setup the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // Opt-in to diagnostics logging of environment images.
    // If this is enabled, the diagnostics bundle will include images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submitting-the-diagnostic-bundle"></a>De diagnostische bundel verzenden

Het volgende codefragment toont hoe u kunt een bundel diagnostische gegevens naar Microsoft verzenden. Let op: dit bevat installatiekopieën van de omgeving die is opgenomen door de sessie na het inschakelen van diagnostische gegevens. Bundels van diagnostische gegevens naar Microsoft verzonden zonder voorafgaande bevestiging door een Microsoft-vertegenwoordiger gaat verder onbeantwoorde.

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest  to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="anatomy-of-the-diagnostics-bundle"></a>De anatomie van de bundel diagnostische gegevens
De volgende informatie is mogelijk aanwezig zijn in een bundel diagnostische gegevens:

- Installatiekopieën van sleutelframes - installatiekopieën van de omgeving die is vastgelegd tijdens de sessie terwijl de diagnostische gegevens zijn ingeschakeld.
- Logboeken - logboekgebeurtenissen vastgelegd door de runtime.
- Metagegevens van sessie - metagegevens waarmee de sessie.
