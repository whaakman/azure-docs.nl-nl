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
ms.subservice: ''
ms.openlocfilehash: f592cfdba940b95a9935c6b68f3294233d9fbf15
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56888488"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Bewaken van uw Azure-services in Grafana
U kunt nu Azure-services en toepassingen van bewaken [Grafana](https://grafana.com/) met behulp van de [Azure Monitor-gegevensbron invoegtoepassing](https://grafana.com/plugins/grafana-azure-monitor-datasource). De invoegtoepassing verzamelt prestatiegegevens van toepassingen die door Azure Monitor, met inbegrip van verschillende logboeken en metrische gegevens worden verzameld. U kunt deze gegevens vervolgens weergeven in het Grafana-dashboard.

De invoegtoepassing is momenteel in preview.

Gebruik de volgende stappen uit op een server Grafana instellen en dashboards bouwen om de metrische gegevens en logboeken van Azure Monitor.

## <a name="set-up-a-grafana-server"></a>Een Grafana-server instellen

### <a name="set-up-grafana-locally"></a>Grafana lokaal instellen
Voor het instellen van een lokale server voor het Grafana [downloaden en installeren van Grafana in uw lokale omgeving](https://grafana.com/grafana/download). Voor het gebruik van de invoegtoepassing Azure Monitor-integratie, installeert u Grafana versie 5.3 of hoger.

### <a name="set-up-grafana-on-azure-through-the-azure-marketplace"></a>Instellen van Grafana op Azure via de Azure Marketplace
1. Ga naar de Azure Marketplace en kies Grafana door Grafana Labs.

2. Vul in de namen en gegevens. Maak een nieuwe resourcegroep. De waarden die u voor de gebruikersnaam van de virtuele machine, VM-wachtwoord en het beheerderswachtwoord server Grafana kiest van bijhouden.  

3. VM-grootte en een opslagaccount kiezen.

4. De netwerk-instellingen configureren.

5. De samenvatting weergeven en selecteren **maken** na de gebruiksvoorwaarden accepteren.

6. Nadat de implementatie is voltooid, selecteert u **gaat u naar de resourcegroep**. U ziet een lijst met nieuwe resources.

    ![Grafana resource groepsobjecten](media/grafana-plugin/grafana1.png)

    Als u de netwerkbeveiligingsgroep selecteert (*grafana-nsg* in dit geval), ziet u dat poort 3000 wordt gebruikt voor toegang tot Grafana-server.

7. Het openbare IP-adres van uw server Grafana Get - gaat u terug naar de lijst met resources en selecteer **openbaar IP-adres**.

## <a name="log-in-to-grafana"></a>Meld u aan bij Grafana

1. Met behulp van het IP-adres van uw server, opent u de aanmeldingspagina op *http://\<IP-adres\>: 3000* of de  *\<DNS-naam >\:3000* in uw browser. 3000 is de standaardpoort, Let op: u hebt een andere poort geselecteerd tijdens de installatie. U ziet een aanmeldingspagina voor de Grafana-server die u hebt gemaakt.

    ![Aanmeldingsscherm van Grafana](./media/grafana-plugin/grafana-login-screen.png)

2. Meld u aan met de naam van de gebruiker *admin* en Grafana wachtwoord van de serverbeheerder u eerder hebt gemaakt. Als u een lokale installatie, het standaardwachtwoord zou worden *admin*, en u wordt gevraagd te wijzigen op de eerste keer aanmelden.

## <a name="configure-data-source-plugin"></a>Data source-invoegtoepassing configureren

Wanneer is aangemeld, ziet u de Azure Monitor data source-invoegtoepassing is al opgenomen.

![Grafana bevat Azure Monitor-invoegtoepassing](./media/grafana-plugin/grafana-includes-azure-monitor-plugin-dark.png)

1. Selecteer **gegevensbron toevoegen** toevoegen en configureren van de gegevensbron van Azure Monitor.

2. Kies een naam voor de gegevensbron en selecteer **Azure Monitor** als het type in de vervolgkeuzelijst.

3. Een service-principal maken - Grafana maakt gebruik van een Azure Active Directory-service-principal verbinding maken met Azure Monitor-API's en verzamelen van gegevens. U moet maken of een bestaande service-principal gebruiken voor het beheren van toegang tot uw Azure-resources.
    * Zie [deze instructies](../../azure-resource-manager/resource-group-create-service-principal-portal.md) om een serviceprincipal te maken. Kopiëren en opslaan van uw tenant-ID (Directory-ID), client-ID (toepassings-ID) en het clientgeheim (sleutelwaarde van toepassing).
    * Zie [toepassing toewijzen aan rol](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) om toe te wijzen met de rol lezer in de Azure Active Directory-toepassing op het abonnement, resourcegroep of resource u wilt bewaken. 
    De Log Analytics-API is vereist de [rol van lezer van Log Analytics](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader), die de rol Lezer machtigingen bevat en wordt toegevoegd aan het.

4. Geef de verbindingsdetails naar de API's die u wilt gebruiken. U kunt verbinding maken met alle of enkele ervan. 
    * Als u verbinding kunt maken naar metrische gegevens en Logboeken in Azure Monitor, kunt u dezelfde referenties opnieuw door te selecteren **dezelfde gegevens als de API van Azure Monitor**.
    * Wanneer u de invoegtoepassing configureert, kunt u aangeven welke Azure-Cloud wilt u de invoegtoepassing naar monitor (openbaar, Azure US Government, Azure Duitsland of Azure China).
    * Als u Application Insights gebruikt, kunt u ook uw Application Insights-API en de toepassings-ID voor het verzamelen van Application Insights op basis van metrische gegevens opnemen. Zie voor meer informatie, [aan uw API-sleutel en de toepassings-ID](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

        > [!NOTE]
        > Sommige velden van de gegevensbron zijn anders dan hun gecorreleerde Azure instellingen met de naam:
        >     * Tenant-ID is de Azure-map-ID
        >     * Client-ID is de Azure Active Directory toepassings-ID
        >     * Clientgeheim is de waarde van de Azure Active Directory-toepassing

5. Als u Application Insights gebruikt, kunt u ook uw Application Insights-API en de toepassings-ID voor het verzamelen van Application Insights op basis van metrische gegevens opnemen. Zie voor meer informatie, [aan uw API-sleutel en de toepassings-ID](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

6. Selecteer **opslaan**, en het testen van Grafana worden de referenties voor elke API. U ziet een bericht dat lijkt op de volgende uitvoer.  
    ![Grafana gegevensbron config goedgekeurd](./media/grafana-plugin/grafana-data-source-config-approved-dark.png)

## <a name="build-a-grafana-dashboard"></a>Bouw een Grafana-dashboard

1. Ga naar de startpagina van Grafana en selecteer **nieuw Dashboard**.

2. Selecteer in het nieuwe dashboard, de **Graph**. U kunt experimenteren met andere grafieken opties, maar in dit artikel wordt gebruikgemaakt van *Graph* als voorbeeld.

3. Een lege grafiek wordt weergegeven op uw dashboard. Klik op de titelbalk van het paneel en selecteer **bewerken** in te voeren van de details van de gegevens die u wilt in deze grafiek wordt getekend.
    ![Nieuwe grafiek Grafana](./media/grafana-plugin/grafana-new-graph-dark.png)

4. Selecteer de Azure Monitor-gegevensbron die u hebt geconfigureerd.
    * Verzamelen van Azure Monitor metrics - Selecteer **Azure Monitor** in de vervolgkeuzelijst voor de service. Een lijst met selectors wordt, waarin u de resources en metrische gegevens om te controleren in deze grafiek kunt selecteren. Voor het verzamelen van metrische gegevens van een virtuele machine, gebruikt u de naamruimte **Microsoft.Compute/VirtualMachines**. Nadat u VM's en metrische gegevens hebt geselecteerd, kunt u beginnen met het weergeven van hun gegevens in het dashboard.
    ![Grafana graph-configuratie voor Azure Monitor](./media/grafana-plugin/grafana-graph-config-for-azure-monitor-dark.png)
    * Verzamelen van Azure Monitor logboekgegevens - Selecteer **Azure Log Analytics** in de vervolgkeuzelijst voor de service. Selecteer de werkruimte die u wilt opvragen en de querytekst instellen. U kunt hier elke logboekquery u al hebt of maak een nieuwe kopiëren. Terwijl u in uw query typt, wordt de IntelliSense weergegeven en de opties voor automatisch aanvullen voorstellen. Selecteer het visualisatietype **Time series** **tabel**, en voer de query uit.
    
    > [!NOTE]
    >
    > De standaard-query die is opgegeven met de invoegtoepassing maakt gebruik van twee macro's: "$__timeFilter() en $__interval. 
    > Deze macro's toestaan Grafana het tijdsbereik en het tijdsinterval, dynamisch berekenen wanneer u op een deel van een diagram inzoomen. U kunt deze macro's verwijderen en gebruik een filter (standaardtijd), zoals *TimeGenerated > ago(1h)*, maar dat betekent dat de grafiek wordt geen ondersteuning voor het zoomniveau in functie.
    
    ![Grafana graph-configuratie voor Azure Log Analytics](./media/grafana-plugin/grafana-graph-config-for-azure-log-analytics-dark.png)

5. Hieronder volgt een eenvoudig dashboard met twee grafieken. De naam van links ziet u het CPU-percentage van de twee virtuele machines. De grafiek aan de rechterkant ziet u de transacties in een Azure Storage-account onderverdeeld op basis van de transactie-API-type.
    ![Grafana twee grafieken voorbeeld](media/grafana-plugin/grafana6.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>Optioneel: Uw aangepaste metrische gegevens in dezelfde Grafana-server controleren

U kunt ook installeren Telegraf en InfluxDB te verzamelen en zowel aangepaste als door de agent op basis van metrische gegevens tekenen hetzelfde Grafana-exemplaar. Er zijn veel data source-invoegtoepassingen die u kunt deze metrische gegevens om samen te brengen in een dashboard.

U kunt deze instellingen om op te nemen van metrische gegevens van uw server Prometheus ook hergebruiken. Gebruik de Prometheus data source-invoegtoepassing in het Grafana-invoegtoepassing galerie.

Hier vindt u referentiemateriaal artikelen over het gebruik van Telegraf, InfluxDB Prometheus en Docker
 - [Metrische Systeemmeetgegevens met de MAATSTREEPJES-Stack op Ubuntu 16.04 controleren](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Metrische gegevens voor Docker resources met Grafana, InfluxDB en Telegraf controleren](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/)

 - [Een oplossing voor bewaking voor Docker-hosts, containers en services in containers](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Hier volgt een afbeelding van een volledige Grafana-dashboard met metrische gegevens van Azure Monitor en Application Insights.
![Voorbeeld van Grafana metrische gegevens](media/grafana-plugin/grafana8.png)

## <a name="advanced-grafana-features"></a>Geavanceerde functies van Grafana

### <a name="variables"></a>Variabelen
Sommige querywaarden kunnen worden geselecteerd via de gebruikersinterface vervolgkeuzelijsten en worden bijgewerkt in de query. Houd rekening met de volgende query uit als een voorbeeld:
```
Usage 
| where $__timeFilter(TimeGenerated) 
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```

U kunt een variabele die een met alle beschikbare lijst wordt **oplossing** waarden en werk vervolgens de query om deze te gebruiken.
Voor het maken van een nieuwe variabele, klikt u op de knop instellingen van het dashboard in het bovenste gedeelte van de juiste selecteert **variabelen**, en vervolgens **nieuw**.
Klik op de pagina variabele door de gegevensbron en query wilt uitvoeren om de lijst met waarden te definiëren.
![Grafana variabele configureren](./media/grafana-plugin/grafana-configure-variable-dark.png)

Zodra gemaakt, wordt de query voor het gebruik van de geselecteerde waarde(n) aanpassen en uw grafieken dienovereenkomstig reageren:
```
Usage 
| where $__timeFilter(TimeGenerated) and Solution in ($Solutions)
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```
    
![Grafana gebruiken variabelen](./media/grafana-plugin/grafana-use-variables-dark.png)

### <a name="create-dashboard-playlists"></a>Dashboard afspeellijsten maken

Een van de vele handige voorzieningen van Grafana is de dashboard-afspeellijst. U kunt meerdere dashboards maken en deze toevoegen aan een afspeellijst vindt bij het configureren van een interval voor elk dashboard om weer te geven. Selecteer **afspelen** om de dashboards bladeren door te geven. Kunt u ze aan een grote wall-monitor voor een bord status van uw groep wilt weergeven.

![Voorbeeld van de afspeellijst Grafana](./media/grafana-plugin/grafana7.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u een omgeving met Grafana setup op Azure hebt, worden er kosten berekend wanneer er virtuele machines worden uitgevoerd, ongeacht of u er wel of niet. Om te voorkomen waarvoor extra kosten worden berekend, opschonen van de resourcegroep in dit artikel hebt gemaakt.

1. Klik in het menu links in Azure portal op **resourcegroepen** en klik vervolgens op **Grafana**.
2. Klik op de pagina van uw resourcegroep **verwijderen**, type **Grafana** in het tekstvak in en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Monitor Metrics](../../azure-monitor/platform/data-collection.md)

