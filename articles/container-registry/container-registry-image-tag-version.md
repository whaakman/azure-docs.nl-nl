---
title: De code en versie-installatiekopieën in Azure Container Registry
description: Best practices voor tags en versiebeheer Docker-containerinstallatiekopieën
services: container-registry
author: stevelasker
ms.service: container-registry
ms.topic: article
ms.date: 07/10/2019
ms.author: steve.lasker
ms.openlocfilehash: bd00fd4f8dd247c766eb34849ecf9de603c5171b
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800391"
---
# <a name="recommendations-for-tagging-and-versioning-container-images"></a>Aanbevelingen voor het labelen en versiebeheer containerinstallatiekopieën

Bij het pushen van containerinstallatiekopieën implementeren naar een containerregister en deze vervolgens te implementeren, moet u een strategie voor afbeeldingen taggen en versiebeheer. Dit artikel wordt beschreven, waarbij elk past tijdens de levenscyclus van de container en op twee manieren:

* **Tags stabiele** -labels die u opnieuw, bijvoorbeeld gebruiken om te wijzen op een primaire of secundaire versie zoals *mycontainerimage:1.0*.
* **De unieke tags** -een ander label voor elke afbeelding die u naar een register, zoals pushen *mycontainerimage:abc123*.

## <a name="stable-tags"></a>Stabiele tags

**Aanbeveling**: Stabiele tags gebruiken om te onderhouden **baseren installatiekopieën** voor de container wordt gemaakt. Implementaties met stabiele tags vermijden, omdat deze tags nog steeds updates ontvangen en leiden inconsistenties in productieomgevingen tot kunnen.

*Tags stabiele* betekenen dat een ontwikkelaar of een build-systeem kunt blijven voor het ophalen van een specifieke tag, die wordt voortgezet om updates te downloaden. Stabiele betekent niet dat de inhoud zijn geblokkeerd. In plaats daarvan houdt stabiel in dat de afbeelding moet stabiele voor het doel van deze versie. Om te blijven 'stabiel', kan deze worden verwerkt als beveiligingspatches of framework-updates wilt toepassen.

### <a name="example"></a>Voorbeeld

Een team framework wordt geleverd versie 1.0. Ze weten dat ze updates, inclusief kleine updates verzenden. Ter ondersteuning van stabiele tags voor een bepaalde versie van de primaire en secundaire, hebben ze twee sets met stabiele tags.

* `:1` -een stabiel tag voor de primaire versie. `1` Hiermee geeft u de 'nieuwste' of 'laatste' 1.*-versie.
* `:1.0`-een stabiel tag voor versie 1.0, zodat een ontwikkelaar verbinding maken met updates van 1.0 en niet worden doorgevoerd naar 1.1 wanneer dit wordt uitgebracht.

Het team gebruikt ook de `:latest` tag, die naar de nieuwste stabiele tag verwijst, ongeacht wat de huidige primaire versie is.

Wanneer de basisinstallatiekopie updates beschikbaar zijn, of elk type van het onderhoud van de versie van het framework, afbeeldingen met de stabiele codes zijn bijgewerkt naar de nieuwste digest die staat voor de nieuwste stabiele versie van die versie.

In dit geval worden de primaire en secundaire labels voortdurend wordt afgehandeld. Hiermee kunt de eigenaar van de installatiekopie voor installatiekopieën van het onderhouden van een scenario basisinstallatiekopie.

## <a name="unique-tags"></a>De unieke tags

**Aanbeveling**: De unieke tags voor gebruiken **implementaties**, met name in een omgeving die kan worden geschaald op meerdere knooppunten. U wilt waarschijnlijk doelbewuste implementaties van een consistente versie van onderdelen. Als de container wordt opnieuw opgestart of een orchestrator uitgeschaald meer instanties wordt uw hosts wordt niet per ongeluk een nieuwere versie, niet consistent met de andere knooppunten ophalen.

De unieke tagging gewoon betekent dat elke installatiekopie naar een register gepusht heeft een unieke code. Tags niet opnieuw worden gebruikt. Er zijn verschillende patronen die u volgen kunt om het genereren van unieke tags, met inbegrip van:

* **Datum / tijd stempel** -deze aanpak is redelijk gebruikelijk is, omdat u kunt duidelijk zien wanneer de installatiekopie is gemaakt. Maar hoe u deze terug naar uw build-systeem te correleren? Hebt u de build is voltooid op hetzelfde moment vinden? Welke tijdzone bent u erbij? Zijn al uw build-systemen naar UTC gekalibreerd?
* **GIT-doorvoer** : deze methode werkt totdat u start basisinstallatiekopie updates ondersteunt. Als een update van de basisinstallatiekopie van het geval is, begint uw build-systeem met de dezelfde Git-doorvoer als de vorige versie. De basisinstallatiekopie heeft echter nieuwe inhoud. In het algemeen een Git-doorvoer hier een *semi*-tag stabiel.
* **Manifest digest** -elke containerinstallatiekopie gepusht naar een containerregister is gekoppeld met een manifest geïdentificeerd door een unieke SHA-256-hash of samenvatting. Hoewel uniek is, wordt de samenvatting is lang, moeilijk te lezen is en niet-gerelateerde met uw build-omgeving.
* **Bouw ID** -deze optie worden aanbevolen aangezien dit waarschijnlijk incrementele, en kunt u correlaties zichtbaar maken tussen terug naar de specifieke build te zoeken naar alle artefacten en Logboeken. Echter, zoals een manifest digest, kan het moeilijk zijn voor een human te lezen.

  Als uw organisatie verschillende build-systemen heeft, voorvoegsel van de tag met de naam van build-systeem is een variatie op deze optie: `<build-system>-<build-id>`. Bijvoorbeeld, u builds kan onderscheiden van de API-team Jenkins build-systeem en van de webteam Azure pijplijnen bouwen-systeem.

## <a name="next-steps"></a>Volgende stappen

Voor een uitgebreidere bespreking van de concepten in dit artikel, Zie het blogbericht [Docker labelen: Aanbevolen procedures voor docker-installatiekopieën taggen en versiebeheer](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/).

Om te helpen de prestaties en het rendabele gebruik van uw Azure container registry te maximaliseren, Zie [aanbevolen procedures voor Azure Container Registry](container-registry-best-practices.md).

<!-- IMAGES -->


<!-- LINKS - Internal -->

