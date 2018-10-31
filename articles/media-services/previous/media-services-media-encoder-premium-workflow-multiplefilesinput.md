---
title: Meerdere invoerbestanden en eigenschappen van onderdeel met Premium Encoder - Azure | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe u setRuntimeProperties gebruiken om te gebruiken van meerdere invoerbestanden en aangepaste gegevens doorgeven aan de processor Media Encoder Premium Workflow-media.
services: media-services
documentationcenter: ''
author: xpouyat
manager: femila
editor: ''
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: 6196e2802f31fd5d06418cb722ae628dbf01c8c1
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251002"
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Gebruik van meerdere invoerbestanden en eigenschappen van onderdeel met Premium Encoder
## <a name="overview"></a>Overzicht
Er zijn scenario's waarin u mogelijk om aan te passen van de eigenschappen van onderdeel Clip lijst XML-inhoud opgeven, of meerdere invoerbestanden verzenden wanneer u een taak met verzendt de **Media Encoder Premium Workflow** Mediaprocessor. Een aantal voorbeelden:

* Bedrijfscontext tekst op video en het instellen van de tekstwaarde (bijvoorbeeld, de huidige datum) tijdens runtime voor elke invoervideo.
* Aanpassing van de XML-lijst Clip (om op te geven een of meer bronbestanden bevat, met of zonder opmaak, enz.).
* Bedrijfscontext op de video-invoer een logoafbeelding terwijl de video is gecodeerd.
* Meerdere codering van gesproken taal.

Om te laten de **Media Encoder Premium Workflow** weten dat u bepaalde eigenschappen in de werkstroom wijzigen wanneer u de taak maken of meerdere invoerbestanden verzenden, hebt u een configuratietekenreeks met  **setRuntimeProperties** en/of **transcodeSource**. In dit onderwerp wordt uitgelegd hoe ze te gebruiken.

## <a name="configuration-string-syntax"></a>De syntaxis van configuratie
De configuratietekenreeks om in te stellen in de coderingstaak maakt gebruik van een XML-document er als volgt uitzien:

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

Hieronder volgt de C# code die de XML-configuratie van een bestand wordt gelezen bijwerken met de juiste video bestandsnaam en geeft deze door naar de taak in een taak:

