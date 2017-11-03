---
title: Vloeiende Streaming van Windows Store-App-zelfstudie | Microsoft Docs
description: Informatie over het gebruik van Azure Media Services een C# Windows Store-toepassing maken met een XML-MediaElement besturingselement Smooth Stream afspelen van inhoud.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: juliako
ms.openlocfilehash: b4f8855fe6480bc58acfbbb53819f6eabe362bdb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Het bouwen van een Smooth Streaming van Windows Store-toepassing

De Smooth Streaming Client SDK voor Windows 8 kunnen ontwikkelaars om Windows Store-toepassingen die kunnen op aanvraag en live-Smooth Streaming inhoud afspelen te bouwen. Naast het basic afspelen van inhoud Smooth Streaming, biedt de SDK ook uitgebreide functies zoals Microsoft PlayReady protection, kwaliteit niveau beperking, Live DVR, audiostroom overschakelen, luisteren naar statusupdates (zoals kwaliteit verandert) en foutgebeurtenissen, enzovoort. Zie voor meer informatie de ondersteunde functies van de [release-opmerkingen](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Zie voor meer informatie [Player Framework voor Windows 8](http://playerframework.codeplex.com/). 

Deze zelfstudie bevat vier uitkomsten:

1. Een eenvoudige Smooth Streaming Store-toepassing maken
2. Toevoegen van een schuifbalk voor het beheren van de voortgang van de Media
3. Selecteer Streams Smooth Streaming
4. Selecteer nummers Smooth Streaming

## <a name="prerequisites"></a>Vereisten
> [!NOTE]
> Windows Store-projecten versie 8.1 en lager worden niet ondersteund in Visual Studio 2017.  Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie.

* Windows 8, 32-bits of 64-bits.
* Versies voor Visual Studio 2012 via 2015.
* [Microsoft Smooth Streaming Client SDK voor Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

De voltooide oplossing voor elke les kan worden gedownload van MSDN Developer-codevoorbeelden (Codegalerie): 

* [Les 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) : een eenvoudige Windows 8-Smooth Streaming MediaPlayer 
* [Les 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) : een eenvoudige Windows 8-Smooth Streaming MediaPlayer met een schuifregelaar beheren 
* [Les 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - een Windows 8-Smooth Streaming MediaPlayer met Stream-selectie  
* [4 les](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - een Windows 8-Smooth Streaming MediaPlayer bij selectie bijhouden.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Les 1: Een eenvoudige Smooth Streaming Store-toepassing maken

In deze les maakt u een Windows Store-toepassing met een MediaElement besturingselement Smooth Stream afspelen inhoud.  De actieve toepassing ziet eruit als:

![Voorbeeld van Smooth Streaming Windows Store-toepassing][PlayerApplication]

Zie voor meer informatie over het ontwikkelen van Windows Store-toepassing [ontwikkelen geweldige Apps voor Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Deze les bevat de volgende procedures:

1. Een Windows Store-project maken
2. Ontwerp van de gebruikersinterface (XAML)
3. Wijzigen van de onderliggende-codebestand
4. Compileren en de toepassing testen

**Een Windows Store-project maken**

1. Uitvoeren van Visual Studio; 2012 via 2015-versies worden ondersteund.
2. Klik in het menu **FILE** op **Nieuw** en klik vervolgens op **Project**.
3. In het dialoogvenster Nieuw Project typt of selecteert u de volgende waarden:

| Naam | Waarde |
| --- | --- |
| Sjabloongroep |Geïnstalleerd/sjablonen/Visual C# / van Windows Store |
| Template |Lege App (XAML) |
| Naam |SSPlayer |
| Locatie |C:\SSTutorials |
| Naam van de oplossing |SSPlayer |
| Map voor oplossing maken |(geselecteerd) |

1. Klik op **OK**.

**Een verwijzing naar de Smooth Streaming Client SDK toevoegen**

1. In Solution Explorer met de rechtermuisknop op **SSPlayer**, en klik vervolgens op **verwijzing toevoegen**.
2. Typ of selecteer de volgende waarden:

| Naam | Waarde |
| --- | --- |
| Referentie-groep |Windows-uitbreidingen |
| Naslaginformatie |Selecteer Microsoft Smooth Streaming Client SDK voor Windows 8 en Microsoft Visual C++ Runtime-pakket |

1. Klik op **OK**. 

Nadat de referenties zijn toegevoegd, moet u het doelplatform (x64 of x86), toe te voegen verwijzingen werkt niet voor platformconfiguratie Any CPU.  Klik in solution explorer ziet u gele waarschuwing is ingeschakeld voor deze referenties toegevoegd.

**Voor het ontwerpen van de gebruikersinterface player**

1. Klik in Solution Explorer, dubbelklik op **MainPage.xaml** om dit te openen in de ontwerpweergave.
2. Zoek de  **&lt;raster&gt;**  en  **&lt;/Grid&gt;**  labels van de XAML-bestand en plak de volgende code tussen de twee labels:

         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   
   Het besturingselement MediaElement wordt gebruikt voor het afspelen van media. De schuifregelaar sliderProgress met de naam wordt gebruikt in de volgende les waarmee de voortgang van de media.
3. Druk op **CTRL + S** het bestand wilt opslaan.

Het besturingselement MediaElement biedt geen ondersteuning voor Smooth Streaming inhoud out-of-box. De Smooth Streaming als ondersteuning wilt inschakelen, moet u de handler byte-stroom Smooth Streaming door de bestandsnaamextensie en MIME-typen registreren.  Als u wilt registreren, moet u de methode MediaExtensionManager.RegisterByteStremHandler van de naamruimte Windows.Media gebruiken.

In dit XAML-bestand zijn sommige gebeurtenis-handlers gekoppeld aan de besturingselementen.  U moet deze gebeurtenis-handlers definiëren.

**Wijzigen van de onderliggende-codebestand**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. Voeg het volgende toe aan de bovenkant van het bestand met de instructie:
   
        using Windows.Media;
3. Aan het begin van de **MainPage** klasse, voegt u het volgende gegevenslid:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Aan het einde van de **MainPage** -constructor aan, voeg de volgende twee regels:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Aan het einde van de **MainPage** klasse, plak de volgende code:
   
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click the Play button to play the media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek to position " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion

Hier wordt de gebeurtenis-handler sliderProgress_PointerPressed gedefinieerd.  Er zijn meer works doen om het werkt, worden behandeld in de volgende les van deze zelfstudie.
6. Druk op **CTRL + S** het bestand wilt opslaan.

Het voltooide die het onderliggende-codebestand moet uitzien:

![CodeView in Visual Studio of Smooth Streaming Windows Store-toepassing][CodeViewPic]

**Om te compileren en de toepassing testen**

1. Van de **bouwen** menu, klikt u op **Configuration Manager**.
2. Wijziging **actieve oplossing platform** overeenkomen met uw ontwikkelplatform.
3. Druk op **F6** voor het compileren van het project. 
4. Druk op **F5** om de toepassing uit te voeren.
5. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of voer een andere. 
6. Klik op **bron instellen**. Omdat **automatisch afspelen** is ingeschakeld standaard de media wordt afgespeeld.  U kunt bepalen de media met de **afspelen**, **onderbreken** en **stoppen** knoppen.  U kunt de Mediavolume met de verticale schuifbalk beheren.  Evenwel de horizontale schuifbalk voor het beheren van de voortgang van de media is volledig nog niet geïmplementeerd. 

U kunt lesson1 hebt voltooid.  In deze les gebruikt u een besturingselement MediaElement Smooth Streaming inhoud afspelen.  In de volgende les voegt u een schuifregelaar voor het beheren van de voortgang van de inhoud Smooth Streaming.

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Les 2: Een schuifbalk voor het beheren van de voortgang van de Media toevoegen

In les 1, kunt u een Windows Store-servicetoepassing gemaakt met een MediaElement XAML-besturingselement voor het afspelen Smooth Streaming media-inhoud.  Sommige functies basic media zoals starten, stoppen en onderbreken wordt geleverd.  In deze les wordt u een balk schuifregelaar toevoegen aan de toepassing.

In deze zelfstudie gebruiken we een timer voor het bijwerken van de positie van de schuifregelaar op basis van de huidige positie van het besturingselement MediaElement.  De schuifregelaar begin- en -tijd die ook moeten worden bijgewerkt in geval van een live-inhoud.  Dit kan beter worden verwerkt in de gebeurtenis adaptieve gegevensbron bijwerken.

Mediabronnen zijn objecten die mediagegevens genereren.  De bron-resolver een URL of byte-stroom accepteert en maakt de mediumbron voor de juiste voor die inhoud.  De bronoplossing is de standaardmethode voor de toepassingen voor het maken van media-bronnen. 

Deze les bevat de volgende procedures:

1. De handler Smooth Streaming registreren 
2. De adaptieve bron manager niveau gebeurtenis-handlers toevoegen
3. De adaptieve bron niveau gebeurtenis-handlers toevoegen
4. Gebeurtenis-handlers MediaElement toevoegen
5. Schuifregelaar gerelateerde code toevoegen
6. Compileren en de toepassing testen

**De byte-stroom Smooth Streaming handler registreren en de propertyset doorgeven**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. Voeg het volgende toe aan het begin van het bestand met de instructie:

        using Microsoft.Media.AdaptiveStreaming;
3. Toevoegen aan het begin van de klasse MainPage de gegevensleden van de volgende:

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. In de **MainPage** -constructor aan, voeg de volgende code na de **dit. Initialiseren van Components();**  lijn en de registratie van de regels die zijn geschreven in de vorige les code:

        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. In de **MainPage** constructor wijzigen van de twee methoden RegisterByteStreamHandler toevoegen de beschreven parameters:

         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
6. Druk op **CTRL + S** het bestand wilt opslaan.

**De adaptieve bron manager niveau gebeurtenis-handler toevoegen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. In de **MainPage** klasse, voegt u het volgende gegevenslid:
   
     persoonlijke AdaptiveSource adaptiveSource = null;
3. Aan het einde van de **MainPage** klasse, de volgende gebeurtenis-handler toevoegen:
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. Aan het einde van de **MainPage** -constructor aan, voeg de volgende regel om u te abonneren adaptieve bron gebeurtenis:
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. Druk op **CTRL + S** het bestand wilt opslaan.

**Niveau gebeurtenis-handlers adaptieve bron toevoegen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. In de **MainPage** klasse, voegt u het volgende gegevenslid:
   
     persoonlijke AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   persoonlijke Manifest manifestObject;
3. Aan het einde van de **MainPage** klasse, de volgende gebeurtenis-handlers toevoegen:

         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
4. Aan het einde van de **mediaElement AdaptiveSourceOpened** methode, voeg de volgende code om u te abonneren op de gebeurtenissen:
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. Druk op **CTRL + S** het bestand wilt opslaan.

Dezelfde gebeurtenissen zijn beschikbaar op adaptieve bron Manager niveau, die kan worden gebruikt voor het verwerken van de functionaliteit die gemeenschappelijk zijn voor alle media-elementen in de app. Elke AdaptiveSource omvat zijn eigen gebeurtenissen en worden alle gebeurtenissen voor AdaptiveSource trapsgewijs onder AdaptiveSourceManager.

**Media-Element gebeurtenis-handlers toevoegen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. Aan het einde van de **MainPage** klasse, de volgende gebeurtenis-handlers toevoegen:

         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
3. Aan het einde van de **MainPage** -constructor aan, voeg de volgende code toe subscript op de gebeurtenissen:

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. Druk op **CTRL + S** het bestand wilt opslaan.

**Toevoegen van de schuifregelaar gerelateerde code**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. Voeg het volgende toe aan het begin van het bestand met de instructie:
      
        using Windows.UI.Core;
3. In de **MainPage** klasse, voeg de gegevensleden van de volgende:
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. Aan het einde van de **MainPage** -constructor aan, voeg de volgende code:
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. Aan het einde van de **MainPage** klasse, voeg de volgende code:

         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
      
>[!NOTE]
>CoreDispatcher wordt gebruikt om wijzigingen aanbrengen in de gebruikersinterface-thread van niet-UI-Thread. In geval van een knelpunt op dispatcher-thread kunt developer dispatcher geleverd door de UI-element hij wil bijwerken gebruiken.  Bijvoorbeeld:
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. Aan het einde van de **mediaElement_AdaptiveSourceStatusUpdated** methode, voeg de volgende code:

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. Aan het einde van de **MediaOpened** methode, voeg de volgende code:

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. Druk op **CTRL + S** het bestand wilt opslaan.

**Om te compileren en de toepassing testen**

1. Druk op **F6** voor het compileren van het project. 
2. Druk op **F5** om de toepassing uit te voeren.
3. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of voer een andere. 
4. Klik op **bron instellen**. 
5. Test de schuifregelaar.

U kunt les 2 hebt voltooid.  In deze les kunt u een schuifregelaar toegevoegd aan de toepassing. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Les 3: Selecteer Streams Smooth Streaming
Smooth Streaming is in staat om inhoud te streamen met meerdere taal audio nummers die kunnen geselecteerd door de viewers worden.  In deze les schakelt u viewers streams selecteren. Deze les bevat de volgende procedures:

1. De XAML-bestand wijzigen
2. Wijzigen van het codebestand behand
3. Compileren en de toepassing testen

**Het XAML-bestand te wijzigen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **ontwerper**.
2. Zoek &lt;Grid.RowDefinitions&gt;, en wijzig de RowDefinitions zodat ze eruitzien:
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. In de &lt;raster&gt;&lt;/Grid&gt; tags, voeg de volgende code voor het definiëren van een besturingselement listbox zodat gebruikers kunnen de lijst met beschikbare stromen en streams selecteren:

         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
4. Druk op **CTRL + S** de wijzigingen wilt opslaan.

**Wijzigen van de onderliggende-codebestand**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. Voeg een nieuwe klasse toe binnen de naamruimte SSPlayer:
   
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
3. Voeg de volgende variabele definities aan het begin van de klasse MainPage:
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. Voeg de volgende regio in de klasse MainPage:
   
        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
5. De methode mediaElement_ManifestReady vinden, de volgende code aan het einde van de functie toevoegen:
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    Dus wanneer MediaElement manifest klaar is, wordt de code een lijst van de beschikbare stromen wordt en vult de UI-keuzelijst met de lijst.
6. Zoek de gebruikersinterface in de klasse MainPage knoppen klikt u op gebeurtenissen regio en voeg vervolgens de volgende functiedefinitie:
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Om te compileren en de toepassing testen**

1. Druk op **F6** voor het compileren van het project. 
2. Druk op **F5** om de toepassing uit te voeren.
3. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of voer een andere. 
4. Klik op **bron instellen**. 
5. De standaardtaal is audio_eng. Probeer schakelen tussen audio_eng en audio_es. Telkens wanneer, selecteert u een nieuwe stream, klikt u op de knop verzenden.

U kunt les 3 hebt voltooid.  In deze les voegt u de functionaliteit om stromen te selecteren.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Les 4: Selecteer nummers Smooth Streaming
Een presentatie Smooth Streaming kan meerdere videobestanden gecodeerd met verschillende kwaliteitsniveaus (bitsnelheden) en oplossingen bevatten. In deze les schakelt u gebruikers kunnen selecteren, houdt. Deze les bevat de volgende procedures:

1. De XAML-bestand wijzigen
2. Wijzigen van het codebestand behand
3. Compileren en de toepassing testen

**Het XAML-bestand te wijzigen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **ontwerper**.
2. Zoek de &lt;raster&gt; tag met de naam **gridStreamAndBitrateSelection**, de volgende code aan het einde van de tag toevoegen:
   
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
3. Druk op **CTRL + S** hij wijzigingen op te slaan

**Wijzigen van de onderliggende-codebestand**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **weergavecode**.
2. Voeg een nieuwe klasse toe binnen de naamruimte SSPlayer:
   
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
3. Voeg de volgende variabele definities aan het begin van de klasse MainPage:
   
        private List<Track> availableTracks;
4. Voeg de volgende regio in de klasse MainPage:
   
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>
   
        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
5. De methode mediaElement_ManifestReady vinden, de volgende code aan het einde van de functie toevoegen:
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. Zoek de gebruikersinterface in de klasse MainPage knoppen klikt u op gebeurtenissen regio en voeg vervolgens de volgende functiedefinitie:
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }

**Om te compileren en de toepassing testen**

1. Druk op **F6** voor het compileren van het project. 
2. Druk op **F5** om de toepassing uit te voeren.
3. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of voer een andere. 
4. Klik op **bron instellen**. 
5. Alle nummers van de videostream zijn standaard geselecteerd. Als u wilt experimenteren het bits tarief wordt gewijzigd, kunt u de laagste bitsnelheid beschikbaar selecteren en selecteer vervolgens de hoogste bitsnelheid beschikbaar. U moet op verzenden klikt na elke wijziging.  U kunt de videokwaliteit wijzigingen kan zien.

U kunt les 4 hebt voltooid.  In deze les voegt u de functionaliteit om te kiezen houdt.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Andere bronnen:
* [Het bouwen van een toepassing Smooth Streaming Windows 8 JavaScript met geavanceerde functies](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Smooth Streaming technisch overzicht](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

