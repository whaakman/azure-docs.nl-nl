---
title: Ondersteuning voor Avere vFXT krijgen voor Azure
description: Uitleg van het openen van ondersteuningstickets over Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: d621511cbb6983f8ad57ea8305823039475f40d0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633934"
---
# <a name="get-help-with-your-system"></a>Hulp krijgen bij het systeem

Als u hulp nodig hebt met uw vFXT Avere voor Azure, vindt hier u de verschillende manieren ondersteuning krijgen:

* **Avere vFXT probleem** -de Azure portal gebruiken voor het openen van een ondersteuningsticket voor uw vFXT Avere, zoals wordt beschreven [hieronder](#open-a-support-ticket-for-your-avere-vfxt).
* **Quotum** : als er een probleem met betrekking tot quotum [een verhoging aanvragen](#request-a-quota-increase)
* **Documentatie en voorbeelden** : als u problemen ondervindt met deze documentatie of de voorbeelden, Ga naar de onderkant van de pagina met het probleem en gebruik de **Feedback** sectie om te zoeken naar bestaande problemen en een nieuwe als een bestand nodig.  

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>Open een ondersteuningsticket voor uw vFXT Avere

Als u problemen ondervindt bij het implementeren of Avere vFXT gebruiken, om hulp vragen via de Azure-portal.  

Volg deze stappen om ervoor te zorgen dat uw ondersteuningsticket is gemarkeerd met een resource van het cluster. Labels van het ticket helpt ons deze route naar de juiste ondersteuning-resource. 

1. Van [ https://portal.azure.com ](https://portal.azure.com), selecteer **resourcegroepen**.

   ![schermopname van Azure portal linkermenu met 'Resourcegroepen' omcirkeld](media/avere-vfxt-ticket-rg.png)

1. Blader naar de resourcegroep met de vFXT cluster waar het probleem is opgetreden, en klik op een van de Avere virtuele machines.

    ![schermopname van Azure portal-resource group 'overzicht' paneel met een bepaalde virtuele machine omcirkeld](media/avere-vfxt-ticket-vm.png)

1. Blader op de pagina virtuele machine naar de onderkant van het linkerdeelvenster en klik op **nieuwe ondersteuningsaanvraag**.

    ![Schermopname van de Azure portal VM-pagina voor de virtuele machine in de vorige schermafbeelding. Het menu links wordt verschoven naar beneden en 'Nieuwe ondersteuningsaanvraag' wordt omcirkeld.](media/avere-vfxt-ticket-request.png)

1. Klik op de pagina een van de ondersteuningsaanvraag **alle Services** en kijk onder **opslag** kiezen **Avere vFXT**.

    ![schermopname van het nieuwe scherm van de aanvraag voor ondersteuning in Azure portal met de header 'Basisbeginselen' en een cirkel om de 'Service'-item. De knop 'Alle services' is ingeschakeld en het veld in het menu heeft de waarde "Avere vFXT"](media/avere-vfxt-ticket-service.png)

1. Kies het probleemtype en de categorie die het meest overeenkomen met uw probleem op pagina twee. Voeg een korte titel en een beschrijving met de tijd waarop die het probleem is opgetreden. 

   ![Schermafbeelding van het nieuwe scherm van de aanvraag voor ondersteuning met de header '-probleem', waarin veel velden die moeten worden uitgevoerd](media/avere-vfxt-ticket-problem.png)

1. Op pagina drie, vul uw contactgegevens in en klikt u op **maken**. Een bevestigings- en ticket-nummer wordt verzonden naar uw e-mailadres en een lid van de afdeling ondersteuning neemt contact met u.

## <a name="request-a-quota-increase"></a>Een verhoging aanvragen

Lezen [quotum voor het cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) voor meer informatie over welke onderdelen nodig zijn voor het implementeren van de vFXT Avere voor Azure. U kunt [een verhoging aanvragen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) vanuit Azure portal.