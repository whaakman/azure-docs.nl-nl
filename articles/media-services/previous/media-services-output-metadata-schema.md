---
title: Azure Media Services-metagegevens uitvoerschema | Microsoft Docs
description: Het onderwerp biedt een overzicht van Azure Media Services-uitvoerschema metagegevens.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 42227095c69924cd2922673d020b349aa29f2daa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61129743"
---
# <a name="output-metadata"></a>Uitvoermetagegevens
## <a name="overview"></a>Overzicht
Een coderingstaak is gekoppeld aan een invoeractivum (of activa) op die u wilt uitvoeren van sommige coderingstaken. Voorbeeld: Codeer een MP4-bestand afspelen van H.264 MP4 adaptieve bitrate sets; maken van een miniatuur; Maak overlays. Na voltooiing van een taak, wordt een uitvoerasset geproduceerd.  De uitvoerasset bevat video, audio, miniaturen, enzovoort. De uitvoerasset bevat ook een bestand met metagegevens over de uitvoerasset. De naam van het metagegevens-XML-bestand heeft de volgende indeling: &lt;source_file_name&gt;_manifest.xml (bijvoorbeeld BigBuckBunny_manifest.xml).  

Media Services wordt de invoer van de activa voor het genereren van metagegevens voorzorg niet gescand. Invoermetagegevens wordt alleen als een artefact als invoer die Asset wordt verwerkt in een taak gegenereerd. Dus dit artefact is geschreven naar de uitvoer Asset. Verschillende hulpprogramma's worden gebruikt voor het genereren van metagegevens voor invoer en uitvoer activa. De metagegevens van de invoer is daarom een iets ander schema dan de uitvoermetagegevens van de.

Als u controleren van het bestand met metagegevens wilt, kunt u een **SAS** locator en download het bestand op uw lokale computer.  

Dit artikel worden de elementen en typen van het XML-schema besproken waarop de metada uitvoer (&lt;source_file_name&gt;_manifest.xml) is gebaseerd. Zie voor meer informatie over het bestand met metagegevens over het invoeractivum Invoermetagegevens.  

U vindt de code van de volledige schema en de XML-voorbeeld aan het einde van dit artikel.  

## <a name="AssetFiles"></a> AssetFiles root-element
Verzameling van AssetFile vermeldingen voor de coderingstaak.  

### <a name="child-elements"></a>Onderliggende elementen
| Name | Description |
| --- | --- |
| **AssetFile**<br/><br/> minOccurs="0" maxOccurs="1" |Een element AssetFile die deel uitmaakt van de verzameling AssetFiles. |

## <a name="AssetFile"></a> AssetFile-element
U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **Naam**<br/><br/> Vereist |**xs:String** |De bestandsnaam voor de media-asset. |
| **Grootte**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:long** |Grootte van het assetbestand in bytes. |
| **Duur**<br/><br/> Vereist |**xs:duration** |Inhoud afspelen back duur. |

### <a name="child-elements"></a>Onderliggende elementen
| Name | Description |
| --- | --- |
| **Bronnen** |Verzameling van invoer/transcoderen, die is verwerkt om te kunnen deze AssetFile produceren. Zie voor meer informatie, bron-element. |
| **VideoTracks**<br/><br/> minOccurs="0" maxOccurs="1" |Elke fysieke AssetFile kan nul of meer video's in deze bevatten nummers interleaved in een indeling voor de juiste container. Zie voor meer informatie, VideoTracks element. |
| **AudioTracks**<br/><br/> minOccurs="0" maxOccurs="1" |Elke fysieke AssetFile kan nul of meer audionummers interleaved in een indeling voor de juiste container in het bevatten. Dit is een verzameling van deze audionummers. Zie voor meer informatie, AudioTracks element. |

## <a name="Sources"></a> Bronnen-element
Verzameling van invoer/transcoderen, die is verwerkt om te kunnen deze AssetFile produceren.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="child-elements"></a>Onderliggende elementen
| Name | Description |
| --- | --- |
| **Bron**<br/><br/> minOccurs="1" maxOccurs="unbounded" |Een invoer/bronbestand dat wordt gebruikt bij het genereren van deze asset. Zie voor meer informatie, bron-element. |

## <a name="Source"></a> Bronelement
Een invoer/bronbestand dat wordt gebruikt bij het genereren van deze asset.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **Naam**<br/><br/> Vereist |**xs:String** |De bestandsnaam van de invoerbron. |

## <a name="VideoTracks"></a> VideoTracks-element
Elke fysieke AssetFile kan nul of meer video's in deze bevatten nummers interleaved in een indeling voor de juiste container. De **VideoTracks** element vertegenwoordigt een verzameling van de video worden bijgehouden.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="child-elements"></a>Onderliggende elementen
| Name | Description |
| --- | --- |
| **VideoTrack**<br/><br/> minOccurs="1" maxOccurs="unbounded" |Een specifieke video in de bovenliggende AssetFile bijhouden. Zie voor meer informatie, VideoTrack element. |

