---
title: Azure mediaservices - Smooth Streaming-Protocol (MS-SSTR) wijziging voor HEVC | Microsoft Docs
description: Deze specificatie beschrijft het protocol en de indeling voor gefragmenteerde MP4 gebaseerde live streamen met HEVC in Azure Media Services. Dit is een wijziging van de documentaiton voor Smooth Streaming-protocol (MS-SSTR) voor onder meer ondersteuning voor HEVC opnemen en streaming. Alleen de wijzigingen die zijn vereist voor het leveren van HEVC zijn opgegeven in dit artikel, behalve zijn '(geen wijzigen)' geeft aan tekst voor alleen informatie wordt gekopieerd.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: ''
ms.assetid: f27d85de-2cb8-4269-8eed-2efb566ca2c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: johndeu;
ms.openlocfilehash: 7b4e1d933040225445e83d6e507cdee29a8f2363
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="smooth-streaming-protocol-ms-sstr-amendment-for-hevc"></a>Smooth Streaming Protocol (MS-SSTR) wijziging voor HEVC

## <a name="1-introduction"></a>1 introduction 

In dit artikel biedt gedetailleerde wijzigingen worden toegepast op de specificatie Smooth Streaming Protocol [MS-SSTR] Smooth Streaming van HEVC gecodeerde video inschakelen. In deze specificatie overzicht we alleen de wijzigingen die zijn vereist voor het leveren van de video HEVC-codec. Het artikel volgt hetzelfde nummering schema als de specificatie [MS-SSTR]. De lege koppen weergegeven in het artikel zijn bedoeld om de lezer hun positie in de specificatie [MS-SSTR] plaatsen.  '(Geen wijzigen)' geeft de tekst wordt gekopieerd alleen ter informatie.

Het artikel bevat de vereisten voor de signalering van video codec HEVC in een manifest Smooth Streaming technische implementatie en normatieve verwijzingen worden bijgewerkt om te verwijzen naar de huidige MPEG-normen die HEVC en algemene versleuteling HEVC vak zijn namen voor ISO Base Media-bestandsindeling zijn consistent met de meest recente specificaties bijgewerkt. 

