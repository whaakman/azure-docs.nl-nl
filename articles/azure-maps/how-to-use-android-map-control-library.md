---
title: Android map control gebruiken in Azure Maps | Microsoft Docs
description: Android map control van Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 15706addbe6b7f6310223978130158c792a47c89
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010664"
---
# <a name="how-to-use-the-azure-maps-android-sdk"></a>Het gebruik van de Android SDK van Azure Maps

De Android SDK van Azure kaarten is een bibliotheek van de kaart vector voor Android. In dit artikel begeleidt u door de processen van de installatie van de Android SDK van Azure Maps, het laden van een kaart en een pincode op de kaart te brengen.

## <a name="prerequisites"></a>Vereisten

### <a name="create-an-azure-maps-account"></a>Een Azure Maps-account maken

Als u wilt de procedures in dit artikel hebt voltooid, moet u eerst [maken van een Azure kaarten-account](how-to-manage-account-keys.md) in de prijscategorie S1.

### <a name="download-android-studio"></a>Android Studio downloaden

U moet de Android Studio downloaden en een project maakt met een lege activiteit voordat u de Android SDK van Azure Maps kunt installeren. U kunt [Android Studio downloaden](https://developer.android.com/studio/) gratis van Google. 

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

1. Voeg de volgende code aan de **alle projecten**, **opslagplaatsen** blokkeren uw **build.gradle** bestand.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Update uw **App/build.gradle** en voeg de volgende code toe:

    1. Voeg de volgende code aan het blok Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Werk uw blok met afhankelijkheden en voeg de volgende code toe:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Instellen van machtigingen door toe te voegen van de volgende XML-code naar uw **AndroidManifest.xml** bestand:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Bewerken **res** > **lay-out** > **activity_main.xml** , zodat het ziet als deze XML eruit:
    
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
            app:mapcontrol_cameraTargetLat="47.64"
            app:mapcontrol_cameraTargetLng="-122.33"
            app:mapcontrol_cameraZoom="12"
            />

    </FrameLayout>
    ```

5. Bewerken **MainActivity.java** om een kaart weergeven activiteitenklasse te maken. Nadat u deze hebt bewerkt, ziet deze er als deze klasse:

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
            AzureMaps.setSubscriptionKey("{subscription-key}");
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

![Android-kaart](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Een markering toevoegen aan de kaart

Als u wilt een markering toevoegen aan uw kaart, de `mapView.getMapAsync()` functie `MainActivity.java`. De laatste `MainActivity.java` code dient er als volgt:

```java
package com.example.myapplication;

import android.app.Activity;
import android.os.Bundle;
import com.mapbox.geojson.Feature;
import com.mapbox.geojson.Point;
import com.microsoft.azure.maps.mapcontrol.AzureMaps;
import com.microsoft.azure.maps.mapcontrol.MapControl;
import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
import com.microsoft.azure.maps.mapcontrol.source.DataSource;
import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
public class MainActivity extends AppCompatActivity {
    
    static{
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

    MapControl mapControl;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mapControl = findViewById(R.id.mapcontrol);

        mapControl.onCreate(savedInstanceState);

        mapControl.getMapAsync(map -> {
            DataSource dataSource = new DataSource();
            dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

            SymbolLayer symbolLayer = new SymbolLayer(dataSource);
            symbolLayer.setOptions(iconImage("my-icon"));

            map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            map.sources.add(dataSource);
            map.layers.add(symbolLayer);
        });
    }

    @Override
    public void onStart() {
        super.onStart();
        mapControl.onStart();
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

Voer uw toepassing opnieuw uit. U ziet een markering op de kaart, zoals hier wordt weergegeven:

![Android kaart pincode](./media/how-to-use-android-map-control-library/android-map-pin.png)