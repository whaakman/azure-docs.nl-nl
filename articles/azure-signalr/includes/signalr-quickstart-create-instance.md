---
title: bestand opnemen
description: bestand opnemen
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 15eded28e38279ea01bf019566d4fda5e7ac6c3e
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57588116"
---
## <a name="create-an-azure-signalr-service-instance"></a>Een exemplaar van de Azure SignalR Service maken

Uw toepassing maakt verbinding met een SignalR-Service-exemplaar in Azure.

1. Selecteer de knop Nieuw in de linkerbovenhoek van Azure Portal. Typ in het scherm Nieuw *SignalR Service* in het zoekvak in en druk op Enter.

    ![Zoek naar SignalR Service](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Selecteer **SignalR Service** in de zoekresultaten en selecteer **Maken**.

1. Voer de volgende instellingen in.

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Resourcenaam** | Wereldwijd unieke naam | Naam ter identificatie van uw nieuwe SignalR Service-exemplaar. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarin dit nieuwe SignalR Service-exemplaar is gemaakt. | 
    | **[Resourcegroep](../../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin het SignalR Service-exemplaar moet worden gemaakt. | 
    | **Locatie** | US - west | Kies een [regio](https://azure.microsoft.com/regions/) bij u in de buurt. |
    | **Prijscategorie** | Gratis | Probeer Azure SignalR Service gratis uit. |
    | **Aantal eenheden** |  Niet van toepassing | Het aantal eenheden geeft aan hoeveel verbindingen uw SignalR Service-exemplaar kan accepteren. Dit kan alleen worden geconfigureerd in de Standard-laag. |

    ![SignalR Service maken](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. Selecteer **Maken** om te beginnen met het implementeren van het SignalR-Service-exemplaar.

1. Nadat het exemplaar is geïmplementeerd, opent u het in de portal en zoek de pagina instellingen. Wijzig de modus van Service-instelling in *zonder*.

    ![SignalR-servicemodus](../media/signalr-concept-azure-functions/signalr-service-mode.png)