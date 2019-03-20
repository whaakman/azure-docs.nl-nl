---
title: Richtlijnen voor de effectieve anker ervaringen die gebruikmaken van Azure ruimtelijke ankers | Microsoft Docs
description: Richtlijnen en overwegingen bij het maken en ankers effectief zoeken met behulp van Azure ruimtelijke ankers.
author: mattwojo
manager: jken
services: azure-spatial-anchors
ms.author: mattwoj
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 4714ea7aa4bf55f7cbd4500977b09505788233dd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57895801"
---
# <a name="create-an-effective-anchor-experience-by-using-azure-spatial-anchors"></a>De ervaring van een doeltreffende anker maken met behulp van Azure ruimtelijke ankers

In dit artikel bevat richtlijnen en om effectief te maken en ankers vinden met behulp van ruimtelijke ankers.

## <a name="good-anchors"></a>Goede ankers

Ruimtelijke ankers kunt u goede ankers te maken. Het is belangrijk om te investeren in de trainen of begeleiden van gebruikers in uw gebruikerservaring (UX) goed ankers te maken. Door te investeren in het maken van een goede ankers, helpen u eindgebruikers kunnen zoeken op betrouwbare wijze ankers:

- Op verschillende apparaten.
- Op verschillende tijdstippen.
- In andere belichting.
- Vanuit de gewenste perspectieven binnen de adresruimte.

## <a name="static-and-dynamic-locations"></a>Statische en dynamische locaties

Onderdeel van het ontwerpen van de anker-ervaring is de locaties kiezen. Worden de locaties statische en gedefinieerd door een beheerder van de ruimte? Of ze zijn dynamisch en door de gebruiker gedefinieerde?

Een winkelmanager retail verstandig een statische in-store ervaring te halen van gebruikers te bezoeken. Maar de ontwikkelaar van het bord mixed-reality spel wilt waarschijnlijk zodat gebruikers kunnen kiezen waar u wilt afspelen.

Voor statische locaties, geeft u aan beheerders tijd door de ruimte met goede ankers inrichting.

Voor dynamische locaties, moet u Denk na over hoe u geeft aan of gebruikers begeleiden bij de UX goede ankers te maken.

## <a name="stable-visual-features"></a>Stabiele visuele kenmerken

Systemen voor het bijhouden van Visual gebruikt op mixed-reality en uitgebreide realiteit apparaten zijn afhankelijk van de visuele kenmerken van de omgeving. De meest betrouwbare ervaring ophalen:  

- *Voer* ankers op locaties met stabiele visuele kenmerken (dat wil zeggen, functies die niet vaak veranderen) maken.

- *Geen* ankers op grote lege oppervlakken waarvoor geen onderscheid te maken tussen kenmerken maken.

- *Geen* ankers op sterk reflecteert materiaal maken.

- *Geen* ankers op oppervlakken waar het patroon wordt herhaald, zoals carpet of van de achtergrond te maken.

![Voorbeelden van een goede omgeving voor ankers en een beschadigde omgeving voor ankers](./media/stable-visual.png)

## <a name="various-viewing-perspectives"></a>Verschillende perspectieven van weergeven

Bij het maken van een anker, denken over de mensen die later opnieuw geprobeerd te vinden van het anker.

Overweeg bijvoorbeeld een anker in het midden van een kamer met twee deuren. Hebt u waarschijnlijk wilt toestaan dat gebruikers de ruimte van de deur opgeven. Bij het maken van het anker, moet u de positie van beide pictogrammen kunt scannen. U perspectieven voor het vastleggen van omgevingsgegevens rond het anker zodat gebruikers het anker van een van beide de deur kunnen vinden.

In het algemeen bij het maken van een anker, scant u dit vanuit het perspectief van de mensen die probeert te zoeken. Dus als u een virtuele inhoud op een outdoor beeldhouwwerken plaatst, zinvol het om te helpen om de beeldhouwwerken, tijdens het scannen, zoals het maken van het anker. Als uw anker in de linkerbenedenhoek van een kamer is, is er slechts één richting om deze uit. Bij het maken van deze anker, kunt u deze alleen met dit perspectief scannen.

## <a name="multiple-anchors"></a>Meerdere ankers

Licht kunt maken van een verschil in de visuele kenmerken die door een app gedetecteerd. Ankers gemaakt in sterke natuurlijke licht mogelijk moeilijk te vinden in kunstmatige licht, en vice versa.  

Als u dit probleem hebt, kunt u twee ankers te maken. Op dezelfde locatie, maakt u een anker in de zomer-en en een andere in kunstmatige licht. Uw app kunt vervolgens een query voor beide ankers. Als een van beide anker zich bevindt, is de app heeft een houding voor het anker. 

Op deze manier in omgevingen waar de visuele kenmerken wijzigen, omdat de meeste objecten verplaatsen, kunt meerdere ankers. Wanneer een anker te moeilijk komt te vinden vanwege de belangrijke wijzigingen in de omgeving, kunt u het anker vervangen door een nieuw exemplaar. U kunt dit bijvoorbeeld doen in een winkel waar de lay-out elke paar maanden wordt vernieuwd.

## <a name="targets-and-rooms"></a>Ruimten en doelen

