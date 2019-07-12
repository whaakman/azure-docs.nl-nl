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
ms.openlocfilehash: c05957cda16c96b841433483a90429aab2b4d22d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706509"
---
# <a name="resiliency-and-disaster-recovery"></a>Tolerantie en herstel na noodgevallen

Azure App-configuratie is momenteel een regionale service. Elke opslaan van de configuratie wordt gemaakt in een bepaald Azure-regio. Een regiobrede uitval is van invloed op alle winkels in die regio. App-configuratie biedt geen automatische failover naar een andere regio te bieden. In dit artikel bevat algemene richtlijnen voor hoe u meerdere archieven van de configuratie van Azure-regio's gebruiken kunt om te verhogen van de geo-tolerantie van uw toepassing.

## <a name="high-availability-architecture"></a>Architectuur voor hoge beschikbaarheid

Om te realiseren interregionale redundantie, moet u meerdere app-configuratie worden opgeslagen in verschillende regio's maken. Met deze instelling kunnen heeft uw toepassing ten minste één extra configuratiearchief terugvallen op als de primaire opslag niet meer toegankelijk. Het volgende diagram illustreert de topologie van uw toepassing en de configuratie van de primaire en secundaire stores:

![Geografisch redundante winkels](./media/geo-redundant-app-configuration-stores.png)

Uw toepassing wordt de configuratie van de primaire en secundaire winkels parallel geladen. Dit verhoogt de kans op succes aan de configuratiegegevens. U bent verantwoordelijk voor het bewaren van de gegevens in beide archieven gesynchroniseerd. De volgende secties wordt uitgelegd hoe kunt u geo-flexibiliteit inbouwen in uw toepassing.

## <a name="failover-between-configuration-stores"></a>Failover tussen configuratie

In technisch opzicht kunnen is niet een failover uitvoeren van uw toepassing. Deze probeert om op te halen van dezelfde set configuratiegegevens uit twee App-configuratie winkels tegelijkertijd. Rangschik uw code zodat worden geladen vanuit de secundaire store eerst en vervolgens de primaire opslaan. Deze aanpak zorgt ervoor dat de configuratiegegevens in de primaire opslag prioriteit heeft wanneer deze beschikbaar is. Het volgende codefragment toont hoe u deze benadering kunt implementeren in de .NET Core-CLI:

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

Het is belangrijk dat uw geografisch redundante configuratie van alle winkels dezelfde set gegevens hebben. U kunt de **exporteren** functie in de configuratie van de App gegevens van de primaire opslag kopiëren naar de secundaire on-demand. Deze functie is beschikbaar via de Azure-portal en de CLI.

Vanuit de Azure-portal, kunt u een wijziging pushen naar een andere opslaan van de configuratie door de volgende stappen.

1. Ga naar de **Import/Export** tabblad, en selecteer **exporteren** > **Appconfiguratie** > **doel**  >  **Selecteert u een resource**.

2. Geef in de nieuwe blade die wordt geopend, het abonnement, resourcegroep en resourcenaam van uw secundaire store, en selecteer vervolgens **toepassen**.

3. De gebruikersinterface wordt bijgewerkt, zodat u kunt kiezen welke configuratiegegevens die u wilt exporteren naar uw secundaire opslag. U kunt laat de standaardwaarde voor tijd is en stelt **van het label** en **label** op dezelfde waarde. Selecteer **Toepassen**.

4. Herhaal de vorige stappen voor alle wijzigingen in de configuratie.

Voor het automatiseren van dit proces exporteren, moet u de Azure CLI gebruiken. De volgende opdracht laat zien hoe een wijziging in de configuratie van één van de primaire opslag exporteren naar de secundaire server:

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u uw toepassing met het oog van geo-tolerantie tijdens runtime voor App-configuratie te verbeteren. U kunt ook configuratiegegevens van App-configuratie op build of implementatie moment insluiten. Zie voor meer informatie, [integreren met een CI/CD-pijplijn](./integrate-ci-cd-pipeline.md).

