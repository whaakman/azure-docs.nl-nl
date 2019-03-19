---
title: Azure Data Box Edge bandbreedte schema's beheren | Microsoft Docs
description: Beschrijft hoe u Azure portal gebruiken voor het beheren van bandbreedte schema's voor uw Azure Data Box-edge-apparaten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/12/2019
ms.author: alkohli
ms.openlocfilehash: 2c8d21a9494bace7d60b90637450cb10a5f3576e
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2019
ms.locfileid: "58109355"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-data-box-edge"></a>De Azure-portal gebruiken voor het beheren van bandbreedte schema's voor uw Azure Data Box-edge-apparaten  

In dit artikel wordt beschreven hoe u gebruikers voor uw Azure Data Box-edge-apparaten beheren. Met bandbreedteschema's kunt u het gebruik van netwerkbandbreedte beheren over schema's voor meerdere tijdstippen. Deze schema's kunnen worden toegepast op upload- en downloadbewerkingen van uw apparaat naar de cloud.

U kunt toevoegen, wijzigen of verwijderen van de bandbreedte-schema's voor uw gegevens in Edge via Azure portal.

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een schema toevoegen
> * Een schema wijzigen
> * Een schema verwijderen


## <a name="add-a-schedule"></a>Een schema toevoegen

Voer de volgende stappen uit in de Azure portal om een planning toevoegen.

1. In de Azure-portal voor uw gegevens in Edge, gaat u naar **bandbreedte**.
2. Selecteer in het rechterdeelvenster **+ toevoegen planning**.

    ![Selecteer de bandbreedte](media/data-box-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. Doe het volgende in **Schema toevoegen**: 

   1. Geef de **Eerste dag**, **Laatste dag**, **Begintijd** en **Eindtijd** van de planning op.
   2. Controleer de **alle dag** optie als u dit schema moet alle dag worden uitgevoerd.
   3. **Bandbreedtesnelheid** is de bandbreedte in Megabits per seconde (Mbps) die door uw apparaat wordt gebruikt bij bewerkingen die betrekking hebben op de cloud (uploaden en downloaden). Geef een getal tussen 20 en 1,000,000,007 voor dit veld.
   4. Schakel **Onbeperkte** bandbreedte in als u de datumupload en -download niet wilt regelen.
   5. Selecteer **Toevoegen**.

      ![Schema toevoegen](media/data-box-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Er wordt een schema gemaakt met de opgegeven parameters. Dit schema wordt vervolgens weergegeven in de lijst van bandbreedteschema's in de portal.

    ![Bijgewerkte lijst van bandbreedte schema 's](media/data-box-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Schema bewerken

Voer de volgende stappen uit als u een bandbreedteschema wilt bewerken.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **bandbreedte**. 
2. Selecteer in de lijst van bandbreedte schema's, en selecteer een planning die u wilt wijzigen.
    ![Bandbreedte planning selecteren](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. Breng de gewenste wijzigingen aan en sla de wijzigingen op.

    ![Gebruiker wijzigen](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. Wanneer het schema is gewijzigd, wordt de lijst met schema's bijgewerkt met het gewijzigde schema.

    ![Gebruiker wijzigen](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Een schema verwijderen

Voer de volgende stappen uit als u wilt verwijderen van de planning van een bandbreedte die is gekoppeld met uw gegevens in het Edge-apparaat.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **bandbreedte**.  

2. Selecteer in de lijst met bandbreedteschema's een schema dat u wilt verwijderen. In de **planning bewerken**, selecteer **verwijderen**. Wanneer u hierom wordt gevraagd om bevestiging, selecteert u **Ja**.

   ![Een gebruiker verwijderen](media/data-box-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. Wanneer de planning is verwijderd, wordt de lijst met schema's bijgewerkt.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van bestandsshares](data-box-edge-manage-shares.md).
