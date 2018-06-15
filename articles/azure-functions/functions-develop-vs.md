---
title: Azure Functions met Visual Studio ontwikkelen | Microsoft Docs
description: Informatie over het ontwikkelen en testen van Azure Functions met behulp van Azure Functions-hulpprogramma's voor Visual Studio 2017.
services: functions
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: glenga
ms.openlocfilehash: 93d5883071a012842106bdd946e4f09a0d7aa751
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260451"
---
# <a name="azure-functions-tools-for-visual-studio"></a>Azure Functions-Tools voor Visual Studio  

Azure Functions-hulpprogramma's voor Visual Studio 2017 is een uitbreiding voor Visual Studio waarmee u ontwikkelen, testen en implementeren van C#-functies in Azure. Als deze ervaring uw eerste met Azure Functions is, kunt u meer informatie op [een inleiding tot Azure Functions](functions-overview.md).

De hulpprogramma's van Azure Functions biedt de volgende voordelen: 

* Bewerken, bouwen en uitvoeren van functies op de lokale computer. 
* Uw project Azure Functions rechtstreeks publiceren naar Azure. 
* WebJobs kenmerken gebruiken om te declareren functiebindingen rechtstreeks in de C#-code in plaats van het onderhouden van een afzonderlijke function.json voor het binden van definities.
* Ontwikkelen en implementeren van vooraf gecompileerde C#-functies. Vooraf gecompileerde functies bieden een betere koude start prestaties dan C# script gebaseerde functies. 
* Uw functies in C#-code terwijl alle voordelen van Visual Studio-ontwikkeling. 

In dit artikel laat zien hoe de Azure Functions-Tools voor Visual Studio 2017 gebruiken voor het ontwikkelen van uw functies in C#. U leert ook hoe uw project publiceren naar Azure als een .NET-assembly.

> [!IMPORTANT]
> Combineer geen lokale ontwikkeling met portal-ontwikkeling in dezelfde functie-app. Wanneer u vanaf een lokaal project naar een functie-app publiceert, overschrijft het implementatieproces kunnen functies die u hebt ontwikkeld in de portal.

## <a name="prerequisites"></a>Vereisten

Azure Functions-hulpprogramma's is opgenomen in de werklast Azure ontwikkeling van [Visual Studio 2017 versie 15,5](https://www.visualstudio.com/vs/), of een latere versie. Zorg ervoor dat u de **ontwikkelen van Azure** werkbelasting in de installatie van Visual Studio 2017:

