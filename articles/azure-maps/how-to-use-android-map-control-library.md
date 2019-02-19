---
title: Over het gebruik van Android kaartbesturingselement in Azure Maps | Microsoft Docs
description: Android kaartbesturingselement in Azure Maps gebruiken.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 57cc585d621c71872a4b7658c74f581c8998b245
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341076"
---
# <a name="how-to-use-azure-maps-android-sdk"></a>Het gebruik van Azure Maps Android SDK

De Azure-kaarten Android SDK is een vector maps-bibliotheek voor Android. In dit artikel leidt u door het proces van het installeren van de Android SDK van Azure Maps, het laden van een kaart en een pincode brengen.

## <a name="prerequisites-to-get-started"></a>Vereisten aan de slag

### <a name="create-an-azure-maps-account"></a>Een Azure Maps-account maken 

Als u wilt volgen de stappen in deze handleiding, moet u eerst om te zien [account en sleutels beheren](how-to-manage-account-keys.md) maken en beheren van uw account-abonnement met S1 prijscategorie.

### <a name="download-android-studio"></a>Android studio downloaden

U kunt downloaden [Android Studio](https://developer.android.com/studio/) gratis van Google. Als u wilt installeren van de Android SDK van Azure Maps, moet u eerst Android Studio downloaden en een project maakt met een lege activiteit.

## <a name="create-a-project-in-android-studio"></a>Maak een project in Android Studio

U moet een nieuw project maakt met een lege activiteit. Volg de stappen hieronder om een nieuw Android Studio-project maken:

1. Onder *Kies uw project*, 'Telefoon en Tablet' controleren als vormfactor die uw toepassing wordt uitgevoerd.
2. Klik op *lege activiteit* onder vormfactor en klikt u op **volgende**.
3. Onder *configureren van uw project*, selecteer `API 21: Android 5.0.0 (Lollipop)` als de minimale SDK. Dit is de laagste versie die wordt ondersteund door Android SDK van Azure Maps.
4. Accepteer de standaardwaarde `Activity Name` en `Layout Name` en klikt u op **voltooien**

Zie [Android Studio-documentatie](https://developer.android.com/studio/intro/) help Android Studio installeren en het maken van een nieuw project voor meer informatie.

![Een nieuw project maken](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Instellen van een virtueel apparaat

Android Studio kunt u een virtueel Android-apparaat op uw computer instellen. Dit kan helpen om te testen van uw toepassing bij het ontwikkelen van. Voor het instellen van een virtueel apparaat klikt u op het pictogram beheer van Android Virtual Device (AVD) in de rechterbovenhoek van het projectscherm van uw. Klik vervolgens op de **virtueelapparaat maken** knop. U krijgt ook naar de manager via **Tools > Android > AVD Manager** in de werkbalk. Uit de **telefoons** categorie, selecteer **Nexus 5 X** en klikt u op **volgende**.

Meer informatie over het instellen van een AVD in de [Android Studio-documentatie](https://developer.android.com/studio/run/managing-avds).

![Android Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-azure-maps-android-sdk"></a>Azure Maps Android SDK installeren

Voordat u verdergaat voor het bouwen van uw toepassing, de volgende stappen voor het installeren van Azure Maps Android SDK. 

1. Het volgende toevoegen aan de **alle projecten**, opslagplaatsen blokkeren uw **build.gradle** bestand.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Update uw **App/build.gradle** en voeg het volgende toe:

    1. Voeg het volgende toe aan het blok Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Werk uw blok met afhankelijkheden en voeg de volgende:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Instellen van machtigingen door toe te voegen van de volgende uw **AndroidManifest.xml**

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Bewerken **res > Opmaak > activity_main.xml**, zodat het ziet als de XML-code hieronder eruit:
    
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

5. Bewerken **MainActivity.java** om een kaart weergeven activiteitenklasse te maken. Na het bewerken van ziet het resultaat, zoals de onderstaande klasse:

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

Na het voltooien van de bovenstaande stappen, waarschijnlijk ontvangt u waarschuwingen van Android Studio op een van de tekst in de code. Als u wilt deze waarschuwingen oplossen, importeert u de klassen waarnaar wordt verwezen in `MainActivity.java`.

U kunt deze klassen automatisch importeren door te drukken `Alt` + `Enter`(`Option` + `Return` op Mac). 

Klik op de **uitvoeren 'App'** knop (of `Control` + `R` op een Mac) om uw toepassing te bouwen.

![Klik op uitvoeren](./media/how-to-use-android-map-control-library/run-app.png)

Het duurt een paar seconden voor android studio de toepassing te bouwen. Nadat de build voltooid is, kunt u uw toepassing testen in de geëmuleerde Android-apparaat. Er wordt een kaart, zoals hieronder wordt weergegeven.

![Android-kaart](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Een markering toevoegen aan de kaart

Als u wilt toevoegen een markering u aan bij uw kaart, toevoegen `mapView.getMapAsync()` functie de `MainActivity.java`. De laatste `MainActivity.java` ziet er als volgt:

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

Opnieuw uitvoeren van uw toepassing en ziet u een markering op de kaart zoals hieronder.

![Android kaart pincode](./media/how-to-use-android-map-control-library/android-map-pin.png)