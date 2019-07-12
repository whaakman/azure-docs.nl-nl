---
title: Analyseprogramma's toevoegen in Azure Migrate | Microsoft Docs
description: Beschrijft hoe u analyseprogramma's toevoegt in de Azure Migrate-hub.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: d176e6276d69cd3465aa4943efa86ea1e6b0736d
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811646"
---
# <a name="add-assessment-tools"></a>Analyseprogramma's toevoegen

In dit artikel wordt beschreven hoe u om toe te voegen analyseprogramma's in [Azure Migrate](migrate-overview.md).

Azure Migrate biedt een hub van hulpprogramma's voor evaluatie en migratie naar Azure. Het bevat systeemeigen hulpprogramma's, hulpprogramma's die door andere Azure-services en van derden independent software vendor (ISV)-aanbiedingen.

Als u wilt toevoegen van een beoordelingshulpprogramma voor en u nog een Azure Migrate-project hebt, volgt u deze [artikel](how-to-add-tool-first-time.md).

## <a name="selecting-an-isv-tool"></a>Een ISV-gereedschap selecteren

Als u ervoor kiest een [ISV hulpprogramma](migrate-services-overview.md#isv-integration) voor evaluatie, kunt u beginnen met een licentie worden aangeschaft of zich aanmelden voor een gratis proefversie, in overeenstemming met de ISV-beleid. Er is een optie verbinding maken met Azure Migrate in elk hulpprogramma. Volg de instructies voor het hulpprogramma en de documentatie om te verbinden van de werkruimte hulpprogramma met Azure Migrate. 


## <a name="select-an-assessment-scenario"></a>Selecteer een evaluatie-scenario

1. Klik in de Azure Migrate-project op **overzicht**.
2. Selecteer de evaluatie-scenario dat u wilt gebruiken:

    - Als u wilt detecteren en evalueren van machines en workloads voor migratie naar Azure, selecteer **evalueren en migreren van servers**.
    - Selecteer voor het evalueren van on-premises SQL-computers, **evalueren en migreren van databases**.
    - Selecteer voor het evalueren van on-premises web-apps, **evalueren en migreren van web-apps**.

    ![Evaluatie-scenario](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Selecteer een hulpprogramma voor beoordeling 

1. Klik op **evalueren en migreren van Servers**.
2. In **Azure Migrate - Servers**, als u nog een assessment tool onder toegevoegd **analyseprogramma's**, selecteer **Klik hier als u wilt toevoegen van een hulpprogramma voor upgradebeoordeling**. Als u analyseprogramma's, al hebt toegevoegd **toevoegen van meer analyseprogramma's**, selecteer **wijziging**.

    > [!NOTE]
    > Als u nodig hebt om te navigeren naar een ander project in **Azure Migrate - Servers**, naast **Zie de details voor een andere migrate-project**, klikt u op **Klik hier**.

3. In **Azure Migrate**, selecteert u het hulpprogramma assessment waarmee u wilt gebruiken.

    
    ![Analyseprogramma 's](./media/how-to-assess/assess-tool.png)

    - Als u Azure Migrate-Server-evaluatie, kunt u instellen, uitvoeren en evaluaties rechtstreeks in de Azure Migrate-project weergeven.
    - Als u een beoordelingshulpprogramma voor van derden gebruikt, gaat u naar de koppeling voor hun site en de evaluatie in overeenstemming met de instructies die ze bieden uitvoeren.


## <a name="select-a-database-assessment-tool"></a>Selecteer een hulpmiddel voor database-evaluatie

1. Klik op **evalueren en migreren van databases**
2. In **Databases**, klikt u op **toevoegen van extra**.
3. In een programma toevoegen > **Selecteer assessment tool**, selecteert u het hulpprogramma die u wilt gebruiken voor het evalueren van uw database.

## <a name="select-a-web-app-assessment-tool"></a>Beoordelingshulpprogramma voor een web-app selecteren

1. Klik op **evalueren en migreren van web-apps**.
2. Volg de koppeling naar het hulpprogramma voor migratie voor Azure App Service. Gebruik het hulpprogramma voor migratie naar:

    - **Apps online beoordelen**: U kunt apps met een openbare URL online, met behulp van de Azure App Service Migration Assistant beoordelen.
    - **.NET/PHP**: U kunt downloaden en uitvoeren van de migratie-assistent voor interne .NET en PHP-apps.



## <a name="next-steps"></a>Volgende stappen

Probeer een evalueren met behulp van Azure Migrate-Server-evaluatie voor [Hyper-V](tutorial-prepare-hyper-v.md) of [VMware](tutorial-prepare-vmware.md) VM's.