De specificatie voor waarnaar wordt verwezen Smooth Streaming Protocol [MS-SSTR] beschrijft de draadindeling die wordt gebruikt voor het afleveren van live en on-demand digitaal medium, zoals audio en video op de volgende wijzen: vanaf een coderingsprogramma met een webserver vanaf een server naar een andere server en van een de server op een HTTP-client.
Het gebruik van een MPEG-4 ([[MPEG4 RA])](http://go.microsoft.com/fwlink/?LinkId=327787)-gebaseerde gegevens structuur levering via HTTP kunt naadloze overschakelen in bijna realtime tussen verschillende kwaliteitsniveaus van gecomprimeerde media-inhoud. Het resultaat is een constante afspelen ervaring voor de eindgebruiker van HTTP-client, zelfs als de netwerk- en video rendering voorwaarden voor de clientcomputer of het apparaat wijzigen.

## <a name="11-glossary"></a>1.1 verklarende woordenlijst 

De volgende voorwaarden zijn gedefinieerd in *[MS-GLOS]*:

>   **(UUID) globally unique identifier (GUID) universally unique identifier**

De volgende voorwaarden zijn specifiek voor dit document:

>  **samenstelling tijd:** de tijd een steekproef wordt weergegeven op de client, zoals gedefinieerd in [[ISO/IEC-14496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

>   **CENC**: Common Encryption, zoals gedefinieerd in de tweede editie [ISO/IEC 23001-7].

>   **tijd worden gedecodeerd:** de tijd een voorbeeld is vereist om te worden gedecodeerd op de client, zoals gedefinieerd in [[ISO/IEChttp://go.microsoft.com/fwlink/?LinkId=18369514496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

**fragment:** een afzonderlijk downloadbare eenheid **media** die bestaat uit een of meer **voorbeelden**.

>   **HEVC:** hoge efficiëntie Video coderen, zoals gedefinieerd in [ISO/IEC 23008-2]

>   **manifest:** metagegevens over de **presentatie** waarmee een client om aanvragen voor **media**. **Media:** gecomprimeerd audio-, video- en gegevens die door de client gebruikt om af te spelen een **presentatie**. **Media-indeling:** een goed gedefinieerde indeling voor de weergave van audio- of als een gecomprimeerd **voorbeeld**.

>   **presentatie:** de set van alle **streams** en verwante metagegevens die nodig zijn voor een enkele film afspelen. **aanvraag:** een HTTP-bericht verzonden vanuit de client naar de server, zoals gedefinieerd in [[RFC2616].](http://go.microsoft.com/fwlink/?LinkId=90372) **antwoord:** een HTTP-bericht verzonden van de server naar de client, zoals gedefinieerd in [[RFC2616].](http://go.microsoft.com/fwlink/?LinkId=90372)

>   **Voorbeeld:** de kleinste fundamentele eenheid (zoals een frame) waarin **media** is opgeslagen en verwerkt.

>   **KUNNEN SHOULD, moet, mogen niet zo is, moet:** deze termen (in hoofdletters) worden gebruikt, zoals beschreven in [[RFC2119].](http://go.microsoft.com/fwlink/?LinkId=90317) Alle instructies van verschijnsel Gebruik ofwel kan SHOULD of mag geen.

## <a name="12-references"></a>1.2-verwijzingen 
-----------

>   Verwijzingen naar de documentatie van de Microsoft Open-specificaties bevatten geen een publishing jaar omdat koppelingen naar de nieuwste versie van de documenten die vaak worden gewijzigd. Verwijzingen naar andere documenten bevatten een publishing jaar wanneer een beschikbaar is.

 ### <a name="121-normative-references"></a>1.2.1 normatieve verwijzingen 

>  [MS-SSTR] Smooth Streaming Protocol *v20140502* [http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/[MS-SSTR].pdf](http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

>   [ISO/IEC 14496 12] International Organization for Standardization, '--coderen van audio-visuele objecten--informatietechnologie onderdeel 12: Base Media ISO-bestandsindeling ', ISO/IEC 14496-12:2014, versie 4, Plus rectificatie 1, wijzigingen van-1 en 2.
>   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c061988_ISO_IEC_14496-12_2012.zip>

>   [ISO/IEC 14496-15] International Organization for Standardization, '--coderen van audio-visuele objecten--informatietechnologie onderdeel 15: vervoer van NAL eenheid gestructureerd video in de Base Media ISO-bestandsindeling ', ISO 14496-15:2015, Edition 3.
>   <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=65216>

>   [ISO/IEC 23008-2] Informatietechnologie--hoge efficiëntie coderen en media leveren in een heterogene omgeving--deel 2: hoge efficiëntie video coderen: 2013 of de nieuwste versie   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c035424_ISO_IEC_23008-2_2013.zip>

>   [ISO/IEC 23001-7] Informatietechnologie: MPEG systemen technologieën: deel 7: Common encryption in ISO-bestand-indeling mediabestanden CENC Edition 2:2015 baseren <http://www.iso.org/iso/catalogue_detail.htm?csnumber=65271>

>   [RFC 6381] IETF RFC-6381 'de 'Codecs' en 'Profielen'-Parameters voor 'Bucket' mediatypen' <http://tools.ietf.org/html/rfc6381>

>   [MPEG4 RA] De MP4-registratieautoriteit 'MP4REG' [http://www.mp4ra.org   ](http://go.microsoft.com/fwlink/?LinkId=327787)

>   [RFC2119] Bradner, S., 'sleutel woorden voor gebruik in RFC's aangeven vereiste niveaus', BCP 14, RFC 2119, maart 1997   [http://www.rfc-editor.org/rfc/rfc2119.txt   ](http://go.microsoft.com/fwlink/?LinkId=90317)

### <a name="122-informative-references"></a>1.2.2 informatieve verwijzingen 

>   [MS-GLOS] Microsoft Corporation "*Windows protocollen Master verklarende woordenlijst*."

>   [RFC3548] Josefsson, S., editie, 'De Base16, Base32 en Base64-gegevens coderingen', RFC 3548, juli 2003 [http://www.ietf.org/rfc/rfc3548.txt   ](http://go.microsoft.com/fwlink/?LinkId=90432)

>   [RFC5234] Crocker, D., editie, en Overell, P., ' BNF voor syntaxis specificaties uitgebreid: ABNF ', STD 68 RFC 5234, januari 2008   [http://www.rfc-editor.org/rfc/rfc5234.txt   ](http://go.microsoft.com/fwlink/?LinkId=123096)


## <a name="13-overview"></a>1.3-overzicht 
---------

>   Alleen de wijzigingen aan de specificatie Smooth Streaming is vereist voor de levering van HEVC zijn hieronder opgegeven. Ongewijzigde sectieheaders worden weergegeven voor het onderhouden van de locatie in de waarnaar wordt verwezen door de Smooth Streaming specificatie [MS-SSTR].

## <a name="14-relationship-to-other-protocols"></a>1.4-relatie met andere protocollen 
--------------------------------

## <a name="15-prerequisitespreconditions"></a>1.5 vereisten/voorwaarden 
----------------------------

## <a name="16-applicability-statement"></a>1.6 toepasselijkheid instructie 
------------------------

## <a name="17-versioning-and-capability-negotiation"></a>1.7 versiebeheer en mogelijkheid onderhandeling 
--------------------------------------

## <a name="18-vendor-extensible-fields"></a>1.8 leverancier Extensible velden 
-------------------------

>   De volgende methode worden gebruikt met behulp van de video HEVC-indeling streams identificeren:

>   * **Aangepaste beschrijvende Codes voor Media-indelingen:** deze mogelijkheid wordt geleverd door de **code** veld, zoals opgegeven in de sectie *2.2.2.5*.
>   Implementeerders kunnen ervoor zorgen dat extensies niet in strijd met extensie codes registreren met de MPEG4-RA, zoals opgegeven in [[ISO/IEC-14496-12] ](http://go.microsoft.com/fwlink/?LinkId=183695)

## <a name="19-standards-assignments"></a>1.9 standaarden toewijzingen 
----------------------

# <a name="2-messages"></a>2 berichten 

## <a name="21-transport"></a>2.1-transport 
----------

## <a name="22-message-syntax"></a>2.2 bericht syntaxis 
---------------

### <a name="221-manifest-request"></a>2.2.1 manifest aanvraag 

### <a name="222-manifest-response"></a>2.2.2 manifest antwoord 

#### <a name="2221-smoothstreamingmedia"></a>2.2.2.1 SmoothStreamingMedia 

>   **MinorVersion (variabele):** secundaire versie van het Manifest Response-bericht. MOET worden ingesteld op 2. (Geen wijzigen)

>   **Tijdschaal (variabele):** de schaal van het kenmerk duur opgegeven als het aantal stappen in één seconde. De standaardwaarde is
>   10000000. (Geen wijzigen)

>   De aanbevolen waarde is 90000 voor het voorstellen van de exacte duur van video frames en fragmenten met breukdeel framesnelheid video (bijvoorbeeld: 30/1.001 Hz).

#### <a name="2222-protectionelement"></a>2.2.2.2 ProtectionElement 

De ProtectionElement bedraagt aanwezig wanneer Common Encryption (CENC) is toegepast op video of audio stromen. HEVC versleuteld streams moeten overeenstemmen met Common Encryption 2e editie [ISO/IEC 23001-7]. Alleen segment gegevens in VCL NAL eenheden moeten worden versleuteld.

#### <a name="2223-streamelement"></a>2.2.2.3 StreamElement 

>   **StreamTimeScale (variabele):** de schaal voor de duur en tijd in deze stroom opgegeven als het aantal stappen in één seconde. Een waarde van 90000 wordt aanbevolen voor HEVC stromen. Een waarde die overeenkomt met de samplefrequentie Wave (bijvoorbeeld 48000 of 44100) wordt aanbevolen voor audio stromen.

##### <a name="22231-streamprotectionelement"></a>2.2.2.3.1 StreamProtectionElement

#### <a name="2224-urlpattern"></a>2.2.2.4 UrlPattern 

#### <a name="225-trackelement"></a>2.2.5 TrackElement 

>   **Code (variabele):** een code van vier tekens die aangeeft welke indeling van de media wordt gebruikt voor elk voorbeeld. Het volgende bereik van waarden is met de volgende semantische betekenis gereserveerd:

>  * 'hev1': voorbeelden van de Video voor dit nummer HEVC video, met de indeling 'hev1' voorbeeld beschrijving opgegeven in [ISO/IEC-14496-15] gebruiken.

>   **CodecPrivateData (variabele):** gegevens met parameters die specifiek zijn voor de media-indeling en gemeenschappelijk zijn voor alle voorbeelden in de track weergegeven als een reeks van hex-gecodeerde bytes. De indeling en semantisch reeks bytes varieert met de waarde van de **code** veld als volgt:

>   * Als een TrackElement HEVC video beschrijft de **code** veld moet gelijk zijn aan **'hev1'** en;

>   De **CodecPrivateData** veld bevat een hexadecimaal gecodeerde tekenreeksrepresentatie van de volgende byte-reeks opgegeven in ABNF [[RFC5234]:](http://go.microsoft.com/fwlink/?LinkId=123096) (geen wijziging van MS SSTR)

>   * %x 00 %x 00 %x 00 %x 01 SPSField %x 00 %x 00 %x 00 %x 01 PPSField

>   * SPSField bevat de reeks Parameter ingesteld (SP's).

>   * PPSField bevat de segment Parameter ingesteld (PPS).

>   Opmerking: De Video Parameter ingesteld (directeuren) niet is opgenomen in CodecPrivateData, maar moet zich bevinden in de bestands-header-bestanden opgeslagen in het vak 'hvcC'. Systemen met Smooth Streaming-Protocol moeten signaal extra decoderen parameters (bijvoorbeeld HEVC laag) met het aangepaste kenmerk "codecs."

##### <a name="22251-customattributeselement"></a>2.2.2.5.1 CustomAttributesElement 

#### <a name="226-streamfragmentelement"></a>2.2.6 StreamFragmentElement 

>   De **van SmoothStreamingMedia MajorVersion** veld moet worden ingesteld op 2, en **MinorVersion** veld moet worden ingesteld op 2. (Geen wijzigen)

##### <a name="22261-trackfragmentelement"></a>2.2.2.6.1 TrackFragmentElement 

### <a name="223-fragment-request"></a>2.2.3 fragment-aanvraag 

>   **Opmerking**: de standaardindeling media aangevraagd voor **MinorVersion** 2 en 'hev1' is 'iso8' merk ISO Base Media opgegeven bestandsindeling in [ISO/IEC 14496 12] ISO Base Media File Format vierde Edition en [ISO/IEC 23001-7] Algemene versleuteling tweede editie.

### <a name="224-fragment-response"></a>2.2.4 fragment antwoord 

#### <a name="2241-moofbox"></a>2.2.4.1 MoofBox 

#### <a name="2242-mfhdbox"></a>2.2.4.2 MfhdBox 

#### <a name="2243-trafbox"></a>2.2.4.3 TrafBox 

#### <a name="2244-tfxdbox"></a>2.2.4.4 TfxdBox 

>   De **TfxdBox** is afgeschaft en zijn vervangen door bijhouden Fragment decoderen van het Time (tfdt) opgegeven in de sectie [ISO/IEC 14496 12] 8.8.12 functie.

>   **Opmerking**: een client kan de duur van een fragment berekenen door op te tellen van de duur van de steekproef die worden vermeld in het vak in bijhouden uitvoeren (trun) of de standaardduur voorbeeld vermenigvuldigen met het aantal steekproeven time-out. De baseMediaDecodeTime in 'tfdt' plus fragment duur is gelijk aan de tijd URL-parameter voor het volgende fragment.

>   Een vak producent verwijzing tijd (prft) moet worden ingevoegd voordat het Fragment film (moof), indien nodig, om aan te geven van de UTC-tijd die overeenkomt met de bijhouden Fragment decoderen tijd van het eerste voorbeeld waarnaar wordt verwezen door het Fragment film als die is opgegeven in [ISO/IEC 14496 -12] sectie 8.16.5.

#### <a name="2245-tfrfbox"></a>2.2.4.5 TfrfBox 

>   De **TfrfBox** is afgeschaft en zijn vervangen door bijhouden Fragment decoderen van het Time (tfdt) opgegeven in de sectie [ISO/IEC 14496 12] 8.8.12 functie.

>   **Opmerking**: een client kan de duur van een fragment berekenen door op te tellen van de duur van de steekproef die worden vermeld in het vak in bijhouden uitvoeren (trun) of de standaardduur voorbeeld vermenigvuldigen met het aantal steekproeven time-out. De baseMediaDecodeTime in 'tfdt' plus fragment duur is gelijk aan de tijd URL-parameter voor het volgende fragment. Vooruit kijken adressen zijn afgeschaft omdat ze uit te live streamen stellen.

#### <a name="2246-tfhdbox"></a>2.2.4.6 TfhdBox 

>   De **TfhdBox** en verwante velden inkapselen standaardwaarden voor de per voorbeeld metagegevens in het fragment. De syntaxis van de **TfhdBox** veld is een strikte subset van de syntaxis van het vak bijhouden Fragment koptekst is gedefinieerd in [[ISO/IEC-14496-12]](http://go.microsoft.com/fwlink/?LinkId=183695) 8.8.7 sectie.

>   **BaseDataOffset (8 bytes):** de verschuiving, in bytes, vanaf het begin van de **MdatBox** veld naar het voorbeeldveld in de **MdatBox** veld. Vanwege deze beperking, moet de standaard-base-is-moof vlag (0x020000) worden ingesteld.

#### <a name="2247-trunbox"></a>2.2.4.7 TrunBox 

>   De **TrunBox** en verwante velden inkapselen per voorbeeld metagegevens voor de aangevraagde fragment. De syntaxis van **TrunBox** is een strikte subset van versie 1 bijhouden Fragment het uitvoeren in gedefinieerd [[ISO/IEC-14496-](http://go.microsoft.com/fwlink/?LinkId=183695)*12]* 8.8.8 sectie.

>   **SampleCompositionTimeOffset (4 bytes):** verschuiving van de tijd van de steekproef samenstelling van elk voorbeeld aangepast, zodat de presentatietijd van de eerste aangeboden voorbeeld in het fragment gelijk aan de tijd decoderen van het eerste voorbeeld gedecodeerd is. Negatieve video voorbeeld samenstelling offsets moeten worden gebruikt

>   zoals gedefinieerd in [[ISO/IEC-14496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

>   Opmerking: Dit vermijdt u een video synchronisatiefout veroorzaakt door vertraging audio gelijk is aan de grootste gedecodeerde afbeelding buffer verwijdering vertraging video en onderhoudt presentatie timing tussen alternatieve fragmenten die mogelijk andere verwijdering vertragingen hebben.

>   De syntaxis van de velden die zijn gedefinieerd in deze sectie worden opgegeven in ABNF [[RFC5234]](http://go.microsoft.com/fwlink/?LinkId=123096) blijft hetzelfde, behalve als volgt:

>   SampleCompositionTimeOffset = SIGNED_INT32

#### <a name="2248-mdatbox"></a>2.2.4.8 MdatBox 

#### <a name="2249-fragment-response-common-fields"></a>2.2.4.9 fragment algemene antwoordvelden 

### <a name="225-sparse-stream-pointer"></a>2.2.5 de aanwijzer sparse Stream 

### <a name="226-fragment-not-yet-available"></a>2.2.6 fragment nog niet beschikbaar 

### <a name="227-live-ingest"></a>2.2.7 live opnemen 

#### <a name="2271-filetype"></a>2.2.7.1 bestandstype 

>   **Bestandstype (variabele):** Hiermee geeft u het subtype en bedoelde gebruik van de MPEG-4 ([[MPEG4 RA])](http://go.microsoft.com/fwlink/?LinkId=327787) bestands- en kenmerken op hoog niveau.

>   **MajorBrand (variabele):** het primaire merk van het mediabestand. MOET worden ingesteld op 'isml'.

>   **MinorVersion (variabele):** secundaire versie van het mediabestand. MOET worden ingesteld op 1.

>   **CompatibleBrands (variabele):** Hiermee geeft u de ondersteunde merken van MPEG-4.
>   MOET 'ccff' en 'iso8.' bevatten

>   De syntaxis van de velden die zijn gedefinieerd in deze sectie worden opgegeven in ABNF [[RFC5234]](http://go.microsoft.com/fwlink/?LinkId=123096) is als volgt:

    FileType = MajorBrand MinorVersion CompatibleBrands
    MajorBrand = STRING_UINT32
    MinorVersion = STRING_UINT32
    CompatibleBrands = "ccff" "iso8" 0\*(STRING_UINT32)

**Opmerking**: de compatibiliteit merken 'ccff' en 'iso8' aangeven dat fragmenten aan de 'Algemene indeling van Container' en Common Encryption [ISO/IEC 23001-7] en ISO Base Media bestand indeling Edition 4 [ISO/IEC 14496 12 voldoet].

#### <a name="2272-streammanifestbox"></a>2.2.7.2 StreamManifestBox 

##### <a name="22721-streamsmil"></a>2.2.7.2.1 StreamSMIL 

#### <a name="2273-liveservermanifestbox"></a>2.2.7.3 LiveServerManifestBox 

##### <a name="22731-livesmil"></a>2.2.7.3.1 LiveSMIL 

#### <a name="2274-moovbox"></a>2.2.7.4 MoovBox 

#### <a name="2275-fragment"></a>2.2.7.5 fragment 

##### <a name="22751-track-fragment-extended-header"></a>2.2.7.5.1 bijhouden Fragment berekende koptekst 

### <a name="228-server-to-server-ingest"></a>2.2.8 server-naar-Server opnemen 

# <a name="3-protocol-details"></a>Gegevens van 3-protocol 


## <a name="31-client-details"></a>3.1 Clientdetails 

### <a name="311-abstract-data-model"></a>3.1.1 abstracte gegevensmodel 

#### <a name="3111-presentation-description"></a>3.1.1.1 presentatie beschrijving 

>   Het gegevenselement presentatiebeschrijving ingekapseld alle metagegevens voor de presentatie.

>   Presentatie metagegevens: Een set van metagegevens die geldt voor alle gegevensstromen in de presentatie. Presentatie metagegevens omvat de volgende velden, opgegeven in de sectie *2.2.2.1*:

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

>   LookaheadCount = 0 (Opmerking: vakken afgeschaft)

>   Presentaties moeten ook worden ingesteld:

    TimeScale = 90000

>   Stroom verzameling: Een verzameling is elementen beschrijving van de stroom van gegevens, zoals aangegeven in punt *3.1.1.1.2*.

>   Beveiliging beschrijving: Een verzameling gegevenselementen systeembeschrijving van beveiliging voor metagegevens, zoals aangegeven in punt *3.1.1.1.1*.

##### <a name="31111-protection-system-metadata-description"></a>3.1.1.1.1 de metagegevens systeembeschrijving beveiliging 

>   Het gegevenselement beveiliging metagegevens systeembeschrijving ingekapseld metagegevens die specifiek zijn voor één Content Protection systeem. (Geen wijzigen)

>   Beschrijving van de Header Protection: Inhoudsbeveiliging metagegevens die betrekking op één Content Protection systeem hebben. Beschrijving van de Header Protection omvat de volgende velden, opgegeven in de sectie *2.2.2.2*:

>   * **Systeem-id**
>   * **ProtectionHeaderContent**

##### <a name="31112-stream-description"></a>3.1.1.1.2 beschrijving van de stroom 

###### <a name="311121-track-description"></a>3.1.1.1.2.1 Beschrijving van de bijhouden 

###### <a name="3111211-custom-attribute-description"></a>3.1.1.1.2.1.1 Beschrijving van aangepaste kenmerk 

##### <a name="3113-fragment-reference-description"></a>3.1.1.3 beschrijving van fragment-verwijzing 

###### <a name="31131-track-specific-fragment-reference-description"></a>3.1.1.3.1 Beschrijving van track-specifieke Fragment-verwijzing 

#### <a name="3112-fragment-description"></a>3.1.1.2 fragment beschrijving 

##### <a name="31121-sample-description"></a>3.1.1.2.1 Steekproefbeschrijving 

### <a name="312-timers"></a>3.1.2 timers 

### <a name="313-initialization"></a>3.1.3 de initialisatie van de 

### <a name="314-higher-layer-triggered-events"></a>3.1.4 hogere laag geactiveerd gebeurtenissen 

#### <a name="3141-open-presentation"></a>3.1.4.1 presentatie openen 

#### <a name="3142-get-fragment"></a>3.1.4.2 Fragment ophalen 

#### <a name="3143-close-presentation"></a>3.1.4.3 sluiten presentatie 

### <a name="315-processing-events-and-sequencing-rules"></a>3.1.5 verwerking van gebeurtenissen en regels voor sequentiëren 

#### <a name="3151-manifest-request-and-manifest-response"></a>3.1.5.1 manifest aanvraag en antwoord Manifest 

#### <a name="3152-fragment-request-and-fragment-response"></a>3.1.5.2 fragment aanvraag en antwoord Fragment

## <a name="32-server-details"></a>3.2-servergegevens

## <a name="33-live-encoder-details"></a>3.3 Details van live codering 

# <a name="4-protocol-examples"></a>4 protocol-voorbeelden 

# <a name="5-security"></a>5-beveiliging 

## <a name="51-security-considerations-for-implementers"></a>5.1 beveiligingsoverwegingen voor implementeerders 
-----------------------------------------

>   Als de inhoud die via dit protocol getransporteerde hoge commerciële waarde heeft, kan een systeem van de beveiliging inhoud moet worden gebruikt om te voorkomen dat onbevoegde gebruik van de inhoud. De **ProtectionElement** kan worden gebruikt voor het transport van metagegevens die zijn gerelateerd aan het gebruik van een Content Protection-systeem. Beveiligde audio en video-inhoud worden versleuteld zoals opgegeven door MPEG Common Encryption tweede editie: 2015 [ISO/IEC 23001-7].

>   **Opmerking**: HEVC video en alleen de gegevens in VCL NALs segment is versleuteld. Segment headers en andere NALs zijn toegankelijk voor presentatietoepassingen voordat ze worden gedecodeerd. versleutelde gegevens zijn niet beschikbaar voor presentatietoepassingen in een beveiligde video pad.

# <a name="52-index-of-security-parameters"></a>5.2-Index van beveiligingsparameters 
-----------------------------


| **Beveiligingsparameter**  | **Sectie**         |
|-------------------------|---------------------|
| ProtectionElement       | *2.2.2.2*           |
| Algemene versleuteling vakken | *[ISO/IEC 23001-7]* |

# <a name="53-common-encryption-boxes"></a>5.3 common Encryption vakken
-----------------------

De volgende vakken aanwezig mag zijn in fragment-antwoorden als Common Encryption wordt toegepast en zijn opgegeven in [ISO/IEC 23001-7] of [ISO/IEC 14496 12]:

1.  Beveiliging System-vak voor specifieke koptekst (pssh)

2.  Voorbeeld codering in (senc)

3.  Voorbeeld ondersteunende informatievak verschuiving (saio)

4.  Aanvullende informatie grootte vak Voorbeeld (saiz)

5.  Voorbeeld beschrijving groepsvak (sgpd)

6.  Steekproef groepsvak (sbgp)

-----------------------

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
