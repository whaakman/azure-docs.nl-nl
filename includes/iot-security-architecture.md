# <a name="internet-of-things-security-architecture"></a>Internet der dingen-beveiligingsarchitectuur

Bij het ontwerpen van een systeem, is het belangrijk om te begrijpen van de mogelijke bedreigingen tot dat systeem en dienovereenkomstig juiste beveiliging niet toevoegen als het systeem is ontworpen en ontworpen. Het is belangrijk voor het ontwerpen van het product vanaf het begin rekening met beveiliging omdat informatie over hoe een aanvaller mogelijk een systeem kunt u ervoor dat de juiste oplossingen in plaats vanaf het begin.

## <a name="security-starts-with-a-threat-model"></a>Beveiliging begint met een risicomodel

Microsoft heeft lang threat modellen gebruikt voor de producten en van het bedrijf threat modeling proces openbaar beschikbaar heeft gesteld. De ervaring van het bedrijf laat zien dat het modelobjectmodel heeft onverwachte voordelen na het direct inzicht in wat bedreigingen de meest zijn betreffende. Bijvoorbeeld, maakt het ook een avenue voor een open discussie met anderen buiten het ontwikkelingsteam, wat tot nieuwe ideeën en verbeteringen in het product leiden kan.

Het doel van risicomodel is om te begrijpen hoe een aanvaller mogelijk plegen op een systeem en controleer vervolgens of de juiste oplossingen zijn geïnstalleerd. Threat modellering dwingt het ontwerpteam rekening houden met beperkingen als het systeem is ontworpen in plaats van nadat het systeem wordt geïmplementeerd. Dit feit is zeer belangrijk, omdat het met terugwerkende kracht beveiligingen voor een groot aantal apparaten in het veld is onbruikbare, foutgevoelig en leaves klanten op risico.

Veel ontwikkelteams doen een uitstekende taak voor het vastleggen van de functionele vereisten voor het systeem die profiteren van klanten. Identificeren niet duidelijk manieren dat iemand zouden het systeem misbruiken kan is echter moeilijker. Risicomodel kunt ontwikkelteams begrijpen wat een aanvaller kan doen en waarom. Risicomodel is een gestructureerde proces waarbij een discussie over de beveiliging ontwerpbeslissingen maakt in het systeem, evenals wijzigingen in het ontwerp dat langs de manier zijn aangebracht die gevolgen beveiliging. Een risicomodel is gewoon een document, wordt in deze documentatie ook een ideale manier om te controleren of de continuïteit van de retentie van opgedane kennis geleerd en nieuwe help team vrijgeven snel vertegenwoordigt. Ten slotte is een resultaat van risicomodel waarmee u rekening houden met andere aspecten van beveiliging, zoals welke beveiligingsverplichtingen die u wilt bieden aan uw klanten. Deze verplichtingen in combinatie met risicomodel kennis en station testen van uw oplossing voor het Internet der dingen (IoT).

### <a name="when-to-threat-model"></a>Wanneer dreiging model

[Risicomodel](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) biedt de grootste waarde wanneer u deze in de ontwerpfase opnemen. Wanneer u ontwerpt, hebt u de grootste flexibiliteit te wijzigen om te elimineren bedreigingen. Bedreigingen bespaard omdat niet met opzet, is het gewenste resultaat. Het is veel eenvoudiger dan beperkingen toe te voegen, deze testen en blijven ze actueel blijven en bovendien die verwijdering is niet altijd mogelijk. Wordt het moeilijker te elimineren bedreigingen, zoals een product meer volwassen wordt en op zijn beurt uiteindelijk vereist meer werk en veel moeilijker voor-en nadelen dan threat modeling vroeg stadium in de ontwikkeling.

### <a name="what-to-threat-model"></a>Wat u moet een risicomodel

U moet de dreiging model de oplossing in zijn geheel en ook zich richten op de volgende gebieden:

* De beveiliging en privacy-functies
* De functies waarvoor fouten beveiliging relevante zijn
* De functies die grens van een vertrouwensrelatie touch

### <a name="who-threat-models"></a>Wie dreiging modellen

Risicomodel is een proces zoals elke andere. Het is verstandig om te behandelen van het document threat model zoals elk ander onderdeel van de oplossing en te valideren. Veel ontwikkelteams doen een uitstekende taak voor het vastleggen van de functionele vereisten voor het systeem die profiteren van klanten. Identificeren niet duidelijk manieren dat iemand zouden het systeem misbruiken kan is echter moeilijker. Risicomodel kunt ontwikkelteams begrijpen wat een aanvaller kan doen en waarom.

### <a name="how-to-threat-model"></a>Hoe risicomodel

De threat modeling proces bestaat uit vier stappen; de stappen zijn:

* Model van de toepassing
* Bedreigingen opsommen
* Bedreigingen te verhelpen
* Valideren van de oplossingen

