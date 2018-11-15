---
title: Verwijderen en exporteren van persoonlijke gegevens van Azure DevTest Labs | Microsoft Docs
description: Informatie over het verwijderen en exporteren van persoonlijke gegevens van de service Azure DevLast Labs ter ondersteuning van uw verplichtingen onder de General Data Protection Regulation (GDPR).
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: spelluru
ms.openlocfilehash: e681652c13e521bd33524e247db65088f47a794c
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51642435"
---
# <a name="export-or-delete-personal-data-from-azure-devtest-labs"></a>Exporteren of verwijderen van persoonlijke gegevens uit Azure DevTest Labs
Dit artikel bevat stappen voor het verwijderen en exporteren van persoonlijke gegevens van de service Azure DevTest Labs. 

## <a name="what-personal-data-does-devtest-labs-collect"></a>Welke persoonlijke gegevens verzamelt DevTest Labs?
DevTest Labs worden twee belangrijkste onderdelen van persoonlijke gegevens verzameld van de gebruiker. Ze zijn: e-mailadres van gebruiker en gebruikers-object-ID. Deze informatie is van cruciaal belang voor de service te bieden in gebruik zijnde voorzieningen voor lab-beheerders en gebruikers van de testomgeving.

### <a name="user-email-address"></a>E-mailadres gebruiker
DevTest Labs maakt gebruik van de e-mailadres van de gebruiker automatisch afsluiten e-mailmeldingen verzenden naar labgebruikers. Het e-mailbericht waarschuwt gebruikers van de machine wordt afgesloten. De gebruikers kunnen uitstellen of overslaan het afsluiten als ze willen doen. U kunt het e-mailadres configureren op het niveau van het lab of op het niveau van de virtuele machine.

**E-mail instellen aan het lab:**

![E-mail instellen op het niveau van de testomgeving](./media/personal-data-delete-export/lab-user-email.png)

**E-mailadres in te stellen op de virtuele machine:**

![E-mail instellen op het niveau van de virtuele machine](./media/personal-data-delete-export/vm-user-email.png)

### <a name="user-object-id"></a>Object-ID van gebruiker
DevTest Labs maakt gebruik van de object-ID van de gebruiker om weer te geven van de maand na maand kostentrends en kosten door informatie over de resources voor lab-beheerders. Hiermee kunnen de kosten bijhouden en beheren van drempelwaarden voor de testomgeving. 

**Trend van de geschatte kosten voor de huidige kalendermaand:**
![trend van de geschatte kosten voor de huidige kalendermaand](./media/personal-data-delete-export/estimated-cost-trend-per-month.png)

**Geschatte kosten maand tot heden per resource:**
![geschatte kosten maand tot heden per resource](./media/personal-data-delete-export/estimated-month-to-date-cost-by-resource.png)


## <a name="why-do-we-need-this-personal-data"></a>Waarom moeten we deze persoonlijke gegevens?
De DevTest Labs-service gebruikt de persoonlijke gegevens voor operationele doeleinden. Deze gegevens is essentieel voor de service voor de levering van de belangrijkste functies. Als u een bewaarbeleid ingesteld op de e-mailadres van gebruiker, doen labgebruikers niet tijdig automatisch afsluiten e-mailmeldingen ontvangen nadat het e-mailadres is verwijderd van ons systeem. De beheerder lab weergeven niet op dezelfde manier maand na maand kostentrends en kosten per resource voor computers in hun labs als het gebruikersobject dat id 's zijn verwijderd op basis van een beleid voor Gegevensretentie. Daarom moet deze gegevens worden bewaard voor als de bron van de gebruiker actief in het Lab is.

## <a name="how-can-i-have-the-system-to-forget-my-personal-data"></a>Hoe kan ik het systeem vergeten mijn persoonlijke gegevens hebben?
Als een lab-gebruiker, als u wilt deze persoonlijke gegevens hebt verwijderd, kunt u doen door het verwijderen van de overeenkomende resource in het Lab. De service DevTest Labs anoniem gemaakt op de verwijderde gegevens 30 dagen nadat deze verwijderd door de gebruiker.

Bijvoorbeeld, als u uw virtuele machine verwijdert of verwijderd van uw e-mailadres, de service DevTest Labs anoniem gemaakt deze gegevens 30 dagen na de resource wordt verwijderd. Het beleid voor het bewaren van 30 dagen na verwijdering is om ervoor te zorgen dat wij een nauwkeurige maand na maand kostenramingen naar het lab-beheerder bieden.

## <a name="how-can-i-request-an-export-on-my-personal-data"></a>Hoe kan ik op mijn persoonlijke gegevens exporteren van een vragen?
Als een gebruiker lab, kunt u aanvragen exporteren van een van de persoonlijke gegevens die de DevTest Labs-service worden opgeslagen. Om aan te vragen voor een exporteren, gaat u naar de **persoonsgegevens** kiezen op de **overzicht** pagina van uw testomgeving. Selecteer de **aanvragen exporteren** knop serversysteemstatus van het maken van een downloadbare excel-bestand in de storage-account van de beheerder van uw testomgeving. U kunt vervolgens contact op met uw lab-beheerder om deze gegevens weer te geven.

1. Selecteer **persoonsgegevens** in het menu links. 

    ![Pagina persoonlijke gegevens](./media/personal-data-delete-export/personal-data-page.png)
2. Selecteer de **resourcegroep** die in het lab bevat.

    ![Resourcegroep selecteren](./media/personal-data-delete-export/select-resource-group.png)
3. Selecteer de **opslagaccount** in de resourcegroep.
4. Op de **opslagaccount** weergeeft, schakelt **Blobs**.

    ![Selecteer de tegel Blobs](./media/personal-data-delete-export/select-blobs-tile.png)
5. Selecteer de container met de naam **labresourceusage** in de lijst met containers.

    ![Blobcontainer selecteren](./media/personal-data-delete-export/select-blob-container.png)
6. Selecteer de **map** met de naam van uw testomgeving. U vindt **csv** bestanden voor **schijven** en **virtuele machines** in uw testomgeving in deze map. U kunt deze csv-bestanden downloaden, de inhoud voor de lab-gebruiker die een toegang aanvragen filteren en met hen delen.

    ![CSV-bestand downloaden](./media/personal-data-delete-export/download-csv-file.png)

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- [Beleidsregels op te stellen voor een lab](devtest-lab-get-started-with-lab-policies.md)
- [Veelgestelde vragen](devtest-lab-faq.md)
