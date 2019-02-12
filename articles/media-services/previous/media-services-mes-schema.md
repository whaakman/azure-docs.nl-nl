---
title: Media Encoder Standard schema | Microsoft Docs
description: Het artikel biedt een overzicht van de Media Encoder Standard-schema.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 58306780978189749b592b6cd9d13c63ecd25641
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996148"
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard-schema
In dit artikel worden enkele van de elementen en typen van het XML-schema beschreven waarop [voorinstellingen van Media Encoder Standard](media-services-mes-presets-overview.md) zijn gebaseerd. Het artikel bevat uitleg over elementen en hun geldige waarden.  

## <a name="Preset"></a> De vooraf ingestelde standaardcodering (hoofdelement)
Hiermee definieert u een vooraf ingestelde standaardcodering.  

### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **Codering** |[Codering](media-services-mes-schema.md#Encoding) |Root-element geeft aan dat de invoerbronnen moeten worden gecodeerd. |
| **Uitvoer** |[Uitvoer](media-services-mes-schema.md#Output) |Verzameling van gewenste uitvoerbestanden. |
| **StretchMode**<br/>minOccurs="0"<br/>default="AutoSize|xs:String|Bepalen van het formaat van de video frame uitvoer, opvulling, pixel of hoogte-breedteverhouding weergegeven. **StretchMode** mogelijk een van de volgende waarden: **Geen**, **AutoSize** (standaard), of **AutoAanpassen**.<br/><br/>**Geen**: Strikt gaat u als volgt de uitvoer-oplossing (bijvoorbeeld de **breedte** en **hoogte** in de voorinstelling) zonder rekening te houden de pixel hoogte-breedteverhouding of hoogte-breedteverhouding weergegeven van de invoervideo. Aanbevolen in scenario's zoals [bijsnijden](media-services-crop-video.md), waar de uitvoervideo heeft een andere hoogte-breedteverhouding ten opzichte van de invoer. <br/><br/>**AutoSize**: De uitvoerresolutie van de wordt in het venster past (breedte * hoogte) opgegeven door de voorinstelling. Het coderingsprogramma levert echter een uitvoervideo met vierkante (1:1) pixels hoogte-breedteverhouding. Daarom op uitvoer van de breedte of hoogte uitvoer kan worden overschreven als u wilt overeenkomen met de hoogte-breedteverhouding van de weergave van de invoer, zonder opvulling. Als de invoer 1920 x 1080 is en de vooraf ingestelde standaardcodering 1280 x 1280 gevraagd, klikt u vervolgens de Height-waarde in de voorinstelling is genegeerd en de uitvoer worden op 1280 x 720, waarmee de invoer hoogte-breedteverhouding van 16:9 worden bewaard. <br/><br/>**AutoAanpassen**: Indien nodig, opvullen met het teken van de uitvoervideo (met letterbox of pillarbox) om te voldoen aan de gewenste uitvoerresolutie, terwijl u ervoor te zorgen dat de actieve video regio in de uitvoer de dezelfde hoogte-breedteverhouding als invoer heeft. Stel bijvoorbeeld dat de invoer is 1920 x 1080 en de vooraf ingestelde standaardcodering 1280 x 1280 gevraagd. Vervolgens worden de video-uitvoer op 1280 x 1280, maar deze bevat een binnenste rechthoek 1280 x 720 van 'active video' met hoogte-breedteverhouding van 16:9 en letterbox regio's 280 pixels hoog boven of onder. Voor een ander voorbeeld: als de invoer 1440 x 1080 is en de vooraf ingestelde standaardcodering om 1280 x 720 vraagt, klikt u vervolgens de uitvoer worden weergegeven op 1280 x 720, waarin een binnenste rechthoek van 960 x 720 op de hoogte-breedteverhouding 4:3 en pijler vak regio's 160 pixels breed bij de links en rechts. 

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **Versie**<br/><br/> Vereist |**xs: decimaal** |De vooraf ingestelde versie. De volgende beperkingen zijn van toepassing: xs:fractionDigits waarde = "1" en xs:minInclusive value = "1" bijvoorbeeld **versie = "1.0"**. |

## <a name="Encoding"></a> Codering
Bevat een reeks van de volgende elementen:  

### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Instellingen voor H.264-codering van video. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Instellingen voor de AAC-codering van audio. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Instellingen voor Bmp-afbeelding. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Instellingen voor PNG-afbeelding. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Instellingen voor het Jpg-afbeelding. |

## <a name="H264Video"></a> H264Video
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs="0" |**xs:boolean** |Op dit moment wordt slechts één keer codering ondersteund. |
| **KeyFrameInterval**<br/><br/> minOccurs="0"<br/><br/> **default="00:00:02"** |**xs:time** |Bepaalt de vaste afstand tussen de IDR frames in eenheden van seconden. Als de duur van de GOP ook genoemd. Zie **SceneChangeDetection** voor het beheren van of het coderingsprogramma van deze waarde afwijken kan. |
| **SceneChangeDetection**<br/><br/> minOccurs="0"<br/><br/> standaardinstelling = "false" |**xs: Boolean-waarde** |Als is ingesteld op true, encoder probeert te detecteren scène wijzigen in de video en een frame IDR voegt. |
| **Complexiteit**<br/><br/> minOccurs="0"<br/><br/> default="Balanced" |**xs:String** |Hiermee bepaalt u de verhouding tussen coderen snelheid en video. Een van de volgende waarden kan zijn: **Snelheid**, **met gelijke taakverdeling**, of **kwaliteit**<br/><br/> Standaard: **Met gelijke taakverdeling** |
| **SyncMode**<br/><br/> minOccurs="0" | |Functie weergegeven in een toekomstige release. |
| **H264Layers**<br/><br/> minOccurs="0" |[H264Layers](media-services-mes-schema.md#H264Layers) |Verzameling van uitvoer video lagen. |

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **voorwaarde** |**xs:String** | Wanneer de invoer geen video heeft, kunt u om af te dwingen van het coderingsprogramma om in te voegen een zwart-wit video bijhouden. Om dit te doen, gebruikt u de voorwaarde = "InsertBlackIfNoVideoBottomLayerOnly" (aan een video op alleen de laagste bitrate invoegen) of een voorwaarde = "InsertBlackIfNoVideo" (uitvoer bitsnelheden helemaal invoegen van een video). Raadpleeg [dit artikel](media-services-advanced-encoding-with-mes.md#no_video) voor meer informatie.|

## <a name="H264Layers"></a> H264Layers

Als u een invoer voor de codering met alleen audio en geen video, verzendt bevat de uitvoerasset standaard bestanden met alleen audio gegevens. Sommige spelers niet mogelijk voor het afhandelen van dergelijke uitvoerstromen. U kunt een van de H264Video **InsertBlackIfNoVideo** kenmerk instelling om af te dwingen van het coderingsprogramma een video bijhouden toevoegen aan de uitvoer in dat scenario. Raadpleeg [dit artikel](media-services-advanced-encoding-with-mes.md#no_video) voor meer informatie.
              
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs = "0" maxOccurs = "niet-gebonden" |[H264Layer](media-services-mes-schema.md#H264Layer) |Een verzameling van H264 lagen. |

## <a name="H264Layer"></a> H264Layer
> [!NOTE]
> Limieten voor video's zijn gebaseerd op de waarden die worden beschreven in de [H264 niveaus](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tabel.  
> 
> 

### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **Profiel**<br/><br/> minOccurs="0"<br/><br/> standaardwaarde = 'Auto' |**xs: tekenreeks** |Kan worden van een van de volgende **xs: tekenreeks** waarden: **Auto**, **Baseline**, **Main**, **High**. |
| **Niveau**<br/><br/> minOccurs="0"<br/><br/> standaardwaarde = 'Auto' |**xs: tekenreeks** | |
| **Bitrate**<br/><br/> minOccurs="0" |**xs:int** |De bitrate gebruikt voor deze video laag, opgegeven in kbps. |
| **MaxBitrate**<br/><br/> minOccurs="0" |**xs: int** |De maximale bitrate gebruikt voor deze video laag, opgegeven in kbps. |
| **BufferWindow**<br/><br/> minOccurs="0"<br/><br/> default="00:00:05" |**xs: tijd** |De lengte van de video buffer. |
| **Breedte**<br/><br/> minOccurs="0" |**xs: int** |De breedte van de video frame van uitvoer in pixels.<br/><br/> Op dit moment moet u zowel de breedte en hoogte opgeven. De breedte en hoogte moet even getallen. |
| **Hoogte**<br/><br/> minOccurs="0" |**xs:int** |Hoogte van de video frame van uitvoer in pixels.<br/><br/> Op dit moment moet u zowel de breedte en hoogte opgeven. De breedte en hoogte moet even getallen.|
| **BFrames**<br/><br/> minOccurs="0" |**xs: int** |Aantal frames tussen verwijzing frames B. |
| **ReferenceFrames**<br/><br/> minOccurs="0"<br/><br/> standaardinstelling = "3" |**xs:int** |Aantal frames van de referentie in een GOP. |
| **EntropyMode**<br/><br/> minOccurs="0"<br/><br/> default=”Cabac” |**xs: tekenreeks** |Een van de volgende waarden kan zijn: **Cabac** en **Cavlc**. |
| **FrameRate**<br/><br/> minOccurs="0" |Rationeel getal |Bepaalt de framesnelheid van de uitvoervideo. Gebruik standaard '0/1' om te laten de encoder die de dezelfde framesnelheid gebruiken als de video-invoer. Toegestane waarden zijn algemene video framesnelheden verwacht. Echter, een geldige rationele is toegestaan. Bijvoorbeeld: 1/1 1 fps zou zijn en is geldig.<br/><br/> - 12/1  (12 fps)<br/><br/> - 15/1 (15 fps)<br/><br/> - 24/1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> - 25/1 (25 fps)<br/><br/>  - 30/1 (30 fps)<br/><br/> 30000/1001 (29,97 fps) <br/> <br/>**Houd er rekening mee** als u een aangepaste voorinstelling voor multiple-bitrate codering, klikt u vervolgens alle lagen van de vooraf ingestelde **moet** gebruik dezelfde waarde van de framesnelheid.|
| **AdaptiveBFrame**<br/><br/> minOccurs="0" |**xs: Boolean-waarde** |Kopiëren van Azure media encoder |
| **Segmenten**<br/><br/> minOccurs="0"<br/><br/> default="0" |**xs:int** |Bepaalt het aantal segmenten een kader is onderverdeeld in. Raden u aan met behulp van standaard. |

## <a name="AACAudio"></a> AACAudio
 Bevat een reeks van de volgende elementen en groepen.  

 Zie voor meer informatie over AAC [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **Profiel**<br/><br/> minOccurs="0 "<br/><br/> default="AACLC" |**xs: tekenreeks** |Een van de volgende waarden kan zijn: **AACLC**, **HEAACV1**, of **HEAACV2**. |

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **voorwaarde** |**xs: tekenreeks** |Als u wilt afdwingen van het coderingsprogramma voor het produceren van een asset met een op de achtergrond audiotrack invoer heeft geen audio, geeft u de waarde 'InsertSilenceIfNoAudio'.<br/><br/> Standaard, als u de invoer voor het coderingsprogramma dat alleen video en geen audio bevat, stuurt bevat vervolgens de uitvoerasset bestanden die alleen video gegevens bevatten. Sommige spelers niet mogelijk voor het afhandelen van dergelijke uitvoerstromen. U kunt deze instelling gebruiken om af te dwingen van het coderingsprogramma een op de achtergrond audiotrack toevoegen aan de uitvoer in dat scenario. |

### <a name="groups"></a>Groepen
| Referentie | Description |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs="0" |Beschrijving van [AudioGroup](media-services-mes-schema.md#AudioGroup) weten van het juiste aantal kanalen, samplefrequentie en bitsnelheid die kan worden ingesteld voor elk profiel. |

## <a name="AudioGroup"></a> AudioGroup
Zie de volgende tabel 'details Audio codec' voor meer informatie over welke waarden geldig zijn voor elk profiel zijn.  

### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **kanalen**<br/><br/> minOccurs="0" |**xs: int** |Het aantal audio kanalen gecodeerd. Hier volgen de geldige opties: 1, 2, 5, 6, 8.<br/><br/> Standaard: 2. |
| **SamplingRate**<br/><br/> minOccurs="0" |**xs: int** |De samplefrequentie van audio, opgegeven in Hz. |
| **Bitrate**<br/><br/> minOccurs="0" |**xs: int** |De bitrate gebruikt wanneer de audio-codering opgegeven in kbps. |

### <a name="audio-codec-details"></a>Details van audiocodec
Audio Codec|Details  
-----------------|---  
**AACLC**|1:<br/><br/> - 11025: 8 &lt;= bitrate &lt; 16<br/><br/> - 12000: 8 &lt;= bitrate &lt; 16<br/><br/> - 16000: 8 &lt;= bitrate &lt;32<br/><br/>- 22050: 24 uur per dag &lt;= bitrate &lt; 32<br/><br/> - 24000: 24 uur per dag &lt;= bitrate &lt; 32<br/><br/> - 32000: 32 &lt;= bitrate &lt;= 192<br/><br/> - 44100: 56 &lt;= bitrate &lt;= 288<br/><br/> - 48000: 56 &lt;= bitrate &lt;= 288<br/><br/> - 88200 : 128 &lt;= bitrate &lt;288 =<br/><br/> - 96000 : 128 &lt;= bitrate &lt;288 =<br/><br/> 2:<br/><br/> - 11025: 16 &lt;= bitrate &lt; 24 uur per dag<br/><br/> - 12000: 16 &lt;= bitrate &lt; 24 uur per dag<br/><br/> - 16000: 16 &lt;= bitrate &lt; 40<br/><br/> - 22050: 32 &lt;= bitrate &lt; 40<br/><br/> - 24000 : 32 &lt;= bitrate &lt; 40<br/><br/> - 32000:  40 &lt;= bitrate &lt;= 384<br/><br/> - 44100: 96 &lt;= bitrate &lt;= 576<br/><br/> - 48000 : 96 &lt;= bitrate &lt;= 576<br/><br/> - 88200: 256 &lt;= bitrate &lt;576 =<br/><br/> - 96000: 256 &lt;= bitrate &lt;576 =<br/><br/> 5/6:<br/><br/> - 32000: 160 &lt;= bitrate &lt;= 896<br/><br/> - 44100: 240 &lt;= bitrate &lt;1024 =<br/><br/> - 48000: 240 &lt;= bitrate &lt;1024 =<br/><br/> - 88200: 640 &lt;= bitrate &lt;1024 =<br/><br/> - 96000: 640 &lt;= bitrate &lt;1024 =<br/><br/> 8:<br/><br/> - 32000 : 224 &lt;= bitrate &lt;1024 =<br/><br/> - 44100 : 384 &lt;= bitrate &lt;= 1024<br/><br/> - 48000: 384 &lt;= bitrate &lt;= 1024<br/><br/> - 88200: 896 &lt;= bitrate &lt;1024 =<br/><br/> - 96000: 896 &lt;= bitrate &lt;1024 =  
**HEAACV1**|1:<br/><br/> -22050: bitrate = 8<br/><br/> - 24000: 8 &lt;= bitrate &lt;= 10<br/><br/> - 32000: 12 &lt;= bitrate &lt;= 64<br/><br/> - 44100: 20 &lt;= bitrate &lt;= 64<br/><br/> - 48000: 20 &lt;= bitrate &lt;= 64<br/><br/> -88200: bitrate 64 =<br/><br/> 2:<br/><br/> - 32000: 16 &lt;bitrate = &lt;= 128<br/><br/> - 44100: 16 &lt;bitrate = &lt;= 128<br/><br/> - 48000: 16 &lt;bitrate = &lt;= 128<br/><br/> - 88200 : 96 &lt;= bitrate &lt;= 128<br/><br/> - 96000: 96 &lt;= bitrate &lt;= 128<br/><br/> 5/6:<br/><br/> - 32000 : 64 &lt;= bitrate &lt;= 320<br/><br/> - 44100: 64 &lt;= bitrate &lt;= 320<br/><br/> - 48000: 64 &lt;= bitrate &lt;= 320<br/><br/> - 88200 : 256 &lt;= bitrate &lt;= 320<br/><br/> - 96000: 256 &lt;= bitrate &lt;= 320<br/><br/> 8:<br/><br/> - 32000: 96 &lt;bitrate = &lt;= 448<br/><br/> - 44100: 96 &lt;bitrate = &lt;= 448<br/><br/> - 48000: 96 &lt;bitrate = &lt;= 448<br/><br/> - 88200: 384 &lt;= bitrate &lt;= 448<br/><br/> - 96000: 384 &lt;= bitrate &lt;= 448  
**HEAACV2**|2:<br/><br/> - 22050: 8 &lt;= bitrate &lt;= 10<br/><br/> - 24000: 8 &lt;= bitrate &lt;= 10<br/><br/> - 32000: 12 &lt;= bitrate &lt;= 64<br/><br/> - 44100: 20 &lt;= bitrate &lt;= 64<br/><br/> - 48000: 20 &lt;= bitrate &lt;= 64<br/><br/> - 88200: 64 &lt;= bitrate &lt;= 64  
  
## <a name="Clip"></a> Clip
### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **startTime** |**xs:duration** |Hiermee geeft u de begintijd van de presentatie. De waarde van de starttijd moet overeenkomen met de absolute tijdstempel van de invoervideo. Bijvoorbeeld, als het eerste frame van de invoervideo een tijdstempel van 12:00:10.000 heeft, klikt u vervolgens StartTime moet ten minste 12:00:10.000 of hoger. |
| **Duur** |**xs:duration** |Hiermee geeft u de duur van een presentatie (bijvoorbeeld de weergave van een overlay in de video). |

## <a name="Output"></a> Uitvoer
### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **FileName** |**xs:String** |De naam van het uitvoerbestand.<br/><br/> Macro's die worden beschreven in de volgende tabel kunt u de namen van de uitvoer te bouwen. Bijvoorbeeld:<br/><br/> **"Outputs": [      {       "FileName": "{Basename}*{Resolution}*{Bitrate}.mp4",       "Format": {         "Type": "MP4Format"}}]** |

### <a name="macros"></a>Macro 's
| Macro | Description |
| --- | --- |
| **{Basename}** |Als u VoD-codering, is de {Basename} in de eerste 32 tekens van de eigenschap AssetFile.Name van het primaire bestand in de invoer asset.<br/><br/> Als het invoeractivum een live-archief is, is klikt u vervolgens de {Basename} afgeleid van de trackName kenmerken in het manifest van de server. Als u een subclip-taak met behulp van de TopBitrate, zoals in indient: ' < VideoStream\>TopBitrate < / VideoStream\>', en het uitvoerbestand video bevat, wordt de {Basename} is de eerste 32 tekens van de trackName van de video-laag met de hoogste bitrate.<br/><br/> Als u in plaats daarvan het indienen van een subclip-taak met alle van de invoer bitsnelheden, zoals ' < VideoStream\>* < / VideoStream\>', en het uitvoerbestand video bevat, wordt {Basename} is de eerste 32 tekens van de trackName van de bijbehorende video laag. |
| **{Codec}** |Video is toegewezen aan 'H264' en 'AAC' voor audio. |
| **{Bitrate}** |De doel-video bitrate als het uitvoerbestand bevat video en audio- of audio bitrate doel als het uitvoerbestand alleen audio bevat. De waarde die wordt gebruikt, is de bitrate in kbps. |
| **{Channel}** |Aantal audio kanalen als het bestand audio bevat. |
| **{Width}** |Breedte van de video, in pixels, in het uitvoerbestand, als het bestand bevat de video. |
| **{Height}** |De hoogte van de video, in pixels, in het uitvoerbestand, als het bestand bevat de video. |
| **{Extension}** |Neemt over van de eigenschap 'Type' voor het uitvoerbestand. Naam van het uitvoerbestand heeft een extensie die een is van: "mp4", "ts", "jpg", 'png' of 'bmp'. |
| **{Index}** |Verplicht voor miniatuur. Alleen moet aanwezig zijn één keer. |

## <a name="Video"></a> Video (complex type neemt over van Codec)
### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **Beginnen** |**xs:String** | |
| **Stap** |**xs:String** | |
| **Range** |**xs:String** | |
| **PreserveResolutionAfterRotation** |**xs:boolean** |Zie de volgende sectie voor gedetailleerde uitleg: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a> PreserveResolutionAfterRotation
Het verdient aanbeveling om te gebruiken de **PreserveResolutionAfterRotation** vlag in combinatie met het probleem zou moeten waarden uitgedrukt in percentages (Width = "100%", Height = "100%").  

Standaard worden de coderen resolutie-instellingen (breedte, hoogte) in de Media Encoder Standard (MES)-standaardinstellingen gericht op video's met 0 graden. Bijvoorbeeld, als uw invoervideo 1280 x 720 met nul graden is, klikt u vervolgens de standaardvoorinstellingen Zorg ervoor dat de uitvoer dezelfde resolutie.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Als de invoervideo is opgenomen met niet-nul-rotatie (bijvoorbeeld een smartphone of tablet verticaal bewaard), klikt u vervolgens MES standaard de coderen resolutie-instellingen (breedte, hoogte) is van toepassing op de video-invoer en vervolgens compenseren voor de rotatie. Zie bijvoorbeeld de volgende afbeelding. De definitie maakt gebruik van breedte = "100%", Height = "100% ', die MES wordt geïnterpreteerd als de uitvoer 1280 pixels breed en 720 pixels hoog vereisen. Na het draaien van de video, wordt deze vervolgens de afbeelding om aan te passen in dit venster leidt tot pillar-box gebieden aan de linkerkant en rechts kleiner.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

U kunt ook kunt u het gebruik van de **PreserveResolutionAfterRotation** markeren en stel deze in op 'true' (de standaardwaarde is "false"). Dus als uw voorinstelling Width is = "100%", hoogte = "100%" en PreserveResolutionAfterRotation ingesteld op "true", een invoervideo 1280 pixels breed en 720 pixels hoog met 90 graden te draaien produceert een uitvoer met nul graden te draaien, maar 720 pixels breed en 1280 pixels hoog. Zie de volgende afbeelding:  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a> FormatGroup (groep)
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a> BmpLayer
### <a name="element"></a>Element
| Name | Type | Description |
| --- | --- | --- |
| **Breedte**<br/><br/> minOccurs="0" |**xs:int** | |
| **Hoogte**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **voorwaarde** |**xs:String** | |

## <a name="PngLayer"></a> PngLayer
### <a name="element"></a>Element
| Name | Type | Description |
| --- | --- | --- |
| **Breedte**<br/><br/> minOccurs="0" |**xs:int** | |
| **Hoogte**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **voorwaarde** |**xs:String** | |

## <a name="JpgLayer"></a> JpgLayer
### <a name="element"></a>Element
| Name | Type | Description |
| --- | --- | --- |
| **Breedte**<br/><br/> minOccurs="0" |**xs:int** | |
| **Hoogte**<br/><br/> minOccurs="0" |**xs:int** | |
| **Kwaliteit**<br/><br/> minOccurs="0" |**xs:int** |Geldige waarden: 1(worst)-100(best) |

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **voorwaarde** |**xs:String** | |

## <a name="PngLayers"></a> PngLayers
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs = "0" maxOccurs = "niet-gebonden" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a> BmpLayers
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs = "0" maxOccurs = "niet-gebonden" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a> JpgLayers
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs = "0" maxOccurs = "niet-gebonden" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a> BmpImage (complex type overneemt van de Video)
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lagen |

## <a name="JpgImage"></a> JpgImage (complex type overneemt van de Video)
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lagen |

## <a name="PngImage"></a> PngImage (complex type overneemt van de Video)
### <a name="elements"></a>Elementen
| Name | Type | Description |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lagen |

## <a name="examples"></a>Voorbeelden
Zie voorbeelden van XML-standaardinstellingen die zijn gebouwd op basis van dit schema, Zie [taak voorinstellingen voor MES (Media Encoder Standard)](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