![Visual Studio 2017 installeren met de Azure-ontwikkelworkload](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Zorg ervoor dat uw Visual Studio up-to-date is en dat u gebruikmaakt van de [meest recente versie](#check-your-tools-version) van de Azure Functions-hulpprogramma's.

### <a name="other-requirements"></a>Overige vereisten

Als u wilt maken en implementeren van functies, moet u ook:

* Een actief Azure-abonnement. Als u een Azure-abonnement geen [gratis accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) beschikbaar zijn.

* Een Azure Storage-account. Zie het maken van een opslagaccount [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account).

### <a name="check-your-tools-version"></a>Controleer uw versie van de hulpprogramma 's

1. Van de **extra** menu kiezen **uitbreidingen en Updates**. Vouw **geïnstalleerde** > **extra** en kies **Azure-functies en hulpprogramma's voor Web-taken**.

    ![Controleer of de versie van de hulpprogramma's voor functies](./media/functions-develop-vs/functions-vstools-check-functions-tools.png)

2. Houd er rekening mee de geïnstalleerde **versie**. U kunt deze versie vergelijken met de meest recente versie vermeld [in de release-opmerkingen](https://github.com/Azure/Azure-Functions/blob/master/VS-AzureTools-ReleaseNotes.md). 

3. Als uw versie ouder, de hulpprogramma's in Visual Studio bijwerken zoals weergegeven in de volgende sectie.

### <a name="update-your-tools"></a>De hulpprogramma's bijwerken

1. In de **uitbreidingen en Updates** dialoogvenster Vouw **Updates** > **Visual Studio Marketplace**, kies **Azure-functies en hulpprogramma's voor Web-taken**  en selecteer **Update**.

    ![Werk de versie van de hulpprogramma's voor functies](./media/functions-develop-vs/functions-vstools-update-functions-tools.png)   

2. Nadat de hulpprogramma's-update is gedownload, sluit u Visual Studio die de hulpprogramma's bijwerken met behulp van het installatieprogramma VSIX activeren.

3. Kies in het installatieprogramma **OK** starten en vervolgens **wijzigen** bijwerken van de hulpprogramma's. 

4. Nadat de update voltooid is, kiest u **sluiten** en start Visual Studio opnieuw.

## <a name="create-an-azure-functions-project"></a>Een Azure Functions-project maken

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]

De projectsjabloon maakt een C#-project, installeert de `Microsoft.NET.Sdk.Functions` NuGet-pakket en stelt het doel-framework. Functies 1.x doelen .NET Framework en 2.x doelen .NET Standard fungeert. Het nieuwe project heeft de volgende bestanden:

* **host.JSON**: Hiermee kunt u de functies host configureren. Deze instellingen gelden beide wanneer lokaal en in Azure wordt uitgevoerd. Zie voor meer informatie [host.json verwijzing](functions-host-json.md).

* **Local.Settings.JSON**: onderhoudt instellingen bij lokale uitvoering van de functies die worden gebruikt. Deze instellingen worden niet gebruikt door Azure, ze worden gebruikt door de [kernonderdelen van Azure Functions](functions-run-local.md). Dit bestand gebruiken om op te geven van app-instellingen voor variabelen die worden vereist door uw functies. Een nieuw item toevoegen aan de **waarden** matrix voor elke verbinding vereist door de bindingen van de functies in uw project. Zie voor meer informatie [lokale instellingenbestand](functions-run-local.md#local-settings-file) in hulpprogramma's voor Azure Functions Core artikel.

Zie voor meer informatie [functies class library-project](functions-dotnet-class-library.md#functions-class-library-project).

## <a name="configure-the-project-for-local-development"></a>Het project voor lokale ontwikkeling configureren

De runtime van Functions wordt intern gebruikt een Azure Storage-account. Voor alle typen dan HTTP en webhooks activeert, moet u instellen de **Values.AzureWebJobsStorage** sleutel op een geldige verbindingsreeks voor Azure Storage-account. Functie-app kunt ook de [Azure-opslagemulator](../storage/common/storage-use-emulator.md) voor de **AzureWebJobsStorage** verbinding instellingen vereist voor het project. Stel de waarde van voor het gebruik van de emulator **AzureWebJobsStorage** naar `UseDevelopmentStorage=true`. U moet deze instelling in een werkelijke opslagverbinding vóór de implementatie wijzigen.

De verbindingsreeks voor opslag account instellen:

1. Open in Visual Studio **Cloud Explorer**, vouw **Opslagaccount** > **uw Opslagaccount**, selecteer daarna **eigenschappen**en kopieer de **primaire verbindingsreeks** waarde.

2. Open het bestand local.settings.json in uw project en stel de waarde van de **AzureWebJobsStorage** sleutel op de verbindingsreeks die u hebt gekopieerd.

3. Herhaal de vorige stap om toe te voegen unieke sleutels zijn aan de **waarden** matrix voor eventuele andere verbindingen die vereist zijn voor uw functies.

## <a name="create-a-function"></a>Een functie maken

In de vooraf gecompileerde functies worden de bindingen die wordt gebruikt door de functie gedefinieerd door het toepassen van kenmerken in de code. Wanneer u de hulpprogramma's van Azure Functions voor het maken van uw functies van de opgegeven sjablonen gebruikt, worden deze kenmerken worden toegepast voor u. 

1. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer  > **Nieuw item****Toevoegen**. Selecteer **Azure-functie**, typ een **naam** voor de klasse en klik op **toevoegen**.

2. Kies de trigger, stelt u de bindingeigenschappen en klikt u op **maken**. Het volgende voorbeeld ziet de instellingen wanneer de functie voor het maken van een Queue storage geactiveerd. 

    ![Maak een functie van de wachtrij is geactiveerd](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)

    In dit voorbeeld trigger wordt een verbindingsreeks met een sleutel met de naam **QueueStorage**. Deze instelling van de tekenreeks moet worden gedefinieerd in de [local.settings.json bestand](functions-run-local.md#local-settings-file).

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
    
Toevoegen van meer functies aan uw app-project voor de functie kunt u de bovenstaande procedure. Elke functie in het project een andere trigger kan hebben, maar een functie moet exact één trigger hebben. Zie voor meer informatie [Azure Functions triggers en bindingen concepten](functions-triggers-bindings.md).

## <a name="add-bindings"></a>Bindingen toevoegen

Net als bij triggers, invoer en uitvoer bindingen toegevoegd aan de functie als binding kenmerken. Bindingen aan een functie als volgt toevoegen:

1. Zorg ervoor dat u hebt [geconfigureerd van het project voor lokale ontwikkeling](#configure-the-project-for-local-development).

2. Het juiste NuGet-extensie-pakket voor de specifieke binding toevoegen. Zie voor meer informatie [lokale C#-ontwikkeling met behulp van Visual Studio](functions-triggers-bindings.md#local-csharp) in het artikel Triggers en bindingen. De binding-specifieke NuGet-pakketvereisten zijn gevonden in het verwijzingsartikel voor de binding. Bijvoorbeeld zoeken naar pakketvereisten voor de trigger Event Hubs in de [Event Hubs binding-verwijzingsartikel](functions-bindings-event-hubs.md).

3. Als er appinstellingen die u de binding moet, voeg deze toe aan de **waarden** verzameling in de [lokale instellingenbestand](functions-run-local.md#local-settings-file). Deze waarden worden gebruikt wanneer de functie wordt lokaal uitgevoerd. Als de functie wordt uitgevoerd in de functie-app in Azure, de [werken app-instellingen](#function-app-settings) worden gebruikt.

4. De juiste binding-kenmerk toevoegen aan de methodehandtekening. In het volgende voorbeeld wordt een wachtrijbericht activeert de functie en de uitvoer-binding maakt een nieuw wachtrijbericht met dezelfde tekst in een andere wachtrij.

    ```csharp
    public static class SimpleExampleWithOutput
    {
        [FunctionName("CopyQueueMessage")]
        public static void Run(
            [QueueTrigger("myqueue-items-source", Connection = "AzureWebJobsStorage")] string myQueueItem, 
            [Queue("myqueue-items-destination", Connection = "AzureWebJobsStorage")] out string myQueueItemCopy,
            TraceWriter log)
        {
            log.Info($"CopyQueueMessage function processed: {myQueueItem}");
            myQueueItemCopy = myQueueItem;
        }
    }
    ```
De verbinding met Queue storage wordt opgehaald uit de `AzureWebJobsStorage` instelling. Zie voor meer informatie het verwijzingsartikel voor de specifieke binding. 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

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
* [De Azure CLI gebruiken](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set). 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hulpprogramma's van Azure Functions, de sectie Veelgestelde vragen van de [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blogbericht.

Zie voor meer informatie over de hulpprogramma's van Azure Functions Core, [Code en testen van Azure functions lokaal](functions-run-local.md).

Zie voor meer informatie over het ontwikkelen van functies als .NET-klassebibliotheken, [Azure Functions C# referentie voor ontwikkelaars](functions-dotnet-class-library.md). In dit artikel bevat ook koppelingen naar enkele voorbeelden van het gebruik van kenmerken voor de verschillende typen bindingen die worden ondersteund door Azure Functions declareren.    
