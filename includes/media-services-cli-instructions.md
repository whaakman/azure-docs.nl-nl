---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/25/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b335cf996de41f4eea15af1899a0c6654c2f679f
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104980"
---
## <a name="open-cli-shell"></a>Open CLI Shell

Het verdient aanbeveling gebruik [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) CLI-opdrachten uitvoeren. **Cloud Shell** is een gratis, interactieve shell die u gebruiken kunt om uit te voeren van de stappen in dit artikel. Er zijn vooraf algemene Azure-hulpprogramma's geïnstalleerd en geconfigureerd in Cloud Shell die u kunt gebruiken bij uw account. Selecteer de knop kopiëren aan de code kopiëren, plak deze in de Cloud Shell en druk op Enter uit te voeren. U kunt Cloud Shell op verschillende manieren openen:

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Aanmelden

Aan de slag met de CLI-shell (in de cloud of lokaal), voer `az login` voor het maken van een verbinding met Azure.

Als de CLI uw standaardbrowser kan openen, gebeurt dat ook en wordt er een aanmeldingspagina gedownload. Anders moet u een browserpagina openen en volg de instructies op de opdrachtregel voor het invoeren van een autorisatiecode na het navigeren naar https://aka.ms/devicelogin in uw browser.

### <a name="specify-location-of-files"></a>Geef de locatie van bestanden

Veel Media Services CLI-opdrachten kunnen u een parameter met een bestandsnaam doorgeven. 

Als u **Azure Cloud Shell**, het bestand te uploaden **Azure Cloud Shell**. Hier vindt u de knop uploaden/downloaden van bestanden aan de bovenkant van de shell-venster. Vervolgens verwijzen naar het bestand als volgt: `@{FileName}.` 

![Bestanden uploaden]

Als u de Azure CLI lokaal gebruikt, geeft u het volledige bestandspad. Bijvoorbeeld `@c:\tracks.json`.


[Bestanden uploaden]: ./media/media-services-cli/upload-download-files.png
