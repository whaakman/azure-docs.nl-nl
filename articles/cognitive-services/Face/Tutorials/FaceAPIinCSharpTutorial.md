---
title: 'Zelfstudie: Gezichten in een afbeelding detecteren en omlijsten - Face-API, C#'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie maakt u een Windows-app die gebruikmaakt van de Face-API om gezichten in een afbeelding te herkennen en omlijsten.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: pafarley
ms.openlocfilehash: ba850c1d4f9d8018f333662eead56f8e91b3fbd7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340950"
---
# <a name="tutorial-create-a-wpf-app-to-detect-and-frame-faces-in-an-image"></a>Zelfstudie: een WPF-app maken om gezichten in een afbeelding te herkennen en te omlijsten

In deze zelfstudie maakt u een Windows Presentation Framework (WPF)-toepassing die gebruikmaakt van de Face-service via de .NET-clientbibliotheek. De app detecteert gezichten in een afbeelding, tekent een kader rond elk gezicht en geeft een beschrijving van het gezicht weer op de statusbalk. De volledige voorbeeldcode is via GitHub beschikbaar op [Detect and frame faces in an image on Windows](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) (Gezichten in een afbeelding detecteren en omlijsten in Windows).

![Schermafbeelding met gedetecteerde gezichten omlijst door rechthoeken](../Images/getting-started-cs-detected.png)

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> - Een WPF-toepassing maken
> - De clientbibliotheek van de Face-service installeren
> - De clientbibliotheek gebruiken om gezichten in een afbeelding te detecteren
> - Een kader rond elk gedetecteerd gezicht tekenen
> - Een beschrijving van het gezicht op de statusbalk weergeven

## <a name="prerequisites"></a>Vereisten

- U hebt een abonnementssleutel nodig om het voorbeeld uit te voeren. U kunt abonnementssleutels voor een gratis proefversie downloaden op [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/). Voor Visual Studio 2017 is de workload voor de ontwikkeling van de .NET-Desktop-toepassing is vereist. In deze zelfstudie wordt Visual Studio 2017 Community Edition gebruikt.
- Het NuGet-pakket van de [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)-clientbibliotheek. U hoeft het pakket niet te downloaden. Hieronder vindt u de installatie-instructies.

## <a name="create-the-visual-studio-solution"></a>De Visual Studio-oplossing maken

Volg deze stappen voor het maken van een Windows WPF-toepassingsproject.

1. Open Visual Studio en klik in het menu **File** op **New** en vervolgens op **Project**.
   - Vouw in Visual Studio 2017 **Installed** uit en vervolgens **Other Languages**. Selecteer **Visual C#** en vervolgens **WPF App (.NET Framework)**.
   - Vouw in Visual Studio 2015 **Installed** uit en vervolgens **Templates**. Selecteer **Visual C#** en vervolgens **WPF Application**.
1. Noem de toepassing **FaceTutorial** en klik vervolgens op **OK**.

## <a name="install-the-face-service-client-library"></a>De clientbibliotheek van de Face-service installeren

Volg deze instructies voor het installeren van de clientbibliotheek.

1. Selecteer in het menu **Tools** de optie **NuGet Package Manager** en vervolgens **Package Manager Console**.
1. Plak het volgende in **Package Manager Console** en druk op **Enter**.

    `Install-Package Microsoft.Azure.CognitiveServices.Vision.Face -Version 2.2.0-preview`

## <a name="add-the-initial-code"></a>De initiële code toevoegen

### <a name="mainwindowxaml"></a>MainWindow.xaml

Open *MainWindow.xaml* (tip: wissel van deelvenster met behulp van het **pictogram pijl-omhoog/omlaag**) en vervang de inhoud door de volgende code. Deze xaml-code wordt gebruikt om het venster voor de gebruikersinterface te maken. Let op de gebeurtenis-handlers `FacePhoto_MouseMove` en `BrowseButton_Click`.

