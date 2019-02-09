---
title: Azure mediaservices - Smooth Streaming-Protocol (MS-SSTR) amendement voor HEVC | Microsoft Docs
description: Deze specificatie beschrijving van het protocol en de indeling voor gefragmenteerde MP4 op basis van live streamen met HEVC in Azure Media Services. Dit is een wijziging in de documentatie van Smooth Streaming-protocol (MS-SSTR) om op te nemen van ondersteuning voor HEVC opnemen en streaming. Alleen de wijzigingen die zijn vereist voor het leveren van HEVC zijn opgegeven in dit artikel, met uitzondering van zijn '(geen wijzigen)' geeft aan dat voor een uitleg van alleen tekst gekopieerd.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: f27d85de-2cb8-4269-8eed-2efb566ca2c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: johndeu;
ms.openlocfilehash: 3261e84dcd3dee06071b0fed78b61e2959e3bbf9
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978762"
---
# <a name="smooth-streaming-protocol-ms-sstr-amendment-for-hevc-legacy"></a>Smooth Streaming-Protocol (MS-SSTR) amendement voor HEVC (verouderd)

## <a name="1-introduction"></a>1-Inleiding 

Dit artikel bevat gedetailleerde wijzigingen worden toegepast op de specificatie Smooth Streaming-Protocol [MS-SSTR] om in te schakelen Smooth Streaming van HEVC gecodeerde video. In deze specificatie beschrijven we alleen de wijzigingen die zijn vereist voor het leveren van de video codec HEVC. Het artikel volgt hetzelfde nummering schema als het [MS-SSTR]-specificatie. De lege koppen weergegeven in het artikel vindt u om te zien van de lezers hun positie in de [MS-SSTR]-specificatie.  '(Geen wijzigen)' geeft aan dat de tekst wordt gekopieerd alleen ter informatie.

Dit artikel bevat vereisten voor de signalen van HEVC video codec in een manifest Smooth Streaming technische implementatie en normatief verwijzingen worden bijgewerkt om te verwijzen naar de huidige MPEG-normen die HEVC en algemene versleuteling van HEVC vak zijn namen voor de ISO Base Media-indeling zijn bijgewerkt zodat deze consistent zijn met de meest recente specificaties. 

