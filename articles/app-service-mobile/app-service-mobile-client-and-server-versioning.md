---
title: Client en server SDK-versies in Mobile Apps- en Mobile Services | Microsoft Docs
description: Lijst met client-SDK's en compatibiliteit met versies van de server-SDK voor Mobile Services en Azure Mobile Apps
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 37bf36af535eb9b5c8b0ba38434b71f1a6686811
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Client- en versiebeheer in Mobile Apps- en Mobile Services
De meest recente versie van Azure Mobile Services is de **Mobile Apps** functie van Azure App Service.

De Mobile Apps-client en server SDK's oorspronkelijk zijn gebaseerd op die in Mobile Services, maar ze zijn *niet* compatibel met elkaar.
Dat wil zeggen, moet u een *Mobile Apps* SDK voor clients met een *Mobile Apps* server SDK en op dezelfde manier voor *Mobile Services*. Dit contract wordt afgedwongen via een speciale headerwaarde die wordt gebruikt door de client en server SDK's, `ZUMO-API-VERSION`.

Opmerking: als dit document verwijst naar een *Mobile Services* back-end van het niet per se hoeft te worden gehost in Mobile Services. Het is nu mogelijk om te migreren van een mobiele service worden uitgevoerd op App Service zonder codewijzigingen, maar de service nog steeds gebruikt *Mobile Services* SDK-versies.

Zie het artikel voor meer informatie over het migreren naar App Service zonder codewijzigingen [een mobiele Service migreren naar Azure App Service].

## <a name="header-specification"></a>Header-specificatie
De sleutel `ZUMO-API-VERSION` mag worden opgegeven in de HTTP-header of de query-tekenreeks. De waarde is een versietekenreeks in het formulier **x.y.z**.

Bijvoorbeeld:

Https://service.azurewebsites.net/tables/TodoItem ophalen

HEADERS: ZUMO-API-VERSIE: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Buiten-versiecontrole uitschakelen
U kunt ervoor kiezen niet versie controleren door een waarde van **true** voor de app-instelling **MS_SkipVersionCheck**. Geef dit in uw web.config of in de sectie Toepassingsinstellingen van de Azure portal.

> [!NOTE]
> Er zijn een aantal gedragswijzigingen tussen Mobile Services en mobiele Apps, met name op het gebied van het offline synchroniseren, verificatie en pushmeldingen. U moet alleen versiecontrole na testen voltooien om ervoor te zorgen dat deze wijzigingen het van uw app-functionaliteit niet breken afmelden.
>
>

## <a name="summary-of-compatibility-for-all-versions"></a>Samenvatting van compatibiliteit voor alle versies
Het onderstaande diagram ziet u de compatibiliteit tussen alle typen van client en server. Een back-end is geclassificeerd als beide Mobile **Services** of Mobile **Apps** op basis van de server-SDK die wordt gebruikt.

|  | **Mobile Services** Node.js- of .NET | **Mobiele Apps** Node.js- of .NET |
| --- | --- | --- |
| [Mobile Services-clients] |Ok |Fout\* |
| [Clients voor mobiele Apps] |Fout\* |Ok |

\*Dit kan worden beheerd door te geven **MS_SkipVersionCheck**.

<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobile Services-client en server
De client-SDK's in de onderstaande tabel compatibel zijn met **Mobile Services**.

Opmerking: de Mobile Services-client SDK's *niet* een headerwaarde verzenden voor `ZUMO-API-VERSION`. Als de service deze kop- of queryreekswaarde ontvangt, een fout geretourneerd, tenzij u expliciet uit zoals hierboven beschreven hebt gekozen.

### <a name="MobileServicesClients"></a>Mobiele *Services* client-SDK's
| Clientplatform | Versie | De versieheaderwaarde |
| --- | --- | --- |
| Beheerde client (Windows, Xamarin) |[1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |N.v.t. |
| iOS |[2.2.2](http://aka.ms/gc6fex) |N.v.t. |
| Android |[2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) |N.v.t. |
| HTML |[1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |N.v.t. |

### <a name="mobile-services-server-sdks"></a>Mobiele *Services* server SDK's
| Server-platform | Versie | Geaccepteerde versie-header |
| --- | --- | --- |
| .NET |[WindowsAzure.MobileServices.Backend.* versie 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |** Geen versiekop ** |
| Node.js |(binnenkort) |**Er is geen versie-header** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Gedrag van de back-ends voor Mobile Services
| ZUMO-API-VERSIE | Waarde van MS_SkipVersionCheck | Antwoord |
| --- | --- | --- |
| Niet opgegeven |Alle |200 - OK |
| Een willekeurige waarde |True |200 - OK |
| Een willekeurige waarde |ONWAAR/niet opgegeven |400 - onjuiste aanvraag |

## <a name="2.0.0"></a>Azure Mobile Apps-client en server
### <a name="MobileAppsClients"></a>Mobiele *Apps* client-SDK's
Beginnen met de volgende versies van de client-SDK versiecontrole is geïntroduceerd voor **Azure Mobile Apps**:

| Clientplatform | Versie | De versieheaderwaarde |
| --- | --- | --- |
| Beheerde client (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobiele *Apps* server SDK's
Versiecontrole is opgenomen in de volgende server SDK-versies:

| Server-platform | SDK | Geaccepteerde versie-header |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[Azure mobile apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Gedrag van de back-ends voor mobiele Apps
| ZUMO-API-VERSIE | Waarde van MS_SkipVersionCheck | Antwoord |
| --- | --- | --- |
| x.y.z of Null zijn |True |200 - OK |
| Null |ONWAAR/niet opgegeven |400 - onjuiste aanvraag |
| 1.x.y |ONWAAR/niet opgegeven |400 - onjuiste aanvraag |
| 2.0.0-2.x.y |ONWAAR/niet opgegeven |200 - OK |
| 3.0.0-3.x.y |ONWAAR/niet opgegeven |400 - onjuiste aanvraag |

## <a name="next-steps"></a>Volgende stappen
* [een mobiele Service migreren naar Azure App Service]

[Mobile Services-clients]: #MobileServicesClients
[Clients voor mobiele Apps]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[een mobiele Service migreren naar Azure App Service]: app-service-mobile-migrating-from-mobile-services.md
