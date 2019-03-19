---
title: Real-User-metingen naar Azure Traffic Manager met Visual Studio Mobile Center | Microsoft Docs
description: Instellen van uw mobiele toepassing ontwikkeld met behulp van Visual Studio Mobile Center voor het verzenden van Real-User-metingen naar Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 1a5b883a8c9688d4545c0e98c00f78a2e982a611
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58079708"
---
# <a name="how-to-send-real-user-measurements-to-traffic-manager-with-visual-studio-mobile-center"></a>Over het verzenden van Real-User-metingen naar Traffic Manager met Visual Studio Mobile Center

U kunt instellen van uw mobiele toepassing ontwikkeld met behulp van Visual Studio Mobile Center voor het verzenden van Real-User-metingen naar Traffic Manager door de stappen te volgen:

>[!NOTE]
> Echte Gebruikersmetingen verzenden naar Traffic manager wordt op dit moment alleen ondersteund voor Android.

Voor het configureren van Real User Measurements, moet u een sleutel ophalen en uw app instrumenteren met het pakket uitvoeren.

## <a name="step-1-obtain-a-key"></a>Stap 1: Een sleutel ophalen
    
De metingen die u maakt en verzonden naar Traffic Manager van uw clienttoepassing worden aangeduid met de service met een unieke tekenreeks, de sleutel echte gebruiker metingen (RUM) genoemd. U kunt een uitvoeren met behulp van Azure portal, een REST-API-sleutel ophalen of met behulp van de PowerShell / CLI-interfaces.

Als u de RUM-sleutel met behulp van Azure portal met behulp van de volgende procedure:
1. Vanuit een browser, moet u zich aanmelden bij de Azure-portal. Als u nog een account hebt, kunt u zich aanmelden voor een gratis proefversie van één maand.
2. Zoek in de zoekbalk van de portal, de naam van het Traffic Manager-profiel dat u wilt wijzigen en klik vervolgens op het Traffic Manager-profiel in de resultaten die de weergegeven.
3. Klik in de pagina Traffic Manager-profiel op **Real User Measurements** onder **instellingen**.
4. Klik op **sleutel genereren** om een nieuwe RUM-sleutel te maken.
        
   ![Real User Measurements sleutel genereren](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Afbeelding 1: Real-User-metingen genereren**

5. De pagina wordt weergegeven de RUM-sleutel die wordt gegenereerd en een JavaScript-codefragment die moeten worden ingesloten in de HTML-pagina.
 
   ![JavaScript-code voor Real User Measurements sleutel](./media/traffic-manager-create-rum-visual-studio/rum-key.png)

   **Afbeelding 2: Real-User Meetsleutel en meting JavaScript**
 
6. Klik op de **kopie** knop om te kopiëren van de sleutel RUM. 

## <a name="step-2-instrument-your-app-with-the-rum-package-of-mobile-center-sdk"></a>Stap 2: Instrumenteer uw app met het uitvoeren van Mobile Center SDK-pakket

Als u geen ervaring met Visual Studio Mobile Center, gaat u naar de [website](https://mobile.azure.com). Zie voor gedetailleerde instructies voor het SDK-integratie [aan de slag met de Android SDK](https://docs.microsoft.com/mobile-center/sdk/getting-started/Android).

Voer de volgende procedure voor het gebruik van Real User Measurements:

1.  De SDK toevoegen aan het project

    Tijdens de Preview-versie van de ATM-RUM SDK moet u expliciet verwijzen naar de pakketopslagplaats.

    In uw **App/build.gradle** bestand en voeg de volgende regels:

    ```groovy
    repositories {
        maven {
            url "https://dl.bintray.com/mobile-center/mobile-center-snapshot"
        }
    }
    ```
    In uw **App/build.gradle** bestand en voeg de volgende regels:

    ```groovy
    dependencies {
     
        def mobileCenterSdkVersion = '0.12.1-16+3fe5b08'
        compile "com.microsoft.azure.mobile:mobile-center-rum:${mobileCenterSdkVersion}"
    }
    ```

2. Start de SDK

    Openen van de belangrijkste activiteitklasse van uw app en voeg de volgende importinstructies toe:

    ```java
    import com.microsoft.azure.mobile.MobileCenter;
    import com.microsoft.azure.mobile.rum.RealUserMeasurements;
    ```

    Zoek de `onCreate` callback in hetzelfde bestand en voeg de volgende code toe:

    ```java
    RealUserMeasurements.setRumKey("<Your RUM Key>");
    MobileCenter.start(getApplication(), "<Your Mobile Center AppSecret>", RealUserMeasurements.class);
    ```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Real User Measurements](traffic-manager-rum-overview.md)
- Informatie over [hoe Traffic Manager werkt](traffic-manager-overview.md)
- Meer informatie over [Mobile Center](https://docs.microsoft.com/mobile-center/)
- [Meld u](https://mobile.azure.com) voor Mobile Center
- Meer informatie over de [routeringsmethoden voor verkeer](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

