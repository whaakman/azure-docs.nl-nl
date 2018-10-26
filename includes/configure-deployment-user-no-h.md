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
ms.openlocfilehash: b5512ed5810ebd05596dc3ca7a29957b4c595c59
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035101"
---
Maak implementatiereferenties in Cloud Shell met de opdracht [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set). Deze implementatiegebruiker is vereist voor FTP- en lokale Git-implementatie naar een webtoepassing. De gebruikersnaam en het wachtwoord staan op accountniveau. _Deze verschillen van de referenties van uw Azure-abonnement._

Vervang in het volgende voorbeeld *\<username>* en *\<password>* (inclusief punthaken) door een nieuwe gebruikersnaam en een nieuw wachtwoord. De gebruikersnaam moet binnen Azure uniek zijn. Het wachtwoord moet ten minste acht tekens lang zijn en minimaal twee van de volgende drie typen elementen bevatten: letters, cijfers, symbolen. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

U zou een JSON-uitvoer moeten krijgen waarin het wachtwoord wordt weergegeven als `null`. Als er een `'Conflict'. Details: 409`-fout optreedt, wijzigt u de gebruikersnaam. Als er een ` 'Bad Request'. Details: 400`-fout optreedt, kiest u een sterker wachtwoord.

U hoeft deze implementatiegebruiker maar één keer te maken. Vervolgens kunt u deze voor al uw Azure-implementaties gebruiken.

> [!NOTE]
> Leg de gebruikersnaam en het wachtwoord vast. U hebt ze later nodig bij het implementeren van de webtoepassing.
>
>
