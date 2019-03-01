---
title: Maken en beheren van integratieaccounts voor oplossingen voor B2B - Azure Logic Apps | Microsoft Docs
description: Maken, koppelen, verplaatsen en verwijderen van integratieaccounts voor bedrijfsintegratie en B2B-oplossingen met Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/30/2018
ms.author: estfan
ms.openlocfilehash: 43ecdafac4f0a5cdc9e619537cdbe2a42ff7fe1b
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191915"
---
# <a name="create-and-manage-integration-accounts-for-b2b-solutions-with-logic-apps"></a>Maken en beheren van integratieaccounts voor B2B-oplossingen met logic apps

Voordat u kunt bouwen [bedrijfsintegratie en B2B-oplossingen](../logic-apps/logic-apps-enterprise-integration-overview.md) met [Azure Logic Apps](../logic-apps/logic-apps-overview.md), u moet eerst een integratieaccount, dit is waar u maken, opslaan en beheren van B2B-artefacten, zoals hebben handel partners, overeenkomsten, kaarten, schema's, certificaten, enzovoort. Voordat uw logische app kunt met de artefacten in uw integratie-account werken en de logische Apps B2B-connectors, zoals XML-validatie gebruiken, moet u [uw integratieaccount koppelen](#link-account) aan uw logische app. Om ze te koppelen, beide uw integratie-account en logica voor de app moet de *dezelfde* Azure-locatie of regio.

In dit artikel laat zien hoe u deze taken uit te voeren:

* Uw integratieaccount maken.
* Uw integratieaccount koppelen aan een logische app.
* Uw integratie-account verplaatsen naar een andere Azure-resourcegroep of abonnement.
* Uw integratieaccount verwijderen.

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Gebruik de referenties van uw Azure-account om u aan melden bij het <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

## <a name="create-integration-account"></a>integratie-account maken

