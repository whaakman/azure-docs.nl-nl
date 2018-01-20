---
title: Azure Functions met Visual Studio ontwikkelen | Microsoft Docs
description: Informatie over het ontwikkelen en testen van Azure Functions met behulp van Azure Functions-hulpprogramma's voor Visual Studio 2017.
services: functions
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: glenga
ms.openlocfilehash: 4681138dfc7ed67c8c9da0c55abfc27351736be4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-functions-tools-for-visual-studio"></a>Azure Functions-Tools voor Visual Studio  

Azure Functions-hulpprogramma's voor Visual Studio 2017 is een uitbreiding voor Visual Studio waarmee u ontwikkelen, testen en implementeren van C#-functies in Azure. Als deze ervaring uw eerste met Azure Functions is, kunt u meer informatie op [een inleiding tot Azure Functions](functions-overview.md).

De hulpprogramma's van Azure Functions biedt de volgende voordelen: 

* Bewerken, bouwen en uitvoeren van functies op de lokale computer. 
* Uw project Azure Functions rechtstreeks publiceren naar Azure. 
* WebJobs kenmerken gebruiken om te declareren functiebindingen rechtstreeks in de C#-code in plaats van het onderhouden van een afzonderlijke function.json voor het binden van definities.
* Ontwikkelen en implementeren van vooraf gecompileerde C#-functies. Vooraf gecompileerde functies bieden een betere koude start prestaties dan C# script gebaseerde functies. 
* Uw functies in C#-code terwijl alle voordelen van Visual Studio-ontwikkeling. 

Dit onderwerp leest u hoe de Azure Functions-Tools voor Visual Studio 2017 gebruiken voor het ontwikkelen van uw functies in C#. U leert ook hoe uw project publiceren naar Azure als een .NET-assembly.

> [!IMPORTANT]
> Combineer geen lokale ontwikkeling met portal-ontwikkeling in dezelfde functie-app. Wanneer u vanaf een lokaal project naar een functie-app publiceert, overschrijft het implementatieproces kunnen functies die u hebt ontwikkeld in de portal.

## <a name="prerequisites"></a>Vereisten

Azure Functions-hulpprogramma's is opgenomen in de werklast Azure ontwikkeling van [Visual Studio 2017 versie 15,4](https://www.visualstudio.com/vs/), of een latere versie. Zorg ervoor dat u de **ontwikkelen van Azure** werkbelasting in de installatie van Visual Studio 2017:

