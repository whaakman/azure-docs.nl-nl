---
title: 'Zelfstudie: Gegevensexports uit Azure Cost Management instellen en beheren | Microsoft Docs'
description: In dit artikel leest u hoe u gegevensexports uit Azure Cost Management instelt en beheert, zodat u deze kunt gebruiken in externe systemen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 0765e79b90eed49742f5eead33063907eb1db1f4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030843"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Zelfstudie: Gegevensexports instellen en beheren

Als u de zelfstudie over kostenanalyse hebt doorgenomen, bent u al bekend met het handmatig downloaden van gegevens uit Cost Management. U kunt echter ook een dagelijks uitgevoerde taak instellen om uw Cost Management-gegevens automatisch naar uw Azure-opslag te exporteren. De gegevens worden in CSV-indeling geëxporteerd en bevatten alle informatie die door Cost Management is verzameld. U kunt de geëxporteerde gegevens in uw Azure-opslag vervolgens in externe systemen laden en combineren met uw eigen gegevens. Externe systemen waarin de geëxporteerde gegevens kunnen worden gebruikt, zijn bijvoorbeeld dashboards of andere financiële systemen.

De voorbeelden in deze zelfstudie laten u stapsgewijs zien hoe u uw Cost Management-gegevens exporteert en vervolgens controleert of de export is gelukt.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een dagelijkse export uitvoeren
> * Controleren of de gegevens zijn geëxporteerd

## <a name="prerequisites"></a>Vereisten

Gegevensexport is beschikbaar voor alle klanten met een [Enterprise Overeenkomst (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). In abonnementen worden de volgende Azure-machtigingen ondersteund voor het exporteren van gegevens door gebruikers en groepen:

- Eigenaar: eigenaren kunnen geplande exports voor een abonnement instellen, wijzigen en verwijderen.
- Inzender: inzenders kunnen hun eigen geplande exports instellen, wijzigen en verwijderen. Zij kunnen bovendien de naam wijzigen van geplande exports die door anderen zijn ingesteld.
- Lezer: lezers kunnen exports plannen waarvoor ze zijn gemachtigd.

Voor Azure-opslagaccounts:
- Voor het wijzigen van een geconfigureerd opslagaccount zijn schrijfmachtigingen vereist, ongeacht de machtiging voor het exporteren.
- Uw Azure-opslagaccount moet zijn geconfigureerd voor blob- of bestandsopslag.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Een dagelijkse export uitvoeren

Kostenbeheer en facturering &gt; selecteer een abonnement of een resourcegroep in een abonnement &gt; klik op Exporteren &gt; klik op **Toevoegen**.

Typ een naam voor de export en geef het abonnement, het Azure-opslagaccount, de container en de map voor bestandsopslag of de blobcontainer op. Klik vervolgens op **Maken**.

![Nieuwe export](./media/tutorial-export-acm-data/new-export01.png)

Uw nieuwe export wordt weergegeven in de lijst met exports. Standaard worden nieuwe exports meteen ingeschakeld en dagelijks uitgevoerd. Als u een geplande export wilt uitschakelen of verwijderen, klikt u op het betreffende item in de lijst en vervolgens op **Uitschakelen** of **Verwijderen**.

Het kan één tot twee uur duren voordat de export voor het eerst wordt uitgevoerd. Het kan echter maximaal vier uur duren voordat er in het exportbestand gegevens worden opgenomen.

## <a name="verify-that-data-is-collected"></a>Controleren of de gegevens zijn geëxporteerd

U kunt eenvoudig controleren of uw Cost Management-gegevens daadwerkelijk zijn geëxporteerd door het CSV-exportbestand in Azure Storage Explorer te bekijken.

Klik in de lijst met exports op de naam van het opslagaccount. Klik op de pagina van het opslagaccount op Openen in Explorer. Er wordt nu een bevestigingsvenster weergegeven. Klik op **Ja** om het bestand te openen in Azure Storage Explorer.

![Pagina van het opslagaccount](./media/tutorial-export-acm-data/storage-account-page.png)

Ga in Storage Explorer naar de container die u wilt openen en selecteer de map voor de huidige maand. U ziet nu een lijst met CSV-bestanden. Selecteer het gewenste bestand en klik op **Openen**.

![Storage Explorer](./media/tutorial-export-acm-data/storage-explorer.png)

Het bestand wordt geopend met het programma of de toepassing waarmee bestanden met de extensie CSV standaard worden geopend. Dit is een voorbeeld met Excel.

![Voorbeeld van geëxporteerde gegevens](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Geëxporteerde gegevens gebruiken in externe systemen

Het exporteren van Cost Management-gegevens heeft vaak als doel om deze gegevens in een extern systeem te gebruiken. Denk bijvoorbeeld aan een dashboard of een ander financieel systeem. Omdat al deze systemen sterk van elkaar verschillen, heeft het weinig zin om hier een voorbeeld van te laten zien.  In [Introduction to Azure Storage](../storage/common/storage-introduction.md) (Inleiding tot Azure Storage) kunt u echter aan de slag met het openen van uw gegevens in de door u gebruikte toepassingen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een dagelijkse export uitvoeren
> * Controleren of de gegevens zijn geëxporteerd

Ga naar de volgende zelfstudie, waarin u leert om de efficiëntie te optimaliseren en te verbeteren door inactieve en onderbenutte resources te identificeren.

> [!div class="nextstepaction"]
> [Aanbevelingen bekijken en hier actie op ondernemen](tutorial-acm-opt-recommendations.md)
