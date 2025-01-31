---
title: Quickstart met Azure Application Insights | Microsoft Docs
description: Vindt u instructies voor het snel een ASP.NET Core Web-App instellen voor de bewaking met Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/26/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 931de532aa6e09b2cd00955df6ba1f05d7e4f42c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428499"
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>Beginnen met controleren van de ASP.NET Core-webtoepassing

Met Azure Application Insights kunt u eenvoudig de beschikbaarheid, de prestaties en het gebruik van een webtoepassing controleren. U kunt ook snel fouten in de toepassing identificeren en er een diagnose voor uitvoeren, zonder dat u hoeft te wachten totdat een gebruiker ze heeft gerapporteerd. 

Deze snelstartgids helpt u de Application Insights SDK toe te voegen aan een bestaande ASP.NET Core-webtoepassing. Voor meer informatie over het configureren van Application Insights zonder Visual Studio afhandeling dit [artikel](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core).

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze snelstart:

- [Installeer Visual Studio 2019](https://www.visualstudio.com/downloads/) met de volgende workloads:
  - ASP.NET-ontwikkeling en webontwikkeling
  - Azure-ontwikkeling
- [.NET Core 2.0 SDK installeren](https://www.microsoft.com/net/core)
- U hebt een Azure-abonnement en een bestaande .NET Core-webtoepassing nodig.

Als u een ASP.NET Core web-App niet hebt, kunt u onze stapsgewijze handleiding voor het [een ASP.NET Core-app maken en toevoegen van Application Insights.](../../azure-monitor/app/asp-net-core.md)

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="enable-application-insights"></a>Application Insights inschakelen

Met Application Insights kunnen telemetriegegevens worden verzameld vanuit elke toepassing met een internetverbinding, ongeacht of deze on-premises wordt uitgevoerd of in de cloud. Gebruik de volgende stappen om deze gegevens te bekijken.

1. Selecteer **Een resource maken** > **Hulpprogramma's voor ontwikkelaars** > **Application Insights**.

   > [!NOTE]
   >Als dit de eerste keer is het maken van een Application Insights-resource vindt u meer informatie in de [maken van een Application Insights-Resource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) doc-bestand.

    Er wordt een configuratiescherm weergegeven. Gebruik de volgende tabel om de invoervelden in te vullen.

   | Instellingen        |  Waarde           | Beschrijving  |
   | ------------- |:-------------|:-----|
   | **Name**      | Globaal unieke waarde | Naam die de app beschrijft die u wilt controleren |
   | **Resourcegroep**     | myResourceGroup      | Naam voor de nieuwe resourcegroep waarin App Insights-gegevens worden gehost |
   | **Location** | East US | Kies een locatie in uw buurt of in de buurt van waar de app wordt gehost |

2. Klik op **Create**.

## <a name="configure-app-insights-sdk"></a>App Insights-SDK configureren

1. Open het **project** uit ASP.NET Core-web-app in Visual Studio > Klik met de rechtermuisknop op de naam van de app in **Solution Explorer** > Selecteer **Toevoegen** > **Application Insights Telemetry**.

    ![Application Insights Telemetry toevoegen](./media/dotnetcore-quick-start/2vsaddappinsights.png)

2. Klik op de **aan de slag** knop

3. Selecteer uw account en abonnement > Selecteer de **bestaande resource** u hebt gemaakt in Azure portal > klikt u op **registreren**.

4. Selecteer **Foutopsporing** > **Starten zonder foutopsporing** (Ctrl + F5) om de app te starten

    ![Menu Overzicht van Application Insights](./media/dotnetcore-quick-start/3debug.png)

> [!NOTE]
> Het duurt 3-5 minuten voordat de gegevens worden weergegeven in de portal. Let op: als deze app een test-app met weinig verkeer is, worden de meeste metrische gegevens alleen vastgelegd bij actieve aanvragen en bewerkingen.

## <a name="start-monitoring-in-the-azure-portal"></a>Beginnen met controleren in Azure Portal

1. Open de Application Insights **overzicht** pagina in de Azure portal door **Start** en selecteer de resource die u eerder hebt gemaakt, voor meer informatie over uw actieve onder recente bronnen de toepassing.

   ![Menu Overzicht van Application Insights](./media/dotnetcore-quick-start/4overview.png)

2. Klik op **Toepassingsoverzicht** voor een visueel overzicht van de afhankelijkheidsrelaties tussen de onderdelen van de toepassing. Voor elk onderdeel worden KPI's weergegeven, zoals belasting, prestaties, fouten en waarschuwingen.

   ![Toepassingskaart](./media/dotnetcore-quick-start/5appmap.png)

3. Klik op de **Appanalyses** pictogram ![pictogram Toepassingskaart](./media/dotnetcore-quick-start/006.png) **weergeven in Analytics**. Hierdoor wordt **Application Insights Analytics** geopend. Dit biedt een querytaal met opmaak voor het analyseren van alle gegevens die zijn verzameld met Application Insights. In dit geval wordt er een query gegenereerd waarmee het aantal aanvragen wordt weergegeven als een grafiek. U kunt uw eigen query's schrijven om andere gegevens te analyseren.

   ![Analytics-grafiek met gebruikersaanvragen gedurende een tijdsperiode](./media/dotnetcore-quick-start/6analytics.png)

4. Ga terug naar de pagina **Overzicht** en bekijk de KPI-dashboards.  Dit dashboard biedt statistische gegevens over de toepassingsstatus, waaronder het aantal inkomende aanvragen, de duur van deze aanvragen en eventuele fouten die optreden. 

   ![Tijdlijngrafieken voor het Statusoverzicht](./media/dotnetcore-quick-start/7kpidashboards.png)

5. Op de linkermuisknop op **metrische gegevens**. U kunt de metrics explorer gebruiken als u voor het onderzoeken van de status en het gebruik van uw resource. U kunt op **Nieuwe grafiek toevoegen** klikken om aanvullende aangepaste weergaven te maken, of **Bewerken** selecteren om type, hoogte, kleurenpalet, groeperingen en metrische gegevens van bestaande grafieken te wijzigen. Bijvoorbeeld, kunt u een grafiek waarin de gemiddelde laadtijd van browserpagina 'Laadtijd van browserpagina' kiezen door in de vervolgkeuzelijst voor metrische gegevens en 'Gem.' van aggregatie wordt weergegeven. Voor meer informatie over Azure Metrics Explorer Bezoek [aan de slag met Azure Metrics Explorer](../../azure-monitor/platform/metrics-getting-started.md).

     ![Metrische gegevens op het tabblad: Grafiek met gemiddelde browser pagina laden](./media/dotnetcore-quick-start/8metrics.png)

## <a name="video"></a>Video

- Externe stapsgewijze video over [Application Insights configureren met .NET Core en Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) helemaal.
- Externe stapsgewijze video over [Application Insights configureren met .NET Core en Visual Studio Code](https://youtu.be/ygGt84GDync) helemaal.

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met testen, kunt u de resourcegroep verwijderen en alle gerelateerde resources. Om te dus de volgende stappen.

1. Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myResourceGroup**.
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myResourceGroup** in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Runtime-uitzonderingen zoeken en diagnoses uitvoeren](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-runtime-exceptions)
