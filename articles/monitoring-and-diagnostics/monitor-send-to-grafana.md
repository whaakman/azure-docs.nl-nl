---
title: Azure-services en toepassingen die gebruikmaken van Grafana bewaken | Microsoft Docs
description: Route Azure Monitor en Application Insights gegevens zodat u deze in Grafana bekijken kunt.
services: monitoring-and-diagnostics
keywords: 
author: rboucher
ms.author: robb
ms.date: 11/06/2017
ms.topic: article
ms.service: monitoring-and-diagnostics
ms.openlocfilehash: c189e67c481239a8a68f2e2b30d05bb615cfa24e
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="monitor-your-azure-services-in-grafana"></a>Uw Azure-services in Grafana bewaken
U kunt nu ook bewaken Azure-services en toepassingen van [Grafana](https://grafana.com/) met behulp van de [Azure Monitor gegevensbron invoegtoepassing](https://grafana.com/plugins/grafana-azure-monitor-datasource). De invoegtoepassing verzamelt prestatiegegevens van toepassingen die door de Application Insights-SDK worden verzameld, evenals infrastructuur gegevens die zijn verstrekt door Azure bewaken. U kunt vervolgens deze gegevens op uw dashboard Grafana weergeven.

De invoegtoepassing is momenteel in preview.

Gebruik de volgende stappen op een server Grafana uit Azure Marketplace instellen en het bouwen van dashboards voor metrische gegevens van Azure Monitor en Application Insights.

## <a name="set-up-a-grafana-instance"></a>Een exemplaar Grafana instellen
1. Ga naar Azure Marketplace en kies Grafana door Grafana Labs.

2. Vul in de namen en gegevens. Maak een nieuwe resourcegroep. Bijhouden van de waarden die u voor de gebruikersnaam van de virtuele machine, VM-wachtwoord en beheerderswachtwoord Grafana-server kiest.  

3. VM-grootte en een opslagaccount kiezen.

4. Configureer de netwerkinstellingen van de configuratie.

5. De samenvatting weergeven en selecteren **maken** na de gebruiksvoorwaarden accepteren.

## <a name="log-in-to-grafana"></a>Aanmelden bij Grafana
1. Nadat de implementatie is voltooid, selecteert u **gaat u naar de resourcegroep**. U ziet een lijst met nieuwe resources. 

    ![Grafana resource groepsobjecten](.\media\monitor-how-to-grafana\grafana1.png) 

    Als u de netwerkbeveiligingsgroep selecteert (*grafana nsg* in dit geval), ziet u dat 3000-poort wordt gebruikt voor toegang tot Grafana-server. 

2. Ga terug naar de lijst van resources en selecteer **openbaar IP-adres**. Gebruik de waarden die op dit scherm en typ *http://<IP address>: 3000* of de  *<DNSName>: 3000* in uw browser. U ziet een aanmeldingspagina voor de Grafana-server die u zojuist hebt gemaakt.
    
    ![Grafana aanmeldingsscherm](.\media\monitor-how-to-grafana\grafana2.png) 

3. Meld u aan met de naam van de gebruiker als *admin* en de Grafana server-beheerderswachtwoord dat u eerder hebt gemaakt. 

## <a name="configure-data-source-plugin"></a>Data source-invoegtoepassing configureren

Zodra het is aangemeld, ziet u de bron-invoegtoepassing van Azure Monitor gegevens is al toegevoegd.

![Grafana toont Azure Monitor-invoegtoepassing](.\media\monitor-how-to-grafana\grafana3.png) 

1. Selecteer **gegevensbron toevoegen** Azure Monitor en Application Insights configureren. 
    
2. Kies een naam voor de gegevensbron en selecteer **Azure Monitor** als de gegevensbron uit de vervolgkeuzelijst.
    
    
## <a name="create-a-service-principal"></a>Een service-principal maken 

Grafana maakt gebruik van een Azure Active Directory-service-principal verbinding maken met Azure Monitor-API's en metrische gegevens te verzamelen. U moet een service principal voor het beheren van toegang tot uw Azure-resources maken.

1. Zie [deze instructies](../azure-resource-manager/resource-group-create-service-principal-portal.md) voor het maken van een service-principal. Kopiëren en opslaan van uw tenant-ID, client-ID en een clientgeheim.

2. Zie [toepassing toewijzen aan rol](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) met de lezersrol toewijzen aan de Azure Active Directory-toepassing.     

3. Als u Application Insights gebruikt, kunt u ook uw Application Insights-API en de toepassings-ID voor het verzamelen van Application Insights op basis van metrische gegevens opnemen. Zie voor meer informatie [ophalen van uw API-sleutel en de toepassings-ID](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

4. Nadat u alle gegevens hebt ingevoerd, selecteert u **opslaan** en Grafana test de API. U ziet een bericht dat lijkt op de volgende.  

    ![Grafana toont Azure Monitor-invoegtoepassing](.\media\monitor-how-to-grafana\grafana4.png) 
    
## <a name="build-a-grafana-dashboard"></a>Een dashboard Grafana bouwen

1. Ga naar de startpagina en selecteer **nieuwe Dashboard**.

2. Selecteer in het nieuwe dashboard de **grafiek**. U kunt andere opties voor grafieken, maar in dit artikel gebruikt *grafiek* als voorbeeld. 

    ![Nieuwe Dashboard Grafana](.\media\monitor-how-to-grafana\grafana5.png) 

3. Een lege grafiek wordt weergegeven op uw dashboard. 

4. Klik op de titelbalk van het paneel en selecteer **bewerken** invoeren van de details van de gegevens die u wilt weergeven in deze grafiek.
    
5. Wanneer u de juiste virtuele machines hebt geselecteerd, kunt u beginnen met het bekijken van de metrische gegevens in het dashboard. 

Hier volgt een eenvoudige dashboard met twee grafieken. De taak voor links geeft het percentage CPU van twee virtuele machines. Aan de rechterkant van de grafiek geeft de transacties in een Azure Storage-account onderverdeeld op basis van het type transactie-API.
    
![Grafana twee grafieken voorbeeld](.\media\monitor-how-to-grafana\grafana6.png) 
    

## <a name="optional-create-dashboard-playlists"></a>Optioneel: Dashboard afspeellijsten maken

Een van de vele handige voorzieningen van Grafana is de afspeellijst dashboard. U kunt meerdere dashboards maken en toe te voegen aan een afspeellijst configureren van een interval voor elke dashboard om weer te geven. Selecteer **afspelen** om te zien van de dashboards doorlopen. U kunt ze aan een grote wanden-monitor voor een kaart' status' voor uw groep wilt weergeven. 
    
![Voorbeeld van de afspeellijst Grafana](.\media\monitor-how-to-grafana\grafana7.png) 


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>Optioneel: Uw aangepaste metrische gegevens controleren in dezelfde Grafana server

U kunt ook Telegraf en InfluxDB te verzamelen en metrische gegevens voor zowel aangepaste en op basis van een agent in dezelfde instantie Grafana tekenen installeren. Er zijn veel gegevensbron invoegtoepassingen die u gebruiken kunt bij elkaar brengt van deze metrische gegevens in een dashboard. 
    
U kunt ook opnieuw wilt gebruiken om op te nemen van metrische gegevens van uw server Prometheus instellen. Gebruik de Prometheus data source-invoegtoepassing in Grafana van invoegtoepassing galerie.
    
Hier vindt u goed naslaginformatie over het gebruik van Telegraf, InfluxDB Prometheus en Docker
 - [Het systeem metrische gegevens met de Stack MAATSTREEPJES op Ubuntu 16.04 bewaken](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Metrische gegevens Docker-resource met Grafana, InfluxDB en Telegraf controleren](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/)

 - [Een bewakingsoplossing voor Docker-hosts, containers en beperkte services](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Hier volgt een installatiekopie van een volledige Grafana-dashboard met metrische gegevens van Azure Monitor en Application Insights.
![Voorbeeld van Grafana metrische gegevens](.\media\monitor-how-to-grafana\grafana8.png) 


## <a name="clean-up-resources"></a>Resources opschonen

U in rekening worden gebracht wanneer er virtuele machines worden uitgevoerd of u ze worden gebruikt of niet. Om te voorkomen dat extra kosten zijn verbonden, opschonen van de resourcegroep in dit artikel hebt gemaakt. 

1. Klik in het menu links in de Azure portal op **resourcegroepen** en klik vervolgens op **Grafana**. 
2. Klik op de pagina van de groep resource **verwijderen**, type **Grafana** in het tekstvak en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Monitor metrische gegevens](monitoring-overview-metrics.md)