#### <a name="the-process-steps"></a>De stappen

Drie regels van miniatuur rekening moet houden bij het bouwen van een risicomodel:

1. Diagram van een buiten-referentiearchitectuur maken.
1. Breedte eerst worden gestart. Een overzicht en inzicht in het systeem als geheel deep wilt voordat. Deze aanpak zorgt ervoor dat u dieper in de juiste plaatsen worden uitgevoerd.
1. Het proces schijf, niet toestaan dat het proces dat u station. Als u een probleem te in de fase modelleren vinden en wilt verkennen, gaat u voor deze! Niet van mening bent dat u moet de volgende stappen slavishly.

#### <a name="threats"></a>Bedreigingen

De vier belangrijkste elementen van een risicomodel zijn:

* Processen zoals Win32-services, webservices en * nix daemons. Sommige complexe entiteiten (bijvoorbeeld veld gateways en sensoren) kunnen worden gescheiden, zoals een proces als technische inzoomen in deze gebieden niet mogelijk is.
* Gegevens worden opgeslagen (anywhere gegevens worden opgeslagen, zoals een configuratiebestand of de database)
* Gegevensstroom (waarbij gegevens worden verplaatst tussen andere elementen in de toepassing)
* Externe entiteiten (Alles die communiceert met het systeem, maar is niet onder het beheer van de toepassing, zijn bijvoorbeeld gebruikers en feeds satelliet)

Alle elementen in het architectuurdiagram zijn onderworpen aan verschillende bedreigingen; in dit artikel de STRIDE verkorte weergave. Lees [Threat Modeling opnieuw, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) voor meer informatie over de STRIDE-elementen.

Andere elementen van het diagram van de toepassing worden bepaalde STRIDE bedreigingen:

* Processen zijn onderworpen aan STRIDE
* Gegevensstromen TID gelden
* Opgeslagen gegevens zijn onderworpen aan TID en soms op R, als de gegevensarchieven logboekbestanden.
* Externe entiteiten zijn onderworpen aan SRD

## <a name="security-in-iot"></a>Beveiliging in IoT

Verbonden speciale apparaten hebben een groot aantal potentiële surface gebieden van interactie en interactie patronen, die allemaal moet worden beschouwd als een raamwerk bieden voor het beveiligen van digitale toegang tot die apparaten. De term 'digitale toegang' wordt hier gebruikt om te onderscheiden van bewerkingen die worden uitgevoerd via directe apparaatinteractie indien toegangsbeveiliging via beheer op fysieke toegang wordt verleend. Bijvoorbeeld, als het apparaat in een ruimte met een vergrendeling op de deur. Hoewel fysieke toegang kan niet worden geweigerd met behulp van software en hardware, kunnen voorkomen dat u de fysieke toegang ertoe leidt dat tot system storing maatregelen worden getroffen.

Als u de interactie patronen verkennen, kijken 'apparaat control' en 'apparaatgegevens' met dezelfde mate van aandacht. 'Apparaat control' kan worden geclassificeerd als alle informatie die met het doel van het wijzigen van of het gedrag van het naar de staat of de status van de omgeving invloed op een apparaat wordt geleverd door een partij. 'Apparaatgegevens' kunnen worden geclassificeerd als alle informatie op die een apparaat naar een andere partij over de status en de waargenomen status van de omgeving verzendt.

Om te optimaliseren best practices voor beveiliging, is het raadzaam een typische IoT-architectuur is onderverdeeld in meerdere onderdeel/zones als onderdeel van de threat modeling oefening. Deze zones volledig in deze sectie worden beschreven, en omvatten:

* Apparaat
* Veldgateway
* Cloud gateways, en
* Services.

Zones zijn brede manier om te segmenteren van een oplossing. elke zone heeft vaak een eigen gegevens en verificatie en autorisatie-vereisten. Zones kunnen ook worden gebruikt voor isolatie schade en de impact van lage vertrouwensrelatie zones op hoger vertrouwen zones beperken.

Elke zone wordt gescheiden door een grens vertrouwen die wordt geregistreerd als de rode stippellijn in het volgende diagram. Dit vertegenwoordigt een migratie van gegevens van/vanuit één bron naar een andere. Tijdens deze overgang kan de gegevens worden onderworpen aan Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service en verhoging van bevoegdheden (STRIDE).

![IoT-beveiligingszones](media/iot-security-architecture/iot-security-architecture-fig1.png) 

De onderdelen die binnen elke grens ook zijn onderworpen aan STRIDE, het inschakelen van een volledige 360 threat modeling weergave van de oplossing. De volgende secties uitwerken op elk van de onderdelen en specifieke beveiligingsproblemen en oplossingen die moeten worden ingesteld.

De volgende secties worden besproken standaardonderdelen gewoonlijk in deze zones.

### <a name="the-device-zone"></a>De apparaat-Zone

