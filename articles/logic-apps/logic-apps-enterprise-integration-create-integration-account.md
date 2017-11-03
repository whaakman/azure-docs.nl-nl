---
title: Maken, koppelen, verwijderen of verplaatsen van een account integratie in Azure logic apps | Microsoft Docs
description: Het maken van een account integratie en koppel deze aan uw logische apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 716e7b5bab8725dea0fd2b760d0e46e8e892c5b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-an-integration-account"></a>Wat is een integratie-account?

Een account integratie kunt enterprise integratie-apps voor het beheren van artefacten, met inbegrip van schema's, kaarten, certificaten, partners en -overeenkomsten. Integratie van apps die hebt u gebruikmaakt van een integratie-account voor toegang tot deze schema's, kaarten, certificaten, enzovoort.

## <a name="create-an-integration-account"></a>Een integratie-account maken

1.  Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal"). Selecteer in het menu links **meer services**.

    !['Meer services' selecteren](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Typ in het zoekvak '-integratie"voor uw filter. Selecteer in de lijst met resultaten **Integratieaccounts**.

    ![Filteren op "-integratie", selecteert u 'Integratieaccounts'](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Kies aan de bovenkant van de pagina **toevoegen**.

    ![Kies toevoegen](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Naam van uw account integratie en selecteer de Azure-abonnement dat u wilt gebruiken. U kunt een nieuwe maken **resourcegroep** of Selecteer een bestaande resourcegroep. Selecteer vervolgens een **locatie** voor het hosten van uw account integratie en een **prijscategorie**. 

    Als u klaar bent, kiest u **maken**.

    ![Informatie opgeven voor uw account integratie](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure richt uw integratie-account in de geselecteerde locatie, die moet worden voltooid binnen 1 minuut.

5. Vernieuw de pagina. U ziet uw nieuwe integratie account weergegeven.

    ![Uw nieuwe account voor de integratie wordt weergegeven](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Koppel vervolgens de integratie-account dat u aan uw logische app hebt gemaakt. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Een integratie-account koppelen aan een logische app

Uw logische apps om toegang te verlenen aan maps, schema's, overeenkomsten en andere artefacten in uw account integratie, door de integratie-account te koppelen aan uw logische app.

### <a name="prerequisites"></a>Vereisten

* Een account voor de integratie
* Een logische app

> [!NOTE] 
> Zorg ervoor dat uw account en logica app voor integratie zijn in de *dezelfde Azure-locatie* voordat u begint.


1. In de Azure portal, selecteer uw logische app en uw logische app locatie controleren.

    ![Selecteer uw logische app, locatie controleren](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Onder **instellingen**, selecteer **integratie Account**.

    ![Selecteer 'Integratie Account'](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Van de **selecteert u een account integratie** , selecteert u de integratie-account dat u wilt koppelen aan uw logische app. Kies voor het voltooien van de koppeling **opslaan**.

    ![Selecteer uw integratie-account](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Krijgt u een melding dat uw integratie account is gekoppeld aan uw logische app en alle artefacten in uw account integratie zijn nu beschikbaar voor uw logische app weergeeft.

    ![Uw logische app is gekoppeld aan uw account integratie](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Nu uw integratie-account is gekoppeld aan uw logische app, kunt u de B2B-connectors gebruiken in logic apps. Sommige algemene B2B-connectors zijn XML-validatie en plat bestand coderen/decoderen.  

## <a name="delete-your-integration-account"></a>Uw account integratie verwijderen

1. Selecteer **meer services**.

    !['Meer services' selecteren](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Typ in het zoekvak '-integratie"voor uw filter. Selecteer in de lijst met resultaten **Integratieaccounts**.

    ![Filteren op "-integratie", selecteert u 'Integratieaccounts'](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Selecteer de integratie-account dat u wilt verwijderen.

    ![Selecteer integratie-account verwijderen](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. Kies in het menu **verwijderen**.

    ![Kies 'Verwijderen'](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. Bevestig uw keuze om de integratie-account te verwijderen.

## <a name="move-your-integration-account"></a>Uw account integratie verplaatsen

Volg deze stappen om een account integratie naar een andere Azure-abonnement of resourcegroep groep verplaatst.

> [!IMPORTANT]
> Alle scripts voor het gebruik van de nieuwe resource-id nadat u een account integratie verplaatst, moet u bijwerken.

1. Selecteer **meer services**.

    !['Meer services' selecteren](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Typ in het zoekvak '-integratie"voor uw filter. Selecteer in de lijst met resultaten **Integratieaccounts**.

    ![Filteren op "-integratie", selecteert u 'Integratieaccounts'](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Selecteer de integratie-account dat u wilt verplaatsen. Onder **instellingen**, kies **eigenschappen**.

    ![Selecteer de account integratie te verplaatsen. Kies onder instellingen eigenschappen](./media/logic-apps-enterprise-integration-accounts/move.png)

5. De resourcegroep of een Azure-abonnement dat is gekoppeld aan uw account integratie wijzigen.

    ![Kies wijziging resourcegroep of abonnement wijzigen](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  

