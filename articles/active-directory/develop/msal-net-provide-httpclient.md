---
title: Geef een HttpClient en -proxy (MSAL.NET) | Azure
description: Meer informatie over het leveren van uw eigen httpclient maakt en -proxy kunt koppelen aan Azure AD met behulp van Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 234c9d0724021017ec8c411d637420b05284ea52
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544161"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Uw eigen httpclient maakt en met behulp van MSAL.NET-proxy
Bij het initialiseren van een openbare client-toepassing, kunt u de `.WithHttpClientFactory method` voor uw eigen httpclient maakt.  Bieden van uw eigen HttpClient kunt geavanceerde scenario's die heel nauwkeurig bepalen van een HTTP-proxy, aanpassen van de gebruiker agent headers of forceren MSAL gebruik van een specifieke HttpClient (bijvoorbeeld in een ASP.NET Core web-apps /-API's).

## <a name="initialize-with-httpclientfactory"></a>Met HttpClientFactory initialiseren
Het volgende voorbeeld ziet u een `HttpClientFactory` en een openbare client-toepassing met deze vervolgens initialiseren:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient en Xamarin iOS
Wanneer met behulp van Xamarin iOS, wordt het aanbevolen om u te maken van een `HttpClient` die expliciet gebruikmaakt van de `NSURLSession`-op basis van de handler voor iOS 7 en hoger. MSAL.NET maakt automatisch een `HttpClient` die gebruikmaakt van `NSURLSessionHandler` voor iOS 7 en hoger. Lees voor meer informatie de [Xamarin iOS-documentatie voor HttpClient](/xamarin/cross-platform/macios/http-stack).