De apparaat-omgeving is de onmiddellijke fysieke ruimte rond het apparaat, waar fysieke toegang en/of 'lokale netwerk' peer-to-peer digitale toegang tot het apparaat is mogelijk. 'Lokale netwerk' wordt ervan uitgegaan dat een netwerk dat is uniek en geïsoleerd van – maar bevat beperkt bereik draadloze radio-technologie waarmee peer-to-peer-communicatie van apparaten mogelijk met – het openbare Internet overbrugd. Dit gebeurt *niet* bevatten netwerk virtualisatietechnologie de illusie van een lokaal netwerk maken en ook bevat geen openbare operator netwerken waarvoor geen twee apparaten communiceren via openbaar netwerk ruimte alsof ze een peer-to-peer-communicatie relatie invoeren.

### <a name="the-field-gateway-zone"></a>De Zone van de Gateway veld

Veldgateway is een apparaat/apparaat of bepaalde server voor computersoftware die als communicatie factor en potentieel als een apparaat besturingselement systeem en het apparaat gegevensverwerking hub fungeert. De zone van de gateway veld bevat het veldgateway zelf en alle apparaten die zijn gekoppeld. Zoals de naam al aangeeft, veld gateways buiten toegewezen gegevensverwerking faciliteiten fungeren, zijn meestal locatie gebonden, worden mogelijk onderhevig aan fysieke inbraakdetectie en beperkte operationele redundantie. Alle om te melden dat een veldgateway meestal een ding is kan een touch en sabotage achterhoofd wat de functie is.

Een veldgateway wijkt af van een router alleen verkeer in dat deze een actieve rol bij het beheren van toegang heeft gehad en informatiestromen, wat betekent dat een toepassing is verholpen entiteit en de netwerkverbinding of terminal-sessie. Een NAT-apparaat of de firewall, in tegenstelling komt niet in aanmerking als veld gateways omdat ze geen expliciete verbinding of sessie aansluitingen, maar in plaats daarvan een route (of blok)-verbindingen zijn of tot stand gebracht via deze sessies. De veldgateway heeft twee verschillende surface gebieden. Een bespreekt de apparaten die zijn gekoppeld aan deze en binnen de zone vertegenwoordigt en de andere bespreekt alle externe partijen en de rand van de zone is.

### <a name="the-cloud-gateway-zone"></a>De zone van de gateway cloud

Cloudgateway is een systeem waarmee externe communicatie van en naar de apparaten of veld gateways uit diverse verschillende sites in de ruimte openbaar netwerk, meestal naar een cloud-gebaseerd beheer en het systeem voor analyse, een federatieve van dergelijke systemen. In sommige gevallen kan vergemakkelijken een cloudgateway onmiddellijk toegang voor apparaten voor speciale doeleinden van aansluitingen zoals tablets en telefoons. In de context die hier worden besproken, is 'cloud' bedoeld om te verwijzen naar een specifieke gegevensverwerkingssysteem die niet is gekoppeld aan dezelfde site als de gekoppelde apparaten of veld gateways. Ook operationele metingen in een Zone Cloud kennis te voorkomen dat bepaalde fysieke toegang en worden niet per se blootgesteld aan een 'openbare cloud'-infrastructuur.  

Een cloudgateway kan mogelijk worden toegewezen in een netwerkvirtualisatie-overlay naar de cloudgateway en alle gekoppelde apparaten of het veld gateways van ander netwerkverkeer kastje. De cloudgateway zelf is niet een controlesysteem apparaat of een verwerking of opslagfaciliteit voor gegevens van apparaten; deze interface met de cloudgateway van de. De gateway cloud zone bevat de cloudgateway zelf samen met alle veld gateways en apparaten direct of indirect gekoppeld. De rand van de zone is een afzonderlijke oppervlak waar alle externe partijen via communiceren.

### <a name="the-services-zone"></a>De zone services

Een 'service' is gedefinieerd voor deze context als alle software-onderdelen of module die in aanraking met apparaten via een gateway veld- of cloud voor gegevensverzameling en -analyse, evenals voor opdracht en controle komt. Services worden tussenstation. Ze onder hun identiteit naar gateways en andere subsystemen fungeren, opslaan en analyseren van gegevens, autonoom opdrachten uitgeven voor apparaten die zijn gebaseerd op gegevens insights of schema's en gegevens openbaar en mogelijkheden voor geautoriseerde eindgebruikers te beheren.

### <a name="information-devices-versus-special-purpose-devices"></a>Informatie-apparaten en apparaten voor speciale doeleinden

Pc's, telefoons en tablets zijn voornamelijk interactieve informatie-apparaten. Telefoons en tablets zijn expliciet rond het optimaliseren van de levensduur van de batterij geoptimaliseerd. Ze bij voorkeur uitschakelen gedeeltelijk wanneer niet direct communiceert met een persoon of bij het opgeven van niet-services zoals muziek afspelen of begeleiden van hun eigenaar naar een bepaalde locatie. Vanuit het oogpunt van systemen van fungeren deze technologie informatie apparaten hoofdzakelijk als proxy's naar mensen. Ze zijn 'mensen actuators' voorstellen acties en 'mensen sensoren' verzamelen van de invoer.

