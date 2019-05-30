---
title: bestand opnemen
description: bestand opnemen
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: c98a17be394887ef4e008b079467c85d4ded7e09
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393307"
---
1. Voor het maken van een nieuwe App-configuratiearchief, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com). Selecteer in de linkerbovenhoek van het deelvenster **+ een resource maken**. In de **Marketplace doorzoeken** Voer **Appconfiguratie** en selecteer Enter.

    ![Zoeken naar App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

1. Selecteer **Appconfiguratie** uit de zoekresultaten, en selecteer vervolgens **maken**.

1. Op de **Appconfiguratie** > **maken** deelvenster, voer de volgende instellingen:

    | Instelling | Voorgestelde waarde | Description |
    |---|---|---|
    | **Resourcenaam** | Wereldwijd unieke naam | Voer een unieke resourcenaam moet worden gebruikt voor de configuratie van de App store-resource. De naam moet een tekenreeks zijn van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De naam mag niet beginnen of eindigen met de `-` teken en opeenvolgende `-` tekens zijn niet geldig.  |
    | **Abonnement** | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken om App Configuration te testen. Als uw account slechts één abonnement heeft, wordt deze automatisch geselecteerd en de **abonnement** lijst wordt niet weergegeven. |
    | **Resourcegroep** | *AppConfigTestResources* | Selecteer of maak een resourcegroep voor uw configuratie van de App store. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk wilt verwijderen op hetzelfde moment door de resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](/azure/azure-resource-manager/resource-group-overview) voor meer informatie. |
    | **Locatie** | *US - centraal* | Gebruik **Locatie** om de geografische locatie op te geven waar het app-configuratiearchief wordt gehost. Voor de beste prestaties, de resource te maken in dezelfde regio als andere onderdelen van uw toepassing. |

    ![Maak een configuratie van de App store-resource](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

1. Selecteer **Maken**. De implementatie kan een paar minuten langer duren.

1. Nadat de implementatie is voltooid, selecteert u **instellingen** > **toegangssleutels**. Maak een notitie van een van beide de alleen-lezen of een primaire Lees-/ verbindingsreeks primaire sleutel. U gaat deze verbindingsreeks later gebruiken om uw toepassing om te communiceren met de configuratie van de App store die u hebt gemaakt te configureren.
