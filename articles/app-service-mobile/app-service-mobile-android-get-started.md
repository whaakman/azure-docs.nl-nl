---
title: Een Android-app maken in Azure App Service Mobile Apps | Microsoft Docs
description: Volg deze zelfstudie om aan de slag te gaan met back-ends voor mobiele apps van Azure voor Android-ontwikkeling.
services: app-service\mobile
documentationcenter: android
author: elamalani
manager: crdun
editor: ''
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: conceptual
ms.date: 5/6/2019
ms.author: emalani
ms.openlocfilehash: 238fde023ba1b5c8b21f181655c9633329c22699
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443611"
---
# <a name="create-an-android-app"></a>Een Android-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center investeert in nieuwe en geïntegreerde services centraal staat in de ontwikkeling van mobiele Apps. Ontwikkelaars kunnen gebruikmaken van **bouwen**, **Test** en **verdelen** services voor het instellen van de pijplijn voor continue integratie en levering. Zodra de app is geïmplementeerd, ontwikkelaars controleren de status en het gebruik van het gebruik van de app de **Analytics** en **Diagnostics** -services en Communiceer met gebruikers met behulp van de **Push** de service. Ontwikkelaars kunnen ook gebruikmaken van **Auth** om hun gebruikers te verifiëren en **gegevens** service behouden en synchroniseren van app-gegevens in de cloud. Bekijk [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-android-get-started) vandaag nog.
>

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Android-app met een back-end voor een mobiele apps van Azure.  U maakt zowel een nieuwe back-end voor een mobiele app als een eenvoudige Android-app voor *takenlijsten* die app-gegevens opslaat in Azure.

Het volgen van deze zelfstudie is een vereiste voor alle andere Android-zelfstudies over het gebruik van de functie Mobile Apps in Azure App Service.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De [Android-hulpprogramma's voor ontwikkelaars](https://developer.android.com/sdk/index.html), waaronder Integrated Development Environment van Android Studio en het meest recente Android-platform.
* Azure Mobile Android SDK.
* Een [actief Azure-account](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Een nieuwe back-end voor mobiele apps van Azure maken
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>De verbinding met een database maken en configureren van het project client en server
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-android-app"></a>De Android-app uitvoeren
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