```xml
<Window x:Class="FaceTutorial.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="700" Width="960">
    <Grid x:Name="BackPanel">
        <Image x:Name="FacePhoto" Stretch="Uniform" Margin="0,0,0,50" MouseMove="FacePhoto_MouseMove" />
        <DockPanel DockPanel.Dock="Bottom">
            <Button x:Name="BrowseButton" Width="72" Height="20" VerticalAlignment="Bottom" HorizontalAlignment="Left"
                    Content="Browse..."
                    Click="BrowseButton_Click" />
            <StatusBar VerticalAlignment="Bottom">
                <StatusBarItem>
                    <TextBlock Name="faceDescriptionStatusBar" />
                </StatusBarItem>
            </StatusBar>
        </DockPanel>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Vouw *MainWindow.xaml* uit, open *MainWindow.xaml.cs* en vervang de inhoud door de volgende code. Negeer de rode kronkellijntjes; deze verdwijnen na de eerste build.

Met de eerste twee regels worden de naamruimten van de clientbibliotheek geïmporteerd. Vervolgens wordt `FaceClient` gemaakt en de abonnementssleutel doorgegeven, terwijl de Azure-regio wordt ingesteld in de `MainWindow`-constructor. De twee methoden `BrowseButton_Click` en `FacePhoto_MouseMove` corresponderen met de gebeurtenis-handlers die in *MainWindow.xaml* zijn gedeclareerd.

`BrowseButton_Click` maakt een `OpenFileDialog`, waardoor de gebruiker een JPG-afbeelding kan selecteren. De afbeelding wordt gelezen en in het hoofdvenster weergegeven. De resterende code voor `BrowseButton_Click` en de code voor `FacePhoto_MouseMove` wordt in volgende stappen ingevoegd.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;

namespace FaceTutorial
{
    public partial class MainWindow : Window
    {
        // Replace <SubscriptionKey> with your valid subscription key.
        // For example, subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // Replace or verify the region.
        //
        // You must use the same region as you used to obtain your subscription
        // keys. For example, if you obtained your subscription keys from the
        // westus region, replace "westcentralus" with "westus".
        //
        // NOTE: Free trial subscription keys are generated in the westcentralus
        // region, so if you are using a free trial subscription key, you should
        // not need to change this region.
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials(subscriptionKey),
            new System.Net.Http.DelegatingHandler[] { });

        IList<DetectedFace> faceList;   // The list of detected faces.
        String[] faceDescriptions;      // The list of descriptions for the detected faces.
        double resizeFactor;            // The resize factor for the displayed image.

        public MainWindow()
        {
            InitializeComponent();

            if (Uri.IsWellFormedUriString(faceEndpoint, UriKind.Absolute))
            {
                faceClient.Endpoint = faceEndpoint;
            }
            else
            {
                MessageBox.Show(faceEndpoint,
                    "Invalid URI", MessageBoxButton.OK, MessageBoxImage.Error);
                Environment.Exit(0);
            }
        }

        // Displays the image and calls UploadAndDetectFaces.
        private async void BrowseButton_Click(object sender, RoutedEventArgs e)
        {
            // Get the image file to scan from the user.
            var openDlg = new Microsoft.Win32.OpenFileDialog();

            openDlg.Filter = "JPEG Image(*.jpg)|*.jpg";
            bool? result = openDlg.ShowDialog(this);

            // Return if canceled.
            if (!(bool)result)
            {
                return;
            }

            // Display the image file.
            string filePath = openDlg.FileName;

            Uri fileUri = new Uri(filePath);
            BitmapImage bitmapSource = new BitmapImage();

            bitmapSource.BeginInit();
            bitmapSource.CacheOption = BitmapCacheOption.None;
            bitmapSource.UriSource = fileUri;
            bitmapSource.EndInit();

            FacePhoto.Source = bitmapSource;
        }

        // Displays the face description when the mouse is over a face rectangle.
        private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
        {
        }
    }
}
```

