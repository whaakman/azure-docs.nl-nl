---
title: Wat is Azure Redis-cache? | Microsoft Docs
description: Hier vindt u informatie over Azure Redis Cache en hoe dit vaak wordt gebruikt.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 585dcd120c42562b1520d4454f9d04e445553101
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096189"
---
# <a name="what-is-azure-redis-cache"></a>Wat is Azure Redis Cache?

Azure Redis Cache is gebaseerd op de populaire open-source [Redis Cache](https://redis.io/). Dit wordt meestal gebruikt als cache voor het verbeteren van de prestaties en schaalbaarheid van systemen die sterk afhankelijk zijn van back-endgegevensopslag. De prestaties worden verbeterd door veelgebruikte gegevens tijdelijk te kopiëren naar een snelle opslag die zich vlakbij de toepassing bevindt. Met [Redis Cache](https://redis.io/) bevindt deze snelle opslag zich in het geheugen met Redis Cache in plaats van dat de opslag door een database vanaf een schijf wordt geladen.

Azure Redis Cache kan ook worden gebruikt als een gegevensstructuuropslag in het geheugen, een gedistribueerde niet-relationele database en een message broker. De prestaties van toepassingen worden verbeterd door gebruik te maken van de snelle gegevensdoorvoer met lage latentie van de Redis-engine.

Azure Redis Cache biedt toegang tot een beveiligde, toegewezen Redis-cache die wordt beheerd in Microsoft, wordt gehost in Azure en toegankelijk is voor elke toepassing binnen of buiten Azure.

## <a name="why-use-azure-redis-cache"></a>Waarom zou u Azure Redis Cache gebruiken?

Er zijn veel algemene patronen waar Redis Cache wordt gebruikt ter ondersteuning van toepassingsarchitectuur of ter verbetering van de prestaties van toepassingen. Sommige van de meest voorkomende zijn de volgende:

| Patroon      | Beschrijving                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Aangezien een database groot kan zijn, is het laden van een volledige database in een cache niet de aanbevolen aanpak. Het is gebruikelijk om het [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside)-patroon te gebruiken voor het laden van gegevensitems in de cache, alleen indien dit nodig is. Als het systeem wijzigingen aanbrengt in de back-endgegevens, kan het op dat moment ook de cache bijwerken, die wordt gedistribueerd met andere clients. Het systeem kan bovendien een vervaldatum instellen voor gegevensitems of een verwijderingsbeleid gebruiken om te zorgen dat gegevensupdates opnieuw worden geladen in de cache.|
| [Inhoud in cache opslaan](cache-aspnet-output-cache-provider.md) | De meeste webpagina's worden gegenereerd op basis van sjablonen met kopteksten, voetteksten, werkbalken, menu's, enzovoort. Ze worden feitelijk niet vaak gewijzigd en moeten niet dynamisch worden gegenereerd. Met behulp van een cache in het geheugen, zoals Azure Redis Cache, krijgen uw webservers snel toegang tot dit type statische inhoud vergeleken met de back-end-datastores. Dit patroon vermindert de verwerkingstijd en serverbelasting die nodig zouden zijn voor het dynamisch genereren van de inhoud. Hierdoor kunnen webservers sneller reageren en hebt u minder servers nodig voor het afhandelen van belasting. Azure Redis Cache biedt de Redis Output Cache Provider om dit patroon te ondersteunen met ASP.NET.|
| [Gebruikerssessie opslaan in cache](cache-aspnet-session-state-provider.md) | Dit patroon wordt vaak gebruikt met winkelwagens en andere informatie van het type gebruikersgeschiedenis, die een webapp mogelijk wil koppelen aan gebruikerscookies. Te veel informatie opslaan in een cookie kan negatieve gevolgen hebben voor de prestaties als de cookie groter wordt en bij elke aanvraag wordt doorgegeven en gevalideerd. Een gangbare oplossing is om de cookie als sleutel te gebruiken voor het opvragen van gegevens in een back-enddatabase. Het gebruik van een cache in het geheugen zoals Azure Redis Cache om gegevens te koppelen aan een gebruiker, is veel sneller dan interactie met een volledige relationele database. |
| Wachtrij met taken en berichten | Wanneer toepassingen aanvragen ontvangen, nemen de bewerkingen die zijn gekoppeld aan de aanvraag vaak extra tijd in beslag. Het is gebruikelijk dat langdurige bewerkingen worden toegevoegd aan een wachtrij die later wordt verwerkt, en mogelijk door een andere server. Deze methode van werk uitstellen heet taken in de wachtrij plaatsen. Er zijn veel softwareonderdelen die zijn ontworpen ter ondersteuning van wachtrijen. Redis Cache is ook geschikt voor dit doel, en werkt ook als een gedistribueerde wachtrij.|
| Gedistribueerde transacties | Een algemene vereiste is dat toepassingen een reeks opdrachten op basis van een back-endopslagplaats, evenals een enkele bewerking (atomisch) moeten kunnen uitvoeren. Alle opdrachten moeten slagen of alle moet worden teruggezet naar de beginstatus. Redis Cache ondersteunt het uitvoeren van een batch met opdrachten als één bewerking in de vorm van [Transacties](https://redis.io/topics/transactions). |

## <a name="azure-redis-cache-offerings"></a>Aanbod van Azure Redis Cache

Azure Redis Cache is beschikbaar in de volgende lagen:

| Laag | Beschrijving |
|---|---|
Basic | Een cache met één knooppunt. Deze laag biedt ondersteuning voor meerdere geheugengrootten (250 MB - 53 GB). Dit is een ideale laag voor het ontwikkelen/testen en voor niet-essentiële werkbelastingen. De Basic-laag heeft geen SLA (Service Level Agreement) |
| Standard | Een gerepliceerde cache in een configuratie met twee knooppunten (primair/secundair) die door Microsoft wordt beheerd, met een SLA met hoge beschikbaarheid (99,9%). |
| Premium | De Premium-laag is de laag die geschikt is voor bedrijven. Caches op de Premium-laag ondersteunen meer functies en hebben een hogere doorvoersnelheid met een lagere latentie. Caches op de Premium-laag worden geïmplementeerd op krachtigere hardware en bieden daarom betere prestaties in vergelijking met de Basic- of Standard-laag. Dit voordeel betekent dat de doorvoer voor een cache van dezelfde grootte sneller gaat bij Premium dan bij Standard |

> [!TIP]
> Raadpleeg [Veelgestelde vragen over Azure Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte van premiumcaches.
>

U kunt de cache opschalen naar een hogere laag nadat u de cache al hebt gemaakt. Omlaag schalen naar een lagere laag wordt niet ondersteund. Zie voor stapsgewijze instructies [De schaal aanpassen van Azure Redis Cache](cache-how-to-scale.md) en [Een schaalbewerking automatiseren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparision"></a>Vergelijking van functies

De pagina [Prijzen van Redis Cache](https://azure.microsoft.com/pricing/details/cache/) bevat een gedetailleerde vergelijking van elke laag. In de volgende tabel worden de functies beschreven die door elke laag worden ondersteund:

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

* [Snelstart ASP.NET-webapp](cache-web-app-howto.md) Maak een eenvoudige ASP.NET-webapp die gebruikmaakt van een Azure Redis Cache.
* [Snelstart .NET](cache-dotnet-how-to-use-azure-redis-cache.md) Maak een eenvoudige .NET-app die gebruikmaakt van een Azure Redis Cache.
* [Quickstart voor .NET Core](cache-dotnet-core-quickstart.md) Maak een eenvoudige .NET-app die gebruikmaakt van een Azure Redis Cache.
* [Snelstart Node.js](cache-nodejs-get-started.md) Maak een eenvoudige Node.js-app die gebruikmaakt van een Azure Redis Cache.
* [Snelstart Java](cache-java-get-started.md) Maak een eenvoudige Java-app die gebruikmaakt van een Azure Redis Cache.
* [Snelstart Python](cache-python-get-started.md) Maak een eenvoudige Python-app die gebruikmaakt van een Azure Redis Cache.
