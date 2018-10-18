---
title: 'Zelfstudie: Gezichten in een afbeelding detecteren en omlijsten - Face-API, Java voor Android'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie maakt u een Android-app die gebruikmaakt van de Face-API om gezichten in een afbeelding te detecteren en omlijsten.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: pafarley
ms.openlocfilehash: 08344e21d4f425a021bdefe840390ede8b3fb01e
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342378"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Zelfstudie: een Android-app maken om gezichten in een afbeelding te herkennen en te omlijsten

In deze zelfstudie maakt u een eenvoudige Android-toepassing die gebruikmaakt van de Java-klassebibliotheek van de Face-service, waarmee menselijke gezichten in een afbeelding kunnen worden herkend. De toepassing toont een geselecteerde afbeelding waarbij elk gedetecteerd gezicht door een rechthoek is omlijst. De volledige voorbeeldcode is via GitHub beschikbaar op [Detect and frame faces in an image on Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) (Gezichten in een afbeelding detecteren en omlijsten in Android).

![Android-schermafbeelding van een foto met gezichten in een rood vierkant](../Images/android_getstarted2.1.PNG)

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> - Een Android-app maken
> - De clientbibliotheek van de Face-service installeren
> - De clientbibliotheek gebruiken om gezichten in een afbeelding te detecteren
> - Een kader rond elk gedetecteerd gezicht tekenen

## <a name="prerequisites"></a>Vereisten

- U hebt een abonnementssleutel nodig om het voorbeeld uit te voeren. U kunt abonnementssleutels voor een gratis proefversie downloaden op [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
- [Android Studio](https://developer.android.com/studio/) met ten minste SDK 22 (vereist voor de clientbibliotheek van de Face-service).
- De clientbibliotheek van Face [com.microsoft.projectoxford:face:1.4.3](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.projectoxford%22) van Maven. U hoeft het pakket niet te downloaden. Hieronder vindt u de installatie-instructies.

## <a name="create-the-project"></a>Het project maken

Maak uw Android-toepassingsproject door deze stappen te volgen:

1. Open Android Studio. In deze zelfstudie wordt Android Studio 3.1 gebruikt.
1. Selecteer **Start a new Android Studio project**.
1. Wijzig in het scherm **Create Android Project** zo nodig de standaardvelden en klik op **Volgende**.
1. Gebruik in het scherm **Target Android Devices** de vervolgkeuzeselector om **API 22** of hoger te kiezen en klik op **Volgende**.
1. Selecteer **Empty Activity** en klik op **Next**.
1. Schakel het selectievakje **Backwards Compatibility** uit en klik op **Finish**.

## <a name="create-the-ui-for-selecting-and-displaying-the-image"></a>De gebruikersinterface voor het selecteren en weergeven van de afbeelding maken

Open *activity_main.xml*. De indelingseditor wordt weergegeven. Selecteer het tabblad **Text** en vervang de inhoud door de volgende code.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:id="@+id/imageView1"
        android:layout_above="@+id/button1"
        android:contentDescription="Image with faces to analyze"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Browse for face image"
        android:id="@+id/button1"
        android:layout_alignParentBottom="true"/>
</RelativeLayout>
```

Open *MainActivity.java* en vervang alles, behalve de eerste `package`-instructie, door de volgende code.

Met de code wordt een gebeurtenis-handler voor `Button` ingesteld, waardoor een nieuwe activiteit wordt gestart zodat de gebruiker een afbeelding kan selecteren. Als de afbeelding is geselecteerd, wordt deze in `ImageView` weergegeven.

```java
import java.io.*;
import android.app.*;
import android.content.*;
import android.net.*;
import android.os.*;
import android.view.*;
import android.graphics.*;
import android.widget.*;
import android.provider.*;

public class MainActivity extends Activity {
    private final int PICK_IMAGE = 1;
    private ProgressDialog detectionProgressDialog;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button button1 = (Button)findViewById(R.id.button1);
            button1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("image/*");
                startActivityForResult(Intent.createChooser(
                        intent, "Select Picture"), PICK_IMAGE);
            }
        });

        detectionProgressDialog = new ProgressDialog(this);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                        getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);

                // Uncomment
                //detectAndFrame(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }
}
```

De app kan nu een foto zoeken en in het venster weergeven, zoals hieronder wordt getoond.

![Android-schermafbeelding van een foto met gezichten](../Images/android_getstarted1.1.PNG)

## <a name="configure-the-face-client-library"></a>De Face-clientbibliotheek configureren

De Face-API is een cloud-API, die u kunt aanroepen met HTTPS-aanvragen. In de zelfstudie wordt de Face-clientbibliotheek gebruikt. Deze kapselt deze webaanvragen in om uw werk te vereenvoudigen.

Gebruik in het deelvenster **Project** de vervolgkeuzeselector om **Android** te selecteren. Vouw **Gradle Scripts** uit en open *build.gradle (Module: app)*.

Voeg een afhankelijkheid toe voor de Face-clientbibliotheek, `com.microsoft.projectoxford:face:1.4.3`, zoals weergegeven in de onderstaande schermafbeelding. Klik vervolgens op **Sync Now**.

![Android Studio-schermafbeelding van app-bestand build.gradle](../Images/face-tut-java-gradle.png)

Open **MainActivity.java** en voeg de volgende importeerinstructies toe:

```java
import com.microsoft.projectoxford.face.*;
import com.microsoft.projectoxford.face.contract.*;
```

## <a name="add-the-face-client-library-code"></a>De code van de Face-clientbibliotheek toevoegen

Voeg de volgende code in de `MainActivity`-klasse in, boven de `onCreate`-methode:

```java
private final String apiEndpoint = "<API endpoint>";
private final String subscriptionKey = "<Subscription Key>";

