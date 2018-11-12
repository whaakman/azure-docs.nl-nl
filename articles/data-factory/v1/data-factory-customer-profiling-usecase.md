---
title: 'Use case: klantprofilering'
description: Meer informatie over hoe Azure Data Factory wordt gebruikt voor het maken van een werkstroom gegevensgestuurde (pijplijn) wilt profileren gaming-klanten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 5345ca3a9c2f35b45669a3a54ecb42ca627cde32
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259816"
---
# <a name="use-case---customer-profiling"></a>Use case: klantprofilering
Azure Data Factory is een van vele services die worden gebruikt voor het implementeren van de Cortana Intelligence Suite van Oplossingsverbeteringen.  Bezoek voor meer informatie over Cortana Intelligence [Cortana Intelligence Suite](https://www.microsoft.com/cortanaanalytics). In dit document wordt beschreven een eenvoudige use-case zodat u kunt aan de slag met informatie over hoe Azure Data Factory analytics van veelvoorkomende problemen kunt oplossen.

## <a name="scenario"></a>Scenario
Contoso is een gamingbedrijf dat wordt gemaakt van games voor meerdere platforms: game-consoles, handheld-apparaten en pc's (pc's). Spelers afspelen deze games, wordt groot volume aan gegevens waarmee de gebruikspatronen, gaming-stijl en voorkeuren van de gebruiker wordt bijgehouden geproduceerd.  In combinatie met demografische, regionale en de productgegevens, Contoso analyses uitvoeren waardoor ze over het verbeteren van spelers ervaring te helpen kunt uitvoeren en doel deze voor upgrades en in-game-aankopen. 

Contoso doel is het identificeren van up-selling/cross-selling kansen op basis van de geschiedenis games de spelers en interessante functies toevoegen aan zakelijke groei te stimuleren en een betere ervaring bieden aan klanten. Gebruik in dit geval gebruiken we nemen een gamingbedrijf als voorbeeld van een bedrijf. Het bedrijf wil de games op basis van gedrag van spelers optimaliseren. Deze principes gelden voor elk bedrijf die wil Neem contact op de klanten over de goederen en diensten en verbeter de ervaring van hun klanten.

In deze oplossing wil Contoso evalueren van de effectiviteit van een marketingcampagne die het laatst is gestart. We beginnen met de onbewerkte gaming-Logboeken, verwerken en te verrijken met gegevens over geografische locaties, voeg deze toe met het adverteren van referentiegegevens en ten slotte kopiëren naar een Azure SQL-Database voor het analyseren van de impact van de campagne.

## <a name="deploy-solution"></a>Oplossing implementeren
Alles wat u nodig om te openen en proberen dit eenvoudige voorbeeld wordt een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/), een [Azure Blob storage-account](../../storage/common/storage-quickstart-create-account.md), en een [Azure SQL Database](../../sql-database/sql-database-get-started.md). U implementeert de klantprofilering pijplijn van de **voorbeeld pijplijnen** tegel op de startpagina van uw data factory.

1. Een data factory maken of open een bestaande gegevensfactory. Zie [gegevens kopiëren van Blob Storage naar SQL Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stappen voor het maken van een data factory.
2. In de **DATA FACTORY** blade voor de data factory, klikt u op de **voorbeeld pijplijnen** tegel.

    ![Voorbeeld van pijplijnen tegel](./media/data-factory-samples/SamplePipelinesTile.png)
3. In de **voorbeeld pijplijnen** blade, klikt u op de **Klantprofilering** die u wilt implementeren.

    ![Voorbeeld pijplijnen-blade](./media/data-factory-samples/SampleTile.png)
4. Configuratie-instellingen voor het voorbeeld opgeven. Bijvoorbeeld: uw Azure storage-accountnaam en -sleutel, Azure SQL-servernaam, -database, gebruikers-ID en wachtwoord.

    ![Voorbeeld van blade](./media/data-factory-samples/SampleBlade.png)
5. Nadat u klaar bent met de configuratie-instellingen op te geven, klikt u op **maken** maken/implementatie van de voorbeeldpijplijnen en gekoppelde services/tabellen die worden gebruikt door de pijplijnen.
6. U ziet de status van de implementatie van op de voorbeeld-tegel die u hebt geklikt op eerder de **voorbeeld pijplijnen** blade.

    ![Implementatiestatus](./media/data-factory-samples/DeploymentStatus.png)
7. Als u ziet de **implementatie is voltooid** bericht op de tegel voor het voorbeeld, sluit de **voorbeeld pijplijnen** blade.  
8. Op **DATA FACTORY** blade ziet u dat de gekoppelde services, gegevenssets en pijplijnen worden toegevoegd aan uw gegevensfactory.  

    ![Blade Gegevensfactory](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Overzicht van de oplossing
In dit eenvoudige voorbeeld kan worden gebruikt als een voorbeeld van hoe u Azure Data Factory gebruiken kunt om opnemen, voorbereiden, transformeren, analyseren en publiceren van gegevens.

![End-to-end werkstroom](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

In deze afbeelding ziet u hoe de gegevens-pipelines in Azure portal worden weergegeven nadat ze zijn geïmplementeerd.

1. De **PartitionGameLogsPipeline** leest de ruwe gebeurtenissen van blob-opslag en partities op basis van het jaar, maand en dag maakt.
2. De **EnrichGameLogsPipeline** lid wordt van gepartitioneerde gamegebeurtenissen met referentiegegevens voor geo-code en verrijkt de gegevens door het IP-adressen toewijzen aan de bijbehorende geografische locaties.
3. De **AnalyzeMarketingCampaignPipeline** pijplijn gebruikt de verrijkt gegevens en verwerkt deze met de reclame-gegevens voor het maken van de uiteindelijke uitvoer met de marketing effectiviteit van campagnes.

In dit voorbeeld wordt Data Factory gebruikt voor het indelen van activiteiten die invoergegevens, transformeren en verwerken van de gegevens te kopiëren en voert u de laatste gegevens naar een Azure SQL-Database.  U kunt ook het netwerk van gegevenspijplijnen visualiseren, ze beheren en controleren van de status vanuit de gebruikersinterface.

## <a name="benefits"></a>Voordelen
Door de analyse van hun gebruikers-profiel te optimaliseren en het afstemmen van deze met de bedrijfsdoelen, kan gaming-bedrijf snel gebruikspatronen verzamelen en analyseren van de effectiviteit van marketingcampagnes.

