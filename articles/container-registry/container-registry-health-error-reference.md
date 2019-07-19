---
title: Fout referentie voor status controle-Azure Container Registry
description: Fout codes en mogelijke oplossingen voor problemen die worden gevonden door de opdracht AZ ACR check-Health Diagnostic uit te voeren in Azure Container Registry
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 672d446fa8dc27612c7b046cac109bfa4ca5fec5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309378"
---
# <a name="health-check-error-reference"></a>Naslag informatie voor status controle fout

Hieronder vindt u informatie over de fout codes die worden geretourneerd door de opdracht [AZ ACR check-Health][az-acr-check-health] . Voor elke fout worden mogelijke oplossingen weer gegeven.

## <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

Deze fout houdt in dat docker-client voor CLI niet kan worden gevonden. Als gevolg hiervan worden de volgende aanvullende controles niet uitgevoerd: een docker-versie vinden, de status van docker daemon evalueren en een docker-pull-opdracht uitvoeren.

*Mogelijke oplossingen*: Docker-client installeren; Docker-pad toevoegen aan de systeem variabelen.

## <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

Deze fout betekent dat de status van de docker-daemon niet beschikbaar is of niet kan worden bereikt met de CLI. Als gevolg hiervan zijn docker-bewerkingen (zoals `docker login` en `docker pull`) niet beschikbaar via de cli.

*Mogelijke oplossingen*: Start docker daemon opnieuw of Controleer of deze juist is geïnstalleerd.

## <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

Deze fout betekent dat CLI de opdracht `docker --version`niet kan uitvoeren.

*Mogelijke oplossingen*: Probeer de opdracht hand matig uit te voeren, zorg ervoor dat u over de meest recente CLI-versie beschikt en onderzoek het fout bericht.

## <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

Deze fout betekent dat de CLI geen voorbeeld installatie kopie in uw omgeving kan ophalen.

*Mogelijke oplossingen*: Controleer of alle onderdelen die nodig zijn om een installatie kopie te halen goed worden uitgevoerd.

## <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

Deze fout betekent dat de helm-client niet kan worden gevonden door de CLI, waardoor andere helm-bewerkingen worden uitgesloten.

*Mogelijke oplossingen*: Controleer of de helm-client is geïnstalleerd en of het pad wordt toegevoegd aan de omgevings variabelen van het systeem.

## <a name="helmversionerror"></a>HELM_VERSION_ERROR

Deze fout betekent dat de CLI niet kan bepalen welke helm-versie is geïnstalleerd. Dit kan gebeuren als de Azure CLI-versie (of de versie van de helm) die wordt gebruikt, verouderd is.

*Mogelijke oplossingen*: Update naar de nieuwste versie van Azure CLI of naar de aanbevolen helm-versie. Voer de opdracht hand matig uit en onderzoek het fout bericht.

## <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

Deze fout betekent dat de DNS voor de opgegeven register aanmeldings server is gepingd, maar niet reageert, wat betekent dat deze niet beschikbaar is. Dit kan duiden op problemen met de verbinding. Het is ook mogelijk dat het REGI ster niet bestaat. de gebruiker heeft mogelijk niet de juiste machtigingen voor het REGI ster (om de aanmeldings server correct op te halen) of het doel register bevindt zich in een andere Cloud dan het REGI ster dat wordt gebruikt in de Azure CLI.

*Mogelijke oplossingen*: Verbinding valideren; Controleer de spelling van het REGI ster en het REGI ster bestaat. Controleer of de gebruiker de juiste machtigingen heeft en of de cloud van het REGI ster hetzelfde is als in de Azure CLI.

## <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

Deze fout houdt in dat het controle-eind punt voor het opgegeven REGI ster heeft gereageerd met een 403 verboden HTTP-status. Deze fout betekent dat gebruikers geen toegang hebben tot het REGI ster, waarschijnlijk vanwege een configuratie van een virtueel netwerk.

*Mogelijke oplossingen*: Verwijder de regels voor het virtuele netwerk of Voeg het huidige IP-adres van de client toe aan de lijst met toegestane clients.

## <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

Deze fout houdt in dat het controle-eind punt van het doel register geen uitdaging heeft uitgegeven.

*Mogelijke oplossingen*: Probeer het na enige tijd opnieuw. Als de fout zich blijft voordoen, opent u https://aka.ms/acr/issues een probleem op.

## <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Deze fout houdt in dat het controle-eind punt van het doel register een Challenge heeft uitgegeven, maar het REGI ster biedt geen ondersteuning voor Azure Active Directory-verificatie.

*Mogelijke oplossingen*: Probeer een andere manier om te verifiëren, bijvoorbeeld met beheerders referenties. Als gebruikers moeten worden geverifieerd met behulp van Azure Active Directory, open https://aka.ms/acr/issues dan een probleem op.

## <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Deze fout betekent dat de aanmeldings server van het REGI ster niet met een vernieuwings token heeft gereageerd, zodat de toegang tot het doel register is geweigerd. Deze fout kan optreden als de gebruiker niet over de juiste machtigingen beschikt voor het REGI ster of als de gebruikers referenties voor de Azure CLI verouderd zijn.

*Mogelijke oplossingen*: Controleer of de gebruiker over de juiste machtigingen beschikt voor het REGI ster. uitvoeren `az login` om machtigingen, tokens en referenties te vernieuwen.

## <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Deze fout betekent dat de aanmeldings server van het REGI ster niet met een toegangs token heeft gereageerd, zodat de toegang tot het doel register is geweigerd. Deze fout kan optreden als de gebruiker niet over de juiste machtigingen beschikt voor het REGI ster of als de gebruikers referenties voor de Azure CLI verouderd zijn.

*Mogelijke oplossingen*: Controleer of de gebruiker over de juiste machtigingen beschikt voor het REGI ster. uitvoeren `az login` om machtigingen, tokens en referenties te vernieuwen.

## <a name="loginservererror"></a>LOGIN_SERVER_ERROR

Deze fout betekent dat de CLI de aanmeldings server van het opgegeven REGI ster niet kan vinden en dat er geen standaard achtervoegsel is gevonden voor de huidige Cloud. Deze fout kan optreden als het REGI ster niet bestaat, als de gebruiker niet over de juiste machtigingen beschikt voor het REGI ster, als de cloud van het REGI ster en de huidige Azure CLI-Cloud niet overeenkomen of als de Azure CLI-versie verouderd is.

*Mogelijke oplossingen*: Controleer of de spelling juist is en of het REGI ster bestaat. Controleer of de gebruiker over de juiste machtigingen beschikt voor het REGI ster en of de Clouds van het REGI ster en de CLI-omgeving overeenkomen. werk Azure CLI bij naar de nieuwste versie.

## <a name="next-steps"></a>Volgende stappen

Zie [de status van een Azure container Registry controleren](container-registry-check-health.md)voor opties voor het controleren van de status van een REGI ster.

Raadpleeg de [Veelgestelde](container-registry-faq.md) vragen over Azure container Registry en andere bekende problemen met betrekking tot de beveiliging.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
