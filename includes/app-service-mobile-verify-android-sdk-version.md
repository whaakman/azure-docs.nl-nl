---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 46cfb27b8bde95990d13ec4bca4e96f25cfe9dc5
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133819"
---
Vanwege de lopende ontwikkeling mogelijk de Android SDK-versie is geïnstalleerd in Android Studio niet overeen met de versie in de code. De Android SDK waarnaar wordt verwezen in deze zelfstudie is versie 26, het laatste nieuws op het moment van schrijven. Het versienummer verhogen als nieuwe versies van de SDK worden weergegeven en wordt geadviseerd om de meest recente beschikbare versie.

Er zijn twee symptomen van niet-overeenkomende versie:

- Wanneer u samenstellen of opnieuw samenstellen van het project, krijgt u mogelijk de foutberichten Gradle, zoals `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Standaard-Android objecten in de code die moet worden omgezet op basis van `import` instructies kunnen foutberichten worden gegenereerd.

Als een van deze wordt weergegeven, de versie van de Android SDK geïnstalleerd in Android Studio mogelijk niet overeen met de SDK-doel van het gedownloade project. Als u wilt controleren welke versie is, moet u de volgende wijzigingen aanbrengen:

1. In Android Studio, klikt u op **extra** > **Android** > **SDK Manager**. Als u niet de nieuwste versie van het Platform SDK hebt geïnstalleerd, klikt u op om het te installeren. Noteer het versienummer.

2. Op de **Projectverkenner** tabblad onder **Gradle-Scripts**, open het bestand **build.gradle (Module: app)**. Zorg ervoor dat de **compileSdkVersion** en **targetSdkVersion** zijn ingesteld op de meest recente SDK-versie geïnstalleerd. De `build.gradle` als volgt uitzien:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
