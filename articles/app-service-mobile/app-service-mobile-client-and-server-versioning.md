---
title: SDK-versiebeheer voor client en server in Mobile Apps en Mobile Services | Microsoft Docs
description: Lijst met client-SDK's en compatibiliteit met server SDK-versies voor Mobile Services en Azure Mobile Apps
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: cfa6a363725c35083b32d6de1dd1371777f91907
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240292"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Versiebeheer voor client en server in Mobile Apps en Mobile Services
De meest recente versie van Azure Mobile Services is de **Mobile Apps** functie van Azure App Service.

De client en server SDK's voor Mobile Apps oorspronkelijk zijn gebaseerd op die in Mobile Services, maar ze zijn *niet* compatibel zijn met elkaar.
Dat wil zeggen, moet u een *Mobile Apps* client-SDK met een *Mobile Apps* server SDK en op dezelfde manier voor *Mobile Services*. Deze overeenkomst wordt afgedwongen via een speciale headerwaarde die wordt gebruikt door de client en server-SDK's, `ZUMO-API-VERSION`.

Opmerking: wanneer dit document verwijst naar een *Mobile Services* back-end, deze niet per se hoeft te worden gehost op Mobile Services. Het is nu mogelijk voor het migreren van een mobiele service om uit te voeren in App Service zonder codewijzigingen, maar de service dan nog steeds gebruiken *Mobile Services* SDK-versies.

Zie het artikel [migreren een mobiele Service in Azure App Service] voor meer informatie over het migreren naar App Service zonder codewijzigingen.

## <a name="header-specification"></a>Header-specificatie
De sleutel `ZUMO-API-VERSION` kan worden opgegeven in de HTTP-header of de query-tekenreeks. De waarde is een versietekenreeks in het formulier **x.y.z**.

Bijvoorbeeld:

TOEVOEGEN https://service.azurewebsites.net/tables/TodoItem

HEADERS: ZUMO-API-VERSIE: 2.0.0

VERZENDEN https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Wanneer u geen gebruik versie controleren
U kunt afmelden voor versie controleren door een waarde van **waar** voor de app-instelling **MS_SkipVersionCheck**. Geef dit in uw web.config of in het gedeelte instellingen van de toepassing van de Azure-portal.

> [!NOTE]
> Er zijn een aantal gedragswijzigingen tussen Mobile Services en mobiele Apps, met name op het gebied van offline synchroniseren, verificatie en pushmeldingen te verzenden. U moet alleen afmelden versiecontrole na testen voltooien om ervoor te zorgen dat deze wijzigingen de functionaliteit van uw app niet verbreken.

## <a name="2.0.0"></a>Azure Mobile Apps-client en server
### <a name="MobileAppsClients"></a> Mobiele *Apps* client-SDK's
Beginnen met de volgende versies van de client-SDK versie controleren is geïntroduceerd voor **Azure Mobile Apps**:

| Clientplatform | Versie | De versieheaderwaarde |
| --- | --- | --- |
| Beheerde client (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>Mobiele *Apps* server-SDK's
Versiecontrole van is opgenomen in de volgende server SDK-versies:

| Server-platform | SDK | Geaccepteerde versie-header |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[azure-mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Gedrag van de back-ends voor mobiele Apps
| ZUMO-API-VERSIE | Waarde van MS_SkipVersionCheck | Antwoord |
| --- | --- | --- |
| x.y.z of null zijn |True |200 - OK |
| Null |ONWAAR/niet opgegeven |400 - Ongeldige aanvraag |
| 1.x.y |ONWAAR/niet opgegeven |400 - Ongeldige aanvraag |
| 2.0.0-2.x.y |ONWAAR/niet opgegeven |200 - OK |
| 3.0.0-3.x.y |ONWAAR/niet opgegeven |400 - Ongeldige aanvraag |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
