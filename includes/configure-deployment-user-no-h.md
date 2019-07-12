---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/14/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 4e699707db02de07f3d1ebb7d1fa8d0575a10aa3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836933"
---
FTP en lokale Git, kunt implementeren op een Azure-web-app met behulp van een *implementatiegebruiker*. Nadat u uw gebruikers-implementatie configureert, kunt u deze kunt gebruiken voor al uw Azure-implementaties. Uw implementatie accountniveau gebruikersnaam en wachtwoord verschillen van de referenties van uw Azure-abonnement. 

Voor het configureren van de implementatiegebruiker, voer de [az webapp deployment gebruikersset](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) opdracht in Azure Cloud Shell. Vervang \<username > en \<wachtwoord > met een implementatie-gebruikersnaam en wachtwoord. 

- De gebruikersnaam moet uniek zijn binnen Azure en voor lokale Git mag geen pushes, bevatten de ' @' symbool. 
- Het wachtwoord moet ten minste acht tekens lang zijn, met twee van de volgende drie elementen: letters, cijfers en symbolen. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

De JSON-uitvoer ziet u het wachtwoord als `null`. Als er een `'Conflict'. Details: 409`-fout optreedt, wijzigt u de gebruikersnaam. Als er een `'Bad Request'. Details: 400`-fout optreedt, kiest u een sterker wachtwoord. 

Uw gebruikersnaam en wachtwoord te gebruiken voor het implementeren van uw web-apps opnemen.
