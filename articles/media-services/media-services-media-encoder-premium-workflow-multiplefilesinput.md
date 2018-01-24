---
title: Meerdere invoerbestanden en eigenschappen van onderdeel met Premium-Encoder - Azure | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe setRuntimeProperties gebruiken om te gebruiken van meerdere invoerbestanden en aangepaste gegevens aan de Media Encoder Premium media werkstroomverwerking doorgeven.
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: a55d6dc212da05d7c14679579258e28921fecdc8
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Gebruik van meerdere invoerbestanden en eigenschappen van onderdeel met Premium-codering
## <a name="overview"></a>Overzicht
Er zijn scenario's waarin u moet voor het aanpassen van eigenschappen van onderdeel Clip lijst XML-inhoud opgeven of meerdere invoerbestanden wilt verzenden als u een taak met dient de **Media Encoder Premium werkstroom** Mediaprocessor. Een aantal voorbeelden:

* Keer tekst over video en het instellen van de tekstwaarde (bijvoorbeeld, de huidige datum) tijdens runtime voor elke invoervideo.
* Aanpassing van de XML-lijst Clip (om op te geven een of meer bronbestanden met of zonder opmaak, enz.).
* Keer een installatiekopie van het logo van de video invoer terwijl de video is gecodeerd.
* Meerdere audio taalcode.

Om te laten de **Media Encoder Premium werkstroom** weten dat u enkele eigenschappen in de werkstroom wijzigt wanneer u de taak maakt of meerdere invoerbestanden verzenden, hebt u een configuratietekenreeks met  **setRuntimeProperties** en/of **transcodeSource**. Dit onderwerp wordt uitgelegd hoe u deze gebruikt.

## <a name="configuration-string-syntax"></a>De syntaxis van configuratie
De configuratietekenreeks instellen in de codering taak maakt gebruik van een XML-document ziet er als volgt:

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

Hier volgt de C#-code die de XML-configuratie uit een bestand lezen, bijwerken met de juiste video bestandsnaam en wordt doorgegeven aan de taak in een taak:

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
In sommige gevallen is het handig voor het aanpassen van de eigenschap van een onderdeel samen met het bestand van de werkstroom die door Media Encoder Premium werkstroom moet worden uitgevoerd.

Stel dat u een werkstroom die tekst overlays op opgezet uw video's en de tekst (bijvoorbeeld, de huidige datum) moet worden ingesteld tijdens runtime. U kunt dit doen door de tekst die moet worden ingesteld als de nieuwe waarde voor de eigenschap text van de overlay-onderdeel van de taak codering verzenden. U kunt dit mechanisme gebruiken om andere eigenschappen van een onderdeel in de werkstroom (zoals de positie of kleur van de overlay, de bitrate van AVC-codering, enz.) te wijzigen.

**setRuntimeProperties** wordt gebruikt voor het onderdrukken van een eigenschap in de onderdelen van de werkstroom.

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
Een eigenschap die een XML-waarde verwacht wilt instellen, inkapselen met behulp van `<![CDATA[ and ]]>`.

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
> Zorg ervoor dat u niet een vervoer net na return plaatsen `<![CDATA[`.

### <a name="propertypath-value"></a>propertyPath-waarde
In de voorgaande voorbeelden de propertyPath is ' / Media File invoer/bestandsnaam ' of ' / inactiveTimeout ' of 'clipListXml'.
Dit kan in het algemeen is de naam van het onderdeel en vervolgens de naam van de eigenschap. Het pad kan meer of minder niveaus, zoals hebben ' / primarySourceFile ' (omdat de eigenschap in de hoofdmap van de werkstroom) of ' / Video-Overlay verwerking/afbeelding/dekking ' (omdat de Overlay bevindt zich in een groep).    

Om te zien van het pad en de eigenschap name, gebruik de knop voor de actie die wordt onmiddellijk naast elke eigenschap. U kunt op deze actieknop en selecteer **bewerken**. Hier ziet u de werkelijke naam van de eigenschap en onmiddellijk boven, de naamruimte.