```csharp
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass to it the name of the 
// processor to use for the specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with the encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify the input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset to contain the results of the job. 
// This output is specified as AssetCreationOptions.None, which 
// means the output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a>Eigenschappen van onderdeel aanpassen
### <a name="property-with-a-simple-value"></a>Eigenschap met een eenvoudige waarde
In sommige gevallen is het handig om aan te passen van de eigenschap van een component, samen met het bestand van de werkstroom die u wilt met Media Encoder Premium Workflow worden uitgevoerd.

Stel dat u een werkstroom die tekst overlays op opgezet uw video's en de tekst (bijvoorbeeld, de huidige datum) moet worden ingesteld tijdens runtime. U kunt dit doen door de tekst die moet worden ingesteld als de nieuwe waarde voor de eigenschap text van het onderdeel overlay van de coderingstaak verzenden. U kunt dit mechanisme gebruiken om andere eigenschappen van een onderdeel in de werkstroom (zoals de positie of de kleur van de overlay, de bitrate van het coderingsprogramma AVC, enz.) te wijzigen.

**setRuntimeProperties** wordt gebruikt voor het overschrijven van een eigenschap in de onderdelen van de werkstroom.

Voorbeeld:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a>Eigenschap met een XML-waarde
Een eigenschap die wordt verwacht een XML-waarde dat wilt instellen, verpakken met behulp van `<![CDATA[ and ]]>`.

Voorbeeld:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> Zorg ervoor dat geen te plaatsen direct na de afzender van een regelterugloop `<![CDATA[`.

### <a name="propertypath-value"></a>propertyPath-waarde
In de vorige voorbeelden wordt de propertyPath is ' / Media File invoer/filename ' of ' / inactiveTimeout ' of 'clipListXml'.
Dit kan in het algemeen is de naam van het onderdeel, gevolgd door de naam van de eigenschap. Het pad kunt hebben meer of minder niveaus, zoals ' / primarySourceFile ' (omdat de eigenschap in de hoofdmap van de werkstroom is) of ' / Video verwerking/afbeelding Overlay/dekking ' (omdat de Overlay zich in een groep).    

Om te controleren of de naam van het pad en de eigenschap, gebruik de actieknop die direct naast elke eigenschap. U kunt op deze actieknop en selecteer **bewerken**. Hier ziet u de werkelijke naam van de eigenschap en onmiddellijk erboven, de naamruimte.

![Actie/bewerken](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Eigenschap](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Meerdere invoerbestanden
Elke taak die u naar verzendt de **Media Encoder Premium Workflow** vereist twee elementen:

* De eerste is een *werkstroom Asset* die een werkstroombestand bevat. U kunt Werkstroombestanden ontwerpen met behulp van de [Workflow Designer](media-services-workflow-designer.md).
* De tweede waarde is een *Media-Asset* die de media-bestanden die u wilt coderen bevat.

Wanneer u meerdere mediabestanden verzendt de **Media Encoder Premium Workflow** encoder, de volgende beperkingen zijn van toepassing:

* Alle mediabestanden moeten zich in dezelfde *Media-Asset*. Met behulp van meerdere Media-activa wordt niet ondersteund.
* U moet het primaire bestand in deze Media-Asset instellen (in het ideale geval, dit is het belangrijkste videobestand dat het coderingsprogramma is gevraagd om u te verwerken).
* Het is nodig om door te geven van de configuratiegegevens met de **setRuntimeProperties** en/of **transcodeSource** element aan de processor.
  * **setRuntimeProperties** wordt gebruikt voor het overschrijven van de eigenschap filename of een andere eigenschap in de onderdelen van de werkstroom.
  * **transcodeSource** wordt gebruikt om op te geven van de Clip lijst XML-inhoud.

Verbindingen in de werkstroom:

* Als u een of meer onderdelen van Media-bestandsinvoer gebruikt en wilt gebruiken **setRuntimeProperties** als u wilt de bestandsnaam opgeven, klikt u vervolgens geen verbinding maken het primaire bestand onderdeel pincode toe. Zorg ervoor dat er geen verbinding tussen het primaire bestand-object en het bestand Media invoerwaarde(n).
* Als u liever Clip lijst XML en één Media brononderdeel, kunt klikt u vervolgens u beide samen.

![Er is geen verbinding van primaire bronbestand naar Media-bestandsinvoer](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Er is geen verbinding van het primaire bestand met Media-bestandsinvoer onderdelen als u setRuntimeProperties gebruiken om in te stellen de eigenschap filename.*

![Verbinding van een videoclip lijst XML in de lijst met brongegevens Clip](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*U kunt verbinding maken met XML-lijst van een videoclip mediabron en transcodeSource gebruiken.*

### <a name="clip-list-xml-customization"></a>Illustratie van de lijst met XML-aanpassing
U kunt de lijst met Clip XML in de werkstroom tijdens runtime opgeven met behulp van **transcodeSource** XML in de configuratie van de tekenreeks. Hiervoor moet de pincode van de XML-lijst van een videoclip te verbinden met de Media-Source-component in de werkstroom.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Als u opgeven /primarySourceFile wilt voor het gebruik van deze eigenschap om de naam van de uitvoerbestanden met behulp van 'Expressies', wordt aangeraden de Clip lijst XML wordt doorgegeven als een eigenschap *nadat* de eigenschap /primarySourceFile, om te voorkomen dat de Clip Lijst met worden genegeerd door de instelling /primarySourceFile.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Met extra nauwkeurige frame opmaak:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-the-video"></a>Voorbeeld 1: Een afbeelding die boven op de video Overlay

### <a name="presentation"></a>Presentatie
Bekijk een voorbeeld waarin u een logo-installatiekopie op de video-invoer overlay wilt terwijl de video is gecodeerd. In dit voorbeeld wordt de invoervideo heet 'Microsoft_HoloLens_Possibilities_816p24.mp4' en het logo met de naam 'logo.png'. U moet de volgende stappen uitvoeren:

* Een werkstroom-Asset maken met de werkstroombestand (Zie het volgende voorbeeld).
* Maak een Media-activa, die twee bestanden bevat: MyInputVideo.mp4 als het primaire bestand en MyLogo.png.
* Een taak verzenden naar de processor Media Encoder Premium Workflow media met de bovenstaande invoer assets en geeft u de volgende configuratietekenreeks.

Configuratie:

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

In het bovenstaande voorbeeld is de naam van het videobestand verzonden naar het onderdeel Media bestanden invoeren en de eigenschap primarySourceFile. De naam van het logobestand wordt verzonden naar een andere invoer van de Media-bestand die is verbonden met het onderdeel grafische overlay.

> [!NOTE]
> De naam van het videobestand wordt verzonden naar de eigenschap primarySourceFile. De reden hiervoor is het gebruik van deze eigenschap in de werkstroom voor het bouwen van de naam van het juiste uitvoerbestand met behulp van expressies, bijvoorbeeld.

### <a name="step-by-step-workflow-creation"></a>Stapsgewijze werkstroom is gemaakt
Hier volgen de stappen voor het maken van een werkstroom die wordt gebruikt twee bestanden als invoer: een video en een afbeelding. Dit wordt de afbeelding die boven op de video overlay.

Open **Workflow Designer** en selecteer **bestand** > **nieuwe werkruimte** > **transcoderen blauwdruk**.

De nieuwe werkstroom ziet u drie elementen:

* Primaire bronbestand
* Illustratie van de XML-lijst
* Uitvoerasset bestand  

![Nieuwe coderingsworkflow nauwkeurig af](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nieuwe coderingsworkflow nauwkeurig af*

Begin met het toevoegen van een Media-bestand invoer onderdeel om te kunnen accepteren van de invoer mediabestand. Een onderdeel toevoegen aan de werkstroom, zoek in het zoekvak van de opslagplaats en de gewenste post naar het ontwerpoppervlak deelvenster slepen.

Voeg vervolgens het videobestand moet worden gebruikt voor het ontwerpen van uw werkstroom. Om dit te doen, klikt u op de achtergrond deelvenster in de ontwerpfunctie voor werkstromen en zoek naar de primaire bronbestand-eigenschap in het deelvenster met de rechter-eigenschap. Klik op het mappictogram en selecteer het juiste videobestand.

![Primaire bestandsbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primaire bestandsbron*

Geef vervolgens het videobestand in de component bestandsinvoer Media.   

![Mediabestand invoerbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Mediabestand invoerbron*

Zodra dit is gebeurd, wordt het onderdeel Media-bestandsinvoer inspecteren van het bestand en vullen van de uitvoer pincodes aanleiding van het bestand dat wordt gecontroleerd.

De volgende stap is het toevoegen van een 'Video Data Type Updater' als u wilt de kleurruimte Rec.709 opgeven. Toevoegen van een 'Video-indelingconverter' die is ingesteld op gegevensindeling lay-out type = vlak kunnen worden geconfigureerd. Hiermee wordt de videostream converteren naar een indeling die kan worden uitgevoerd als een bron van het onderdeel overlay.

![video gegevens Type Updater- en Format Converter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video-Data Type Updater en indeling*

![Indelingstype = configureerbare vlak](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Indelingstype is configureerbaar vlak*

Vervolgens een Video-Overlay-component toevoegen en verbinding maken met de (niet-gecomprimeerde) video pincode voor de (niet-gecomprimeerde) video pincode van de invoer van media-bestand.

Toevoegen van een andere invoer door Media-bestand (voor het laden van het logobestand), klikt u op dit onderdeel en wijzig de naam 'Media bestand invoer logo' en selecteert u een installatiekopie (bijvoorbeeld een PNG-bestand) in de bestandseigenschap. De pincode niet-gecomprimeerde installatiekopie verbinden met de pincode niet-gecomprimeerde installatiekopie van de overlay.

![Overlay component- en afbeeldingsbestanden bestandsbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Overlay component- en afbeeldingsbestanden bestandsbron*

Als u wilt wijzigen van de positie van het logo in de video (bijvoorbeeld, u mogelijk wilt plaatsen op 10 procent korting op de linkerbovenhoek van de video), schakel het selectievakje 'Handmatige invoer'. U kunt dit doen omdat u een Media-bestandsinvoer het logobestand naar de overlay component op te geven.

![Positie van de overlay](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Positie van de overlay*

Toevoegen als u wilt de videostream te H.264 coderen, de encoder AVC Video Encoder en AAC onderdelen naar het ontwerpoppervlak. Verbinding maken met de pincodes.
Instellen van het coderingsprogramma AAC en Audio-indeling converteren/voorinstelling selecteren: 2.0 (L, R).

![Audio en Video coderingsprogramma 's](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio en Video coderingsprogramma 's*

Voeg nu de **ISO Mpeg-4 Multiplexer** en **uitvoer in een bestand** onderdelen en sluit de pincodes, zoals wordt weergegeven.

![MP4 multiplexer en uitvoer in een bestand](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexer en uitvoer in een bestand*

U moet de naam van het uitvoerbestand instellen. Klik op de **uitvoer in een bestand** onderdeel en de expressie voor het bestand bewerken:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Naam van bestand voor uitvoer](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Naam van bestand voor uitvoer*

De werkstroom lokaal om te controleren of deze correct wordt uitgevoerd, kunt u uitvoeren.

Nadat deze is voltooid, kunt u deze uitvoeren in Azure Media Services.

Bereid eerst een activum in Azure Media Services met twee bestanden: het videobestand en het logo. U kunt dit doen met behulp van .NET of REST-API. U kunt dit ook doen met behulp van Azure portal of [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Deze zelfstudie leert u hoe u voor het beheren van activa met AMSE. Er zijn twee manieren bestanden toevoegen aan een asset:

* Maken van een lokale map, het kopiëren van de twee bestanden, en het slepen en neerzetten van de map naar de **Asset** tabblad.
* De video uploaden als een asset, de activa-informatie wordt weergegeven, gaat u naar het tabblad bestanden en uploaden van een extra bestand (logo).

> [!NOTE]
> Zorg ervoor dat het instellen van een primair bestand in de asset (de belangrijkste videobestand).

![Assetbestanden in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Assetbestanden in AMSE*

Selecteer de asset en kies het coderen met Premium Encoder. Uploaden van de werkstroom en selecteer deze.

Klik op de knop gegevens doorgeven aan de processor en het volgende XML-bestand om in te stellen van de runtime-eigenschappen toe:

![Premium Encoder in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium Encoder in AMSE*

Plak de volgende XML-gegevens. U moet de naam van het videobestand opgeven voor de Media-bestandsinvoer en primarySourceFile. Geef de naam van de bestandsnaam voor het logo te.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*

Als u de .NET SDK gebruiken om te maken en de taak wordt uitgevoerd, heeft dit XML-gegevens moeten worden doorgegeven aan de configuratietekenreeks.

```csharp
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

Nadat de taak voltooid is, wordt het MP4-bestand in de uitvoer de overlay!

![Op de video overlay](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Op de video overlay*

U kunt de voorbeeldwerkstroom van downloaden [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>Voorbeeld 2: Meerdere audio taalcode

Een voorbeeld van meerdere codering workfkow beschikbaar in de is taal [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Deze map bevat een voorbeeldwerkstroom die kan worden gebruikt om een MXF-bestand naar een asset multi MP4-bestanden met meerdere audionummers coderen.

Deze werkstroom wordt ervan uitgegaan dat het MXF-bestand één audiotrack bevat; de aanvullende audionummers moeten worden doorgegeven als afzonderlijke audio-bestanden (WAV of MP4...).

Als u wilt coderen, volgt u deze stappen:

* Maak een Media Services-asset met het bestand MXF- en de Audio-bestanden (0 tot en met 18 audio).
* Zorg ervoor dat het bestand MXF is ingesteld als primaire-bestand.
* Maak een taak en een taak met behulp van de processor Premium Workflow Encoder. De opgegeven workflow (MultiMP4-1080p-19audio-v1.workflow) gebruiken.
* De setruntime.xml gegevens doorgeven aan de taak (als u Azure Media Services Explorer gebruikt, gebruik de knop 'XML-gegevens doorgeven aan de werkstroom').
  * Werk de XML-gegevens om op te geven van de juiste namen en talen bestandslabels.
  * De werkstroom heeft audio onderdelen met de naam Audio 1 tot en met 18 Audio.
  * RFC5646 wordt ondersteund voor de taalcode.

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* De gecodeerde asset bevat meerdere taal audionummers en deze nummers moet worden geselecteerd in Azure Media Player.

## <a name="see-also"></a>Zie ook
* [Introductie van Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Het gebruik van Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [De inhoud op aanvraag met Azure Media Services Encoding](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Media Encoder Premium Workflow-indelingen en codecs voor](media-services-premium-workflow-encoder-formats.md)
* [Werkstroom voorbeeldbestanden](https://github.com/Azure/azure-media-services-samples)
* [Azure Media Services Explorer-hulpmiddel](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
