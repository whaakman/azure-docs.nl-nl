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
ms.openlocfilehash: 026540290710d039dbc06c394ab538ebe2d7c12f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344666"
---
Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang _&lt;deploymentLocalGitUrl-from-create-step>_ door de URL van de externe Git-instantie die u hebt opgeslagen in [Een web-app maken](#create-a-web-app).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Wanneer u door Git Credential Manager om referenties wordt gevraagd, geeft u de referenties op die u hebt gemaakt in [Een implementatiegebruiker configureren](#configure-a-deployment-user) en niet de referenties die u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

Het kan enkele minuten duren voor deze opdracht is uitgevoerd. De opdracht geeft informatie weer die lijkt op het volgende voorbeeld:
