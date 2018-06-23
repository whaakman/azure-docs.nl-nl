---
title: Java-API wordt geconfronteerd voor Android-zelfstudie | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Maak een eenvoudige Android-app die gebruikmaakt van de cognitieve Services Face-API om te detecteren en menselijke vlakken in een installatiekopie van het frame.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 5164a261d482d0cca3842a973d2109b17999bd25
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345676"
---
# <a name="getting-started-with-face-api-in-java-for-android-tutorial"></a>Aan de slag met Face-API in Java voor Android-zelfstudie

In deze zelfstudie leert u te maken en een eenvoudige Android-toepassing die roept de Face-API om menselijke vlakken in een installatiekopie vast te ontwikkelen. De toepassing wordt het resultaat door framing de vlakken die worden gedetecteerd.

![GettingStartedAndroid](../Images/android_getstarted2.1.PNG)

## <a name="preparation"></a> Voorbereiding

Als u wilt gebruiken de zelfstudie, moet u de volgende vereisten:

- Android Studio en SDK is geïnstalleerd
- Android-apparaat (optioneel voor testdoeleinden).

## <a name="step1"></a>Stap 1: Abonneren voor Face-API en de abonnementssleutel van uw ophalen

Voordat u een Face-API gebruiken, moet u zich aanmelden voor een abonnement op Face-API in de portal cognitieve Microsoft-Services. Zie [abonnementen](https://azure.microsoft.com/try/cognitive-services/). Primaire en secundaire sleutel kan worden gebruikt in deze zelfstudie.

## <a name="step2"></a>Stap 2: Maak het toepassingsframework

In deze stap maakt u een Android-toepassing-project voor het implementeren van de standaard gebruikersinterface voor het ophalen en weergeven van een installatiekopie. Gewoon de instructies hieronder: 

1. Open Android Studio.
2. Klik in het menu bestand **nieuw Project...**
3. Naam van de toepassing **MyFirstApp**, en klik op volgende. 

    ![GettingStartAndroidNewProject](../Images/AndroidNewProject.png)

4. Kies doelplatform waar nodig, en klik op volgende. 

    ![GettingStartAndroidNewProject2](../Images/AndroidNewProject2.png)

5. Selecteer **Basic activiteit** en klik op volgende.
6. De activiteit als volgt een naam en klik op voltooien. 

    ![GettingStartAndroidNewProject4](../Images/AndroidNewProject4.png)

7. Open **activity_main.xml**, ziet u de Editor voor de indeling van deze activiteit.
8. Tekstbestand bron weergeven en bewerk vervolgens de indeling van de activiteit:

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
     
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:id="@+id/imageView1"
            android:layout_above="@+id/button1" />
    
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Browse"
            android:id="@+id/button1"
            android:layout_alignParentBottom="true" />
    </RelativeLayout>
    ```  

9. Open **MainActivity.java** en voeg de volgende instructies voor importeren toe aan het begin van het bestand:

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
    ```
      
    Wijzig de klasse in de tweede plaats als volgt:  
    
    ```java
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
                Intent gallIntent = new Intent(Intent.ACTION_GET_CONTENT);
                gallIntent.setType("image/*");
                startActivityForResult(Intent.createChooser(gallIntent, "Select Picture"), PICK_IMAGE);
            }
        });
         
        detectionProgressDialog = new ProgressDialog(this);
    }
    
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
                } catch (IOException e) {
                e.printStackTrace();
                }
        }
    }
    ```

Nu kunt uw app zoeken naar een foto uit de galerie en weer te geven in het venster vergelijkbaar met de onderstaande afbeelding:

![GettingStartAndroidUI](../Images/android_getstarted1.1.PNG)

## <a name="step3"></a>Stap 3: Configureer de clientbibliotheek Face-API

De Face-API is een cloud API die u kunt aanroepen via HTTPS-aanvragen. Voor een handige methode van het gebruik van de Face-API in .NET platform-toepassingen is een clientbibliotheek ook beschikbaar op het web inkapselen aanvragen. In dit voorbeeld gebruiken we de clientbibliotheek ons werk vereenvoudigen. 

Volg de onderstaande instructies voor het configureren van de clientbibliotheek: 

1. Zoek het hoogste niveau **build.gradle** -bestand van uw project uit het deelvenster Project weergegeven in het voorbeeld. Houd er rekening mee dat er diverse andere zijn **build.gradle** bestanden in de projectstructuur van uw en u wilt openen van het hoogste niveau **build.gradle** bestand eerst.
2. Voeg **mavenCentral()** aan uw projecten-opslagplaatsen. U kunt ook jcenter() die de standaard-opslagplaats van Android Studio omdat jcenter() een hoofdverzameling van mavenCentral() is gebruiken.  

```
    allprojects {
        repositories {
            ...
            mavenCentral()
        }
    }
```

3. Open de **build.gradle** bestand in uw appproject.
4. Een afhankelijkheid voor onze clientbibliotheek opgeslagen in de centrale opslagplaats met Maven toevoegen:

```
    dependencies {  
        ...  
        implementation 'com.microsoft.projectoxford:face:1.4.3'  
    }
```

5. Open **MainActivity.java** in uw 'app'-project en insert importeert de volgende richtlijnen: 
    
    ```java
    import com.microsoft.projectoxford.face.*;  
    import com.microsoft.projectoxford.face.contract.*;  
    ```
    
   Voeg vervolgens de volgende code in de klasse:

    ```java
    private FaceServiceClient faceServiceClient = new FaceServiceRestClient("your API endpoint", "<Subscription Key>");
    ```

   De eerste parameter hierboven vervangen door de API-eindpunt dat is toegewezen aan de sleutel in stap 1. Bijvoorbeeld:
   
        https://eastus2.api.cognitive.microsoft.com/face/v1.0
   
   De tweede parameter vervangen door de sleutel van het abonnement dat u hebt verkregen in stap 1.
   
6. Open het bestand **AndroidManifest.xml** in uw appproject. Voeg het volgende element als onderliggend element van de **manifest** element:  

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />  
    ```

7. U bent nu klaar om aan te roepen de Face-API van uw toepassing. 

## <a name="step4"></a>Stap 4: Afbeeldingen voor het detecteren van vlakken uploaden

De eenvoudigste manier om te detecteren vlakken is door het aanroepen van de [geconfronteerd – detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API door het installatiekopiebestand rechtstreeks te uploaden. Wanneer u de clientbibliotheek gebruikt, kunt u dit doen met behulp van de asynchrone methode **DetectAsync** van de **FaceServiceClient** klasse. Elke geretourneerde face bevat een rechthoek voor het aangeven van de locatie, gecombineerd met een reeks optionele face-kenmerken. In dit voorbeeld moet er alleen voor het ophalen van de face-locatie. Hier moet invoegen van een methode in de **MainActivity** klasse voor face detection: 

```java

    // Detect faces by uploading face images
    // Frame faces after detection
    
    private void detectAndFrame(final Bitmap imageBitmap)
    {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
        ByteArrayInputStream inputStream = 
            new ByteArrayInputStream(outputStream.toByteArray());
        AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0], 
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null           // returnFaceAttributes: a string like "age, gender"
                /* If you want value of FaceAttributes, try adding 4th argument like below.
                            new FaceServiceClient.FaceAttributeType[] {
                    FaceServiceClient.FaceAttributeType.Age,
                    FaceServiceClient.FaceAttributeType.Gender }
                */              
                        );
                        if (result == null)
                        {
                            publishProgress("Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(
                                String.format("Detection Finished. %d face(s) detected",
                                        result.length));
                        return result;
                    } catch (Exception e) {
                        publishProgress("Detection failed");
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
```

## <a name="step5"></a>Stap 5: Markeren bespreekt in de afbeelding

In deze laatste stap we de bovenstaande stappen samen combineren en markeert de gedetecteerde vlakken met frames in de installatiekopie. Open eerst **MainActivity.java** en voeg een Help-methode om te tekenen rechthoeken: 

```java
    private static Bitmap drawFaceRectanglesOnBitmap(Bitmap originalBitmap, Face[] faces) {
        Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        int stokeWidth = 2;
        paint.setStrokeWidth(stokeWidth);
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

Nu de TODO-onderdelen in de **detectAndFrame** methode om te vlakken frame en status rapporteren.

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
        if (result == null) return;
        ImageView imageView = (ImageView)findViewById(R.id.imageView1);
        imageView.setImageBitmap(drawFaceRectanglesOnBitmap(imageBitmap, result));
        imageBitmap.recycle();
    }
```
 
Voeg een aanroep van de **detectAndFrame** methode van de **onActivityResult** methode, zoals hieronder wordt weergegeven. 

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
     
                // This is the new addition.
                // detectAndFrame(bitmap);
     
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

Deze toepassing en blader naar een installatiekopie van een gezicht uitvoeren. Wacht een paar seconden waarmee de cloud-API om te reageren. Daarna krijgt u een resultaat vergelijkbaar met de onderstaande afbeelding: 

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a> Samenvatting

In deze zelfstudie hebt geleerd het basisproces voor het gebruik van de Face-API en een toepassing om weer te geven face aanhalingstekens in de afbeeldingen hebt gemaakt. Raadpleeg voor meer informatie over de Face-API's de instructies en [API Reference](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

## <a name="related"></a> Verwante zelfstudies

- [Aan de slag met Face-API in CSharp-zelfstudie](FaceAPIinCSharpTutorial.md)
- [Aan de slag met Face-API in Python-zelfstudie](FaceAPIinPythonTutorial.md)