Apparaten voor speciale doeleinden zijn van eenvoudige temperatuursensors naar complexe factory productie regels met duizenden onderdelen in deze, verschillend. Deze apparaten zijn veel meer bereik in het doel en zelfs als ze een gebruikersinterface bieden, ze zijn voornamelijk gericht op tussenliggende met of worden geïntegreerd in de activa in de fysieke wereld. Ze meten en rapporteren milieu omstandigheden, kleppen inschakelen servos beheren, alarmen geluid, switch lichten en veel andere taken uitvoeren. Ze helpen bij te werken waarvoor een gegevens-apparaat ofwel te algemeen, te duur, te groot of te brokkelig is. Het doel van de concrete bepaalt het technische ontwerp onmiddellijk als ook de beschikbare financieel budget voor hun productie en de levensduur van de geplande bewerking. De combinatie van deze twee belangrijke factoren Hiermee beperkt u de beschikbare operationele energie budget, fysieke footprint en dus beschikbare opslag compute- en beveiligingsmogelijkheden.

Als er iets 'gaan verkeerde' met geautomatiseerde of externe instelbare apparaten, bijvoorbeeld fysieke defecte producten of besturingselement logica gebreken willful inbreuk te bewerken. De productie veel mogelijk worden vernietigd, gebouwen mogelijk looted of gebrand omlaag en personen gewonde of zelfs die mogelijk zijn. Dit is een geheel andere klasse van de schade dan iemand bezetten een gestolen creditcard limiet. De beveiligingsbalk voor apparaten die zaken verplaatsen, en ook voor sensorgegevens die uiteindelijk in de opdrachten die ertoe leiden dingen resulteert dat te verplaatsen, moet hoger zijn dan in een e-commerce of scenario voor bankieren.

### <a name="device-control-and-device-data-interactions"></a>Apparaatbeheer en -interacties van apparaat-gegevens

Verbonden speciale apparaten hebben een groot aantal potentiële surface gebieden van interactie en interactie patronen, die allemaal moet worden beschouwd als een raamwerk bieden voor het beveiligen van digitale toegang tot die apparaten. De term 'digitale toegang' wordt hier gebruikt om te onderscheiden van bewerkingen die worden uitgevoerd via directe apparaatinteractie indien toegangsbeveiliging via beheer op fysieke toegang wordt verleend. Bijvoorbeeld, als het apparaat in een ruimte met een vergrendeling op de deur. Hoewel fysieke toegang kan niet worden geweigerd met behulp van software en hardware, kunnen voorkomen dat u de fysieke toegang ertoe leidt dat tot system storing maatregelen worden getroffen.

Als u de interactie patronen verkennen, bekijkt u 'apparaat control' en 'apparaatgegevens' met hetzelfde niveau aandachtspunten bij risicomodel. 'Apparaat control' kan worden geclassificeerd als alle informatie die met het doel van het wijzigen van of het gedrag van het naar de staat of de status van de omgeving invloed op een apparaat wordt geleverd door een partij. 'Apparaatgegevens' kunnen worden geclassificeerd als alle informatie op die een apparaat naar een andere partij over de status en de waargenomen status van de omgeving verzendt.

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Threat modeling van de Azure IoT reference-architectuur

Microsoft gebruikt de eerder beschreven hiervoor threat modeling voor Azure IoT framework. De volgende sectie wordt het concrete voorbeeld van Azure IoT Reference Architecture om te demonstreren hoe om na te denken over threat modeling voor IoT en hoe de geïdentificeerd bedreigingen kunnen weren. In dit voorbeeld identificeert de vier gebieden van de focus:

* Apparaten en gegevensbronnen,
* Data-Transport
* Apparaat en de verwerking van gebeurtenissen en
* Presentatie

![Threat Modeling voor Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png)

Het volgende diagram biedt een vereenvoudigde weergave van de IoT-architectuur van Microsoft die gebruikmaken van een gegevensstroom-Diagram model dat wordt gebruikt door het hulpprogramma Microsoft Threat Modeling:

![Threat Modeling voor Azure IoT met MS Threat Modeling hulpprogramma](media/iot-security-architecture/iot-security-architecture-fig3.png)

Het is belangrijk te weten dat de architectuur worden gescheiden van de mogelijkheden van het apparaat en de gateway. Deze methode kan de gebruiker voor het benutten van de gatewayapparaten die veiliger zijn: kan communiceren met de cloudgateway met behulp van veilige protocollen hiervoor meestal groter verwerkingsoverhead dat een systeemeigen apparaat - zoals een thermostaat - kan zijn Geef op de eigen. In de zone van de Azure-services, wordt ervan uitgegaan dat de Cloudgateway wordt vertegenwoordigd door de service Azure IoT Hub.

