---
title: Richtlijnen voor de effectieve anker ervaringen in Azure ruimtelijke ankers | Microsoft Docs
description: Richtlijnen en overwegingen voor het maken en te zoeken naar ankers effectief met ruimtelijke ankers van Azure.
author: mattwojo
manager: jken
services: azure-spatial-anchors
ms.author: mattwoj
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 196958e4818251bd7f2ee78ca472e6f28292d908
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753057"
---
# <a name="guidelines-for-an-effective-anchor-experience-with-azure-spatial-anchors"></a>Richtlijnen voor een effectieve anker-ervaring met Azure ruimtelijke ankers

Dit artikel bevat richtlijnen en overwegingen voor het maken en te zoeken naar ankers effectief met ruimtelijke ankers van Azure.

## <a name="creating-anchors"></a>Het maken van ankers

Het maken van goede ankers is een van de meeste impact hebben aspecten van het werken met Azure ruimtelijke ankers. Het is belangrijk dat u de tijd in trainen of hoe u goede ankers te maken voor gebruikers in uw UX begeleiden investeren. Door te investeren in een anker maken, kunt u eindgebruikers kunnen zoeken op betrouwbare wijze ankers inschakelen:

- Op verschillende apparaten
- Op verschillende tijdstippen
- Op andere belichting
- Vanuit de gewenste perspectieven binnen de adresruimte
- Enz.

Hieronder vindt u overwegingen en richtlijnen om u te helpen bij het ontwerpen van uw anker-ervaring voor het maken en de locatie.

## <a name="static-and-dynamic-locations"></a>Statische en dynamische locaties

Onderdeel van het ontwerpen van de anker-ervaring wordt de locaties die deelnemen aan te kiezen. Worden de locaties statische en gedefinieerd door een beheerder van de ruimte? Of ze zijn dynamisch en door de gebruiker gedefinieerde?

Een winkelmanager retail er wellicht een statische in-store ervaring te halen van gebruikers te bezoeken. Terwijl de ontwikkelaar van een game van gemengde realiteit bord waarschijnlijk wil zodat gebruikers kunnen kiezen waar u wilt afspelen.

Voor statische locaties, kunt u beheerders tijd door de ruimte met goede ankers inrichting informeren.

Voor dynamische locaties, moet u Denk na over hoe u informeren of gebruikers begeleiden bij de UX goede ankers te maken.

## <a name="stable-visual-features"></a>Stabiele visuele kenmerken

Visual systemen die worden gebruikt voor het bijhouden van gemengde realiteit en augmented reality weergegeven apparaten zijn afhankelijk van visuele kenmerken van de omgeving. De meest betrouwbare ervaring ophalen:  

- Maak ankers op locaties met stabiele visuele kenmerken (dat wil zeggen, functies die niet vaak te wijzigen).

- Maak geen ankers op grote lege oppervlakken met geen onderscheid te maken tussen kenmerken.

- Maak geen ankers op sterk reflecteert materialen.

- Maak geen ankers op oppervlakken waar het alleen-patroon wordt herhaald, zoals Tapijt of achtergrond.

![Voorbeeld van afbeeldingen met een omgeving goed of slecht](./media/stable-visual.png)

## <a name="consider-various-viewing-perspectives"></a>Houd rekening met verschillende perspectieven van weergeven

Bij het maken van een anker, denken over de mensen die later wordt bezoeken om te zoeken van het anker.

Overweeg bijvoorbeeld een anker in het midden van een kamer met twee deuren. Hebt u waarschijnlijk wilt toestaan dat gebruikers die de ruimte van de mogelijkheid tot het vinden van het anker invoert. Bij het maken van het anker, moet u de positie van beide pictogrammen kunt scannen. Dit bevat omgevingsgegevens rond het anker van beide perspectieven zodat gebruikers het anker van een van beide de deur kunnen vinden.

In het algemeen bij het maken van een anker, moet deze worden gescand van de verschillende plaatsen of perspectieven die u verwacht dat de gebruikers worden permanent wanneer wordt geprobeerd te zoeken.  

Als u virtuele inhoud op een outdoor beeldhouwwerken plaatst, worden het zinvol om u te helpen om de beeldhouwwerken, tijdens het bekijken, zoals het maken van het anker.  

Aan de andere kant als uw anker in de linkerbenedenhoek van een lokaal is, is er slechts één richting om deze uit. Bij het maken van het anker, moet deze worden gescand van dit perspectief.

## <a name="multiple-anchors"></a>Meerdere ankers

Licht kunt maken van een verschil in de visuele kenmerken die zijn gedetecteerd. Ankers gemaakt in sterke natuurlijke licht mogelijk moeilijk te vinden na donker onder kunstmatige belichting, en vice versa.  

Als u dit probleem ondervindt, kunt u twee ankers – één in de zomer-en en een andere onder kunstmatige verlichting – maken op dezelfde locatie. Uw app kunt vervolgens een query voor beide ankers. Als u zich bevinden, is de app heeft een houding voor het anker. 

Op deze manier in omgevingen waar de visuele kenmerken wijzigen, omdat de meeste objecten verplaatsen, kunt meerdere ankers. Wanneer een anker moeilijk te vinden vanwege de belangrijke wijzigingen in de omgeving, kunt u deze vervangen door een nieuwe. Dit kan bijvoorbeeld het geval is, zijn in een winkel waar de lay-out elke paar maanden wordt vernieuwd.

## <a name="targets-and-rooms"></a>Ruimten en doelen

