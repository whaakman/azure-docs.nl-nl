---
title: 'Quickstart: Aangepaste spraak op de eerste virtuele assistent (Preview), C# (UWP) - spraakservices'
titleSuffix: Azure Cognitive Services
description: In dit artikel maakt u een C# Universal Windows Platform (UWP)-toepassing met behulp van de Cognitive Services spraak Software Development Kit (SDK). Verbindt u uw clienttoepassing naar een eerder gemaakte Bot Framework-bot geconfigureerd voor het gebruik van het kanaal directe regel spraak. De toepassing is gemaakt met het Speech-SDK NuGet-pakket en Microsoft Visual Studio 2017.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: 4044f8d48efae4e8423f780c85e0f3ccfde12461
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467062"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-uwp"></a>Quickstart: Maken van een stem op de eerste virtuele assistent met de SDK-spraak, UWP

Snelstartgidsen zijn ook beschikbaar voor [spraak-naar-tekst](quickstart-csharp-uwp.md), [tekst naar spraak](quickstart-text-to-speech-csharp-uwp.md) en [spraakomzetting](quickstart-translate-speech-uwp.md).

In dit artikel, zult u ontwikkelt een C# Universal Windows Platform (UWP)-toepassing met behulp van de [spraak SDK](speech-sdk.md). Het programma maakt verbinding met een bot eerder hebt gemaakt en geconfigureerd om in te schakelen van een virtuele assistent stem op de eerste ervaring van de clienttoepassing. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> Het Universal Windows Platform stelt u in staat om apps te ontwikkelen die kunnen worden uitgevoerd op elk apparaat dat ondersteuning biedt voor Windows 10, met inbegrip van pc's, Xbox, Surface Hub en andere apparaten.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* De sleutel van een Azure-abonnement voor Speech Services. [Vraag een gratis](get-started.md) of maken door op de [Azure-portal](https://portal.azure.com).
* Een eerder gemaakte bot geconfigureerd met de [channel voor directe regel spraak](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)

    > [!NOTE]
    > Directe regel spraak (Preview) is momenteel beschikbaar in een subset van Services voor spraak-regio's. Raadpleeg [de lijst met ondersteunde regio's voor virtuele voice-first-assistenten](regions.md#voice-first-virtual-assistants) en zorg ervoor dat uw resources worden geïmplementeerd in een van deze regio's.

## <a name="optional-get-started-fast"></a>Optioneel: Snel aan de slag

In deze Quick Start wordt beschreven, stap voor stap het maken van een eenvoudige clienttoepassing die verbinding maken met uw bot spraak ingeschakeld. Als u liever meteen in de volledige, klaar om te compileren broncode gebruikt in deze snelstartgids is beschikbaar in de [spraak SDK voorbeelden](https://aka.ms/csspeech/samples) onder de `quickstart` map.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. De gebruikersinterface van de toepassing wordt gedefinieerd met behulp van XAML. Open `MainPage.xaml` in Solution Explorer. Vervang in de weergave van de ontwerpfunctie XAML, de volledige inhoud met de hieronder.

    ```xml
    <Page
        x:Class="helloworld.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:helloworld"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid>
            <StackPanel Orientation="Vertical" HorizontalAlignment="Center"  Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
                <Button x:Name="EnableMicrophoneButton" Content="Enable Microphone"  Margin="0,0,10,0" Click="EnableMicrophone_ButtonClicked" Height="35"/>
                <Button x:Name="ListenButton" Content="Talk to your bot" Margin="0,10,10,0" Click="ListenButton_ButtonClicked" Height="35"/>
                <StackPanel x:Name="StatusPanel" Orientation="Vertical" RelativePanel.AlignBottomWithPanel="True" RelativePanel.AlignRightWithPanel="True" RelativePanel.AlignLeftWithPanel="True">
                    <TextBlock x:Name="StatusLabel" Margin="0,10,10,0" TextWrapping="Wrap" Text="Status:" FontSize="20"/>
                    <Border x:Name="StatusBorder" Margin="0,0,0,0">
                        <ScrollViewer VerticalScrollMode="Auto"  VerticalScrollBarVisibility="Auto" MaxHeight="200">
                            <!-- Use LiveSetting to enable screen readers to announce the status update. -->
                            <TextBlock x:Name="StatusBlock" FontWeight="Bold" AutomationProperties.LiveSetting="Assertive"
                    MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}" Margin="10,10,10,20" TextWrapping="Wrap"  />
                        </ScrollViewer>
                    </Border>
                </StackPanel>
            </StackPanel>
            <MediaElement x:Name="mediaElement"/>
        </Grid>
    </Page>
    ```

1. Open het code-behind-bronbestand `MainPage.xaml.cs`. U vindt deze gegroepeerd onder `MainPage.xaml`. Vervang de inhoud door de onderstaande code. Dit is wat in dit voorbeeld bevat informatie over: 

    * Met behulp van instructies voor de spraak- en Speech.Dialog naamruimten
    * Een eenvoudige implementatie om te controleren of de toegang tot de microfoon, kabels verbonden met een knop-handler
    * Basic UI-hulpprogramma's om weer te geven van berichten en fouten in de toepassing
    * Een punt landingspagina voor de initialisatie van codepad die later worden ingevuld
    * Een hulpprogramma om af te spelen terug Text to Speech (zonder ondersteuning voor streaming)
    * Een lege knop handler beginnen met luisteren die later worden ingevuld

    ```csharp
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.CognitiveServices.Speech.Dialog;
    using System;
    using System.Diagnostics;
    using System.IO;
    using System.Text;
    using Windows.Foundation;
    using Windows.Storage.Streams;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Media;

    namespace helloworld
    {
        public sealed partial class MainPage : Page
        {
            private DialogServiceConnector connector;

            private enum NotifyType
            {
                StatusMessage,
                ErrorMessage
            };

            public MainPage()
            {
                this.InitializeComponent();
            }

            private async void EnableMicrophone_ButtonClicked(object sender, RoutedEventArgs e)
            {
                bool isMicAvailable = true;
                try
                {
                    var mediaCapture = new Windows.Media.Capture.MediaCapture();
                    var settings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    settings.StreamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.Audio;
                    await mediaCapture.InitializeAsync(settings);
                }
                catch (Exception)
                {
                    isMicAvailable = false;
                }
                if (!isMicAvailable)
                {
                    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
                }
                else
                {
                    NotifyUser("Microphone was enabled", NotifyType.StatusMessage);
                }
            }

            private void NotifyUser(string strMessage, NotifyType type = NotifyType.StatusMessage)
            {
                // If called from the UI thread, then update immediately.
                // Otherwise, schedule a task on the UI thread to perform the update.
                if (Dispatcher.HasThreadAccess)
                {
                    UpdateStatus(strMessage, type);
                }
                else
                {
                    var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
                }
            }

            private void UpdateStatus(string strMessage, NotifyType type)
            {
                switch (type)
                {
                    case NotifyType.StatusMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Green);
                        break;
                    case NotifyType.ErrorMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Red);
                        break;
                }
                StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text) ? strMessage : "\n" + strMessage;

                if (!string.IsNullOrEmpty(StatusBlock.Text))
                {
                    StatusBorder.Visibility = Visibility.Visible;
                    StatusPanel.Visibility = Visibility.Visible;
                }
                else
                {
                    StatusBorder.Visibility = Visibility.Collapsed;
                    StatusPanel.Visibility = Visibility.Collapsed;
                }
                // Raise an event if necessary to enable a screen reader to announce the status update.
                var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
                if (peer != null)
                {
                    peer.RaiseAutomationEvent(Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
                }
            }

            // Waits for accumulates all audio associated with a given PullAudioOutputStream and then plays it to the
            // MediaElement. Long spoken audio will create extra latency and a streaming playback solution (that plays
            // audio while it continues to be received) should be used -- see the samples for examples of this.
            private void SynchronouslyPlayActivityAudio(PullAudioOutputStream activityAudio)
            {
                var playbackStreamWithHeader = new MemoryStream();
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("RIFF"), 0, 4); // ChunkID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // ChunkSize: max
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("WAVE"), 0, 4); // Format
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("fmt "), 0, 4); // Subchunk1ID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 4); // Subchunk1Size: PCM
                playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // AudioFormat: PCM
                playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // NumChannels: mono
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16000), 0, 4); // SampleRate: 16kHz
                playbackStreamWithHeader.Write(BitConverter.GetBytes(32000), 0, 4); // ByteRate
                playbackStreamWithHeader.Write(BitConverter.GetBytes(2), 0, 2); // BlockAlign
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 2); // BitsPerSample: 16-bit
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("data"), 0, 4); // Subchunk2ID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // Subchunk2Size

                byte[] pullBuffer = new byte[2056];

                uint lastRead = 0;
                do
                {
                    lastRead = activityAudio.Read(pullBuffer);
                    playbackStreamWithHeader.Write(pullBuffer, 0, (int)lastRead);
                }
                while (lastRead == pullBuffer.Length);

                var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
                {
                    mediaElement.SetSource(playbackStreamWithHeader.AsRandomAccessStream(), "audio/wav");
                    mediaElement.Play();
                });
            }

            private void InitializeDialogServiceConnector()
            {
                // New code will go here
            }

            private async void ListenButton_ButtonClicked(object sender, RoutedEventArgs e)
            {
                // New code will go here
            }
        }
    }
    ```

1. Vervolgens maakt u de `DialogServiceConnector` met gegevens van uw abonnement. Voeg het volgende toe aan de hoofdtekst van de methode van `InitializeDialogServiceConnector`, de tekenreeksen vervangen `YourChannelSecret`, `YourSpeechSubscriptionKey`, en `YourServiceRegion` door uw eigen waarden voor uw bot, spraak-abonnement en [regio](regions.md).

    > [!NOTE]
    > Directe regel spraak (Preview) is momenteel beschikbaar in een subset van Services voor spraak-regio's. Raadpleeg [de lijst met ondersteunde regio's voor virtuele voice-first-assistenten](regions.md#voice-first-virtual-assistants) en zorg ervoor dat uw resources worden geïmplementeerd in een van deze regio's.

    > [!NOTE]
    > Zie voor informatie over het configureren van uw bot en bij het ophalen van een geheim kanaal de Bot Framework-documentatie voor [het kanaal directe regel spraak](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

    ```csharp
    // create a DialogServiceConfig by providing a bot secret key and Cognitive Services subscription key
    // the RecoLanguage property is optional (default en-US); note that only en-US is supported in Preview
    const string channelSecret = "YourChannelSecret"; // Your channel secret
    const string speechSubscriptionKey = "YourSpeechSubscriptionKey"; // Your subscription key
    const string region = "YourServiceRegion"; // Your subscription service region. Note: only a subset of regions are currently supported

    var botConfig = DialogServiceConfig.FromBotSecret(channelSecret, speechSubscriptionKey, region);
    botConfig.SetProperty(PropertyId.SpeechServiceConnection_RecoLanguage, "en-US");
    connector = new DialogServiceConnector(botConfig);
    ```

1. `DialogServiceConnector` afhankelijk van diverse gebeurtenissen om te communiceren de bot-activiteiten, herkenningsresultaten spraak en andere informatie. Toevoegen van handlers voor deze gebeurtenissen, het volgende toe te voegen aan het einde van de hoofdtekst van de methode van `InitializeDialogServiceConnector`.

    ```csharp
    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
    {
        NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

        if (activityReceivedEventArgs.HasAudio)
        {
            SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
        }
    };
    // Canceled will be signaled when a turn is aborted or experiences an error condition
    connector.Canceled += (sender, canceledEventArgs) =>
    {
        NotifyUser($"Canceled, reason={canceledEventArgs.Reason}");
        if (canceledEventArgs.Reason == CancellationReason.Error)
        {
            NotifyUser($"Error: code={canceledEventArgs.ErrorCode}, details={canceledEventArgs.ErrorDetails}");
        }
    };
    // Recognizing (not 'Recognized') will provide the intermediate recognized text while an audio stream is being processed
    connector.Recognizing += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Recognizing! in-progress text={recognitionEventArgs.Result.Text}");
    };
    // Recognized (not 'Recognizing') will provide the final recognized text once audio capture is completed
    connector.Recognized += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Final speech-to-text result: '{recognitionEventArgs.Result.Text}'");
    };
    // SessionStarted will notify when audio begins flowing to the service for a turn
    connector.SessionStarted += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Now Listening! Session started, id={sessionEventArgs.SessionId}");
    };
    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    connector.SessionStopped += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Listening complete. Session ended, id={sessionEventArgs.SessionId}");
    };
    ```

1. Met de configuratie tot stand gebracht en de gebeurtenis-handlers die is geregistreerd, de `DialogServiceConnector` moet nu net om te luisteren. Voeg het volgende toe aan de hoofdtekst van de `ListenButton_ButtonClicked` methode in de `MainPage` klasse.

    ```csharp
    private async void ListenButton_ButtonClicked(object sender, RoutedEventArgs e)
    {
        if (connector == null)
        {
            InitializeDialogServiceConnector();
            // Optional step to speed up first interaction: if not called, connection happens automatically on first use
            var connectTask = connector.ConnectAsync();
        }

        try
        {
            // Start sending audio to your speech-enabled bot
            var listenTask = connector.ListenOnceAsync();

            // You can also send activities to your bot as JSON strings -- Microsoft.Bot.Schema can simplify this
            string speakActivity = @"{""type"":""message"",""text"":""Greeting Message"", ""speak"":""Hello there!""}";
            await connector.SendActivityAsync(speakActivity);

        }
        catch (Exception ex)
        {
            NotifyUser($"Exception: {ex.ToString()}", NotifyType.ErrorMessage);
        }
    }
    ```

1. Sla alle wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. In het menu-balk van Visual Studio, selecteer **bouwen** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-uwp-08-build.png "Geslaagde build")

1. Start de toepassing. In het menu-balk van Visual Studio, selecteer **Debug** > **Start Debugging**, of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-uwp-09-start-debugging.png "Start foutopsporing van de app")

1. Er verschijnt een venster. Selecteer in uw toepassing, **microfoon inschakelen**, en te bevestigen van de machtiging-aanvraag die wordt weergegeven.

    ![Schermafbeelding van verzoek om toegang](media/sdk/qs-csharp-uwp-10-access-prompt.png "Start foutopsporing van de app")

1. Selecteer **Neem contact op met uw bot**, en spreek een Engelse woordgroep of zin in de microfoon van uw apparaat. Uw stem is verzonden naar het kanaal directe regel spraak en getranscribeerde tekst, die in het venster wordt weergegeven.

    ![Schermopname van geslaagde bot inschakelen](media/voice-first-virtual-assistants/quickstart-cs-uwp-bot-successful-turn.png "een geslaagde bot inschakelen")

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maken en implementeren van een basic-bot](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Zie ook

- [Over stem op de eerste virtuele assistent](voice-first-virtual-assistants.md)
- [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
- [Aangepaste wake woorden](speech-devices-sdk-create-kws.md)
- [Directe regel spraak verbinden met uw bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)