### <a name="device-and-data-sourcesdata-transport"></a>Apparaat- en bronnen/data-transport

Deze sectie behandelt de eerder beschreven door de lens van risicomodel architectuur en een overzicht van hoe u voor het oplossen van enkele van de intrinsieke problemen. In dit voorbeeld is gericht op de belangrijkste elementen van een risicomodel:

* Processen (zowel onder uw beheer- en externe items)
* Communicatie (ook wel gegevensoverdrachten)
* Opslag (ook wel gegevensarchieven)

#### <a name="processes"></a>Processen

In elk van de categorieën die worden beschreven in de Azure IoT-architectuur in dit voorbeeld wordt geprobeerd om het risico van een aantal verschillende bedreigingen tussen de verschillende fasen gegevens/bestaat in te: proces, communicatie en opslag. Hier volgt een overzicht van de meest voorkomende waarden voor de categorie 'proces', gevolgd door een overzicht van hoe deze bedreigingen beste kunnen worden verholpen:

**Adresvervalsing (spoofing) (S)**: een aanvaller kan cryptografische sleutelmateriaal ophalen van een apparaat op het niveau van de software of hardware en vervolgens toegang tot het systeem met een ander fysiek of virtueel apparaat onder de identiteit van het apparaat het sleutelmateriaal van is genomen. Een goede illustratie is extern besturingselementen die elke TV kunt inschakelen en die populaire prankster hulpprogramma's zijn.

**Denial of Service (D)**: een apparaat kan worden gerenderd waarvoor niet werkt of communiceren door interactie aangaan met keuzerondjes frequenties of knippen bedrading. Bijvoorbeeld kan geen een camera toezicht waarop de stroom of netwerk verbinding opzettelijk toenemende rapportgegevens, helemaal.

**Manipulatie (T)**: een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat waardoor de vervangen software gebruikmaken van de identiteit van de legitimiteit van het apparaat als het sleutelmateriaal of cryptografische faciliteiten sleutel materialen die beschikbaar voor het illegale programma waren. Een aanvaller kan bijvoorbeeld gebruikmaken van uitgepakte sleutelmateriaal onderscheppen en gegevens van het apparaat op het communicatiepad onderdrukken en vervang deze door false gegevens die met het gestolen sleutelmateriaal is geverifieerd.

**Vrijgeven van informatie (I)**: als het apparaat gezelschapsdieren software wordt uitgevoerd, dergelijke gezelschapsdieren software kan gegevens niet-geautoriseerde partijen mogelijk lekken. Een aanvaller kan bijvoorbeeld gebruikmaken van uitgepakte sleutelmateriaal zelf invoeren in het communicatiepad tussen het apparaat en een domeincontroller of het veld gateway of cloudgateway naar siphon uitschakelen informatie.

**Uitbreiding van bevoegdheden (E)**: een apparaat dat een specifieke functie biedt kan worden afgedwongen om iets anders te doen. Bijvoorbeeld kunt een klep dat openen halverwege is geprogrammeerd laten misleiden helemaal te openen.

| **Onderdeel** | **Threat** | **Risicobeperking** | **Risk** | **Implementatie** |
| --- | --- | --- | --- | --- |
| Apparaat |S |Identiteit toewijzen aan het apparaat en het apparaat te verifiëren |Apparaat of een deel van het apparaat vervangen door een ander apparaat. Hoe weet u dat het nu op het juiste apparaat? |Het apparaat, met behulp van Transport Layer Security (TLS) of IPSec-verificatie. Infrastructuur moet ondersteuning met behulp van vooraf gedeelde sleutel (PSK) op apparaten die volledige asymmetrische cryptografie kunnen niet worden verwerkt. Gebruikmaken van Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Toepassen tamperproof mechanismen voor het apparaat, bijvoorbeeld door waardoor het moeilijk voor onmogelijk om sleutels en andere cryptografische materiaal extraheren van het apparaat. |Het risico is als iemand het apparaat (fysieke storing) is geknoeid. Hoe weet u zeker weet, dat het apparaat is niet geknoeid met. |De meest effectieve vermindering is een vertrouwde platform module (TPM)-functie waarmee het opslaan van sleutels in speciale op chip circuits waaruit de sleutels kunnen niet worden gelezen, maar kunnen alleen worden gebruikt voor cryptografische bewerkingen uit waarmee de sleutel worden gebruikt, maar de sleutel nooit worden vermeld. Geheugen-codering van het apparaat. Sleutelbeheer voor het apparaat. De code voor ondertekening. | |
| E |Toegangsbeheer van het apparaat is. Autorisatieschema voor. |Als het apparaat kunt u afzonderlijke acties worden uitgevoerd op basis van de opdrachten van een externe bron, of zelfs waarmee is geknoeid sensoren, kan de aanval bewerkingen uit te voeren niet toegankelijk. |Autorisatieschema voor het apparaat dat | |
| Veldgateway |S |Verifiëren van de gateway veld Cloudgateway (zoals cert gebaseerd, PSK, of op basis van claims.) |Als iemand Veldgateway vervalsen kan, klikt u vervolgens het kan worden weergegeven als een apparaat. |TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Dezelfde sleutel problemen voor de opslag en attestation van apparaten in het algemeen – beste is TPM gebruiken. 6LowPAN-extensie voor IPSec-authenticatie voor de ondersteuning van draadloze Sensor netwerken (WSN). |
| TRID |Beveiligen van de Gateway veld tegen knoeien (TPM)? |Adresvervalsing (spoofing) aanvallen die ertoe verleiden om de cloud gateway denken dat deze met een veldgateway communiceert kan leiden tot het vrijgeven van informatie en geknoei met gegevens |Geheugen-versleuteling, TPM van, verificatie. | |
| E |Mechanisme voor toegangsbeheer voor Veldgateway | | | |

