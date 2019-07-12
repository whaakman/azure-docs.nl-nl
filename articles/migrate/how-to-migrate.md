---
title: Hulpprogramma's voor migratie in Azure Migrate toevoegen
description: Beschrijft hoe u hulpprogramma's voor migratie toevoegt in de Azure Migrate-hub.
author: rayne-wiselman
ms.service: azure-migrate
ms.manager: carmonm
ms.topic: article
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: b3c77f052ed92db235b363e63859b9beb9e4f5a2
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811867"
---
# <a name="add-migration-tools"></a>Toevoegen van hulpprogramma's voor migratie

In dit artikel wordt beschreven hoe u om toe te voegen van de hulpprogramma's voor migratie in [Azure Migrate](migrate-overview.md).

Azure Migrate biedt een hub van hulpprogramma's voor evaluatie en migratie naar Azure. Het bevat systeemeigen hulpprogramma's, hulpprogramma's die door andere Azure-services en van derden independent software vendor (ISV)-aanbiedingen.

Als u wilt toevoegen van een hulpprogramma voor migratie en een Azure Migrate-project nog niet hebt ingesteld, volgt u deze [artikel](how-to-add-tool-first-time.md).



## <a name="selecting-an-isv-tool"></a>Een ISV-gereedschap selecteren

Als u ervoor kiest een [ISV hulpprogramma](migrate-services-overview.md#isv-integration) voor migratie, kunt u beginnen met een licentie worden aangeschaft of zich aanmelden voor een gratis proefversie, in overeenstemming met de ISV-beleid. Er is een optie verbinding maken met Azure Migrate in elk hulpprogramma. Implementatie van het hulpprogramma en volg de instructies voor het hulpprogramma en de documentatie om te verbinden van de werkruimte hulpprogramma met Azure Migrate. 

## <a name="select-a-migration-scenario"></a>Selecteer een scenario voor cloudmigratie

1. Klik in de Azure Migrate-project op **overzicht**.
2. Selecteer het migratiescenario die u wilt gebruiken:

    - Selecteer voor het migreren van computers en werkbelastingen naar Azure, **evalueren en migreren van servers**.
    - Selecteer voor het migreren van on-premises SQL-computers, **evalueren en migreren van databases**.
    - Selecteer voor het migreren van on-premises web-apps, **evalueren en migreren van web-apps**.
    - Voor het migreren van grote hoeveelheden gegevens van on-premises naar Azure in de offlinemodus bevindt, selecteert u **een Data Box bestellen**.

    ![Evaluatie-scenario](./media/how-to-migrate/assess-scenario.png)

## <a name="select-a-server-migration-tool"></a>Selecteer een server-hulpprogramma voor migratie

1. Klik op **evalueren en migreren van Servers**.
2. In **Azure Migrate - Servers**, als u hulpprogramma's voor migratie nog niet hebt toegevoegd onder **hulpprogramma voor migratie van**, selecteer **Klik hier als u wilt toevoegen van een hulpprogramma voor migratie van**. Als u hulpprogramma's voor migratie, al hebt toegevoegd **toevoegen van meer hulpprogramma's voor migratie**, selecteer **wijziging**.

    > [!NOTE]
    > Als u nodig hebt om te navigeren naar een ander project in **Azure Migrate - Servers**, naast **Zie de details voor een andere migrate-project**, klikt u op **Klik hier**.

3. In **Azure Migrate**, selecteert u het hulpprogramma voor migratie u wilt gebruiken.
    - Als u Azure Migrate-servermigratie gebruikt, kunt u instellen en migraties rechtstreeks in de Azure Migrate-project uitvoeren.
    - Als u een beoordelingshulpprogramma voor van derden gebruikt, gaat u naar de koppeling voor de ISV en voer de migratie in overeenstemming met de instructies die ze bieden.

## <a name="select-a-database-migration-tool"></a>Selecteer een database-hulpprogramma voor migratie

1. Klik op **evalueren en migreren van databases**
2. In **Databases**, klikt u op **toevoegen van extra**.
3. In een programma toevoegen > **hulpprogramma voor migratie selecteren**, selecteert u het hulpprogramma die u gebruiken wilt voor het migreren van uw database.

## <a name="select-a-web-app-migration-tool"></a>Selecteer een hulpprogramma voor migratie van web-app

1. Klik op **evalueren en migreren van web-apps**.
2. Volg de koppeling naar het hulpprogramma voor migratie voor Azure App Service. Gebruik het hulpprogramma voor migratie naar:

    - **Apps online beoordelen**: U kunt beoordelen en migreren van apps met een openbare URL online, met behulp van de Azure App Service Migration Assistant.
    - **.NET/PHP**: U kunt downloaden en uitvoeren van de migratie-assistent voor interne .NET en PHP-apps.

## <a name="order-an-azure-data-box"></a>Een Azure Data Box bestellen

Voor het migreren van grote hoeveelheden gegevens naar Azure, kunt u een Azure DAta Box voor de overdracht van offline bestellen.

1. Klik op **een Data Box bestellen**.
2. In **selecteert u uw Azure Data Box**, Geef uw abonnement. 
3. De overdracht is een importeren naar Azure. Geef op de gegevensbron en de doel-Azure-regio voor de gegevens.

## <a name="next-steps"></a>Volgende stappen

Probeer een migratie met behulp van Azure Migrate servermigratie voor [Hyper-V](tutorial-migrate-hyper-v.md) of [VMware](tutorial-migrate-vmware.md) VM's.
