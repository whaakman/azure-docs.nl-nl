---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b9d0d2b97472eb3264f5e4600fddbfc7d3250918
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704181"
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