Hier volgen enkele voorbeelden van bedreigingen die in deze categorie:

Adresvervalsing (spoofing): Kan een aanvaller vanaf een apparaat, op de software of hardwareniveau, en vervolgens toegang tot die het systeem met een ander fysiek of virtueel apparaat onder de identiteit van het apparaat het sleutelmateriaal onttrokken cryptografische sleutelmateriaal extraheren.

**DOS-aanval**: een apparaat kan worden gerenderd waarvoor niet werkt of communiceren door interactie aangaan met keuzerondjes frequenties of knippen bedrading. Bijvoorbeeld kan geen een camera toezicht waarop de stroom of netwerk verbinding opzettelijk toenemende rapportgegevens, helemaal.

**Knoeien**: een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat waardoor de vervangen software gebruikmaken van de identiteit van de legitimiteit van het apparaat als het sleutelmateriaal of cryptografische faciliteiten sleutel materialen die beschikbaar voor het illegale programma waren.

**Knoeien**: een toezicht camera die wordt weergegeven een afbeelding zichtbare spectrum van een leeg gang kan worden gericht op een foto van een dergelijke hotellobby. Een sensor rook of brand kan iemand een lichter onder het bedrijf rapportage. In beide gevallen moet het apparaat mogelijk technisch volledig betrouwbare naar het systeem, maar het rapport gezelschapsdieren informatie.

**Knoeien**: een aanvaller kan gebruikmaken van uitgepakte sleutelmateriaal onderscheppen en gegevens van het apparaat op het communicatiepad onderdrukken en vervang deze door false gegevens die met het gestolen sleutelmateriaal is geverifieerd.

**Knoeien**: een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat waardoor de vervangen software gebruikmaken van de identiteit van de legitimiteit van het apparaat als het sleutelmateriaal of cryptografische faciliteiten sleutel materialen die beschikbaar voor het illegale programma waren.

**Vrijgeven van informatie**: als het apparaat gezelschapsdieren software wordt uitgevoerd, dergelijke gezelschapsdieren software kan gegevens niet-geautoriseerde partijen mogelijk lekken.

**Vrijgeven van informatie**: een aanvaller kan gebruikmaken van uitgepakte sleutelmateriaal zelf invoeren in de communicatie tussen de gateway-apparaat en een domeincontroller of een veld of de cloudgateway naar siphon uitschakelen informatie.

**DOS-aanval**: het apparaat kan worden uitgeschakeld of ingeschakeld op een modus waarbij de communicatie is niet mogelijk (dit is opzettelijk in veel industriële machines).

**Knoeien**: het apparaat opnieuw kan worden geconfigureerd om te werken in een onbekende met het controlesysteem (buiten bekende kalibreren parameters) staat en zodoende op gegevens die kan worden geïnterpreteerd

**Onrechtmatige uitbreiding van toegangsrechten**: een apparaat dat een specifieke functie biedt kan worden afgedwongen om iets anders te doen. Bijvoorbeeld kunt een klep dat openen halverwege is geprogrammeerd laten misleiden helemaal te openen.

**DOS-aanval**: het apparaat kan worden omgezet in een status waar communicatie niet mogelijk is.

**Knoeien**: het apparaat opnieuw kan worden geconfigureerd om te werken in een onbekende met het controlesysteem (buiten bekende kalibreren parameters) staat en zodoende op gegevens die kan worden geïnterpreteerd.

**Adresvervalsing (spoofing) / Tampering/Repudiation**: als niet zijn beveiligd (dit is bijna nooit het geval is bij de consument afstand besturingselementen), een aanvaller de status van een apparaat anoniem kunt bewerken. Een goede illustratie is extern besturingselementen die elke TV kunt inschakelen en die populaire prankster hulpprogramma's zijn.

