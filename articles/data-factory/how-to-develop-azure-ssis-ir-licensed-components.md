---
title: Gelicentieerde onderdelen installeren voor Azure-SSIS Integration runtime | Microsoft Docs
description: Meer informatie over hoe een ISV betaalde of gelicentieerde aangepaste onderdelen kan ontwikkelen en installeren voor Azure SSIS Integration runtime
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 1c574578e6ed6ee032be01718eb3e8afd27fdf6f
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708013"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Betaalde of gelicentieerde aangepaste onderdelen installeren voor Azure-SSIS Integration runtime

In dit artikel wordt beschreven hoe een ISV, betaalde of gelicentieerde aangepaste onderdelen voor SQL Server Integration Services (SSIS)-pakketten die worden uitgevoerd in azure, kan ontwikkelen en installeren in de Azure SSIS Integration runtime.

## <a name="the-problem"></a>Het probleem

De aard van de Azure SSIS Integration runtime biedt verschillende uitdagingen, waardoor de gebruikelijke licentie methoden worden gebruikt voor de on-premises installatie van aangepaste onderdelen die niet toereikend zijn. Als gevolg hiervan is een andere benadering vereist voor de Azure-SSIS IR.

-   De knoop punten van de Azure-SSIS IR zijn vluchtig en kunnen op elk gewenst moment worden toegewezen of vrijgegeven. U kunt knoop punten bijvoorbeeld starten of stoppen om de kosten te beheren, of omhoog en omlaag schalen door verschillende groottes van knoop punten. Als gevolg hiervan is het binden van een licentie van een derde partij aan een bepaald knoop punt met behulp van computerspecifieke info, zoals MAC-adres of CPU-ID niet langer beschikbaar.

-   U kunt ook de Azure-SSIS-IR in-of uitschalen, zodat het aantal knoop punten op elk gewenst moment kan worden verkleind of uitgevouwen.

## <a name="the-solution"></a>De oplossing

Als gevolg van de beperkingen van traditionele licentie methoden die in de vorige sectie zijn beschreven, biedt Azure-SSIS IR een nieuwe oplossing. Deze oplossing maakt gebruik van Windows-omgevings variabelen en SSIS-systeem variabelen voor de licentie binding en validatie van onderdelen van derden. Isv's kunnen deze variabelen gebruiken om unieke en permanente informatie te verkrijgen voor een Azure-SSIS-IR, zoals cluster-ID en aantal cluster knooppunten. Met deze informatie kunnen Isv's de licentie voor hun onderdeel vervolgens binden aan een Azure-SSIS IR *als een cluster*. Deze binding maakt gebruik van een ID die niet verandert wanneer klanten worden gestart of gestopt, omhoog of omlaag schalen, in-of uitschalen of de Azure-SSIS IR op een wille keurige manier opnieuw configureren.

In het volgende diagram ziet u de typische installatie, activering en licentie bindingen en validatie stromen voor onderdelen van derden die gebruikmaken van deze nieuwe variabelen:

![Installatie van gelicentieerde onderdelen](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Instructies
1. Isv's kunnen hun gelicentieerde onderdelen aanbieden in verschillende Sku's of lagen (bijvoorbeeld één knoop punt, Maxi maal vijf knoop punten, Maxi maal 10 knoop punten, enzovoort). De ISV levert de bijbehorende product code wanneer klanten een product kopen. De ISV kan ook een Azure Storage BLOB-container bieden die een ISV-installatie script en bijbehorende bestanden bevat. Klanten kunnen deze bestanden in hun eigen opslag container kopiëren en deze wijzigen met hun eigen product code (bijvoorbeeld door uit te voeren `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Klanten kunnen vervolgens de Azure-SSIS IR inrichten of opnieuw configureren met de SAS-URI van hun container als para meter. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie.

2. Wanneer de Azure-SSIS IR wordt ingericht of opnieuw geconfigureerd, wordt ISV Setup uitgevoerd op elk knoop punt om een query uit te voeren op `SSIS_CLUSTERID` de `SSIS_CLUSTERNODECOUNT`Windows-omgevings variabelen, en. Vervolgens verzendt de Azure-SSIS IR de cluster-ID en de product code voor het gelicentieerde product naar de ISV-activerings server om een activerings sleutel te genereren.

3. Na ontvangst van de activerings sleutel kan ISV Setup de sleutel lokaal opslaan op elk knoop punt (bijvoorbeeld in het REGI ster).

4. Wanneer klanten een pakket uitvoeren dat gebruikmaakt van het gelicentieerde onderdeel van de ISV op een knoop punt van de Azure-SSIS-IR, leest het pakket de lokaal opgeslagen activerings sleutel en valideert deze met de cluster-ID van het knoop punt. Het pakket kan ook optioneel het aantal cluster knooppunten rapporteren aan de ISV-activerings server.

    Hier volgt een voor beeld van code die de activerings sleutel valideert en het aantal cluster knooppunten rapporteert:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="isv-partners"></a>ISV-partners

U vindt een lijst met ISV-partners die hun onderdelen en uitbrei dingen voor de Azure-SSIS IR aan het eind van deze blog post- [Enter prise Edition, aangepaste installatie en uitbreid baarheid van derden voor SSIS in ADF](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Enterprise-Edition-Custom-Setup-and-3rd-Party-Extensibility-for/ba-p/388360)hebben aangepast.

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste installatie voor Azure-SSIS Integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Enter prise Edition van het Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)