1. Selecteer in het hoofdmenu van Azure **alle services**. Typ in het zoekvak 'integratieaccounts' als filter en selecteer **integratieaccounts**.

   ![Integratieaccounts zoeken](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Onder **integratieaccounts**, kiest u **toevoegen**.

   ![Kies "Toevoegen" integratie-account maken](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

3. Bevatten informatie over de integratie-account: 

   ![Geef details op voor uw integratie-account](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Eigenschap | Vereist | Voorbeeldwaarde | Description | 
   |----------|----------|---------------|-------------|
   | Name | Ja | test-integration-account | De naam voor uw integratie-account. Gebruik de opgegeven naam voor dit voorbeeld. | 
   | Abonnement | Ja | <*Azure-subscription-name*> | De naam van het Azure-abonnement gebruiken | 
   | Resourcegroep | Ja | test-integration-account-rg | De naam voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) gebruikt om verwante resources te organiseren. In dit voorbeeld maakt u een nieuwe resourcegroep met de opgegeven naam. | 
   | Prijscategorie | Ja | Gratis | De prijscategorie die u wilt gebruiken. Selecteer voor dit voorbeeld **gratis**, maar voor meer informatie, Zie [Logic Apps-limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md) en [prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). | 
   | Locatie | Ja | US - west | De regio waar de gegevens van uw integratie-account wordt opgeslagen. Selecteer dezelfde locatie als uw logische app of een logische app maken in dezelfde locatie als uw integratie-account. | 
   | Log Analytics-werkruimte | Nee | Uit | Behoud de instelling **Uit** voor het vastleggen van diagnostische gegevens. | 
   ||||| 

4. Als u klaar bent, selecteert u **Vastmaken aan dashboard** en kiest u **Maken**.

   Nadat Azure uw integratie-account heeft geïmplementeerd op de geselecteerde locatie, die meestal is voltooid binnen één minuut, wordt uw integratie-account in Azure geopend.

   ![Azure wordt geopend uw integratie-account](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

Voordat uw logische app uw integratieaccount gebruiken kunt, moet u nu de integratie-account koppelen aan uw logische app.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Koppelen aan de logische app

Uw logic apps om toegang te verlenen aan een integratieaccount waarin uw B2B-artefacten, zoals trading partners, overeenkomsten, toewijzingen en schema's, moet u uw integratie-account koppelen aan uw logische app. 

> [!NOTE]
> Uw integratie-account en de logische app moet zich in dezelfde regio bevinden.

1. In de Azure-portal, zoeken en openen van uw logische app.

2. In het menu van uw logische app onder **instellingen**, selecteer **Werkstroominstellingen**. In de **selecteert u een integratieaccount** , selecteert u de integratie-account te koppelen aan uw logische app.

   ![Selecteer uw integratieaccount](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-2.png)

3. Kies voor het voltooien van koppelen, **opslaan**.

   ![Selecteer uw integratieaccount](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-3.png)

   Als uw integratie-account is gekoppeld, wordt in Azure een bevestigingsbericht weergegeven. 

   ![Azure geslaagd koppeling wordt bevestigd](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-5.png)

Uw logische app kan nu alle de artefacten in uw integratieaccount plus de B2B-connectors, zoals XML-validatie en platte bestanden coderen of decoderen gebruiken.  

## <a name="unlink-from-logic-app"></a>Ontkoppelen van de logische app

Als u wilt uw logische app koppelen aan een andere integratie-account of een integratieaccount niet meer gebruiken met uw logische app, verwijdert u de koppeling via Azure Resource Explorer.

1. Ga in uw browser naar <a href="https://resources.azure.com" target="_blank">Azure Resource Explorer (https://resources.azure.com)</a>. Zorg ervoor dat u bent aangemeld met de dezelfde Azure-referenties.

   ![Azure Resource Explorer](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

2. In het zoekvak, typ de naam van uw logische app, en zoek en selecteer uw logische app.

   ![Zoek en selecteer de logische app](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

3. Kies op de titelbalk explorer **lezen/schrijven**.

   ![' Lezen/schrijven'-modus inschakelen](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-read-write-mode.png)

4. Op de **gegevens** tabblad **bewerken**.

   ![Kies op het tabblad 'Data', 'Bewerken'](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-edit.png)

5. Zoek in de editor de `integrationAccount` eigenschap voor de integratie-account en het verwijderen van deze eigenschap, die deze indeling heeft:

   ```json
   "integrationAccount": {
      "name": "<integration-account-name>",
      "id": "<integration-account-resource-ID>",
      "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Bijvoorbeeld:

   ![Definitie van de eigenschap "integrationAccount" zoeken](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

6. Op de **gegevens** tabblad **plaatsen** uw wijzigingen op te slaan. 

   ![Kies 'Plaats' om wijzigingen hebt opgeslagen](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

7. In de Azure-portal onder uw logische app **Werkstroominstellingen**, controleert u of de **integratieaccount** eigenschap is nu leeg.

   ![Controleer dat integratie-account niet is gekoppeld](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Integratie-account verplaatsen

U kunt uw integratie-account verplaatsen naar een andere Azure-abonnement of resourcegroep groep.

1. Selecteer in het hoofdmenu van Azure **alle services**. Typ in het zoekvak 'integratieaccounts' als filter en selecteer **integratieaccounts**.

   ![Uw integratie-account zoeken](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Onder **integratieaccounts**, selecteer het integratieaccount dat u wilt verplaatsen. Op uw integratie-account in het menu onder **instellingen**, kiest u **eigenschappen**.

   ![Kies onder 'Instellingen', 'Eigenschappen'](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-properties.png)

3. De Azure-resourcegroep of een abonnement voor uw integratie-account wijzigen.

   ![Kies 'Resourcegroep wijzigen' of "Abonnement wijzigen"](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

4. Wanneer u klaar bent, zorg ervoor dat u alle scripts met de nieuwe resource-id's voor uw artefacten bijwerken.  

## <a name="delete-integration-account"></a>Integratieaccount verwijderen

1. Selecteer in het hoofdmenu van Azure **alle services**. Typ in het zoekvak 'integratieaccounts' als filter en selecteer **integratieaccounts**.

   ![Uw integratie-account zoeken](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Onder **integratieaccounts**, selecteer het integratieaccount dat u wilt verwijderen. Kies in het menu van de account integratie **overzicht**, en kies vervolgens **verwijderen**. 

   ![Integratie-account selecteren. Kies op de pagina 'Overzicht', 'Verwijderen'](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

3. Om te bevestigen dat u wilt verwijderen van uw integratie-account, kies **Ja**.

   ![Als u wilt verwijderen te bevestigen, kiest u 'Ja'](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Volgende stappen

* [Handelspartners maken](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Overeenkomsten maken](../logic-apps/logic-apps-enterprise-integration-agreements.md)