## <a name="VideoTrack"></a> VideoTrack-element
Een specifieke video in de bovenliggende AssetFile bijhouden.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **Id**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Op nul gebaseerde index van deze video bijhouden. **Opmerking:**  Dit **Id** is niet noodzakelijkerwijs de TrackID zoals gebruikt in een MP4-bestand. |
| **FourCC**<br/><br/> Vereist |**xs:String** |Video-codec code code. |
| **Profiel** |**xs:String** |H264-profiel (alleen van toepassing op H264 codec). |
| **Niveau** |**xs:String** |H264-niveau (alleen van toepassing op H264 codec). |
| **Breedte**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Gecodeerde video breedte in pixels. |
| **Hoogte**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Gecodeerde video hoogte in pixels. |
| **DisplayAspectRatioNumerator**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:Double** |Beeldscherm hoogte-breedteverhouding teller. |
| **DisplayAspectRatioDenominator**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:Double** |Noemer voor het hoogte-breedteverhouding van de video weergeven. |
| **Framerate**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:decimal** |Framesnelheid van video in de indeling .3f gemeten. |
| **TargetFramerate**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:decimal** |Vooraf ingestelde doel video framesnelheid .3f-indeling. |
| **Bitrate**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Gemiddelde video bitsnelheid in kilobits per seconde, berekend op basis van de AssetFile. De nettolading van de elementaire stream geteld en bevat geen de overhead voor het verpakken. |
| **TargetBitrate**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Doel gemiddelde bitrate voor deze video bijhouden, zoals aangevraagd via de codering vooraf ingestelde in kilobits per seconde. |
| **MaxGOPBitrate**<br/><br/> minInclusive ="0" |**xs:int** |Max GOP gemiddelde bitrate voor deze video spoor in kilobits per seconde. |

## <a name="AudioTracks"></a> AudioTracks-element
Elke fysieke AssetFile kan nul of meer audionummers interleaved in een indeling voor de juiste container in het bevatten. De **AudioTracks** element vertegenwoordigt een verzameling van deze audionummers.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="child-elements"></a>Onderliggende elementen
| Name | Description |
| --- | --- |
| **AudioTrack**<br/><br/> minOccurs="1" maxOccurs="unbounded" |Een specifieke audiotrack in de bovenliggende AssetFile. Zie voor meer informatie, AudioTrack element. |

## <a name="AudioTrack"></a> AudioTrack-element
Een specifieke audiotrack in de bovenliggende AssetFile.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **Id**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Op nul gebaseerde index van deze audiotrack. **Opmerking:**  Dit is niet noodzakelijkerwijs de TrackID zoals gebruikt in een MP4-bestand. |
| **Codec** |**xs:String** |Audiotrack codec tekenreeks. |
| **EncoderVersion** |**xs:String** |Optionele encoder versietekenreeks, vereist voor EAC3. |
| **kanalen**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Het aantal audio-kanalen. |
| **SamplingRate**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Audio samplefrequentie in de voorbeelden per seconde of Hz. |
| **Bitrate**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Gemiddelde audio bitsnelheid in bits per seconde, berekend op basis van de AssetFile. De nettolading van de elementaire stream geteld en bevat geen de overhead voor het verpakken. |
| **BitsPerSample**<br/><br/> minInclusive ="0"<br/><br/> Vereist |**xs:int** |Bits per voorbeeld voor de indeling wFormatTag typt. |

### <a name="child-elements"></a>Onderliggende elementen
| Name | Description |
| --- | --- |
| **LoudnessMeteringResultParameters**<br/><br/> minOccurs="0" maxOccurs="1" |Volume softwarelicentiecontrole resultaat parameters. Zie voor meer informatie, LoudnessMeteringResultParameters-element. |

## <a name="LoudnessMeteringResultParameters"></a> LoudnessMeteringResultParameters-element
Volume softwarelicentiecontrole resultaat parameters.  

