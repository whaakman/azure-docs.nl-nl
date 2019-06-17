---
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 05/02/2019
ms.author: danlep
ms.openlocfilehash: 40cc1856a5e943ca5596e7d11712febadd30e3ec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133118"
---
## <a name="prerequisites"></a>Vereisten

### <a name="get-sample-code"></a>Voorbeeldcode ophalen

In deze zelfstudie wordt ervan uitgegaan dat u de stappen in de [vorige zelfstudie](../articles/container-registry/container-registry-tutorial-quick-task.md) hebt voltooid en dat u de voorbeeldopslagplaats hebt gesplitst en gekloond. Als u dit nog niet hebt gedaan, voert u de stappen in de sectie [Vereisten](../articles/container-registry/container-registry-tutorial-quick-task.md#prerequisites) van de vorige zelfstudie uit voordat u doorgaat.

### <a name="container-registry"></a>Containerregister

U moet een Azure-containerregister in uw Azure-abonnement hebben om deze zelfstudie te voltooien. Als u een register nodig hebt, raadpleegt u de [vorige zelfstudie](../articles/container-registry/container-registry-tutorial-quick-task.md) of [Quickstart: een containerregister maken met de Azure-CLI](../articles/container-registry/container-registry-get-started-azure-cli.md).

## <a name="create-a-github-personal-access-token"></a>Een persoonlijk toegangstoken van GitHub maken

Voor het activeren van een taak op een wijziging aan een Git-opslagplaats, moet de ACR-taken een persoonlijk toegangstoken (PAT) voor toegang tot de opslagplaats. Als u nog geen een PAT, volgt u deze stappen voor het genereren van een in GitHub:

1. Navigeer naar de pagina PAT maken op GitHub op https://github.com/settings/tokens/new
1. Geef een korte **beschrijving** voor het token op, bijvoorbeeld 'Demo ACR Tasks'
1. Selecteer bereiken voor ACR voor toegang tot de opslagplaats. Toegang krijgen tot een openbare opslagplaats, zoals in deze zelfstudie onder **opslagplaats**, inschakelen **opslagplaats: status** en **public_repo**

   ![Schermafbeelding van de pagina Persoonlijk toegangstoken genereren in GitHub][build-task-01-new-token]

   > [!NOTE]
   > Voor het genereren van een PAT voor toegang tot een *persoonlijke* opslagplaats, selecteert u het bereik voor volledige **opslagplaats** besturingselement.

1. Selecteer de knop **Token genereren** (u wordt mogelijk gevraagd om uw wachtwoord te bevestigen)
1. Kopieer en bewaar het gegenereerde token in een **beveiligde locatie** (u gebruikt dit token bij het definiëren van een taak in de volgende sectie)

   ![Schermafbeelding van het gegeneerde persoonlijke toegangstoken in GitHub][build-task-02-generated-token]

<!-- Images -->
[build-task-01-new-token]: ./media/container-registry-task-tutorial-prereq/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-task-tutorial-prereq/build-task-02-generated-token.png
