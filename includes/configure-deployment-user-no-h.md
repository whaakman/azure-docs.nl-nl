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
ms.openlocfilehash: 4f061dc66829c49fba6722215dc36ed2ce6464ee
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58919211"
---
In de Azure Cloud Shell, configureert u referenties voor implementatie met de [ `az webapp deployment user set` ](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) opdracht. Deze implementatiegebruiker is vereist voor FTP- en lokale Git-implementatie naar een webtoepassing. De gebruikersnaam en het wachtwoord staan op accountniveau. _Ze verschillen van de referenties van uw Azure-abonnement._

Vervang in het volgende voorbeeld  *\<gebruikersnaam >* en  *\<wachtwoord >*, met inbegrip van de vierkante haken, met een nieuwe gebruikersnaam en wachtwoord. De gebruikersnaam moet uniek zijn binnen Azure. Het wachtwoord moet ten minste acht tekens lang zijn, met twee van de volgende drie elementen: letters, cijfers en symbolen. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

U krijgt een JSON-uitvoer met het wachtwoord wordt weergegeven als `null`. Als er een `'Conflict'. Details: 409`-fout optreedt, wijzigt u de gebruikersnaam. Als er een ` 'Bad Request'. Details: 400`-fout optreedt, kiest u een sterker wachtwoord. Gebruikersnaam van de implementatie mag geen ' @' symbool voor lokale Git-pushes.

U configureren deze implementatiegebruiker maar één keer. U kunt deze gebruiken voor al uw Azure-implementaties.

> [!NOTE]
> Noteer de gebruikersnaam en wachtwoord. U hebt ze later nodig bij het implementeren van de webtoepassing.
>
>
