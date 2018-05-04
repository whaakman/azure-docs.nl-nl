---
title: Maken en beheren van integratieaccounts voor oplossingen voor B2B - Azure Logic Apps | Microsoft Docs
description: Maken, koppelen, verplaatsen en verwijderen van integratieaccounts voor enterprise integration en B2B-oplossingen met Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: SyntaxC4
editor: ''
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/30/2018
ms.author: ecfan; LADocs
ms.openlocfilehash: 8e31a84d4508075dcb7a1d7ad8a64fa8e142681d
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="create-and-manage-integration-accounts-for-b2b-solutions-with-logic-apps"></a>Integratie van rekeningen voor B2B-oplossingen met logic apps maken en beheren

Voordat u kunt bouwen [enterprise integration en B2B-oplossingen](../logic-apps/logic-apps-enterprise-integration-overview.md) met [Azure Logic Apps](../logic-apps/logic-apps-overview.md), u moet eerst een integratie-account is waar u maken, opslaan en beheren van B2B-artefacten, zoals hebben handel partners, overeenkomsten, kaarten, schema's, certificaten, enzovoort. Voordat uw logische app kunt met de artefacten in uw account integratie werken en de connectors Logic Apps B2B zoals XML-validatie gebruiken, moet u [Koppel uw account integratie](#link-account) aan uw logische app. Als u wilt koppelen, zowel uw integratie-account en logica voor de app moet hebben de *dezelfde* Azure-locatie of regio.

In dit artikel wordt beschreven hoe u deze taken uitvoeren:

* Uw account integratie gemaakt.
* Uw account integratie aan een logische app koppelen.
* Uw account integratie verplaatsen naar een andere Azure resourcegroep of abonnement.
* Uw account integratie verwijderen.

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Gebruik de referenties van uw Azure-account om u aan melden bij het <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

## <a name="create-integration-account"></a>integratie-account maken

