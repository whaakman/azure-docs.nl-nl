---
title: Style-functies van Azure Maps toewijzen | Microsoft Docs
description: Meer informatie over Azure Maps stijl verwante functies voor Android-SDK.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 3c8c5d4bae16d8e15c8f2c5b1cc8e00eb14e4ce3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870970"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Stijl van de kaart met behulp van Azure Maps Android SDK instellen

In dit artikel ziet u twee manieren om in te stellen met behulp van de Android SDK van Azure Maps stijlen. Azure kaarten is zes verschillende maps stijlen om uit te kiezen. Zie voor meer informatie over ondersteunde kaartstijlen [kaartstijlen ondersteund in Azure Maps](./supported-map-styles.md).


## <a name="prerequisites"></a>Vereisten

Als u wilt het proces in dit artikel hebt voltooid, moet u installeren [Android SDK van Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) laden van een kaart.


## <a name="set-map-style-in-the-layout"></a>De stijl van de kaart in de lay-out instellen

U kunt de stijl van een kaart in het bestand lay-out instellen voor uw activiteitenklasse. Bewerken **res > Opmaak > activity_main.xml**, zodat het er zoals hieronder uitziet:

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
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

De `mapcontrol_style` kenmerk hierboven stelt de stijl van de kaart aan **grayscale_dark**. 

<center>

![style-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>De stijl van de kaart in de activiteitenklasse instellen

Stijl van de kaart kan worden ingesteld in de activiteitenklasse. Kopieer het volgende codefragment in de **onCreate()** -methode van uw `MainActivity.java` klasse. Hierdoor wordt de stijl van de kaart ingesteld op **satellite_road_labels**.

```Java
    mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![style-satellite-road-labels](./media/set-android-map-styles/satellite-road-labels.png)</center>