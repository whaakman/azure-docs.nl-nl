---
title: Naslaginformatie voor fouten voor statuscontrole - Azure Container Registry
description: Foutcodes en mogelijke oplossingen voor problemen die zijn gevonden door het uitvoeren van de opdracht az acr-status in Azure Container Registry
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: fc29b27cbb7eea983140c59529d981ad95c27ae8
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555109"
---
# <a name="health-check-error-reference"></a>Naslaginformatie voor fouten van de gezondheid van selectievakje

Hieronder vindt u meer informatie over foutcodes die zijn geretourneerd door de [az acr-status][az-acr-check-health] opdracht. Voor elke fout worden mogelijke oplossingen weergegeven.

## <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

Deze fout betekent dat Docker-client voor CLI niet kan worden gevonden. Als gevolg hiervan niet de volgende extra controles worden uitgevoerd: Docker-versie, evaluatie van Docker-daemon-status zoeken en het uitvoeren van een Docker pull-opdracht.

*Mogelijke oplossingen*: Installeren van Docker-client. Docker-pad toevoegen aan de systeemvariabelen.

## <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

Deze fout betekent dat de status van de Docker-daemon niet beschikbaar is, of dat deze kan niet worden bereikt met behulp van de CLI. Als gevolg hiervan, Docker-bewerkingen (zoals `docker login` en `docker pull`) zijn niet beschikbaar via de CLI.

*Mogelijke oplossingen*: Docker-daemon opnieuw, of te valideren dat deze correct is geïnstalleerd.

## <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

Deze fout betekent CLI is niet kunnen worden uitgevoerd de opdracht `docker --version`.

*Mogelijke oplossingen*: Probeer de opdracht handmatig uit te voeren, zorg ervoor dat u de meest recente CLI-versie en het foutbericht te onderzoeken.

## <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

Deze fout betekent dat de CLI kan niet voor het ophalen van een voorbeeldafbeelding aan uw omgeving.

*Mogelijke oplossingen*: Valideren dat alle onderdelen die nodig zijn voor het ophalen van een installatiekopie van een correct worden uitgevoerd.

## <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

Deze fout betekent dat Helm-client kan niet worden gevonden door de CLI, die andere bewerkingen Helm uitsluit.

*Mogelijke oplossingen*: Controleer of dat Helm-client is geïnstalleerd en dat het pad wordt toegevoegd aan de omgevingsvariabelen van het systeem.

## <a name="helmversionerror"></a>HELM_VERSION_ERROR

Deze fout betekent dat de CLI is niet mogelijk te bepalen van de Helm-versie die is geïnstalleerd. Dit kan gebeuren als de Azure CLI-versie (of als de Helm-versie) wordt gebruikt is verouderd.

*Mogelijke oplossingen*: Update naar de meest recente versie van Azure CLI of naar de aanbevolen Helm-versie; Voer de opdracht handmatig en onderzoek van het foutbericht.

## <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

Deze fout betekent dat de DNS-server voor de aanmeldingsserver van het opgegeven register is gepingd, maar niet reageert, wat betekent dat deze niet beschikbaar is. Dit kan duiden op sommige problemen met de netwerkverbinding. U kunt ook het register bestaat mogelijk niet, de gebruiker mogelijk niet de machtigingen op het register (om op te halen de aanmeldingsserver correct) of de doelregister is in een andere cloud dan de versie die wordt gebruikt in de Azure CLI.

*Mogelijke oplossingen*: Connectiviteit; valideren Controleer de spelling van het register en deze registersleutel bestaat. Controleer of dat de gebruiker de juiste machtigingen op deze heeft en van het register cloud is hetzelfde die wordt gebruikt in de Azure CLI.

## <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

Deze fout betekent dat het eindpunt van de uitdaging voor de opgegeven register gereageerd met de status 403 HTTP is niet toegestaan. Deze fout betekent dat gebruikers geen toegang tot het register, waarschijnlijk vanwege de configuratie van een virtueel netwerk hebt.

*Mogelijke oplossingen*: Regels voor virtueel netwerk verwijderen of de huidige IP-adres van client toevoegen aan de lijst met toegestane.

## <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

Deze fout betekent dat het eindpunt van de uitdaging van het doelregister niet een hele uitdaging heeft verleend.

*Mogelijke oplossingen*: Probeer het na enige tijd opnieuw. Als de fout blijft optreden, opent u een probleem op https://aka.ms/acr/issues.

## <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Deze fout betekent dat het eindpunt van de uitdaging van het doelregister een uitdaging uitgegeven, maar het register biedt geen ondersteuning voor Azure Active Directory-verificatie.

*Mogelijke oplossingen*: Probeer een andere manier om te verifiëren, bijvoorbeeld met beheerdersreferenties. Als u gebruikers wilt verifiëren met behulp van Azure Active Directory, opent u een probleem op https://aka.ms/acr/issues.

## <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Deze fout betekent dat de aanmeldingsserver van het register niet heeft gereageerd met een vernieuwingstoken zodat toegang tot het doelregister is geweigerd. Deze fout kan optreden als de gebruiker niet de juiste machtigingen in het register heeft of als de referenties van de gebruiker voor de Azure CLI verlopen zijn.

*Mogelijke oplossingen*: Controleer of de gebruiker de juiste machtigingen op het register heeft. Voer `az login` machtigingen, tokens en referenties te vernieuwen.

## <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Deze fout betekent dat de aanmeldingsserver van het register niet heeft gereageerd met een toegangstoken zodat de toegang tot het doelregister is geweigerd. Deze fout kan optreden als de gebruiker niet de juiste machtigingen in het register heeft of als de referenties van de gebruiker voor de Azure CLI verlopen zijn.

*Mogelijke oplossingen*: Controleer of de gebruiker de juiste machtigingen op het register heeft. Voer `az login` machtigingen, tokens en referenties te vernieuwen.

## <a name="loginservererror"></a>LOGIN_SERVER_ERROR

Deze fout betekent dat de CLI is niet gevonden de aanmeldingsserver van het register opgegeven en geen standaard-achtervoegsel voor de huidige cloud is gevonden. Deze fout kan optreden als het register niet bestaat, als de gebruiker heeft niet de juiste machtigingen in het register als van het register cloud en de huidige Azure CLI-cloud komen niet overeen, of als de Azure CLI-versie is verouderd.

*Mogelijke oplossingen*: Controleer of de spelling juist is en dat het register bestaat; Controleer of dat de gebruiker de juiste machtigingen heeft op het register en die de clouds van het register en de CLI-omgeving overeenkomen met; Azure CLI bijwerken naar de nieuwste versie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer opties om te controleren of de status van een register [controleert de status van een Azure container registry](container-registry-check-health.md).

Zie de [Veelgestelde vragen over](container-registry-faq.md) voor antwoorden op veelgestelde vragen en andere bekende problemen met Azure Container Registry.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
