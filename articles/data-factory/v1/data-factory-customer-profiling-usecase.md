---
title: 'Use case: klantprofilering'
description: Meer informatie over hoe Azure Data Factory wordt gebruikt voor het maken van een werkstroom gegevensgestuurde (pijplijn) om het profiel gaming-klanten.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 3a3c9c1d483ece71ecc38c7eaa364c35464d80f6
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="use-case---customer-profiling"></a>Use case: klantprofilering
Azure Data Factory is een van de vele services die worden gebruikt voor het implementeren van de Cortana Intelligence Suite accelerators oplossing.  Voor meer informatie over de Cortana Intelligence, gaat u naar [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). In dit document beschrijven we een eenvoudige gebruiksvoorbeeld zodat u aan de slag met informatie over hoe Azure Data Factory algemene analytics problemen kunt oplossen.

## <a name="scenario"></a>Scenario
Contoso is een gaming-bedrijf dat games voor meerdere platforms maakt: game-consoles, handheld apparaten en pc's (pc's). Als deze spelen spelers bevat, moet op grote hoeveelheid gegevens aan het logboek waarmee wordt bijgehouden van de gebruikspatronen, gaming-stijl en voorkeuren van de gebruiker wordt geproduceerd.  In combinatie met demografische gegevens, regio en productgegevens, Contoso analytics om de stappen waarmee ze over het verbeteren van spelers ervaring kunt uitvoeren en ze voor upgrades en spel koopt. 

Contoso doel is het identificeren van verkopen/cross-Upsell-mogelijkheden op basis van de gaming-geschiedenis van de spelers en dwingende functies toevoegen aan een station zakelijke groei en een betere ervaring te bieden aan klanten. Voor deze aanvraag gebruiken we een gaming-bedrijf gebruiken als een voorbeeld van een bedrijf. Het bedrijf wil de games op basis van gedrag spelers optimaliseren. Deze principes zijn van toepassing op alle bedrijven die wil benaderen van zijn klanten rond de goederen en diensten en hun klanten ervaring te verbeteren.

In deze oplossing wil Contoso evalueren van de effectiviteit van een marketingcampagne die onlangs zijn gestart. We beginnen met de onbewerkte gaming-Logboeken, verwerkt en ze aanvullen met geolocatie gegevens basislid referentiegegevens reclame en ten slotte kopiëren naar een Azure SQL Database voor het analyseren van de impact van de campagne.

## <a name="deploy-solution"></a>Oplossing implementeren
Alles wat u nodig hebt voor toegang tot en probeer deze eenvoudige gebruiksvoorbeeld is een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/), een [Azure Blob storage-account](../../storage/common/storage-create-storage-account.md#create-a-storage-account), en een [Azure SQL Database](../../sql-database/sql-database-get-started.md). Implementeren van de klant profilering pijplijn van de **steekproef pijplijnen** tegel op de startpagina van uw gegevensfactory.

1. Maak een gegevensfactory of open een bestaande gegevensfactory. Zie [gegevens kopiëren van Blob-opslag naar SQL Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stappen voor het maken van een gegevensfactory.
2. In de **DATA FACTORY** blade voor de gegevensfactory klikt u op de **steekproef pijplijnen** tegel.

    ![Voorbeeld pijplijnen tegel](./media/data-factory-samples/SamplePipelinesTile.png)
3. In de **steekproef pijplijnen** blade, klikt u op de **klant profilering** die u wilt implementeren.

    ![Voorbeeld pijplijnen-blade](./media/data-factory-samples/SampleTile.png)
4. Geef de configuratie-instellingen voor het voorbeeld. Bijvoorbeeld uw Azure-opslag-accountnaam en sleutel, naam van de Azure SQL-server, database, gebruikers-ID en wachtwoord.

    ![Voorbeeld-blade](./media/data-factory-samples/SampleBlade.png)
5. Nadat u klaar bent met het opgeven van de configuratie-instellingen, klikt u op **maken** voor het maken/implementeren van de steekproef pijplijnen en gekoppelde services/tabellen die worden gebruikt door de pijplijnen.
6. U ziet de status van implementatie op de voorbeeld-tegel waarop u hebt geklikt eerder op de **steekproef pijplijnen** blade.

    ![Implementatiestatus](./media/data-factory-samples/DeploymentStatus.png)
7. Als u ziet de **implementatie is voltooid** bericht op de tegel voor het voorbeeld, sluit de **steekproef pijplijnen** blade.  
8. Op **DATA FACTORY** blade ziet u dat de gekoppelde services, gegevenssets en pijplijnen worden toegevoegd aan uw gegevensfactory.  

    ![Blade Gegevensfactory](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Overzicht van de oplossing
Deze eenvoudige gebruiksvoorbeeld kan worden gebruikt als een voorbeeld van hoe u Azure Data Factory gebruiken kunt voor het opnemen, voorbereiden, transformeren, analyseren en gegevens publiceren.

![End-to-end werkstroom](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Deze afbeelding ziet hoe de gegevenspijplijnen in de Azure portal worden weergegeven nadat deze zijn geïmplementeerd.

1. De **PartitionGameLogsPipeline** leest de onbewerkte game gebeurtenissen van blob-opslag en wordt gemaakt op basis van het jaar, maand en dag partities.
2. De **EnrichGameLogsPipeline** gepartitioneerde game gebeurtenissen met de referentiegegevens geo-code koppelt en verrijkt de gegevens door het IP-adressen toewijzen aan de bijbehorende geografische locaties.
3. De **AnalyzeMarketingCampaignPipeline** pijplijn gebruikt de verrijkt gegevens en verwerkt deze met de promotie-gegevens voor het maken van de uiteindelijke uitvoer met de effectiviteit van marketing campagne.

In dit voorbeeld wordt de Data Factory gebruikt voor het indelen van activiteiten die invoergegevens, transformeren en proces de gegevens te kopiëren en uitvoer van de laatste gegevens naar een Azure SQL-Database.  U kunt ook het netwerk van gegevenspijplijnen visualiseren, ze beheren en controleren van de status van de gebruikersinterface.

## <a name="benefits"></a>Voordelen
Door het optimaliseren van hun profiel gebruiker analytics en het afstemmen met doelen, kan games bedrijf snel gebruikspatronen verzamelen en analyseren van de effectiviteit van marketingcampagnes.

