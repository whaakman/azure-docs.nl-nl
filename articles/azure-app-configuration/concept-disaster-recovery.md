---
title: Azure App-configuratie tolerantie en herstel na noodgevallen | Microsoft Docs
description: Een overzicht van het implementeren van tolerantie en herstel na noodgevallen met Azure App-configuratie.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 05/29/2019
ms.author: yegu
ms.openlocfilehash: 39e7a62899a7d1d6feb5bfd3b45ad4adc3c996a0
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394025"
---
# <a name="resiliency-and-disaster-recovery"></a>Tolerantie en herstel na noodgevallen

Azure App-configuratie is momenteel een regionale service. Elke opslaan van de configuratie wordt gemaakt in een bepaald Azure-regio. Een regiobrede uitval is van invloed op alle winkels in die regio. App-configuratie biedt geen automatische failover naar een andere regio te bieden. In dit artikel bevat algemene richtlijnen voor hoe u meerdere archieven van de configuratie van Azure-regio's gebruiken kunt om te verhogen van de geo-tolerantie van uw toepassing.

## <a name="high-availability-architecture"></a>Architectuur voor hoge beschikbaarheid

Om te realiseren interregionale redundantie, moet u meerdere app-configuratie worden opgeslagen in verschillende regio's maken. Met deze instelling kunnen uw toepassing heeft ten minste één extra configuratiearchief terugvallen op met de primaire opslag niet meer toegankelijk. Hieronder vindt u een diagram waarin de topologie van uw toepassing en de configuratie van de primaire en secundaire winkels.

![Geografisch redundante winkels](./media/geo-redundant-app-configuration-stores.png)

Uw toepassing wordt de configuratie van de primaire en secundaire winkels parallel laden. Dit verhoogt de kans dat het is sterk aan de configuratiegegevens. U bent verantwoordelijk voor het bewaren van de gegevens in beide archieven gesynchroniseerd. De volgende secties wordt uitgelegd hoe kunt u geo-flexibiliteit inbouwen in uw toepassing.

## <a name="failover-between-configuration-stores"></a>Failover tussen configuratie

Technisch uw toepassing is niet een failover wordt uitgevoerd. Deze probeert om op te halen van dezelfde set configuratiegegevens in twee app-configuratie stores tegelijkertijd. U moet uw code rangschikken zodanig dat deze wordt geladen eerst uit de secundaire store eerst en vervolgens de primaire opslaan. Deze aanpak zorgt ervoor dat de configuratiegegevens in de primaire opslag voorrang krijgt wanneer ze beschikbaar zijn. Het onderstaande codefragment laat zien hoe u deze kunt implementeren in .NET Core.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    }
```

U ziet dat de `optional` parameter doorgegeven aan de `AddAzureAppConfiguration` functie. Als de waarde `true`, deze parameter wordt voorkomen dat de toepassing niet kan worden voortgezet als de functie configuratiegegevens niet laden.

## <a name="synchronization-between-configuration-stores"></a>Synchronisatie tussen configuratie

Het is belangrijk dat uw geografisch redundante configuratie van alle winkels dezelfde set gegevens hebben. U kunt de **exporteren** functie in de configuratie van de App gegevens van de primaire opslag kopiëren naar het secundaire on-demand. Deze functie is beschikbaar via de Azure-portal en de CLI.

Vanuit de Azure-portal, kunt u een wijziging pushen naar een andere opslaan van de configuratie door de volgende stappen:

1. Navigeer naar **Import/Export** tabblad **exporteren**, selecteer **App-configuratie** als de **doel** service, klikt u op  **Selecteer een resource**.

2. In de nieuwe blade die is geopend omhoog, geef het abonnement, resourcegroep en resourcenaam van uw secundaire store en klik vervolgens op **toepassen**.

3. De gebruikersinterface worden bijgewerkt zodat u kunt kiezen welke configuratiegegevens die u wilt exporteren naar uw secundaire opslag. U kunt laat de standaardwaarde voor tijd is en stelt **van het label** en **Label** op dezelfde waarde. Klik op **Toepassen**.

4. Herhaal de bovenstaande stappen voor alle wijzigingen in de configuratie.

U kunt automatiseren dit proces met de Azure CLI te exporteren. De onderstaande opdracht laat zien hoe een wijziging in de configuratie van één van de primaire opslag exporteren naar de secundaire server.

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u uw toepassing met het oog van geo-tolerantie tijdens runtime voor App-configuratie te verbeteren. U kunt ook configuratiegegevens van App-configuratie insluiten build of implementatie tegelijk. Zie voor meer informatie, [integreren met een CI/CD-pijplijn](./integrate-ci-cd-pipeline.md).

