---
author: IEvangelist
ms.author: dapine
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.openlocfilehash: 993b0e8cc5b1ec482b2f6041dfc970dc7e7409dd
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229324"
---
Invullen en verzenden de [Cognitive Services beeld Containers aanvraagformulier](https://aka.ms/VisionContainersPreview) toegang vragen tot de container. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u de container gebruikt. Nadat u het formulier hebt verzonden, controleert het team van Azure Cognitive Services om er zeker van te zijn dat u voldoet aan de criteria voor toegang tot de persoonlijke container registry.

> [!IMPORTANT]
> U moet een e-mailadres dat is gekoppeld aan een Microsoft-Account (MSA) of Azure Active Directory (Azure AD)-account in het formulier.

Als uw aanvraag is goedgekeurd, ontvangt u een e-mailbericht met instructies over het verkrijgen van uw referenties en toegang tot het privé-container-register.

## <a name="log-in-to-the-private-container-registry"></a>Meld u aan bij de privé containerregister

Er zijn verschillende manieren om te verifiëren met de privécontainerregister voor Cognitive Services-containers. Het is raadzaam dat u de methode via de opdrachtregel met behulp van gebruiken de [Docker-Opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/).

Gebruik de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht, zoals wordt weergegeven in het volgende voorbeeld, voor aanmelding bij `containerpreview.azurecr.io`, dit is de privécontainerregister voor Cognitive Services-containers. Vervang *\<gebruikersnaam\>* met de naam van de gebruiker en *\<wachtwoord\>* met het wachtwoord dat is opgegeven in de referenties die u hebt ontvangen van de Azure Cognitive Services-team.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Als u uw referenties in een tekstbestand beveiligd, kunt u de inhoud van het tekstbestand op samenvoegen de `docker login` opdracht. Gebruik de `cat` opdracht, zoals wordt weergegeven in het volgende voorbeeld. Vervang *\<passwordFile\>* met het pad en de naam van het tekstbestand dat het wachtwoord bevat. Vervang *\<gebruikersnaam\>* met de naam van de gebruiker opgegeven in uw referenties.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

