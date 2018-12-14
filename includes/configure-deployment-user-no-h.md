---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 95d89da66935ce933fee082a5f53ee2e36ea953f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344683"
---
In de Cloud Shell, configureert u referenties voor implementatie met de [ `az webapp deployment user set` ](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) opdracht. Deze implementatiegebruiker is vereist voor FTP- en lokale Git-implementatie naar een webtoepassing. De gebruikersnaam en het wachtwoord staan op accountniveau. _Deze verschillen van de referenties van uw Azure-abonnement._

Vervang in het volgende voorbeeld *\<username>* en *\<password>* (inclusief punthaken) door een nieuwe gebruikersnaam en een nieuw wachtwoord. De gebruikersnaam moet binnen Azure uniek zijn. Het wachtwoord moet ten minste acht tekens lang zijn en minimaal twee van de volgende drie typen elementen bevatten: letters, cijfers, symbolen. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

U zou een JSON-uitvoer moeten krijgen waarin het wachtwoord wordt weergegeven als `null`. Als er een `'Conflict'. Details: 409`-fout optreedt, wijzigt u de gebruikersnaam. Als er een ` 'Bad Request'. Details: 400`-fout optreedt, kiest u een sterker wachtwoord.

U moet slechts één keer; deze implementatiegebruiker configureren u kunt deze gebruiken voor al uw Azure-implementaties.

> [!NOTE]
> Leg de gebruikersnaam en het wachtwoord vast. U hebt ze later nodig bij het implementeren van de webtoepassing.
>
>
