---
title: Azure-services en toepassingen met behulp van Grafana bewaken
description: Route Azure Monitor en Application Insights gegevens, zodat u ze in Grafana bekijken kunt.
services: azure-monitor
keywords: ''
author: rboucher
ms.author: robb
ms.date: 11/06/2017
ms.topic: conceptual
ms.service: azure-monitor
ms.component: ''
ms.openlocfilehash: b3b4b1f637eae87e9b21c81cf645c642d2043d4c
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821885"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Bewaken van uw Azure-services in Grafana
U kunt nu ook bewaken Azure-services en toepassingen van [Grafana](https://grafana.com/) met behulp van de [Azure Monitor-gegevensbron invoegtoepassing](https://grafana.com/plugins/grafana-azure-monitor-datasource). De invoegtoepassing worden verzameld voor prestatiegegevens van toepassingen die door de Application Insights-SDK worden verzameld, evenals infrastructuur gegevens geleverd door Azure Monitor. U kunt deze gegevens vervolgens weergeven in het Grafana-dashboard.

De invoegtoepassing is momenteel in preview.

Gebruik de volgende stappen uit het instellen van een server Grafana uit Azure Marketplace en dashboards bouwen om de metrische gegevens van Azure Monitor en Application Insights.

## <a name="set-up-a-grafana-instance"></a>Instellen van een exemplaar van Grafana
1. Ga naar de Azure Marketplace en kies Grafana door Grafana Labs.

2. Vul in de namen en gegevens. Maak een nieuwe resourcegroep. De waarden die u voor de gebruikersnaam van de virtuele machine, VM-wachtwoord en het beheerderswachtwoord server Grafana kiest van bijhouden.  

3. VM-grootte en een opslagaccount kiezen.

4. De netwerk-instellingen configureren.

5. De samenvatting weergeven en selecteren **maken** na de gebruiksvoorwaarden accepteren.

## <a name="log-in-to-grafana"></a>Meld u aan bij Grafana
1. Nadat de implementatie is voltooid, selecteert u **gaat u naar de resourcegroep**. U ziet een lijst met nieuwe resources.

    ![Grafana resource groepsobjecten](media/monitor-send-to-grafana/grafana1.png)

    Als u de netwerkbeveiligingsgroep selecteert (*grafana-nsg* in dit geval), ziet u dat poort 3000 wordt gebruikt voor toegang tot Grafana-server.

2. Ga terug naar de lijst met resources en selecteer **openbaar IP-adres**. Met behulp van de waarden in dit scherm, typt u *http://<IP address>: 3000* of de  *<DNSName>: 3000* in uw browser. U ziet een aanmeldingspagina voor de Grafana-server die u net hebt gemaakt.

    ![Aanmeldingsscherm van Grafana](media/monitor-send-to-grafana/grafana2.png)

3. Meld u aan met de naam van de gebruiker als *admin* en Grafana wachtwoord van de serverbeheerder u eerder hebt gemaakt.

## <a name="configure-data-source-plugin"></a>Data source-invoegtoepassing configureren

Wanneer is aangemeld, ziet u de Azure Monitor data source-invoegtoepassing is al opgenomen.

![Grafana geeft Azure Monitor-invoegtoepassing](media/monitor-send-to-grafana/grafana3.png)

1. Selecteer **gegevensbron toevoegen** om Azure Monitor en Application Insights te configureren.

2. Kies een naam voor de gegevensbron en selecteer **Azure Monitor** als de gegevensbron in de vervolgkeuzelijst.


## <a name="create-a-service-principal"></a>Een service-principal maken

Grafana maakt gebruik van een Azure Active Directory-service-principal verbinding maken met Azure Monitor-API's en metrische gegevens verzamelen. U moet een service-principal voor het beheren van toegang tot uw Azure-resources maken.

1. Zie [deze instructies](../active-directory/develop/howto-create-service-principal-portal.md) om een serviceprincipal te maken. Kopiëren en opslaan van uw tenant-ID, client-ID en een clientgeheim in.

2. Zie [toepassing toewijzen aan rol](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) de rol van lezer toewijzen aan de Azure Active Directory-toepassing.     

3. Als u Application Insights gebruikt, kunt u ook uw Application Insights-API en de toepassings-ID voor het verzamelen van Application Insights op basis van metrische gegevens opnemen. Zie voor meer informatie, [aan uw API-sleutel en de toepassings-ID](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

4. Nadat u alle gegevens hebt ingevoerd, selecteert u **opslaan** en Grafana test de API. U ziet een bericht dat lijkt op de volgende uitvoer.  

    ![Grafana geeft Azure Monitor-invoegtoepassing](media/monitor-send-to-grafana/grafana4-1.png)

> [!NOTE]
> Tijdens het configureren van de invoegtoepassing kunt u aangeven welke Azure-Cloud (openbaar, Azure US Government, Azure Duitsland of Azure China) u wilt dat de invoegtoepassing moet worden geconfigureerd op basis van.
>
>

## <a name="build-a-grafana-dashboard"></a>Bouw een Grafana-dashboard

1. Ga naar de startpagina en selecteer **nieuw Dashboard**.

2. Selecteer in het nieuwe dashboard, de **Graph**. U kunt experimenteren met andere grafieken opties, maar in dit artikel wordt gebruikgemaakt van *Graph* als voorbeeld.

    ![Nieuwe Grafana-Dashboard](media/monitor-send-to-grafana/grafana5.png)

3. Een lege grafiek wordt weergegeven op uw dashboard.

4. Klik op de titelbalk van het paneel en selecteer **bewerken** in te voeren van de details van de gegevens die u wilt in deze grafiek wordt getekend.

5. Nadat u de juiste virtuele machines hebt geselecteerd, kunt u de metrische gegevens weergeven in het dashboard kunt starten.

Hieronder volgt een eenvoudig dashboard met twee grafieken. De naam van links ziet u het CPU-percentage van de twee virtuele machines. De grafiek aan de rechterkant ziet u de transacties in een Azure Storage-account onderverdeeld op basis van de transactie-API-type.

![Grafana twee grafieken voorbeeld](media/monitor-send-to-grafana/grafana6.png)


## <a name="optional-create-dashboard-playlists"></a>Optioneel: Dashboard afspeellijsten maken

Een van de vele handige voorzieningen van Grafana is de dashboard-afspeellijst. U kunt meerdere dashboards maken en deze toevoegen aan een afspeellijst vindt bij het configureren van een interval voor elk dashboard om weer te geven. Selecteer **afspelen** om de dashboards bladeren door te geven. Kunt u ze aan een grote wall-monitor voor een bord' status' voor uw groep wilt weergeven.

![Voorbeeld van de afspeellijst Grafana](media/monitor-send-to-grafana/grafana7.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>Optioneel: Het bewaken van uw aangepaste metrische gegevens op dezelfde server Grafana

U kunt ook installeren Telegraf en InfluxDB te verzamelen en zowel aangepaste als door de agent op basis van metrische gegevens in dezelfde instantie Grafana tekenen. Er zijn veel data source-invoegtoepassingen die u kunt deze metrische gegevens om samen te brengen in een dashboard.

U kunt deze instellingen om op te nemen van metrische gegevens van uw server Prometheus ook hergebruiken. Gebruik de Prometheus data source-invoegtoepassing in het Grafana-invoegtoepassing galerie.

Hier vindt u referentiemateriaal artikelen over het gebruik van Telegraf, InfluxDB Prometheus en Docker
 - [Metrische Systeemmeetgegevens met de MAATSTREEPJES-Stack op Ubuntu 16.04 controleren](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Metrische gegevens voor Docker resources met Grafana, InfluxDB en Telegraf controleren](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/)

 - [Een oplossing voor bewaking voor Docker-hosts, containers en services in containers](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Hier volgt een afbeelding van een volledige Grafana-dashboard met metrische gegevens van Azure Monitor en Application Insights.
![Voorbeeld van Grafana metrische gegevens](media/monitor-send-to-grafana/grafana8.png)


## <a name="clean-up-resources"></a>Resources opschonen

De kosten worden berekend wanneer er virtuele machines worden uitgevoerd, ongeacht of u er wel of niet. Om te voorkomen waarvoor extra kosten worden berekend, opschonen van de resourcegroep in dit artikel hebt gemaakt.

1. Klik in het menu links in Azure portal op **resourcegroepen** en klik vervolgens op **Grafana**.
2. Klik op de pagina van uw resourcegroep **verwijderen**, type **Grafana** in het tekstvak in en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Monitor Metrics](../azure-monitor/platform/data-collection.md)