In veel gevallen is een anker een beginpunt voor uw app ervaring. Wilt u deze stap voor stap door snel en betrouwbaar zodat gebruikers uw ervaring kunnen invoeren. Tijd besteed aan het hoe gebruikers uw ankers vinden, is een belangrijke stap. Is het handig om na te denken over het zoeken naar ankers in termen van twee scenario's voor brede: *doelen* en *ruimten*.

### <a name="targets"></a>Doelen

In het doelscenario is de locatie van een anker erg bekend is. Bijvoorbeeld in een app fictieve mixed-reality tekenen plaatst een gebruiker een virtuele canvas op de wand. Ze Hiermee geeft u de andere gebruikers in de ruimte op het punt van hun apparaten op dezelfde plaats op het prikbord om te zoeken van het anker en start de ervaring.  

Een ander voorbeeld van een doelscenario voor het is mogelijk een teken in een restaurant die leest, "-Scan voor deals." De restaurant heeft hier een anker geplaatst. Als gebruikers het teken scant, ze vinden van het anker en voer de uitgebreide reality-ervaring voor deals over koffie.

In het doelscenario kunt foto's. Als u gebruikers een foto van het beoogde doel op hun apparaat weergeven, kunnen ze snel wat u wilt scannen in de praktijk identificeren. Bijvoorbeeld, kan u helpen uw gebruikers binnen het algemene gedeelte van een beoogde doel met behulp van de GPS aankomen. Als de gebruiker is bereikt, ziet uw app een foto van het doel. De gebruiker ziet er uit om de ruimte, vindt het doel en zoekt naar het anker.

![Afbeelding van een anker van een foto van het doel op het mobiele apparaat van een gebruiker](./media/start-here-edit.png)

### <a name="rooms"></a>ruimten

Gebruikers opgeven in het scenario ruimte, een spatie gewoon wetenschap dat er ergens is hier een anker. Gebruikers de ruimte scannen met hun apparaat en het anker snel te vinden.

Deze ervaring doorgaans vereist dat u goed samengestelde ankers te maken zoals beschreven in [verschillende perspectieven van de weergave](#consider-various-viewing-perspectives). Als u de ruimte van vele perspectieven gescand tijdens het maken van het anker, kunnen gebruikers vrijwel overal scannen wanneer ze proberen te zoeken.

![Afbeelding van hoe een gebruiker een ruimte om een anker kunt scannen](./media/scan-room.png)

In principe u meer tijd besteden aan de ruimte scannen bij het maken van het anker zodat u later gebruikers kunnen scannen en zoek het anker snel. Wanneer u uw ervaring maakt, moet u rekening houden met deze belangrijke wisselwerking.

Het voorbeeld van de app voor mixed-reality tekenen die eerder is besproken werkt niet goed als een kamer-scenario. De gebruiker die het anker wil hier, anderen de ervaring snel te koppelen. De gebruikers wilt niet wachten met het starten van ervaring totdat de ruimte wordt ook gescand. Omdat alle gebruikers waar hun apparaat om te zoeken ankers verwijzen weten, werkt in dit voorbeeld als een doelscenario.

## <a name="anchor-location"></a>Ankerlocatie

Visual bijhouden systemen zijn afhankelijk van de visuele kenmerken in een omgeving. De meer visuele kenmerken met een scan, hoe hoger de kans van het zoeken naar een anker.

Ga als volgt de algemene richtlijnen in deze sectie voor het bouwen van een UX die een handig scan van de omgeving worden gebruikers aangemoedigd.

Als de gebruiker niet van een anker binnen een paar seconden vinden, moet de app eerst gebruikers het apparaat om vast te leggen meer perspectieven verplaatsen stimuleren. De app kan ook gebruikers navigeren de omgeving om te scannen op het anker van meer perspectieven stimuleren. Meer functie perspectieven die door het apparaat, hoe beter.

Voor doelscenario's mogelijk, vraagt de gebruiker het doel om dit te bekijken vanuit verschillende perspectieven herinrichten. Met andere woorden, vraagt de gebruiker om vast te leggen van het doel van de nieuwe perspectieven totdat het anker zich bevindt.

Voor scenario's ruimte, vraagt de gebruiker langzaam scant de ruimte bevindt. Bijvoorbeeld, vraagt de gebruiker om in te schakelen om vast te leggen 180 graden of zelfs 360 graden van de ruimte. Of vraag de gebruiker om de ruimte vanuit het oogpunt van de nieuwe weer te geven. 

De meest zinvolle methode is om te scannen in de ruimte bevindt. Een scan voor de ruimte wordt bijvoorbeeld meer visuele kenmerken van de omgeving dan een scan van een in de buurt wand vastgelegd. Een scan van een muur in de buurt wordt niet zo veel handige visuele kenmerken van de omgeving vastleggen.

Het is niet nuttig herhaaldelijk het apparaat verplaatsen naar een andere naast bij het zoeken naar een anker. Dit gewoon vastgelegd de dezelfde punten vanuit het perspectief van de dezelfde.

## <a name="experience-tests"></a>Ervaring tests

In dit artikel hebben we besproken algemene richtlijnen. Met ruimtelijke ankers schrijft u apps die interactie met de echte wereld werkt hebben. Vanwege die, moet u tijd besteden aan het testen van uw app anker-scenario's in real-omgevingen. Dit geldt met name in omgevingen die staan voor waar u verwacht dat uw gebruikers de app te gebruiken.