---
title: De belangrijkste metrische gegevens weergeven in Azure kosten Management dashboards | Microsoft Docs
description: Dit artikel wordt beschreven hoe dashboards de belangrijkste metrische gegevens weergeven in Azure kosten Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b1dc2e2eca900ca0ae72329c3c373b2d24f1b2e0
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303485"
---
# <a name="view-key-cost-metrics-with-dashboards"></a>Weergave sleutel kosten metrische gegevens met dashboards

Dashboards in Cloudyn bieden een globaal overzicht van rapporten. Dashboards kunnen u de kosten van de belangrijkste metrische gegevens weergeven in één weergave. Ze bieden ook zakelijke trend gemarkeerd om te helpen u belangrijke zakelijke beslissingen te nemen.

Dashboards worden ook gebruikt voor het maken van weergaven voor mensen met verschillende verantwoordelijkheden in uw organisatie, waaronder:

- Financiële-controller
- Eigenaar van de toepassing of het project
- DevOps engineering
- Leidinggevenden

Dashboards zijn opgebouwd uit widgets en elke widget wordt in wezen een miniatuur rapport. Klik op een widget om het rapport te openen. Wanneer u rapporten wilt aanpassen, moet u deze opslaan op mijn rapporten en ze zijn toegevoegd aan het dashboard.

