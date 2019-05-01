---
title: Aan de slag met Android kaartbesturingselement in Azure Maps | Microsoft Docs
description: Android map control van Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e655b442ba9290d4b4525108521f2d1a0c766b48
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869798"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Aan de slag met Azure Maps Android SDK

De Android SDK van Azure kaarten is een bibliotheek van de kaart vector voor Android. In dit artikel leidt u door de processen van de installatie van de Android SDK van Azure Maps en het laden van een kaart.

## <a name="prerequisites"></a>Vereisten

### <a name="create-an-azure-maps-account"></a>Een Azure Maps-account maken

Als u wilt de procedures in dit artikel hebt voltooid, moet u eerst [maken van een Azure kaarten-account](how-to-manage-account-keys.md) in de prijscategorie S1.

### <a name="download-android-studio"></a>Android Studio downloaden

U moet de Android Studio downloaden en een project maakt met een lege activiteit voordat u de Azure Maps Android SDK installeert. U kunt [Android Studio downloaden](https://developer.android.com/studio/) gratis van Google. 

## <a name="create-a-project-in-android-studio"></a>Maak een project in Android Studio

Eerst moet u een nieuw project maakt met een lege activiteit. Deze stappen om een Android Studio-project te maken:

1. Onder **Kies uw project**, selecteer **telefoons en tablets**. Uw toepassing wordt uitgevoerd op deze vorm van meerdere factoren.
2. Op de **telefoons en tablets** tabblad **lege activiteit**, en selecteer vervolgens **volgende**.
3. Onder **configureren van uw project**, selecteer `API 21: Android 5.0.0 (Lollipop)` als de minimale SDK. Dit is de oudste versie wordt ondersteund door de Android SDK van Azure Maps.
4. Accepteer de standaardwaarde `Activity Name` en `Layout Name` en selecteer **voltooien**.

Zie de [Android Studio-documentatie](https://developer.android.com/studio/intro/) helpen bij de installatie van Android Studio en het maken van een nieuw project voor meer informatie.

![Een project maken](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Instellen van een virtueel apparaat

Android Studio kunt u een virtueel Android-apparaat op uw computer instellen. In dat geval kunt u uw toepassing tijdens het ontwikkelen van testen. Als u een virtueel apparaat instelt, selecteert u het pictogram beheer van Android Virtual Device (AVD) in de rechterbovenhoek van het projectscherm van uw en selecteer vervolgens **virtueelapparaat maken**. U kunt ook de AVD Manager ophalen door het selecteren van **extra** > **Android** > **AVD Manager** via de werkbalk. In de **telefoons** categorie, selecteer **Nexus 5 X**, en selecteer vervolgens **volgende**.

U kunt meer informatie over het instellen van een AVD in de [Android Studio-documentatie](https://developer.android.com/studio/run/managing-avds).

![Android Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Installeer de Android SDK van Azure Maps

De volgende stap in het bouwen van uw toepassing is het installeren van de Android SDK van Azure Maps. Deze stappen voor het installeren van de SDK:

1. Open het hoogste niveau **build.gradle** -bestand en voeg de volgende code aan de **alle projecten**, **opslagplaatsen** sectie blokkeren:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Update uw **App/build.gradle** en voeg de volgende code toe:
    
    1. Zorg ervoor dat van uw project **minSdkVersion** op API 21 of hoger.

    2. Voeg de volgende code naar de sectie Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Werk uw blok met afhankelijkheden en een nieuwe regel van de implementatie-afhankelijkheid toevoegen voor de meest recente Azure Maps Android SDK:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

    > [!Note]
    > De Android SDK van Azure Maps wordt regelmatig bijgewerkt en verbeterd. U ziet de [aan de slag met Android kaartbesturingselement](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) documentatie, om op te halen van het hoogste versienummer van de Azure Maps-implementatie. U kunt ook het versienummer van '0,2' instellen op '0 +"zodat deze altijd verwijzen naar de nieuwste versie.

3. Bewerken **res** > **lay-out** > **activity_main.xml** en vervang deze door het volgende:
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    </FrameLayout>
    ```

4. In de **MainActivity.java** u moet bestand:
    
    * invoer voor de Azure Maps-SDK toevoegen
    * Stel de verificatiegegevens van uw Azure-kaarten
    * de map control-instantie ophalen de **onCreate** methode

    Instellen van de verificatie-informatie over de AzureMaps klasse wereldwijd met behulp van de methoden setSubscriptionKey of setAadProperties maakt het u uw verificatiegegevens toevoegen op elke weergave hoeft dus niet. Map control bevat een eigen lifecycle-methoden voor het beheren van Android OpenGL levenscyclus, die rechtstreeks vanuit de betreffende activiteit moet worden aangeroepen. In de volgorde voor uw app juist van het kaartbesturingselement lifecycle methoden aanroepen, moet u overschrijven van de volgende levenscyclus methoden in de activiteit die het kaartbesturingselement bevat en de bijbehorende kaart besturingselement-methode aanroept. 

    Bewerk de **MainActivity.java** bestand als volgt:
    
    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);

        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }

    }

    ```

## <a name="import-classes"></a>Importeren van klassen

Nadat u de voorgaande stappen hebt voltooid, krijgt waarschijnlijk u waarschuwingen van Android Studio over een aantal van de code. Als u wilt deze waarschuwingen oplossen, importeert u de klassen waarnaar wordt verwezen in `MainActivity.java`.

U kunt deze klassen automatisch importeren door te selecteren (optie + terug op een Mac) Alt + Enter.

Selecteer de knop uitvoeren, zoals weergegeven in de volgende afbeelding (of druk op Control + R op een Mac), om uw toepassing te bouwen.

![Klik op uitvoeren](./media/how-to-use-android-map-control-library/run-app.png)

Android Studio duurt een paar seconden om de toepassing te bouwen. Nadat de bewerking voltooid is, kunt u uw toepassing testen in de geëmuleerde Android-apparaat. Hier ziet u een kaart zoals deze:

<center>

![Android-kaart](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="next-steps"></a>Volgende stappen

Items toevoegen aan uw kaart, Zie:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen aan een Android-kaart](https://review.docs.microsoft.com/azure/azure-maps/how-to-add-symbol-to-android-map)

> [!div class="nextstepaction"]
> [Vormen toevoegen aan een Android-kaart](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Stijl van kaart wijzigen in Android maps](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)


