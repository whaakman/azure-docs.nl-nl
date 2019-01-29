---
title: App met extensies in Azure Active Directory B2C | Microsoft Docs
description: Herstellen van de b2c-extensions-app.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 9/06/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1c3e96c10af9085854840029867eaf238e5a593d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55172864"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: App met extensies

Wanneer u een Azure AD B2C-adreslijst is gemaakt, een app genoemd `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` wordt automatisch gemaakt in de nieuwe map. Deze app, aangeduid als de **b2c-extensions-app**, zichtbaar is in *App-registraties*. Deze wordt gebruikt door de Azure AD B2C-service voor het opslaan van informatie over gebruikers en aangepaste kenmerken. Als de app wordt verwijderd, Azure AD B2C werkt niet correct en uw productie-omgeving wordt beïnvloed.

> [!IMPORTANT]
> Verwijder de b2c-extensions-app niet, tenzij u van plan bent uw tenant onmiddellijk te verwijderen. Als de app gedurende meer dan 30 dagen verwijderd blijft, worden gegevens van gebruiker permanent verbroken.

## <a name="verifying-that-the-extensions-app-is-present"></a>Controleren of de app met extensies aanwezig is

Om te controleren of de b2c-extensions-app aanwezig:

1. Binnen uw Azure AD B2C-tenant, klikt u op **alle services** in het navigatiemenu aan.
1. Zoek en open **App-registraties**.
1. Een app die met begint zoeken **b2c-extensions-app**

## <a name="recover-the-extensions-app"></a>Herstellen van de app met extensies

Als u de b2c-extensions-app per ongeluk hebt verwijderd, hebt u 30 dagen om het te herstellen. U kunt de met de Graph API-app herstellen:

1. Blader naar [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/).
1. Meld u aan bij de site aan als globale beheerder voor de Azure AD B2C-map die u wilt herstellen van de app verwijderd voor. Deze globale beheerder moet een e-mailadres die vergelijkbaar is met het volgende hebben: `username@{yourTenant}.onmicrosoft.com`.
1. Uitgeven van een HTTP GET voor de URL `https://graph.windows.net/myorganization/deletedApplications` = met api-versie 1.6. Met deze bewerking wordt een lijst alle toepassingen die in de afgelopen 30 dagen zijn verwijderd.
1. De toepassing niet vinden in de lijst die waar de naam met 'b2c-extensie-app' en kopieer begint de `objectid` eigenschapswaarde.
1. Uitgeven van een HTTP POST voor de URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Vervang de `{OBJECTID}` deel van de URL met de `objectid` uit de vorige stap. 

U zou nu moeten kunnen [de herstelde App](#verifying-that-the-extensions-app-is-present) in Azure portal.

> [!NOTE]
> Een toepassing kan alleen worden hersteld als deze is verwijderd in de afgelopen 30 dagen. Als dit meer dan 30 dagen is, gaan gegevens verloren definitief. Meer hulp nodig hebt, kunt u een ondersteuningsticket indienen.
