---
title: Uw Azure Enterprise Overeenkomst registreren bij Cloudyn | Microsoft Docs
description: Gebruik uw Enterprise Overeenkomst om u te registreren bij Cloudyn.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/14/2018
ms.topic: quickstart
ms.custom: ''
ms.service: cost-management
manager: dougeby
ms.openlocfilehash: bed1ea3785c35d1053a0ff2147c3bdd797e28581
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996624"
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Een Azure Enterprise Overeenkomst registreren en kostengegevens weergeven

U gebruikt uw Azure Enterprise Overeenkomst om u te registreren bij Cloudyn. Uw registratie biedt toegang tot de Cloudyn-portal. In deze quickstart vindt u de details van de registratieprocedure voor het maken van een Cloudyn-proefabonnement en het aanmelden bij de Cloudyn-portal. U vindt er ook informatie over het weergeven van kostengegevens.

Azure Cost Management biedt vergelijkbare functionaliteit als Cloudyn. Azure Cost Management is een systeemeigen Azure-oplossing voor kostenbeheer. Het helpt u kosten te analyseren, budgetten te maken en beheren, gegevens te exporteren en aanbevelingen voor optimalisatie te bekijken en er actie op te ondernemen om geld te besparen. Zie [Azure Cost Management](overview-cost-mgt.md) voor meer informatie.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

- Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="register-with-cloudyn"></a>Registreren bij Cloudyn

1. Klik in Azure Portal, in de lijst met services, op **Cost Management en facturering**.
2. Klik onder **Overzicht** op **Cloudyn**  
    ![Cloudyn-pagina](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Klik op de **Cloudyn**-pagina op **Go to Cloudyn** om de registratiepagina van Cloudyn te openen in een nieuw venster.
4. Typ op de pagina voor de registratie van het proefabonnement in de Cloudyn-portal de naam van uw bedrijf en selecteer **Azure Enterprise Enrollment Administrator**.  
    ![registratie voor proefabonnement](./media/quick-register-ea/trial-reg.png)
5. Voer de API-sleutel voor de Enterprise Portal-registratie in. Als u de sleutel niet bij de hand hebt, klikt u op de koppeling [Enterprise Portal](https://ea.azure.com) en voert u de volgende stappen uit:
  1. Meld u aan bij de Azure Enterprise-website en klik op **Rapporten**, klik op **API Access Key** en kopieer de primaire sleutel.  
    ![EA API-sleutel](./media/quick-register-ea/ea-key.png)
  3. Ga terug naar de registratiepagina en plak de API-sleutel erin.
6. Accepteer de Gebruiksvoorwaarden en valideer de sleutel. Klik op **Volgende** om Cloudyn te autoriseren Azure-brongegevens te verzamelen. De gegevens die worden verzameld zijn onder meer gegevens over gebruik, prestaties, facturering en tags van uw abonnementen.  
    ![validatie van de sleutel](./media/quick-register-ea/ea-key-validated.png)
7. Onder **Invite other stakeholders** kunt u gebruikers toevoegen door hun e-mailadressen in te voeren. Klik op **Volgende** als u klaar bent. Afhankelijk van de grootte van uw Azure-inschrijving duurt het maximaal 24 uur voor al uw factureringsgegevens zijn toegevoegd aan Cloudyn.
8. Klik op **Go to Cloudyn** op de Cloudyn-portal te openen. Op de pagina **Cloud Accounts Management** zou u de geregistreerde EA-accountgegevens moeten kunnen zien.

Zie [How to Find Your EA Enrollment ID and API Key for use in Cloudyn](https://youtu.be/u_phLs_udig) (Uw EA-registratie-id en API-sleutel zoeken voor gebruik in Cloudyn) als u een video met een zelfstudie wilt bekijken over het registreren van uw Enterprise Overeenkomst.

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u uw Azure Enterprise Overeenkomst gebruikt om u te registreren bij Cloudyn. U hebt u ook aangemeld bij de Cloudyn-portal en de gegevenskosten weergegeven. Ga voor meer informatie over Cloudyn verder met de zelfstudie voor Cloudyn.

> [!div class="nextstepaction"]
> [Gebruik en kosten controleren](./tutorial-review-usage.md)
