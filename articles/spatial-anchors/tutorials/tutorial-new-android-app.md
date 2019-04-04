---
title: 'Zelfstudie: Stapsgewijze instructies voor het maken van een nieuwe Android-app met behulp van Azure ruimtelijke ankers | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een nieuwe Android-app met behulp van Azure ruimtelijke ankers te maken.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 04/03/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 64f78b04d433c81302499addf15c3d19621bbf9f
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58919600"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-android-app-using-azure-spatial-anchors"></a>Zelfstudie: Stapsgewijze instructies voor het maken van een nieuwe Android-app met behulp van Azure ruimtelijke ankers

Deze zelfstudie leert u hoe u een nieuwe Android-app die ARCore functionaliteit kan worden geïntegreerd met Azure ruimtelijke ankers maakt.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:

- Een Windows- of macOS-machine met <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a>.
- Een <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">door een ontwikkelaar geactiveerd</a> en <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">voor ARCore geschikt</a> Android-apparaat.

## <a name="getting-started"></a>Aan de slag

Android Studio starten. In de **Welkom bij Android Studio** venster, klikt u op **Start een nieuw Android Studio-project**. Hebt u een project al is geopend, selecteert u **bestand**->**nieuw Project**.

In de **nieuw Project maken** venster onder de **telefoons en tablets** sectie **lege activiteit**, en klikt u op **volgende**. Klik vervolgens onder **minimaal API-niveau**, kiest u `API 26: Android 8.0 (Oreo)`, en zorg ervoor dat de **taal** is ingesteld op `Java`. Kunt u de projectnaam en locatie en naam van het pakket te wijzigen. Laat de overige opties zoals ze zijn. Klik op **Voltooien**. De **onderdeel installatieprogramma** wordt uitgevoerd. Nadat dit voltooid, klikt u op **voltooien**. Na het verwerken ervan, wordt de Android Studio de IDE geopend.

## <a name="trying-it-out"></a>De functies uitgeprobeerd

Als u wilt testen van uw nieuwe app, verbinding maken met uw developer-apparaat naar uw ontwikkelmachine met een USB-kabel. Klik op **uitvoeren**->**uitvoeren 'app'**. In de **implementatiedoel Selecteer** venster, selecteer uw apparaat en klikt u op **OK**. Android Studio de app wordt geïnstalleerd op het aangesloten apparaat en deze wordt gestart. U ziet nu "Hallo wereld!" in de app die wordt uitgevoerd op uw apparaat weergegeven. Klik op **uitvoeren**->**Stop 'app'**.

## <a name="integrating-arcore"></a>Integratie van _ARCore_

<a href="https://developers.google.com/ar/discover/" target="_blank">_ARCore_ </a> van Google-platform voor het bouwen van Augmented Reality ervaringen, zodat uw apparaat om bij te houden de positie wordt verplaatst en een eigen inzicht in de praktijk is gebaseerd.

Wijzigen `app\manifests\AndroidManifest.xml` om op te nemen van de volgende vermeldingen in de hoofdmap `<manifest>` knooppunt. Dit codefragment wordt een aantal dingen:

- Hiermee wordt de app toegang tot de camera van uw apparaat.
- Dit zorgt u ervoor dat uw app is alleen zichtbaar in de Google Play Store op apparaten die ondersteuning bieden voor ARCore.
- Het configureert de Google Play Store wilt downloaden en installeren van ARCore, als deze al is niet geïnstalleerd wanneer uw app is geïnstalleerd.

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera.ar" />

<application>
    ...
    <meta-data android:name="com.google.ar.core" android:value="required" />
    ...