1. Selecteer in het Azure hoofdmenu **alle services**. Voer in het zoekvak 'integratieaccounts' als filter en selecteer **integratieaccounts**.

   ![Integratieaccounts vinden](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Onder **integratieaccounts**, kies **toevoegen**.

   ![Kies "Toevoegen" integratie-account maken](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

3. Bevatten informatie over uw account integratie: 

   ![Informatie opgeven voor uw account integratie](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Eigenschap | Vereist | Voorbeeldwaarde | Beschrijving | 
   |----------|----------|---------------|-------------|
   | Naam | Ja | Test-integratie-account | De naam voor uw account integratie. Gebruik de opgegeven naam voor dit voorbeeld. | 
   | Abonnement | Ja | <*Naam van een Azure-abonnement*> | De naam voor het Azure-abonnement gebruiken | 
   | Resourcegroep | Ja | Test-integratie-account-rg | De naam voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) gebruikt om verwante resources te organiseren. Bijvoorbeeld, een nieuwe resourcegroep te maken met de opgegeven naam. | 
   | Prijscategorie | Ja | Gratis | De prijscategorie die u wilt gebruiken. Selecteer voor dit voorbeeld **vrije**. | 
   | Locatie | Ja | VS - west | De regio waar uw integratieaccountgegevens wordt opgeslagen. Selecteer dezelfde locatie als uw logische app, of een logische app maken in dezelfde locatie als uw integratie-account. In dit voorbeeld | 
   | Log Analytics | Nee | Uit | Behoud de instelling **Uit** voor het vastleggen van diagnostische gegevens. | 
   ||||| 

4. Wanneer u klaar bent, selecteert u **vastmaken aan dashboard**, en kies **maken**.

   Nadat Azure heeft uw account integratie geïmplementeerd naar de geselecteerde locatie, meestal binnen een minuut wordt voltooid, wordt uw account integratie geopend in Azure.

   ![Azure Hiermee opent u uw integratie-account](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

Voordat u uw logische app uw integratie-account kunt gebruiken, moet u nu de integratie-account koppelen aan uw logische app.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Koppelen aan de logische app

Uw logische apps om toegang te verlenen aan een account voor integratie met uw B2B-artefacten, zoals handelspartners, overeenkomsten, maps en schema's, moet u uw integratie-account koppelen aan uw logische app. 

> [!NOTE]
> Uw account en logica app voor integratie moet zich in dezelfde regio.

1. Zoek en open uw logische app in de Azure portal.

2. Uw logische app menu onder **instellingen**, selecteer **Werkstroominstellingen**. In de **selecteert u een account integratie** , selecteert u de integratie-account koppelen aan uw logische app.

   ![Selecteer uw integratie-account](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-2.png)

3. Kies voor het voltooien van de koppeling **opslaan**.

   ![Selecteer uw integratie-account](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-3.png)

   Als uw integratie-account is gekoppeld, wordt in Azure een bevestigingsbericht weergegeven. 

   ![Azure bevestigt geslaagde koppeling](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-5.png)

Nu kan uw logische app en alle artefacten in uw account integratie plus de connectors B2B zoals XML-validatie en plat bestand coderen of decoderen gebruiken.  

## <a name="unlink-from-logic-app"></a>Ontkoppelen van de logische app

Als u uw logische app koppelen aan een andere integratie-account of een account voor de integratie met uw logische app niet meer kan gebruiken, verwijdert u de koppeling via Azure Resource Explorer.

1. Ga in uw browser naar <a href="https://resources.azure.com" target="_blank">Azure Resourceverkenner (https://resources.azure.com)</a>. Zorg ervoor dat u bent aangemeld met dezelfde Azure-referenties.

   ![Azure Resource Explorer](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

2. Voer de naam van uw logische app in in het zoekvak en vervolgens zoeken en selecteer uw logische app.

   ![Zoek en selecteer de logische app](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

3. Kies op de titelbalk explorer **lezen/schrijven**.

   ![' Lezen/schrijven'-modus inschakelen](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-read-write-mode.png)

4. Op de **gegevens** Kies **bewerken**.

   ![Kies op het tabblad "Gegevens", 'Bewerken'](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-edit.png)

5. Zoek in de editor het `integrationAccount` eigenschap voor de integratie-account in en verwijderen die eigenschap met deze indeling:

   ```json
   "integrationAccount": {
      "name": "<integration-account-name>",
      "id": "<integration-account-resource-ID>",
      "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Bijvoorbeeld:

   ![Definitie van de eigenschap 'integrationAccount' vinden](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

6. Op de **gegevens** Kies **plaatsen** uw wijzigingen op te slaan. 

   ![Kies "geplaatst' wijzigingen op te slaan](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

7. In de Azure-portal onder uw logische app **Werkstroominstellingen**, controleert u of de **integratie account** eigenschap nu is leeg.

   ![Controleer dat integratie-account niet is gekoppeld](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Integratie account verplaatsen

U kunt uw account integratie verplaatsen naar een andere Azure-abonnement of resourcegroep groep.

1. Selecteer op het Azure hoofdmenu **alle services**. Voer in het zoekvak 'integratieaccounts' als filter en selecteer **integratieaccounts**.

   ![Uw account integratie vinden](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Onder **integratieaccounts**, selecteer het integratie-account dat u wilt verplaatsen. Op uw integratie account menu onder **instellingen**, kies **eigenschappen**.

   ![Kies onder 'Instellingen', 'Eigenschappen'](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-properties.png)

3. De Azure-resourcegroep of een abonnement voor uw account integratie wijzigen.

   ![Kies 'Wijziging bronnengroep' of "Abonnement wijzigen"](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

4. Als u bent klaar, controleert u dat u alle scripts met de nieuwe resource-id's voor uw artefacten bijwerken.  

## <a name="delete-integration-account"></a>Integratie-account verwijderen

1. Selecteer op het Azure hoofdmenu **alle services**. Voer in het zoekvak 'integratieaccounts' als filter en selecteer **integratieaccounts**.

   ![Uw account integratie vinden](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Onder **integratieaccounts**, selecteer het integratie-account dat u wilt verwijderen. Kies in het menu van de account integratie **overzicht**, en kies vervolgens **verwijderen**. 

   ![Integratie-account selecteren. Kies op de pagina 'Overzicht', 'Verwijderen'](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

3. Om te bevestigen dat u wilt uw integratie-account verwijderen, kiest u **Ja**.

   ![Om te bevestigen verwijderen, kiest u 'Ja'](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Volgende stappen

* [Handelspartners maken](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Overeenkomsten maken](../logic-apps/logic-apps-enterprise-integration-agreements.md)