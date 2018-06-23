---
title: Het schrijven van een Microsoft Translator WPF-app met C# en cognitieve Azure-Services | Microsoft Docs
description: Informatie over het gebruik van de tekst conversieprogramma service voor tekst vertalen, een gelokaliseerde lijst met ondersteunde talen en meer.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.devlang: csharp
ms.topic: article
ms.date: 10/25/2017
ms.author: v-jansko
ms.openlocfilehash: fb58fd087de09561a0ea930748562e595d3dde1c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344814"
---
# <a name="how-to-write-a-microsoft-translator-wpf-application-in-c"></a>Het schrijven van een toepassing Microsoft Translator WPF in C#

In deze zelfstudie hebt wij een interactieve tekst vertaling hulpprogramma met behulp van Microsoft Translator tekst API (V3), een onderdeel van Microsoft cognitieve Services in Azure bouwen. U leert hoe:

> [!div class="checklist"]
> * Een lijst met korte codes voor de talen die worden ondersteund door de service aanvragen
> * Een lijst met namen van de gelokaliseerde taal overeenkomt met deze taalcodes ophalen
> * De vertaalde versie van de gebruiker ingevoerde tekst naar een andere bemachtigen één taal

Deze toepassing biedt ook integratie met twee andere Microsoft cognitieve-Services.

