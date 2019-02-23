---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: 11a336bbcf75c6c4de61f1bb681ab6ee7aa05650
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741406"
---
U moet eerst invullen en verzenden de [Cognitive Services beeld Containers aanvraagformulier](https://aka.ms/VisionContainersPreview) toegang vragen tot de container. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u de container gebruikt. Wanneer verzonden, controleert het team van Azure Cognitive Services het formulier om ervoor te zorgen dat u voldoet aan de criteria voor toegang tot de persoonlijke container registry.

> [!IMPORTANT]
> U moet een e-mailadres dat is gekoppeld aan een Microsoft-Account (MSA) of Azure Active Directory (Azure AD)-account in het formulier.

Als uw aanvraag is goedgekeurd, ontvangt u vervolgens een e-mailbericht met instructies voor het verkrijgen van uw referenties en toegang tot het privé-container-register.

## <a name="log-in-to-the-private-container-registry"></a>Meld u aan bij de privé containerregister

Er zijn verschillende manieren om te verifiëren met de privécontainerregister voor Cognitive Services-Containers, maar wordt aanbevolen om vanaf de opdrachtregel met behulp van de [Docker-Opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/).

Gebruik de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht, zoals wordt weergegeven in het volgende voorbeeld wordt aan te melden bij `containerpreview.azurecr.io`, de privécontainerregister voor Cognitive Services-Containers. Vervang *\<gebruikersnaam\>* met de naam van de gebruiker en *\<wachtwoord\>* met het wachtwoord dat is opgegeven in de referenties die u hebt ontvangen van de Azure Cognitive Services-team.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Als u uw referenties in een tekstbestand hebt beveiligd, kunt u de inhoud van deze tekst samenvoegen-bestand, met behulp van de `cat` opdracht naar de `docker login` opdracht zoals wordt weergegeven in het volgende voorbeeld. Vervang *\<passwordFile\>* met het pad en de naam van het tekstbestand met het wachtwoord en *\<gebruikersnaam\>* met de naam van de gebruiker opgegeven in uw referenties.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

