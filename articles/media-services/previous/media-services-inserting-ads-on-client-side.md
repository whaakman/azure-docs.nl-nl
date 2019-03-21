---
title: Advertenties invoeren op de client | Microsoft Docs
description: In dit onderwerp laat zien hoe advertenties invoeren op de client.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: c3c2101576f9b0d0c7908e62bd5cc1d6e6eeb0b2
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189798"
---
# <a name="inserting-ads-on-the-client-side"></a>Advertenties invoeren op de client
In dit artikel bevat informatie over het invoegen van verschillende typen advertenties op de client.

Zie voor meer informatie over gesloten ondertiteling en ad-ondersteuning in Live streaming video's [ondersteund gesloten ondertiteling en standaarden voor advertentie-invoeging](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Azure Media Player ondersteunt momenteel geen advertenties.
> 
> 

## <a id="insert_ads_into_media"></a>Advertenties invoeren in uw Media
Azure Media Services biedt ondersteuning voor advertentie-invoeging via het Windows Media-Platform: Player Frameworks. Frameworks voor spelers met ad-ondersteuning zijn beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elk speler framework bevat voorbeeldcode die laat zien u hoe u een player-toepassing implementeert. Er zijn drie verschillende soorten advertenties die u in uw media: de lijst invoegen kunt.

* **Lineaire** – full-frame-advertenties die de belangrijkste video onderbreken.
* **Niet-lineaire** -overlay-advertenties die worden weergegeven als de belangrijkste video wordt afgespeeld, meestal een logo of andere statische afbeelding geplaatst in de speler.
* **Aanvullende** – advertenties die buiten de speler worden weergegeven.

Advertenties kunnen op elk gewenst moment in de tijdlijn van de belangrijkste video worden geplaatst. U moet de speler zien bij het afspelen van de ad en welke advertenties om af te spelen. Dit wordt gedaan met behulp van een set standaard op basis van een XML-bestanden: Video Ad Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP), Media abstracte sequentiëren sjabloon (b) en digitale Video speler Ad Interface Definition (VPAID). GROTE bestanden opgeven welke advertenties om weer te geven. VMAP bestanden opgeven wanneer verschillende advertenties afspelen en ENORME XML bevatten. B-bestanden zijn een andere manier om de volgorde advertenties die ook VAST XML kan bevatten. VPAID bestanden definieert een interface tussen de videospeler en de ad of ad-server.

Elk speler framework werkt anders en elk in een eigen artikel worden behandeld. Dit artikel beschrijft de basic mechanismen gebruikt voor het invoegen van advertenties. Videospelertoepassingen aanvragen advertenties van een ad-server. De ad-server kan reageren op een aantal manieren:

* Een groot bestand retourneren
* Retourneert een VMAP-bestand (met ingesloten VAST)
* Retourneert een b-bestand (met ingesloten VAST)
* Resultaat van een groot bestand met VPAID advertenties

### <a name="using-a-video-ad-service-template-vast-file"></a>Met behulp van een sjabloon (VAST)-bestanden van de Video Ad-Service
Een ENORME bestand wordt aangegeven welke ad of advertenties om weer te geven. De volgende XML-bestand is een voorbeeld van een groot bestand voor een lineaire ad:

```xml
    <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
```

