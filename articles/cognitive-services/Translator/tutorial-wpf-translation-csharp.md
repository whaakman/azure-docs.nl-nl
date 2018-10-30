---
title: 'Zelfstudie: Een WPF-toepassing maken in C# - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie leert u hoe u de Translator Text-API gebruikt om tekst te vertalen, een gelokaliseerde lijst met ondersteunde talen op te halen, en meer, door een WPF-toepassing te bouwen met behulp van C#.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: tutorial
ms.date: 07/20/2018
ms.author: erhopf
ms.openlocfilehash: e302c1aa1cd4021b0d449fd981181b790546c0f8
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647473"
---
# <a name="tutorial-write-a-wpf-application-for-translator-text-using-c35"></a>Zelfstudie: Een WPF-toepassing voor Translator Text maken met behulp van C&#35;

In deze zelfstudie bouwt u een interactief hulpprogramma om tekst te vertalen met behulp van de Translator Text-API (V3), een onderdeel van Microsoft Cognitive Services in Azure. U leert het volgende:

> [!div class="checklist"]
> * Een lijst ophalen met talen die door de service worden ondersteund
> * Door een gebruiker ingevoerde tekst vertalen van de ene naar de andere taal

Deze toepassing biedt ook integratie met twee andere Microsoft Cognitive Services.

