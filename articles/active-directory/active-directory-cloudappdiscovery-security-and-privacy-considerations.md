---
title: Cloud App Discovery beveiligings- en privacyoverwegingen | Microsoft Docs
description: Dit onderwerp beschrijft de beveiliging en privacy overwegingen ten aanzien van Cloud App Discovery.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 7775bad1503e62e1887e6d85f67c3107f48866bf
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery beveiligings- en privacyoverwegingen
Dit onderwerp wordt uitgelegd hoe gegevens die worden verzameld, verwerkt en beveiligd binnen Azure Active Directory Cloud App Discovery. Microsoft hecht veel belang aan het beveiligen van uw privacy en beveiliging van uw gegevens. Microsoft voldoet als u wilt beveiligen software development lifecycle procedures voor het besturingssysteem van een service. Beveiliging en bescherming van gegevens is een topprioriteit bij Microsoft.

> [!TIP] 
> Met de nieuwe zonder agent Cloud App Discovery in Azure Active Directory (Azure AD), die worden verhoogd door uitchecken [integratie met Microsoft Cloud App Security](https://portal.cloudappsecurity.com). 

## <a name="overview"></a>Overzicht
Cloud App Discovery is een functie van Azure AD en wordt gehost in Microsoft Azure.  
De Cloud App Discovery endpoint agent wordt gebruikt voor het verzamelen van detectiegegevens van de toepassing van beheerde IT-machines. De verzamelde gegevens wordt veilig via een versleuteld kanaal verzonden naar de Azure AD Cloud App Discovery-service. De Cloud App Discovery-gegevens voor een organisatie wordt vervolgens weergegeven in de Azure-portal. 

![De werking van Cloud App Discovery](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

De volgende secties volgt de beveiligde stroom van gegevens van uw organisatie naar de Cloud App Discovery-service en uiteindelijk naar de Cloud App Discovery-portal.

## <a name="collecting-data-from-your-organization"></a>Verzamelen van gegevens van uw organisatie
Om te gebruiken Azure Active Directory Cloud App discovery-functies voor het verkrijgen van inzicht in de toepassingen die worden gebruikt door werknemers in uw organisatie, moet u eerst de Azure AD Cloud App Discovery endpoint agent implementeren op computers in uw organisatie.

Beheerders van de Azure Active Directory-tenant (of hun gemachtigde) kunnen het agent-installatiepakket downloaden van de Azure-portal. De agent kan worden handmatig geïnstalleerd of geïnstalleerd via meerdere computers in de organisatie via SCCM of Groepsbeleid.

Zie voor meer instructies voor de implementatie-opties [Cloud App Discovery Groepsbeleid Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-the-agent"></a>Gegevens die worden verzameld door de agent
De gegevens die worden beschreven in de volgende lijst worden verzameld door de agent als een verbinding is gemaakt voor een webtoepassing. De gegevens worden alleen verzameld voor de toepassingen die de beheerder heeft geconfigureerd voor detectie. U kunt de lijst met cloud-apps die de agent via de Cloud App Discovery in Azure AD in de Microsoft bewaakt bewerken [Azure-portal](https://portal.azure.com/)onder **instellingen**->**verzamelen van gegevens**  -> **App-verzameling lijst**. 

**Informatie categorie**: gebruikersgegevens  
**Beschrijving**: de Windows-gebruikersnaam van het proces dat een aanvraag met de doel-webtoepassing (bijvoorbeeld DOMEIN\gebruikersnaam) en de Windows Security ID (SID) van de gebruiker gemaakt.

**Informatie categorie**: de verwerking van gegevens  
**Beschrijving**: de naam van het proces dat de aanvraag heeft ingediend met de doel-webtoepassing (bijvoorbeeld iexplore.exe)

**Informatie categorie**: computerinformatie  
**Beschrijving**: de machine NetBIOS-naam van de agent is geïnstalleerd.

**Informatie categorie**: informatie over App-verkeer  
**Beschrijving**: de volgende verbindingsinformatie:

* De bron (lokale computer) en doel-IP-adressen en poortnummers
* Het openbare IP-adres van de organisatie via welke de aanvraag gedeeld wordt.
* De tijd van de aanvraag
* De hoeveelheid verkeer verzonden en ontvangen
* De IP-versie (4 of 6)
* Voor de TLS-verbindingen: de host-naam van het doel van de Servernaamindicatie-extensie of het servercertificaat.

De volgende HTTP-gegevens:

* Methode (GET, POST, enz.)
* Protocol (HTTP/1.1, enz.)
* Tekenreeks van de gebruikersagent
* Hostnaam
* Doel-URI (met uitzondering van querytekenreeks)
* Informatie over inhoudstype
* Verwijzende site URL-gegevens (met uitzondering van de query-tekenreeks)

> [!NOTE]
> De bovenstaande HTTP-gegevens worden verzameld voor alle niet-versleutelde verbindingen.
> Voor de TLS-verbindingen, wordt deze informatie alleen vastgelegd wanneer de instelling 'Diepe inspectie' is ingeschakeld in de portal. 'ON' is de instelling standaard.
> Zie voor meer informatie hieronder en [ophalen van de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Naast de gegevens die de agent over de activiteit op het netwerk verzamelt, verzamelt het ook anonieme gegevens over
* Software en hardware-configuratie
* Foutenrapporten
* Gegevens over hoe de agent wordt gebruikt.


### <a name="how-the-agent-works"></a>De werking van de agent
Installatie van de agent bevat twee onderdelen:

* Een onderdeel van de gebruikersmodus
* Een onderdeel in kernelmodus stuurprogramma (Windows Filtering Platform stuurprogramma)

Wanneer de agent wordt eerst geïnstalleerd worden op de computer die vervolgens wordt een beveiligde verbinding met de Cloud App Discovery-service een vertrouwd certificaat voor computer-specifieke opgeslagen. Configuratie van beleid voor de agent periodiek opgehaald uit de Cloud App Discovery-service via deze beveiligde verbinding. Het beleid bevat informatie over welke cloudtoepassingen bewaken en Hiermee wordt aangegeven of Automatische updates moet worden ingeschakeld, onder andere.

Als het internetverkeer wordt verzonden en ontvangen op de virtuele machine van Internet Explorer en Chrome, de agent voor Cloud App Discovery analyseert het verkeer en haalt de metagegevens van de relevante (Zie de **gegevens verzameld door de agent** sectie hierboven).  
Elke minuut uploadt de agent u de metagegevens van de verzamelde naar de Cloud App Discovery-service via een versleuteld kanaal.

Het stuurprogrammaonderdeel versleuteld verkeer onderschept en zelf wordt ingevoegd in de versleutelde gegevensstroom. Meer informatie in de **gegevens van de versleutelde verbindingen (met diepe inspectie) onderscheppen** hieronder.

### <a name="respecting-user-privacy"></a>Privacy van gebruikers te respecteren
Ons doel is de beheerders van de hulpprogramma's voor het instellen van de balans tussen gedetailleerde beeldverwerkingstoepassingen in toepassing gebruiks- en privacy geschikt is voor hun organisatie te bieden. Daartoe bieden we de volgende knoppen op de instellingenpagina in de Portal:

* **Gegevensverzameling**: beheerders kunnen kiezen om op te geven welke toepassingen of ze krijgen discovery-gegevens willen voor categorieën.
* **Met diepe inspectie**: beheerders kunnen hebt gekozen om op te geven als de agent verzamelt HTTP-verkeer voor SSL/TLS-verbindingen (aka **'Diepe inspectie'**). Meer informatie over dit in de volgende sectie.
* **Toestemming geven opties**: beheerders kunnen de Cloud App Discovery-portal gebruiken om te kiezen of gebruikers van de gegevensverzameling door de agent en of het nodig van de gebruiker toestemming geven voordat de agent wordt gestart voor het verzamelen van gebruikersgegevens.

De Cloud App Discovery endpoint agent verzamelt alleen de informatie die wordt beschreven in de **gegevens verzameld door de agent** sectie hierboven.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Gegevens van de versleutelde verbindingen (met diepe inspectie) onderscheppen
Zoals eerder vermeld, kunnen beheerders de agent voor het bewaken van gegevens van versleutelde verbindingen ('diepe inspectie') configureren. TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) is een van de meest gangbare protocollen in gebruik is op het Internet vandaag. Door het versleutelen van communicatie met TLS, kan een client een vertrouwelijk communicatiekanaal met een webserver; maken. TLS bevat essentiële beveiliging voor het doorgeven van referenties voor verificatie en voorkomen dat de openbaarmaking van gevoelige informatie.

Terwijl het end-to-end beveiligde versleutelde kanaal geleverd door TLS kunt u belangrijke en privacybescherming, wordt het protocol vaak doelwit voor schadelijke of slechte doeleinden. Veel dus in feite wordt die TLS vaak genoemd 'universal firewall-bypass-protocol'. De hoofdmap van het probleem is dat de meeste firewalls kan niet controleren van TLS-communicatie, omdat de toepassingslaag-gegevens worden versleuteld met SSL. Weten, aanvallers vaak maken gebruik van TLS voor het leveren van schadelijke nettoladingen aan een gebruiker die erop kunnen vertrouwen dat zelfs de meest intelligent toepassingslaag firewalls volledig blind in TLS zijn en moeten de TLS-communicatie tussen hosts gewoon relay. Eindgebruikers vaak maken gebruik van TLS om over te slaan toegangsbeheer afgedwongen door hun zakelijke firewalls en proxyservers bevinden, met behulp van deze verbinding maken en voor niet-TLS tunnelingprotocollen via de firewall die anders mogelijk geblokkeerd door het beleid voor openbare proxy's.

Met diepe inspectie kan de agent voor Cloud App Discovery om te fungeren als een vertrouwde man-in-the-middle. Wanneer een clientaanvraag wordt gedaan voor toegang tot een beveiligde HTTPS-bron, het stuurprogramma Endpoint Agent de verbinding onderschept en er wordt dat een nieuwe verbinding met de doelserver om te worden opgehaald van de SSL-certificaat namens de client. De agent vervolgens wordt gecontroleerd of het certificaat kan worden vertrouwd (door te controleren of deze niet is ingetrokken en er andere certificaat controles worden uitgevoerd) en als deze op te geven, klikt u vervolgens de Endpoint Agent kopieert de gegevens van het servercertificaat en maakt een eigen server certificaat--bekend als een certificaat worden onderschept--die gegevens. Het certificaat worden onderschept is ondertekend op het moment door de agent eindpunt met een basiscertificaat dat in het vertrouwde certificaatarchief van Windows is geïnstalleerd. Dit zelfondertekende basiscertificaat is niet exporteerbaar gemarkeerd en is ACL moest beheerders. Het is bedoeld om u te verlaten nooit de machine waarop deze is gemaakt. Wanneer de eindgebruiker clienttoepassing het certificaat worden onderschept ontvangt, wordt deze het vertrouwen omdat het de certificaatketen helemaal tot aan het basiscertificaat met succes kunt valideren. Dit proces is voornamelijk transparant uit oogpunt van een eindgebruiker met enkele aanvullende opmerkingen zoals hieronder wordt beschreven.

Doordat diepe inspectie Cloud App Discovery Endpoint Agent ontsleutelen en inspecteren communicatie TLS versleuteld, zodat de service Ruis reduceren en inzicht bieden over het gebruik van de versleutelde cloud-apps.

#### <a name="a-word-of-caution"></a>Waarschuwing
Voordat u met diepe inspectie inschakelt, wordt sterk aangeraden dat u uw bedoelingen uw juridische en HR afdelingen communiceren en hun toestemming verkrijgen. Persoonlijke gecodeerde communicatie van de eindgebruiker te bekijken, kan een gevoelige onderwerp, zijn voor de hand. Voordat u een productie uitrollen grondige inspectie, Controleer of de beveiliging van uw bedrijf en beleidsregels voor aanvaardbaar gebruik zijn bijgewerkt om aan te geven dat gecodeerde communicatie worden gecontroleerd. Melding voor gebruikers en uitsluiting van sites geacht gevoelige (bijvoorbeeld Bank- en medische sites) kunnen ook nodig zijn als u Cloud Appdetectie configureren zodat ze te controleren. Zoals eerder vermeld, kunnen beheerders de Cloud App Discovery-portal gebruiken om te kiezen of gebruikers van de gegevensverzameling door de agent een melding ontvangen, en of u toestemming van de gebruiker vereist voordat de agent wordt gestart met het verzamelen van gebruikersgegevens.

### <a name="known-issues-and-drawbacks"></a>Bekende problemen en nadelen
Er zijn enkele gevallen waarbij de eindgebruiker mogelijk van invloed op TLS onderschept:

* Uitgebreide validatie (EV)-certificaten weergeven de adresbalk van de webbrowser groen om te fungeren als een visuele aanwijzing dat u een vertrouwde website bezoeken. TLS inspectie EV geen duplicaat in het certificaat dat deze aan de client, uitgeeft zodat de websites die gebruikmaken van validatiecertificaten werken normaal, maar de adresbalk groen weergegeven.  
* Openbare sleutel vastmaken (ook wel bekend als het certificaat vastmaken) zijn ontworpen om te helpen om gebruikers te beschermen tegen man-in-the-middle-aanvallen en rogue certificeringsinstanties. Wanneer het basiscertificaat voor een vastgezette site komt niet overeen met een van de bekende goede CA, wordt de verbinding met een fout geweigerd in de browser. Omdat TLS worden onderschept, in feite een man-in-the-middle, mislukken deze verbindingen.
* Als gebruikers op het vergrendelingspictogram in de browser adres balk browser om te controleren van de site-informatie, zien ze een keten eindigt op de certificeringsinstantie die wordt gebruikt voor het ondertekenen van certificaat van de website, maar in plaats daarvan een certificaatketen eindigt met het Windows vertrouwd certificaatarchief.

Als u de instanties van deze problemen, we bijhouden van cloudservices- en clienttoepassingen bekend zijn bij gebruik uitgebreide validatie of vastmaken van openbare sleutel en de Endpoint Agent instrueren om te voorkomen dat betrokken verbindingen onderscheppen. Zelfs in dergelijke gevallen echter nog steeds ontvangt u meldingen van het gebruik van deze cloud-apps en de hoeveelheid gegevens die worden overgedragen, maar omdat ze niet grondige geïnspecteerd, geen informatie over hoe de apps die zijn gebruikt zijn beschikbaar.

## <a name="sending-data-to-cloud-app-discovery"></a>Verzenden van gegevens naar de Cloud App Discovery
Nadat de metagegevens zijn verzameld door de agent, wordt het tijdelijk opgeslagen op de computer voor maximaal één minuut of totdat de gegevens in de cache heeft een grootte van 5MB bereikt. Het is vervolgens gecomprimeerd en verzonden via een beveiligde verbinding met de Cloud App Discovery-service.

Als de agent kan niet communiceren met de Cloud App Discovery-service voor een of andere reden is, worden de verzamelde metagegevens worden opgeslagen in een lokaal bestandscache die alleen toegankelijk zijn voor bevoegde gebruikers op de computer (zoals de groep Administrators).  
De agent probeert automatisch opnieuw te verzenden van de metagegevens in cache totdat deze is ontvangen door de Cloud App Discovery-service.

## <a name="receiving-the-data-at-the-service-end"></a>Ontvangen van de gegevens aan het einde van de service
De agents voor Cloud App Discovery-service met behulp van de machine specifieke certificaat voor clientverificatie bovengenoemde en verzendt gegevens via een versleuteld kanaal worden geverifieerd.  
Analysepijplijn van de Cloud App Discovery-service verwerkt metagegevens voor elke klant afzonderlijk door het partitioneren van het logisch via alle stadia van de pijplijn analytics.
De metagegevens van de geanalyseerde stations diverse rapporten in de portal.

De niet-verwerkte metagegevens en de metagegevens van de geanalyseerde opgeslagen gedurende 180 dagen. Bovendien kunnen klanten ervoor kiezen om vast te leggen van de geanalyseerde metagegevens in een Azure blob storage-account van hun keuze.
Dit is handig voor offline-analyse van metagegevens, evenals langer bewaren van de gegevens.

## <a name="accessing-the-data-using-the-azure-portal"></a>Toegang tot de gegevens met behulp van de Azure-portal
In een poging de metagegevens die zijn verzameld om veilig te houden, standaard hebben alleen globale beheerders van de tenant die toegang tot de Cloud App Discovery-functie in de Azure portal.  
Beheerders kunnen echter kiezen deze toegang verlenen aan andere gebruikers of groepen overdragen.

> [!NOTE]
> Zie voor meer informatie [ophalen van de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Elke gebruiker toegang tot de gegevens in de portal moet beschikken over een Azure AD Premium-licentie.

## <a name="additional-resources"></a>Aanvullende resources
* [Hoe kan ik cloudapps die worden gebruikt in mijn organisatie detecteren](active-directory-cloudappdiscovery-whatis.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