#### <a name="communication"></a>Communicatie

Bedreigingen rond communicatiepad tussen apparaten, apparaten en gateways veld en apparaat en cloud-gateway. De volgende tabel bevat enkele richtlijnen rond sockets geopend op het apparaat/VPN:

| **Onderdeel** | **Threat** | **Risicobeperking** | **Risk** | **Implementatie** |
| --- | --- | --- | --- | --- |
| Apparaat IoT-Hub |TID |(D) TLS (PSK/RSA) voor het versleutelen van het verkeer |Afgeluisterd of dat de communicatie tussen het apparaat en de gateway wordt verstoord |Beveiliging op het protocolniveau. Met aangepaste protocollen moet u bedenken hoe ze beveiligen. In de meeste gevallen plaatsvindt de communicatie van het apparaat met de IoT-Hub (apparaat start de verbinding). |
| Apparaat-apparaat |TID |(D) TLS (PSK/RSA) voor het versleutelen van het verkeer. |Lezen van gegevens tijdens de overdracht tussen apparaten. Knoeit met de gegevens. Overbelasting van het apparaat met nieuwe verbindingen |Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP/CoAP. Met aangepaste protocollen moet u bedenken hoe ze beveiligen. De beperking voor de bedreiging DoS is het peer-apparaten via een cloud- of -gateway en hebben ze alleen act als clients op het netwerk. De peering kan leiden tot een rechtstreekse verbinding tussen de peers na dat is geleverd door de gateway |
| Externe entiteit apparaat |TID |Sterke koppelen van de externe entiteit op het apparaat |De verbinding met het apparaat afgeluisterd. De communicatie verstoren op het apparaat |De externe entiteit op het apparaat NFC/Bluetooth RP koppelen veilig. Het operationele deelvenster van het apparaat (fysiek) beheren |
| Veld Gateway Cloudgateway |TID |TLS (PSK/RSA) voor het versleutelen van het verkeer. |Afgeluisterd of dat de communicatie tussen het apparaat en de gateway wordt verstoord |Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP/CoAP). Met aangepaste protocollen moet u bedenken hoe ze beveiligen. |
| De Cloudgateway apparaat |TID |TLS (PSK/RSA) voor het versleutelen van het verkeer. |Afgeluisterd of dat de communicatie tussen het apparaat en de gateway wordt verstoord |Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP/CoAP). Met aangepaste protocollen moet u bedenken hoe ze beveiligen. |

Hier volgen enkele voorbeelden van bedreigingen die in deze categorie:

**DOS-aanval**: beperkte apparaten zijn doorgaans onder DoS threat wanneer ze actief naar binnenkomende verbindingen of ongevraagde datagrammen in een netwerk, luisteren omdat een aanvaller kunt veel verbindingen parallel openen en die niet worden deze service of -service ze langzaam, of het apparaat kan worden overspoeld met ongevraagd verkeer. In beide gevallen wordt kunt het apparaat effectief vastlopen op het netwerk.

**Adresvervalsing (spoofing), vrijgeven van informatie**: beperkte apparaten en apparaten voor speciale doeleinden vaak beschikken over een voor alle beveiligings-voorzieningen zoals wachtwoord of PINCODE beveiliging of zijn ze geheel afhankelijk van het netwerk, wat betekent dat ze geen toegang verlenen tot vertrouwende gegevens over wanneer een apparaat op hetzelfde netwerk is en dat netwerk wordt vaak alleen beveiligd door een gedeelde sleutel. Dat betekent dat bij het gedeelde geheim op apparaat of netwerk wordt vermeld, is het mogelijk voor het beheren van het apparaat of gegevens die van het apparaat te observeren.  

**Adresvervalsing (spoofing)**: een aanvaller kan intercept gedeeltelijk overschrijven van de verzending en vervalsen van de oorspronkelijke aanvrager (man-in het midden)

**Knoeien**: een aanvaller kan intercept gedeeltelijk overschrijven van de verzending en ONWAAR informatie verzenden 

**Vrijgeven van informatie:** een aanvaller kan op een uitzending afluisteren en informatie zonder toestemming verkrijgen **Denial of Service:** een aanvaller kan het signaal jam en informatie distributie weigeren

#### <a name="storage"></a>Storage

Elk gateway-apparaat en het veld heeft een vorm van opslag (tijdelijke voor queuing van de gegevens, het besturingssysteem (OS) installatiekopie opslag).