De lineaire ad wordt beschreven door de <**lineair**> element. Hiermee geeft u aan de duur van de ad, bijhouden van gebeurtenissen, klikt u op via klikken traceren in, en een aantal **MediaFile** elementen. Traceringsgebeurtenissen zijn opgegeven in de <**TrackingEvents**> element en het toestaan van een ad-server voor het bijhouden van diverse gebeurtenissen die optreden tijdens het bekijken van de ad. In dit geval het begin, midden, voltooid, en vouw gebeurtenissen worden bijgehouden. De begingebeurtenis treedt op wanneer de advertentie wordt weergegeven. Het middelpunt gebeurtenis treedt op wanneer ten minste 50% van de tijdlijn van de ad heeft bekeken. De volledige gebeurtenis treedt op wanneer de ad is uitgevoerd naar het einde. De uit te breiden gebeurtenis treedt op wanneer de gebruiker de videospeler uitgebreid naar het volledige scherm. Clickthroughs zijn opgegeven met een <**ClickThrough**> element in een <**VideoClicks**> element en geeft u een URI naar een resource om weer te geven wanneer de gebruiker op de ad klikt. ClickTracking is opgegeven in een <**ClickTracking**>-element, ook in de <**VideoClicks**> element en geeft u een resource bijhouden voor de speler om aan te vragen wanneer de gebruiker op de ad klikt . De <**MediaFile**> elementen Geef informatie op over een specifieke codering van een AD-tenant. Wanneer er meer dan één <**MediaFile**>-element de video speler kunt ervoor kiezen de beste codering voor het platform.

Lineaire advertenties kunnen worden weergegeven in een opgegeven volgorde. Om dit te doen, voegt u extra <Ad> elementen op de VAST bestand en de volgorde die met behulp van de sequence-kenmerk opgeven. Het volgende voorbeeld wordt deze:

```xml
    <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
```

Niet-lineaire advertenties zijn opgegeven in een <Creative> element ook. Het volgende voorbeeld wordt een <Creative> element dat een niet-lineaire ad wordt beschreven.

```xml
    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>
```

De <**NonLinearAds**> element kan bevatten een of meer <**NonLinear**>-elementen, die elk een niet-lineaire Active Directory kunt beschrijven. De <**NonLinear**> element Hiermee geeft u de resource voor de niet-lineaire ad. De bron kan bestaan uit een <**StaticResource**>, een <**IFrameResource**>, of een <**HTMLResource**>. <**StaticResource**> Beschrijving van een niet-HTML-bron en definieert een creativeType-kenmerk geeft aan hoe de resource wordt weergegeven:

Afbeelding/gif, afbeelding/jpeg, afbeelding/png: de resource wordt weergegeven in een HTML-code <**img**> tag.

Application/x-javascript: de resource wordt weergegeven in een HTML-code <**script**> tag.

Application/x-shockwave-flash: de resource wordt weergegeven in een Flash player.

**IFrameResource** beschrijft een HTML-bron die kan worden weergegeven in een IFrame. **HTMLResource** beschrijft een stukje HTML-code die in een webpagina kan worden ingevoegd. **TrackingEvents** traceringsgebeurtenissen en de URI om aan te vragen bij de gebeurtenis opgeven. In dit voorbeeld worden de gebeurtenissen acceptInvitation en samenvouwen bijgehouden. Voor meer informatie over de **NonLinearAds** -element en alle onderliggende items, Zie IAB.NET/VAST. Houd er rekening mee dat de **TrackingEvents** element bevindt zich in de **NonLinearAds** element in plaats van de **NonLinear** element.