### <a name="insert-your-subscription-key-and-verify-or-change-the-region"></a>Uw abonnementssleutel invoegen en de regio verifiëren of wijzigen

- Zoek de volgende regel in *MainWindow.xaml.cs* en vervang `<Subscription Key>` door uw Face-API-abonnementssleutel:

    ```csharp
    private const string subscriptionKey = "<SubscriptionKey>";
    ```

- Zoek de volgende regel in *MainWindow.xaml.cs* en vervang of verifieer de Azure-regio die aan uw abonnementssleutel is gekoppeld:

    ```csharp
    private const string Endpoint =
        "https://westcentralus.api.cognitive.microsoft.com";
    ```

    Zorg ervoor dat de locatie dezelfde is als die waar u de abonnementssleutels hebt verkregen. Als u uw abonnementssleutels hebt verkregen van bijvoorbeeld de regio **westus**, vervangt u `Westcentralus` door `Westus`.

    Als u de abonnementssleutels hebt ontvangen door de proefversie te gebruiken, is **westcentralus** de regio voor uw sleutels en is er dus geen wijziging vereist.

### <a name="test-the-app"></a>De app testen

Druk op **Start** in het menu om uw app te testen. Als het venster wordt geopend, klikt u in de linkerbenedenhoek op **Browse**. Het dialoogvenster **File open** verschijn. Hier kunt u naar een foto zoeken en deze selecteren, waarna de foto in het venster wordt weergegeven.

![Schermafbeelding met ongewijzigde afbeelding van gezichten](../Images/getting-started-cs-ui.png)

## <a name="upload-an-image-to-detect-faces"></a>Een afbeelding uploaden om gezichten te detecteren

De eenvoudigste manier om gezichten te detecteren, is door de methode `FaceClient.Face.DetectWithStreamAsync` aan te roepen. Hiermee wordt de API-methode [Detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) ingepakt om de lokale afbeelding te uploaden.

Voeg de volgende methode in de `MainWindow`-klasse in, onder de methode `FacePhoto_MouseMove`.

Er wordt een lijst met gezichtskenmerken gemaakt en het ingediende afbeeldingsbestand wordt in een `Stream` gelezen. Beide worden doorgegeven aan de `DetectWithStreamAsync`-aanroep.

```csharp
// Uploads the image file and calls DetectWithStreamAsync.
private async Task<IList<DetectedFace>> UploadAndDetectFaces(string imageFilePath)
{
    // The list of Face attributes to return.
    IList<FaceAttributeType> faceAttributes =
        new FaceAttributeType[]
        {
            FaceAttributeType.Gender, FaceAttributeType.Age,
            FaceAttributeType.Smile, FaceAttributeType.Emotion,
            FaceAttributeType.Glasses, FaceAttributeType.Hair
        };

    // Call the Face API.
    try
    {
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            // The second argument specifies to return the faceId, while
            // the third argument specifies not to return face landmarks.
            IList<DetectedFace> faceList =
                await faceClient.Face.DetectWithStreamAsync(
                    imageFileStream, true, false, faceAttributes);
            return faceList;
        }
    }
    // Catch and display Face API errors.
    catch (APIErrorException f)
    {
        MessageBox.Show(f.Message);
        return new List<DetectedFace>();
    }
    // Catch and display all other errors.
    catch (Exception e)
    {
        MessageBox.Show(e.Message, "Error");
        return new List<DetectedFace>();
    }
}
```

## <a name="draw-rectangles-around-each-face"></a>Rechthoeken rond de gezichten tekenen

Voeg de code toe om een rechthoek rond elk gedetecteerd gezicht in de afbeelding te tekenen.

Voeg in *MainWindow.xaml.cs* de modifier `async` toe aan de methode `BrowseButton_Click`.

```csharp
private async void BrowseButton_Click(object sender, RoutedEventArgs e)
```

Voeg de volgende code toe aan het einde van de methode `BrowseButton_Click`, na de regel `FacePhoto.Source = bitmapSource`.