|||
|-|-|
|[Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Gebruikt voor het detecteren van eventueel automatisch de taal van de bron van de tekst die moet worden omgezet|
|[Spellingcontrole van Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Voor Engels brontekst gebruikt voor het corrigeren van de spelling, zodat de vertaling nauwkeuriger is

![[De zelfstudie program uitgevoerd]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie doet, moet u een versie van Visual Studio-2017, waaronder de Community-versie.

U moet ook abonnement sleutels voor de drie Azure-services in het programma gebruikt. U kunt een sleutel voor de service conversieprogramma tekst ophalen vanuit het Azure-dashboard. Er is een gratis prijscategorie beschikbaar waarmee u maximaal twee miljoen tekens per maand gratis vertalen.

De Text Analytics en spellingcontrole van Bing aanbieding gratis proefversies, die u kunt zich aanmelden voor services [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/). U kunt ook een abonnement voor een van deze services via de Azure-dashboard maken. Tekstanalyse heeft een gratis laag.

De broncode voor deze zelfstudie vindt u hieronder. Uw abonnement sleutels moeten worden gekopieerd naar de broncode als de variabelen `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY`, enzovoort, in `MainWindow.xaml.cs`.

> [!IMPORTANT]
> De tekst Analytics-service is beschikbaar in meerdere regio's. De URI in onze zelfstudie broncode is in de `westus` regio, wat de regio is gratis proefversies gebruikt. Als u een abonnement in een andere regio hebt, worden deze URI dienovereenkomstig bijgewerkt.

## <a name="source-code"></a>Broncode

Dit is de broncode voor de tekst van Microsoft Translator API. Uitvoeren van de app door de broncode te kopiëren naar het juiste bestand in een nieuw WPF-project in Visual Studio.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Dit is de code-behind-bestand waarmee de functionaliteit van de toepassing.

```cs
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
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string
    /// one language to another. The langauges are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected.  English text is spell-checked.
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
            // at least show an error dialog when we get an unexpected error
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

            // read and and parse JSON response
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

Dit bestand definieert de gebruikersinterface voor de toepassing, een WPF-formulier. Als u uw eigen versie van het formulier ontwerpt wilt, moet u niet deze XAML.

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

De service Microsoft Translator heeft meerdere eindpunten die verschillende soorten vertaling functionaliteit bieden. De waarden die in deze zelfstudie we gebruiken zijn:

|||
|-|-|
|`Languages`|Retourneert de set van talen die momenteel door andere bewerkingen van de tekst conversieprogramma API ondersteund.|
|`Translate`|Brontekst gezien, retourneert een taalcode van gegevensbron en een doel taalcode een vertaling van de brontekst op de doel-taal.|

## <a name="the-translation-app"></a>De NAT-app

De gebruikersinterface van de toepassing van onze vertaler wordt gebouwd met behulp van Windows Presentation Foundation (WPF). Maak een nieuwe WPF-project in Visual Studio met de volgende stappen.

* Van de **bestand** menu kiezen **Nieuw > Project**.
* Open in het venster Nieuw Project **geïnstalleerde > Sjablonen > Visual C#**. Er wordt een lijst van de beschikbare sjablonen weergegeven in het midden van het dialoogvenster.
* Zorg ervoor dat **.NET Framework 4.5.2** in de vervolgkeuzelijst boven de lijst van de sjabloon project is gekozen.
* Klik op **WPF-App (.NET Framework)** in de lijst van de sjabloon project.
* Met behulp van de velden aan de onderkant van het dialoogvenster, het nieuwe project en de oplossing die deze bevat een naam.
* Klik op **OK** om het nieuwe project en de oplossing te maken.

![[Een nieuwe WPF-app maken in Visual Studio]](media/translator-text-csharp-new-project.png)

Verwijzingen naar de volgende .NET framework-assembly's toevoegen aan uw project.

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Installeer ook het NuGet-pakket `Newtonsoft.Json` in uw project.

Nu zoeken de `MainWindow.xaml` bestand in Solution Explorer en open het bestand. Het is in eerste instantie leeg. Dit is wat de gebruikersinterface van de voltooide moet eruitzien als, met de namen van de besturingselementen in blauw aantekeningen. Dezelfde namen voor de besturingselementen in de gebruikersinterface niet gebruiken omdat ze ook worden weergegeven in de code.

![[Aantekeningen weergave van het hoofdvenster in Visual Studio designer]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> De broncode voor deze zelfstudie bevat de XAML-bron voor dit formulier. U kunt deze plakken aan uw project in plaats van het bouwen van het formulier in Visual Studio.

* `FromLanguageComboBox` *(ComboBox)*  -Geeft een lijst met de talen die worden ondersteund door Microsoft Translator voor tekstvertaling. De gebruiker selecteert de taal die ze vertaalt uit.
* `ToLanguageComboBox` *(ComboBox)*  -Geeft dezelfde lijst met talen als `FromComboBox`, maar wordt gebruikt voor de taal van de gebruiker te vertalen selecteren.
* `TextToTranslate` *(Tekstvak)*  -De gebruiker de tekst die moet worden vertaald hier invoert.
* `TranslateButton` *(Knop)*  -De gebruiker klikt op deze knop (of op Enter drukt) om de tekst te vertalen.
* `TranslatedTextLabel` *(Label)*  -De omzetting van de tekst van de gebruiker hier weergegeven.

Als u uw eigen versie van dit formulier, hoeft u geen zodat het *exact* zoals onze. Maar zorg ervoor dat de taal-keuzelijsten zijn breed genoeg is om te voorkomen dat de naam van een taal wordt afgekapt.

## <a name="the-mainwindow-class"></a>De klasse MainWindow

Het bestand code-behind `MainWindow.xaml.cs` is waar komt de code op die manier kunt doen wat het doet. Het werk gebeurt op twee keer:

* Als het programma wordt gestart. Wanneer `MainWindow` is gestart, we de lijst van de taal met behulp van conversieprogramma ophalen `GetLanguagesForTranslate` en `GetLanguageNames` API's en onze vervolgkeuzelijsten met hen te vullen. Deze taak wordt slechts één keer gedaan aan het begin van elke sessie.

* Wanneer de gebruiker klikt op de **vertalen** knop we ophalen taalselectie voor de gebruiker en de tekst die ze ingevoerd. Vervolgens we Roep de `Translate` API's voor het uitvoeren van de vertaling. We kunnen ook andere functies die de taal van de tekst te bepalen en corrigeer de spelling voordat de vertaling van aanroepen.

Bekijk hoe we onze klasse beginnen:

```cs
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
        // at least show an error dialog when we get an unexpected error
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

Twee variabelen gedeclareerd houdt hier informatie over onze talen:

|||
|-|-|
|`languageCodes`<br>matrix van tekenreeks|Slaat de taalcodes. De NAT-service gebruikt korte codes, zoals `en` voor Engels talen te identificeren.|
|`languageCodesAndTitles`<br>SortedDictionary|Maps 'beschrijvende' namen in de gebruikersinterface terug naar de korte codes in de API wordt gebruikt. Opgeslagen gesorteerde alfabetisch zonder te letten op aanvraag.|

De eerste code uitgevoerd door de toepassing is de `MainWindow` constructor. Eerst we een methode instellen `HandleExceptions` als de globale foutafhandeling. Op deze manier ten minste krijgen we een foutmelding als een uitzondering is niet verwerkt.

Vervolgens controleren we om ervoor te zorgen dat de API-abonnement sleutels alle exact 32 tekens lang zijn. Als ze niet, is de meest waarschijnlijke reden *iemand* geplakt nog niet in de API-sleutels (tsk). In dit geval wordt een foutbericht weergegeven en oplossen uit. (Deze test wordt doorgegeven betekent niet dat de sleutels zijn geldig is, natuurlijk.)

Als we sleutels die ten minste de juiste lengte hebben de `InitializeComponent()` aanroep van de gebruikersinterface te zoeken, laden en de XAML-beschrijving van het venster hoofdtoepassing instantiëren rolling opgehaald.

Ten slotte wordt de vervolgkeuzelijsten taal hebt ingesteld. Deze taak vereist drie afzonderlijke methodeaanroepen. We gaan via deze methoden in de volgende secties in detail.

## <a name="get-supported-languages"></a>Ondersteunde talen niet ophalen

De service Microsoft Translator ondersteunt een totaal van op deze schrijven 61 talen en meer kan van tijd tot tijd kan worden toegevoegd. Daarom is het handig niet op harde code de ondersteunde talen in uw programma. Vraag de NAT-service in plaats daarvan welke talen wordt ondersteund. Elke ondersteunde taal kan worden omgezet in een andere ondersteunde taal.

Roep de `Languages` API voor de lijst met ondersteunde talen.

De `Languages` API duurt een optionele parameter in de query GET, *bereik*. *bereik* kan een van drie waarden hebben: `translation`, `transliteration`, en `dictionary`. We gebruiken de waarde `translation`.

De `Languages` API neemt ook een optionele HTTP-header `Accept-Language`. De waarde van deze header bepaalt de taal waarin de namen van de ondersteunde talen worden geretourneerd. De waarde moet een goed ingedeelde BCP 47 taallabel. We gebruiken de waarde `en` ophalen van de taalnamen van de in het Engels.

De `Languages` API retourneert een JSON-antwoord dat op het volgende lijkt.

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

We willen halen de taalcodes (bijvoorbeeld `af`) en de taalnamen van de (bijvoorbeeld `Afrikaans`). We gebruiken de methode NewtonSoft.Json [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm) om dit te doen.

Met deze achtergrondkennis maken we de volgende methode voor het ophalen van de taalcodes en hun namen.

```cs
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

`GetLanguagesForTranslate()` eerst maakt de HTTP-aanvraag. De `scope=translation` query tekenreeksparameter geeft aan dat we de talen die worden ondersteund voor de tekstvertaling van. We toevoegen de sleutel tekst vertaling API-abonnement aan onze aanvraagheaders. We ook toevoegen de `Accept-Language` header met de waarde `en` om aan te geven we de ondersteunde talen geretourneerd in het Engels.

Nadat de aanvraag is voltooid, we parseren van het JSON-antwoord en deze omzetten in een woordenlijst. We de taalcodes toevoegen de `languageCodes` lidvariabele. Vervolgens doorlopen we de sleutel/waarde-paren die de taalcodes en de van de beschrijvende taalnamen bevatten en deze toevoegen aan de `languageCodesAndTitles` lidvariabele. (De vervolgkeuzelijsten in onze formulier weer de beschrijvende namen, maar moeten we de codes om aan te vragen van de vertaling.)

## <a name="populate-the-language-menus"></a>Vullen van de taal van menu 's

De meeste van de gebruikersinterface is gedefinieerd in XAML, zodat we niet hoeven te veel om in te stellen naast aanroep doen `InitializeComponent()`. Het alleen andere wat we moeten doen is de van de beschrijvende taalnamen naar de en toevoegen van vervolgkeuzelijsten, die wordt uitgevoerd in `PopulateLanguageMenus()`.

```cs
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

Vullen van de menu's is een eenvoudige kwestie van iteratie van de `languageCodesAndTitles` woordenlijst en elke sleutel is de naam van de 'beschrijvende', toe te voegen aan beide menu's. Na het vullen van de menu's, wordt de standaardwaarde instellen naar en van de talen te detecteren (voor de taal automatische detectie) en Engels.

> [!TIP]
> Als we een standaardselectie niet ingesteld voor onze menu's, de gebruiker kan klikken **vertalen** zonder een te kiezen of van taal. De standaardinstellingen hoeft te bekommeren om dit probleem.

Nu `MainWindow` is geïnitialiseerd, maakt de gebruikersinterface. We niet opnieuw besturingselement ophalen totdat de gebruiker klikt op de **vertalen** knop.

## <a name="perform-translation"></a>Vertaling uitvoeren

Wanneer de gebruiker klikt op **vertalen**, WPF roept de `TranslateButton_Click()` gebeurtenis-handler die hier worden weergegeven.

```cs
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

Hier het eerst worden opgehaald en talen, samen met de tekst die de gebruiker heeft ingevoerd, vanuit het formulier.

Als de bron-taal is ingesteld om te detecteren, noemen we `DetectLanguage()` de taal van de tekst te bepalen. De tekst kan zich in een taal die niet van de NAT-API's ondersteuning (vele andere talen kunnen worden gevonden dan kan worden omgezet) of de tekst Analytics API mogelijk niet kunnen worden gedetecteerd. In dat geval wordt een bericht gemeld en retourneren zonder de te vertalen weergegeven.

Als de taal van de bron is Engels (opgegeven of gedetecteerd), we spellingcontrole de tekst met `CorrectSpelling()` en correcties toepassen. De gecorrigeerde tekst is gevuld terug naar het veld invoer zodat de gebruiker weet dat de correcties is gedaan. (De gebruiker kan worden voorafgegaan door de tekst wordt geconverteerd met een afbreekstreepje correctie onderdrukken.)

Als de gebruiker geen tekst is opgegeven, of als de regel aan en talen zijn hetzelfde, geen vertaling nodig is. In dit geval voorkomen we dat de aanvraag.

De code voor het uitvoeren van de aanvraag vertaling ziet er waarschijnlijk vertrouwd. We de URI bouwen, maakt u een aanvraag verzenden en parseren van het antwoord. Als u wilt de tekst wordt weergegeven, opgeslagen in de `TranslatedTextLabel` besturingselement.

Geven we tekst naar de `Translate` API in een geserialiseerde JSON-matrix in de hoofdtekst van een POST-aanvraag. De JSON-matrix meerdere tekst om te vertalen kan bevatten, maar hier we zojuist een opnemen.

De HTTP-header met de naam `X-ClientTraceId` is optioneel. De waarde moet een GUID zijn. De client opgegeven traceer-ID is handig voor trace-aanvragen wanneer dingen niet werken zoals verwacht. Niet handig, moet de waarde van de X-ClientTraceID worden vastgelegd door de client. Een client traceer-ID en de datum van de aanvragen kunt Microsoft vaststellen van problemen die zich kunnen voordoen.

> [!NOTE]
> Deze zelfstudie richt zich op de service Microsoft Translator, dus niet wordt behandeld de `DetectLanguage()` en `CorrectSpelling()` methoden in detail. De Text Analytics en spellingcontrole van Bing-services bieden antwoorden in JSON in plaats van XML- en Tekstanalyse vereist dat de aanvraag worden opgemaakt als JSON ook. Deze kenmerken zijn verantwoordelijk voor de meeste code verschillen van de methoden die we al hebt gezien.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Microsoft Translator tekst API-referentiemateriaal](http://docs.microsofttranslator.com/text-translate.html)
