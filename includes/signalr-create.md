---
title: bestand opnemen
description: bestand opnemen
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 28d003e123069c47d87d81570b4a5b69b3b9d64b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176405"
---
1. Voor het maken van een Azure SignalR Service-resource, eerst aanmelden bij de [Azure-portal](https://portal.azure.com). Selecteer in de linkerbovenhoek-zijde van de pagina, **+ een resource maken**. In de **Marketplace doorzoeken** tekst Voer **SignalR Service**.

2. Selecteer **SignalR Service** in de resultaten en selecteer **maken**.

3. Op de nieuwe **SignalR** instellingenpagina van de volgende instellingen voor uw nieuwe SignalR-resource toevoegen:

    | Name | Aanbevolen waarde | Description |
    | ---- | ----------------- | ----------- |
    | Resourcenaam | *testsignalr* | Voer een unieke resourcenaam in voor de SignalR-resource. De naam moet een tekenreeks van 1 tot 63 tekens en alleen cijfers, letters en afbreekstreepjes bevatten (`-`) teken. De naam mag niet beginnen of eindigen met het koppelteken, en opeenvolgende afbreekstreepjes bevatten, zijn niet geldig.|
    | Abonnement | Kies uw abonnement |  Selecteer het Azure-abonnement dat u wilt gebruiken om SignalR te testen. Als uw account slechts één abonnement heeft, wordt deze automatisch geselecteerd en de **abonnement** vervolgkeuzelijst wordt niet weergegeven.|
    | Resourcegroep | Maak een resourcegroep met de naam *SignalRTestResources*| Selecteer of maak een resourcegroep voor uw SignalR-resource. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk wilt verwijderen op hetzelfde moment door de resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../articles/azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    | Locatie | *US - oost* | Gebruik **Locatie** om de geografische locatie op te geven waar SignalR-resource wordt gehost. Voor de beste prestaties wordt u aangeraden om de resource te maken in dezelfde regio als de andere onderdelen van uw toepassing. |
    | Prijscategorie | *Gratis* | Op dit moment **gratis** en **Standard** opties zijn beschikbaar. |
    | Vastmaken aan dashboard | ✔ | Schakel dit in dat de resource die is vastgemaakt aan uw dashboard zodat u gemakkelijker te vinden. |

4. Selecteer **Maken**. De implementatie kan enkele minuten duren.

5. Nadat de implementatie voltooid is, selecteert u **sleutels** onder **instellingen**. Kopieer de verbindingsreeks voor de primaire sleutel. U gebruikt deze tekenreeks later uw app voor het gebruik van de bron voor Azure SignalR Service configureren.

    Deze verbindingsreeks heeft de volgende vorm:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
