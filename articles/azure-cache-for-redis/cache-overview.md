---
title: Wat is Azure Cache voor Redis? | Microsoft Docs
description: Hier vindt u informatie over Azure Cache voor Redis en hoe dit vaak wordt gebruikt.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 5b2b3a3ec0e9aec603a69211a7493afc31e83283
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236546"
---
# <a name="what-is-azure-cache-for-redis"></a>Wat is Azure Cache voor Redis

Azure Cache voor Redis is gebaseerd op de populaire [Redis](https://redis.io/)-software. Dit wordt meestal gebruikt als cache voor het verbeteren van de prestaties en schaalbaarheid van systemen die sterk afhankelijk zijn van back-endgegevensopslag. De prestaties worden verbeterd door veelgebruikte gegevens tijdelijk te kopiëren naar een snelle opslag die zich vlakbij de toepassing bevindt. Met [Azure Cache voor Redis](https://redis.io/) bevindt deze snelle opslag zich in het geheugen met Azure Cache voor Redis in plaats van dat de opslag door een database vanaf een schijf wordt geladen.

Azure Cache voor Redis kan ook worden gebruikt als een gegevensstructuuropslag in het geheugen, een gedistribueerde niet-relationele database en een berichtenbroker. De prestaties van toepassingen worden verbeterd door gebruik te maken van de snelle gegevensdoorvoer met lage latentie van de Redis-engine.

Azure Cache voor Redis biedt toegang tot een beveiligde, toegewezen Azure Cache voor Redis die wordt beheerd in Microsoft, wordt gehost in Azure en toegankelijk is voor elke toepassing binnen of buiten Azure.

## <a name="why-use-azure-cache-for-redis"></a>Waarom Azure Cache voor Redis gebruiken

Er zijn veel algemene patronen waar Azure Cache voor Redis wordt gebruikt ter ondersteuning van toepassingsarchitectuur of ter verbetering van de prestaties van toepassingen. Sommige van de meest voorkomende zijn de volgende:

| Patroon      | Beschrijving                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Aangezien een database groot kan zijn, is het laden van een volledige database in een cache niet de aanbevolen aanpak. Het is gebruikelijk om het [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside)-patroon te gebruiken voor het laden van gegevensitems in de cache, alleen indien dit nodig is. Als het systeem wijzigingen aanbrengt in de back-endgegevens, kan het op dat moment ook de cache bijwerken, die wordt gedistribueerd met andere clients. Het systeem kan bovendien een vervaldatum instellen voor gegevensitems of een verwijderingsbeleid gebruiken om te zorgen dat gegevensupdates opnieuw worden geladen in de cache.|
| [Inhoud in cache opslaan](cache-aspnet-output-cache-provider.md) | De meeste webpagina's worden gegenereerd op basis van sjablonen met kopteksten, voetteksten, werkbalken, menu's, enzovoort. Ze worden feitelijk niet vaak gewijzigd en moeten niet dynamisch worden gegenereerd. Met behulp van een cache in het geheugen, zoals Azure Cache voor Redis, krijgen uw webservers snel toegang tot dit type statische inhoud vergeleken met de back-end-datastores. Dit patroon vermindert de verwerkingstijd en serverbelasting die nodig zouden zijn voor het dynamisch genereren van de inhoud. Hierdoor kunnen webservers sneller reageren en hebt u minder servers nodig voor het afhandelen van belasting. Azure Cache voor Redis biedt de Redis Output Cache Provider om dit patroon te ondersteunen met ASP.NET.|
| [Gebruikerssessie opslaan in cache](cache-aspnet-session-state-provider.md) | Dit patroon wordt vaak gebruikt met winkelwagens en andere informatie van het type gebruikersgeschiedenis, die een webapp mogelijk wil koppelen aan gebruikerscookies. Te veel informatie opslaan in een cookie kan negatieve gevolgen hebben voor de prestaties als de cookie groter wordt en bij elke aanvraag wordt doorgegeven en gevalideerd. Een gangbare oplossing is om de cookie als sleutel te gebruiken voor het opvragen van gegevens in een back-enddatabase. Het gebruik van een cache in het geheugen zoals Azure Cache voor Redis om gegevens te koppelen aan een gebruiker, is veel sneller dan interactie met een volledige relationele database. |
| Wachtrij met taken en berichten | Wanneer toepassingen aanvragen ontvangen, nemen de bewerkingen die zijn gekoppeld aan de aanvraag vaak extra tijd in beslag. Het is gebruikelijk dat langdurige bewerkingen worden toegevoegd aan een wachtrij die later wordt verwerkt, en mogelijk door een andere server. Deze methode van werk uitstellen heet taken in de wachtrij plaatsen. Er zijn veel softwareonderdelen die zijn ontworpen ter ondersteuning van wachtrijen. Azure Cache voor Redis is ook geschikt voor dit doel en werkt ook als een gedistribueerde wachtrij.|
| Gedistribueerde transacties | Een algemene vereiste is dat toepassingen een reeks opdrachten op basis van een back-endopslagplaats, evenals een enkele bewerking (atomisch) moeten kunnen uitvoeren. Alle opdrachten moeten slagen of alle moet worden teruggezet naar de beginstatus. Azure Cache voor Redis ondersteunt het uitvoeren van een batch met opdrachten als één bewerking in de vorm van [Transacties](https://redis.io/topics/transactions). |

## <a name="azure-cache-for-redis-offerings"></a>Aanbiedingen voor Azure Cache voor Redis

Azure Cache voor Redis is beschikbaar in de volgende lagen:

| Laag | Beschrijving |
|---|---|
Basic | Een cache met één knooppunt. Deze laag biedt ondersteuning voor meerdere geheugengrootten (250 MB - 53 GB). Dit is een ideale laag voor het ontwikkelen/testen en voor niet-essentiële werkbelastingen. De Basic-laag heeft geen SLA (Service Level Agreement) |
| Standard | Een gerepliceerde cache in een configuratie met twee knooppunten (primair/secundair) die door Microsoft wordt beheerd, met een SLA met hoge beschikbaarheid (99,9%). |
| Premium | De Premium-laag is de laag die geschikt is voor bedrijven. Caches op de Premium-laag ondersteunen meer functies en hebben een hogere doorvoersnelheid met een lagere latentie. Caches op de Premium-laag worden geïmplementeerd op krachtigere hardware en bieden daarom betere prestaties in vergelijking met de Basic- of Standard-laag. Dit voordeel betekent dat de doorvoer voor een cache van dezelfde grootte sneller gaat bij Premium dan bij Standard |

> [!TIP]
> Raadpleeg [Veelgestelde vragen over Azure Cache voor Redis](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte van premiumcaches.
>

U kunt de cache opschalen naar een hogere laag nadat u de cache al hebt gemaakt. Omlaag schalen naar een lagere laag wordt niet ondersteund. Zie voor stapsgewijze instructies [De schaal aanpassen van Azure Cache voor Redis](cache-how-to-scale.md) en [Een schaalbewerking automatiseren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparision"></a>Vergelijking van functies

De pagina [Prijzen van Azure Cache voor Redis](https://azure.microsoft.com/pricing/details/cache/) bevat een gedetailleerde vergelijking van elke laag. In de volgende tabel worden de functies beschreven die door elke laag worden ondersteund:

| Omschrijving | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis-cluster](cache-how-to-premium-clustering.md) |✔|-|-|
| [Beveiliging via de firewallregels](cache-configure.md#firewall) |✔|✔|✔|
| [Verbeterde veiligheid en isolatie met VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Import/export](cache-how-to-import-export-data.md) |✔|-|-|
| [Updates plannen](cache-administration.md#schedule-updates) |✔|-|-|
| [Geo-replicatie](cache-how-to-geo-replication.md) |✔|-|-|
| [Opnieuw opstarten](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Volgende stappen

* [Snelstart ASP.NET-webapp](cache-web-app-howto.md) Maak een eenvoudige ASP.NET-webapp die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart .NET](cache-dotnet-how-to-use-azure-redis-cache.md) Maak een .NET-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart .NET Core](cache-dotnet-core-quickstart.md) Maak een .NET Core-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart Node.js](cache-nodejs-get-started.md) Maak een eenvoudige Node.js-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart Java](cache-java-get-started.md) Maak een eenvoudige Java-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart Python](cache-python-get-started.md) Maak een Python-app die gebruikmaakt van een Azure Cache voor Redis.
