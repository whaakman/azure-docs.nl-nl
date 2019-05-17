---
title: Overwegingen voor Universal Windows Platform (Microsoft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke aandachtspunten bij het gebruik van Universal Windows Platform met de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83fb999b0cf66cfd8d96e82d23ed43626352a8aa
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544144"
---
# <a name="universal-windows-platform-specific-considerations-with-msalnet"></a>Overwegingen voor Universal Windows Platform-specifieke met MSAL.NET
Er zijn verschillende overwegingen waarmee u rekening houden moet bij het gebruik van MSAL.NET voor Xamarin iOS geldt.

## <a name="the-usecorporatenetwork-property"></a>De eigenschap UseCorporateNetwork
In het platform WinRT `PublicClientApplication` heeft de volgende Booleaanse eigenschap ``UseCorporateNetwork``. Met deze eigenschap kunt Win8.1 en UWP-toepassingen profiteren van geïntegreerde Windows-verificatie (en dus eenmalige aanmelding met de gebruiker aangemeld met het besturingssysteem) als de gebruiker aangemeld met een account in een federatieve Azure is AD-tenant. Dit maakt gebruik van Windows-Adresboek (Web Authentication Broker). 

> [!IMPORTANT]
> Als deze eigenschap instelt op true, wordt ervan uitgegaan dat de ontwikkelaar geïntegreerde Windows-verificatie (IWA) is ingeschakeld in de toepassing. Voor deze:
> - In de ``Package.appxmanifest`` voor uw UWP-toepassing, in de **mogelijkheden** tabblad, schakelt u de volgende mogelijkheden:
>   - Enterprise-verificatie
>   - Particuliere netwerken (Client en Server)
>   - Gedeelde gebruikerscertificaten

IWA is niet standaard ingeschakeld omdat aanvragen van de mogelijkheden voor Ondernemingsverificatie of gedeelde gebruikerscertificaten toepassingen vereisen een hoger niveau van controle om te worden geaccepteerd in de Windows Store en niet alle ontwikkelaars mogelijk wil een hogere uitvoeren de mate van verificatie. 

De onderliggende implementatie op het UWP-platform (Windows-Adresboek) werkt niet correct in zakelijke scenario's waarin voorwaardelijke toegang is ingeschakeld. Het symptoom is dat de gebruiker wil aanmelden met Windows hello en kies een certificaat, maar het certificaat voor de pincode is niet gevonden of de gebruiker ervoor kiest deze echter nooit om wordt gevraagd de pincode wordt voorgesteld. Een tijdelijke oplossing is om een alternatieve methode te gebruiken (gebruikersnaam en wachtwoord en telefoon verificatie), maar de ervaring is niet goed. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie vindt u in de volgende voorbeelden:

Voorbeeld | Platform | Description 
|------ | -------- | -----------|
|[active-directory-dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | Een Universal Windows Platform-clienttoepassing met behulp van msal.net, toegang tot de Microsoft Graph voor een gebruiker worden geverifieerd met Azure AD v2.0-eindpunt. <br>![Topologie](media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Een eenvoudige Xamarin Forms-app die laat zien hoe u MSAL MSA en Azure AD via het AAD v2.0-eindpunt te verifiëren en toegang tot de Microsoft Graph met het resulterende token. <br>![Topologie](media/msal-net-uwp-considerations/topology-xamarin-native.png)|