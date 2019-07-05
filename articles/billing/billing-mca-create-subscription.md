---
title: Een extra Azure-abonnement voor uw factureringsrekening maken
description: Leer hoe u een nieuwe Azure-abonnement toevoegen in Azure portal.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 558410d980d261780f7287d1e27ed704b356fc2b
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490944"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Een extra Azure-abonnement voor Microsoft-KLANTOVEREENKOMST maken

Als u meer abonnementen voor uw factureringsrekening voor het instellen van afzonderlijke omgevingen voor ontwikkelen en testen, beveiliging of om te isoleren van de gegevens om wettelijke redenen maken.

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access). Als u wilt maken van abonnementen voor andere soorten facturering accounts, Zie [een extra abonnement maken in Azure portal](billing-create-subscription.md).

Voor het maken van een abonnement, moet u een **factuur sectie eigenaar**, **factuur sectie Inzender**, of **de maker van de Azure-abonnement**. Zie voor meer informatie, [abonnement facturering rollen en taken](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Zie voor andere machtiging voor het maken van Azure-abonnementen voor uw factureringsrekening, [Geef anderen toestemming voor het maken van Azure-abonnementen](#give-others-permission).

## <a name="create-a-subscription"></a>Een abonnement maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken naar **abonnementen**.

   ![Schermafbeelding van zoeken in de portal voor abonnementen](./media/billing-mca-create-subscription/billing-search-subscriptions.png)

3. Selecteer **toevoegen**

4. Als u toegang tot meerdere accounts van facturering hebt, selecteert u de rekening voor uw Microsoft-KLANTOVEREENKOMST.

   ![Schermafbeelding van pagina-abonnement maken](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Selecteer een profiel voor facturering. De kosten voor uw abonnement wordt gefactureerd voor het geselecteerde profiel voor facturering. Als u toegang tot slechts één factureringsprofiel hebt, wordt de selectie grijs weergegeven.

6. Selecteer een gedeelte van de factuur. De kosten voor uw abonnement wordt gefactureerd in deze sectie van de facturering van het profiel factuur. Als u toegang tot slechts een gedeelte van de factuur hebt, wordt de selectie grijs weergegeven.

7. Selecteer een plan voor het abonnement. Selecteer **Microsoft Azure-abonnement voor DevTest**, als u van plan bent dit abonnement gebruiken voor ontwikkeling of testworkloads anders **Microsoft Azure-abonnement**. Als u toegang tot slechts één abonnement hebt, wordt de selectie grijs weergegeven.

8. Voer een naam in voor het abonnement. De naam van de helpt u bij het herkennen van het abonnement in Azure portal.

9. Selecteer **Maken**.

## <a name="give-others-permission"></a>Anderen toestemming geven

Gebruikers toevoegen als ontwikkelaars van Azure-abonnement op een gedeelte van de factuur ze om toestemming te geven aan Azure-abonnementen maken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken naar **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in de portal voor abonnementen](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Ga naar de factureringssectie. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount of factureringsprofiel selecteren. Selecteer in de facturering-account of profiel **factuur secties** en vervolgens een factuur sectie in de lijst. Alle abonnementen die worden gemaakt door de gebruikers worden gefactureerd in deze sectie factuur.
   
   ![Schermafbeelding van selecteren secties factuur](./media/billing-mca-create-subscription/mca-select-invoice-sections.png)        

4. Selecteer **Access Management (IAM)** vanaf de linksboven.

5. Selecteer **Toevoegen** bovenaan de pagina.

6. Selecteer **de maker van de Azure-abonnement** voor de rol.

7. Voer het e-mailadres van de gebruiker aan wie u wilt om toegang te geven.

8. Selecteer **Opslaan**.

## <a name="check-access"></a>Toegang controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- [Geef anderen toestemming voor het maken van Azure-resources met behulp van ingebouwde rollen](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Een windows-machine maken](../virtual-machines/windows/quick-create-portal.md)
- [Een linux-machine maken](../virtual-machines/linux/quick-create-portal.md)
- [Beheergroepen voor resource organisatie en beheer maken](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
