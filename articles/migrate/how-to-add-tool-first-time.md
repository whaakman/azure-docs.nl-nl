---
title: Toevoegen van een hulpprogramma voor beoordeling/migratie voor de eerste keer in Azure Migrate | Microsoft Docs
description: Beschrijft hoe u een Azure Migrate-project maken en toevoegen van een hulpprogramma voor beoordeling/migratie.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: b226f7c5879673b573133cde45db78d8d1f2fffa
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812023"
---
# <a name="add-an-assessmentmigration-tool-for-the-first-time"></a>Een hulpprogramma voor beoordeling/migratie voor de eerste keer toevoegen

In dit artikel wordt beschreven hoe u een evaluatie- of migratie hulpprogramma toevoegen een [Azure Migrate](migrate-overview.md) project voor de eerste keer.  
Azure Migrate biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen en workloads en persoonlijke/openbare cloud, virtuele machines naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden, onafhankelijke softwareleverancier (ISV) [aanbiedingen](migrate-services-overview.md#isv-integration) . 

## <a name="create-a-project-and-add-a-tool"></a>Maak een project en een programma toevoegen

Instellen van een nieuwe Azure Migrate-project in een Azure-abonnement en een programma toevoegen.

- Een Azure Migrate-project wordt gebruikt voor het opslaan van detectie, beoordeling en migratie metagegevens die zijn verzameld uit de omgeving u beoordelen of migreren. 
- U kunt in een project gedetecteerde activa bijhouden en maakt u gecoördineerde evaluatie en migratie.

1. In Azure portal > **alle services**, zoeken naar **Azure Migrate**.
2. Onder **Services**, selecteer **Azure Migrate**.

    ![Instellen van Azure Migrate](./media/how-to-add-tool-first-time/azure-migrate-search.png)

3. In **overzicht**, klikt u op **evalueren en migreren van servers**.
4. Onder **ontdekken, beoordelen en servers migreren**, klikt u op **evalueren en migreren van servers**.

    ![Detecteren en evalueren van servers](./media/how-to-add-tool-first-time/assess-migrate.png)

1. In **ontdekken, beoordelen en servers migreren**, klikt u op **toevoegen van extra**.
2. In **Migrate-project**, selecteer uw Azure-abonnement en maak een resourcegroep als u dit niet hebt.
3. In **projectdetails**, geef de naam van het project en het geografische gebied waarin u wilt maken van het project. 

    ![Een Azure Migrate-project maken](./media/how-to-add-tool-first-time/migrate-project.png)

    U kunt een Azure Migrate-project maken in een van deze locaties.

    **Geografie** | **Opslag locatie regio**
    --- | ---
    Azië | Zuidoost-Azië of Oost-Azië
    Europa | Zuid-Europa of West-Europa
    Verenigd Koninkrijk | UK-Zuid of UK-West
    Verenigde Staten | -Centraal VS of VS-West 2

    De opgegeven geografie voor het project wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines. U kunt een doelregio voor de daadwerkelijk migratie selecteren.

4. Klik op **volgende**, en voeg een hulpprogramma voor beoordeling of migratie.

    > [!NOTE]
    > Wanneer u een project maakt die u wilt toevoegen van ten minste één hulpprogramma voor beoordeling of migratie.

5. In **Selecteer assessment tool**, een beoordelingshulpprogramma voor toevoegen. Als u niet een beoordelingshulpprogramma voor nodig hebt, selecteert u **toevoegen van een hulpprogramma voor upgradebeoordeling voorlopig overslaan** > **volgende**. 
2. In **hulpprogramma voor migratie selecteren**, een hulpprogramma voor migratie toevoegen als vereist. Als u niet een hulpprogramma voor migratie nu nodig hebt, selecteert u **toevoegen van een hulpprogramma voor migratie van voorlopig overslaan** > **volgende**.
3. In **bekijken en toevoegen van extra**, Controleer de instellingen en klik op **hulpprogramma's toevoegen**.

U kunt extra hulpprogramma's voor evaluatie en migratie van servers en werkbelastingen, databases en web-apps selecteren na het maken van het project.

## <a name="create-additional-projects"></a>Extra projecten maken

In sommige gevallen moet u mogelijk aanvullende Azure Migrate-projecten maken. Bijvoorbeeld als u datacenters in verschillende fysieke locaties hebt, of u moet voor het opslaan van metagegevens in een andere Geografie bevinden. Maak een extra project als volgt:

1. In de huidige Azure Migrate-project, klikt u op **Servers** of **Databases**.
2. Klik in de rechterbovenhoek op **wijziging** naast de naam van het huidige project.
3. In **instellingen**, selecteer **Klik hier om te maken van een nieuw project**.
4. Maak een nieuw project en voeg een nieuw hulpprogramma toe zoals beschreven in de vorige procedure.

## <a name="next-steps"></a>Volgende stappen

Informatie over het toevoegen van extra [evaluatie](how-to-assess.md) en [migratie](how-to-migrate.md) hulpprogramma's. 
