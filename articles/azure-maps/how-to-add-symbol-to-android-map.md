---
title: Toevoegen van een laag symbool op Android-kaarten in Azure kaarten | Microsoft Docs
description: Symbolen toevoegen aan een kaart met behulp van Azure Maps Android SDK
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: add6e23d023753e217c102dc946837a71a64c781
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64871075"
---
# <a name="add-a-symbol-layer-to-a-map-using-azure-maps-android-sdk"></a>Een symbool laag toevoegen aan een kaart met behulp van Azure Maps Android SDK

In dit artikel wordt beschreven hoe u gegevens uit een gegevensbron weergeven als een laag symbool op een kaart met behulp van de Android SDK van Azure Maps.

## <a name="prerequisites"></a>Vereisten

Volledig stappen in dit artikel, moet u installeren [Android SDK van Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) laden van een kaart.

## <a name="add-a-symbol-layer"></a>Een symboollaag toevoegen

Als u wilt toevoegen een markering op de kaart met behulp van het symbool-laag, de volgende stappen uit te voeren:

1. Bewerken **res** > **lay-out** > **activity_main.xml** , zodat het ziet als de volgende XML eruit:
    
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
            app:mapcontrol_centerLat="47.64"
            app:mapcontrol_centerLng="-122.33"
            app:mapcontrol_zoom="12"
            />

    </FrameLayout>
    ```

2. Kopieer het volgende codefragment in de **onCreate()** -methode van uw `MainActivity.java` klasse.

    ```Java
    mapControl.onReady(map -> {
    
        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a point feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
    
        //Add a custom image icon to the map resources.
        map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
    
        //Create a symbol layer and add it to the map.
        map.layers.add(new SymbolLayer(dataSource,
            iconImage("my-icon")));
        });
    
    ```
    
    Het bovenstaande codefragment verkrijgt eerst een Azure-kaarten map control-exemplaar met de **onReady()** terugbelmethode. Het maakt vervolgens een gegevensbron object via de **DataSource** klasse en voegt deze toe aan de kaart. Vervolgens wordt toegevoegd een **functie** met een punt geometrie toe. Een afbeelding van rode markering wordt vervolgens ingesteld als pictogram voor het symbool. Een **symbool laag** tekst of pictogrammen gebruikt om op basis van een punt-gegevens die zijn ingepakt in de gegevensbron als symbool op de kaart weer te geven. Een symbool-laag wordt vervolgens gemaakt en de gegevensbron wordt doorgegeven aan deze om weer te geven, en wordt vervolgens toegevoegd aan de lagen van de kaart.
    
    Na het toevoegen van het bovenstaande codefragment uw `MainActivity.java` moet er uitzien zoals hieronder:
    
    ```Java
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
                AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
            }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {
    
                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a point feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
            
                //Add a custom image icon to the map resources.
                map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            
                //Create a symbol layer and add it to the map.
                map.layers.add(new SymbolLayer(dataSource,
                    iconImage("my-icon")));
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
    
Op dit moment als u uw toepassing uitvoert ziet u een markering op de kaart, zoals hier wordt weergegeven:

<center>

![Android kaart pincode](./media/how-to-add-symbol-to-android-map/android-map-pin.png)</center>


## <a name="next-steps"></a>Volgende stappen

Meer moois toevoegen aan uw kaart, Zie:

> [!div class="nextstepaction"]
> [Vormen toevoegen aan een Android-kaart](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)