Dashboard versies verschillen voor beheer (MSP), Enterprise en Cloudyn Premium-gebruikers. De verschillen worden bepaald door de entiteit toegangsniveaus. Zie voor meer informatie over toegangsniveaus [toegangsniveaus entiteit](tutorial-user-access.md#entity-access-levels).

Dashboard beschikbaarheid, is afhankelijk van het type van de cloud serviceprovider account dat wordt gebruikt bij het weergeven van dashboards. Het type informatie beschikbaar is en die worden verzameld door Cloudyn is van invloed op de rapporten in dashboards. Voor bijvoorbeeld als u een AWS-account geen zien vervolgens u niet het dashboard S3 vastleggen. Op dezelfde manier als u Azure Resource Manager toegang tot Cloudyn niet inschakelen zien vervolgens u niet alle Azure-specifieke informatie in dashboardwidgets optimaliseren.

U kunt een van de vooraf gemaakte dashboards gebruiken of u kunt uw eigen dashboard met aangepaste rapporten maken. Als u niet bekend met Cloudyn rapporten bent, Zie [kostenbeheer gebruik rapporten](use-reports.md).

## <a name="create-a-custom-dashboard"></a>Een aangepaste dashboard maken

U kunt een bestaande voor het gebruik van de eigenschappen ervan dupliceren om snel aan de slag met een aangepast dashboard. U kunt deze aanpassen aan uw behoeften. Klik op het dashboard dat u wilt kopiëren, **OpslaanAls**. U kunt alleen aangepaste dashboards dupliceren — u kunt de dashboards die deel van Cloudyn uitmaken niet dupliceren.

Een aangepast dashboard maken:

1. Klik op de startpagina **nieuwe toevoegen +**. De pagina Dashboard van mijn wordt weergegeven.  
    ![Mijn dashboard](./media/dashboards/my-dashboard.png)
2. Klik op **nieuw rapport toevoegen**. Het rapport toevoegen wordt weergegeven.
3. Selecteer het rapport dat u wilt toevoegen aan het dashboard-object. De widget wordt toegevoegd aan het dashboard.
4. Herhaal de voorgaande stappen totdat het dashboard voltooid is.
5. Wijzig de naam van het dashboard, klikt u op de naam van het dashboard op de startpagina Dashboard en typ de nieuwe naam.

## <a name="modify-a-custom-dashboard"></a>Een aangepast dashboard wijzigen

Zoals het maken van een aangepast dashboard kunt u de dashboards die deel uitmaakt van Cloudyn niet wijzigen. Een rapport aangepaste dashboard wijzigen:

1. Zoek in het dashboard, het rapport dat u wilt wijzigen en klik op **bewerken**. Het rapport wordt weergegeven.
2. Breng eventuele wijzigingen die u wilt het rapport en klikt u op **opslaan**. Het rapport wordt bijgewerkt en uw wijzigingen.

## <a name="share-a-custom-dashboard"></a>Een aangepast dashboard te delen

U kunt een aangepast dashboard delen met anderen _openbare_ of _mijn entiteit_. Wanneer u naar openbaar deelt, kunnen alle gebruikers het dashboard te bekijken. Alleen gebruikers met toegang tot de huidige entiteit kunnen het dashboard weergeven bij het delen van mijn entiteit. De stappen voor het delen van een aangepast dashboard met openbare en Mijn entiteit zijn vergelijkbaar.

Een aangepast dashboard naar openbaar delen:

1. In een dashboard, klikt u op **Dashboardinstellingen**. De instellingen van het Dashboard wordt weergegeven.  
    ![dashboard-opties](./media/dashboards/dashboard-options.png)
2. Klik op de pijl in het Dashboard-instellingen en klik vervolgens op **openbare**. Het openbare Dashboard bevestigingsdialoogvenster wordt weergegeven.
3. Klik op **Ja**. Het dashboard is nu beschikbaar voor anderen.

## <a name="delete-a-custom-dashboard-report"></a>Een rapport aangepaste dashboard verwijderen

U kunt een aangepast rapport-onderdeel verwijderen vanuit het dashboard. Het rapport verwijderen vanuit het dashboard niet van het rapport in de rapportenlijst. In plaats daarvan het rapport verwijdert, wordt deze vanuit het dashboard alleen.

Als u wilt verwijderen van een onderdeel dashboard, klikt u op het onderdeel dashboard, klikt u op **verwijderen**. Te klikken op **verwijderen** onmiddellijk worden verwijderd van het onderdeel van het dashboard.

## <a name="share-a-dashboard-enterprise"></a>Delen van een dashboard (Enterprise)

U kunt aangepaste dashboards voor alle gebruikers in uw organisatie of met de gebruikers van de huidige entiteit delen. Een dashboard delen kan anderen geeft een overzicht op hoog niveau van de KPI. Wanneer u een dashboard deelt, worden automatisch het dashboard voor al uw Cloudyn entiteiten/klanten repliceert. Wijzigingen aan het gedeelde dashboard worden automatisch bijgewerkt.

Een dashboard delen met alle gebruikers subentiteiten inclusief:

1. Klik op de startpagina dashboard **bewerken**.
2. Klik op **Share** en selecteer vervolgens **openbare**.
3. Globale openbare Dashboard bevestigingsvenster weergegeven.
4. Klik op **Ja** het dashboard instellen als een globale openbare dashboard.

Een dashboard delen met alle gebruikers van een huidige entiteit:

1. Klik op de startpagina Dashboard **bewerken**.
2. Klik op **Share** en selecteer vervolgens **mijn entiteit**.
3. Klik op **Ja** het dashboard instellen als een openbare dashboard.

## <a name="duplicate-a-custom-dashboard"></a>Een aangepast dashboard dupliceren

Wanneer u een nieuw dashboard maakt, kunt u vergelijkbare eigenschappen uit een bestaand dashboard gebruiken. U kunt het dashboard voor het maken van een nieuwe dupliceren.

U kunt alleen aangepaste dashboards dupliceren. U kunt standaard dashboards niet dupliceren.

Moeten worden gedupliceerd (klonen) een aangepast dashboard:

1. Klik op het Dashboard dat u wilt dupliceren, **OpslaanAls**. Een nieuw dashboard wordt geopend met dezelfde naam en een nummer.
2. Wijzig de naam van het gedupliceerde dashboard en als u wilt wijzigen.

- Of -

1. Klik in de instellingen van Dashboard **OpslaanAls** op de regel van het dashboard dat u wilt dupliceren.
2. Hiermee opent u het gedupliceerde dashboard.
3. Wijzig de naam van het dashboard en als u wilt wijzigen.

## <a name="set-a-default-dashboard"></a>Een standaarddashboard instellen

U kunt een dashboard als standaard instellen. Instellen op uw standaard kan worden weergegeven als het meest linkse tabblad in de lijst dashboard. De standaarddashboard worden weergegeven wanneer de Cloudyn portal openen.

- Klik op het dashboardtabblad wilt ingesteld als standaard en klik vervolgens op **standaard** aan de rechterkant.

- Of -

1. Klik op **Dashboardinstellingen** de lijst met beschikbare dashboards zien en selecteren van het dashboard dat u wilt instellen als standaardbrowser.  
    ![dashboard-opties](./media/dashboards/dashboard-options.png)
2. Klik op **standaard** in de regel van het dashboard. Standaard Dashboard bevestigingsvenster weergegeven.
3. Klik op **Ja**. Het dashboard is ingesteld op standaard.

## <a name="management-dashboard"></a>Management Dashboard
Het beheer (of MSP-dashboard voor MSP gebruikers) dashboard bevat licht van de belangrijkste rapporttypen.  
![Management Dashboard](./media/dashboards/management-dash.png)

### <a name="cost-entity-summary-enterprise-only"></a>Samenvatting van de kosten entiteit (alleen voor Enterprise)
Deze widget bevat een overzicht van de kosten van beheerde entiteiten, waaronder het aantal entiteiten en aantal accounts.
- Klik op de widget om de Details van de Enterprise-rapport te openen.

### <a name="cost-over-time"></a>Na verloop van tijd kosten
Deze widget kunt u kosten trends herkennen. Deze handleiding komen de kosten voor de laatste dag op basis van de trend van de afgelopen 30 dagen.
- Klik op de widget om de werkelijke kosten gedurende een periode rapport weergeven te openen en aanvullende gegevens te filteren.

### <a name="asset-controller"></a>Asset-Controller
Deze widget markeert het aantal actieve exemplaren van de vorige dag, boven de trend gebruik gedurende de afgelopen 30 dagen.
- Klik op de widget om te openen van het dashboard Asset-Controller.

### <a name="unused-ri-detector"></a>Ongebruikte k detectie
Deze widget markeert het aantal Amazon EC2 ongebruikte reserveringen.
- Klik op de widget om te openen van het momenteel ongebruikte reserveringen rapport waar u de ongebruikte kunt bekijken reserveringen, kunt u wijzigen.

### <a name="cost-by-service"></a>Kosten per Service
Deze widget licht afgeschreven kosten door de service voor de afgelopen 30 dagen. Beweeg de muisaanwijzer over het cirkeldiagram om te zien van de kosten per service.
- Klik op de widget om te openen van het rapport analyse van de werkelijke kosten.

### <a name="potential-savings"></a>Besparing
Deze widget bevat aanbevelingen voor Amazon EC2 en Amazon RDS. prijzen exemplaartype
- Klik op de widget open het rapport analyse van besparingen. Hiermee geeft u de kosten per exemplaar typen met besparing.

### <a name="compute-instances---daily-trend"></a>COMPUTE exemplaren - dagelijkse Trend
Deze widget geeft de actieve exemplaren per type voor de afgelopen 30 dagen.
- Klik op de widget om te openen van het rapport exemplaren gedurende een periode, waar u een overzicht van alle exemplaren die worden uitgevoerd tijdens de afgelopen 30 dagen kunt bekijken.

### <a name="storage-by-department"></a>Opslag per afdeling
Deze widget geeft de storage-services die worden gebruikt door afdelingen. Beweeg de muisaanwijzer over het cirkeldiagram om te zien van uw opslagverbruik per afdeling.
- Klik op de widget om te openen van het dashboard S3 vastleggen.

## <a name="cost-controller-dashboard"></a>Kosten Controller-dashboard
Het dashboard kosten domeincontroller bevat vooraf ingestelde kosten toewijzing licht.  
![Kosten Controller-dashboard](./media/dashboards/cost-controller-dashboard.png)

### <a name="cost-over-time"></a>Na verloop van tijd kosten
Deze widget helpt u kosten trends herkennen. Deze handleiding komen de kosten voor de laatste dag op basis van de trend van de afgelopen 30 dagen.
- Klik op de widget om de werkelijke kosten gedurende een periode rapport weergeven te openen en aanvullende gegevens te filteren.

### <a name="monthly-cost-trends"></a>Maandelijkse kosten Trends
Deze widget worden gemarkeerd met het verwachte afgeschreven uitgaven en uw werkelijke besteden sinds het begin van de maand.
- Klik op de widget om de huidige maand geschatte kosten-rapport waarmee kosten maand tot datum samenvatting te openen.

Dit rapport geeft de kosten van het begin van de maand, de kosten van vorige maand en de huidige maand geschatte kosten. De huidige maand geschatte kosten wordt berekend door de up-to-date maandelijkse kosten en projectie toe te voegen. De projectie is gebaseerd op de kosten bewaakt in de afgelopen 30 dagen.

### <a name="12-month-planner"></a>Planner voor 12 maanden
Deze widget licht de geschatte kosten via de volgende 12 maanden en de besparing.
- Klik op de widget om de jaarlijkse geschatte kosten-rapport te openen.

### <a name="cost-by-service"></a>Kosten per Service
Deze widget licht afgeschreven kosten door de service voor de afgelopen 30 dagen.
- Beweeg de muisaanwijzer over het cirkeldiagram om te zien van de kosten per service.
- Klik op de widget om te openen van het rapport analyse van de werkelijke kosten.

### <a name="cost-by-account"></a>Kosten per Account
Deze widget markeert afgeschreven kosten door account voor de afgelopen 30 dagen.
- Beweeg de muisaanwijzer over het cirkeldiagram om te zien van de kosten per account.
- Klik op de widget om te openen van het rapport analyse van de werkelijke kosten.

### <a name="cost-trend-by-day"></a>Kosten Trend per dag
Deze widget te besteden aan de licht gedurende de afgelopen 30 dagen.
- Beweeg de muisaanwijzer over het staafdiagram om te zien van de kosten per dag.
- Klik op de widget om de werkelijke kosten gedurende een periode-rapport te openen.

### <a name="cost-trend-by-month---last-6-months"></a>Trend van kosten per maand - laatste 6 maanden

Deze widget licht hoeven te besteden aan in de afgelopen zes maanden.
- Beweeg de muisaanwijzer over het staafdiagram om te zien van de kosten per maand.
- Klik op de widget om de werkelijke kosten gedurende een periode-rapport te openen.

## <a name="asset-controller-dashboard"></a>Asset-Controller-dashboard

Dit dashboard geeft het aantal actieve instanties, beschikbaar is en in gebruik schijven, distributie van het exemplaar typen en storage-gegevens weer.  
![Asset-Controller-dashboard](./media/dashboards/asset-controller-dashboard.png)

### <a name="compute-instances"></a>COMPUTE-exemplaren
Deze widget geeft het aantal exemplaren op basis van de trend gebruik gedurende de afgelopen 30 dagen uitgevoerd.
- Klik op de widget om het rapport exemplaren gedurende een periode te openen.

### <a name="disks"></a>Disks
Deze widget markeert het totale aantal en het volume van de schijven die in gebruik en beschikbaar zijn.
- Klik op de widget om de schijven actieve rapport te openen.

### <a name="instance-type-distribution"></a>Exemplaar Type distributiepunt
Deze widget licht de exemplaar-typen in een cirkeldiagram.
- Klik op de widget om het rapport exemplaar distributie, waarmee u een overzicht van uw actieve exemplaren door de geselecteerde aggregatie te openen.

### <a name="compute-instances---daily-trend"></a>COMPUTE exemplaren - dagelijkse Trend
Deze widget licht de compute-exemplaren (ter plaatse, gereserveerde en op aanvraag) per dag voor de afgelopen 30 dagen.
- Beweeg de muisaanwijzer over de grafiek om het aantal compute-exemplaren per type per dag weer te geven.
- Klik op de widget om het rapport exemplaren gedurende een periode te openen.

### <a name="all-buckets-s3"></a>Alle Buckets (S3)
Deze widget illustreert de totale S3 opslag en het aantal objecten die zijn opgeslagen.
- Klik op de widget om te openen van het Dashboard van de tracering S3. Het dashboard kunt vinden, analyseren en weergave van uw huidige gebruik van opslag en trends.

### <a name="sql-db-instances-rds"></a>SQL database-exemplaren (RDS)
Deze widget markeert het aantal actieve Amazon RDS-exemplaren op basis van de trend van de afgelopen 30 dagen.
- Klik op de widget om het rapport RDS exemplaar gedurende een periode te openen.

## <a name="optimizer-dashboard"></a>Optimalisatie van Dashboard
Dit dashboard geeft aanbevelingen downsizing, niet-gebruikte resources en besparing.  
![Optimalisatie van dashboard](./media/dashboards/optimizer-dashboard.png)

### <a name="ri-calculator"></a>K Rekenmachine
Deze widget geeft het aantal k kopen aanbevelingen en markeert de jaarlijkse besparing.
- Klik op de widget om te openen van de gereserveerde exemplaar Rekenmachine waar u wanneer bepalen kunt te gebruiken op aanvraag versus prijsstelling gereserveerd.

### <a name="sizing"></a>Grootte aanpassen
Deze widget worden de aanbevolen grootte en potentiële besparingen gemarkeerd als geïmplementeerd.
- Klik op de widget om het rapport EC2 voordelige Sizing aanbevelingen te openen.

### <a name="unused-ri-detector"></a>Ongebruikte k detectie
Deze widget markeert het aantal Amazon EC2 ongebruikte reserveringen.
- Klik op de widget u opent de momenteel ongebruikte reserveringen lijst Hier vindt u de ongebruikte reserveringen die u kunt wijzigen.

###  <a name="available-disks"></a>Beschikbare schijven
Deze widget markeert het aantal niet-gekoppelde schijven in uw implementatie.
- Klik op de widget om het rapport niet-gekoppelde schijven te openen.

### <a name="rds-ri-calculator"></a>RDS k Rekenmachine
Deze widget wordt het aantal reservering aanbevelingen voor uw Amazon RDS-exemplaren en de mogelijke besparingen gemarkeerd.
- Klik op de widget u opent de RDS k kopen aanbevelingen lijst waarin u Cloudyn aanbevelingen voor het gereserveerde exemplaren gebruiken in plaats van exemplaren op aanvraag kunt zien.

### <a name="rds-sizing"></a>Formaat van RDS
Deze widget geeft het aantal sizing aanbevelingen en de besparing.
- Klik op de widget om het rapport RDS Sizing aanbevelingen, welke geeft gedetailleerde Amazon RDS aanbevelingen formaat te openen.

De optimalisatie aanbevelingen zijn gebaseerd op het gebruik en prestaties gegevens bewaakt in de afgelopen maand.

## <a name="s3-tracker-dashboard"></a>S3-tracering in-dashboard
De tracering S3-dashboard kunt u vinden, analyseren en weergave van uw huidige gebruik van opslag en trends.  
![S3-tracering in-dashboard](./media/dashboards/s3-tracker-dashboard.png)

### <a name="all-buckets"></a>Alle Buckets
Deze widget licht de totale grootte van alle uw buckets, GB, en het totale aantal objecten in uw buckets.
- Klik op de widget om de distributie van de grootte van de S3-rapport te openen. Het rapport kunt u de grootte van uw S3 door bucket bronmap, opslag en de versieregistratietstatus analyseren.

### <a name="bucket-properties"></a>Bucket-eigenschappen
Deze widget markeert het totale aantal buckets voor opslag.
- Klik op de widget om de eigenschappen van S3-Bucket-rapport te bekijken.

### <a name="scan-status"></a>Scannen van Status
Deze widget illustreert wanneer de laatste S3-scan is voltooid en wanneer er een wordt gestart.
- Klik op de widget om te openen van het statusrapport S3 scannen.

### <a name="storage-by-bucket"></a>Opslag door Bucket
Deze widget markeert het percentage dat elke klasse bucket-opslag gebruikt.
- Klik op de widget om de distributie van de grootte van de S3-rapport te openen. Het rapport kunt u de grootte van uw S3 door bucket bronmap, opslag en de versieregistratietstatus analyseren.

### <a name="number-of-objects-by-bucket"></a>Aantal objecten dat door Bucket
Deze widget wordt het aantal objecten per bucket in werkelijke nummer en percentage gemarkeerd. Beweeg de muisaanwijzer over de bucket om te zien van het totale aantal objecten.
- Klik op de widget om de distributie van de grootte van de S3-rapport (Scan gebaseerd) te openen.

## <a name="cloud-comparison-dashboard"></a>Dashboard voor cloud-vergelijking
Het dashboard Cloud vergelijking helpt bij het vergelijken van kosten van verschillende cloud-providers op basis van de prijzen, het type van de CPU en RAM-geheugen.  
![Dashboard voor cloud-vergelijking](./media/dashboards/cloud-comparison-dashboard.png)

### <a name="ec2-cost-in-azure-by-instance-type"></a>EC2 Kosten in Azure door Type sjabloonexemplaar
Deze widget licht de afgelopen 30 dagen van gebruik in de tarieven voor op aanvraag. Deze comares de kosten met de huidige Amazon EC2 kosten tegenover eventuele kosten in Azure.
- Beweeg de muisaanwijzer over de balken kosten per type sjabloonexemplaar vergelijken.
- Klik op de widget om te openen van het overdragen van de implementatie – kosten analyserapport.

### <a name="ec2-cost-in-azure"></a>EC2 Kosten in Azure
Deze widget geeft de kosten van uw huidige Amazon EC2 en vergelijkt ze met Azure. De vergelijking is gebaseerd op de laatste 30 dagen van gebruik in de tarieven voor op aanvraag.
- Klik op de widget om te openen van het overdragen van de implementatie - analyserapport kosten.

### <a name="ec2azure-instance-type-mapping"></a>Toewijzing van het Type EC2/Azure-exemplaar
Deze widget licht de beste toewijzing van elastische compute-eenheden tussen Amazon EC2 en Azure.
- Klik op de widget om het rapport exemplaren Type toewijzing te openen.