private final FaceServiceClient faceServiceClient =
        new FaceServiceRestClient(apiEndpoint, subscriptionKey);
```

Vervang `<API endpoint>` door het API-eindpunt dat aan uw sleutel is toegewezen. Abonnementssleutels voor een gratis proefversie worden gegenereerd in de regio **westcentralus**. Dus als u een abonnementssleutel voor een gratis proefversie gebruikt, dan wordt de instructie:

```java
apiEndpoint = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
```

Vervang `<Subscription Key>` door uw abonnementssleutel. Bijvoorbeeld:

```java
subscriptionKey = "0123456789abcdef0123456789ABCDEF"
```

Vouw in het deelvenster **Project** achtereenvolgens **app**, **manifests** uit en open *AndroidManifest.xml*.

Voeg het volgende element in als een direct onderliggend element van het `manifest`-element:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Bouw het project zodat op fouten kan worden gecontroleerd. U bent nu klaar om de Face-service aan te roepen.

## <a name="upload-an-image-to-detect-faces"></a>Een afbeelding uploaden om gezichten te detecteren

De eenvoudigste manier om gezichten te detecteren, is door de `FaceServiceClient.detect`-methode aan te roepen. In deze methode wordt de API-methode [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) ingepakt en wordt een reeks `Face`'s geretourneerd.

Elke geretourneerde `Face` bevat een rechthoek om de locatie ervan aan te geven, plus een reeks optionele gezichtskenmerken. In dit voorbeeld zijn alleen de locaties van de gezichten vereist.

Als er een fout optreedt, geeft een `AlertDialog` de onderliggende oorzaak weer.

Voeg de volgende methoden in de `MainActivity`-klasse in.

```java
// Detect faces by uploading a face image.
// Frame faces after detection.
private void detectAndFrame(final Bitmap imageBitmap) {
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
    ByteArrayInputStream inputStream =
            new ByteArrayInputStream(outputStream.toByteArray());

    AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                String exceptionMessage = "";

                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0],
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null          // returnFaceAttributes:
                                /* new FaceServiceClient.FaceAttributeType[] {
                                    FaceServiceClient.FaceAttributeType.Age,
                                    FaceServiceClient.FaceAttributeType.Gender }
                                */
                        );
                        if (result == null){
                            publishProgress(
                                    "Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(String.format(
                                "Detection Finished. %d face(s) detected",
                                result.length));
                        return result;
                    } catch (Exception e) {
                        exceptionMessage = String.format(
                                "Detection failed: %s", e.getMessage());
                        return null;
                    }
                }

                @Override
                protected void onPreExecute() {
                    //TODO: show progress dialog
                }
                @Override
                protected void onProgressUpdate(String... progress) {
                    //TODO: update progress
                }
                @Override
                protected void onPostExecute(Face[] result) {
                    //TODO: update face frames
                }
            };

    detectTask.execute(inputStream);
}

