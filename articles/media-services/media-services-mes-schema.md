---
title: Media Encoder Standard schema | Microsoft Docs
description: Het artikel biedt een overzicht van de Media Encoder Standard schema.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4c060062-8ef2-41d9-834e-e81e8eafcf2e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: e936f5c47abe5bb5531f9af3be48662ea2f48c97
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard schema
In dit artikel worden enkele van de elementen en typen van het XML-schema beschreven waarop [Media Encoder Standard voorinstellingen](media-services-mes-presets-overview.md) zijn gebaseerd. Het artikel geeft uitleg van de elementen en hun geldige waarden.  

## <a name="Preset"></a>Voorinstelling (hoofdelement)
Hiermee definieert u een codering voorinstelling.  

### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Codering** |[Codering](media-services-mes-schema.md#Encoding) |Hoofdelement, geeft aan dat de invoerbronnen moeten worden gecodeerd. |
| **Uitvoer** |[Uitvoer](media-services-mes-schema.md#Output) |Verzameling van gewenste uitvoerbestanden. |

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Versie**<br/><br/> Vereist |**xs: decimaal** |De vooraf ingestelde versie. Gelden de volgende beperkingen: xs:fractionDigits waarde = "1" en xs:minInclusive value = "1" bijvoorbeeld **versie = "1.0"**. |

## <a name="Encoding"></a>Codering
Bevat een reeks van de volgende elementen:  

### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Instellingen voor H.264-codering van video. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Instellingen voor AAC audio-codering. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Instellingen voor Bmp-afbeelding. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Instellingen voor PNG-afbeelding. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Instellingen voor Jpg-afbeelding. |

## <a name="H264Video"></a>H264Video
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs = '0' |**xs:Boolean** |Codering in slechts één keer wordt momenteel ondersteund. |
| **KeyFrameInterval**<br/><br/> minOccurs = '0'<br/><br/> **standaardwaarde = "00: 00:02 '** |**xs:time** |Bepaalt de vaste afstand tussen IDR frames in eenheden van seconden. Ook de duur GOP genoemd. Zie **SceneChangeDetection** voor het beheren van of het coderingsprogramma van deze waarde afwijken kan. |
| **SceneChangeDetection**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = "false" |**xs: Booleaanse** |Als is ingesteld op true, encoder probeert te detecteren scene wijziging in de video en een frame IDR invoegen. |
| **Complexiteit**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = 'Evenwichtig' |**xs:String** |Hiermee bepaalt u de verhouding tussen coderen snelheid en video. Kan een van de volgende waarden zijn: **snelheid**, **Gebalanceerd**, of **kwaliteit**<br/><br/> Standaardwaarde: **met gelijke taakverdeling** |
| **SyncMode**<br/><br/> minOccurs = '0' | |Functie zichtbaar in een toekomstige release. |
| **H264Layers**<br/><br/> minOccurs = '0' |[H264Layers](media-services-mes-schema.md#H264Layers) |Verzameling van uitvoer video lagen. |

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Voorwaarde** |**xs:String** | Wanneer de invoer geen video heeft, is het raadzaam om af te dwingen van het coderingsprogramma invoegen monochroom video bijgehouden. Hiertoe voorwaarde gebruiken = 'InsertBlackIfNoVideoBottomLayerOnly' (voor een video invoegen op alleen de laagste bitrate) of voorwaarde = 'InsertBlackIfNoVideo' (een video helemaal invoegen uitvoer bitsnelheden). Raadpleeg [dit artikel](media-services-advanced-encoding-with-mes.md#no_video) voor meer informatie.|

## <a name="H264Layers"></a>H264Layers

Als u de invoer voor de codering met alleen audio en geen video verzendt bevat de uitvoerasset standaard bestanden met alleen audiogegevens. Sommige spelers kan wellicht geen dergelijke uitvoerstromen verwerkt. U kunt de H264Video **InsertBlackIfNoVideo** kenmerk instelling om af te dwingen van het coderingsprogramma video bijgehouden toevoegen aan de uitvoer in dat scenario. Raadpleeg [dit artikel](media-services-advanced-encoding-with-mes.md#no_video) voor meer informatie.
              
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs = "0" maxOccurs = 'unbounded' |[H264Layer](media-services-mes-schema.md#H264Layer) |Een verzameling van lagen van de geselecteerde instelling H264. |

## <a name="H264Layer"></a>H264Layer
> [!NOTE]
> Video ondersteuningslimieten zijn gebaseerd op de waarden die worden beschreven de [niveaus van de geselecteerde instelling H264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tabel.  
> 
> 

### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Profiel**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = 'Auto' |**xs: tekenreeks** |Kan niet van een van de volgende **xs: tekenreeks** waarden: **automatisch**, **basislijn**, **Main**, **hoge**. |
| **Niveau**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = 'Auto' |**xs: tekenreeks** | |
| **Bitrate**<br/><br/> minOccurs = '0' |**xs:int** |De bitrate gebruikt voor deze video laag, opgegeven in kbps. |
| **MaxBitrate**<br/><br/> minOccurs = '0' |**xs: int** |De maximale bitrate gebruikt voor deze video laag, opgegeven in kbps. |
| **BufferWindow**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = "00: 00:05 ' |**xs: tijd** |De lengte van de video buffer. |
| **Breedte**<br/><br/> minOccurs = '0' |**xs: int** |De breedte van de video frame van uitvoer in pixels.<br/><br/> Op dit moment moet u zowel de breedte en hoogte. De breedte en hoogte moeten even getallen. |
| **Hoogte**<br/><br/> minOccurs = '0' |**xs:int** |De hoogte van de video frame van uitvoer in pixels.<br/><br/> Op dit moment moet u zowel de breedte en hoogte. De breedte en hoogte moeten even getallen.|
| **BFrames**<br/><br/> minOccurs = '0' |**xs: int** |Het aantal B frames tussen verwijzing frames. |
| **ReferenceFrames**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = "3" |**xs:int** |Aantal frames in een GOP verwijzing. |
| **EntropyMode**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = 'Cabac' |**xs: tekenreeks** |Kan een van de volgende waarden zijn: **Cabac** en **Cavlc**. |
| **Framesnelheid**<br/><br/> minOccurs = '0' |rationele getal |Bepaalt de framesnelheid van de uitvoervideo. De standaardwaarde van '0/1' het coderingsprogramma de dezelfde framesnelheid gebruiken als invoer video laten gebruiken. Toegestane waarden zijn algemene video framesnelheid verwacht. Echter, een geldige rationele is toegestaan. Bijvoorbeeld: 1/1 1 fps zou zijn en is geldig.<br/><br/> -12/1 (12 fps)<br/><br/> -15/1 (15 fps)<br/><br/> -24/1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> -25/1 (25 fps)<br/><br/>  -30/1 (30 fps)<br/><br/> 30000/1001 (29,97 fps) <br/> <br/>**Opmerking** als u een aangepaste voorinstelling voor het coderen van meerdere bitrate vervolgens alle lagen van de vooraf ingestelde **moet** framesnelheid dezelfde waarde gebruiken.|
| **AdaptiveBFrame**<br/><br/> minOccurs = '0' |**xs: Booleaanse** |Kopiëren van Azure media encoder |
| **Segmenten**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = '0' |**xs:int** |Hiermee wordt bepaald hoeveel segmenten een frame is onderverdeeld in. Raden het gebruik van standaard. |

## <a name="AACAudio"></a>AACAudio
 Bevat een reeks van de volgende elementen en groepen.  

 Zie voor meer informatie over AAC [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Profiel**<br/><br/> minOccurs = '0'<br/><br/> standaardwaarde = 'AACLC' |**xs: tekenreeks** |Kan een van de volgende waarden zijn: **AACLC**, **HEAACV1**, of **HEAACV2**. |

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Voorwaarde** |**xs: tekenreeks** |Als u wilt dat de codering voor het produceren van een activum met een achtergrond-nummer invoer heeft geen audio, geeft u de waarde 'InsertSilenceIfNoAudio'.<br/><br/> Standaard als u de invoer voor de codering met alleen video en geen audio verzendt bevat vervolgens de uitvoerasset bestanden die alleen video gegevens bevatten. Sommige spelers kan wellicht geen dergelijke uitvoerstromen verwerkt. U kunt deze instelling gebruiken om af te dwingen van het coderingsprogramma een achtergrond-nummer toevoegen aan de uitvoer in dat scenario. |

### <a name="groups"></a>Groepen
| Referentie | Beschrijving |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs = '0' |Zie de beschrijving van [AudioGroup](media-services-mes-schema.md#AudioGroup) weten van het juiste aantal kanalen, samplefrequentie en bitsnelheid die kan worden ingesteld voor elk profiel. |

## <a name="AudioGroup"></a>AudioGroup
Zie de onderstaande tabel "Audio codec gegevens" voor meer informatie over welke waarden geldig voor elk profiel zijn.  

### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Kanalen**<br/><br/> minOccurs = '0' |**xs: int** |Het aantal audio kanalen die zijn gecodeerd. De volgende geldige opties zijn: 1, 2, 5, 6, 8.<br/><br/> Standaard: 2. |
| **SamplingRate**<br/><br/> minOccurs = '0' |**xs: int** |De audio samplefrequentie, opgegeven in Hz. |
| **Bitrate**<br/><br/> minOccurs = '0' |**xs: int** |De bitrate gebruikt wanneer de audio-codering opgegeven in kbps. |

### <a name="audio-codec-details"></a>Audio-codec details
Audio-Codec|Details  
-----------------|---  
**AACLC**|1:<br/><br/> -11025: 8 &lt;= bitrate &lt; 16<br/><br/> -12000: 8 &lt;= bitrate &lt; 16<br/><br/> -16000: 8 &lt;= bitrate &lt;32<br/><br/>-22050: 24 &lt;= bitrate &lt; 32<br/><br/> -24000: 24 &lt;= bitrate &lt; 32<br/><br/> -32000: 32 &lt;bitrate = &lt;= 192<br/><br/> -44100: 56 &lt;= bitrate &lt;288 =<br/><br/> -48000: 56 &lt;= bitrate &lt;288 =<br/><br/> -88200: 128 &lt;= bitrate &lt;288 =<br/><br/> -96000: 128 &lt;= bitrate &lt;288 =<br/><br/> 2:<br/><br/> -11025: 16 &lt;= bitrate &lt; 24<br/><br/> -12000: 16 &lt;= bitrate &lt; 24<br/><br/> -16000: 16 &lt;= bitrate &lt; 40<br/><br/> -22050: 32 &lt;= bitrate &lt; 40<br/><br/> -24000: 32 &lt;= bitrate &lt; 40<br/><br/> -32000: 40 &lt;bitrate = &lt;= 384<br/><br/> -44100: 96 &lt;= bitrate &lt;576 =<br/><br/> -48000: 96 &lt;= bitrate &lt;576 =<br/><br/> -88200: 256 &lt;= bitrate &lt;576 =<br/><br/> -96000: 256 &lt;= bitrate &lt;576 =<br/><br/> 5/6:<br/><br/> -32000: 160 &lt;= bitrate &lt;896 =<br/><br/> -44100: 240 &lt;= bitrate &lt;1024 =<br/><br/> -48000: 240 &lt;= bitrate &lt;1024 =<br/><br/> -88200: 640 &lt;= bitrate &lt;1024 =<br/><br/> -96000: 640 &lt;= bitrate &lt;1024 =<br/><br/> 8:<br/><br/> -32000: 224 &lt;= bitrate &lt;1024 =<br/><br/> -44100: 384 &lt;= bitrate &lt;1024 =<br/><br/> -48000: 384 &lt;= bitrate &lt;1024 =<br/><br/> -88200: 896 &lt;= bitrate &lt;1024 =<br/><br/> -96000: 896 &lt;= bitrate &lt;1024 =  
**HEAACV1**|1:<br/><br/> -22050: bitrate = 8<br/><br/> -24000: 8 &lt;bitrate = &lt;= 10<br/><br/> -32000: 12 &lt;bitrate = &lt;= 64<br/><br/> -44100: 20 &lt;bitrate = &lt;= 64<br/><br/> -48000: 20 &lt;bitrate = &lt;= 64<br/><br/> -88200: bitrate = 64<br/><br/> 2:<br/><br/> -32000: 16 &lt;bitrate = &lt;= 128<br/><br/> -44100: 16 &lt;bitrate = &lt;= 128<br/><br/> -48000: 16 &lt;bitrate = &lt;= 128<br/><br/> -88200: 96 &lt;bitrate = &lt;= 128<br/><br/> -96000: 96 &lt;bitrate = &lt;= 128<br/><br/> 5/6:<br/><br/> -32000: 64 &lt;= bitrate &lt;320 =<br/><br/> -44100: 64 &lt;= bitrate &lt;320 =<br/><br/> -48000: 64 &lt;= bitrate &lt;320 =<br/><br/> -88200: 256 &lt;= bitrate &lt;320 =<br/><br/> -96000: 256 &lt;= bitrate &lt;320 =<br/><br/> 8:<br/><br/> -32000: 96 &lt;bitrate = &lt;= 448<br/><br/> -44100: 96 &lt;bitrate = &lt;= 448<br/><br/> -48000: 96 &lt;bitrate = &lt;= 448<br/><br/> -88200: 384 &lt;bitrate = &lt;= 448<br/><br/> -96000: 384 &lt;bitrate = &lt;= 448  
**HEAACV2**|2:<br/><br/> -22050: 8 &lt;bitrate = &lt;= 10<br/><br/> -24000: 8 &lt;bitrate = &lt;= 10<br/><br/> -32000: 12 &lt;bitrate = &lt;= 64<br/><br/> -44100: 20 &lt;bitrate = &lt;= 64<br/><br/> -48000: 20 &lt;bitrate = &lt;= 64<br/><br/> -88200: 64 &lt;bitrate = &lt;= 64  
  

## <a name="Clip"></a>Clip
### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **StartTime** |**xs: duration** |Hiermee geeft u de begintijd van een presentatie. De waarde van StartTime moet overeenkomen met het absolute tijdstempels van de invoer video. Bijvoorbeeld, als het eerste frame van de invoer video een tijdstempel van 12:00:10.000 heeft, vervolgens StartTime moet ten minste 12:00:10.000 of hoger. |
| **Duur** |**xs: duration** |Hiermee geeft u de duur van een presentatie (bijvoorbeeld de vormgeving van een overlay in de video). |

## <a name="Output"></a>Uitvoer
### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Bestandsnaam** |**xs:String** |De naam van het uitvoerbestand.<br/><br/> U kunt macro's in de volgende tabel beschreven voor het bouwen van de namen van de uitvoer. Bijvoorbeeld:<br/><br/> **'Uitvoer': [{"FileName": "{Basename}*{resolutie}*MP4 {Bitrate} ', 'Indeling': {'Type': 'MP4Format'}}] ** |

### <a name="macros"></a>Macro 's
| Macro | Beschrijving |
| --- | --- |
| **{Basename}** |Als u VoD-codering, is de {Basename} in de eerste 32 tekens van de eigenschap AssetFile.Name van het primaire bestand in de invoer asset.<br/><br/> Als de invoer asset een live-archief is, wordt de {Basename} afgeleid van de kenmerken trackName in het manifest van de server. Als u een subclip taak met de TopBitrate als in indient: ' < VideoStream\>TopBitrate < / VideoStream\>', en het uitvoerbestand video bevat, wordt de {Basename} is de eerste 32 tekens van de trackName van de video laag met de hoogste bitrate.<br/><br/> Als u in plaats daarvan het indienen van een subclip taak met alle van de invoer bitsnelheden, zoals ' < VideoStream\>* < / VideoStream\>', en het uitvoerbestand bevat video en klik vervolgens {Basename} is de eerste 32 tekens van de trackName van de bijbehorende video laag. |
| **{Codec}** |Video is toegewezen aan 'Standaardinstelling H264' en 'AAC' voor audio. |
| **{Bitrate}** |De gewenste video bitsnelheid als het uitvoerbestand bevat video en audio- of audio bitrate doel als het uitvoerbestand alleen audio bevat. De waarde die wordt gebruikt, is de bitrate in kbps. |
| **{Kanaal}** |Aantal audio-kanalen als het bestand audio bevat. |
| **{Breedte}** |Breedte van de video in pixels op in het uitvoerbestand, als het bestand video bevat. |
| **{Hoogte}** |Hoogte van de video in pixels op in het uitvoerbestand, als het bestand video bevat. |
| **{De extensie}** |Neemt over van de eigenschap 'Type' voor het uitvoerbestand. De naam van het uitvoerbestand heeft een extensie die een van: 'mp4', 'ts', 'jpg', 'png' of 'bmp'. |
| **{Index}** |Verplicht voor de miniatuur. Alleen te aanwezig zijn eenmaal. |

## <a name="Video"></a>Video (complex type neemt over van Codec)
### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Beginnen** |**xs:String** | |
| **Stap** |**xs:String** | |
| **Bereik** |**xs:String** | |
| **PreserveResolutionAfterRotation** |**xs:Boolean** |Zie de volgende sectie voor een gedetailleerde uitleg: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a>PreserveResolutionAfterRotation
Het is raadzaam de **PreserveResolutionAfterRotation** vlag in combinatie met de omzetting van waarden die zijn uitgedrukt in percentages (Width = "100%", hoogte = "100%").  

Standaard worden de coderen resolutie-instellingen (Width en Height) in de standaardinstellingen voor Media Encoder Standard (MES) gericht op video's met 0 graden. Bijvoorbeeld, als uw invoervideo 1280 x 720 met nul graden, klikt u vervolgens de standaardinstellingen van de standaard Zorg ervoor dat de uitvoer dezelfde resolutie.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Als de invoer video is opgenomen met niet-nul draaihoek (bijvoorbeeld een smartphone of tablet ondergebracht verticaal), vervolgens MES standaard de instellingen voor het omzetten coderen (Width en Height) van toepassing is op de video invoer en vervolgens compenseren voor de draaihoek. Bijvoorbeeld, Zie de volgende afbeelding. De vooraf ingestelde gebruikt breedte = "100%", hoogte = "100%", die MES wordt opgevat als het vereisen van de uitvoer moet 1280 pixels breed en 720 pixels hoog. Na het roteren van de video wordt deze vervolgens de afbeelding in dit venster inpast leidt tot pillar-box gebieden op links, rechts kleiner.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

U kunt ook kunt u het gebruik van de **PreserveResolutionAfterRotation** vlag en wordt ingesteld op 'true' (de standaardwaarde is "false"). Dus als uw vooraf ingestelde breedte = heeft "100%", hoogte = "100%" en PreserveResolutionAfterRotation ingesteld op 'true', een invoervideo die 1280 pixels breed en 720 pixels met 90 graden hoog is, wordt een uitvoer met nul graden maar 720 pixels breed en 1280 pixels hoog. Zie de volgende afbeelding:  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a>FormatGroup (groeps)
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a>BmpLayer
### <a name="element"></a>Element
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Breedte**<br/><br/> minOccurs = '0' |**xs:int** | |
| **Hoogte**<br/><br/> minOccurs = '0' |**xs:int** | |

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Voorwaarde** |**xs:String** | |

## <a name="PngLayer"></a>PngLayer
### <a name="element"></a>Element
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Breedte**<br/><br/> minOccurs = '0' |**xs:int** | |
| **Hoogte**<br/><br/> minOccurs = '0' |**xs:int** | |

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Voorwaarde** |**xs:String** | |

## <a name="JpgLayer"></a>JpgLayer
### <a name="element"></a>Element
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Breedte**<br/><br/> minOccurs = '0' |**xs:int** | |
| **Hoogte**<br/><br/> minOccurs = '0' |**xs:int** | |
| **Kwaliteit**<br/><br/> minOccurs = '0' |**xs:int** |Geldige waarden: 1(worst)-100(best) |

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Voorwaarde** |**xs:String** | |

## <a name="PngLayers"></a>PngLayers
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs = "0" maxOccurs = 'unbounded' |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a>BmpLayers
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs = "0" maxOccurs = 'unbounded' |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a>JpgLayers
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs = "0" maxOccurs = 'unbounded' |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a>BmpImage (complex type overneemt van de Video)
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = '0' |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lagen |

## <a name="JpgImage"></a>JpgImage (complex type overneemt van de Video)
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = '0' |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lagen |

## <a name="PngImage"></a>PngImage (complex type overneemt van de Video)
### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = '0' |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lagen |

## <a name="examples"></a>Voorbeelden
Zie voorbeelden van XML-standaardinstellingen die zijn gebouwd op basis van dit schema, Zie [taak standaardinstellingen voor MES (Media Encoder Standard)](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

