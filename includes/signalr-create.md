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
ms.openlocfilehash: 57407606214d8d3a305476cfbfdabca9eee937e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60690299"
---
1. Als u een nieuwe Azure SignalR Service-resource wilt maken, meldt u zich eerst aan bij [Azure Portal](https://portal.azure.com). Selecteer **+ Een resource maken** linksboven van de pagina. Typ in het tekstvak **Marketplace doorzoeken** de tekst **SignalR Service** en druk op **Enter**.

2. Klik op **SignalR Service** in de resultaten en klik op **Maken**.

3. Op de nieuwe pagina met **SignalR**-instellingen voegt u de volgende instellingen toe voor uw nieuwe SignalR-resource:

    | Naam | Aanbevolen waarde | Beschrijving |
    | ---- | ----------------- | ----------- |
    | Resourcenaam | *testsignalr* | Voer een unieke resourcenaam in voor de SignalR-resource. De naam moet een tekenreeks zijn van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De naam mag niet beginnen of eindigen met het teken `-`, en opeenvolgende `-`-tekens zijn niet toegestaan.|
    | Abonnement | Kies uw abonnement |  Selecteer het Azure-abonnement dat u wilt gebruiken om SignalR te testen. Als uw account maar één abonnement heeft, wordt dit automatisch geselecteerd en wordt de vervolgkeuzelijst **Abonnement** niet weergegeven.|
    | Resourcegroep | Maak een nieuwe resourcegroep met de naam *SignalRTestResources*| Selecteer of maak een resourcegroep voor uw SignalR-resource. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk op een bepaald moment wilt verwijderen door resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../articles/azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    | Locatie | *US - oost* | Gebruik **Locatie** om de geografische locatie op te geven waar SignalR-resource wordt gehost. Voor de beste prestaties wordt u aangeraden om de resource te maken in dezelfde regio als de andere onderdelen van uw toepassing. |
    | Prijscategorie | *Gratis* | Er zijn momenteel **gratis** en **Standard** opties die beschikbaar. |
    | Vastmaken aan dashboard | ✔ | Schakel dit selectievakje in als u wilt dat de resource wordt vastgemaakt aan uw dashboard, zodat u deze eenvoudiger kunt vinden. |

4. Klik op **Create**. De implementatie kan enkele minuten duren.

5. Wanneer de implementatie is voltooid, klikt u onder **INSTELLINGEN** op **Sleutels**. Kopieer uw verbindingsreeks van de primaire sleutel. U gebruikt deze waarde later om uw app te configureren voor het gebruik van de Azure SignalR Service-resource.

    Deze verbindingsreeks heeft de volgende vorm:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
