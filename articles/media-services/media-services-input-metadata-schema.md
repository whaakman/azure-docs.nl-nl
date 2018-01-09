---
title: Azure Media Services invoer metagegevensschema | Microsoft Docs
description: Het onderwerp overzicht een van de invoer metagegevensschema Azure Media Services.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d72848e2-4b65-4c84-94bc-e2a90a6e7f47
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 85a3662fec08cfc081095ef252842a30fde7dd27
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="input-metadata"></a>Invoer metagegevens
Een codeertaak is gekoppeld aan een invoer asset (of activa) op waarop u wilt uitvoeren van sommige taken codering.  Na voltooiing van een taak wordt een uitvoerasset geproduceerd.  De uitvoerasset bevat video, audio, miniaturen, het manifest, enzovoort. De uitvoerasset bevat ook een bestand met metagegevens over de invoer asset. De naam van de metagegevens-XML-bestand heeft de volgende indeling: &lt;asset_id&gt;_metadata.xml (bijvoorbeeld 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), waarbij &lt;asset_id&gt; is de waarde van het item-id hebt van de invoer asset.  

Als u onderzoeken metagegevensbestand van de wilt, kunt u een **SAS** locator en download het bestand op uw lokale computer. U kunt een voorbeeld vinden op het maken van een SAS-locator en downloaden van een bestand [met Media Services .NET SDK Extensions](media-services-dotnet-get-started.md).  

Dit artikel worden de elementen en typen van het XML-schema beschreven waarvoor de invoer metada (&lt;asset_id&gt;_metadata.xml) is gebaseerd.  Zie voor meer informatie over het bestand met metagegevens over de uitvoerasset [uitvoer metagegevens](media-services-output-metadata-schema.md).  