</application>
```

Wijzigen `Gradle Scripts\build.gradle (Module: app)` om op te nemen van de volgende vermelding. Deze code zorgt ervoor dat uw app is gericht op ARCore versie 1.5 (ondersteuning voor ARCore 1.6 + op ruimtelijke ankers Azure wordt binnenkort beschikbaar). Na deze wijziging kunt u mogelijk ontvangt een melding van Gradle of u wilt synchroniseren: klik op **nu synchroniseren**.

```
dependencies {
    ...
    implementation 'com.google.ar:core:1.5.0'
    ...
}
```

## <a name="integrating-sceneform"></a>Integratie van _Sceneform_

<a href="https://developers.google.com/ar/develop/java/sceneform/" target="_blank">_Sceneform_ </a> is het eenvoudig om realistische 3D-scènes in Augmented Reality-apps, zonder dat u hoeft voor meer informatie over OpenGL weer te geven.

Wijzigen `Gradle Scripts\build.gradle (Module: app)` om op te nemen van de volgende vermeldingen. Deze code wordt kan uw app gebruiken taalconstructies met Java 8, die `Sceneform` is vereist. Dit zorgt u ervoor dat uw app is bedoeld voor `Sceneform` versie 1.5, omdat deze moet overeenkomen met de versie van uw App ARCore. Na deze wijziging kunt u mogelijk ontvangt een melding van Gradle of u wilt synchroniseren: klik op **nu synchroniseren**.

```
android {
    ...

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    ...
    implementation 'com.google.ar.sceneform.ux:sceneform-ux:1.5.0'
    ...
}
```

Open uw `app\res\layout\activity_main.xml`, en vervang de bestaande Hello World `<TextView>` element met de volgende ArFragment. Deze code zorgt ervoor dat de camerafeed om te worden weergegeven op het scherm voor het inschakelen van ARCore om bij te houden van de positie van uw apparaat wordt ingecheckt, krijgt.

```xml
<fragment android:name="com.google.ar.sceneform.ux.ArFragment"
    android:id="@+id/ux_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

[Opnieuw implementeren](#trying-it-out) uw app op uw apparaat zodra er meer valideren. Dit moment moet u gevraagd om de machtigingen van de camera. Na goedkeuring, ziet u dat uw camera feed rendering op uw scherm.

## <a name="place-an-object-in-the-real-world"></a>Plaats een object in de praktijk

We gaan maken en plaatst u een object met behulp van uw app. Voeg eerst toe de volgende invoer in uw `app\java\<PackageName>\MainActivity`:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=23-33)]

Voeg de volgende variabelen in uw `MainActivity` klasse:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=52-57)]

Voeg vervolgens de volgende code in uw `app\java\<PackageName>\MainActivity` `onCreate()` methode. Deze code wordt een listener met de naam koppelt `handleTap()`, die wordt gedetecteerd wanneer de gebruiker tikt op het scherm op uw apparaat. Als de Tik gebeurt op een Praktijkscenario oppervlak dat al is herkend door de ARCore bijhouden, wordt de listener wordt uitgevoerd.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=68-74,85&highlight=6-7)]

Voeg de volgende `handleTap()` methode, die wordt alles met elkaar verbinden. Het maakt een sphere, en plaats het op de locatie waarop u hebt getikt. De bol wordt in eerste instantie worden zwart, sinds `this.recommendedSessionProgress` is ingesteld op nul nu. Deze waarde wordt later worden aangepast.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-171,174-182,198-199)]

