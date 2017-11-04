---
title: Echte gebruiker metingen aan Azure Traffic Manager met Visual Studio Mobile Center | Microsoft Docs
description: Instellen van uw mobiele toepassing die is ontwikkeld met behulp van Visual Studio Mobile Center echte gebruiker metingen verzenden aan Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 756496e5291d932ee9ac89265291e6892c4304fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-send-real-user-measurements-to-traffic-manager-with-visual-studio-mobile-center"></a>Het verzenden van echte metingen van de gebruiker aan Traffic Manager met Visual Studio Mobile Center

>[!NOTE]
>De functie echte gebruiker metingen in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

U kunt instellen van uw mobiele toepassing die is ontwikkeld met behulp van Visual Studio Mobile Center verzenden van echte metingen van de gebruiker aan Traffic Manager door de stappen te volgen:

>[!NOTE]
> Verzenden van echte metingen van de gebruiker naar het Traffic manager is op dit moment wordt alleen ondersteund voor Android.

Voor het configureren van echte metingen van de gebruiker, moet u een sleutel te verkrijgen softwareontwikkelaars van uw app met de RUMSECTOR pakket.

## <a name="step-1-obtain-a-key"></a>Stap 1: Een sleutel ophalen
    
De metingen die u maakt en verzonden naar het Traffic Manager van uw clienttoepassing worden geïdentificeerd door de service met een unieke tekenreeks met de naam van de sleutel echte gebruiker metingen (RUM). U krijgt een RUMSECTOR sleutel met de Azure portal, een REST-API of met behulp van de PowerShell / CLI-interfaces.

Als u de RUM sleutel met Azure portal met de volgende procedure:
   1. Vanuit een browser, moet u zich aanmelden bij de Azure-portal. Als u nog een account hebt, kunt u zich aanmelden voor een gratis proefversie van één maand.
   2. In de portal zoekbalk, zoekt u de naam van het Traffic Manager-profiel dat u wilt wijzigen en klik vervolgens op het Traffic Manager-profiel in de resultaten die de weergegeven.
   3. Klik in de pagina Traffic Manager-profiel op **echte gebruiker metingen** onder **instellingen**.
   4. Klik op **sleutel genereren** een nieuwe RUM sleutel te maken.
        
   ![Echte gebruiker metingen sleutel genereren](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Afbeelding 1: Echte gebruiker metingen genereren van sleutels**

   5.   De pagina wordt weergegeven voor de RUM-sleutel die wordt gegenereerd en een JavaScript-codefragment die moet worden ingesloten in de HTML-pagina.
 
   ![JavaScript-code voor de sleutel van echte metingen van de gebruiker](./media/traffic-manager-create-rum-visual-studio/rum-key.png)

   **Afbeelding 2: Echte gebruikerssleutel van metingen en meting JavaScript**
 
   6. Klik op de **kopie** knop om te kopiëren van de sleutel RUM. 

## <a name="step-2-instrument-your-app-with-the-rum-package-of-mobile-center-sdk"></a>Stap 2: Softwareontwikkelaars uw app met het RUMSECTOR Mobile Center SDK-pakket

Als u geen ervaring met Visual Studio Mobile Center, gaat u naar de [website](https://mobile.azure.com). Zie voor gedetailleerde instructies voor het SDK-integratie [aan de slag met de Android SDK](https://docs.microsoft.com/mobile-center/sdk/getting-started/Android).

Voer de volgende procedure voor het gebruik van echte metingen van de gebruiker:

1.  De SDK toevoegen aan het project

    Tijdens de preview van de RUM ATM-SDK moet u expliciet verwijzen naar opslagplaats van het pakket.

    In uw **app/build.gradle** bestand de volgende regels toevoegen:

    ```groovy
    repositories {
        maven {
            url "https://dl.bintray.com/mobile-center/mobile-center-snapshot"
        }
    }
    ```
    In uw **app/build.gradle** bestand de volgende regels toevoegen:

    ```groovy
    dependencies {   
     
        def mobileCenterSdkVersion = '0.12.1-16+3fe5b08'
        compile "com.microsoft.azure.mobile:mobile-center-rum:${mobileCenterSdkVersion}"
    }
    ```

2. Start de SDK

    Open de belangrijkste activiteitklasse van uw app en voeg de volgende importinstructies:

    ```java
    import com.microsoft.azure.mobile.MobileCenter;
    import com.microsoft.azure.mobile.rum.RealUserMeasurements;
    ```

    Zoek naar de `onCreate` retouraanroep in hetzelfde bestand en voeg de volgende code:

    ```java
    RealUserMeasurements.setRumKey("<Your RUM Key>");
    MobileCenter.start(getApplication(), "<Your Mobile Center AppSecret>", RealUserMeasurements.class);
    ```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [echte metingen van de gebruiker](traffic-manager-rum-overview.md)
- Meer informatie over [de werking van Traffic Manager](traffic-manager-overview.md)
- Meer informatie over [Mobile Center](https://docs.microsoft.com/mobile-center/)
- [Aanmelden](https://mobile.azure.com) voor mobiele Center
- Meer informatie over de [verkeersroutering methoden](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