In veel gevallen vertegenwoordigt zoeken naar een anker een ingangspunt in zijn eigen unieke ervaring van uw app. Wilt u deze stap voor stap door snel en betrouwbaar zodat gebruikers uw ervaring kunnen invoeren. Tijd besteed aan het hoe gebruikers uw ankers vinden, is een belangrijke stap. Is het handig om na te denken over dit in termen van twee brede scenario's: **Doelen** en **ruimten**.

### <a name="targets"></a>Doelen

![Begin hier bewerken](./media/start-here-edit.png)

In het doelscenario is de locatie van een anker erg bekend is. Bijvoorbeeld in een fictieve MR tekenen app plaatst een gebruiker een virtuele canvas op het prikbord. Ze Hiermee geeft u de andere gebruikers in de ruimte op het punt van hun apparaten op dezelfde plaats op het prikbord om te zoeken van het anker en start de ervaring.  

Een ander voorbeeld van een doelscenario voor het is mogelijk een aanmelding met de mededeling 'scannen voor deals"tijdens het wachten op regel in een restaurant. De restaurant heeft een anker eerder hier geplaatst. Als gebruikers het teken scant, ze vinden van het anker en voer de AR-ervaring voor aanbiedingen op koffie.

In het doelscenario kunt foto's. Als u gebruikers een foto op het apparaat van het beoogde doel weergeven kunt, kunnen ze snel wat u wilt scannen in de praktijk identificeren. Bijvoorbeeld, kan u helpen uw gebruikers binnen de algemene nabijheid van een beoogde doel met GPS aankomen. Zodra de gebruiker ontvangt, bevat uw app een foto van het doel. De gebruiker ziet er uit om de ruimte, vindt het doel en gaat deze naar het scannen naar het anker.

### <a name="rooms"></a>ruimten

![Scannen van ruimte](./media/scan-room.png)

Gebruikers opgeven in het scenario ruimte, een spatie gewoon wetenschap dat er ergens is hier een anker. Gebruikers de ruimte scannen met hun apparaat en Ga snel naar het anker.

Deze ervaring doorgaans te bereiken, moet er goed samengestelde ankers gemaakt zoals beschreven in [kunt u verschillende perspectieven van de weergave](#consider-various-viewing-perspectives) eerder. Als de ruimte is gescand van vele perspectieven bij het maken van het anker, kunnen klikt u vervolgens gebruikers scannen vrijwel elke locatie wanneer u wilt zoeken.

In wezen de persoon die het maken van het anker in de taakwachtrij doorbrengt meer tijd scannen van de ruimte, zodat mensen die afkomstig later zijn kunt scannen en zoek het anker snel. Dit is een belangrijk afweging die u moet rekening houden met uw ervaring.

De MR tekenen-app die eerder zijn besproken, is een voorbeeld die niet geschikt als een kamer-scenario. De gebruiker plaatsen van het anker wil hier, de andere lid worden van de ervaring snel. De gebruikers wilt niet wachten totdat de ruimte is goed gescand om te starten van ervaring. Omdat alle gebruikers waar hun apparaat om te zoeken ankers verwijzen weten, wordt dit scenario beter beschouwd als een doelscenario.

## <a name="effectively-locating-anchors"></a>Effectief te zoeken naar ankers

Visual bijhouden systemen zijn afhankelijk van de visuele kenmerken van een omgeving naar de functie. De meer visuele onderdelen maken deel uit van een scan, hoe hoger de kans van het zoeken naar een anker.

Er zijn enkele algemene richtlijnen die u volgen kunt om het bouwen van een manier die een handig scan van de omgeving worden gebruikers aangemoedigd.

Als de gebruiker niet van een anker binnen een paar seconden vinden, moet de app eerst, bevordering van gebruikers te verplaatsen van het apparaat, zodat u meer perspectieven worden vastgelegd.  De app kan ook gebruikers zelf te verplaatsen om de omgeving bij het zoeken naar het anker van meer perspectieven stimuleren. Meer perspectieven van waaruit het apparaat krijgt te hoe beter de dezelfde punten zien.

Voor doelscenario's mogelijk, vraagt de gebruiker het doel verplaatsen terwijl deze vanuit verschillende perspectieven bekijken. Met andere woorden, vraagt de gebruiker te verplaatsen van een plekje om vast te leggen van het doel van de nieuwe perspectieven totdat het anker zich bevindt.

Voor scenario's ruimte, vraagt de gebruiker langzaam scant de ruimte bevindt. Bijvoorbeeld, vraagt de gebruiker te draaien om vast te leggen, 180 of zelfs 360 graden van de ruimte. Of vraag de gebruiker om te verplaatsen naar een nieuw perspectief in de ruimte bevindt. De meest zinvolle manier van scannen is om te scannen in de ruimte bevindt. Dit bevat meer visuele kenmerken van de omgeving dan als een muur in de buurt te scannen. Scannen van een muur in de buurt wordt niet vastgelegd als veel handige visuele kenmerken van de omgeving.

Het is niet handig om te verplaatsen van het apparaat zij-aan-zij verloop bij het zoeken naar een anker. Dit zou gewoon de dezelfde punten vanuit het perspectief van de dezelfde vastleggen.

## <a name="testing-the-experience"></a>De ervaring te testen

De bovenstaande zijn algemene richtlijnen. Met Azure ruimtelijke ankers schrijft u apps die interactie met de praktijk hebben. Hierdoor is het belangrijk dat u tijd besteden aan het testen van scenario's voor het anker van van uw app in real-omgevingen. Dit geldt met name in omgevingen die representatief is voor waarin u verwacht dat uw gebruikers de app te gebruiken.