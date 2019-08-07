---
title: Geef een httpclient maakt en een proxy op (MSAL.NET) | Azure
description: Meer informatie over het bieden van uw eigen httpclient maakt en proxy om verbinding te maken met Azure AD met behulp van micro soft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: daae88cd8e76d0ae1af04c45a7191027e9adece9
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68834950"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Uw eigen httpclient maakt en proxy bieden met behulp van MSAL.NET
Wanneer u [een open bare client toepassing initialiseert](msal-net-initializing-client-applications.md), kunt u `.WithHttpClientFactory method` de gebruiken om uw eigen httpclient maakt te bieden.  Door uw eigen httpclient maakt te bieden, kunt u geavanceerde scenario's maken met een dergelijk nauw keurig beheer van een HTTP-proxy, het aanpassen van de headers van de gebruikers agent of het afdwingen van MSAL om een specifieke httpclient maakt te gebruiken (bijvoorbeeld in ASP.NET Core web apps/Api's).

## <a name="initialize-with-httpclientfactory"></a>Initialiseren met HttpClientFactory
In het volgende voor beeld ziet u `HttpClientFactory` hoe u een open bare client toepassing maakt en deze vervolgens initialiseert:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>Httpclient maakt en Xamarin iOS
Wanneer u Xamarin IOS gebruikt, wordt het aanbevolen een `HttpClient` expliciete `NSURLSession`handler te maken voor IOS 7 en hoger. MSAL.net maakt automatisch een `HttpClient` die gebruik `NSURLSessionHandler` maakt van Ios 7 en hoger. Raadpleeg de [Xamarin IOS-documentatie voor httpclient maakt](/xamarin/cross-platform/macios/http-stack)voor meer informatie.