[Opnieuw implementeren](#trying-it-out) uw app op uw apparaat zodra er meer valideren. Deze tijd kunt u verplaatsen om uw apparaat om op te halen ARCore om te beginnen met het herkennen van uw omgeving. Tik vervolgens op het scherm voor het maken en de zwarte sphere plaats op het vlak van uw keuze.

## <a name="attach-a-local-azure-spatial-anchor"></a>Een lokale Azure ruimtelijke anker koppelen

Wijzigen `Gradle Scripts\build.gradle (Module: app)` om op te nemen van de volgende vermelding. Deze code wordt ervoor te zorgen dat uw app doelen Azure ruimtelijke ankers versie 1.0.2. Die al zei, die verwijst naar een recente versie van Azure ruimtelijke ankers zou moeten werken.

```
dependencies {
    ...
    implementation "com.microsoft.azure.spatialanchors:spatialanchors_jni:[1.0.2]"
    implementation "com.microsoft.azure.spatialanchors:spatialanchors_java:[1.0.2]"
    ...
}
```

Met de rechtermuisknop op `app\java\<PackageName>` -> **nieuwe**->**Java-klasse**. Stel **naam** naar _Mijneersteapp_, en **superklasse** naar _android.app.Application_. Laat de overige opties zoals ze zijn. Klik op **OK**. Een bestand met de naam `MyFirstApp.java` wordt gemaakt. Voeg de volgende import toe:

```java
import com.microsoft.CloudServices;
```

Voeg de volgende code in de nieuwe `MyFirstApp` klasse, die ruimtelijke ankers Azure zorgt u ervoor wordt geïnitialiseerd met de context van uw toepassing.

```java
    @Override
    public void onCreate() {
        super.onCreate();
        CloudServices.initialize(this);
    }
```

Nu wijzigen `app\manifests\AndroidManifest.xml` om op te nemen van de volgende vermelding in de hoofdmap `<application>` knooppunt. Deze code wordt de Toepassingsklasse die u hebt gemaakt in uw app koppelen.

```xml
    <application
        android:name=".MyFirstApp"
        ...
    </application>
```

Terug in `app\java\<PackageName>\MainActivity`, voeg de volgende invoer in het:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=33-40&highlight=2-8)]

Voeg de volgende variabelen in uw `MainActivity` klasse:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=57-60&highlight=3-4)]

Vervolgens gaan we toevoegen de volgende `initializeSession()` methode binnen uw `mainActivity` klasse. Wanneer met de naam, zorgt het ervoor dat de sessie van een Azure ruimtelijke ankers is gemaakt en tijdens het opstarten van uw app de juiste wijze geïnitialiseerd.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=89-97,146)]

Nu gaan we een toepassing aansluiten uw `initializeSession()` methode in uw `onCreate()` methode. We u Zorg er ook voor dat frames van de camera feed met ruimtelijke ankers-SDK van Azure worden verzonden voor verwerking.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=68-85&highlight=9-17)]

Voeg de volgende code in uw `handleTap()` methode. Er wordt een lokale Azure ruimtelijke anker koppelen aan de zwarte sphere die we in de praktijk wilt plaatsen.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-182,198-199&highlight=12-13)]

[Opnieuw implementeren](#trying-it-out) uw app zodra er meer. Verplaatsen van uw apparaat, tik op het scherm en een zwarte sphere plaatsen. Dit moment echter wilt uw code maken en een lokale Azure ruimtelijke anker koppelen aan uw sphere.

Voordat u verdergaat, moet u een Azure-ruimtelijke ankers te maken account-id en sleutel, als u deze nog niet hebt. Volg de volgende sectie om ze te verkrijgen.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Uw lokale anker uploaden naar de cloud

Zodra u uw ruimtelijke ankers Azure account-id en -sleutel hebt, kunnen we weer `app\java\<PackageName>\MainActivity`, voeg de volgende invoer in het:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=40-45&highlight=3-6)]

Voeg de volgende variabelen in uw `MainActivity` klasse:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=60-65&highlight=3-6)]

Voeg nu de volgende code in uw `initializeSession()` methode. Deze code kan eerst uw app om de voortgang te controleren dat de Azure-SDK voor ruimtelijke ankers frames worden verzameld van uw camera feed maakt. Als dit wel gebeurt, wordt de kleur van uw sphere gestart wijzigen van de oorspronkelijke zwart in grijs. Het wordt Schakel wit zodra voldoende frames worden verzameld voor het verzenden van het anker naar de cloud. Deze code wordt ten tweede leveren dat de referenties die nodig zijn om te communiceren met de cloud back-end. Dit is waar u uw app naar uw account-id en-sleutel gaat configureren. U ze hebt gekopieerd in een teksteditor als [instellen van de bron van ruimtelijke ankers](#create-a-spatial-anchors-resource).

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=89-120,142-146&highlight=11-36)]