![Actie/bewerken](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Eigenschap](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Meerdere invoerbestanden
Elke taak die u aanvragen indienen bij de **Media Encoder Premium werkstroom** vereist twee elementen:

* De eerste is een *werkstroom Asset* die een werkstroombestand bevat. U kunt Werkstroombestanden ontwerpen met behulp van de [Workflow Designer](media-services-workflow-designer.md).
* Het tweede is een *Media Asset* die de media-bestanden die u wilt coderen bevat.

Wanneer u media-bestanden worden verzendt de **Media Encoder Premium werkstroom** encoder, de volgende beperkingen toepassen:

* Alle mediabestanden moeten zich in dezelfde *Media Asset*. Gebruik van meerdere elementen van de Media wordt niet ondersteund.
* U moet het primaire bestand ingesteld in dit activum Media (in het ideale geval dit is het belangrijkste videobestand dat het coderingsprogramma is gevraagd om te verwerken).
* Het is nodig om door te geven van configuratiegegevens met de **setRuntimeProperties** en/of **transcodeSource** element op de processor.
  * **setRuntimeProperties** wordt gebruikt voor het onderdrukken van de eigenschap filename of een andere eigenschap in de onderdelen van de werkstroom.
  * **transcodeSource** wordt gebruikt om de Clip lijst XML-inhoud te geven.

De verbindingen in de werkstroom:

* Als u een of meer onderdelen van Media bestandsinvoer gebruiken en wilt gebruiken **setRuntimeProperties** voor de bestandsnaam opgeven, klikt u vervolgens geen verbinding maken het primaire bestand onderdeel pincode toe. Zorg ervoor dat er geen verbinding tussen het primaire bestand-object en de Input(s) Media-bestand is.
* Als u liever Clip lijst XML en een mediabron onderdeel, kunt klikt u vervolgens u beide samen.

![Er is geen verbinding van primaire bronbestand op invoer van de Media-bestand](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Er is geen verbinding van het primaire bestand voor invoer van Media-bestand onderdelen als u setRuntimeProperties gebruikt voor het instellen van de eigenschap filename.*

![Verbinding van Clip lijst XML aan de lijst met brongegevens bijsnijden](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*U kunt verbinding maken met XML-Clip lijst mediabron en transcodeSource gebruiken.*

### <a name="clip-list-xml-customization"></a>Lijst met XML-aanpassing bijsnijden
U kunt de lijst Clip XML in de werkstroom tijdens runtime opgeven met behulp van **transcodeSource** string XML in de configuratie. Hiervoor moet de pincode Clip lijst XML zijn verbonden met het brononderdeel van Media in de werkstroom.

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

Als u opgeven /primarySourceFile wilt voor het gebruik van deze eigenschap om de uitvoerbestanden met behulp van 'Expressies', wordt aangeraden om de lijst Clip XML wordt doorgegeven als een eigenschap *nadat* de eigenschap /primarySourceFile om te voorkomen dat de Clip Lijst worden overschreven door de instelling /primarySourceFile.

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
Bekijk een voorbeeld waarin u een logo-installatiekopie op de video invoer overlay wilt terwijl de video is gecodeerd. In dit voorbeeld heeft de naam van de invoer video 'Microsoft_HoloLens_Possibilities_816p24.mp4' en het logo heet 'logo.png'. U moet de volgende stappen uitvoeren:

* Een werkstroom-Asset maken met de werkstroombestand (Zie het volgende voorbeeld).
* Maken van een activum Media, waarin twee bestanden: MyInputVideo.mp4 als het primaire bestand en MyLogo.png.
* Een taak verzenden naar de processor Media Encoder Premium werkstroom media met de bovenstaande invoer bedrijfsmiddelen en geef de volgende configuratietekenreeks.

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

In het bovenstaande voorbeeld is de naam van het bestand naar de Media bestandsinvoer-onderdeel en de eigenschap primarySourceFile verzonden. De naam van de logobestand wordt verzonden naar een andere invoer van de Media-bestand dat is verbonden met het onderdeel grafische overlay.

> [!NOTE]
> De video bestandsnaam wordt verzonden naar de eigenschap primarySourceFile. De reden hiervoor is het gebruik van deze eigenschap in de werkstroom voor het bouwen van de naam van het juiste uitvoerbestand met behulp van expressies, bijvoorbeeld.

### <a name="step-by-step-workflow-creation"></a>Stapsgewijze werkstroom is gemaakt
Hier volgen de stappen voor het maken van een werkstroom die twee bestanden als invoer neemt: een video- en een installatiekopie. Dit wordt de afbeelding die boven op de video overlay.

Open **Workflow Designer** en selecteer **bestand** > **nieuwe werkruimte** > **transcoderen blauwdruk**.

De nieuwe werkstroom ziet u drie elementen:

* Primaire bronbestand
* Clip XML-lijst
* Uitvoerasset bestand  

![Nieuwe codering werkstroom](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nieuwe codering werkstroom*

Begin met het toevoegen van een Media bestand invoer onderdeel om het mediabestand voor invoer accepteert. Een onderdeel toevoegen aan de werkstroom, zoekt in het zoekvak opslagplaats en sleep de gewenste vermelding naar het deelvenster ontwerpen.

Voeg vervolgens het videobestand moet worden gebruikt voor het ontwerpen van uw werkstroom. Klik in het deelvenster achtergrond in de werkstroomontwerper en zoekt u naar de primaire bronbestand-eigenschap in het deelvenster met de rechter eigenschap om dit te doen. Klik op het mappictogram en selecteer het juiste videobestand.

![Primaire bestandsbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primaire bestandsbron*

Geef het videobestand vervolgens in het onderdeel Media bestandsinvoer.   

![Mediabestand invoerbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Mediabestand invoerbron*

Zodra dit is gebeurd, wordt het onderdeel Media bestandsinvoer inspecteren van het bestand en bijbehorende pincodes uitvoer naar aanleiding van het bestand dat het geïnspecteerd vullen.

De volgende stap is het toevoegen van een 'Video gegevens Type Updater' als u wilt de kleurruimte Rec.709 opgeven. Toevoegen van een 'Video Format Converter' die is ingesteld op gegevensindeling indeling type = configureerbare platte. Hiermee wordt de videostream converteren naar een indeling die kan worden uitgevoerd als een bron van het onderdeel overlay.

![video gegevens Type Updater en Format Converter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video-Data Type Updater en Format Converter*

![Indelingstype configureerbare platte =](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Indelingstype is configureerbaar platte*

Vervolgens wordt een Video-Overlay-component toevoegen en de (niet-gecomprimeerde) video pincode verbinding met de (niet-gecomprimeerde) video pincode van de invoer van de media-bestand.

Toevoegen van een andere invoer door Media-bestand (voor het laden van het logobestand), klikt u op dit onderdeel en wijzig de naam 'Media bestand invoer logo' en een installatiekopie (bijvoorbeeld een PNG-bestand) in de bestandseigenschap selecteren. Verbinding maken met de pincode niet-gecomprimeerde installatiekopie van de pincode niet-gecomprimeerde installatiekopie van de overlay.

![Bron voor overlay onderdeel en de installatiekopie van bestand](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Bron voor overlay onderdeel en de installatiekopie van bestand*

Als u wilt wijzigen van de positie van het logo in de video (bijvoorbeeld, u mogelijk wilt plaatsen op 10 procent van de linkerbovenhoek van de video), schakel het selectievakje 'Handmatige invoer'. U kunt dit doen omdat u een Media-bestand voor invoer gebruikt om de logobestand naar de overlay-component.

![Positie van de overlay](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Positie van de overlay*

Toevoegen als u wilt de videostream naar H.264 coderen, de onderdelen van de encoder AVC Video Encoder en AAC naar het ontwerpoppervlak. Verbinding maken met de pincodes.
Instellen van het coderingsprogramma AAC en selecteer Audio indeling conversie/definitie: 2.0 (L, R).

![Audio en Video coderingsprogramma 's](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio en Video coderingsprogramma 's*

Voeg nu de **ISO Mpeg-4 Multiplexer** en **uitvoer in een bestand** onderdelen en sluit de pincodes, zoals wordt weergegeven.

![Multiplexer MP4- en uitvoer in een bestand](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*Multiplexer MP4- en uitvoer in een bestand*

U moet de naam voor het uitvoerbestand instellen. Klik op de **uitvoer in een bestand** onderdeel en de expressie voor het bestand bewerken:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Bestandsnaam voor uitvoer](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Bestandsnaam voor uitvoer*

De werkstroom lokaal om te controleren of deze correct wordt uitgevoerd, kunt u uitvoeren.

Nadat deze is voltooid, kunt u deze uitvoeren in Azure Media Services.

Een actief in Azure Media Services aan twee bestanden erin eerst voorbereiden: het videobestand en het logo. U kunt dit doen met behulp van de .NET of REST-API. U kunt dit ook doen met behulp van de Azure-portal of [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Deze zelfstudie laat zien hoe u voor het beheren van activa met AMSE. Er zijn twee manieren bestanden toevoegen aan een asset:

* Maken van een lokale map, het kopiëren van de twee bestanden in dit en het slepen en neerzetten van de map voor de **Asset** tabblad.
* Upload het videobestand als een actief, de activa-informatie wordt weergegeven, gaat u naar het tabblad bestanden en een extra (logo)-bestand uploaden.

> [!NOTE]
> Zorg ervoor dat u een primair bestand instelt in de asset (video hoofdbestand).

![Assetbestanden in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Assetbestanden in AMSE*

Selecteer de asset en kies voor het coderen van deze met Premium-Encoder. Uploaden van de werkstroom en selecteer deze.

Klik op de knop gegevens doorgeven aan de processor en het volgende XML-bestand voor het instellen van de runtime-eigenschappen toe:

![Premium-codering in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium-codering in AMSE*

Plak de volgende XML-gegevens. U moet de naam van het bestand opgeven voor de Media bestandsinvoer- en primarySourceFile. Geef de naam van de bestandsnaam voor het logo te.

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

Als u de .NET SDK gebruiken om te maken en de taak wordt uitgevoerd, is deze XML-gegevens worden doorgegeven als de configuratietekenreeks voor.

```csharp
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

Nadat de taak voltooid is, wordt het MP4-bestand in de uitvoerasset de overlay!

![Op de video overlay](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Op de video overlay*

U kunt downloaden via de voorbeeldwerkstroom van [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>Voorbeeld 2: Meerdere audio taalcode

Een voorbeeld van meerdere audio taal codering workfkow is beschikbaar in [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Deze map bevat een voorbeeldwerkstroom die kan worden gebruikt voor het coderen van een bestand MXF naar een activum multi MP4-bestanden met meerdere audio houdt.

Deze werkstroom wordt ervan uitgegaan dat het bestand MXF bevat één-nummer; de aanvullende audio nummers moeten worden doorgegeven als afzonderlijke audio-bestanden (WAV of MP4...).

Als u wilt coderen, Ga als volgt te werk:

* Een Media Services-activum maken met de MXF en Audio-bestanden (0 tot en met 18 audio-bestanden).
* Zorg ervoor dat het bestand MXF is ingesteld als een primair bestand.
* Maak een taak en een taak met behulp van de processor Premium werkstroom Encoder. De opgegeven workflow (MultiMP4-1080p-19audio-v1.workflow) gebruiken.
* De gegevens setruntime.xml doorgeven aan de taak (als u Azure Media Services Explorer gebruikt, gebruik de knop 'xml-gegevens doorgeven aan de werkstroom').
  * Werk de XML-gegevens om op te geven van de juiste tags voor namen en talen.
  * De werkstroom heeft audio-onderdelen met de naam Audio 1 tot en met 18 Audio.
  * RFC5646 wordt ondersteund voor de taal-tag.

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

* De gecodeerde asset multi taal audio-nummers en worden deze nummers moeten worden geselecteerd in Azure Media Player.

## <a name="see-also"></a>Zie ook
* [Inleiding tot Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Het gebruik van Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Codering van inhoud op aanvraag met Azure Media Services](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Werkstroom voor Media Encoder Premium indelingen en codecs](media-services-premium-workflow-encoder-formats.md)
* [Werkstroom voorbeeldbestanden](https://github.com/Azure/azure-media-services-samples)
* [Azure Media Services Explorer-hulpprogramma](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
