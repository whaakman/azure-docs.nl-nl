---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: db431d7815cfcc006563bd6da438154ef77ae6e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814917"
---
## <a name="cli-shell"></a>CLI Shell

Het verdient aanbeveling gebruik [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) CLI-opdrachten uitvoeren. **Cloud Shell** is een gratis, interactieve shell die u gebruiken kunt om uit te voeren van de stappen in dit artikel. Er zijn vooraf algemene Azure-hulpprogramma's geïnstalleerd en geconfigureerd in Cloud Shell die u kunt gebruiken bij uw account. Het biedt de flexibiliteit van het kiezen van de shell-ervaring die het beste bij de manier waarop die u werkt. Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

U kunt ook de CLI lokaal installeren. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor instructies voor uw platform.

### <a name="sign-in"></a>Aanmelden

Met behulp van een lokale installatie van de CLI, moet aanmelden bij Azure. Deze stap is niet vereist voor Azure Cloud Shell. Meld u aan met de `az login` opdracht.

Als de CLI uw standaardbrowser kan openen, gebeurt dat ook en wordt er een aanmeldingspagina gedownload. Anders moet u een browserpagina openen en volg de instructies op de opdrachtregel voor het invoeren van een autorisatiecode na het navigeren naar https://aka.ms/devicelogin in uw browser.

### <a name="specify-location-of-files"></a>Geef de locatie van bestanden

Veel Media Services CLI-opdrachten kunnen u een parameter met een bestandsnaam doorgeven. Als u **Cloud Shell**, kunt u het bestand uploaden naar uw clouddrive (met Bash of PowerShell). 

![Bestanden uploaden]

Of u gebruikmaakt van een lokale CLI of **Cloud Shell**, moet u opgeven van het bestandspad op basis van het besturingssysteem of de Cloud Shell (Bash of PowerShell) die u gebruikt. Hieronder ziet u een aantal voorbeelden:

Relatieve pad naar het bestand (alle OS)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Absolute pad op Linux/Mac en Windows-besturingssysteem

* `@ "/usr/home/mytestfile.json"`
*   `@"c:\tmp\user\mytestfile.json"`

Gebruik `{file}` als de opdracht om een pad naar het bestand vraagt. Bijvoorbeeld `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Gebruik `@{file}` als de opdracht wordt het opgegeven bestand niet laden. Bijvoorbeeld `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Bestanden uploaden]: ./media/media-services-cli/upload-download-files.png
