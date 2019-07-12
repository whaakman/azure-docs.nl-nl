---
title: Verbinding maken met Windows virtuele bureaublad Preview-versie van Windows 10 of Windows 7 - Azure
description: Klik hier voor meer informatie over het verbinding maken met de Windows-Preview voor virtuele bureaublad van Windows 10 of Windows 7.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: helohr
ms.openlocfilehash: 66b423ea27cc0f9112349964fcfd0d75f08c12bd
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620123"
---
# <a name="connect-from-windows-10-or-windows-7"></a>Verbinding maken vanaf Windows 10 of Windows 7

> Van toepassing op: Windows 7 en Windows 10.

Een downloadbare client beschikbaar is dat toegang biedt tot Windows Virtual Desktop Preview resources op apparaten met Windows 7 en Windows 10.

> [!IMPORTANT]
> Gebruik geen **RemoteApp- en bureaublad-verbindingen (RADC)** of **Remote Desktop Connection (MSTSC)** voor toegang tot resources van de virtuele Windows-bureaublad omdat de client biedt geen ondersteuning voor virtuele Windows-bureaublad.

## <a name="install-the-client"></a>De client installeren

[Download](https://go.microsoft.com/fwlink/?linkid=2068602) en installeer de client op uw lokale computer. De installatie vereist beheerdersrechten.

## <a name="subscribe-to-a-feed"></a>Abonneren op een feed

De lijst met beheerde resources beschikbaar voor u ophalen Abonneer u op de feed die wordt geleverd door uw beheerder. Abonneren, maakt de resources beschikbaar op de lokale computer.

Om u te abonneren op een feed:

1. Start de client uit de lijst met alle Apps, zoekt u naar voor **extern bureaublad**.
1. Selecteer **abonneren** op de hoofdpagina verbinding maken met de service en uw resources ophalen.
1. **Meld u aan** met uw gebruikersaccount wanneer u hierom wordt gevraagd.

Verificatie is gelukt, ziet u nu een lijst van beschikbare resources voor u.

U kunt resources starten door een van twee methoden.

- Dubbelklik op een bron uit voor het starten van de hoofdpagina van de client.
- Start een resource, zoals u gewend andere apps vanuit het Menu Start.
  - U kunt ook zoeken naar de apps in de zoekbalk.

Zodra u zich hebt geabonneerd op een feed, wordt de inhoud van de feed automatisch regelmatig bijgewerkt. Resources kunnen worden toegevoegd, gewijzigd of verwijderd op basis van wijzigingen die zijn aangebracht door uw beheerder.

## <a name="view-the-details-of-a-feed"></a>Bekijk de details van een feed

Nadat u een abonnement, kunt u aanvullende informatie over de feed weergeven door het openen van het deelvenster met details.

1. Op de hoofdpagina van de client, selecteer het weglatingsteken ( **...** ) aan de rechterkant van de naam van de feed.
1. Selecteer in het vervolgkeuzemenu **Details**.
1. Het deelvenster met Details wordt weergegeven aan de rechterkant van de client.

Het deelvenster met Details bevat nuttige informatie over de feed:

- De URL en de gebruikersnaam die wordt gebruikt om u te abonneren
- Het aantal apps en pc 's
- De datum/tijd van de laatste update
- De status van de laatste update

Indien nodig, kunt u een handmatige update starten door te selecteren op **nu bijwerken**.

## <a name="unsubscribe-from-a-feed"></a>Afmelden bij een feed

In deze sectie leert u hoe u afmelden voor een feed. U kunt zich afmelden op te abonneren opnieuw met een ander account of uw resources te verwijderen uit het systeem.

1. Op de hoofdpagina van de client, selecteer het weglatingsteken ( **...** ) aan de rechterkant van de naam van de feed.
1. Selecteer in het vervolgkeuzemenu **Afmelden**.
1. Bekijk en selecteer **doorgaan** in het dialoogvenster.

## <a name="update-the-client"></a>De client bijwerken

Wanneer een nieuwe versie van de client beschikbaar is, krijgt u een bericht door de client en het Actiecentrum van Windows. Selecteer de melding om de updateproces te starten.
