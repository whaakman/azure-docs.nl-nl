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

U kunt maken en gebruiken van meerdere versies van een Azure Blockchain Workbench-app. Als er meerdere versies van dezelfde toepassing zijn geüpload, een versiegeschiedenis is beschikbaar en gebruikers kunnen kiezen welke versie moet worden gebruikt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie [Azure Blockchain Workbench-implementatie](deploy.md) voor details over implementatie
* Een geïmplementeerde blockchain-toepassing in Blockchain Workbench. Zie [een blockchain-toepassing maken in Azure Blockchain Workbench](create-app.md)

## <a name="add-an-app-version"></a>Een appversie toevoegen

Om toe te voegen een nieuwe versie, moet u de nieuwe configuratie en slimme contract bestanden uploaden Blockchain Workbench.

1. Navigeer in een webbrowser naar het webadres van Blockchain Workbench. Bijvoorbeeld, `https://{workbench URL}.azurewebsites.net/` Zie voor meer informatie over het vinden van uw Blockchain Workbench webadres [Blockchain Workbench Web-URL](deploy.md#blockchain-workbench-web-url)
2. Meld u aan als een [Blockchain Workbench beheerder](manage-users.md#manage-blockchain-workbench-administrators).
3. Selecteer de blockchain-toepassing die u wilt bijwerken met een andere versie.
4. Selecteer **toevoegen versie**. De **toevoegen versie** deelvenster wordt weergegeven.
5. Kiest u de nieuwe configuratie van de versie-contract en contract codebestanden te uploaden. Het configuratiebestand wordt automatisch gevalideerd. Corrigeer eventuele validatiefouten voordat u de toepassing implementeert.
6. Selecteer **toevoegen versie** om toe te voegen van de nieuwe versie van de blockchain-toepassingen.

    ![Een nieuwe versie toevoegen](media/version-app/add-version.png)

Implementatie van de blockchain-toepassing kan een paar minuten duren. Wanneer de implementatie is voltooid, vernieuwt u de toepassingspagina. Kies de toepassing en selecteer de **versiegeschiedenis** knop, bevat een overzicht van de toepassing.

> [!IMPORTANT]
> Vorige versies van de toepassing zijn uitgeschakeld. U kunt afzonderlijk opnieuw inschakelen vorige versies.
>
> U moet mogelijk opnieuw toevoegen van leden aan de toepassingsrollen als er wijzigingen zijn aangebracht aan de rollen van de toepassing in de nieuwe versie.

## <a name="using-app-versions"></a>Met behulp van app-versies

De meest recente ingeschakelde versie van de toepassing wordt standaard gebruikt in Blockchain Workbench. Als u gebruiken van een eerdere versie van een toepassing wilt, moet u eerst kiest u de versie van de toepassingspagina.

1. In de sectie Blockchain Workbench-toepassing, het selectievakje toepassing met de opdracht die u wilt gebruiken. Als eerdere versies zijn ingeschakeld, is de knop versie geschiedenis beschikbaar.
2. Selecteer de **versiegeschiedenis** knop.
3. Kies in het deelvenster van de geschiedenis van versie de versie van de toepassing door te selecteren van de koppeling in de *wijzigingsdatum* kolom.

    ![Kies een vorige versie](media/version-app/use-version.png)

    U kunt maken van nieuwe opdrachten of acties uitvoeren op de vorige versie contracten. De versie van de toepassing wordt weergegeven na de toepassingsnaam en een waarschuwing wordt weergegeven over de oudere versie.

## <a name="next-steps"></a>Volgende stappen

* [Azure Blockchain Workbench oplossen van problemen](troubleshooting.md)
