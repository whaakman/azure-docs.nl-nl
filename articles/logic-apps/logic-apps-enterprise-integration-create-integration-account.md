---
title: Maken, koppelen, verwijderen of verplaatsen van een account integratie in Azure logic apps | Microsoft Docs
description: Het maken van een account integratie en koppel deze aan uw logische apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; divswa
ms.openlocfilehash: fb1d0ceb26c5ed792f22051e2af10a7572200bdc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="what-is-an-integration-account"></a>Wat is een integratie-account?

Een account integratie biedt een manier voor uw onderneming integratie-apps, speciaal logic apps, raadplegen en beheren van B2B-artefacten, bijvoorbeeld handel partners, overeenkomsten, kaarten, schema's, certificaten, enzovoort. Dit om toegang te bieden, uw integratie-account koppelen aan uw logische app na om ervoor te zorgen dat beide de integratie-account en logica voor de app beschikt over de *dezelfde Azure-locatie*.

## <a name="create-an-integration-account"></a>Een integratieaccount maken

1. Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal"). 

2. Selecteer in het Azure hoofdmenu **alle services**. Voer in het zoekvak "-integratie" en selecteer vervolgens **integratieaccounts**.

   ![integratie-account maken](./media/logic-apps-enterprise-integration-accounts/account-1.png)

3. Kies aan de bovenkant van de pagina **toevoegen**.

   ![Kies toevoegen](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Naam van uw account integratie en selecteer de Azure-abonnement dat u wilt gebruiken. U kunt een nieuwe maken **resourcegroep** of Selecteer een bestaande resourcegroep. Selecteer een **locatie** voor het hosten van uw account integratie en een **prijscategorie**. Als u klaar bent, kiest u **maken**.

   ![Informatie opgeven voor uw account integratie](./media/logic-apps-enterprise-integration-accounts/account-4.png)

   Azure richt uw integratie-account in de geselecteerde locatie, die moet worden voltooid binnen één minuut.

5. Vernieuw de pagina. U ziet uw nieuwe integratie account weergegeven.

   ![Uw nieuwe account voor de integratie wordt weergegeven](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Koppel vervolgens de integratie-account dat u aan uw logische app hebt gemaakt. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Een integratie-account koppelen aan een logische app

Uw logische apps om toegang te verlenen tot B2B-artefacten, zoals handelspartners, overeenkomsten, maps en schema's in uw account integratie door de integratie-account te koppelen aan uw logische app. 

1. In de Azure portal, selecteer uw logische app en uw logische app locatie controleren.

   ![Selecteer uw logische app, locatie controleren](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Onder **instellingen**, selecteer **integratie Account**.

   ![Selecteer 'Integratie Account'](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Van de **selecteert u een account integratie** , selecteert u de integratie-account dat u wilt koppelen aan uw logische app. Kies voor het voltooien van de koppeling **opslaan**.

   ![Selecteer uw integratie-account](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

   Krijgt u een melding dat uw integratie account is gekoppeld aan uw logische app en alle artefacten in uw account integratie zijn nu beschikbaar voor uw logische app weergeeft.

   ![Uw logische app is gekoppeld aan uw account integratie](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Nu uw integratie-account is gekoppeld aan uw logische app, kunt u de B2B-connectors gebruiken in uw logische app. Sommige algemene B2B-connectors zijn XML-validatie en plat bestand coderen/decoderen.  

## <a name="delete-your-integration-account"></a>Uw account integratie verwijderen

1. Selecteer in het Azure hoofdmenu **alle services**. Voer in het zoekvak "-integratie" en selecteer vervolgens **integratieaccounts**.

   ![Uw account integratie vinden](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Selecteer de integratie-account dat u wilt verwijderen.

    ![Selecteer integratie-account verwijderen](./media/logic-apps-enterprise-integration-accounts/account-5.png)

3. Kies in het menu **verwijderen**.

    ![Kies 'Verwijderen'](./media/logic-apps-enterprise-integration-accounts/delete.png)

4. Bevestig uw keuze om de integratie-account te verwijderen.

## <a name="move-your-integration-account"></a>Uw account integratie verplaatsen

Volg deze stappen om een account integratie naar een andere Azure-abonnement of resourcegroep groep verplaatst. Nadat u het account integratie verplaatst, zorg er dan voor dat alle scripts voor het gebruik van de nieuwe resource-id's bij te werken.

1. Selecteer in het Azure hoofdmenu **alle services**. Voer in het zoekvak "-integratie" en selecteer vervolgens **integratieaccounts**.

   ![Uw account integratie vinden](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Selecteer de integratie-account dat u wilt verplaatsen. Onder **instellingen**, kies **eigenschappen**.

   ![Selecteer de account integratie te verplaatsen. Kies onder instellingen eigenschappen](./media/logic-apps-enterprise-integration-accounts/move.png)

3. De resourcegroep of een Azure-abonnement dat is gekoppeld aan uw account integratie wijzigen.

   ![Kies wijziging resourcegroep of abonnement wijzigen](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  

