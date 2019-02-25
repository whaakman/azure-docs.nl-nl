---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 97387e24d5b55c1438a69da1a1fd0a9bc1720e47
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56751284"
---
1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**.

1. In de **publiceren** dialoogvenster, selecteer **Microsoft Azure App Service**, kiest u **nieuw**, en selecteer vervolgens **publiceren**.

   ![Kies publiceren doel](./media/webjobs-publish-netcore/pick-publish-target.png)

1. In de **Create App Service** dialoogvenster, gebruiken de hostinginstellingen zoals in de tabel onder de afbeelding opgegeven:

    ![Het dialoogvenster App Service maken](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Instelling      | Voorgestelde waarde  | Description                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app uniek wordt aangeduid. |
    | **Abonnement** | Kies uw abonnement | Het te gebruiken Azure-abonnement. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Naam van de resourcegroep waarin uw functie-app moet worden gemaakt. Kies **Nieuw** om een nieuwe resourcegroep te maken.|
    | **[Hostingabonnement](../articles/app-service/overview-hosting-plans.md)** | App Service-abonnement | Een [App Service-plan](../articles/app-service/overview-hosting-plans.md) geeft de locatie, de grootte en de functies van de webserverfarm aan die als host fungeert voor uw app. U kunt geld besparen bij het hosten van meerdere apps door de web-apps te configureren voor het delen van één App Service-plan. App Service-abonnementen definieert de regio, instantiegrootte, schaal en SKU (Free, Shared, Basic, Standard of Premium). Kies **nieuw** te maken van een nieuwe App Service-plan. |

1. Klik op **maken** te maken van een webtaak en gerelateerde resources in Azure met deze instellingen en implementeer de projectcode van uw.