> [!NOTE]
> U vindt de [Schema Code](media-services-input-metadata-schema.md#code) een [XML-voorbeeld](media-services-input-metadata-schema.md#xml) aan het einde van dit artikel.  
> 
> 

## <a name="AssetFiles"></a>AssetFiles-element (element root)
Bevat een verzameling [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s voor de coderingstaak.  

Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

| Naam | Beschrijving |
| --- | --- |
| **AssetFile**<br /><br /> minOccurs = "1" maxOccurs = 'unbounded' |Een één onderliggend element. Zie voor meer informatie [AssetFile element](media-services-input-metadata-schema.md#AssetFile). |

## <a name="AssetFile"></a>AssetFile element
 Kenmerken en elementen die beschrijving van een assetbestand bevat.  

 Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Naam**<br /><br /> Vereist |**xs:String** |Asset-bestandsnaam. |
| **Grootte**<br /><br /> Vereist |**xs:Long** |Grootte van het assetbestand in bytes. |
| **Duur**<br /><br /> Vereist |**xs: duration** |Inhoud afspelen back duur. Voorbeeld: Duur = 'PT25M37.757S'. |
| **NumberOfStreams**<br /><br /> Vereist |**xs:int** |Het aantal stromen in het assetbestand. |
| **FormatNames**<br /><br /> Vereist |**xs: tekenreeks** |Indelingsnamen. |
| **FormatVerboseNames**<br /><br /> Vereist |**xs: tekenreeks** |Uitgebreide indelingsnamen. |
| **StartTime** |**xs: duration** |Begintijd van inhoud. Voorbeeld: StartTime = 'PT2.669S'. |
| **OverallBitRate** |**xs: int** |Gemiddelde bitrate van het assetbestand in kbps. |

> [!NOTE]
> De volgende vier onderliggende elementen moeten worden weergegeven in een reeks.  
> 
> 

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Programma 's**<br /><br /> minOccurs = '0' | |Verzameling van alle [programma's element](media-services-input-metadata-schema.md#Programs) wanneer het assetbestand MPEG-TS-indeling heeft. |
| **VideoTracks**<br /><br /> minOccurs = '0' | |Elk bestand fysiek activum kan bevatten nul of meer video's nummers interleaved naar een indeling voor de juiste container. Dit element bevat een verzameling van alle [VideoTracks](media-services-input-metadata-schema.md#VideoTracks) die deel uitmaken van het assetbestand. |
| **AudioTracks**<br /><br /> minOccurs = '0' | |Elk bestand fysiek activum kan nul of meer audio houdt interleaved naar een indeling voor de juiste container bevatten. Dit element bevat een verzameling van alle [AudioTracks](media-services-input-metadata-schema.md#AudioTracks) die deel uitmaken van het assetbestand. |
| **Metagegevens**<br /><br /> minOccurs = "0" maxOccurs = 'unbounded' |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |De metagegevens van assetbestand weergegeven als key\value tekenreeksen. Bijvoorbeeld:<br /><br /> **&lt;Metagegevenssleutel = waarde 'taal' = 'eng' /&gt;** |

## <a name="TrackType"></a>TrackType
Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **ID**<br /><br /> Vereist |**xs:int** |Op nul gebaseerde index van dit audio- of -nummer.<br /><br /> Dit is niet noodzakelijkerwijs dat de TrackID als moet worden gebruikt in een MP4-bestand. |
| **Codec** |**xs:String** |Video bijhouden codec tekenreeks. |
| **CodecLongName** |**xs: tekenreeks** |Audio- of bijhouden codec lange naam. |
| **Tijdsbasis**<br /><br /> Vereist |**xs:String** |Basis. Voorbeeld: Tijdsbasis = ' 1/48000' |
| **NumberOfFrames** |**xs:int** |Aantal frames (aanwezig voor video houdt). |
| **StartTime** |**xs: duration** |Begintijd bijhouden. Voorbeeld: StartTime = "PT2.669S" |
| **Duur** |**xs: duration** |Duur bijhouden. Voorbeeld: Duur = 'PTSampleFormat M37.757S'. |

> [!NOTE]
> De volgende twee onderliggende elementen moeten worden weergegeven in een reeks.  
> 
> 

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Toestand**<br /><br /> minOccurs = "0" maxOccurs = '1' |[StreamDispositionType](media-services-input-metadata-schema.md#StreamDispositionType) |Bevat presentatie-informatie (bijvoorbeeld, of een bepaalde-nummer is voor een visuele handicap viewers). |
| **Metagegevens**<br /><br /> minOccurs = "0" maxOccurs = 'unbounded' |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Algemene sleutel/waarde-tekenreeksen die kunnen worden gebruikt voor een verscheidenheid aan informatie bevatten. Bijvoorbeeld, sleutel = 'taal' en waarde = 'eng'. |

## <a name="AudioTrackType"></a>AudioTrackType (neemt over van TrackType)
 **AudioTrackType** is globale complex type die eigenschappen van overneemt [TrackType](media-services-input-metadata-schema.md#TrackType).  

 Het type vertegenwoordigt een specifieke-nummer in het assetbestand.  

 Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **SampleFormat** |**xs:String** |Voorbeeld-indeling. |
| **ChannelLayout** |**xs: tekenreeks** |Lay-out van kanaal. |
| **Kanalen**<br /><br /> Vereist |**xs:int** |Aantal (0 of meer) audio kanalen. |
| **SamplingRate**<br /><br /> Vereist |**xs:int** |Samplefrequentie van audio in steekproeven per seconde of Hz. |
| **Bitrate** |**xs:int** |Gemiddelde audio bitsnelheid in bits per seconde berekend op basis van het assetbestand. De nettolading van de elementaire stroom wordt beschouwd en de overhead van het pakket is niet opgenomen in deze telling. |
| **Het BitsPerSample** |**xs:int** |Bits per voorbeeld voor de indeling wFormatTag typt. |

## <a name="VideoTrackType"></a>VideoTrackType (neemt over van TrackType)
**VideoTrackType** is globale complex type die eigenschappen van overneemt [TrackType](media-services-input-metadata-schema.md#TrackType).  

Het type vertegenwoordigt een specifieke video bijhouden in het assetbestand.  

Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Code**<br /><br /> Vereist |**xs:String** |Video-codec code code. |
| **Profiel** |**xs: tekenreeks** |Profiel van de video bijhouden. |
| **Niveau** |**xs: tekenreeks** |Niveau van de video bijhouden. |
| **PixelFormat** |**xs: tekenreeks** |Video bijhouden pixelindeling. |
| **Breedte**<br /><br /> Vereist |**xs:int** |Gecodeerde video breedte in pixels. |
| **Hoogte**<br /><br /> Vereist |**xs:int** |Gecodeerde video hoogte in pixels. |
| **DisplayAspectRatioNumerator**<br /><br /> Vereist |**xs: dubbele** |Beeldscherm hoogte-breedteverhouding teller. |
| **DisplayAspectRatioDenominator**<br /><br /> Vereist |**xs:Double** |Noemer voor de hoogte-breedteverhouding beeld weergegeven. |
| **DisplayAspectRatioDenominator**<br /><br /> Vereist |**xs: dubbele** |Video voorbeeld hoogte-breedteverhouding teller. |
| **SampleAspectRatioNumerator** |**xs: dubbele** |Video voorbeeld hoogte-breedteverhouding teller. |
| **SampleAspectRatioNumerator** |**xs:Double** |Noemer voor de hoogte-breedteverhouding video voorbeeld. |
| **Framesnelheid**<br /><br /> Vereist |**xs:decimal** |Framesnelheid van video .3f indeling gemeten. |
| **Bitrate** |**xs:int** |Gemiddelde video bitsnelheid in kilobits per seconde berekend op basis van het assetbestand. De nettolading van de elementaire stroom wordt beschouwd en de overhead van het pakket is niet opgenomen. |
| **MaxGOPBitrate** |**xs: int** |Gemiddelde bitrate Max GOP voor deze video spoor in kilobits per seconde. |
| **HasBFrames** |**xs:int** |Video bijhouden aantal B-frames. |

## <a name="MetadataType"></a>MetadataType
**MetadataType** is globale complex type die worden beschreven van metagegevens van een assetbestand als sleutel/waarde-tekenreeksen. Bijvoorbeeld, sleutel = 'taal' en waarde = 'eng'.  

Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **sleutel**<br /><br /> Vereist |**xs:String** |De sleutel in de sleutel-waardepaar. |
| **waarde**<br /><br /> Vereist |**xs:String** |De waarde in de sleutel-waardepaar. |

## <a name="ProgramType"></a>ProgramType
**ProgramType** is globale complex type die worden beschreven van een programma.  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **ProgramId**<br /><br /> Vereist |**xs:int** |Programma-Id |
| **NumberOfPrograms**<br /><br /> Vereist |**xs:int** |Het aantal programma's. |
| **PmtPid**<br /><br /> Vereist |**xs:int** |Programma kaart tabellen (PMTs) bevatten informatie over programma's.  Zie voor meer informatie [BET](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT). |
| **PcrPid**<br /><br /> Vereist |**xs: int** |Door de decoder gebruikt. Zie voor meer informatie [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR) |
| **StartPTS** |**xs: lang** |Presentatie tijdstempel wordt gestart. |
| **EndPTS** |**xs: lang** |Beëindigen presentatie tijdstempel. |

## <a name="StreamDispositionType"></a>StreamDispositionType
**StreamDispositionType** is globale complex type die de stroom beschrijft.  

Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Kenmerken
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Standaard**<br /><br /> Vereist |**xs: int** |Dit kenmerk instelt op 1 om aan te geven dat dit de standaardpresentatie. |
| **Dub**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dat dit de Nagesynchroniseerde presentatie. |
| **Origineel**<br /><br /> Vereist |**xs: int** |Dit kenmerk instelt op 1 om aan te geven dat dit de oorspronkelijke presentatie. |
| **Opmerking**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dat dit nummer commentaar bevat. |
| **Tekst**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dat dit nummer tekst bevat. |
| **Karaoke**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dat dit verwijst naar het nummer karaoke (achtergrondmuziek, geen zang). |
| **Gedwongen**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dat deze de geforceerde presentatie. |
| **HearingImpaired**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dit nummer voor de slechthorenden. |
| **VisualImpaired**<br /><br /> Vereist |**xs:int** |Dit kenmerk instelt op 1 om aan te geven dit nummer voor de visueel gehinderd. |
| **CleanEffects**<br /><br /> Vereist |**xs: int** |Dit kenmerk instelt op 1 om aan te geven dat dit nummer schone gevolgen heeft. |
| **AttachedPic**<br /><br /> Vereist |**xs: int** |Dit kenmerk instelt op 1 om aan te geven dit nummer afbeeldingen. |

## <a name="Programs"></a>Programma's-element
Wrapper-element dat bevat meerdere **programma** elementen.  

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **Programma**<br /><br /> minOccurs = "0" maxOccurs = 'unbounded' |[ProgramType](media-services-input-metadata-schema.md#ProgramType) |Bevat informatie over software in het assetbestand voor assetbestanden MPEG-TS-indeling. |

## <a name="VideoTracks"></a>VideoTracks element
 Wrapper-element dat bevat meerdere **VideoTrack** elementen.  

 Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="child-elements"></a>Onderliggende elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **VideoTrack**<br /><br /> minOccurs = "0" maxOccurs = 'unbounded' |[VideoTrackType (neemt over van TrackType)](media-services-input-metadata-schema.md#VideoTrackType) |Bevat informatie over video nummers in het assetbestand. |

## <a name="AudioTracks"></a>AudioTracks element
 Wrapper-element dat bevat meerdere **AudioTrack** elementen.  

 Een XML-voorbeeld aan het einde van dit artikel: [XML-voorbeeld](media-services-input-metadata-schema.md#xml).  

### <a name="elements"></a>Elementen
| Naam | Type | Beschrijving |
| --- | --- | --- |
| **AudioTrack**<br /><br /> minOccurs = "0" maxOccurs = 'unbounded' |[AudioTrackType (neemt over van TrackType)](media-services-input-metadata-schema.md#AudioTrackType) |Bevat informatie over audio nummers in het assetbestand. |

## <a name="code"></a>Schema-Code
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
        <xs:attribute name="Id" use="required">  
          <xs:annotation>  
            <xs:documentation>zero-based index of this video track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Width" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video width in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Height" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video height in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
              <xs:annotation>  
                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:decimal">  
                  <xs:minInclusive value="0"/>  
                  <xs:fractionDigits value="3"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="MaxGOPBitrate">  
              <xs:annotation>  
                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Channels" use="required">  
              <xs:annotation>  
                <xs:documentation>number of audio channels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SamplingRate" use="required">  
              <xs:annotation>  
                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for the encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is the collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>the media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of the asset file in kbps</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:int">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  


## <a name="xml"></a>XML-voorbeeld
Hier volgt een voorbeeld van het metagegevensbestand van de invoer.  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