![Visual Studio 2017 installeren met de Azure-ontwikkelworkload](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Als u wilt maken en implementeren van functies, moet u ook:

* Een actief Azure-abonnement. Als u een Azure-abonnement geen [gratis accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) beschikbaar zijn.

* Een Azure Storage-account. Zie het maken van een opslagaccount [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-an-azure-functions-project"></a>Een Azure Functions-project maken 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a>Het project voor lokale ontwikkeling configureren

Als u een nieuw project met de Azure Functions-sjabloon maakt, krijgt u een leeg C#-project met de volgende bestanden:

* **host.JSON**: Hiermee kunt u de functies host configureren. Deze instellingen gelden beide wanneer lokaal en in Azure wordt uitgevoerd. Zie voor meer informatie [host.json verwijzing](functions-host-json.md).
    
* **Local.Settings.JSON**: onderhoudt instellingen bij lokale uitvoering van de functies die worden gebruikt. Deze instellingen worden niet gebruikt door Azure, ze worden gebruikt door de [kernonderdelen van Azure Functions](functions-run-local.md). Dit bestand opgeven van instellingen, zoals de verbindingsreeksen om andere Azure-services te gebruiken. Een nieuwe sleutel toevoegen aan de **waarden** matrix voor elke verbinding vereist voor functies in uw project. Zie voor meer informatie [lokale instellingenbestand](functions-run-local.md#local-settings-file) in het onderwerp kernonderdelen van Azure Functions.

De runtime van Functions wordt intern gebruikt een Azure Storage-account. Voor alle typen dan HTTP en webhooks activeert, moet u instellen de **Values.AzureWebJobsStorage** sleutel op een geldige verbindingsreeks voor Azure Storage-account. 

[!INCLUDE [Note on local storage](../../includes/functions-local-settings-note.md)]

 De verbindingsreeks voor opslag account instellen:

1. Open in Visual Studio **Cloud Explorer**, vouw **Opslagaccount** > **uw Opslagaccount**, selecteer daarna **eigenschappen**en kopieer de **primaire verbindingsreeks** waarde.   

2. Open het bestand local.settings.json in uw project en stel de waarde van de **AzureWebJobsStorage** sleutel op de verbindingsreeks die u hebt gekopieerd.

3. Herhaal de vorige stap om toe te voegen unieke sleutels zijn aan de **waarden** matrix voor eventuele andere verbindingen die vereist zijn voor uw functies.  

## <a name="create-a-function"></a>Een functie maken

In de vooraf gecompileerde functies worden de bindingen die wordt gebruikt door de functie gedefinieerd door het toepassen van kenmerken in de code. Wanneer u de hulpprogramma's van Azure Functions voor het maken van uw functies van de opgegeven sjablonen gebruikt, worden deze kenmerken worden toegepast voor u. 

1. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer  > **Nieuw item****Toevoegen**. Selecteer **Azure-functie**, typ een **naam** voor de klasse en klik op **toevoegen**.

2. Kies de trigger, stelt u de bindingeigenschappen en klikt u op **maken**. Het volgende voorbeeld ziet de instellingen wanneer de functie voor het maken van een Queue storage geactiveerd. 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    In dit voorbeeld trigger wordt een verbindingsreeks met een sleutel met de naam **QueueStorage**. Deze instelling van de tekenreeks moet worden gedefinieerd in het bestand local.settings.json. 
 
3. Onderzoek de zojuist toegevoegde klasse. U ziet een statische **uitvoeren** methode, dat is toegewezen met de **functienaam** kenmerk. Dit kenmerk geeft aan dat de methode het toegangspunt voor de functie. 

    De volgende C#-klasse staat bijvoorbeeld voor een eenvoudige wachtrij geactiveerd opslagfunctie:

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    Een kenmerk binding-specifieke wordt toegepast op elke binding parameter doorgegeven aan de methode post point. Het kenmerk neemt de bindingsgegevens als parameters. In het vorige voorbeeld wordt de eerste parameter heeft een **QueueTrigger** kenmerk dat wordt toegepast, die wachtrij geactiveerd functie aangeeft. De wachtrijnaam en de naam van instelling verbindingsreeks worden doorgegeven als parameters voor de **QueueTrigger** kenmerk.

## <a name="testing-functions"></a>Functies testen

Met Azure Functions Core-hulpprogramma's kunt u Azure Functions-projecten uitvoeren op uw lokale ontwikkelcomputer. De eerste keer dat u een functie vanuit Visual Studio start, wordt u gevraagd deze hulpprogramma's te installeren.  

Druk op F5 om de functie testen. Accepteer desgevraagd de aanvraag van Visual Studio om Azure Functions Core (CLI)-hulpprogramma's te downloaden en installeren. Mogelijk moet u ook een firewall-uitzondering inschakelen, zodat de hulpprogramma's HTTP-aanvragen kunnen afhandelen.

Aan het project dat wordt uitgevoerd, kunt u uw code testen, zoals u zou geïmplementeerde functie testen. Zie voor meer informatie [strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md). Wanneer in de foutopsporingsmodus wordt uitgevoerd, worden de onderbrekingspunten druk in Visual Studio zoals verwacht. 

Zie voor een voorbeeld van het testen van een wachtrij geactiveerd door de functie, de [wachtrij geactiveerd functie Quick Start-zelfstudie](functions-create-storage-queue-triggered-function.md#test-the-function).  

Zie voor meer informatie over het gebruik van de hulpprogramma's van Azure Functions Core, [Code en testen van Azure functions lokaal](functions-run-local.md).

## <a name="publish-to-azure"></a>Publiceren naar Azure

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="function-app-settings"></a>Instellingen voor functie-apps   

Alle instellingen die u hebt toegevoegd in de local.settings.json moeten ook worden toegevoegd aan de functie-app in Azure. Deze instellingen niet automatisch worden verzonden wanneer u het project publiceert. 

De eenvoudigste manier om de vereiste instellingen uploaden naar uw functie-app in Azure is met de **Toepassingsinstellingen beheren...**  koppeling die wordt weergegeven nadat u uw project is gepubliceerd. 

![](./media/functions-develop-vs/functions-vstools-app-settings.png)

U ziet nu de **toepassingsinstellingen** dialoogvenster voor de functie-app, kunt u toepassingsinstellingen voor nieuwe toevoegen of bestaande wijzigen.

![](./media/functions-develop-vs/functions-vstools-app-settings2.png)

U kunt ook de toepassingsinstellingen in een van deze andere manieren beheren:

* [Met de Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).
* [Met behulp van de `--publish-local-settings` optie publiceren in de hulpprogramma's van Azure Functions Core](functions-run-local.md#publish).
* [De Azure CLI gebruiken](/cli/azure/functionapp/config/appsettings#set). 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hulpprogramma's van Azure Functions, de sectie Veelgestelde vragen van de [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blogbericht.

Zie voor meer informatie over de hulpprogramma's van Azure Functions Core, [Code en testen van Azure functions lokaal](functions-run-local.md).  
Zie voor meer informatie over het ontwikkelen van functies als .NET-klassebibliotheken, [Azure Functions C# referentie voor ontwikkelaars](functions-dotnet-class-library.md). In dit onderwerp bevat ook koppelingen naar enkele voorbeelden van het gebruik van kenmerken voor de verschillende typen bindingen die worden ondersteund door Azure Functions declareren.    
