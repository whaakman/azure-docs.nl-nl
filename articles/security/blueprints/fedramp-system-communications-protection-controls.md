---
title: FedRAMP Azure blauwdruk Automation - systeem en beveiliging van communicatie
description: Webtoepassingen voor FedRAMP - systeem en beveiliging van communicatie
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: b96cdef1-ce3a-4f73-9a9e-f2cbd056d485
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 02e2d07eb29d0d5d436afed1cdab4fe710674a8c
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="system-and-communications-protection-sc"></a>Systeem en beveiliging van communicatie (SC)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-sc-1"></a>NIST 800 53 besturingselement SC-1

#### <a name="system-and-communications-protection-policy-and-procedures"></a>Systeem en het beveiligingsbeleid van de communicatie en Procedures

**SC-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een systeem- en beveiligingsbeleid die zijn gericht op doel, bereik, rollen, taken, management het streven, samenwerking tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van het systeem en communicatie protection-beleid en bijbehorende communicatie beveiliging controles; en controleert en updates van het huidige systeem- en beveiligingsbeleid [toewijzing: organisatie gedefinieerde frequentie]; en -procedures voor systeem- en beveiliging [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Op bedrijfsniveau systeem- en beveiligingsbeleid van de klant en -procedures mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-2"></a>NIST 800 53 besturingselement SC-2

#### <a name="application-partitioning"></a>Partitioneren van de toepassing

**SC-2** het informatiesysteem gebruiker-functionaliteit (met inbegrip van gebruikersinterface) gescheiden van informatie system management-functionaliteit.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk scheidt functionaliteit van de gebruiker van de system management-functionaliteit afdwinging van logische toegang besturingselementen en systeemarchitectuur. Functionaliteit van de gebruiker is beperkt tot klant geïmplementeerde toepassing webinterfaces. Interfaces voor de system management-functionaliteit zijn gescheiden van gebruikersinterfaces. Alle beheer-connectiviteit is via een beveiligde bastion host (jumpbox) zich in een subnet management met netwerkbeveiligingsgroepen om te beperken van toegang tot productiebronnen naar gelang van toepassing. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-3"></a>NIST 800 53 besturingselement SC-3

#### <a name="security-function-isolation"></a>Functie isolatie

**SC-3** het informatiesysteem isoleert beveiligingsfuncties van de functies niet voor beveiliging.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows houdt de uitvoering van de afzonderlijke domeinen voor elk proces dat wordt uitgevoerd door een persoonlijke virtuele adresruimte toewijzen aan elk proces. Bovendien de oplossing implementeert een architectuur en toegang tot de besturingselementen die zijn ontworpen om te isoleren van beveiligingsfuncties indien nodig. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-4"></a>NIST 800 53 besturingselement SC-4

#### <a name="information-in-shared-resources"></a>Gegevens in gedeelde bronnen

**SC-4** het informatiesysteem wordt voorkomen dat onbevoegde en onbedoeld overdracht via gedeelde systeembronnen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Het besturingssysteem beheert resources (bijvoorbeeld geheugen, opslag), zodat informatie alleen toegankelijk voor gebruikers en rollen met de juiste machtigingen is. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-5"></a>NIST 800 53 besturingselement SC-5

#### <a name="denial-of-service-protection"></a>Denial of Service-beveiliging

**SC-5** het informatiesysteem wordt beschermd tegen of beperkt de gevolgen van de volgende typen aanvallen: [toewijzing: organisatie gedefinieerde typen denial of service-aanvallen of verwijzingen naar bronnen voor dergelijke gegevens] door om [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert een toepassingsgateway die load balancing mogelijkheden en bevatten een web application firewall. Geïmplementeerde virtuele machines ondersteunen de weblaag, databaselaag en Active Directory zijn geïmplementeerd in een schaalbare beschikbaarheidsset. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-6"></a>NIST 800 53 besturingselement SC-6

#### <a name="resource-availability"></a>Beschikbaarheid van resources

**SC-6** het informatiesysteem beveiligt de beschikbaarheid van resources door toe te wijzen [toewijzing: organisatie gedefinieerde resources] met [selectie (één of meer) prioriteit; quotum; [Toewijzing: organisatie gedefinieerd beveiligingsmaatregelen]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Elke Windows-proces biedt de resources die nodig zijn voor het uitvoeren van een programma. Prioriteit van de resource wordt beheerd door het besturingssysteem. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-7a"></a>NIST 800 53 besturingselement SC-7.

#### <a name="boundary-protection"></a>Beveiliging van de grens

**SC 7.** het informatiesysteem monitors en communicatie beheert op de externe grenzen van het systeem en op interne grenzen van de sleutel in het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk een Application Gateway wordt geïmplementeerd, de load balancer en netwerkbeveiligingsgroepen om te bepalen met bewerkingen op de externe grenzen en tussen de interne subnetten configureert. Toepassingsgateway, load balancer en network security groep gebeurtenissen en logboeken met diagnostische gegevens worden verzameld door OMS Log Analytics om toe te staan van de bewaking van de klant. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-7b"></a>NIST 800 53 besturingselement SC-7.b

#### <a name="boundary-protection"></a>Beveiliging van de grens

**SC 7.b** subnetwerken voor openbaar toegankelijk systeemonderdelen die door het informatiesysteem wordt geïmplementeerd [selectie: fysiek; logisch] wordt gescheiden van het interne organisatie-netwerken.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert-resources in een architectuur met een afzonderlijke web subnet, database-subnet, Active Directory-subnet en management subnet. Subnetten logisch worden gescheiden door netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten voor verkeer tussen subnetten alleen noodzakelijke systeem en de beheerfunctionaliteit te beperken (bijvoorbeeld externe verkeer geen toegang tot de database, beheer, (of Active Directory-subnetten). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-7c"></a>NIST 800 53 besturingselement SC-7.c

#### <a name="boundary-protection"></a>Beveiliging van de grens

**SC 7.c** het informatiesysteem verbinding maakt met externe netwerken of informatiesystemen beperken tot beheerde interfaces die bestaan uit grens beveiliging apparaten gerangschikt in overeenstemming met een architectuur van de beveiliging van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert een toepassingsgateway voor het beheren van externe verbindingen met een webtoepassing die door de klant is geïmplementeerd. Externe verbindingen voor toegang tot zijn beperkt tot een bastionomgeving host / jumpbox geïmplementeerd in een subnet management met netwerkbeveiligingsregels toegepast op externe verbindingen beperken tot geautoriseerde IP-adressen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-3"></a>NIST 800 53 besturingselement SC-7 (3)

#### <a name="boundary-protection--access-points"></a>Beveiliging van de grens | Toegangspunten

**SC-7 (3)** de organisatie beperkt het aantal externe verbindingen met het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert twee openbare IP-adressen: die is gekoppeld aan de toepassingsgateway; die is gekoppeld aan de host van de bastionomgeving management / jumpbox. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-4a"></a>Een van NIST 800 53 besturingselement SC-7 (4)

#### <a name="boundary-protection--external-telecommunications-services"></a>Beveiliging van de grens | Externe telecommunicatieservices

**SC-7 (4) een** de organisatie implementeert een beheerde-interface voor elke externe telecommunicatie-service.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert twee openbare IP-adressen: die is gekoppeld aan de toepassingsgateway; die is gekoppeld aan de host van de bastionomgeving management / jumpbox. Beheer van deze interfaces is door software gedefinieerde netwerken ingeschakeld. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-4b"></a>NIST 800 53 besturingselement SC-7 (4) .b

#### <a name="boundary-protection--external-telecommunications-services"></a>Beveiliging van de grens | Externe telecommunicatieservices

**SC-7 (4) .b** de organisatie stelt een beleid van de stroom verkeer voor elke beheerde interface.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert twee openbare IP-adressen: die is gekoppeld aan de toepassingsgateway; die is gekoppeld aan de host van de bastionomgeving management / jumpbox. Beheer van deze interfaces is door software gedefinieerde netwerken ingeschakeld. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-4c"></a>.C van NIST 800 53 besturingselement SC-7 (4)

#### <a name="boundary-protection--external-telecommunications-services"></a>Beveiliging van de grens | Externe telecommunicatieservices

**SC-7 (4) .c** de organisatie beveiligt de vertrouwelijkheid en integriteit van de informatie wordt verzonden via elke interface.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De web application gateway geïmplementeerd door deze blauwdruk Azure is geconfigureerd met een HTTPS-listener, daaropvolgende vertrouwelijkheid en integriteit van communicatie-sessies. Extern bureaublad-verbindingen naar de jumpbox zijn eveneens versleuteld bieden vertrouwelijkheid en integriteit. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-4d"></a>.D van NIST 800 53 besturingselement SC-7 (4)

#### <a name="boundary-protection--external-telecommunications-services"></a>Beveiliging van de grens | Externe telecommunicatieservices

**SC-7 (4) .d** elke uitzondering aan het beleid van de stroom verkeer met een ondersteunende missie/zakelijke behoeften en de duur van nodig en documenten desgewenst de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Klanten zijn niet verantwoordelijk voor het datacenter bewerkingen (waaronder telecommunicatieservices). Alle telecommunicatie services zijn opgegeven en beheerd door Microsoft Azure. Dit besturingselement is overgenomen van Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-4e"></a>CF,o.e van NIST 800 53 besturingselement SC-7 (4)

#### <a name="boundary-protection--external-telecommunications-services"></a>Beveiliging van de grens | Externe telecommunicatieservices

**CF,o.e SC-7 (4)** De organisatie beoordeelt uitzonderingen aan het beleid van de stroom verkeer [toewijzing: organisatie gedefinieerde frequentie] en verwijdert u de uitzonderingen die niet langer worden ondersteund door een expliciete missie/zakelijke behoeften.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Klanten zijn niet verantwoordelijk voor het datacenter bewerkingen (waaronder telecommunicatieservices). Alle telecommunicatie services zijn opgegeven en beheerd door Microsoft Azure. Dit besturingselement is overgenomen van Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-5"></a>NIST 800 53 besturingselement SC-7 (5)

#### <a name="boundary-protection--deny-by-default--allow-by-exception"></a>Beveiliging van de grens | Standaard weigeren / toestaan door uitzondering

**SC-7 (5)** het informatiesysteem op de beheerde interfaces communicatie netwerkverkeer weigert standaard en kunt u netwerkverkeer van de communicatie door uitzondering (dat wil zeggen, Alles weigeren, mogelijk vanwege uitzondering).

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | RuleSets toegepast beveiligingsgroepen die zijn geïmplementeerd door deze Azure blauwdruk van het netwerk zijn geconfigureerd met behulp van een schema weigeren standaard. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-7"></a>NIST 800 53 besturingselement SC-7 (7)

#### <a name="boundary-protection--prevent-split-tunneling-for-remote-devices"></a>Beveiliging van de grens | Gesplitste Tunneling voor externe apparaten voorkomen

**SC-7 (7)** het informatiesysteem in combinatie met een extern apparaat voorkomt dat het apparaat uit tegelijk tot stand brengen van verbindingen met het systeem niet extern en communiceert via een andere verbinding met de bronnen in externe netwerken.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Configuratiebeleid voor externe apparaten op ondernemingsniveau van de klant mogelijk adres split tunneling. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-8"></a>NIST 800 53 besturingselement SC-7 (8)

#### <a name="boundary-protection--route-traffic-to-authenticated-proxy-servers"></a>Beveiliging van de grens | Verkeer leiden met geverifieerde proxyservers

**SC-7 (8)** de systeemroutes informatie [toewijzing: interne communicatie organisatie gedefinieerd verkeer] naar [toewijzing: externe netwerken organisatie gedefinieerd] via geverifieerde proxyservers op de beheerde interfaces.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de klant gedefinieerde routinggegevens via een geverifieerde proxyserver tot een extern netwerk. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-10"></a>NIST 800 53 besturingselement SC-7 (10)

#### <a name="boundary-protection--prevent-unauthorized-exfiltration"></a>Beveiliging van de grens | Voorkomen dat onbevoegde Exfiltration

**SC-7 (10)** de organisatie wordt voorkomen dat de niet-geautoriseerde exfiltration van informatie over beheerde interfaces.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het verhinderen van niet-geautoriseerde exfiltration van informatie over beheerde interfaces. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-12"></a>NIST 800 53 besturingselement SC-7 (12)

#### <a name="boundary-protection--host-based-protection"></a>Beveiliging van de grens | Beveiliging op basis van host

**SC-7 (12)** de organisatie implementeert [toewijzing: mechanismen voor het beveiligen van host gebaseerde grens organisatie gedefinieerd] op [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Virtuele machines die zijn geïmplementeerd door deze blauwdruk Azure zijn geconfigureerd met een host gebaseerde firewall is ingeschakeld. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-13"></a>NIST 800 53 besturingselement SC-7 (13)

#### <a name="boundary-protection--isolation-of-security-tools--mechanisms--support-components"></a>Beveiliging van de grens | Isolatie van beveiligingsprogramma's / mechanismen / onderdelen

**SC-7 (13)** de organisatie zondert [toewijzing: organisatie gedefinieerde informatie beveiligingsprogramma's, mechanismen en ondersteuning-onderdelen] scheiden van andere onderdelen van het systeem interne informatie door het implementeren van fysiek subnetwerken met beheerde interfaces voor andere onderdelen van het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert-resources in een architectuur met een afzonderlijke management-subnet voor de implementatie van de klant van informatie beveiligingsprogramma's en onderdelen. Subnetten zijn logisch van elkaar gescheiden door netwerkbeveiligingsgroepen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-18"></a>NIST 800 53 besturingselement SC-7 (18)

#### <a name="boundary-protection--fail-secure"></a>Beveiliging van de grens | Beveiligde mislukt

**SC-7 (18)** het systeem gegevens veilig in geval van een operationeel storing van een apparaat van de beveiliging grens is mislukt.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er zijn geen apparaten van de beveiliging fysieke begrenzing van binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert geografisch gescheiden en redundante gatewayservers en de SSL VPN. Wanneer een Gateway-systeem uitvalt, veilig is mislukt en toegang wordt beperkt tot de omgeving. Om te kunnen geen verbinding maken met de Microsoft Azure-omgeving, moet een gebruiker een afzonderlijke verbinding met een actieve gatewayserver beheerd door Microsoft Azure maken. <br /> Bovendien als Microsoft Azure-netwerk-apparaten (inclusief randrouters, toegang routers, load balancers, aggregatie-switches en blijven) is mislukt, de betrokken circuit verbinding wordt verbroken, waardoor veilig mislukken. Een storing van een Microsoft Azure-netwerkapparaat kan niet leiden tot of ervoor zorgen dat gegevens buiten het systeem in te voeren van het apparaat, noch een storing kunt verlenen onbevoegde informatie release. De ingebouwde redundantie kunt u Microsoft Azure-activa zijn niet van invloed op de beschikbaarheid. |


 ### <a name="nist-800-53-control-sc-7-20"></a>NIST 800 53 besturingselement SC-7 (20)

#### <a name="boundary-protection--dynamic-isolation--segregation"></a>Beveiliging van de grens | Dynamische isolatie / scheiding

**SC-7 (20)** het informatiesysteem biedt de mogelijkheid dynamisch isolaat/scheiden [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie] uit andere onderdelen van het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het systeem heeft de mogelijkheid om te isoleren dynamisch klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-7-21"></a>NIST 800 53 besturingselement SC-7 (21)

#### <a name="boundary-protection--isolation-of-information-system-components"></a>Beveiliging van de grens | Isolatie van onderdelen van het systeem

**SC-7 (21)** mechanismen voor het beveiligen van grens voor het scheiden van de veiligheidsmaatregelen voor de organisatie [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie] ondersteunt [toewijzing: missies organisatie gedefinieerd en/of bedrijfsfuncties].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert-resources in een architectuur met een afzonderlijke web subnet, database-subnet, Active Directory-subnet en management subnet. Subnetten worden logisch gescheiden door netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten voor verkeer tussen subnetten alleen noodzakelijke systeem en de beheerfunctionaliteit te beperken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-8"></a>NIST 800 53 besturingselement SC-8

#### <a name="transmission-confidentiality-and-integrity"></a>Transmission vertrouwelijkheid en integriteit.

**SC-8** het informatiesysteem beveiligt de [selectie (een of meer): vertrouwelijkheid, integriteit] van verzonden gegevens.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | (((1) SI-8-implementatie voldoet aan deze eis. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-8-1"></a>NIST 800 53 besturingselement SC-8 (1)

#### <a name="transmission-confidentiality-and-integrity--cryptographic-or-alternate-physical-protection"></a>Transmission vertrouwelijkheid en integriteit. | Cryptografische of een andere fysieke beveiliging

**SC-8 (1)** cryptografische mechanismen worden door het informatiesysteem wordt geïmplementeerd [selectie (een of meer): voorkomen dat onbevoegde openbaarmaking van informatie; wijzigingen om informatie te detecteren] tijdens de verzending tenzij anders wordt beveiligd door [toewijzing: organisatie gedefinieerde alternatieve fysieke beveiliging].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk configureert bronnen te laten communiceren met alleen beveiligde protocollen. Het onderdeel WAF van de toepassingsgateway is geconfigureerd voor de schrijver van externe gebruikt via HTTPS/TLS accepteren en communiceren met de back-endpool alleen via HTTPS/TLS. SQL Server is geconfigureerd voor communicatie alleen via HTTPS/TLS. Extern bureaublad-services zijn geconfigureerd voor gebruik van beveiligde verbindingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-10"></a>NIST 800 53 besturingselement SC-10

#### <a name="network-disconnect"></a>Netwerk verbinding verbreken

**SC-10** het informatiesysteem beëindigt de netwerkverbinding die zijn gekoppeld aan een communicatiesessie aan het einde van de sessie of na [toewijzing: organisatie gedefinieerde periode] van inactiviteit.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Verificatie voor extern bureaublad-sessies wordt beheerd door Active Directory. Zodra de toegang is uitgeschakeld voor een gebruiker in Active Directory, is externe sessies onmiddellijk beëindigd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-12"></a>NIST 800 53 besturingselement SC-12

#### <a name="cryptographic-key-establishment-and-management"></a>Cryptografische sleutel inrichting en beheer

**SC-12** vaststelt en beheerd cryptografiesleutels voor vereiste cryptografie van het in het informatiesysteem in overeenstemming met de organisatie [toewijzing: vereisten voor het genereren van sleutels, distributie, organisatie gedefinieerd opslag, toegang en vernietiging].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert een Azure Sleutelkluis. Met Azure Key Vault kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Azure Sleutelkluis kunt genereren van sleutels met behulp van een FIPS 140-2 level 2 hardware security module (HSM) genereren van sleutels mogelijkheid. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-12-1"></a>NIST 800 53 besturingselement SC-12 (1)

#### <a name="cryptographic-key-establishment-and-management--availability"></a>Cryptografische sleutel inrichting en beheer | Beschikbaarheid

**SC-12 (1)** de organisatie onderhoudt beschikbaarheid van gegevens in het geval van het verlies van cryptografische sleutels die door gebruikers.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure Sleutelkluis wordt gebruikt voor het opslaan van de cryptografische sleutels en geheimen in deze blauwdruk Azure gebruikt. Sleutelkluis stroomlijnt het beheerproces voor sleutels die toegang en gegevens te versleutelen. De volgende verificators worden opgeslagen in de Sleutelkluis: Azure-wachtwoord voor account implementeren, administrator-wachtwoord voor virtuele machine, het wachtwoord van SQL Server-serviceaccount. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-12-2"></a>NIST 800 53 besturingselement SC-12 (2)

#### <a name="cryptographic-key-establishment-and-management--symmetric-keys"></a>Cryptografische sleutel inrichting en beheer | Symmetrische sleutels

**SC-12 (2)** de organisatie produceert, beheert en distribueert symmetrische cryptografische sleutels met [selectie: NIST FIPS-compatibele; Sleutelbeheer NSA-goedgekeurde] technologie en processen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure Sleutelkluis wordt gebruikt voor het produceren, beheren en distribueren van de cryptografische sleutels. Azure Sleutelkluis kunt genereren van sleutels met behulp van een FIPS 140-2 level 2 hardware security module (HSM) genereren van sleutels mogelijkheid. Sleutels worden opgeslagen en beheerd in gecodeerde containers in Azure Sleutelkluis. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-12-3"></a>NIST 800 53 besturingselement SC-12 (3)

#### <a name="cryptographic-key-establishment-and-management--asymmetric-keys"></a>Cryptografische sleutel inrichting en beheer | Asymmetrische sleutels

**SC-12 (3)** de organisatie produceert, beheert en distribueert asymmetrische cryptografische sleutels met [selectie: Sleutelbeheer NSA goedgekeurd technologie en processen; goedgekeurde klasse 3 van PKI-certificaten of prepositioned sleutelmateriaal; goedgekeurde certificaten voor PKI klasse 3 of 4 van de klasse en hardware-beveiligingstokens die de persoonlijke sleutel van de gebruiker te beschermen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het opstellen, beheren en distribueren van asymmetrische cryptografische sleutels (als ze worden gebruikt binnen resources klant geïmplementeerd). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-13"></a>NIST 800 53 besturingselement SC-13

#### <a name="cryptographic-protection"></a>Cryptografische bescherming

**SC-13** door het informatiesysteem geïmplementeerd [toewijzing: organisatie gedefinieerd cryptografische gebruikt en het type van cryptografie vereist voor elke toepassing] overeenkomstig de richtlijnen voor federale wetgeving, Executive Orders, beleidsregels, voorschriften, en standaarden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Windows-verificatie, extern bureaublad en BitLocker worden gebruikt door deze blauwdruk Azure. Deze onderdelen kunnen zijn afhankelijk van FIPS 140 gevalideerd cryptografiemodules worden geconfigureerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-15a"></a>NIST 800 53 besturingselement SC-15.a

#### <a name="collaborative-computing-devices"></a>Samenwerking Computing apparaten

**SC 15.a** informatie mogen niet worden externe activering van samenwerking apparaten met de volgende uitzonderingen: [toewijzing: organisatie gedefinieerde uitzonderingen waarbij externe activering is toegestaan].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen gezamenlijke computing apparaten geïmplementeerd als onderdeel van deze blauwdruk Azure. Opmerking: Er zijn fysieke gezamenlijke apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-15b"></a>NIST 800 53 besturingselement SC-15.b

#### <a name="collaborative-computing-devices"></a>Samenwerking Computing apparaten

**SC 15.b** het informatiesysteem biedt een expliciete vermelding van de gebruikers te fysiek aanwezig te zijn op de apparaten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen gezamenlijke computing apparaten geïmplementeerd als onderdeel van deze blauwdruk Azure. Opmerking: Er zijn fysieke gezamenlijke apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-17"></a>NIST 800 53 besturingselement SC-17

#### <a name="public-key-infrastructure-certificates"></a>Certificaten voor openbare-sleutelinfrastructuur

**SC-17** de organisatie problemen met certificaten voor openbare sleutels onder een [toewijzing: organisatie gedefinieerd certificaatbeleid] of certificaten voor openbare sleutels verkrijgt van een goedgekeurde-provider.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een op bedrijfsniveau public key infrastructure voor uitgifte van certificaten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-18a"></a>NIST 800 53 besturingselement SC-18.a

#### <a name="mobile-code"></a>Mobiele Code

**SC 18.a** de organisatie en niet acceptabel mobiele code en mobiele code technologieën worden gedefinieerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau system en communicatie beveiliging procedures mogelijk en niet acceptabel mobiele code definiëren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-18b"></a>NIST 800 53 besturingselement SC-18.b

#### <a name="mobile-code"></a>Mobiele Code

**SC 18.b** de organisatie vaststelt gebruiksbeperkingen en implementatie-richtlijnen voor acceptabel mobiele code en -technologieën van mobiele code.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau systeem en communicatie beveiliging procedures mogelijk tot stand brengen beperkingen op het gebruik van mobiele code. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-18c"></a>NIST 800 53 besturingselement SC-18.c

#### <a name="mobile-code"></a>Mobiele Code

**SC 18.c** de organisatie autoriseert, bewaakt en bepaalt het gebruik van mobiele code in het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een proces op bedrijfsniveau voor autorisatie, bewaking en beheer van het gebruik van mobiele code. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-19a"></a>NIST 800 53 besturingselement SC-19.a

#### <a name="voice-over-internet-protocol"></a>Voice Over internetprotocol

**SC 19.a** de organisatie vaststelt gebruiksbeperkingen en implementatie richtlijnen voor Voice over Internet Protocol (VoIP)-technologieën op basis van de schade aan het informatiesysteem veroorzaken als u met kwaadaardige bedoelingen wordt gebruikt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen voice over internet protocol-technologieën is geïmplementeerd als onderdeel van deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-19b"></a>NIST 800 53 besturingselement SC-19.b

#### <a name="voice-over-internet-protocol"></a>Voice Over internetprotocol

**SC 19.b** de organisatie autoriseert, bewaakt en bepaalt het gebruik van VoIP vanuit het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen voice over internet protocol-technologieën is geïmplementeerd als onderdeel van deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-20a"></a>NIST 800 53 besturingselement SC-20.a

#### <a name="secure-name--address-resolution-service-authoritative-source"></a>Naam van Secure / adres van de Service voor naamomzetting (gezaghebbende bron)

**SC 20.a** het informatiesysteem biedt aanvullende gegevens oorsprong verificatie en integriteit verificatie artefacten samen met de naam van de gezaghebbende resolutie gegevens het systeem geretourneerd in antwoord op de externe naam en adres voor query's voor naamomzetting.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor een veilige naam en adres resolution-service. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-20b"></a>NIST 800 53 besturingselement SC-20.b

#### <a name="secure-name--address-resolution-service-authoritative-source"></a>Naam van Secure / adres van de Service voor naamomzetting (gezaghebbende bron)

**SC 20.b** het informatiesysteem, biedt de mogelijkheid om aan te geven van de beveiligingsstatus van de onderliggende zones en (als de onderliggende ondersteuning voor beveiligde naamomzettingsservices) waarmee de verificatie van een keten van de vertrouwensrelatie tussen bovenliggende en onderliggende domeinen, als het wordt uitgevoerd als onderdeel van een gedistribueerde, hiërarchische naamruimte.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor een veilige naam en adres resolution-service. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-21"></a>NIST 800 53 besturingselement SC-21

#### <a name="secure-name--address-resolution-service-recursive-or-caching-resolver"></a>Naam van Secure / adres van de Service voor naamomzetting (recursieve of Caching Resolver)

**SC-21** het informatiesysteem aanvraagt en de verificatie van de oorsprong en gegevens van de systeemintegriteit wordt uitgevoerd op de naam en adres resolutie antwoorden in het systeem van de gezaghebbende bronnen ontvangt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het configureren van de klant geïmplementeerde resources aanvragen en het uitvoeren van de verificatie van de oorsprong en gegevens van de systeemintegriteit op naam en adres resolutie antwoorden die van de gezaghebbende bronnen ontvangen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-22"></a>NIST 800 53 besturingselement SC-22

#### <a name="architecture-and-provisioning-for-name--address-resolution-service"></a>Architectuur en inrichting voor naam / adres van de Service voor naamomzetting

**SC-22** de systemen die gezamenlijk een naam en adres service voor naamomzetting voor een organisatie te bieden fouttolerantie en interne of externe functiescheiding implementeren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het garanderen dat de systemen adres naamomzettingsservices voor klant geïmplementeerd bronnen bieden fouttolerantie en interne of externe functiescheiding implementeren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-23"></a>NIST 800 53 besturingselement SC-23

#### <a name="session-authenticity"></a>Sessie echtheid

**SC-23** het informatiesysteem beveiligt de echtheid van communicatie-sessies.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Externe toegang tot bronnen die zijn geïmplementeerd door deze Blueprint Azure, met inbegrip van de Azure-portal, de verbinding met extern bureaublad en de web application gateway, zijn beveiligd via TLS. Echtheid biedt TLS voor communicatie op het niveau van de sessie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-23-1"></a>NIST 800 53 besturingselement SC-23 (1)

#### <a name="session-authenticity--invalidate-session-identifiers-at-logout"></a>Sessie echtheid | Sessie-id's op afmelding ongeldig maken

**SC-23 (1)** het informatiesysteem sessie-id's bij het afmelden van de gebruiker of een andere sessie beëindiging ongeldig.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Externe toegang tot bronnen die zijn geïmplementeerd door deze Blueprint Azure, met inbegrip van de Azure-portal, de verbinding met extern bureaublad en de web application gateway, zijn beveiligd via TLS. De Azure portal en extern bureaublad-sessies ongeldig sessie-id's bij het afmelden. Web-sessie ongeldig maken wordt afgedwongen via Azure Application Gateway - regels Web Application Firewall (WAF). De WAF geldt cookie affiniteit per sessie en voert de sessietime-out na 30 minuten (configureerbare post-implementatie aan specifieke regels organisatie) van inactiviteit van de client. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-24"></a>NIST 800 53 besturingselement SC-24

#### <a name="fail-in-known-state"></a>Bekende status mislukt

**SC-24** het informatiesysteem niet op een [toewijzing: organisatie gedefinieerd bekende status] voor [toewijzing: organisatie gedefinieerde typen van fouten] behouden [toewijzing: informatie over de status van de organisatie gedefinieerd systeem] mislukken.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het mislukken van de klant geïmplementeerde resources in een bekende-status te garanderen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-28"></a>NIST 800 53 besturingselement SC-28

#### <a name="protection-of-information-at-rest"></a>Bescherming van gegevens in rust

**SC-28** het informatiesysteem beveiligt de [selectie (een of meer): vertrouwelijkheid, integriteit] van [toewijzing: organisatie gedefinieerde gegevens in rust].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | SC-28 (1) implementatie voldoet aan deze eis. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-sc-28-1"></a>NIST 800 53 besturingselement SC-28 (1)

#### <a name="protection-of-information-at-rest--cryptographic-protection"></a>Bescherming van gegevens in rust | Cryptografische bescherming

**SC-28 (1)** het informatiesysteem geïmplementeerd cryptografische mechanismen om te voorkomen dat onbevoegde openbaarmaking en de wijzigingsopties van [toewijzing: organisatie gedefinieerde informatie] op [toewijzing: organisatie gedefinieerde informatiesysteem onderdelen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Virtuele machines die zijn geïmplementeerd door deze blauwdruk Azure implementeren schijfversleuteling om de vertrouwelijkheid en integriteit van gegevens in rust te beveiligen. Versleuteling voor Windows Azure-schijf is geïmplementeerd met behulp van de BitLocker-functie van Windows. SQL Server is geconfigureerd voor gebruik van transparante gegevens codering (TDE), waarmee u realtime versleuteling en ontsleuteling van gegevens en logboekbestanden ter bescherming van gegevens in rust. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang. Klanten ervoor kiezen om extra op toepassingsniveau besturingselementen voor het beveiligen van de integriteit van opgeslagen gegevens te implementeren. Vertrouwelijkheid en integriteit van alle geïmplementeerd door deze blauwdruk Azure storage-blobs zijn beveiligd door gebruik te maken van Azure Storage Service versleuteling (SSE). SSE beschermt gegevens in rust in Azure storage-accounts met behulp van 256-bits AES-versleuteling. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-sc-39"></a>NIST 800 53 besturingselement SC-39

#### <a name="process-isolation"></a>Geïsoleerde processen

**SC-39** het systeem gegevens onderhoudt een domein kan afzonderlijk worden uitgevoerd voor elk proces dat wordt uitgevoerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows houdt de uitvoering van de afzonderlijke domeinen voor elk proces dat wordt uitgevoerd door een persoonlijke virtuele adresruimte toewijzen aan elk proces. |
| **Provider (Microsoft Azure)** | Niet van toepassing |