Companion-advertenties worden gedefinieerd in een <CompanionAds> element. De <CompanionAds> element kan bevatten een of meer <Companion> elementen. Elke <Companion> element beschrijft een aanvullende Active Directory en mag een <StaticResource>, <IFrameResource>, of <HTMLResource> die zijn opgegeven in de dezelfde manier als in een niet-lineaire ad. Een groot bestand kan meerdere companion-advertenties bevatten en de spelertoepassing kunt de meest geschikte ad om weer te geven. Zie voor meer informatie over VAST [ENORME 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Met behulp van een digitale Video meerdere Ad afspeellijst (VMAP)-bestand
Een bestand VMAP kunt u opgeven wanneer ad onderbrekingen optreden, hoe lang elk einde is, hoeveel advertenties kunnen worden weergegeven in een einde en welke typen advertenties kunnen worden weergegeven tijdens een onderbreking. De volgende handelingen uit in een voorbeeldbestand VMAP die een pauze van één ad definieert:

```xml
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[https://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scalable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
```

Een bestand VMAP begint met een <VMAP> element een of meer bevat <AdBreak> elementen, elke definiëren van een ad-einde. Elk ad-einde Hiermee geeft u een einde type, het break-ID en de time-offset. Het kenmerk breakType geeft het type ad die kan worden afgespeeld tijdens de pauze: lineaire, niet-lineaire, of weer te geven. Advertenties kaart uitgebreid companion-advertenties weergeven. Meer dan één ad-type kan worden opgegeven in een lijst met gescheiden door komma's (zonder spaties). De breakID is een optionele id voor de ad. De timeOffset geeft aan wanneer de ad moet worden weergegeven. Het kan worden opgegeven in een van de volgende manieren:

1. Tijd in de indeling uu: mm: of hh:mm:ss.mmm is waar .mmm milliseconden is. De waarde van dit kenmerk geeft de tijd vanaf het begin van de video tijdlijn aan het begin van het ad-einde.
2. Percentage – n % indeling is waarbij n het percentage van de tijdlijn van de video af te spelen voordat het afspelen van de ad
3. Begin/einde: geeft aan dat een advertentie moet worden weergegeven voordat of nadat de video is weergegeven
4. Positie: Hiermee geeft u de volgorde van de ad-einden wanneer de timing van de ad-einden onbekend, zoals live streamen is. De volgorde van elk ad-einde is opgegeven in de indeling van de #n waarbij n staat voor een geheel getal 1 of hoger. 1 geeft aan dat de ad moet worden afgespeeld op de eerste kans 2 geeft aan dat de ad moet worden afgespeeld op de tweede mogelijkheid enzovoort.

Binnen de <AdBreak> -element, kunnen zich een <**AdSource**> element. De <**AdSource**>-element bevat de volgende kenmerken:

1. Id – Hiermee geeft u een id voor de ad-bron
2. allowMultipleAds: een Booleaanse waarde waarmee wordt aangegeven of meerdere advertenties kunnen worden weergegeven tijdens het ad-einde
3. followRedirects: een optionele Booleaanse waarde waarmee wordt aangegeven als de video speler moet voldoen aan leidt binnen een ad-antwoord

De <**AdSource**>-element heeft de speler een inline-ad-antwoord of een verwijzing naar een ad-antwoord. Het kan een van de volgende elementen bevatten:

* <VASTAdData> Geeft aan dat een ENORME ad-antwoord is ingesloten in het bestand VMAP
* <AdTagURI> een URI die verwijst naar een ad-antwoord van een ander systeem
* <CustomAdData> -een willekeurige tekenreeks die staat voor een niet-VAST antwoord

In dit voorbeeld wordt een inline-ad-antwoord wordt opgegeven met een <VASTAdData> -element dat een ENORME ad-antwoord bevat. Zie voor meer informatie over de andere elementen, [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

De <**AdBreak**> element mag ook een <**TrackingEvents**> element. De <**TrackingEvents**>-element kunt u voor het bijhouden van het begin of einde van een ad-einde of of een fout opgetreden tijdens het ad-einde. De <**TrackingEvents**> element bevat een of meer <**bijhouden**>-elementen, die elk Hiermee geeft u een traceringsgebeurtenis voor de en een tracering URI. De mogelijke traceringsgebeurtenissen zijn:

1. het begin van een ad-einde wordt bijgehouden breakStart:
2. breakEnd: bijhouden van de voltooiing van een ad-einde
3. Fout: een fout is opgetreden tijdens het ad-einde wordt bijgehouden

Het volgende voorbeeld ziet u een VMAP-bestand dat Hiermee geeft u traceringsgebeurtenissen

```xml
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
```

Voor meer informatie over het <**TrackingEvents**>-element en alle onderliggende items, Zie http://iab.net/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Met behulp van een Media-Abstract sequentiëren sjabloonbestand (b)
Een bestand b kunt u opgeven triggers die bepalen wanneer een AD-tenant wordt weergegeven. Hier volgt een voorbeeld van de b-bestand met triggers voor een pre-album Active Directory, een halverwege rollen-ad en een na implementatie van de Active Directory.

```xml
    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>
```


Een bestand b begint met een **b** element met een **triggers** element. De <triggers> element bevat een of meer **trigger** elementen die bepalen wanneer een advertentie moet worden afgespeeld.

De **trigger** element bevat een **startConditions** element die opgeven wanneer een advertentie moet beginnen om af te spelen. De **startConditions** element bevat een of meer <condition> elementen. Wanneer elke <condition> resulteert in waar een trigger wordt gestart of ingetrokken, afhankelijk van of u de <condition> is opgenomen in een **startConditions** of **endConditions** element respectievelijk. Wanneer meerdere <condition> elementen aanwezig zijn, worden deze behandeld als een impliciete of een voorwaarde evalueren op true, wordt de trigger te initiëren. <condition> elementen kunnen worden genest. Als onderliggende <condition> elementen zijn vooraf ingesteld, worden deze behandeld als een impliciete en alle voorwaarden moeten resulteren in waar voor de trigger te initiëren. De <condition> element bevat de volgende kenmerken die de voorwaarde definiëren:

1. **type** – Hiermee geeft u het type voorwaarde, gebeurtenis, of de eigenschap
2. **naam** – de naam van de eigenschap of gebeurtenis moet worden gebruikt tijdens de evaluatie
3. **waarde** : de waarde die een eigenschap wordt geëvalueerd op basis van
4. **operator** : de bewerking te gebruiken tijdens de evaluatie: EQ (gelijkteken), NEQ (niet gelijk aan), GTR (meer), GEQ (groter of gelijk zijn), LT (kleiner dan), LEQ (kleiner dan of gelijk aan), rest (modulo)

**endConditions** bevatten ook <condition> elementen. Wanneer een voorwaarde wordt geëvalueerd op waar de trigger wordt opnieuw ingesteld. De <trigger> element bevat ook een <sources> element een of meer bevat <source> elementen. De <source> elementen de URI voor de ad-antwoord en het type ad reactie definiëren. In dit voorbeeld wordt een URI zijn die aan een uitgebreid antwoord gegeven.

```xml
    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
```

### <a name="using-video-player-ad-interface-definition-vpaid"></a>Met behulp van de Video speler-Ad-Interface Definition (VPAID)
VPAID is een API voor het inschakelen van uitvoerbare ad-eenheden om te communiceren met een videospeler. Hiermee wordt zeer interactieve ad-ervaringen. De gebruiker kan communiceren met de ad en de ad kan reageren op acties die worden uitgevoerd door de viewer. Een AD-tenant kan bijvoorbeeld knoppen waarmee de gebruiker om weer te geven voor meer informatie of een langere versie van de ad weergeven. De video speler de VPAID-API moet ondersteunen en de API moet worden geïmplementeerd door de uitvoerbare ad. Wanneer een speler aanvraag doet bij dat een AD-tenant van een ad-server de server reageert met een uitgebreid antwoord met een VPAID Active Directory.

Een uitvoerbaar bestand AD-tenant wordt gemaakt in de code die moet worden uitgevoerd in een runtime-omgeving, zoals Adobe Flash™ of JavaScript die kan worden uitgevoerd in een webbrowser. Wanneer een ad-server een uitgebreid antwoord retourneert met een ad VPAID, de waarde van de apiFramework kenmerk in de <MediaFile> element moet 'VPAID'. Dit kenmerk geeft aan dat de ingesloten ad een VPAID uitvoerbare ad. Het kenmerk van het type moet worden ingesteld op het MIME-type van het uitvoerbare bestand, zoals ' application/x-shockwave-flash' of ' application/x-javascript'. Het volgende XML-codefragment bevat de <MediaFile> element uit een uitgebreid antwoord met een VPAID uitvoerbare ad.

```xml
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
```

Een uitvoerbaar bestand ad kan worden geïnitialiseerd met behulp van de <AdParameters> element in de <Linear> of <NonLinear> elementen in een uitgebreid antwoord. Voor meer informatie over de <AdParameters> -element, Zie [ENORME 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Zie voor meer informatie over de API VPAID [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementatie van een Windows- of Windows Phone 8-speler met Ad-ondersteuning
Het Microsoft Media Platform: Player Framework voor Windows 8 en Windows Phone 8 bevat een verzameling van voorbeeldtoepassingen die laten u hoe u zien voor het implementeren van een videospeler-toepassing met behulp van het framework. U kunt downloaden de Player Framework en de voorbeelden van [Player Framework voor Windows 8 en Windows Phone 8](https://playerframework.codeplex.com).

Wanneer u de oplossing Microsoft.PlayerFramework.Xaml.Samples opent, ziet u een aantal mappen in het project. De reclame-map bevat de voorbeeldcode die relevant zijn voor het maken van een videospeler met ad-ondersteuning. In het adverteren map bevindt zich een aantal XAML/cs bestanden die laten zien hoe u advertenties invoeren op een andere manier. De volgende lijst beschrijft:

* AdPodPage.xaml laat zien hoe een ad-pod weergegeven.
* AdSchedulingPage.xaml laat zien hoe voor het plannen van advertenties.
* FreeWheelPage.xaml ziet u hoe u de invoegtoepassing FreeWheel voor het plannen van advertenties.
* MastPage.xaml laat zien hoe het plannen van advertenties door een b-bestand.
* ProgrammaticAdPage.xaml laat zien hoe programmatisch plannen advertenties in een video.
* ScheduleClipPage.xaml laat zien hoe voor het plannen van een AD-tenant zonder een groot bestand.
* VastLinearCompanionPage.xaml ziet u hoe u een lineaire invoegt en aanvullende ad.
* VastNonLinearPage.xaml laat zien hoe een niet-lineaire ad invoegen.
* VmapPage.xaml laat zien hoe advertenties met een VMAP-bestand opgeven.

Elk van deze voorbeelden maakt gebruik van de Media Player-klasse gedefinieerd door de speler-framework. De meeste voorbeelden gebruiken-invoegtoepassingen die is ondersteuning toegevoegd voor verschillende indelingen voor ad-antwoord. Het voorbeeld ProgrammaticAdPage communiceert via een programma met een exemplaar van de Media Player.

### <a name="adpodpage-sample"></a>AdPodPage Sample
In dit voorbeeld maakt gebruik van de AdSchedulerPlugin om te definiëren wanneer om weer te geven van een AD-tenant. In dit voorbeeld wordt een aankondiging halverwege roll gepland om te worden afgespeeld na vijf seconden. De ad-schil (een groep van advertenties om weer te geven in volgorde) is opgegeven in een groot bestand geretourneerd door een ad-server. De URI moet het OVERGROTE bestand is opgegeven in de <RemoteAdSource> element.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>
```

Zie voor meer informatie over de AdSchedulerPlugin [reclame in het kader van de speler op Windows 8 en Windows Phone 8](https://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
In dit voorbeeld gebruikt ook de AdSchedulerPlugin. Hiermee plant u drie advertenties, een implementatie van de pre-ad, een halverwege rollen-ad en een na implementatie van de Active Directory. De URI moet het VAST voor elke ad is opgegeven in een <RemoteAdSource> element.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="freewheelpage"></a>FreeWheelPage
In dit voorbeeld wordt de FreeWheelPlugin die Hiermee geeft u een kenmerk van de gegevensbron die Hiermee geeft u een URI die verwijst naar een SmartXML-bestand dat Hiermee geeft u de ad-inhoud, evenals ad gegevens over de planning.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="mastpage"></a>MastPage
In dit voorbeeld wordt de MastSchedulerPlugin waarmee u een b-bestand te gebruiken. Het bronkenmerk Hiermee geeft u de locatie van de b-bestand.
```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="programmaticadpage"></a>ProgrammaticAdPage
In dit voorbeeld communiceert via een programma met de Media Player. Het bestand ProgrammaticAdPage.xaml waarmee de Media Player wordt:

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>
```

Het bestand ProgrammaticAdPage.xaml.cs maakt u een AdHandlerPlugin wordt toegevoegd een TimelineMarker om op te geven als een AD-tenant moet worden weergegeven, en wordt vervolgens toegevoegd een handler voor de MarkerReached gebeurtenis die een RemoteAdSource op te geven van een URI naar een groot bestand wordt geladen, en vervolgens speelt de ad.

```csharp
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }
```

### <a name="scheduleclippage"></a>ScheduleClipPage
In dit voorbeeld maakt gebruik van de AdSchedulerPlugin voor het plannen van een halverwege rollen-ad door een WMV-bestand met de ad op te geven.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
In dit voorbeeld ziet u hoe de AdSchedulerPlugin gebruiken voor het plannen van een implementatie van de halverwege lineaire ad met een Basisnetwerk ad. De <RemoteAdSource> element Hiermee geeft u de locatie van de ENORME bestand.

```xml
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
In dit voorbeeld maakt gebruik van de AdSchedulerPlugin een lineaire plannen en een niet-lineaire ad. De locatie van de ENORME wordt opgegeven met de <RemoteAdSource> element.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="vmappage"></a>VMAPPage
In dit voorbeeld maakt gebruik van de VmapSchedulerPlugin advertenties met behulp van een bestand VMAP plannen. De URI moet het bestand VMAP is opgegeven in het bronkenmerk van de <VmapSchedulerPlugin> element.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Implementatie van een iOS-Video speler met Ad-ondersteuning
Het Microsoft Media Platform: Player Framework voor iOS bevat een verzameling van voorbeeldtoepassingen die laten u hoe u zien voor het implementeren van een videospeler-toepassing met behulp van het framework. U kunt downloaden de Player Framework en de voorbeelden van [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). De GitHub-pagina bevat een koppeling naar een Wiki met aanvullende informatie over de player framework en een inleiding tot de player-voorbeeld: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>Planning advertenties met VMAP
Het volgende voorbeeld ziet u hoe om te plannen met behulp van een bestand VMAP advertenties.

```csharp
    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }
            }
```

### <a name="scheduling-ads-with-vast"></a>Planning advertenties met VAST
Het volgende voorbeeld laat zien hoe een latere binding grote Active Directory plannen.


```csharp
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

   Het volgende voorbeeld laat zien hoe een vroege binding ENORME ad plannen.

```csharp
    //Example:4 Schedule an early binding VAST ad
    //Download the VAST file
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]])
    {
        [self logFrameworkError];
    }
    else
    {
        adLinearTime.startTime = 7;
        adLinearTime.duration = 0;

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }
```

Het volgende voorbeeld toont het invoegen van een AD-tenant met behulp van ruwe knippen bewerken (Broncomputer)

```csharp
    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }
```

Het volgende voorbeeld ziet hoe u een ad-schil plannen.

```csharp
    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Het volgende voorbeeld ziet hoe u voor het plannen van een niet-vergrendelde halverwege rollen-ad. Een niet-vergrendelde ad is alleen afgespeeld zodra ongeacht eventuele zoekt de viewer wordt uitgevoerd.

```csharp
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Het volgende voorbeeld ziet hoe u plant een sticky halverwege rollen-ad. Een sticky ad wordt altijd weergegeven wanneer die het opgegeven punt op de tijdlijn van de video is bereikt.

```csharp
    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Het volgende voorbeeld laat zien hoe een na implementatie van de Active Directory plannen.

```csharp
    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Het volgende voorbeeld laat zien hoe een implementatie van de pre-Active Directory plannen.

```csharp
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Het volgende voorbeeld laat zien hoe een implementatie van de halverwege overlay Active Directory plannen.

```csharp
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Videospelertoepassingen ontwikkelen](media-services-develop-video-players.md)

