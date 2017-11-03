---
title: Registreren van uw Azure Enterprise-overeenkomst met Azure kosten Management | Microsoft Docs
description: Gebruik uw Enterprise overeenkomst registreren bij Azure kosten Management Cloudyn.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 41a9df712b07253d9f5f9db8542fb9917592320f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Registreren van een Enterprise Agreement voor Azure en de weergave kostengegevens

Enterprise Agreement voor uw Azure kunt u met Azure kosten Management door Cloudyn registreren. Uw registratie biedt toegang tot de portal Cloudyn. De details van deze Quick Start het registratieproces nodig voor het maken van een proefabonnement Cloudyn en meld u aan bij de portal Cloudyn. Deze ook wordt beschreven hoe u meteen kostengegevens bekijken.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

- Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-trial-registration"></a>Een proefabonnement registratie maken

1. Klik in de Azure-portal op **kostenbeheer + facturering** in de lijst met services.
2. Onder **overzicht**, klikt u op **kostenbeheer**  
    ![Kosten Management-pagina](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Op de **kostenbeheer** pagina **gaat u naar kostenbeheer** naar de registratiepagina Cloudyn in een nieuw venster openen.
4. Typ de naam van uw bedrijf op de pagina van de portal proefversie registratie Cloudyn en selecteer vervolgens **Azure inschrijving ondernemingsbeheerder**.  
    ![de registratie van de evaluatieversie](./media/quick-register-ea/trial-reg.png)
5. Voer uw Enterprise Portal enrollment API-sleutel. Als u uw sleutel bij de hand hebt, klikt u op de [Enterprise Portal](https://ea.azure.com) koppelen en voer de volgende stappen uit:
  1. Aanmelden bij de Azure Enterprise-website en klik op **rapporten**, klikt u op **API toegangssleutel** en kopieer de primaire sleutel.  
    ![EA API-sleutel](./media/quick-register-ea/ea-key.png)
  3. Ga terug naar de registratiepagina en plakken in uw API-sleutel.
6. Ga akkoord met de gebruiksvoorwaarden en vervolgens uw sleutel te valideren. Klik op **volgende** te autoriseren Cloudyn voor het verzamelen van gegevens van de Azure-resource. Verzamelde gegevens omvatten informatie over het gebruik, prestaties, facturering en labelgegevens van uw abonnementen.  
    ![validatie van de sleutel](./media/quick-register-ea/ea-key-validated.png)
7. Onder **andere belanghebbenden uitnodigen**, kunt u gebruikers toevoegen door hun e-mailadressen in te voeren. Wanneer voltooid, klikt u op **volgende**. Het duurt ongeveer twee uur voor uw facturering gegevens ophalen die zijn toegevoegd aan Cloudyn.
8. Klik op **gaat u naar Cloudyn** de Cloudyn portal openen en klik vervolgens op de **Cloud accountbeheer** pagina ziet u geregistreerde EA gegevens over uw account.

Een zelfstudie als video wilt bekijken over het registreren van uw Enterprise overeenkomst, Zie [vinden uw EA inschrijving-ID en API-sleutel voor gebruik in Azure kosten Management Cloudyn](https://youtu.be/u_phLs_udig).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u uw gegevens Enterprise Agreement voor Azure kostenbeheer registreren. U ook aangemeld bij de portal Cloudyn en gestart kostengegevens weer te geven. Voor meer informatie over het beheer van Azure kosten door Cloudyn doorgaan naar de zelfstudie voor het beheer van kosten.

> [!div class="nextstepaction"]
> [Bekijk informatie over het gebruik en de kosten](./tutorial-review-usage.md)