private void showError(String message) {
    new AlertDialog.Builder(this)
    .setTitle("Error")
    .setMessage(message)
    .setPositiveButton("OK", new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
        }})
    .create().show();
}
```

## <a name="frame-faces-in-the-image"></a>Gezichten in de afbeelding omlijsten

Voeg de volgende helper-methode in de `MainActivity`-klasse in. Met deze methode wordt een rechthoek rond elk gedetecteerd gezicht getekend.

```java
private static Bitmap drawFaceRectanglesOnBitmap(
        Bitmap originalBitmap, Face[] faces) {
    Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
    Canvas canvas = new Canvas(bitmap);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.STROKE);
    paint.setColor(Color.RED);
    paint.setStrokeWidth(10);
    if (faces != null) {
        for (Face face : faces) {
            FaceRectangle faceRectangle = face.faceRectangle;
            canvas.drawRect(
                    faceRectangle.left,
                    faceRectangle.top,
                    faceRectangle.left + faceRectangle.width,
                    faceRectangle.top + faceRectangle.height,
                    paint);
        }
    }
    return bitmap;
}
```

Voltooi de `AsyncTask`-methoden, aangegeven door de `TODO`-opmerkingen, in de `detectAndFrame`-methode. Als dit lukt, wordt de geselecteerde afbeelding met omlijste gezichten weergegeven in `ImageView`.

```java
@Override
protected void onPreExecute() {
    detectionProgressDialog.show();
}
@Override
protected void onProgressUpdate(String... progress) {
    detectionProgressDialog.setMessage(progress[0]);
}
@Override
protected void onPostExecute(Face[] result) {
    detectionProgressDialog.dismiss();
    if(!exceptionMessage.equals("")){
        showError(exceptionMessage);
    }
    if (result == null) return;
    ImageView imageView = findViewById(R.id.imageView1);
    imageView.setImageBitmap(
            drawFaceRectanglesOnBitmap(imageBitmap, result));
    imageBitmap.recycle();
}
```

Verwijder ten slotte in de `onActivityResult`-methode de opmerkingen bij de aanroep naar de `detectAndFrame`-methode, zoals hieronder aangegeven.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
        Uri uri = data.getData();
        try {
            Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                    getContentResolver(), uri);
            ImageView imageView = findViewById(R.id.imageView1);
            imageView.setImageBitmap(bitmap);

            // Uncomment
            detectAndFrame(bitmap);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## <a name="run-the-app"></a>De app uitvoeren

Voer de toepassing uit en blader naar een afbeelding met een gezicht. Wacht enkele seconden, zodat de Face-service kan reageren. Het resultaat dat volgt is soortgelijk aan de afbeelding hieronder:

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a>Samenvatting

In deze zelfstudie hebt u het basisproces geleerd voor het gebruik van de Face-service en een toepassing gemaakt om omlijste gezichten in een afbeelding weer te geven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het detecteren en gebruiken van gezichtskenmerken.

> [!div class="nextstepaction"]
> [Gezichten in afbeeldingen detecteren](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

Onderzoek de Face-API's die worden gebruikt voor het detecteren van gezichten en hun kenmerken, zoals de houding, het geslacht, de stand van het hoofd, de gezichtsbeharing en een eventuele bril.

> [!div class="nextstepaction"]
> [Verwijzing naar de Face-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).