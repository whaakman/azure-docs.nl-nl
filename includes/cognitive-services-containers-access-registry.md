---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: 02beb6bdce096c35d8948cc8aefab6cc97be907c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063951"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>De Docker-CLI gebruiken om te verifiëren van de privécontainerregister

U kunt verifiëren met de privécontainerregister voor Cognitive Services-Containers in een van de verschillende manieren, maar de aanbevolen methode vanaf de opdrachtregel is met de [Docker-Opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/).

Gebruik de [ `docker login` opdracht](https://docs.docker.com/engine/reference/commandline/login/), zoals wordt weergegeven in het volgende voorbeeld, voor aanmelding bij `containerpreview.azurecr.io`, de privécontainerregister voor Cognitive Services-Containers. Vervang *\<gebruikersnaam\>* met de naam van de gebruiker en *\<wachtwoord\>* met het wachtwoord dat is opgegeven in de referenties die u hebt ontvangen van de Azure Cognitive Services-team.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Als u uw referenties in een tekstbestand hebt beveiligd, kunt u de inhoud van het tekstbestand, samen met behulp van de `cat` opdracht naar de `docker login` opdracht, zoals wordt weergegeven in het volgende voorbeeld. Vervang *\<passwordFile\>* met het pad en de naam van het tekstbestand dat het wachtwoord bevat en *\<gebruikersnaam\>* met de naam van de gebruiker dat opgegeven in uw referenties.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