Voeg vervolgens de volgende `uploadCloudAnchorAsync()` methode binnen uw `mainActivity` klasse. Wanneer met de naam, wordt deze methode wordt asynchroon wacht totdat voldoende frames worden verzameld van uw apparaat. Als dit gebeurt, zal het de kleur van uw sphere overschakelen naar geel en vervolgens deze start uw lokale Azure ruimtelijke anker uploaden naar de cloud. Nadat het uploaden is voltooid, wordt de code een anker-id geretourneerd.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=uploadCloudAnchorAsync)]

Ten slotte gaan we een toepassing aansluiten alles bij elkaar. In uw `handleTap()` methode, voeg de volgende code toe. Deze roept uw `uploadCloudAnchorAsync()` methode zodra uw sphere is gemaakt. Zodra de methode retourneert, worden de volgende code wordt uitgevoerd een laatste update van uw bol, wijzigt de kleur blauw.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-199&highlight=24-37)]

[Opnieuw implementeren](#trying-it-out) uw app zodra er meer. Verplaatsen van uw apparaat, tik op het scherm en uw sphere plaatsen. Dit moment echter verandert uw sphere de kleur van zwart naar wit, zoals camera frames worden verzameld. Zodra er voldoende frames, de sphere worden omgezet in geel en het uploaden van de cloud wordt gestart. Nadat het uploaden is voltooid, wordt uw sphere vervolgens blauw weergegeven. (Optioneel) u kunt ook gebruiken de `Logcat` venster in Android Studio voor het bewaken van de logboekberichten uw app verzendt. Bijvoorbeeld, de Sessievoortgang van de tijdens frame worden vastgelegd en de anker-id die de cloud geretourneerd nadat het uploaden is voltooid.

## <a name="locate-your-cloud-spatial-anchor"></a>Zoek uw cloud ruimtelijke anker

Een uw anker is geüpload naar de cloud, kunt u proberen deze opnieuw te zoeken. Eerst gaan we de volgende invoer in uw code toevoegen.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=45-48&highlight=3-4)]

Vervolgens voegen we de volgende code in uw `handleTap()` methode. Deze code wordt:

- Onze bestaande blauw sphere verwijderen uit het scherm.
- Onze Azure ruimtelijke ankers-sessie opnieuw starten. Zo zorgt u ervoor dat het anker gaan we zoeken afkomstig is van de cloud in plaats van het lokale anker die we hebben gemaakt.
- Een query voor het anker dat wordt geüpload naar de cloud geven.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=handleTap&highlight=10-19)]

Nu gaan we de code die wordt aangeroepen wanneer het anker we query's voor bevindt van een toepassing aansluiten. Binnen uw `initializeSession()` methode, voeg de volgende code toe. Dit fragment wordt maken en een groene sphere plaatsen zodra het anker van cloud-ruimtelijke zich bevindt. Deze ook inschakelen met scherm te tikken op opnieuw, zodat u kunt het volledige scenario zodra er meer herhalen: maken van een andere lokale anker, uploaden en zoek het opnieuw.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=initializeSession&highlight=34-53)]

Dat is alles. [Opnieuw implementeren](#trying-it-out) uw app een laatste keer dat voor het uitproberen van het hele end-to-end-scenario. Verplaatsen van uw apparaat en de zwarte sphere plaats. Vervolgens, blijven uw apparaat om vast te leggen van de camera frames totdat de sphere geel wordt te verplaatsen. Uw lokale anker wordt geüpload en uw sphere wordt blauw inschakelen. Ten slotte, tikt u op uw scherm zodra er meer, zodat uw lokale anker wordt verwijderd en we vervolgens voor het bijbehorende equivalent van de cloud zoeken. Doorgaan met uw apparaat verplaatsen totdat uw ruimtelijke anker cloud bevindt. Een groene sphere moet worden weergegeven in de juiste locatie, en u kunt spoel en herhaal het hele scenario opnieuw.

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-new-android-app-finished.md)]