De lijst met gedetecteerde gezichten wordt ingevuld door de aanroep naar `UploadAndDetectFaces`. Vervolgens wordt een rechthoek rond elk gezicht getekend en de aangepaste afbeelding wordt weergegeven in het hoofdvenster.

```csharp
// Detect any faces in the image.
Title = "Detecting...";
faceList = await UploadAndDetectFaces(filePath);
Title = String.Format(
    "Detection Finished. {0} face(s) detected", faceList.Count);

if (faceList.Count > 0)
{
    // Prepare to draw rectangles around the faces.
    DrawingVisual visual = new DrawingVisual();
    DrawingContext drawingContext = visual.RenderOpen();
    drawingContext.DrawImage(bitmapSource,
        new Rect(0, 0, bitmapSource.Width, bitmapSource.Height));
    double dpi = bitmapSource.DpiX;
    resizeFactor = (dpi > 0) ? 96 / dpi : 1;
    faceDescriptions = new String[faceList.Count];

    for (int i = 0; i < faceList.Count; ++i)
    {
        DetectedFace face = faceList[i];

        // Draw a rectangle on the face.
        drawingContext.DrawRectangle(
            Brushes.Transparent,
            new Pen(Brushes.Red, 2),
            new Rect(
                face.FaceRectangle.Left * resizeFactor,
                face.FaceRectangle.Top * resizeFactor,
                face.FaceRectangle.Width * resizeFactor,
                face.FaceRectangle.Height * resizeFactor
                )
        );

        // Store the face description.
        faceDescriptions[i] = FaceDescription(face);
    }

    drawingContext.Close();

    // Display the image with the rectangle around the face.
    RenderTargetBitmap faceWithRectBitmap = new RenderTargetBitmap(
        (int)(bitmapSource.PixelWidth * resizeFactor),
        (int)(bitmapSource.PixelHeight * resizeFactor),
        96,
        96,
        PixelFormats.Pbgra32);

    faceWithRectBitmap.Render(visual);
    FacePhoto.Source = faceWithRectBitmap;

    // Set the status bar text.
    faceDescriptionStatusBar.Text =
        "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="describe-the-faces-in-the-image"></a>Gezichten in de afbeelding beschrijven

Voeg de volgende methode aan de klasse `MainWindow` toe, onder de methode `UploadAndDetectFaces`.

De methode converteert de gezichtskenmerken naar een tekenreeks die het gezicht beschrijft. De tekenreeks wordt weergegeven als de muisaanwijzer boven het vierkant rond het gezicht wordt geplaatst.

```csharp
// Creates a string out of the attributes describing the face.
private string FaceDescription(DetectedFace face)
{
    StringBuilder sb = new StringBuilder();

    sb.Append("Face: ");

    // Add the gender, age, and smile.
    sb.Append(face.FaceAttributes.Gender);
    sb.Append(", ");
    sb.Append(face.FaceAttributes.Age);
    sb.Append(", ");
    sb.Append(String.Format("smile {0:F1}%, ", face.FaceAttributes.Smile * 100));

    // Add the emotions. Display all emotions over 10%.
    sb.Append("Emotion: ");
    Emotion emotionScores = face.FaceAttributes.Emotion;
    if (emotionScores.Anger >= 0.1f)
        sb.Append(String.Format("anger {0:F1}%, ", emotionScores.Anger * 100));
    if (emotionScores.Contempt >= 0.1f)
        sb.Append(String.Format("contempt {0:F1}%, ", emotionScores.Contempt * 100));
    if (emotionScores.Disgust >= 0.1f)
        sb.Append(String.Format("disgust {0:F1}%, ", emotionScores.Disgust * 100));
    if (emotionScores.Fear >= 0.1f)
        sb.Append(String.Format("fear {0:F1}%, ", emotionScores.Fear * 100));
    if (emotionScores.Happiness >= 0.1f)
        sb.Append(String.Format("happiness {0:F1}%, ", emotionScores.Happiness * 100));
    if (emotionScores.Neutral >= 0.1f)
        sb.Append(String.Format("neutral {0:F1}%, ", emotionScores.Neutral * 100));
    if (emotionScores.Sadness >= 0.1f)
        sb.Append(String.Format("sadness {0:F1}%, ", emotionScores.Sadness * 100));
    if (emotionScores.Surprise >= 0.1f)
        sb.Append(String.Format("surprise {0:F1}%, ", emotionScores.Surprise * 100));

    // Add glasses.
    sb.Append(face.FaceAttributes.Glasses);
    sb.Append(", ");

    // Add hair.
    sb.Append("Hair: ");

    // Display baldness confidence if over 1%.
    if (face.FaceAttributes.Hair.Bald >= 0.01f)
        sb.Append(String.Format("bald {0:F1}% ", face.FaceAttributes.Hair.Bald * 100));

    // Display all hair color attributes over 10%.
    IList<HairColor> hairColors = face.FaceAttributes.Hair.HairColor;
    foreach (HairColor hairColor in hairColors)
    {
        if (hairColor.Confidence >= 0.1f)
        {
            sb.Append(hairColor.Color.ToString());
            sb.Append(String.Format(" {0:F1}% ", hairColor.Confidence * 100));
        }
    }

    // Return the built string.
    return sb.ToString();
}
```

## <a name="display-the-face-description"></a>De beschrijving van het gezicht weergeven

Vervang de methode `FacePhoto_MouseMove` door de volgende code.

Deze gebeurtenis-handler geeft de tekenreeks met de beschrijving van het gezicht weer als de muisaanwijzer boven het vierkant rond het gezicht wordt geplaatst.

```csharp
private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
{
    // If the REST call has not completed, return.
    if (faceList == null)
        return;

    // Find the mouse position relative to the image.
    Point mouseXY = e.GetPosition(FacePhoto);

    ImageSource imageSource = FacePhoto.Source;
    BitmapSource bitmapSource = (BitmapSource)imageSource;

    // Scale adjustment between the actual size and displayed size.
    var scale = FacePhoto.ActualWidth / (bitmapSource.PixelWidth / resizeFactor);

    // Check if this mouse position is over a face rectangle.
    bool mouseOverFace = false;

    for (int i = 0; i < faceList.Count; ++i)
    {
        FaceRectangle fr = faceList[i].FaceRectangle;
        double left = fr.Left * scale;
        double top = fr.Top * scale;
        double width = fr.Width * scale;
        double height = fr.Height * scale;

        // Display the face description if the mouse is over this face rectangle.
        if (mouseXY.X >= left && mouseXY.X <= left + width &&
            mouseXY.Y >= top  && mouseXY.Y <= top + height)
        {
            faceDescriptionStatusBar.Text = faceDescriptions[i];
            mouseOverFace = true;
            break;
        }
    }

    // String to display when the mouse is not over a face rectangle.
    if (!mouseOverFace)
        faceDescriptionStatusBar.Text =
            "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="run-the-app"></a>De app uitvoeren

Voer de toepassing uit en blader naar een afbeelding met een gezicht. Wacht enkele seconden, zodat de Face-service kan reageren. Daarna ziet u een rode rechthoek rond de gezichten in de afbeelding. Door de muisaanwijzer boven een gezichtsrechthoek te bewegen, wordt de beschrijving van dat gezicht op de statusbalk weergegeven.

![Schermafbeelding met gedetecteerde gezichten omlijst door rechthoeken](../Images/getting-started-cs-detected.png)

## <a name="summary"></a>Samenvatting

In deze zelfstudie hebt u het basisproces geleerd voor het gebruik van de clientbibliotheek van de Face-service en een toepassing gemaakt om gezichten in een afbeelding weer te geven en te omlijsten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het detecteren en gebruiken van gezichtskenmerken.

> [!div class="nextstepaction"]
> [Gezichten in afbeeldingen detecteren](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
