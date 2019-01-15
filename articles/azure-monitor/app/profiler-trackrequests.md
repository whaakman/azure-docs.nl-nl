---
title: Schrijf code voor het volgen van aanvragen met Azure Application Insights | Microsoft Docs
description: Schrijf code als aanvragen wilt volgen met Application Insights, zodat u kunt profielen voor uw aanvragen ophalen
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: bdbca30d31febe37e6b568894179c88d834d3a83
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266685"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Code voor het volgen van aanvragen met Application Insights schrijven

Als u wilt zien profielen voor uw toepassing op de pagina van de prestaties, moet Application Insights worden aanvragen voor uw toepassing bijhouden. Application Insights kunnen automatisch bijhouden voor aanvragen voor toepassingen die zijn gebouwd op frameworks die al zijn geïnstrumenteerd, zoals ASP.net en ASP.Net Core. Maar voor andere toepassingen, zoals Azure Cloud Service-werkrollen en Service Fabric stateless API's van uw behoeften code schrijven om te zien van Application Insights, waar uw aanvragen beginnen en eindigen. Nadat u deze code geschreven aanvragen telemetrie wordt verzonden naar Application Insights en ziet u de telemetrie op de pagina van de prestaties en profielen voor deze aanvragen worden verzameld. 

Hier volgen de stappen die u moet ondernemen handmatig om aanvragen te volgen:


  1. Voeg de volgende code al vroeg in de levensduur van de toepassing:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Zie voor meer informatie over de configuratie van deze globale instrumentation [met Service Fabric met Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. Voor elk stukje code dat u wilt instrumenteren, voegt u een `StartOperation<RequestTelemetry>` **met** instructie rond het, zoals wordt weergegeven in het volgende voorbeeld:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        Aanroepen van `StartOperation<RequestTelemetry>` binnen een andere `StartOperation<RequestTelemetry>` bereik wordt niet ondersteund. U kunt `StartOperation<DependencyTelemetry>` in de geneste scope in plaats daarvan. Bijvoorbeeld:  
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```