De specificatie voor waarnaar wordt verwezen, Smooth Streaming-Protocol [MS-SSTR] beschrijft de wire-indeling gebruikt voor het leveren van live en on-demand digitale media, zoals audio en video in de volgende manieren: van een coderingsprogramma met een webserver vanaf een server naar een andere server en van een de server naar een HTTP-client.
Het gebruik van een MPEG-4 ([[MPEG4-RA])](https://go.microsoft.com/fwlink/?LinkId=327787)-levering van de structuur op basis van gegevens via HTTP kan naadloze overschakelen in bijna realtime tussen verschillende kwaliteitsniveaus van gecomprimeerde media-inhoud. Het resultaat is een constante afspeelervaring voor de eindgebruiker van HTTP-client, zelfs als de netwerk- en videorendering voorwaarden voor de clientcomputer of het apparaat wijzigen.

## <a name="11-glossary"></a>1.1 verklarende woordenlijst 

De volgende voorwaarden zijn gedefinieerd in *[MS-GLOS]*:

>   **unieke id (GUID) universele, unieke id (UUID)**

De volgende voorwaarden zijn specifiek voor dit document:

>  **samenstelling tijd:** De tijd een voorbeeld dat wordt weergegeven op de client, zoals gedefinieerd in [[ISO/IEC-14496-12].](https://go.microsoft.com/fwlink/?LinkId=183695)

>   **CENC**: Algemene versleuteling als gedefinieerd in [ISO/IEC 23001-7] tweede editie.

>   **decoderen tijd:** De tijd een voorbeeld dat is vereist om te worden gedecodeerd op de client, zoals gedefinieerd in [[ISO/IEC http://go.microsoft.com/fwlink/?LinkId=18369514496-12].](https://go.microsoft.com/fwlink/?LinkId=183695)

**fragment:** Een afzonderlijk downloadbare eenheid **media** die bestaat uit een of meer **voorbeelden**.

>   **HEVC:** Maximale efficiëntie Video coderen, zoals gedefinieerd in [ISO/IEC 23008-2]

>   **manifest:** Metagegevens over de **presentatie** waarmee een client om aan te vragen voor **media**. **Media:** Gecomprimeerd audio, video en tekst gegevens die door de client wordt gebruikt om af te spelen een **presentatie**. **Media-indeling:** Een zorgvuldig gedefinieerde indeling voor de weergave van audio of video als een gecomprimeerd **voorbeeld**.

>   **presentatie:** De set met alle **streams** en verwante metagegevens die nodig zijn voor het afspelen van een enkele film. **Aanvraag:** Een HTTP-bericht verzonden vanaf de client naar de server, zoals gedefinieerd in [[RFC2616].](https://go.microsoft.com/fwlink/?LinkId=90372) **Antwoord:** Een HTTP-bericht verzonden vanaf de server aan de client, zoals gedefinieerd in [[RFC2616].](https://go.microsoft.com/fwlink/?LinkId=90372)

>   **Voorbeeld:** De kleinste fundamentele eenheid (zoals een frame) waarin **media** wordt opgeslagen en verwerkt.

>   **MAY, SHOULD, MUST, SHOULD NOT, MUST NOT:** Deze termen (in hoofdletters) worden gebruikt, zoals beschreven in [[RFC2119].](https://go.microsoft.com/fwlink/?LinkId=90317) Alle instructies voor het gebruik van verschijnsel dat ofwel kan SHOULD of niet moet.

## <a name="12-references"></a>1.2-verwijzingen

>   Verwijzingen naar de documentatie van Microsoft Open specificaties omvatten een publishing jaar omdat koppelingen naar de nieuwste versie van de documenten, waarop worden regelmatig bijgewerkt. Verwijzingen naar andere documenten bevatten een publishing jaar wanneer er een beschikbaar is.

### <a name="121-normative-references"></a>1.2.1 normatief verwijzingen 

>  [MS-SSTR] Smooth Streaming-Protocol *v20140502* [https://msdn.microsoft.com/library/ff469518.aspx](https://msdn.microsoft.com/library/ff469518.aspx)

>   [ISO/IEC 14496-12] Internationale organisatie voor standaardisatie, ' Information technology--coderen van audiovisuele objecten--deel 12: ISO Base Media bestandsindeling", ISO/IEC 14496-12:2014, 4-editie, Plus rectificatie van 1, wijzigingen van 1 en 2.
>   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c061988_ISO_IEC_14496-12_2012.zip>

>   [ISO/IEC 14496-15] Internationale organisatie voor standaardisatie, ' Information technology--coderen van audiovisuele objecten--onderdeel 15: Vervoer van NAL eenheid gestructureerd video in de bestandsindeling van ISO Base Media", ISO 14496-15:2015, versie 3.
>   <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=65216>

>   [ISO/IEC 23008-2] Informatietechnologie--hoge efficiëntie programmeren en media levering in een heterogene omgeving--deel 2: Maximale efficiëntie video coderen: 2013 of de nieuwste versie   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c035424_ISO_IEC_23008-2_2013.zip>

>   [ISO/IEC 23001-7] IT-technologieën voor MPEG-systemen, deel 7: Algemene versleuteling in ISO base bestand-indeling mediabestanden, CENC Edition 2:2015 <http://www.iso.org/iso/catalogue_detail.htm?csnumber=65271>

>   [RFC-6381] IETF RFC-6381, 'de 'Codecs' en 'Profielen'-Parameters voor 'Bucket"mediatypen" <http://tools.ietf.org/html/rfc6381>

>   [MPEG4-RA] De MP4-statusregistratie-instantie, "MP4REG" [http://www.mp4ra.org   ](https://go.microsoft.com/fwlink/?LinkId=327787)

>   [RFC2119] Bradner, S., 'sleutel woorden voor gebruik in RFC's aangeven vereiste niveaus', BCP 14, RFC 2119, maart 1997   [http://www.rfc-editor.org/rfc/rfc2119.txt   ](https://go.microsoft.com/fwlink/?LinkId=90317)

### <a name="122-informative-references"></a>1.2.2 informatieve referenties 

>   [MS-GLOS] Microsoft Corporation, "*Windows protocollen Master verklarende woordenlijst*."

>   [RFC3548] Josefsson, S., editie, 'De Base16, Base32 en Base64 gegevens coderingen', RFC 3548, juli 2003 [http://www.ietf.org/rfc/rfc3548.txt   ](https://go.microsoft.com/fwlink/?LinkId=90432)

>   [RFC5234] Crocker, D., editie, en Overell, s., "BNF uitgebreid voor de syntaxis van de specificaties: ABNF", STD 68, RFC 5234, January 2008,   [http://www.rfc-editor.org/rfc/rfc5234.txt   ](https://go.microsoft.com/fwlink/?LinkId=123096)


## <a name="13-overview"></a>1.3-overzicht 

>   Alleen de wijzigingen aan de specificatie Smooth Streaming is vereist voor de levering van HEVC zijn hieronder opgegeven. Ongewijzigde sectiekoppen worden weergegeven voor het onderhouden van locatie in de waarnaar wordt verwezen, Smooth Streaming-specificatie [MS-SSTR].

## <a name="14-relationship-to-other-protocols"></a>1.4-relatie met andere protocollen 

## <a name="15-prerequisitespreconditions"></a>1.5 vereisten/voorwaarden 

## <a name="16-applicability-statement"></a>1.6 toepasselijkheid instructie 

## <a name="17-versioning-and-capability-negotiation"></a>1.7 versiebeheer en mogelijkheid onderhandeling 

## <a name="18-vendor-extensible-fields"></a>1.8 leverancier Extensible velden 

>   De volgende methode moet worden gebruikt identificeren stromen met behulp van de HEVC video-indeling:

>   * **Aangepaste beschrijvende-Codes voor Media-indelingen:** Deze mogelijkheid wordt geboden door de **code** veld, zoals opgegeven in de sectie *2.2.2.5*.
>   Uitvoerders zorgt ervoor dat extensies niet in strijd met de extensie codes registreren met de MPEG4-RA, zoals opgegeven in [[ISO/IEC-14496-12] ](https://go.microsoft.com/fwlink/?LinkId=183695)

## <a name="19-standards-assignments"></a>1.9 standaarden toewijzingen 

## <a name="2-messages"></a>2 berichten 

## <a name="21-transport"></a>2.1-transport

## <a name="22-message-syntax"></a>2.2 message-syntaxis 

### <a name="221-manifest-request"></a>2.2.1 manifest van de aanvraag 

### <a name="222-manifest-response"></a>2.2.2 manifest van de reactie 

#### <a name="2221-smoothstreamingmedia"></a>2.2.2.1 SmoothStreamingMedia 

>   **Subversie (variabele):** De secundaire versie van het Manifest-antwoordbericht. MOET worden ingesteld op 2. (Geen wijzigen)

>   **Tijdschaal (variabele):** De tijdschaal van het kenmerk duur, opgegeven als het aantal stappen in één seconde. De standaardwaarde is
>   10000000. (Geen wijzigen)

>   De aanbevolen waarde is 90000 voor weergave van de exacte duur van videoframes en fragmenten met decimale framesnelheid video (bijvoorbeeld: 30/1.001 Hz).

#### <a name="2222-protectionelement"></a>2.2.2.2 ProtectionElement 

De ProtectionElement moeten worden gebruikt als algemene versleuteling (CENC) is toegepast op de video-of audiostromen. Streams HEVC versleuteld moeten voldoen aan Common Encryption 2e Edition [ISO/IEC 23001-7]. Alleen segment gegevens VCL NAL eenheden moeten worden versleuteld.

#### <a name="2223-streamelement"></a>2.2.2.3 StreamElement 

>   **StreamTimeScale (variabele):** De tijdschaal voor de duur- en tijdwaarden in deze stroom, opgegeven als het aantal stappen in één seconde. Een waarde van 90000 wordt aanbevolen voor HEVC streams. Een waarde die overeenkomt met de samplefrequentie Wave (bijvoorbeeld 48000 of 44100) wordt aanbevolen voor audiostreams.

##### <a name="22231-streamprotectionelement"></a>2.2.2.3.1 StreamProtectionElement

#### <a name="2224-urlpattern"></a>2.2.2.4 UrlPattern 

#### <a name="225-trackelement"></a>2.2.5 TrackElement 

>   **Code (variabele):** Een code van vier tekens die welke media-indeling aangeeft wordt gebruikt voor elk voorbeeld. Het volgende bereik van waarden wordt gereserveerd met de volgende functionaliteit voor semantische betekenis:

>  * 'hev1': Video-voorbeelden voor deze bijhouden gebruik HEVC video, met behulp van de 'hev1' voorbeeld van de indeling van de beschrijving is opgegeven in [ISO/IEC-14496-15].

>   **CodecPrivateData (variabele):** Gegevens die Hiermee geeft u parameters die specifiek zijn voor de media-indeling en voor alle voorbeelden in het spoor weergegeven als een reeks van hex-vastgelegde bytes. De indeling en semantische betekenis van de reeks bytes is afhankelijk van de waarde van de **code** veld als volgt:

>   * Als een TrackElement HEVC video, beschrijft de **code** veld moet gelijk zijn aan **"hev1"** en;

>   De **CodecPrivateData** veld bevat een hexadecimaal gecodeerde tekenreeksweergave van de volgende byte-reeks opgegeven in ABNF [[RFC5234]:](https://go.microsoft.com/fwlink/?LinkId=123096) (geen wijziging van MS-SSTR)

>   * %x00 %x00 %x00 %x01 SPSField %x00 %x00 %x00 %x01 PPSField

>   * SPSField bevat de Takenreeks Parameter ingesteld (SP's).

>   * PPSField bevat de segment Parameter ingesteld (PPS).

>   Opmerking: De Video Parameter ingesteld (directeuren) niet is opgenomen in CodecPrivateData, maar moeten worden opgenomen in de bestandsheader van opgeslagen bestanden in het vak 'hvcC'. Systemen met Smooth Streaming-Protocol moeten geven aanvullende decoderen parameters (bijvoorbeeld HEVC laag) met behulp van het aangepaste kenmerk "codecs."

##### <a name="22251-customattributeselement"></a>2.2.2.5.1 CustomAttributesElement 

#### <a name="226-streamfragmentelement"></a>2.2.6 StreamFragmentElement 

>   De **van SmoothStreamingMedia MajorVersion** veld moet worden ingesteld op 2, en **subversie** veld moet worden ingesteld op 2. (Geen wijzigen)

##### <a name="22261-trackfragmentelement"></a>2.2.2.6.1 TrackFragmentElement 

### <a name="223-fragment-request"></a>2.2.3 fragment aanvraag 

>   **Opmerking**: De standaardindeling voor media aangevraagd voor **subversie** 2 en 'hev1' is 'iso8' merk ISO Base Media bestandsindeling opgegeven in [ISO/IEC 14496-12] ISO Base Media bestand-indeling vierde editie, en algemene versleuteling-tweede [ISO/IEC 23001-7] Editie.

### <a name="224-fragment-response"></a>2.2.4 fragment antwoord 

#### <a name="2241-moofbox"></a>2.2.4.1 MoofBox 

#### <a name="2242-mfhdbox"></a>2.2.4.2 MfhdBox 

#### <a name="2243-trafbox"></a>2.2.4.3 TrafBox 

#### <a name="2244-tfxdbox"></a>2.2.4.4 TfxdBox 

>   De **TfxdBox** is afgeschaft en zijn vervangen door het spoor Fragment decoderen vak tijd (tfdt) opgegeven in de sectie [ISO/IEC 14496-12] 8.8.12 functie.

>   **Opmerking**: Een client kan de duur van een fragment berekenen door op te tellen van de duur van de steekproef die worden vermeld in het vak in het spoor uitvoeren (trun) of de standaardduur van de steekproef vermenigvuldigen met het aantal steekproeven van tijden. De baseMediaDecodeTime in 'tfdt' plus fragment duur is gelijk aan de URL-parameter voor tijd voor het volgende fragment.

>   Een vak producent verwijzing tijd (prft) moet worden ingevoegd voordat u het Fragment film (moof), indien nodig, om aan te geven van de UTC-tijd overeenkomt met de Track Fragment decoderen tijd van het eerste voorbeeld waarnaar wordt verwezen door het Fragment film, als die is opgegeven in [ISO/IEC 14496 -12] sectie 8.16.5.

#### <a name="2245-tfrfbox"></a>2.2.4.5 TfrfBox 

>   De **TfrfBox** is afgeschaft en zijn vervangen door het spoor Fragment decoderen vak tijd (tfdt) opgegeven in de sectie [ISO/IEC 14496-12] 8.8.12 functie.

>   **Opmerking**: Een client kan de duur van een fragment berekenen door op te tellen van de duur van de steekproef die worden vermeld in het vak in het spoor uitvoeren (trun) of de standaardduur van de steekproef vermenigvuldigen met het aantal steekproeven van tijden. De baseMediaDecodeTime in 'tfdt' plus fragment duur is gelijk aan de URL-parameter voor tijd voor het volgende fragment. Zoek verder adressen zijn afgeschaft omdat ze vertraging live streamen.

#### <a name="2246-tfhdbox"></a>2.2.4.6 TfhdBox 

>   De **TfhdBox** en verwante velden inkapselen standaardwaarden voor per voorbeeld metagegevens in het fragment. De syntaxis van de **TfhdBox** veld vormen een strikte subset van de syntaxis van het vak bijhouden Fragment koptekst is gedefinieerd in [[ISO/IEC-14496-12]](https://go.microsoft.com/fwlink/?LinkId=183695) 8.8.7 sectie.

>   **BaseDataOffset (8 bytes):** De verschuiving in bytes, vanaf het begin van de **MdatBox** veld naar de voorbeeldveld in de **MdatBox** veld. Vanwege deze beperking, moet de standaard-base-is-moof vlag (0x020000) worden ingesteld.

#### <a name="2247-trunbox"></a>2.2.4.7 TrunBox 

>   De **TrunBox** en verwante velden inkapselen per voorbeeld metagegevens voor de aangevraagde fragment. De syntaxis van **TrunBox** vormen een strikte subset van de versie 1 bijhouden Fragment uitvoeren in gedefinieerd in [[ISO/IEC-14496-](https://go.microsoft.com/fwlink/?LinkId=183695)*12]* 8.8.8 sectie.

>   **SampleCompositionTimeOffset (4-bytes):** Het voorbeeld samenstelling Time-offset van elk voorbeeld aangepast, zodat de presentatietijd van de eerste aangeboden voorbeeld in het fragment gelijk aan de tijd decoderen van de eerste gedecodeerde voorbeeld is. Negatieve video voorbeeld samenstelling verschuivingen moeten worden gebruikt

>   zoals gedefinieerd in [[ISO/IEC-14496-12].](https://go.microsoft.com/fwlink/?LinkId=183695)

>   Opmerking: Dit voorkomt een video synchronisatiefout wordt veroorzaakt door video, audio die gelijk is aan de grootste gedecodeerde afbeelding buffer verwijdering vertraging vertraging en presentatie timing tussen alternatieve fragmenten dat mogelijk verschillende verwijdering vertragingen onderhoudt.

>   De syntaxis van de velden die zijn gedefinieerd in deze sectie, opgegeven in ABNF [[RFC5234]](https://go.microsoft.com/fwlink/?LinkId=123096) blijft hetzelfde, behalve als volgt:

>   SampleCompositionTimeOffset SIGNED_INT32 =

#### <a name="2248-mdatbox"></a>2.2.4.8 MdatBox 

#### <a name="2249-fragment-response-common-fields"></a>2.2.4.9 fragment antwoord algemene velden 

### <a name="225-sparse-stream-pointer"></a>2.2.5 sparse Stream aanwijzer 

### <a name="226-fragment-not-yet-available"></a>2.2.6 fragment nog niet beschikbaar 

### <a name="227-live-ingest"></a>2.2.7 Live Ingest 

#### <a name="2271-filetype"></a>2.2.7.1 FileType 

>   **Bestandstype (variabele):** Hiermee geeft u het subtype en beoogde gebruik van de MPEG-4 ([[MPEG4-RA])](https://go.microsoft.com/fwlink/?LinkId=327787) bestands- en op hoog niveau kenmerken.

>   **MajorBrand (variabele):** Het belangrijkste merk van het mediabestand. MOET worden ingesteld op "isml."

>   **Subversie (variabele):** De secundaire versie van het mediabestand. MOET worden ingesteld op 1.

>   **CompatibleBrands (variabele):** Hiermee geeft u de ondersteunde merken van MPEG-4.
>   MOET bevatten "ccff" en "iso8."

>   De syntaxis van de velden die zijn gedefinieerd in deze sectie, opgegeven in ABNF [[RFC5234]](https://go.microsoft.com/fwlink/?LinkId=123096) is als volgt:

    FileType = MajorBrand MinorVersion CompatibleBrands
    MajorBrand = STRING_UINT32
    MinorVersion = STRING_UINT32
    CompatibleBrands = "ccff" "iso8" 0\*(STRING_UINT32)

**Opmerking**: De compatibiliteit merken 'ccff' en 'iso8' aangeven dat fragmenten aan de 'Algemene Container bestand-indeling' en algemene versleuteling [ISO/IEC 23001-7] en ISO Base Media bestand indeling editie 4 [ISO/IEC 14496-12 voldoen].

#### <a name="2272-streammanifestbox"></a>2.2.7.2 StreamManifestBox 

##### <a name="22721-streamsmil"></a>2.2.7.2.1 StreamSMIL 

#### <a name="2273-liveservermanifestbox"></a>2.2.7.3 LiveServerManifestBox 

##### <a name="22731-livesmil"></a>2.2.7.3.1 LiveSMIL 

#### <a name="2274-moovbox"></a>2.2.7.4 MoovBox 

#### <a name="2275-fragment"></a>2.2.7.5 fragment 

##### <a name="22751-track-fragment-extended-header"></a>2.2.7.5.1 bijhouden Fragment berekende koptekst 

### <a name="228-server-to-server-ingest"></a>2.2.8 server-naar-Server opnemen 

## <a name="3-protocol-details"></a>Gegevens van 3-protocol 


## <a name="31-client-details"></a>3.1 Clientdetails 

### <a name="311-abstract-data-model"></a>3.1.1 abstract gegevensmodel 

#### <a name="3111-presentation-description"></a>3.1.1.1 presentatie beschrijving 

>   Het gegevenselement presentatiebeschrijving bevat alle metagegevens voor de presentatie.

>   De metagegevens van de presentatie: Een set van metagegevens die geldt voor alle stromen in de presentatie. De metagegevens van de presentatie omvat de volgende velden, opgegeven in de sectie *2.2.2.1*:

>   * **MajorVersion**
>   * **MinorVersion**
>   * **TimeScale**
>   * **Duur**
>   * **IsLive**
>   * **LookaheadCount**
>   * **DVRWindowLength**

>   Presentaties met HEVC Streams wordt ingesteld:

    MajorVersion = 2
    MinorVersion = 2

>   LookaheadCount = 0 (Opmerking: Vakken afgeschaft)

>   Presentaties moeten ook worden ingesteld:

    TimeScale = 90000

>   Stream-verzameling: Een verzameling van Stream beschrijving gegevenselementen, zoals aangegeven in punt *3.1.1.1.2*.

>   Beschrijving van de beveiliging: Een verzameling van metagegevens voor beveiliging systeembeschrijving gegevenselementen, zoals aangegeven in punt *3.1.1.1.1*.

##### <a name="31111-protection-system-metadata-description"></a>3.1.1.1.1 Protection System Metadata Description 

>   Het gegevenselement Protection metagegevens systeembeschrijving ingekapseld metagegevens die specifiek zijn voor een enkele Content Protection-systeem. (Geen wijzigen)

>   Beschrijving van de koptekst Protection: Metagegevens van de beveiliging van inhoud die betrekking hebben op een enkel Content Protection-systeem. Beschrijving van de koptekst Protection omvat de volgende velden, opgegeven in de sectie *2.2.2.2*:

>   * **Systeem-id**
>   * **ProtectionHeaderContent**

##### <a name="31112-stream-description"></a>3.1.1.1.2 Stream Description 

###### <a name="311121-track-description"></a>3.1.1.1.2.1 Beschrijving van bijhouden 

###### <a name="3111211-custom-attribute-description"></a>3.1.1.1.2.1.1 Beschrijving van aangepaste kenmerk 

##### <a name="3113-fragment-reference-description"></a>3.1.1.3 beschrijving van fragment verwijzing 

###### <a name="31131-track-specific-fragment-reference-description"></a>3.1.1.3.1 Beschrijving van bijhouden-specifieke Fragment verwijzing 

#### <a name="3112-fragment-description"></a>3.1.1.2 Fragment Description 

##### <a name="31121-sample-description"></a>3.1.1.2.1 Steekproefbeschrijving 

### <a name="312-timers"></a>3.1.2 timers 

### <a name="313-initialization"></a>3.1.3 initialisatie van 

### <a name="314-higher-layer-triggered-events"></a>3.1.4 hogere laag geactiveerd gebeurtenissen 

#### <a name="3141-open-presentation"></a>3.1.4.1 presentatie openen 

#### <a name="3142-get-fragment"></a>3.1.4.2 Get Fragment 

#### <a name="3143-close-presentation"></a>3.1.4.3 sluiten presentatie 

### <a name="315-processing-events-and-sequencing-rules"></a>3.1.5 verwerking van gebeurtenissen en regels voor sequentiëren 

#### <a name="3151-manifest-request-and-manifest-response"></a>3.1.5.1 manifest van de aanvraag en antwoord Manifest 

#### <a name="3152-fragment-request-and-fragment-response"></a>3.1.5.2 fragment aanvraag en antwoord Fragment

## <a name="32-server-details"></a>3.2-server-gegevens

## <a name="33-live-encoder-details"></a>3.3 live coderingsprogramma Details 

## <a name="4-protocol-examples"></a>4 protocol-voorbeelden 

## <a name="5-security"></a>5-beveiliging 

## <a name="51-security-considerations-for-implementers"></a>5.1 beveiligingsoverwegingen voor Implementers

>   Als de inhoud getransporteerd met behulp van dit protocol commerciële waardevolle heeft, moet een systeem van de bescherming van inhoud worden gebruikt om te voorkomen dat onbevoegd gebruik van de inhoud. De **ProtectionElement** kan worden gebruikt bij het uitvoeren van metagegevens die betrekking hebben op het gebruik van een Content Protection-systeem. Beveiligde audio en video-inhoud moeten worden versleuteld conform MPEG algemene versleuteling tweede editie: 2015 [ISO/IEC 23001-7].

>   **Opmerking**: Voor HEVC video, worden alleen de gegevens in VCL NALs segment versleuteld. Segment-headers en andere NALs zijn toegankelijk voor presentatietoepassingen voordat ze worden ontsleuteld. versleutelde gegevens zijn niet beschikbaar voor presentatietoepassingen in een beveiligde video pad.

## <a name="52-index-of-security-parameters"></a>5.2-Index van beveiligingsparameters 


| **Beveiligingsparameter**  | **Section**         |
|-------------------------|---------------------|
| ProtectionElement       | *2.2.2.2*           |
| Algemene versleuteling vakken | *[ISO/IEC 23001-7]* |

## <a name="53-common-encryption-boxes"></a>5.3 algemene versleuteling vakken

De volgende vakken in fragment antwoorden aanwezig mag zijn als algemene versleuteling wordt toegepast, en zijn opgegeven in [ISO/IEC 23001-7] of [ISO/IEC 14496-12]:

1.  Beveiliging System-vak voor specifieke koptekst (pssh)

2.  In het voorbeeld-versleuteling (senc)

3.  Voorbeeld van aanvullende informatievak verschuiving (saio)

4.  Aanvullende informatie grootte vak Voorbeeld (saiz)

5.  Groepsvak met beschrijving van voorbeeld (sgpd)

6.  Voorbeeld van groepsvak (sbgp)

-----------------------

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
