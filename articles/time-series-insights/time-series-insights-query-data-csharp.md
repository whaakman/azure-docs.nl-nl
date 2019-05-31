---
title: Opvragen van gegevens uit een Azure Time Series Insights GA-omgeving met C# code | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gegevens uit een Azure Time Series Insights-omgeving op te vragen door een aangepaste app, geschreven in de C# (C-sharp) .NET-taal coderen.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
reviewer: jasonwhowell, kfile, tsidocs
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/09/2019
ms.custom: seodec18
ms.openlocfilehash: e91c6fb4949a4902194bb95f98e49327434cdbfd
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244098"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Opvragen van gegevens uit de Azure Time Series Insights GA omgeving gebruikenC#

Dit C# voorbeeld ziet u hoe u gegevens opvraagt uit de omgeving Azure Time Series Insights algemeen beschikbaar. 

Er worden enkele eenvoudige voorbeelden voor het gebruik van de API-query gegeven:

1. Als voorbereiding moet het toegangstoken via de Azure Active Directory-API aan te schaffen. Geeft dit token in de `Authorization` -header van elke Query-API-aanvraag. Zie voor het instellen van niet-interactieve toepassingen, [verificatie en autorisatie](time-series-insights-authentication-and-authorization.md). Zorg er ook voor alle constanten zijn gedefinieerd aan het begin van het voorbeeld juist zijn ingesteld.
1. De lijst met omgevingen waarin de gebruiker toegang tot heeft wordt verkregen. Een van de omgevingen wordt opgehaald als de belangrijkste omgeving en verdere gegevens opgevraagd voor deze omgeving.
1. Er worden beschikbaarheidsgegevens opgevraagd voor de belangrijkste omgeving als voorbeeld van een HTTPS-aanvraag.
1. Er worden gebeurtenissamenvoegingsgegevens opgevraagd voor de belangrijkste omgeving als voorbeeld van een web socket-aanvraag. Er worden gegevens opgevraagd voor het volledige tijdsbereik van de beschikbaarheid.

> [!NOTE]
> De voorbeeldcode is beschikbaar op [ https://github.com/Azure-Samples/Azure-Time-Series-Insights ](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-ga-preview-sample).

## <a name="project-dependencies"></a>Afhankelijkheden van project

NuGet-pakketten toevoegen `Microsoft.IdentityModel.Clients.ActiveDirectory` en `Newtonsoft.Json`.

## <a name="c-example"></a>C#-voorbeeld

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over het uitvoeren van query's, de [Query API-verwijzing](/rest/api/time-series-insights/ga-query-api).

- Lees hoe naar [verbinding maken met een JavaScript-app met één pagina](tutorial-create-tsi-sample-spa.md) voor Time Series Insights.