---
title: Maken van een extra Azure-abonnement voor uw factureringsrekening | Microsoft Docs
description: Leer hoe u een nieuwe Azure-abonnement toevoegen in Azure portal.
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 105b8481486c088a05e3acb95081d3ee55b55f52
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897097"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Een extra Azure-abonnement voor Microsoft-KLANTOVEREENKOMST maken

Als u meer abonnementen voor uw factureringsrekening voor het instellen van afzonderlijke omgevingen voor ontwikkelen en testen, beveiliging of om te isoleren van de gegevens om wettelijke redenen maken.

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement). Als u wilt maken van abonnementen voor andere rekeningen, Zie [een extra abonnement maken in Azure portal](billing-create-subscription.md).

Voor het maken van een abonnement, moet u een **factuur sectie eigenaar**, **factuur sectie Inzender**, of **de maker van de Azure-abonnement**. Zie voor meer informatie, [abonnement facturering rollen en taken](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Zie voor andere machtiging voor het maken van Azure-abonnementen voor uw factureringsrekening, [Geef anderen toestemming voor het maken van Azure-abonnementen](#give-others-permission-to-create-azure-subscriptions).

## <a name="create-a-subscription-in-the-azure-portal"></a>Een abonnement maken in Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken naar **abonnementen**.

   ![Schermafbeelding van zoeken in de portal voor abonnementen](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Selecteer **toevoegen**

4. Als u toegang tot meerdere accounts van facturering hebt, selecteert u de rekening voor uw Microsoft-KLANTOVEREENKOMST.

   ![Schermafbeelding van pagina-abonnement maken](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Selecteer een profiel voor facturering. De kosten voor uw abonnement wordt weergegeven op de facturering van het profiel factuur en met behulp van de betalingswijzen worden betaald. Als u toegang tot slechts één factureringsprofiel hebt, wordt de selectie grijs weergegeven.

6. Selecteer een gedeelte van de factuur. De kosten voor uw abonnement wordt weergegeven in deze sectie van de facturering van het profiel factuur. Als u toegang tot slechts een gedeelte van de factuur hebt, wordt de selectie grijs weergegeven.

7. Selecteer een plan voor het abonnement. Selecteer **Microsoft Azure-abonnement voor DevTest**, als u van plan bent dit abonnement gebruiken voor ontwikkeling of testworkloads anders **Microsoft Azure-abonnement**. Als u toegang tot slechts één abonnement hebt, wordt de selectie grijs weergegeven.

8. Voer een naam in voor het abonnement. De naam van de helpt u bij het herkennen van het abonnement in Azure portal.

9. Selecteer **Maken**.

## <a name="give-others-permission-to-create-azure-subscriptions"></a>Geef anderen toestemming voor het maken van Azure-abonnementen

Gebruikers toevoegen als ontwikkelaars van Azure-abonnement op een gedeelte van de factuur ze om toestemming te geven aan Azure-abonnementen maken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in de portal voor abonnementen](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Ga naar het gedeelte factuur. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount of factureringsprofiel selecteren. Selecteer in de facturering-account of profiel **factuur secties** en vervolgens een gedeelte van de factuur.

4. Selecteer **Access Management (IAM)** vanaf de linksboven.

5. Vanaf de bovenkant van de pagina, selecteer **toevoegen**.

6. Selecteer **de maker van de Azure-abonnement** voor de rol.

   ![Schermafbeelding van Azure-abonnement Maker rol toekennen aan een gebruiker](./media/billing-mca-create-subscription/billing-mca-add-azure-subscription-creator.png)

7. Voer het e-mailadres van de gebruiker aan wie u wilt om toegang te geven.

8. Selecteer **Opslaan**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Geef anderen toestemming voor het maken van Azure-resources met behulp van ingebouwde rollen](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Een windows-machine maken](../virtual-machines/windows/quick-create-portal.md)
- [Een linux-machine maken](../virtual-machines/linux/quick-create-portal.md)
- [Beheergroepen voor resource organisatie en beheer maken](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
