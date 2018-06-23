---
title: Conversieprogramma spraak zelfstudie (C#) | Microsoft Docs
titleSuffix: Cognitive Services
description: Informatie over het gebruik van de vertaler spraak-service om tekst in realtime te vertalen.
services: cognitive-services
author: v-jerkin
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.devlang: csharp
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jerkin
ms.openlocfilehash: e82c5c5ccfa6b7de8a9ec111140dad1a40ad44f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344709"
---
# <a name="tutorial-microsoft-translator-wpf-application-in-c"></a>Zelfstudie: Microsoft Translator WPF-toepassing in C#

Deze zelfstudie is een rondleiding van een interactieve spraak vertaling hulpprogramma dat gebruikmaakt van Microsoft Translator spraak vertaling service, een onderdeel van Microsoft cognitieve Services in Azure. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een lijst met de talen die worden ondersteund door de service aanvragen
> * Vastleggen van audio en zendt deze toe aan de service
> * Ontvangen en vertalingen van de spraak weergeven als tekst
> * Eventueel een gesproken (spraak) versie van de vertaling afspelen

Een bestand van de Visual Studio-oplossing voor deze toepassing is [beschikbaar op GitHub](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u een versie van Visual Studio-2017, waaronder de Community-versie. 

De Visual Studio-oplossing maakt ook een installatieprogramma voor de toepassing. U moet de [WiX Toolset](http://wixtoolset.org/) en de [WiX Toolset Visual Studio-extensie](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) om deze functionaliteit te ondersteunen.

U moet ook een abonnementssleutel voor de service conversieprogramma spraak, die u van de Microsoft Azure-dashboard verkrijgen kunt. Er is een gratis prijscategorie beschikbaar waarmee u maximaal 10 uur spraak per maand gratis vertalen. Deze laag is voldoende voor deze zelfstudie.

De derde partij [JSON.Net bibliotheek](https://www.newtonsoft.com/json) (van Newtonsoft) is ook vereist. Deze assembly wordt automatisch geïnstalleerd door NuGet als beide selectievakjes pakket herstellen in de opties voor Visual Studio zijn ingeschakeld.

## <a name="trying-the-translation-app"></a>Probeert de NAT-app

Na het openen van de oplossing Microsoft spraak conversieprogramma (`SpeechTranslator.sln`) in Visual STudio op F5 drukken om te bouwen en de toepassing wordt gestart.  Hoofdvenster van het programma wordt weergegeven.

![[Spraak conversieprogramma hoofdvenster]](media/speech-translator-main-window.png)

Kies op de eerste keer uitvoert, **Accountinstellingen** van de **instellingen** menu openen van het venster hier weergegeven.

![[Spraak conversieprogramma hoofdvenster]](media/speech-translator-settings-window.png)

Plak uw abonnementssleutel Microsoft Translator spraak in dit venster en klik vervolgens op **opslaan.** De sleutel wordt opgeslagen tussen wordt uitgevoerd.

Kies in het hoofdvenster de audio-invoer en uitvoerapparaten die u gebruiken en de From en talen wilt. Als u de audio van de vertaling horen wilt, controleert u of de **TTS** (spraak) optie is ingeschakeld. Als u zien speculatieve gedeeltelijke vertalingen als u uitspreken wilt, schakelt u de **gedeeltelijke resultaten** optie.

Tot slot op **Start** om te beginnen met vertaling. Stel dat u iets die u wilt hebben vertaald en bekijk de herkende tekst en de vertaling worden weergegeven in het venster. Als u de optie TTS ingeschakeld, worden ook de vertaling horen.

## <a name="obtaining-supported-languages"></a>Het verkrijgen van ondersteunde talen

In dit artikel biedt ondersteunt de service Microsoft Translator meer dan vijf dozijn talen voor de tekstvertaling van. Een kleiner aantal talen worden ondersteund voor de vertaling van spraak. Dergelijke talen vereist ondersteuning voor beide schrijffouten (spraakherkenning) en voor de uitvoer-naar-spraak, synthese.

Met andere woorden, voor vertaling spraak moet de taal van de bron een ondersteund voor schrijffouten. De taal van de uitvoer is mogelijk een van de talen die worden ondersteund voor de tekstvertaling, ervan uitgaande dat u wilt dat een tekst-resultaat. Als u spraakuitvoer wilt, kunt u alleen vertalen in een taal die wordt ondersteund voor spraak.

Microsoft kan van tijd tot tijd ondersteuning voor nieuwe talen toevoegen. Daarom moet u niet vastleggen kennis heeft van de ondersteunde talen in uw toepassing. In plaats daarvan biedt de vertaler Speech-API een talen eindpunt waarmee u voor het ophalen van de ondersteunde talen tijdens runtime. U kunt kiezen voor het ontvangen van een of meer lijsten van talen: 

| | |
|-|-|
|`speech`|De talen die worden ondersteund voor spraak schrijffouten. Bron talen voor de spraak vertaling kan zijn.|
|`text`|De talen die worden ondersteund voor de vertaling van tekst naar tekst. Is doel talen voor de spraak vertaling als tekstuitvoer die wordt gebruikt.|
|`tts`|De stemmen voor spraak-synthese ondersteund, elk gekoppeld aan een bepaalde taal. Mag doel talen voor de spraak vertaling wanneer spraak wordt gebruikt. Een bepaalde taal kan worden ondersteund door meer dan een stem.|

Het eindpunt talen vereist niet dat de abonnementssleutel van een en het gebruik ervan niet meegeteld van uw quotum. De URI is `https://dev.microsofttranslator.com/languages` en retourneert de resultaten in JSON-indeling.

De methode `UpdateLanguageSettingsAsync()` in `MainWindow.xaml.cs`, weergegeven, roept het eindpunt talen om de lijst met ondersteunde talen. 

```csharp
private async Task UpdateLanguageSettingsAsync()
{
    Uri baseUri = new Uri("https://" + baseUrl);
    string fullUriString = "/Languages?api-version=1.0&scope=text,speech,tts";
    if (MenuItem_Experimental.IsChecked) fullUriString += "&flight=experimental";            
    Uri fullUri = new Uri(baseUri, fullUriString);

    using (HttpClient client = new HttpClient()) //'client' is the var - using statement ensures the dispose method is used even after an exception.
    {
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, fullUri);

        // get language names for current UI culture:
        request.Headers.Add("Accept-Language", CultureInfo.CurrentUICulture.TwoLetterISOLanguageName);

        // add a client-side trace Id. In case of issues, one can contact support and provide this:
        //string traceId = "SpeechTranslator" + Guid.NewGuid().ToString();
        //request.Headers.Add("X-ClientTraceId", traceId);
        //Debug.Print("TraceId: {0}", traceId);

        client.Timeout = TimeSpan.FromMilliseconds(10000);
        HttpResponseMessage response = await client.SendAsync(request); //make the async call to the web using the client var and passing the built up URI
        response.EnsureSuccessStatusCode(); //causes exception if the return is false

        Debug.Print("Request Id returned: {0}", GetRequestId(response));

        //create dictionaries to hold the language specific data
        spokenLanguages = new Dictionary<string, string>();
        fromLanguages = new Dictionary<string, string>();
        textLanguages = new Dictionary<string, string>();
        isLTR = new Dictionary<string, bool>();
        voices = new Dictionary<string, List<TTsDetail>>();

        JObject jResponse = JObject.Parse(await response.Content.ReadAsStringAsync()); //get the json from the async call with the response var created above, parse it and put it in a var called jResponse - JObject is a newton class

        //Gather the set of TTS voices
        foreach (JProperty jTts in jResponse["tts"])
        {
            JObject ttsDetails = (JObject)jTts.Value;

            string code = jTts.Name;
            string language = ttsDetails["language"].ToString();
            string displayName = ttsDetails["displayName"].ToString();
            string gender = ttsDetails["gender"].ToString();

            if (!voices.ContainsKey(language)) //check dictionary for a specific key value
            {
                voices.Add(language, new List<TTsDetail>()); //add to the dictionary the locale key and a ttsDetail object
            }

            voices[language].Add(new TTsDetail() { Code = code, DisplayName = string.Format("{0} ({1})", displayName, gender) });
        }

        // Gather the set of speech translation languages
        foreach (JProperty jSpeech in jResponse["speech"])
        {
            JObject languageDetails = (JObject)jSpeech.Value;
            string code = jSpeech.Name;
            string simplecode = languageDetails["language"].ToString();
            string displayName = languageDetails["name"].ToString();
            spokenLanguages.Add(code, displayName);
            fromLanguages.Add(code,simplecode);
        }

        spokenLanguages = spokenLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        FromLanguage.Items.Clear();
        foreach (var language in spokenLanguages)
        {
            FromLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key});
        }

        // Gather the set of text translation languages
        foreach (JProperty jText in jResponse["text"])
        {
            JObject languageDetails = (JObject)jText.Value;
            string code = jText.Name;
            string displayName = languageDetails["name"].ToString();
            textLanguages.Add(code, displayName);

            string direction = languageDetails["dir"].ToString().ToLowerInvariant();
            bool LTR = true;
            if (direction.ToLowerInvariant() == "rtl") LTR = false;
            isLTR.Add(code, LTR);
        }

        textLanguages = textLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        ToLanguage.Items.Clear();
        foreach (var language in textLanguages)
        {
            ToLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key });
        }

        if (Properties.Settings.Default.FromLanguageIndex >= 0) FromLanguage.SelectedIndex = Properties.Settings.Default.FromLanguageIndex;
        else
        {
            for(int i=0; i < FromLanguage.Items.Count; ++i)
            {
                ComboBoxItem item = (ComboBoxItem)FromLanguage.Items[i];
                if(CultureInfo.CurrentUICulture.Name.Equals((string)item.Tag, StringComparison.OrdinalIgnoreCase))
                {
                    FromLanguage.SelectedIndex = i;
                }
            }
        }
        if (Properties.Settings.Default.ToLanguageIndex >= 0) ToLanguage.SelectedIndex = Properties.Settings.Default.ToLanguageIndex;
        else
        {
            Random rnd = new Random(DateTime.Now.Millisecond);
            ToLanguage.SelectedIndex = (rnd.Next() % textLanguages.Count);
        }
    }
}
```

Deze methode constructs eerst een HTTP-aanvraag voor het eindpunt talen, vraagt alle drie lijsten met talen (`text`, `speech`, en `tts`).

Het eindpunt van de talen van de aanvraag gebruikt `Accept-Languages` header om te bepalen van de taal waarin de namen van de talen die worden weergegeven. De taal Engels luidsprekers bekend als 'Duitse' wordt genoemd in het Duits ' Deutsch' en 'Alemán' in het Spaans en de lijst met talen weerspiegelt bijvoorbeeld deze verschillen. De standaardtaal van het systeem wordt gebruikt voor deze header.

Nadat de aanvraag is verzonden en wordt het JSON-antwoord ontvangen, het antwoord moeten worden verdeeld in interne gegevensstructuur. Deze structuren worden vervolgens gebruikt om de taal van en naar taalinstellingen menu's samen te stellen. 

Aangezien de stemmen die beschikbaar zijn, is afhankelijk van de taal aan die door de gebruiker gekozen, is het niet mogelijk voor het instellen van het menu stem nog. In plaats daarvan worden de beschikbare stemmen voor elke taal opgeslagen voor later gebruik. De `ToLanguage_SelectionChanged` handler (in het bronbestand met dezelfde) later het menu Voice-updates door het aanroepen van `UpdateVoiceComboBox()` wanneer de gebruiker kiest voor een taal op. 

Plezier, een taal voor willekeurig geselecteerd als de gebruiker heeft niet de toepassing voordat wordt uitgevoerd. (De menu-instellingen worden opgeslagen tussen sessies.)

## <a name="authenticating-requests"></a>Aanvragen verifiëren

Aanmelden bij de Microsoft Translator spraak-service moet u de sleutel van uw Azure-abonnement in de header verzenden als de waarde voor `Ocp-Apim-Subscription-Key` in de verbindingsaanvraag.

## <a name="translation-overview"></a>Overzicht van de vertaling

De API vertalen (WebSockets eindpunt `wss://dev.microsofttranslator.com/speech/translate`) accepteert audio worden vertaald in mono, 16 kHz, 16-bits ondertekend WAVE-indeling. De service retourneert een of meer JSON antwoorden met zowel de herkende en vertaalde tekst. Als spraak is aangevraagd, wordt een audiobestand verzonden.

De gebruiker ervoor kiest de audio-bron via het menu bestand microfoon/invoer. De audio kan afkomstig zijn van een audioapparaat (zoals een microfoon) of van een `.WAV` bestand.

De methode `StartListening_Click` wordt aangeroepen wanneer de gebruiker op de knop Start. Deze gebeurtenis-handler roept `Connect()` om te beginnen met het proces van het audio verzenden naar het eindpunt van de service-API. De `Connect()` methode voert de volgende taken uit:


> [!div class="checklist"]
> * Gebruikersinstellingen ophalen uit het hoofdvenster en deze te valideren
> * Tijdens de initialisatie van de audio-invoer en uitvoer stromen
> * Het aanroepen van `ConnectAsync()` voor het afhandelen van de rest van het werk

`ConnectAsync()`, op zijn beurt verwerkt de volgende taken:

> [!div class="checklist"]
> * Verificatie met sleutel in de header Azure-abonnement `Ocp-Apim-Subscription-Key`
> * Maken van een `SpeechClient` exemplaar (gevonden in `SpeechClient.cs`) om te communiceren met de service
> * Tijdens de initialisatie van `TextMessageDecoder` en `BinaryMessageDecoder` exemplaren (Zie `SpeechResponseDecoder.cs`) voor het afhandelen van reacties
> * Verzenden van de audio via de `SpeechClient` exemplaar met de service conversieprogramma spraak
> * Ontvangen en verwerken van de resultaten van de vertaling

De verantwoordelijkheden van `SpeechClient` minder zijn:

> [!div class="checklist"]
> * Tot stand brengen van een WebSocket-verbinding met het conversieprogramma spraak-service
> * Audiogegevens verzenden en ontvangen van antwoorden via de socket

## <a name="a-closer-look"></a>Nader bekijken

Deze moet duidelijker nu hoe onderdelen van de toepassing werken samen om de aanvraag vertaling uitvoeren. Eens kijken wat code, gericht op de relevante delen.

Hier volgt een gedeeltelijke versie van `Connect()` die de audio stromen instelling weergegeven:

```csharp
private void Connect()
{
    if (this.currentState != UiState.ReadyToConnect) return;

    Stopwatch watch = Stopwatch.StartNew();
    UpdateUiState(UiState.Connecting);

    // Omitted: code to validate UI settings

    string tag = ((ComboBoxItem)Mic.SelectedItem).Tag as string;
    string audioFileInputPath = null;
    if (tag == "File")
    {
        audioFileInputPath = this.AudioFileInput.Text;
        foreach (string currFile in audioFileInputPath.Split('|'))
        {
            if (!File.Exists(currFile))
            {
                SetMessage(String.Format($"Invalid audio source: selected file {currFile} does not exist."), "", MessageKind.Error);
                UpdateUiState(UiState.ReadyToConnect);
                return;
            }
        }
    }
    bool shouldSuspendInputAudioDuringTTS = this.CutInputAudioCheckBox.IsChecked.HasValue ? this.CutInputAudioCheckBox.IsChecked.Value : false;

    correlationId = Guid.NewGuid().ToString("D").Split('-')[0].ToUpperInvariant();

    // Setup speech translation client options
    SpeechClientOptions options;

    string voicename = "";
    if (this.Voice.SelectedItem != null)
    {
        voicename = ((ComboBoxItem)this.Voice.SelectedItem).Tag.ToString();
    }
    options = new SpeechTranslateClientOptions()
    {
        TranslateFrom = ((ComboBoxItem)this.FromLanguage.SelectedItem).Tag.ToString(),
        TranslateTo = ((ComboBoxItem)this.ToLanguage.SelectedItem).Tag.ToString(),
        Voice = voicename,
    };
    
    options.Hostname = baseUrl;
    options.AuthHeaderKey = "Authorization";
    options.AuthHeaderValue = ""; // set later in ConnectAsync.
    options.ClientAppId = new Guid("EA66703D-90A8-436B-9BD6-7A2707A2AD99");
    options.CorrelationId = this.correlationId;
    options.Features = GetFeatures().ToString().Replace(" ", "");
    options.Profanity = ((SpeechClient.ProfanityFilter)Enum.Parse(typeof(SpeechClient.ProfanityFilter), GetProfanityLevel(), true)).ToString();
    options.Experimental = MenuItem_Experimental.IsChecked;

    // Setup player and recorder but don't start them yet.
    WaveFormat waveFormat = new WaveFormat(16000, 16, 1);

    // WaveProvider for incoming TTS
    // We use a rather large BufferDuration because we need to be able to hold an entire utterance.
    // TTS audio is received in bursts (faster than real-time).
    textToSpeechBytes = 0;
    playerTextToSpeechWaveProvider = new BufferedWaveProvider(waveFormat);
    playerTextToSpeechWaveProvider.BufferDuration = TimeSpan.FromMinutes(5);

    ISampleProvider sampleProvider = null;
    if (audioFileInputPath != null)
    {
        // Setup mixing of audio from input file and from TTS
        playerAudioInputWaveProvider = new BufferedWaveProvider(waveFormat);
        var srce1 = new Pcm16BitToSampleProvider(playerTextToSpeechWaveProvider);
        var srce2 = new Pcm16BitToSampleProvider(playerAudioInputWaveProvider);
        var mixer = new MixingSampleProvider(srce1.WaveFormat);
        mixer.AddMixerInput(srce1);
        mixer.AddMixerInput(srce2);
        sampleProvider = mixer;
    }
    else
    {
        recorder = new WaveIn();
        recorder.DeviceNumber = (int)((ComboBoxItem)Mic.SelectedItem).Tag;
        recorder.WaveFormat = waveFormat;
        recorder.DataAvailable += OnRecorderDataAvailable;
        sampleProvider = playerTextToSpeechWaveProvider.ToSampleProvider();
    }

    if (!batchMode)
    {
        player = new WaveOut();
        player.DeviceNumber = (int)((ComboBoxItem)Speaker.SelectedItem).Tag;
        player.Init(sampleProvider);
    }

    this.audioBytesSent = 0;

    string logAudioFileName = null;
    if (LogSentAudio.IsChecked|| LogReceivedAudio.IsChecked)
    {
        string logAudioPath = System.IO.Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), Properties.Settings.Default.OutputDirectory);
        try
        {
            Directory.CreateDirectory(logAudioPath);
        }
        catch
        {
            this.AddItemToLog(string.Format("Could not create folder {0}", logAudioPath));
        }

        if (LogSentAudio.IsChecked)
        {
            logAudioFileName = System.IO.Path.Combine(logAudioPath, string.Format("audiosent_{0}.wav", this.correlationId));
        }

        if (LogReceivedAudio.IsChecked)
        {
            string fmt = System.IO.Path.Combine(logAudioPath, string.Format("audiotts_{0}_{{0}}.wav", this.correlationId));
            this.audioReceived = new BinaryMessageDecoder(fmt);
        }
    }
}
```

Een aanzienlijk deel van `Connect()` omvat het maken van een `SpeechClientOptions` exemplaar (Zie `SpeechClientOptions.cs`) om de opties voor vertaling te bevatten. Opties omvatten de benodigde informatie om verbinding met de service (zoals verificatiesleutel en hostnaam) en de functies voor de vertaling gebruikt. De velden hier toewijzen aan de header-velden en HTTP-parameters die worden weergegeven door [de vertaler Speech-API](http://docs.microsofttranslator.com/speech-translate.html).

`Connect()` ook maakt en initialiseert de audio-invoerapparaat (variabele `sampleProvider`) die fungeert als de bron van de spraak worden vertaald. Dit apparaat is een hardware-invoerapparaat zoals een microfoon of een bestand met WAVE-audiogegevens.

Hier volgt de `ConnectAsync()` methode die wordt de `speechClient` klasse en hooks up anonieme functies voor het afhandelen van tekst en binaire antwoorden van de service.

```csharp
private async Task ConnectAsync(SpeechClientOptions options, bool suspendInputAudioDuringTTS)
{
    await ADMAuthenticate(options);
    
    TextMessageDecoder textDecoder;
    
    s2smtClient = new SpeechClient((SpeechTranslateClientOptions)options, CancellationToken.None);
    
    s2smtClient.OnBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnEndOfBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnTextData += (c, a) => { textDecoder.AppendData(a); lastReceivedPacketTick = DateTime.Now.Ticks; };
    s2smtClient.OnEndOfTextData += (c, a) =>
    {
        textDecoder.AppendData(a);
        lastReceivedPacketTick = DateTime.Now.Ticks;
        textDecoder
            .Decode()
            .ContinueWith(t =>
            {
                if (t.IsFaulted)
                {
                    Log(t.Exception, "E: Failed to decode incoming text message.");
                }
                else
                {
                    object msg = t.Result;
                    if (msg.GetType() == typeof(FinalResultMessage))
                    {
                        // omitted: code to process final binary result
                    }
                    if (msg.GetType() == typeof(PartialResultMessage))
                    {
                        // omitted: code to process partial binary result
                    }
                }
            });
    };
    s2smtClient.Failed += (c, ex) =>
    {
        Log(ex, "E: SpeechTranslation client reported an error.");
    };
    s2smtClient.Disconnected += (c, ea) =>
    {
        SafeInvoke(() =>
        {
            // We only care to react to server disconnect when our state is Connected. 
            if (currentState == UiState.Connected)
            {
                Log("E: Connection has been lost.");
                Log($"E: Errors (if any): \n{string.Join("\n", s2smtClient.Errors)}");
                Disconnect();
            }
        });
    };
    await s2smtClient.Connect();
}
```

Als de verificatie is gelukt, de methode maakt u de `SpeechClient` exemplaar. De `SpeechClient` klasse (in `SpeechClient.cs`) gebeurtenis-handlers na ontvangst van binaire bestanden en tekst gegevens aanroept. Aanvullende handlers worden aangeroepen als de verbinding is mislukt of de verbinding verbreekt.

Binaire gegevens is audio (-naar-spraak uitvoer) door de service worden verzonden wanneer TTS is ingeschakeld. Tekst is een gedeeltelijke of een volledige vertaling van de gesproken tekst. Dus na instantiëren, Hiermee worden de methode van de functies voor het afhandelen van deze berichten: audio opslaan voor later afspelen en tekst door deze in het venster wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld is een veelzijdige-toepassing die het gebruik van de vertaler Speech-API. Er zijn als zodanig een evenwichtige aantal bewegende onderdelen om te begrijpen. U hebt doorlopen tot de belangrijkste bits. Het kan enkele onderbrekingspunten instellen in Visual Studio en het proces pakketvertaling doorlopen instructieve zijn voor de rest. Als u de voorbeeldtoepassing begrijpt, bent u de service conversieprogramma spraak gebruiken in uw eigen toepassingen voorzien.

> [!div class="nextstepaction"]
> [Microsoft Translator Speech-API-referentiemateriaal](http://docs.microsofttranslator.com/speech-translate.html)
