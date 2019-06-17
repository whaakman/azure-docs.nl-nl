---
title: Retourneren van uw Azure Data Box-Edge-apparaat | Microsoft Docs
description: Beschrijft hoe u het Azure Data Box-Edge-apparaat en de volgorde voor het apparaat verwijderen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/07/2019
ms.author: alkohli
ms.openlocfilehash: 9aeae0ab68d809b36a3316054f12a5a9657721f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65468602"
---
# <a name="return-your-azure-data-box-edge-device"></a>Retourneren van uw Azure Data Box-Edge-apparaat

In dit artikel wordt beschreven hoe u de gegevens wissen en ga vervolgens terug uw Azure Data Box-Edge-apparaat. Nadat u het apparaat hebt opgevraagd, kunt u ook de resource die is gekoppeld aan het apparaat verwijderen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De gegevens uit de gegevensschijven op het apparaat wissen
> * Een ondersteuningsticket openen om te retourneren van uw apparaat
> * Het apparaat in en ophalen van objecten plannen
> * Verwijderen van de resource in Azure portal

## <a name="erase-data-from-the-device"></a>Gegevens van het apparaat wissen

Voor het wissen van de gegevens uit de gegevensschijven van uw apparaat, moet u uw apparaat opnieuw instelt. U kunt uw apparaat met behulp van de lokale web-UI of de PowerShell-interface opnieuw instellen.

Voordat u opnieuw instelt, moet u een kopie van de lokale gegevens op het apparaat maken indien nodig. U kunt de gegevens van het apparaat kopiëren naar een Azure Storage-container.

Als u wilt met behulp van de lokale web-UI-apparaat opnieuw instelt, moet u de volgende stappen uitvoeren.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > apparaat opnieuw ingesteld**.
2. Selecteer **apparaat opnieuw instellen van**.

    ![Apparaat opnieuw instellen](media/data-box-edge-return-device/device-reset-1.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, bekijk de waarschuwing en selecteer **Ja** om door te gaan.

    ![Bevestig het opnieuw instellen](media/data-box-edge-return-device/device-reset-2.png)  

Opnieuw instellen van het worden de gegevens uit de gegevensschijven van het apparaat gewist. Afhankelijk van de hoeveelheid gegevens op uw apparaat duurt dit proces ongeveer 30-40 minuten.

U kunt ook verbinding maken met de PowerShell-interface van het apparaat en gebruik de `Reset-HcsAppliance` cmdlet om de gegevens uit de gegevensschijven te wissen. Zie voor meer informatie, [uw apparaat opnieuw instelt](data-box-edge-connect-powershell-interface.md#reset-your-device).

> [!NOTE]
> - Als u uitwisselen of een upgrade naar een nieuw apparaat, wordt u aangeraden dat u uw apparaat opnieuw instelt nadat u het nieuwe apparaat hebt ontvangen.
> - Het apparaat alleen opnieuw worden alle lokale gegevens uit het apparaat verwijderd. De gegevens die zich in de cloud is niet verwijderd en verzamelt [kosten](https://azure.microsoft.com/pricing/details/storage/). Deze gegevens moeten worden verwijderd afzonderlijk met een cloud storage management-hulpprogramma zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

## <a name="open-a-support-ticket"></a>Open een ondersteuningsticket

Als u wilt beginnen met de geretourneerde, moet u de volgende stappen uitvoeren.

1. Open een ondersteuningsticket met Microsoft Support waarmee wordt aangegeven dat u wilt terugkeren van het apparaat. Selecteer het probleemtype als **Data Box Edge Hardware**.

    ![Ondersteuningsticket maken](media/data-box-edge-return-device/open-support-ticket-1.png)  

2. Een Microsoft Support engineer neemt contact met u. Geef de verzendgegevens.
3. Als u nodig hebt voor een retour verpakking, kunt u deze aanvragen. Antwoord **Ja** op de vraag **moet een leeg vak om terug te keren**.


## <a name="schedule-a-pickup"></a>Ophalen van objecten plannen

1. Schakel het apparaat uit. Ga in de lokale webgebruikersinterface naar **onderhoud > energie-instellingen**.
2. Selecteer **afsluiten**. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan. Zie voor meer informatie, [energiebeheer](data-box-gateway-manage-access-power-connectivity-mode.md#manage-power).
3. De power-kabels los te koppelen en de netwerkkabels van het apparaat verwijderd.
4. Bereid de verzending-pakket met behulp van uw eigen box of het lege vak die u hebt ontvangen van Azure. Plaats het apparaat en de stroomkabels die zijn verzonden met het apparaat in het vak.
5. Brengt het verzendlabel die u hebt ontvangen van Azure op het pakket.
6. Een ophalen met de regionale provider plannen. Als het apparaat in de Verenigde Staten worden geretourneerd, is de provider Getotaliseerd. Ophalen van objecten plannen:

    1. Aanroepen van de lokale Noodvoeding (landspecifieke gratis telefoonnummer).
    2. Citeren de omgekeerde verzending traceringsnummer zoals in het afgedrukte label wordt weergegeven in uw aanroep.
    3. Als het volgnummer is niet tussen aanhalingstekens, moet UPS u een extra kosten betalen tijdens het ophalen.

    In plaats van de planning van het ophalen, kunt u ook de rand van de gegevens in op de dichtstbijzijnde afgiftelocatie teruglopen.

## <a name="delete-the-resource"></a>De resource verwijderen

Nadat het apparaat is ontvangen op het Azure-datacenter, wordt het apparaat gecontroleerd op beschadiging of tekenen van knoeien.

- Als het apparaat intact en in orde ontvangt, stopt de factureringsmeter voor die bron. Microsoft Support neemt contact met u om te bevestigen dat het apparaat is geretourneerd. Vervolgens kunt u de resource die is gekoppeld aan het apparaat in Azure portal verwijderen.
- Als het apparaat aanzienlijk beschadigd ontvangt, kunnen boetes toepassen. Zie voor meer informatie, de [Veelgestelde vragen over verloren of beschadigd apparaat](https://azure.microsoft.com/pricing/details/databox/edge/) en [servicevoorwaarden Product](https://www.microsoft.com/licensing/product-licensing/products).  


U kunt het apparaat in Azure portal verwijderen:
-   Nadat u hebt de bestelling heeft geplaatst en voordat het apparaat wordt voorbereid door Microsoft.
-   Nadat u hebt het apparaat terug naar Microsoft, de fysieke inspectie wordt doorgegeven in de Azure-datacenter en Microsoft Support-aanroepen om te bevestigen dat het apparaat is geretourneerd.

Als u het apparaat op basis van een ander abonnement of een andere locatie hebt geactiveerd, zal Microsoft uw order verplaatsen naar het nieuwe abonnement of de locatie binnen één werkdag. Nadat de order wordt verplaatst, kunt u deze resource verwijderen.


De volgende stappen om het apparaat en de resource in Azure portal te verwijderen.

1. In de Azure-portal, gaat u naar uw resource en van daaruit naar **overzicht**. Selecteer in de opdrachtbalk **verwijderen**.

    ![Selecteer verwijderen](media/data-box-edge-return-device/delete-resource-1.png)

2. In de **apparaat verwijderen** blade, typ de naam van het apparaat dat u wilt verwijderen en selecteer **verwijderen**.

    ![De verwijdering bevestigen](media/data-box-edge-return-device/delete-resource-2.png)

Krijgt u een melding nadat het apparaat en de gekoppelde resource wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Bandbreedte beheren](data-box-edge-manage-bandwidth-schedules.md).