|||
|-|-|
|[Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Kan worden gebruikt om de brontaal van de te vertalen tekst automatisch te detecteren|
|[Bing Spellingcontrole](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Voor een Engelse brontekst kan deze worden gebruikt om spelfouten te corrigeren, zodat de vertaling nauwkeuriger wordt

![[Het actieve zelfstudieprogramma]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Vereisten

U hebt [Visual Studio 2017](https://www.visualstudio.com/downloads/) nodig om deze code op Windows uit te voeren. (De gratis Community-editie volstaat.)

U hebt ook abonnementssleutels nodig voor de drie Azure-services die in het programma worden gebruikt. U kunt een sleutel voor de Translator Text-service verkrijgen via het Azure-dashboard. Er is een gratis prijscategorie beschikbaar waarmee u kosteloos twee miljoen tekens per maand kunt vertalen.

Voor zowel de Tekstanalyse- als de Bing Spellingcontrole-service is er een gratis proefversie beschikbaar, waarvoor u zich kunt aanmelden via [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/). U kunt ook een abonnement voor beide services maken via het Azure-dashboard. Er is een gratis versie beschikbaar voor Tekstanalyse.

De broncode voor deze zelfstudie is hieronder te vinden. Uw abonnementssleutels moeten in de broncode als de variabelen `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY` enz., in `MainWindow.xaml.cs` worden gekopieerd.

> [!IMPORTANT]
> De Tekstanalyse-service is beschikbaar in meerdere regio's. De URI in de broncode van deze zelfstudie bevindt zich in de regio `westus`. Deze regio wordt gebruikt voor gratis proefversies. Als u een abonnement in een andere regio hebt, moet u deze URI dienovereenkomstig bijwerken.

## <a name="source-code"></a>Broncode

Dit is de broncode voor de tekst-API van Microsoft Translator. Kopieer de broncode naar het juiste bestand in een nieuw WPF-project in Visual Studio om de app uit te voeren.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Dit is het onderliggende-codebestand dat de toepassing voorziet van functionaliteit.

```csharp
using System;
using System.Windows;
using System.Net;
using System.Net.Http;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Text;

// NOTE: Add assembly references to System.Runtime.Serialization, System.Web, and System.Web.Extensions.

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace MSTranslatorTextDemo
{
    /// <summary>
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string from
    /// one language to another. The languages are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected. English text is spell-checked.
    /// </summary>
    public partial class MainWindow : Window
    {
        // Translator text subscription key from Microsoft Azure dashboard
        const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

        public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
        const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
        const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

        private string[] languageCodes;     // array of language codes

        // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
        private SortedDictionary<string, string> languageCodesAndTitles =
            new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

        public MainWindow()
        {
            // at least show an error dialog if there's an unexpected error
            AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

            if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
                || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
                || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
            {
                MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                    "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                    "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
                System.Windows.Application.Current.Shutdown();
            }
            else
            {
                InitializeComponent();          // start the GUI
                GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
                PopulateLanguageMenus();        // fill the drop-down language lists
            }
        }

        // Global exception handler to display error message and exit
        private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
        {
            Exception e = (Exception)args.ExceptionObject;
            MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }

        // ***** POPULATE LANGUAGE MENUS
        private void PopulateLanguageMenus()
        {
            // Add option to automatically detect the source language
            FromLanguageComboBox.Items.Add("Detect");

            int count = languageCodesAndTitles.Count;
            foreach (string menuItem in languageCodesAndTitles.Keys)
            {
                FromLanguageComboBox.Items.Add(menuItem);
                ToLanguageComboBox.Items.Add(menuItem);
            }

            // set default languages
            FromLanguageComboBox.SelectedItem = "Detect";
            ToLanguageComboBox.SelectedItem = "English";
        }

        // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
        private string DetectLanguage(string text)
        {
            string uri = TEXT_ANALYTICS_API_ENDPOINT + "languages?numberOfLanguagesToDetect=1";

            // create request to Text Analytics API
            HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_ANALYTICS_API_SUBSCRIPTION_KEY);
            detectLanguageWebRequest.Method = "POST";

            // create and send body of request
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            string jsonText = serializer.Serialize(text);

            string body = "{ \"documents\": [ { \"id\": \"0\", \"text\": " + jsonText + "} ] }";
            byte[] data = Encoding.UTF8.GetBytes(body);
            detectLanguageWebRequest.ContentLength = data.Length;

            using (var requestStream = detectLanguageWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);

            HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

            // read and parse JSON response
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);

            // fish out the detected language code
            var languageInfo = jsonResponse["documents"][0]["detectedLanguages"][0];
            if (languageInfo["score"] > (decimal)0.5)
                return languageInfo["iso6391Name"];
            else
                return "";
        }

        // ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
        private string CorrectSpelling(string text)
        {
            string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

            // create request to Bing Spell Check API
            HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", BING_SPELL_CHECK_API_SUBSCRIPTION_KEY);
            spellCheckWebRequest.Method = "POST";
            spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

            // create and send body of request
            string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
            byte[] data = Encoding.UTF8.GetBytes(body);
            spellCheckWebRequest.ContentLength = data.Length;
            using (var requestStream = spellCheckWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);
            HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

            // read and parse JSON response and get spelling corrections
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);
            var flaggedTokens = jsonResponse["flaggedTokens"];

            // construct sorted dictionary of corrections in reverse order in string (right to left)
            // so that making a correction can't affect later indexes
            var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
            for (int i = 0; i < flaggedTokens.Length; i++)
            {
                var correction = flaggedTokens[i];
                var suggestion = correction["suggestions"][0];  // consider only first suggestion
                if (suggestion["score"] > (decimal)0.7)         // take it only if highly confident
                    corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                        { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
            }

            // apply the corrections in order from right to left
            foreach (int i in corrections.Keys)
            {
                var oldtext = corrections[i][0];
                var newtext = corrections[i][1];

                // apply capitalization from original text to correction - all caps or initial caps
                if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
                else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

                text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
            }

            return text;
        }

        // ***** GET TRANSLATABLE LANGUAGE CODES
        private void GetLanguagesForTranslate()
        {
            // send request to get supported language codes
            string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
            WebRequest WebRequest = WebRequest.Create(uri);
            WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
            WebRequest.Headers.Add("Accept-Language", "en");
            WebResponse response = null;
            // read and parse the JSON response
            response = WebRequest.GetResponse();
            using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
            {
                var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
                var languages = result["translation"];

                languageCodes = languages.Keys.ToArray();
                foreach (var kv in languages)
                {
                    languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
                }
            }
        }

        // ***** PERFORM TRANSLATION ON BUTTON CLICK
        private async void TranslateButton_Click(object sender, EventArgs e)
        {
            string textToTranslate = TextToTranslate.Text.Trim();

            string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
            string fromLanguageCode;

            // auto-detect source language if requested
            if (fromLanguage == "Detect")
            {
                fromLanguageCode = DetectLanguage(textToTranslate);
                if (!languageCodes.Contains(fromLanguageCode))
                {
                    MessageBox.Show("The source language could not be detected automatically " +
                        "or is not supported for translation.", "Language detection failed",
                        MessageBoxButton.OK, MessageBoxImage.Error);
                    return;
                }
            }
            else
                fromLanguageCode = languageCodesAndTitles[fromLanguage];

            string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

            // spell-check the source text if the source language is English
            if (fromLanguageCode == "en")
            {
                if (textToTranslate.StartsWith("-"))    // don't spell check in this case
                    textToTranslate = textToTranslate.Substring(1);
                else
                {
                    textToTranslate = CorrectSpelling(textToTranslate);
                    TextToTranslate.Text = textToTranslate;     // put corrected text into input field
                }
            }

            // handle null operations: no text or same source/target languages
            if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
            {
                TranslatedTextLabel.Content = textToTranslate;
                return;
            }

            // send HTTP request to perform the translation
            string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
            string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

            System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
                request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
                var translations = result[0]["translations"];
                var translation = translations[0]["text"];

                // Update the translation field
                TranslatedTextLabel.Content = translation;
            }
        }
    }
}
```

### <a name="mainwindowxaml"></a>MainWindow.xaml

Dit bestand definieert de gebruikersinterface voor de toepassing, een WPF-formulier. Als u uw eigen versie van het formulier wilt ontwerpen, hebt u deze XAML niet nodig.

```xml
<Window x:Class="MSTranslatorTextDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:MSTranslatorTextDemo"
        mc:Ignorable="d"
        Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
    <Grid>
        <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
        <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
        <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
        <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

        <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
        <ComboBox x:Name="ToLanguageComboBox"
                HorizontalAlignment="Left"
                Margin="306,88,0,0"
                VerticalAlignment="Top"
                Width="175" FontSize="14" TabIndex="2">

        </ComboBox>
        <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
        <ComboBox x:Name="FromLanguageComboBox"
            HorizontalAlignment="Left"
            Margin="42,88,0,0"
            VerticalAlignment="Top"
            Width="175" FontSize="14" TabIndex="1"/>
        <Label x:Name="TranslatedTextLabel" Content="Translation appears here" HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
    </Grid>
</Window>
```

## <a name="service-endpoints"></a>Service-eindpunten

De Microsoft Translator-service heeft een aantal eindpunten die zorgen voor verschillende onderdelen van de vertaalfunctionaliteit. De eindpunten die in deze zelfstudie worden gebruikt, zijn:

|||
|-|-|
|`Languages`|Retourneert de set talen die momenteel worden ondersteund door andere bewerkingen van de Translator Text-API.|
|`Translate`|Retourneert een vertaling van de brontekst in de doeltaal als er een brontekst, een code voor de brontaal en een code voor de doeltaal zijn opgegeven.|

## <a name="the-translation-app"></a>De vertaalapp

De gebruikersinterface van de vertaaltoepassing is gebouwd met behulp van Windows Presentation Foundation (WPF). Volg onderstaande stappen om een nieuw WPF-project te maken in Visual Studio.

* Kies in het menu **Bestand** voor **Nieuw > Project**.
* Open in het venster Nieuw project **Geïnstalleerd > Sjablonen > Visual C#**. In het midden van het dialoogvenster verschijnt een lijst met de beschikbare projectsjablonen.
* Controleer of **.NET Framework 4.5.2** is geselecteerd in de vervolgkeuzelijst boven de lijst met projectsjablonen.
* Klik op **WPF App (.NET Framework)** in de lijst met projectsjablonen.
* Geef het nieuwe project en de oplossing waarvan het deel uitmaakt een nieuwe naam met behulp van de velden onderaan het dialoogvenster.
* Klik op **OK** om het nieuwe project en de oplossing te maken.

![[Een nieuwe WPF-app maken in Visual Studio]](media/translator-text-csharp-new-project.png)

Voeg verwijzingen naar de volgende .NET Framework-assembly's toe aan uw project.

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Installeer ook het NuGet-pakket `Newtonsoft.Json` in uw project.

Zoek nu in Solution Explorer het `MainWindow.xaml`-bestand op en open het. In eerste instantie is het leeg. Hier ziet u hoe de voltooide gebruikersinterface eruit hoort te zien, voorzien van aantekeningen met de namen van de besturingselementen in het blauw. Gebruik dezelfde namen voor de besturingselementen in uw gebruikersinterface, aangezien deze ook voorkomen in de code.

![[Weergave met aantekeningen van het hoofdvenster van de Visual Studio-designer]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> De broncode voor deze zelfstudie bevat de XAML-bron voor dit formulier. U kunt deze in uw project plakken. Dan hoeft u het formulier niet te bouwen in Visual Studio.

* `FromLanguageComboBox` *(ComboBox)* - Hiermee wordt een lijst weergegeven met alle talen die door Microsoft Translator worden ondersteund voor het vertalen van tekst. De gebruiker selecteert de taal waaruit hij vertaalt.
* `ToLanguageComboBox` *(ComboBox)* - Hiermee wordt dezelfde lijst met talen weergegeven als met `FromComboBox`, maar dit element wordt gebruikt om de taal te selecteren waarin de gebruiker vertaalt.
* `TextToTranslate` *(TextBox)* - Hier voert de gebruiker de te vertalen tekst in.
* `TranslateButton` *(Button)* - De gebruiker klikt op deze knop (of drukt op Enter) om de tekst te vertalen.
* `TranslatedTextLabel` *(Label)* - Hier wordt de vertaling van de tekst van de gebruiker weergegeven.

Als u een eigen versie van dit formulier maakt, hoeft die niet *precies* zo te zijn als het formulier dat hier wordt gebruikt. Zorg er wel voor dat de vervolgkeuzelijsten voor de talen zo breed zijn dat de namen van de talen niet worden afgebroken.

## <a name="the-mainwindow-class"></a>De klasse MainWindow

Het onderliggende-codebestand `MainWindow.xaml.cs` moet de code bevatten die ervoor zorgt dat het programma doet wat het doet. Het werk vindt plaats op twee momenten:

* Wanneer het programma wordt opgestart en `MainWindow` is geïnstantieerd, wordt de lijst met talen opgehaald met behulp van Translator en API's en worden de vervolgkeuzemenu's ermee ingevuld. Deze taak wordt één keer uitgevoerd, aan het begin van iedere sessie.

* Wanneer de gebruiker op de knop **Vertalen** klikt, worden de taalkeuzes van de gebruiker en de tekst die hij heeft ingevoerd, opgehaald. Vervolgens wordt de API `Translate` aangeroepen om de vertaling uit te voeren. Er kunnen ook andere functies worden aangeroepen om de taal van de tekst te bepalen en om spelfouten te verbeteren voordat de tekst wordt vertaald.

Kijk eens naar het begin van de klasse:

```csharp
public partial class MainWindow : Window
{
    // Translator text subscription key from Microsoft Azure dashboard
    const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

    public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
    const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
    const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

    private string[] languageCodes;     // array of language codes

    // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
    private SortedDictionary<string, string> languageCodesAndTitles =
        new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

    public MainWindow()
    {
        // at least show an error dialog if there's an unexpected error
        AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

        if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
            || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
            || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
        {
            MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }
        else
        {
            InitializeComponent();          // start the GUI
            GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
            PopulateLanguageMenus();        // fill the drop-down language lists
        }
    }
// more to come
}
```

Twee lidvariabelen die hier worden gedeclareerd, bevatten informatie over onze talen:

|||
|-|-|
|`languageCodes`<br>tekenreeksmatrix|Hiermee worden de taalcodes opgeslagen in de cache. De Translator-service gebruikt korte codes om talen te identificeren, bijvoorbeeld `en` voor Engels.|
|`languageCodesAndTitles`<br>SortedDictionary|Hiermee worden de beschrijvende namen in de gebruikersinterface terugverwezen naar de korte codes die in de API worden gebruikt. Ze worden op alfabetische volgorde gesorteerd, zonder rekening te houden met het gebruik van hoofdletters.|

De eerste code die wordt uitgevoerd door de toepassing is de constructor `MainWindow`. Stel eerst de methode `HandleExceptions` in als handler voor globale fouten. Op deze manier verschijnt er in ieder geval een foutmelding als een uitzondering niet wordt verwerkt.

Controleer vervolgens of de API-abonnementssleutels allemaal precies 32 tekens bevatten. Als dit niet het geval is, is er waarschijnlijk *iemand* die zijn API-sleutels niet heeft geplakt. In dit geval geeft u een foutbericht weer en sluit u af. (Als de sleutels aan deze voorwaarde voldoen, wil dat natuurlijk nog niet zeggen dat deze geldig zijn.)

Als er sleutels met minstens 32 tekens zijn, brengt de aanroep `InitializeComponent()` de gebruikersinterface op gang door de XAML-beschrijving van het hoofdtoepassingsvenster te zoeken, te laden en te instantiëren.

Ten slotte stelt u de vervolgkeuzelijsten voor de talen in. Voor deze taak moet u drie afzonderlijke methodeaanroepen uitvoeren, die in de volgende secties in detail worden behandeld.

## <a name="get-supported-languages"></a>Ondersteunde talen ophalen

De Microsoft Translator-service ondersteunt nu in totaal 61 talen en af en toe kunnen er nieuwe talen worden toegevoegd. Daarom kunt u de ondersteunde talen beter niet vastleggen in uw programma. Vraag in plaats daarvan aan de Translator-service welke talen worden ondersteund. Tekst in een van de ondersteunde talen kan worden vertaald in een andere ondersteunde taal.

Roep de `Languages`-API aan om de lijst met ondersteunde talen op te halen.

De `Languages`-API maakt gebruik van een optionele GET-queryparameter, *scope*. *scope* kan een van deze drie waarden hebben: `translation`, `transliteration` en `dictionary`. In deze code wordt de waarde `translation` gebruikt.

De `Languages`-API maakt ook gebruik van een optionele HTTP-header, `Accept-Language`. De waarde van deze header bepaalt de taal waarin de namen van de ondersteunde talen worden geretourneerd. De waarde moet een grammaticaal correcte BCP 47-taalcode zijn. Deze code gebruikt de waarde `en` om de namen van de talen in het Engels op te halen.

De `Languages`-API retourneert een JSON-antwoord dat lijkt op het antwoord hieronder.

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
}
```

Deze code gebruikt de NewtonSoft.Json-methode [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm), zodat taalcodes (bijvoorbeeld `af`) en namen van talen (bijvoorbeeld `Afrikaans`) kunnen worden opgehaald.

Met deze achtergrondkennis kunt u de volgende methode maken om de taalcodes en de bijbehorende namen op te halen.

```csharp
private void GetLanguagesForTranslate()
{
    // send request to get supported language codes
    string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
    WebRequest WebRequest = WebRequest.Create(uri);
    WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
    WebRequest.Headers.Add("Accept-Language", "en");
    WebResponse response = null;
    // read and parse the JSON response
    response = WebRequest.GetResponse();
    using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
    {
        var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
        var languages = result["translation"];

        languageCodes = languages.Keys.ToArray();
        foreach (var kv in languages)
        {
            languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
        }
    }
}
```

Met `GetLanguagesForTranslate()` maakt u eerst de HTTP-aanvraag. De querytekenreeksparameter `scope=translation` vraagt alleen de talen die worden ondersteund voor het vertalen van tekst. De abonnementssleutel voor de API voor het vertalen van tekst wordt toegevoegd aan de aanvraagheaders. De header `Accept-Language` met de waarde `en` wordt toegevoegd, zodat de ondersteunde talen worden geretourneerd in het Engels.

Nadat de aanvraag is voltooid, wordt het JSON-antwoord geparseerd en geconverteerd naar een woordenlijst en vervolgens worden de taalcodes toegevoegd aan de lidvariabele `languageCodes`. De sleutel-waardeparen die de taalcodes en de beschrijvende namen van de talen bevatten, worden doorgegeven en toegevoegd aan de lidvariabele `languageCodesAndTitles`. (In de vervolgkeuzelijsten in het formulier worden de beschrijvende namen weergegeven, maar u hebt de codes wel nodig om de vertaling aan te vragen.)

## <a name="populate-the-language-menus"></a>De taal menu's invullen

Het grootste deel van de gebruikersinterface wordt gedefinieerd in XAML, dus voor het instellen hoeft u niet veel te doen, behalve dat u `InitializeComponent()` moet aanroepen. Het enige andere wat u moet doen, is de beschrijvende namen van de talen toevoegen aan de vervolgkeuzelijsten **Vertalen van** en **Vertalen naar**. Dit kunt u doen in `PopulateLanguageMenus()`.

```csharp
private void PopulateLanguageMenus()
{
    // Add option to automatically detect the source language
    FromLanguageComboBox.Items.Add("Detect");

    int count = languageCodesAndTitles.Count;
    foreach (string menuItem in languageCodesAndTitles.Keys)
    {
        FromLanguageComboBox.Items.Add(menuItem);
        ToLanguageComboBox.Items.Add(menuItem);
    }

    // set default languages
    FromLanguageComboBox.SelectedItem = "Detect";
    ToLanguageComboBox.SelectedItem = "English";
}
```

Het invullen van de menu's is gewoon een kwestie van het doorlopen van de woordenlijst `languageCodesAndTitles` en het toevoegen van elke sleutel (de beschrijvende naam) aan beide menu's. Nadat de menu's zijn ingevuld, worden de standaardwaarden voor de 'naar'- en de 'van'-taal ingesteld op **Detecteren** (om de taal automatisch te detecteren) en **Engels**.

> [!TIP]
> Als er geen standaardselectie is voor de menu's, kan de gebruiker op **Vertalen** klikken, zonder dat hij eerst een 'naar'- of 'van'-taal hoeft te kiezen. De standaardinstellingen zorgen ervoor dat het niet meer nodig is om dit probleem op te lossen.

Nu `MainWindow` is geïnitialiseerd en de gebruikersinterface is gemaakt, wacht de code tot de gebruiker op de knop **Vertalen** klikt.

## <a name="perform-translation"></a>Vertalen

Wanneer de gebruiker op **Vertalen** klikt, roept WPF de gebeurtenis-handler `TranslateButton_Click()` aan, die hier wordt weergegeven.

```csharp
private async void TranslateButton_Click(object sender, EventArgs e)
{
    string textToTranslate = TextToTranslate.Text.Trim();

    string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
    string fromLanguageCode;

    // auto-detect source language if requested
    if (fromLanguage == "Detect")
    {
        fromLanguageCode = DetectLanguage(textToTranslate);
        if (!languageCodes.Contains(fromLanguageCode))
        {
            MessageBox.Show("The source language could not be detected automatically " +
                "or is not supported for translation.", "Language detection failed",
                MessageBoxButton.OK, MessageBoxImage.Error);
            return;
        }
    }
    else
        fromLanguageCode = languageCodesAndTitles[fromLanguage];

    string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

    // spell-check the source text if the source language is English
    if (fromLanguageCode == "en")
    {
        if (textToTranslate.StartsWith("-"))    // don't spell check in this case
            textToTranslate = textToTranslate.Substring(1);
        else
        {
            textToTranslate = CorrectSpelling(textToTranslate);
            TextToTranslate.Text = textToTranslate;     // put corrected text into input field
        }
    }

    // handle null operations: no text or same source/target languages
    if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
    {
        TranslatedTextLabel.Content = textToTranslate;
        return;
    }

    // send HTTP request to perform the translation
    string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
    string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

    System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
    var requestBody = JsonConvert.SerializeObject(body);

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Post;
        request.RequestUri = new Uri(uri);
        request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
        request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();

        var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
        var translations = result[0]["translations"];
        var translation = translations[0]["text"];

        // Update the translation field
        TranslatedTextLabel.Content = translation;
    }
}
```

Eerst moeten de 'naar'- en de 'van'-taal, samen met de tekst die de gebruiker heeft ingevoerd, worden opgehaald uit het formulier.

Als de brontaal is ingesteld op **Detecteren**, moet u `DetectLanguage()` aanroepen om de taal van de tekst te bepalen. Mogelijk wordt de taal van de tekst niet ondersteund door de Translator-API's (er kunnen veel meer talen worden gedetecteerd dan vertaald) of is de Tekstanalyse-API niet in staat om de taal te detecteren. In dat geval geeft u een bericht weer om de gebruiker hiervan op de hoogte te stellen en gaat u terug zonder een vertaling te geven.

Als de brontaal Engels is (ongeacht of deze is opgegeven of gedetecteerd), controleert u de tekst op spelfouten met `CorrectSpelling()` en voert u eventuele correcties door. De gecorrigeerde tekst wordt teruggezet in het invoerveld, zodat de gebruiker weet dat de correctie is doorgevoerd. (De gebruiker kan een afbreekstreepje voor de te vertalen tekst zetten om aan te geven dat de spelling niet moet worden gecorrigeerd.)

Als een gebruiker geen tekst heeft ingevoerd, of als de 'naar'- en de 'van'-taal dezelfde zijn, hoeft de tekst niet te worden vertaald en hoeft er geen aanvraag te worden gedaan.

De code waarmee de aanvraag voor de vertaling wordt uitgevoerd, moet u bekend voorkomen: de URI bouwen, een aanvraag maken en verzenden, en het antwoord parseren. Verzend de tekst naar het besturingselement `TranslatedTextLabel` om de tekst weer te geven.

Geef de tekst vervolgens door aan de `Translate`-API in een geserialiseerde JSON-matrix in de hoofdtekst van een POST-aanvraag. De JSON-matrix kan meerdere stukken tekst bevatten om te vertalen, maar hier is er maar één nodig.

De HTTP-header met de naam `X-ClientTraceId` is optioneel. De waarde moet een GUID zijn. Een door de client geleverde traceer-id is handig voor het traceren van aanvragen wanneer iets niet werkt zoals verwacht. De waarde van X-ClientTraceID moet echter door de client worden geregistreerd. Anders hebt u er weinig aan. Met een traceer-id van de client en de datum van de aanvragen kan Microsoft eventuele problemen diagnosticeren.

> [!NOTE]
> Deze zelfstudie is gericht op de Microsoft Translator-service. Daarom worden de methoden `DetectLanguage()` en `CorrectSpelling()` niet in detail behandeld.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over de Translator Text-API van Microsoft](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
