---
title: Schrijf code voor het volgen van aanvragen met Azure Application Insights | Microsoft Docs
description: Schrijf code als aanvragen wilt volgen met Application Insights, zodat u kunt profielen voor uw aanvragen ophalen.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 4782e560b580b7f565724dbb35ed9876bffdc256
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882060"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Code voor het volgen van aanvragen met Application Insights schrijven

Als u wilt weergeven van profielen voor uw toepassing op de pagina van de prestaties, moet Azure Application Insights om aanvragen voor uw toepassing te volgen. Application Insights kunnen aanvragen voor toepassingen die zijn gebouwd op al zijn geïnstrumenteerd frameworks automatisch bijhouden. Twee voorbeelden zijn ASP.NET en ASP.NET Core. 

Voor andere toepassingen, zoals Azure Cloud Services-werkrollen en Service Fabric stateless API's, moet u code schrijven om te zien van Application Insights waar uw verzoeken, begin en einde. Nadat u deze code hebt geschreven, worden aanvragen telemetrie wordt verzonden naar Application Insights. U kunt de telemetrie bekijken op de pagina van de prestaties en profielen worden verzameld voor deze aanvragen. 

Handmatig als aanvragen wilt volgen, het volgende doen:

  1. Voeg de volgende code al vroeg in de levensduur van de toepassing:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Zie voor meer informatie over de configuratie van deze globale instrumentation [met Service Fabric met Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. Voor elk stukje code dat u wilt instrumenteren, voegt u een `StartOperation<RequestTelemetry>` **met behulp van** instructie rond het, zoals wordt weergegeven in het volgende voorbeeld:

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