U vindt een voorbeeld van de XML- [XML-voorbeeld](#xml).  

### <a name="attributes"></a>Kenmerken
| Name | Type | Description |
| --- | --- | --- |
| **DPLMVersionInformation** |**xs:String** |**Dolby** professionele volume softwarelicentiecontrole development kitversie. |
| **DialogNormalization**<br/><br/> minInclusive="-31" maxInclusive="-1"<br/><br/> Vereist |**xs:int** |DialogNormalization die worden gegenereerd via DPLM, vereist wanneer LoudnessMetering is ingesteld |
| **IntegratedLoudness**<br/><br/> minInclusive="-70" maxInclusive="10"<br/><br/> Vereist |**xs:float** |Geïntegreerde volume |
| **IntegratedLoudnessUnit**<br/><br/> Vereist |**xs:String** |Geïntegreerde volume eenheid. |
| **IntegratedLoudnessGatingMethod**<br/><br/> Vereist |**xs:String** |ID beperken |
| **IntegratedLoudnessSpeechPercentage**<br/><br/> minInclusive ="0" maxInclusive="100" |**xs:float** |Spraakinhoud via het programma, als een percentage. |
| **SamplePeak**<br/><br/> Vereist |**xs:float** |Piek absolute Voorbeeldwaarde, sinds opnieuw instellen of sinds het laatste is uitgeschakeld, per kanaal.  Eenheden zijn dBFS. |
| **SamplePeakUnit**<br/><br/> fixed="dBFS"<br/><br/> Vereist |**xs:anySimpleType** |Voorbeeld piek-eenheid. |
| **TruePeak**<br/><br/> Vereist |**xs:float** |Maximale waar piekwaarde, aan de hand van ITU-R BS.1770-2, sinds het opnieuw instellen of omdat deze het laatst uitgeschakeld, per kanaal. Eenheden zijn dBTP. |
| **TruePeakUnit**<br/><br/> fixed="dBTP"<br/><br/> Vereist |**xs:anySimpleType** |De waarde True Piekeenheid. |

## <a name="schema-code"></a>Schema-Code
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
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
                  <xs:element name="Sources">  
                    <xs:annotation>  
                      <xs:documentation>Collection of input/source media files, that was processed in order to produce this AssetFile</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Source" minOccurs="1" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>An input/source file used when generating this asset</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Name" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>input source file name</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
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
                            <xs:attribute name="FourCC" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video codec FourCC code</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Profile" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 profile (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Level" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 level (only appliable for H264 codec)</xs:documentation>  
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
                            <xs:attribute name="Framerate" use="required">  
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
                            <xs:attribute name="TargetFramerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>preset target video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via the encoding preset, in kilobits per second</xs:documentation>  
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
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:sequence>  
                              <xs:element name="LoudnessMeteringResultParameters" minOccurs="0" maxOccurs="1">  
                                <xs:annotation>  
                                  <xs:documentation>Loudness Metering Result Parameters</xs:documentation>  
                                </xs:annotation>  
                                <xs:complexType>  
                                  <xs:attribute name="DPLMVersionInformation" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Dolby Professional Loudness Metering Development Kit Version</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="DialogNormalization" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation> DialogNormalization generated through DPLM, required when LoudnessMetering is set</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:int">  
                                        <xs:minInclusive value="-31"/>  
                                        <xs:maxInclusive value="-1"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudness" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation>Integrated loudness</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="-70"/>  
                                        <xs:maxInclusive value="10"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessUnit" use="required" type="xs:string">  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessGatingMethod" use="required" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Gating identifier</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessSpeechPercentage">  
                                    <xs:annotation>  
                                      <xs:documentation>Speech content over the program, as a percentage.</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="0"/>  
                                        <xs:maxInclusive value="100"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Peak absolute sample value, since reset or since it was last cleared, per channel.  Units are dBFS.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeakUnit" use="required" fixed="dBFS">  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Maximum True Peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.  Units are dBTP.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeakUnit" use="required" fixed="dBTP">  
                                  </xs:attribute>  
                                </xs:complexType>  
                              </xs:element>  
                            </xs:sequence>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Codec" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>audio track codec string</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="EncoderVersion" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>optional encoder version string, required for EAC3</xs:documentation>  
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
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
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
                <xs:attribute name="Duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:duration"/>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  



## <a name="xml"></a> XML-voorbeeld

Het volgende XML-bestand is een voorbeeld van het uitvoerbestand voor metagegevens.  

    <AssetFiles xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema"   
                xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata">  
      <AssetFile Name="BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4" Size="4646283" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.2" Width="1280" Height="720" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="4250" TargetBitrate="3400" MaxGOPBitrate="5514"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4" Size="3166728" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="2846" TargetBitrate="2250" MaxGOPBitrate="3630"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4" Size="2205095" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1932" TargetBitrate="1500" MaxGOPBitrate="2513"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4" Size="1508567" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1271" TargetBitrate="1000" MaxGOPBitrate="1527"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4" Size="1057155" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="843" TargetBitrate="650" MaxGOPBitrate="1086"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4" Size="699262" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="1.3" Width="320" Height="180" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="503" TargetBitrate="400" MaxGOPBitrate="661"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_96kbps.mp4" Size="166780" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_56kbps.mp4" Size="124576" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="53" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
