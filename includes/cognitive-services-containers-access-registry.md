---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: 27c6b59a458fb79b86d7064d710a01593a1745dc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66125133"
---
## <a name="use-docker-cli-to-authenticate-private-container-registry"></a>Docker-Opdrachtregelinterface gebruiken om te verifiëren van privécontainerregister

Er zijn verschillende manieren om te verifiëren met de privécontainerregister voor Cognitive Services-Containers, maar wordt aanbevolen om vanaf de opdrachtregel met behulp van de [Docker-Opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/).

Gebruik de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht, zoals wordt weergegeven in het volgende voorbeeld wordt aan te melden bij `containerpreview.azurecr.io`, de privécontainerregister voor Cognitive Services-Containers. Vervang *\<gebruikersnaam\>* met de naam van de gebruiker en *\<wachtwoord\>* met het wachtwoord dat is opgegeven in de referenties die u hebt ontvangen van de Azure Cognitive Services-team.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Als u uw referenties in een tekstbestand hebt beveiligd, kunt u de inhoud van deze tekst samenvoegen-bestand, met behulp van de `cat` opdracht naar de `docker login` opdracht zoals wordt weergegeven in het volgende voorbeeld. Vervang *\<passwordFile\>* met het pad en de naam van het tekstbestand met het wachtwoord en *\<gebruikersnaam\>* met de naam van de gebruiker opgegeven in uw referenties.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
