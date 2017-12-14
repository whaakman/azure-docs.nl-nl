---
title: Snelstartgids met Azure Application Insights | Microsoft Docs
description: Biedt instructies om snel een Java-web-app in te stellen om te controleren met Application Insights
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/10/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 9246def86fa647213aa3ec12427d829c24fa8034
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="start-monitoring-your-java-web-application"></a>Beginnen met controleren van de Java-webtoepassing

Met Azure Application Insights kunt u eenvoudig de beschikbaarheid, de prestaties en het gebruik van een webtoepassing controleren. U kunt ook snel fouten in de toepassing identificeren en er een diagnose voor uitvoeren, zonder dat u hoeft te wachten totdat een gebruiker ze heeft gerapporteerd. Met de Application Insights Java SDK kunt u de algemene pakketten van derden controleren, inclusief MongoDB, MySQL en Redis.

Deze snelstartgids helpt u de Application Insights SDK toe te voegen aan een bestaand Java Dynamic-webproject.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart:

- Oracle JRE 1.6 of later, of Zulu JRE 1.6 of later installeren
- [Gratis Eclipse IDE voor Java EE-ontwikkelaars](http://www.eclipse.org/downloads/) installeren. In deze snelstartgids wordt gebruikgemaakt van Eclipse Oxygen (4.7)
- U hebt een Azure-abonnement en een bestaand Java Dynamic-webproject nodig
 
Als u geen Java Dynamic-webproject hebt, kunt u er een maken met behulp van de [snelstartgids Een Java-web-app maken](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-java).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="enable-application-insights"></a>Application Insights inschakelen

Met Application Insights kunnen telemetriegegevens worden verzameld vanuit elke toepassing met een internetverbinding, ongeacht of deze on-premises wordt uitgevoerd of in de cloud. Gebruik de volgende stappen om deze gegevens te bekijken.

1. Selecteer **Nieuw** > **Controle + Beheer** > **Application Insights**.

   ![Application Insights-resource toevoegen](./media/app-insights-java-quick-start/001-j.png)

   Er wordt een configuratiescherm weergegeven. Gebruik de onderstaande tabel om de invoervelden in te vullen.

    | Instellingen        | Waarde           | Beschrijving  |
   | ------------- |:-------------|:-----|
   | **Naam**      | Globaal unieke waarde | Naam die de app beschrijft die u wilt controleren |
   | **Toepassingstype** | Java-webtoepassing | Type app dat u wilt controleren |
   | **Resourcegroep**     | myResourceGroup      | Naam voor de nieuwe resourcegroep waarin App Insights-gegevens worden gehost |
   | **Locatie** | VS - oost | Kies een locatie in uw buurt of in de buurt van waar de app wordt gehost |

2. Klik op **Create**.

## <a name="install-app-insights-plugin"></a>App Insights-invoegtoepassing installeren

1. Start **Eclipse** > Klik op **Help** > Selecteer **Nieuwe software installeren**.

   ![Nieuw App Insights-resourceformulier](./media/app-insights-java-quick-start/000-j.png)

2. Kopieer ```http://dl.microsoft.com/eclipse``` in het veld Werken met > Schakel **Azure-toolkit voor Java** in > Selecteer **Application Insights-invoegtoepassing voor Java** > **Schakel** de optie uit voor het contact opnemen met alle updatesites tijdens de installatie om de vereiste software te vinden.

3. Zodra de installatie is voltooid, wordt u gevraagd om **Eclipse opnieuw te starten**.

## <a name="configure-app-insights-plugin"></a>App Insights-invoegtoepassing configureren

1. Start **Eclipse** > Open uw **project** > Klik met de rechtermuisknop op de projectnaam in **Projectverkenner** > Selecteer **Azure** > Klik op **Aanmelden**.

2. Selecteer de verificatiemethode **Interactief** > Klik op **Aanmelden** > Voer uw **Azure-referenties** in wanneer u hierom wordt gevraagd > Selecteer uw **Azure-abonnement**.

3. Klik met de rechtermuisknop op de naam van uw project in **Projectverkenner** > Selecteer **Azure** > Klik op **Application Insights configureren**.

4. Schakel **Telemetrie met Application Insights inschakelen** in > Selecteer de App Insights-resource en bijbehorende **Instrumentatiesleutel** die u wilt koppelen aan de Java-app.

   ![Menu Eclipse Azure Config](./media/app-insights-java-quick-start/0007-j.png)

> [!NOTE]
> Met de Application Insights SDK voor Java kunt u live metrische gegevens vastleggen en visualiseren. Als u het verzamelen van telemetriegegevens de eerste keer inschakelt, kan het echter enkele minuten duren voordat deze worden weergegeven in de portal. Let op: als deze app een test-app met weinig verkeer is, worden de meeste metrische gegevens alleen vastgelegd bij actieve aanvragen en bewerkingen.

## <a name="start-monitoring-in-the-azure-portal"></a>Beginnen met controleren in Azure Portal

1. U kunt de pagina **Overzicht** van Application Insights in Azure Portal, waar u de instrumentatiesleutel hebt opgehaald, nu opnieuw openen om de details te bekijken van de toepassing die momenteel wordt uitgevoerd.

   ![Menu Overzicht van Application Insights](./media/app-insights-java-quick-start/0008-j.png)

2. Klik op **App-kaart** voor een visueel overzicht van de afhankelijkheidsrelaties tussen de onderdelen van de toepassing. Voor elk onderdeel worden KPI's weergegeven, zoals belasting, prestaties, fouten en waarschuwingen.

   ![Toepassingskaart](./media/app-insights-java-quick-start/005-j.png)

3. Klik op het **App Analytics**-pictogram ![Pictogram Toepassingskaart](./media/app-insights-java-quick-start/006.png). Hierdoor wordt **Application Insights Analytics** geopend. Dit biedt een querytaal met opmaak voor het analyseren van alle gegevens die zijn verzameld met Application Insights. In dit geval wordt er een query gegenereerd waarmee het aantal aanvragen wordt weergegeven als een grafiek. U kunt uw eigen query's schrijven om andere gegevens te analyseren.

   ![Analytics-grafiek met gebruikersaanvragen gedurende een tijdsperiode](./media/app-insights-java-quick-start/0010-j.png)

4. Ga terug naar de pagina **Overzicht** en bekijk de **tijdlijn voor het Statusoverzicht**.  Dit dashboard biedt statistische gegevens over de toepassingsstatus, waaronder het aantal inkomende aanvragen, de duur van deze aanvragen en eventuele fouten die optreden.

   ![Tijdlijngrafieken voor het Statusoverzicht](./media/app-insights-java-quick-start/0009-j.png)

   Als u de grafiek **Laadtijd voor paginaweergave** wilt vullen met **telemetriegegevens aan de clientzijde**, voegt u dit script toe aan elke pagina die u wilt bijhouden:

   ```HTML
   <!-- 
   To collect end-user usage analytics about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
     function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
    }({
        instrumentationKey:"<instrumentation key>"
    });

    window.appInsights=appInsights;
    appInsights.trackPageView();
   </script>
    ```

5. Klik op **Live Stream**. Hier vindt u live metrische gegevens die betrekking hebben op de prestaties van de Java-web-app. **Live Metrics Stream** bevat gegevens die betrekking hebben op het aantal inkomende aanvragen, de duur van deze aanvragen en eventuele fouten die optreden. U kunt ook in realtime de kritieke prestatiegegevens controleren, zoals de processor en het geheugen.

   ![Grafieken voor metrische servergegevens](./media/app-insights-java-quick-start/livemetricsjava.png)

Bekijk de [extra Java-documentatie voor App Insights](.\app-insights-java-get-started.md) voor meer informatie over het controleren van Java.

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om door te gaan met andere snelstartgidsen of met de zelfstudies, verwijdert u de resources die u in deze snelstartgids hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die door deze Quick Start in Azure Portal zijn gemaakt.

1. Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myResourceGroup**.
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myResourceGroup** in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Prestatieproblemen zoeken en diagnoses uitvoeren](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)