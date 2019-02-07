---
title: Azure voordeur Service - verkeersrouteringsmethoden | Microsoft Docs
description: In dit artikel krijgt u inzicht in de verschillende verkeersrouteringsmethoden die wordt gebruikt door de voordeur
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: bd1278db43ba31ed78f13a826a330e16c3bc8d57
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820841"
---
# <a name="front-door-routing-methods"></a>Methoden voor het doorsturen van voordeur

Azure voordeur-Service ondersteunt verschillende methoden om te bepalen hoe uw HTTP/HTTPS-verkeer routeren naar de verschillende service-eindpunten voor routering van verkeer. Met elk van de clientaanvragen voordeur wordt bereikt, wordt de geconfigureerde routeringsmethode toegepast om te controleren of dat de aanvragen worden doorgestuurd naar het beste back-end-exemplaar. 

Er zijn vier belangrijke concepten voor de routering van verkeer beschikbaar in de voordeur:

* **[Latentie](#latency):** De latentie-gebaseerde routering, zorgt u ervoor dat aanvragen worden verzonden naar de laagste latentie back-ends acceptabele binnen een bereik gevoeligheid. In principe worden de aanvragen van gebruikers verzonden naar de 'dichtstbijzijnde' set back-ends met betrekking tot de netwerklatentie.
* **[Prioriteit](#priority):** Als u wilt een back-end van de primaire service gebruiken voor al het verkeer en back-ups opgeven als de primaire of de back-ends voor back-up niet beschikbaar zijn, kunt u prioriteiten toewijzen aan uw verschillende back-ends.
* **[Gewogen](#weighted):** Als u verkeer verspreiden over een set back-ends wilt, gelijkmatig of op basis van gewicht coëfficiënten, kunt u uw verschillende back-ends gewicht toewijzen.
* **Sessie-affiniteit:** U kunt de sessie-affiniteit voor uw front-end-hosts of domeinen als u wilt dat de volgende aanvragen van een gebruiker worden verzonden naar de dezelfde back-end, zolang de sessie van de gebruiker nog steeds actief is en het exemplaar van de back-end nog steeds in orde op basis van statuscontroles rapporteert configureren. 

Alle Front Door-configuraties omvatten bewaking van de back-endstatus en geautomatiseerde, directe failover wereldwijd. Zie voor meer informatie, [voordeur back-end bewaking](front-door-health-probes.md). De deur kunnen worden geconfigureerd met een van beide gebaseerd op een enkele routeringsmethode werk en afhankelijk van uw toepassing moet kunt u meerdere of alle van deze methoden voor het doorsturen in combinatie voor het bouwen van een optimale routeringstopologie.

## <a name = "latency"></a>Laagste latentie op basis van routering van verkeer

Implementatie van back-ends op twee of meer locaties over de hele wereld, kunt de reactietijd van veel toepassingen verbeteren door het routeren van verkeer naar de locatie die zich het dichtst bij uw eindgebruikers. De standaardmethode voor de configuratie van de voordeur in Routering van verkeer verzendt aanvragen van uw eindgebruikers naar de dichtstbijzijnde back-end van de voordeur-omgeving die de aanvraag ontvangen. In combinatie met de Anycast-architectuur van Azure voordeur Service biedt deze aanpak zorgt ervoor dat elk van de maximale prestaties van uw eindgebruikers get aangepast op basis van hun locatie.

De 'dichtstbijzijnde' back-end is niet noodzakelijkerwijs dichtstbijzijnde wordt gemeten door geografische afstand. In plaats daarvan bepaalt de voordeur de dichtstbijzijnde back-ends door te meten of de netwerklatentie. Meer informatie over [routering architectuur van de voordeur](front-door-routing-architecture.md). 

Hieronder volgt de algemene stroomschema:

| Beschikbare back-ends | Prioriteit | Latentie signaal (gebaseerd op de statustest) | Waarden |
|-------------| ----------- | ----------- | ----------- |
| Selecteer in de eerste plaats alle back-ends die zijn ingeschakeld en geretourneerd in orde (200 OK) voor de statustest. Bijvoorbeeld, er zijn zes back-ends A, B, C, D, E en F, en tussen deze C is niet in orde en E is uitgeschakeld. Zodat de lijst met beschikbare back-ends A is, B, D, f kan zijn.  | Vervolgens zijn de hoogste prioriteit back-ends met de beschikbare netwerken geselecteerd. Bijvoorbeeld, back-end A, B en D hebben prioriteit 1 en back-end-F heeft prioriteit 2. Zodat de geselecteerde back-ends zijn A, B en D.| Selecteer de back-ends met latentiebereik (minimale latentie en latentie gevoeligheid in ms die zijn opgegeven). Bijvoorbeeld, als A 15 ms, B 30 ms is en D 60 ms weg van de voordeur omgeving waar de aanvraag bevindt zich, en latentie gevoeligheid is 30 ms vervolgens laagste latentie van toepassingen bestaat uit de back-end A en B, is omdat D is meer dan 30 ms weg van de dichtstbijzijnde back-end die is een A. | Ten slotte wordt voordeur round robin het verkeer tussen de laatste geselecteerde groep van back-ends via de verhouding van de gewichten opgegeven. Bijvoorbeeld, als back-end A heeft een gewicht van 5 en back-end B heeft een gewicht van 8, wordt het verkeer wordt gedistribueerd in de verhouding van 5:8 tussen back-ends A en B. |

>[!NOTE]
> Standaard de latentie gevoeligheid-eigenschap is ingesteld op 0 ms, dat wil zeggen, altijd de aanvraag doorsturen naar de snelste beschikbare back-end.


## <a name = "priority"></a>Op basis van prioriteit-routering van verkeer

Vaak wil een organisatie voor de betrouwbaarheid van de services door het implementeren van een of meer back-services als hun primaire service uitvalt. In de industrie, wordt deze topologie ook wel actief/stand-by of actief/passief-implementatietopologie. De methode 'Prioriteit' Routering van verkeer kan Azure-klanten eenvoudig dit patroon wilt implementeren failover.

Uw standaard voordeur bevat een lijst met dezelfde prioriteit van de back-ends. Standaard verzendt voordeur verkeer alleen naar de hoogste prioriteit backends (laagste waarde voor prioriteit) dat wil zeggen, de primaire set van back-ends. Als de primaire back-ends niet beschikbaar zijn, routeert voordeur het verkeer naar de secundaire set back-ends (tweede laagste waarde voor prioriteit). Als zowel de primaire en secundaire back-ends zijn niet beschikbaar is, wordt het verkeer wordt gerouteerd naar de derde, enzovoort. Beschikbaarheid van de back-end is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en de status van lopende back-end zoals wordt bepaald door de status tests.

### <a name="configuring-priority-for-backends"></a>Prioriteit voor back-ends configureren

Elk van de back-ends in de endgroep back-in de configuratie van de voordeur heeft een eigenschap genaamd 'Prioriteit', een getal tussen 1 en 5. Met Azure voordeur Service, moet u de prioriteit van de back-end expliciet het gebruik van deze eigenschap voor elke back-end configureren. Deze eigenschap is een waarde tussen 1 en 5. Lagere waarden geven een hogere prioriteit. Back-ends kunt prioriteitswaarden delen.

## <a name = "weighted"></a>Gewogen verkeersrouteringsmethode
De 'Gewogen' verkeersrouteringsmethode kunt u verkeer gelijkmatig verdelen of een vooraf gedefinieerde weging gebruiken.

In de routering van verkeer gewogen methode, kunt u een gewicht toewijzen aan elke back-end in de voordeur configuratie van uw back-endgroep. Het gewicht is een geheel getal tussen 1 en 1000. Deze parameter maakt gebruik van een standaardgewicht van '50'.

Onder de lijst met beschikbare back-ends in de gevoeligheid geaccepteerde latentie (zoals opgegeven), het verkeer verdeeld in een round robin-mechanisme in de verhouding van de gewichten opgegeven. Als de gevoeligheid van de latentie is ingesteld op 0 milliseconden, klikt u vervolgens deze eigenschap niet van kracht, tenzij er twee back-ends met de dezelfde netwerklatentie zijn. 

De gewogen methode kunt enkele handige scenario's:

* **Upgrade van de toepassing geleidelijk**: Toewijzen van een percentage van het verkeer routeren naar een nieuwe back-end en het verkeer, geleidelijk na verloop van tijd om deze te zetten op par met andere back-ends te verhogen.
* **Toepassingen migreren naar Azure**: Een back endpool maken met Azure en externe back-ends. Het gewicht van de back-ends liever van de nieuwe back-ends aanpassen. U kunt geleidelijk instellen dit vanaf de nieuwe back-ends uitgeschakeld, die vervolgens toe te wijzen ze het laagste gewicht langzaam om niveaus waar ze het meeste verkeer uitvoeren. Vervolgens ten slotte de minder gewenste back-ends uitschakelen en verwijderen uit de groep.  
* **Cloudbursting voor extra capaciteit**: Snelle uitbreiding van een on-premises implementatie naar de cloud door de gegevens achter de voordeur. Als u extra capaciteit in de cloud nodig hebt, kunt u toevoegen of meer back-ends inschakelen en opgeven welk deel van het verkeer wordt gerouteerd naar elke back-end.

## <a name = "affinity"></a>Sessie-affiniteit
Standaard zonder sessieaffiniteit, stuurt de voordeur aanvragen die afkomstig zijn van dezelfde client naar verschillende back-ends op basis van de configuratie voor taakverdeling met name als de latenties naar verschillende back-ends wijzigen of als andere aanvragen van dezelfde gebruiker gebieden op een andere voordeur-omgeving. Bij sommige stateful toepassingen en in bepaalde andere situaties wordt er echter de voorkeur aan gegeven om opeenvolgende aanvragen van één gebruiker naar de back-end te sturen waarmee ook de eerste aanvraag is verwerkt. De functie Sessieaffiniteit op basis van cookies is handig als u een gebruikerssessie op dezelfde back-end wilt behouden. Met behulp van cookies voordeur die worden beheerd, kan Azure voordeur Service sturen daarop volgende verkeer van een gebruikerssessie naar dezelfde back-end voor het verwerken van, zolang de back-end in orde is en de gebruikerssessie is nog niet is verlopen. 

U kunt sessieaffiniteit inschakelen op het hostniveau van de front-end, voor elk van de geconfigureerde domeinen (of subdomeinen). Na het inschakelen voegt Front Door een cookie toe aan de gebruikerssessie. Met op cookies gebaseerde sessieaffiniteit kan Front Door verschillende gebruikers herkennen, zelfs als deze hetzelfde IP-adres hebben. Hierdoor kan er nog meer verkeer worden verdeeld tussen uw verschillende back-ends.

De cookie blijft even lang actief als de gebruikerssessie, omdat Front Door momenteel alleen ondersteuning biedt voor sessiecookies. 

> [!NOTE]
> Openbare proxy's vertonen storen bij sessie-affiniteit. Dit is omdat een sessie tot stand brengen voordeur een sessieaffiniteitscookie toevoegen aan het antwoord kan niet worden uitgevoerd vereist als het antwoord gecachet kan worden is als de cookies die van andere clients die aanvragen van dezelfde bron zou worden onderbroken. Om dit te voorkomen, sessie-affiniteit wordt **niet** tot stand worden gebracht als de back-end een antwoord gecachet kan worden stuurt wanneer deze wordt uitgevoerd. Als de sessie is al tot stand is gebracht, het maakt niet uit als het antwoord van de back-end gecachet kan worden.
> Sessie-affiniteit wordt tot stand worden gebracht in de volgende omstandigheden **tenzij** het antwoord heeft een 304 HTTP-statuscode:
> - Het antwoord heeft specifieke waarden opgeven voor de ```Cache-Control``` -header die voorkomen in cache opslaan, zoals "Vertrouwelijk" of niet-store '.
> - Het antwoord bevat een ```Authorization``` -header die niet is verlopen.
> - Het antwoord heeft een HTTP 302-statuscode.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
