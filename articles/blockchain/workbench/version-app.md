---
title: Versiebeheer van Blockchain-toepassingen in Azure Blockchain Workbench
description: Het gebruik van toepassingen in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 1/8/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: c4c5dcf9c83b79158e3459e79db6dd066d982fc8
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108256"
---
# <a name="azure-blockchain-workbench-application-versioning"></a>Azure Blockchain Workbench-toepassing-versiebeheer

U kunt meerdere versies van een Azure Blockchain Workbench-app maken en gebruiken. Als meerdere versies van dezelfde applicatie zijn geüpload, is er een versiegeschiedenis beschikbaar en kunnen gebruikers kiezen welke versie ze willen gebruiken.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie [Azure Blockchain Workbench-implementatie](deploy.md) voor details over implementatie
* Een geïmplementeerde blockchain-toepassing in Blockchain Workbench. Zie [een blockchain-toepassing maken in Azure Blockchain Workbench](create-app.md)

## <a name="add-an-app-version"></a>Een appversie toevoegen

Voeg de nieuwe configuratie en smart contractbestanden toe aan Blockchain Workbench om een nieuwe versie toe te voegen.

1. Navigeer in een webbrowser naar het webadres van Blockchain Workbench. Bijvoorbeeld, `https://{workbench URL}.azurewebsites.net/` Zie voor meer informatie over het vinden van uw Blockchain Workbench webadres [Blockchain Workbench Web-URL](deploy.md#blockchain-workbench-web-url)
2. Meld u aan als een [Blockchain Workbench beheerder](manage-users.md#manage-blockchain-workbench-administrators).
3. Selecteer de blockchain-toepassing die u wilt bijwerken met een andere versie.
4. Selecteer **toevoegen versie**. Het **toevoegen versie** deelvenster wordt weergegeven.
5. Kies de nieuwe configuratie van het versie-contract en contract codebestanden om te uploaden. Het configuratiebestand wordt automatisch gevalideerd. Corrigeer eventuele validatiefouten voordat u de toepassing implementeert.
6. Selecteer **toevoegen versie** om de nieuwe versie van de blockchain-toepassing toe te voegen.

    ![Een nieuwe versie toevoegen](media/version-app/add-version.png)

Implementatie van de blockchain-toepassing kan een paar minuten duren. Wanneer de implementatie is voltooid, vernieuwt u de toepassingspagina. Kies de toepassing en selecteer de knop **versiegeschiedenis** om een versiegeschiedenis van de toepassing te tonen.

> [!IMPORTANT]
> Vorige versies van de toepassing zijn uitgeschakeld. U kunt vorige versies afzonderlijk opnieuw inschakelen.
>
> Mogelijk moet u leden opnieuw toevoegen aan toepassingsrollen als er wijzigingen zijn aangebracht in de toepassingsrollen in de nieuwe versie.

## <a name="using-app-versions"></a>Gebruik van app-versies

Standaard wordt de nieuwste ingeschakelde versie van de toepassing gebruikt in Blockchain Workbench. Als u een vorige versie van een toepassing wilt gebruiken, moet u eerst de versie kiezen via de toepassingspagina.

1. Selecteer in de sectie toepassingen van Blockchain Workbench het selectievakje de toepassing die het contract bevat dat u wilt gebruiken. Als eerdere versies zijn ingeschakeld, is de knop versie geschiedenis beschikbaar.
2. Selecteer de knop **versiegeschiedenis**.
3. Kies in het deelvenster van de versiegeschiedenis de versie van de toepassing door de koppeling te selecteren in de kolom *wijzigingsdatum*.

    ![Kies een vorige versie](media/version-app/use-version.png)

    U kunt nieuwe contracten maken of acties uitvoeren op contracten van eerdere versies. De versie van de applicatie wordt weergegeven na de naam van de applicatie en er wordt een waarschuwing weergegeven over de oudere versie.

## <a name="next-steps"></a>Volgende stappen

* [Azure Blockchain Workbench oplossen van problemen](troubleshooting.md)