| **Onderdeel** | **Threat** | **Risicobeperking** | **Risk** | **Implementatie** |
| --- | --- | --- | --- | --- |
| Apparaatopslag |TRID |Versleuteling van opslag, ondertekening van de logboeken |Lezen van gegevens uit de opslag (PII-gegevens), gemanipuleerde telemetrische gegevens. Gemanipuleerde in de wachtrij of opdracht besturingselement gegevens uit de cache. Knoeien met configuration of firmware updatepakketten kan terwijl in de cache opgeslagen of lokaal in de wachtrij leiden tot OS en/of system-onderdelen wordt aangetast |Versleuteling, message authentication code (MAC) of digitale handtekening. Waar mogelijk, sterk toegangsbeheer via toegang tot bedrijfsbronnen (ACL's) of machtigingen beheren. |
| De installatiekopie van het besturingssysteem van het apparaat |TRID | |Gemanipuleerde OS / vervangen van de OS-componenten |OS-partitie, alleen-lezen, ondertekend installatiekopie van het besturingssysteem, versleuteling |
| Veldgateway-opslag (de gegevens queuing) |TRID |Versleuteling van opslag, ondertekening van de logboeken |Lezen van gegevens uit de opslag (PII-gegevens), telemetriegegevens, gemanipuleerde gemanipuleerde in de wachtrij of opdracht besturingselement gegevens uit de cache. Knoeien met configuration of firmware updatepakketten (dat is bestemd voor apparaten of veldgateway) kan terwijl in de cache opgeslagen of lokaal in de wachtrij leiden tot OS en/of system-onderdelen wordt aangetast |BitLocker |
| Installatiekopie van het veld Gateway besturingssysteem |TRID | |Gemanipuleerde OS / vervangen van de OS-componenten |OS-partitie, alleen-lezen, ondertekend installatiekopie van het besturingssysteem, versleuteling |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Apparaat- en gebeurtenis verwerking/cloud gateway zone

Een cloudgateway is een systeem dat Hiermee kunt externe communicatie van en naar de apparaten of veld gateways uit diverse verschillende sites op openbaar netwerk ruimte, meestal naar een cloud-gebaseerd beheer en het systeem voor analyse, een federatieve van dergelijke systemen. In sommige gevallen kan vergemakkelijken een cloudgateway onmiddellijk toegang voor apparaten voor speciale doeleinden van aansluitingen zoals tablets en telefoons. In de context die hier worden besproken, 'cloud' is bedoeld om te verwijzen naar een specifieke gegevensverwerkingssysteem die niet is gekoppeld aan dezelfde site als de gekoppelde apparaten of veld gateways en waar operationele maatregelen te voorkomen dat fysieke toegang gericht, maar is niet noodzakelijkerwijs een 'openbare cloud'-infrastructuur. Een cloudgateway kan mogelijk worden toegewezen in een netwerkvirtualisatie-overlay naar de cloudgateway en alle gekoppelde apparaten of het veld gateways van ander netwerkverkeer kastje. De cloudgateway zelf is niet een controlesysteem apparaat of een verwerking of opslagfaciliteit voor gegevens van apparaten; deze interface met de cloudgateway van de. De gateway cloud zone bevat de cloudgateway zelf samen met alle veld gateways en apparaten direct of indirect gekoppeld.

Cloudgateway is voornamelijk aangepaste ingebouwde stukje software uitgevoerd als een service met blootgestelde eindpunten waarmee veldgateway en apparaten verbinding maken. Het moet als zodanig ontworpen beveiligd zijn. Ga als volgt [SDL](http://www.microsoft.com/sdl) processen voor het ontwerpen en bouwen van deze service.

#### <a name="services-zone"></a>Services-zone

Een besturingssysteem (of een domeincontroller) is een oplossing voor software die is gekoppeld aan een apparaat of een veldgateway of cloudgateway omwille van een of meerdere apparaten beheren en/of voor het verzamelen en/of opslaan en/of apparaatgegevens voor presentatie of latere controledoeleinden analyseren. Besturingssystemen zijn de enige entiteiten in het bereik van deze discussie die onmiddellijk interactie met mensen vergemakkelijken. De uitzonderingen zijn tussenliggende fysieke besturingselement verwerkingsinformatie op apparaten, zoals een switch waarmee een gebruiker het apparaat uitschakelen of andere eigenschappen wijzigen en waarvoor er geen functioneel equivalent die digitaal toegankelijk is.

Tussenliggende fysieke besturingselement verwerkingsinformatie zijn deze waar de functie van de fysieke oppervlak van bestuur logica handtekeningcertificaatsleutel zodat een equivalente functie extern kan worden gestart of invoer conflicten met externe invoer kunnen worden vermeden – zoals intermediated besturingselement verwerkingsinformatie zijn conceptueel aan een lokale controlesysteem die gebruikmaakt van dezelfde onderliggende functionaliteit als elk ander systeem beheer op afstand die het apparaat kan worden gekoppeld aan parallel gekoppeld. Bovenste bedreigingen voor de cloud computing kunnen worden gelezen als [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) pagina.

## <a name="additional-resources"></a>Aanvullende resources

Raadpleeg voor meer informatie de volgende artikelen:

* [SDL-hulpmiddel voor het modelleren van Threat](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IoT reference architecture](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
