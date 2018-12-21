---
title: bestand opnemen
description: bestand opnemen
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 0f1decae5fb3ec4a07f01c5bff7475f3d73a3cbb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111875"
---
1. Als u een cache wilt maken, meldt u zich eerst aan bij [Azure Portal](https://portal.azure.com). Selecteer vervolgens **Een resource maken** > **Databases** > **Azure Cache voor Redis**.

    ![Nieuwe cache](media/redis-cache-create/redis-cache-new-cache-menu.png)

2. Configureer in **Nieuwe Azure Cache voor Redis** de instellingen voor de nieuwe cache.

    | Instelling      | Voorgestelde waarde  | Beschrijving |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS-naam** | Wereldwijd unieke naam | Naam van cache. De naam is een tekenreeks van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De cachenaam mag niet beginnen of eindigen met het teken `-`, en opeenvolgende `-`-tekens zijn niet toegestaan.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee dit nieuwe Azure Cache voor Redis-exemplaar is gemaakt. | 
    | **Resourcegroep** |  *TestResources* | Naam voor de nieuwe resourcegroep waarin de cache moet worden gemaakt. Door alle resources voor een app in een groep te plaatsen, kunt u ze samen beheren. Als u bijvoorbeeld de resourcegroep verwijdert, worden alle resources verwijderd die bij de app behoren. | 
    | **Locatie** | US - oost | Kies een [regio](https://azure.microsoft.com/regions/) vlakbij de andere services die gaan gebruikmaken van de cache. |
    | **[Prijscategorie](https://azure.microsoft.com/pricing/details/cache/)** |  Basic C0 (250 MB Cache) |  De prijscategorie bepaalt de grootte, prestaties en functies die beschikbaar zijn voor de cache. Zie het [Azure Cache voor Redis-overzicht](../articles/azure-cache-for-redis/cache-overview.md) voor meer informatie. |
    | **Vastmaken aan dashboard** |  Geselecteerd | Maak de nieuwe cache vast aan het dashboard, zodat u deze gemakkelijk kunt vinden. |

    ![Cache maken](media/redis-cache-create/redis-cache-cache-create.png) 

3. Nadat de instellingen van de cache zijn geconfigureerd, klikt u op **Maken**. 

    Het kan enkele minuten duren voordat de cache is gemaakt. Als u de status wilt zien, kunt u de voortgang bekijken op het dashboard. Nadat de cache is gemaakt, wordt de status **Wordt uitgevoerd** weergegeven en is de cache klaar voor gebruik.

    ![Cache gemaakt](media/redis-cache-create/redis-cache-cache-created.png)

