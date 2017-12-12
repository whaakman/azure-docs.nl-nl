---
title: Extensies app - Azure AD B2C | Microsoft Docs
description: Herstellen van de b2c-uitbreidingen-app
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/06/2017
ms.author: parja
ms.openlocfilehash: b28a1bb6287a0e30eda21d9a7c03abbf14b5d8d9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Extensies app

Als een Azure AD B2C-directory is gemaakt, wordt aangeroepen door een app `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` automatisch wordt gemaakt in de nieuwe map. Deze app aangeduid als de **b2c-uitbreidingen-app**, zichtbaar is in *App registraties*. Deze wordt gebruikt door de Azure AD B2C-service voor het opslaan van informatie over gebruikers en aangepaste kenmerken. Als de app wordt verwijderd, Azure AD B2C werkt niet correct en uw productieomgeving worden beïnvloed.

> [!IMPORTANT]
> Verwijder de b2c-uitbreidingen-app niet tenzij u van plan bent uw tenant onmiddellijk te verwijderen. Als de app gedurende meer dan 30 dagen verwijderd blijft, worden gebruikersgegevens permanent verloren.

## <a name="verifying-that-the-extensions-app-is-present"></a>Controleren of de extensies app aanwezig is

Controleer of de b2c-uitbreidingen-app aanwezig is:

1. Binnen uw Azure AD B2C-tenant, klikt u op **meer services** in het navigatiemenu links.
1. Zoeken en openen **App registraties**.
1. Een app die met begint zoeken **b2c-uitbreidingen-app**

## <a name="recover-the-extensions-app"></a>De app extensies herstellen

Als u de b2c-uitbreidingen-app per ongeluk hebt verwijderd, hebt u 30 dagen om het te herstellen. U kunt de app met behulp van de Graph API herstellen:

1. Blader naar [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Meld u aan bij de site als globale beheerder voor de Azure AD B2C-directory die u wilt herstellen van de verwijderde-app voor. Deze globale beheerder moet een e-mailadres lijkt op de volgende hebben: `username@{yourTenant}.onmicrosoft.com`.
1. Uitgeven van een HTTP GET voor de URL `https://graph.windows.net/myorganization/deletedApplications` = met api-versie 1.6. Deze bewerking wordt een lijst alle toepassingen die in de afgelopen 30 dagen zijn verwijderd.
1. De toepassing niet vinden in de lijst die waar de naam met 'b2c-extensie-app' en kopieer begint de `objectid` eigenschapswaarde.
1. Uitgeven van een HTTP POST voor de URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Vervang de `{OBJECTID}` deel van de URL met de `objectid` van de vorige stap. 

U moet nu kunnen [de herstelde App](#verifying-that-the-extensions-app-is-present) in de Azure portal.

> [!NOTE]
> Een toepassing kan alleen worden teruggezet in de afgelopen 30 dagen is verwijderd. Als het meer dan 30 dagen is, worden gegevens permanent verloren. Het bestand een ondersteuningsticket voor verdere ondersteuning.