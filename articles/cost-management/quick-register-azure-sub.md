---
title: Uw Azure-abonnement registreren bij Azure Cost Management | Microsoft Docs
description: Gebruik uw Azure-abonnement om u te registreren bij Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: quickstart
ms.custom: ''
ms.service: cost-management
manager: dougeby
ms.openlocfilehash: 254a7f2904a63381a4eeec02ee32b19890703ae1
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296627"
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>Een individueel Azure-abonnement registreren en gegevenskosten weergeven

Gebruik uw Azure-abonnement om u te registreren bij Azure Cost Management. Uw registratie biedt toegang tot de Cloudyn-portal. In deze quickstart vindt u de details van de registratieprocedure voor het maken van een Cloudyn-proefabonnement en het aanmelden bij de Cloudyn-portal. U vindt er ook informatie over het weergeven van kostengegevens.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

- Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="register-with-azure-cost-management"></a>Registreren bij Azure Cost Management

1. Klik in Azure Portal, in de lijst met services, op **Cost Management en facturering**.
2. Klik onder **Overzicht** op **Cost Management**  
    ![Pagina Cost Management](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. Klik op de pagina **Cost Management** op **Go to Cost Management** om de Cloudyn-registratiepagina in een nieuw venster te openen.
4. Typ op de pagina voor de registratie van het proefabonnement in de Cloudyn-portal de naam van uw bedrijf en selecteer **Azure Individual Subscription Owner** en klik op **Volgende**. Uw accountnaam en tenant-id worden automatisch toegevoegd aan het formulier.  
    ![registratie voor proefabonnement](./media/quick-register-azure-sub/trial-reg-ind.png)
5. Selecteer uw **Aanbiedings-id - Naam** die is gekoppeld aan uw abonnement. Als u niet zeker bent van de tarief-id voor uw abonnement, kunt u uw Azure-factuur weergeven en zoeken naar **Aanbiedings-id**.
6. Accepteer de Gebruiksvoorwaarden en valideer uw gegevens. Klik vervolgens op **Volgende**.
7. Klik op de pagina **Gather additional data** op **Volgende** om Cloudyn te autoriseren Azure-brongegevens te verzamelen. De gegevens die worden verzameld zijn onder meer gegevens over gebruik, prestaties, facturering en tags van uw abonnementen.  
    ![aanvullende gegevens verzamelen](./media/quick-register-azure-sub/gather-additional.png)
8. U gaat in de browser naar de aanmeldingspagina voor Cloudyn. Meld u aan met de referenties voor uw Azure-abonnement.
9. Klik op **Go to Cloudyn** op de Cloudyn-portal te openen. Op de pagina **Accounts Management** zou u de accountgegevens voor het Azure-abonnement moeten zien.  
    ![Accounts Management](./media/quick-register-azure-sub/accounts-mgt.png)

Zie [Finding your Directory GUID and Rate ID for use in Azure Cost Management](https://youtu.be/PaRjnyaNGMI) (Uw Directory-GUID en tarief-id zoeken voor gebruik in Azure Cost Management) als u een video met een zelfstudie wilt bekijken over het registreren van uw Azure-abonnement.

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u de gegevens van uw Azure-abonnement gebruikt om u te registreren bij Cost Management. U hebt zich ook aangemeld bij de Cloudyn-portal en de gegevenskosten weergegeven. Ga door naar de zelfstudie voor Cost Management voor meer informatie over Azure Cost Management.

> [!div class="nextstepaction"]
> [Gebruik en kosten controleren](./tutorial-review-usage.md)
