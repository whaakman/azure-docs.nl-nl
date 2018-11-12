---
title: Dashboard, bewaken, schaal, configureren, en hybride verbindingen in BizTalk Services | Microsoft Docs
description: Meer informatie over de besturingselementen en de prestaties voor BizTalk Services controleren
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 3f4763b5e15d4b9b84e868262a9e8538b8a407a2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228824"
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>De tabbladen Dashboard, Controleren, Schaal, Configureren en Hybride verbinding controleren

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Nadat u uw BizTalk Service maken en implementeren van uw toepassing, kunt u enkele van de BizTalk Service-instellingen wijzigen en controleren van de toepassingsprestaties. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Hiermee wordt een nieuw venster geopend met de volgende tabbladen. Dit onderwerp beschrijft deze tabbladen.

## <a name="quickstart-quickstartquickstart"></a>Quick Start (![Snelstartgids][Quickstart])
Afhankelijk van de versie van BizTalk Services, alle vermelde opties mogelijk niet beschikbaar. 

<table border="1">
    <tr>
        <td><strong>De hulpprogramma's downloaden</strong></td>
        <td>Download de BizTalk Services SDK voor het installeren van de Visual Studio-projectsjablonen op uw lokale ontwikkelcomputer. Deze sjablonen maken het <strong>BizTalk Services</strong> (brug) en de <strong>BizTalk serviceartefacten</strong> (Transformeren) Visual Studio-projecten die zijn geïmplementeerd voor uw BizTalk Service.
        <br/><br/>
        <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302335"> Hoe begin ik met de Azure BizTalk Services SDK </a> en <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=241589">installatie van de Azure BizTalk Services SDK</a> bevat de stappen die aan de slag.
        </td>
    </tr>
    <tr>
        <td><strong>Partner-overeenkomsten maken</strong></td>
        <td>Hiermee opent u de Azure BizTalk Services-Portal wordt gehost in Azure waar u partners toevoegen en X12, AS2, maken en EDIFACT EDI-overeenkomsten.
        <br/><br/>
        <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">Configuratie van onderdelen voor EDI-berichten op BizTalk Services-Portal</a> bevat de stappen die aan de slag.
        </td>
    </tr>

<tr>
        <td><strong>Meer informatie over BizTalk Services</strong></td>
        <td>Ga naar de <a HREF="https://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> voor meer informatie over Azure BizTalk Services.</td>
</tr>
</table>


In de taakbalk onder, kunt u het volgende doen:

<table border="1">

<tr>
<td><strong>Beheren</strong> implementatie van uw toepassing</td>
<td>Hiermee opent u de Azure BizTalk Services-portal. De BizTalk Services-Portal is de toegang tot de EDI-configuratie, zoals het toevoegen van partners en het maken van X12, AS2 en EDIFACT-overeenkomsten.
<br/><br/>
Dit is hetzelfde als <strong>partner overeenkomsten maken</strong> op de <strong>Quick Start</strong> tabblad.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">Configuratie van onderdelen voor EDI-berichten op BizTalk Services-Portal</a> bevat meer informatie over de BizTalk Services-Portal.</td>
</tr>

<tr>
<td><strong>Verbindingsgegevens</strong> van de Access Control-Namespace</td>
<td>Wanneer u de verbindingsgegevens selecteert, klikt u vervolgens de Access Control Namespace, standaard verlener en sleutel standaard weergegeven. U kunt deze waarden kunt kopiëren.
<br/><br/>
U kunt ook de Access Control-beheerportal openen. <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285670">Maken van een Access control Namespace</a> bevat meer informatie over de Access Control-beheerportal.</td>
</tr>

<tr>
<td><strong>Sleutels synchroniseren</strong> in de Storage-Account</td>
<td>Wanneer u een opslagaccount maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze versleutelingssleutels beheren toegang tot uw Storage-Account. Uw BizTalk Service maakt automatisch gebruik van de primaire sleutel. <strong>Sleutels synchroniseren</strong> kunnen gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.
<br/><br/>
Bijvoorbeeld, wilt u de BizTalk Service met een nieuwe primaire sleutel voor het Opslagaccount. Om dit te doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer de secundaire sleutel. Als u dit doet, wordt er in de BizTalk-Service wordt gestart met behulp van de secundaire sleutel.</li>
<li>Selecteer uw opslagaccount en de primaire sleutel opnieuw genereren. Let op: uw BizTalk Service met behulp van de secundaire sleutel.</li>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>Selecteer uw opslagaccount en de secundaire sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'sleutelrollover' genoemd. Het doel is om in te schakelen gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.</td>
</tr>

<tr>
<td><strong>Verwijder</strong> uw toepassing</td>
<td>Wanneer u selecteert verwijderen, uw BizTalk Service en alle items die zijn geïmplementeerd, worden verwijderd.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Afhankelijk van de versie van BizTalk Services, alle vermelde opties mogelijk niet beschikbaar. 

Wanneer u de naam van uw BizTalk Service selecteert, wordt het tabblad Dashboard weergegeven. In het Dashboard, kunt u het volgende doen:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Overzicht van gebruik: Geeft het aantal gebruikte hybride verbindingen
Het gegevensgebruik worden ook weergegeven in GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrische grafiek: Toont een vaste lijst met metrische gegevens voor prestaties
Deze metrische gegevens bestaan uit realtime waarden die betrekking hebben de status van de BizTalk Service. U kunt ook de **relatieve** of **Absolute** waarden en het tijdsbereik **Interval** van de metrische gegevens die worden weergegeven in de grafiek. 

Voor een beschrijving van deze maatstaven voor prestaties, gaat u naar [beschikbare metrische gegevens](#Metrics) in dit onderwerp.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Snelle weergave: Een lijst met uw BizTalk Service-eigenschappen
<table border="1">

<tr>
<td><strong>De referenties van de Database bijhouden bijwerken</strong></td>
<td>De gebruikersnaam en wachtwoord gebruikt voor aanmelding bij de Database voor het bijhouden van wijzigingen.</td>
</tr>
<tr>
<td><strong>SSL-certificaat bijwerken</strong></td>
<td>Kan de BizTalk-Service voor het gebruik van een ander SSL-certificaat bijwerken. Een zelf-ondertekend SSL-certificaat wordt automatisch gemaakt wanneer u <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">maken van de BizTalk Service</a>.</td>
</tr>
<tr>
<td><strong>Certificaat downloaden</strong></td>
<td>U kunt het SSL-certificaat gebruikt door uw BizTalk-Service naar een lokale computer downloaden.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Geeft de huidige status van uw BizTalk Service. Zie <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: servicestatusgrafiek</a>. </td>
</tr>
<tr>
<td><strong>Service-URL</strong></td>
<td>De URL voor uw BizTalk Service. Dit is hetzelfde als de <strong>domein-URL</strong> ingevoerd wanneer uw BizTalk Service wordt gemaakt.</td>
</tr>
<tr>
<td><strong>Openbare virtuele IP (VIP)-adres</strong></td>
<td>Het IP-adres toegewezen aan uw BizTalk Service. Het wordt gebruikt voor alle ingangseindpunten en het bronadres voor uitgaand verkeer. Dit IP-adres behoort tot uw BizTalk Service, zolang deze is gemaakt. Als u de BizTalk Service verwijdert, wordt het IP-adres toegewezen aan een andere BizTalk Service.</td>
</tr>
<tr>
<td><strong>ACS-Namespace</strong></td>
<td>Geverifieerd met de BizTalk Service.</td>
</tr>
<tr>
<td><strong>Editie</strong></td>
<td>Een lijst met de editie hebt ingevoerd toen de BizTalk-Service is gemaakt.</td>
</tr>
<tr>
<td><strong>Locatie</strong></td>
<td>Geeft de geografische regio die als host fungeert voor uw BizTalk Service.</td>
</tr>
<tr>
<td><strong>Gemaakt</strong></td>
<td>Geeft de datum en tijd waarop die de BizTalk-Service is gemaakt.</td>
</tr>
<tr>
<td><strong>Traceringsdatabase</strong></td>
<td>De naam van Azure SQL Database waarin de Traceringstabellen gebruikt door uw BizTalk Service. 
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten wat</a> vindt u informatie over de Database bijhouden.</td>
</tr>
<tr>
<td><strong>Opslag voor bewaken/archiveren</strong></td>
<td>De naam van de Azure Storage-account waarin de uitvoer van de bewaking van uw BizTalk Service.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten wat</a> vindt u informatie over de Storage-account.</td>
</tr>
<tr>
<td><strong>De naam van abonnement</strong></td>
<td>Geeft een lijst van het abonnement dat als host fungeert voor uw BizTalk Service. Het abonnement bepaalt toegang.</td>
</tr>
<tr>
<td><strong>abonnements-ID</strong></td>
<td>Als u een abonnement maakt, wordt er automatisch een abonnements-ID gegenereerd. Wanneer met behulp van REST-API's, moet u mogelijk de abonnement-id invoeren.</td>
</tr>
</table>

[BizTalk Services: Inrichten](https://go.microsoft.com/fwlink/p/?LinkID=302280) vermeldt de stappen voor het maken van een BizTalk Service.

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Verbindingsgegevens, sleutels synchroniseren, beheren en verwijderen in de taakbalk:
<table border="1">

<tr>
<td><strong>Beheren</strong> implementatie van uw toepassing</td>
<td>Hiermee opent u de Azure BizTalk Services-Portal. De BizTalk Services-Portal is de toegang tot de EDI-configuratie, zoals het toevoegen van partners en het maken van X12, AS2 en EDIFACT-overeenkomsten.
<br/><br/>
Dit is hetzelfde als <strong>partner overeenkomsten maken</strong> op de <strong>Quick Start</strong> tabblad.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=303653">Configuratie van onderdelen voor EDI-berichten op BizTalk Services-Portal</a> bevat meer informatie over de BizTalk Services-Portal.</td>
</tr>
<tr>
<td><strong>Verbindingsgegevens</strong> van de Access Control-Namespace</td>
<td>Geeft de Access Control Namespace, standaard verlener en sleutel standaard waarden; die kunnen worden gekopieerd.
<br/><br/>
U kunt ook de Access Control-beheerportal openen. Deze Access Control-beheerportal is hetzelfde als wanneer u de optie Active Directory in het navigatiedeelvenster links.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285670">Beheer uw ACS-Namespace</a> bevat meer informatie over de Access Control-beheerportal.</td>
</tr>
<tr>
<td><strong>Sleutels synchroniseren</strong> in de Storage-Account</td>
<td>Wanneer u een opslagaccount maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze versleutelingssleutels beheren toegang tot uw Storage-Account. Uw BizTalk Service maakt automatisch gebruik van de primaire sleutel. <strong>Sleutels synchroniseren</strong> kunnen gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.
<br/><br/>
Bijvoorbeeld, wilt u de BizTalk Service met een nieuwe primaire sleutel voor het Opslagaccount. Om dit te doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer de secundaire sleutel. Als u dit doet, wordt er in de BizTalk-Service wordt gestart met behulp van de secundaire sleutel.</li>
<li>Selecteer uw opslagaccount en de primaire sleutel opnieuw genereren. Let op: uw BizTalk Service met behulp van de secundaire sleutel.</li>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>Selecteer uw opslagaccount en de secundaire sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'sleutelrollover' genoemd. Het doel is om in te schakelen gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.</td>
</tr>

<tr>
<td><strong>Verwijder</strong> uw toepassing</td>
<td>Uw BizTalk Service en alle items die zijn geïmplementeerd, worden verwijderd.</td>
</tr>
</table>


## <a name="monitor"></a>Controleren
Geldt niet voor de Free Edition.

Wanneer u de naam van uw BizTalk Service selecteert, wordt het tabblad Monitor is beschikbaar en wordt het volgende weergegeven:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrische grafiek: Geeft de van de geselecteerde metrische prestatiegegevens
Deze metrische gegevens bestaan uit realtime waarden die betrekking hebben de status van de BizTalk Service. U kiezen welke metrische gegevens voor prestaties worden weergegeven. Maximaal zes maatstaven voor prestaties kan tegelijkertijd worden weergegeven. 

U kunt ook de **relatieve** of **Absolute** waarden en het tijdsbereik **Interval** van de metrische gegevens die worden weergegeven. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Om te verwijderen of metrische gegevens weergeven in de grafiek:
1. Selecteer de **Monitor** tabblad.
2. Selecteer **metrische gegevens toevoegen** in de taakbalk:  
   ![Schakel toevoegen metrische gegevens][AddMetrics]
3. Controleer de prestatiegegevens die u wilt weergeven.
4. Selecteer het vinkje om terug te keren naar de **Monitor** tabblad.
5. Selecteer de cirkel naast de metrische gegevens om weer te geven van de waarde van deze metrische gegevens in de grafiek.  
   
    Bijvoorbeeld, de **CPU-gebruik** metrische gegevens is niet beschikbaar; de uitvoer wordt niet weergegeven in de grafiek:  
   ![CPU-gebruik metrische gegevens is niet beschikbaar][GrayedMetric]  
   
    Selecteer de niet-beschikbaar van de cirkel om in te schakelen de **CPU-gebruik** metrische gegevens om de uitvoer ervan weergegeven in de grafiek weer te geven:  
   ![CPU-gebruik metrische gegevens is ingeschakeld][EnabledMetric]
6. Als u wilt een metrische waarde verwijderen uit de grafiek weergegeven als de lijst, selecteer **metrische gegevens verwijderen** in de taakbalk. Als u wilt de metrische gegevens weer toe te voegen aan de lijst, selecteer **metrische gegevens toevoegen** controleren van de metrische gegevens in de taakbalk en selecteer het vinkje om terug te keren naar de **Monitor** tabblad. Selecteer de niet-beschikbaar van de cirkel om in te schakelen van de metrische gegevens.

## <a name="Metrics"></a>Beschikbare metrische gegevens
De volgende items/maatstaven voor prestaties zijn beschikbaar:

<table border="1">

<tr>
<td><strong>RountdTrip latentie</strong></td>
<td>Geeft de gemiddelde tijd in milliseconden (ms) voor het verwerken van een bericht vanaf het moment dat het bericht wordt ontvangen totdat het bericht is volledig wordt verwerkt door de BizTalk-Service weer over alle bruggen. Alleen de verwerkte berichten worden meegeteld.<br/><br/>
Als de volgende gebeurtenissen plaatsvinden, wordt een tijdstempel gemaakt:
<ul>
<li>Bericht voert de gateway</li>
<li>Bericht wordt doorgestuurd naar de bestemming</li>
<li>Doel-antwoord wordt ontvangen</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>
Met deze metriek wordt het resultaat van de berekening van de volgende:
<br/><br/>
[Doel bevestiging antwoord verzonden naar de gateway] - [bericht voert de gateway]</td>
</tr>
<tr>
<td><strong>Fouten bij de bron</strong></td>
<td>Geeft het totale aantal berichten dat is mislukt door de BizTalk-Service bij het ophalen van berichten van de bron-eindpunten.</td>
</tr>
<tr>
<td><strong>CPU-gebruik</strong></td>
<td>Geeft een lijst van het gemiddelde percentage processortijd van alle rolinstanties.</td>
</tr>
<tr>
<td><strong>Latentie voor verwerking</strong></td>
<td>Geeft de gemiddelde tijd In milliseconden (ms) in een bericht te verwerken door de BizTalk-Service weer over alle bruggen, met uitzondering van de tijd besteed aan bestemmingen. Alleen de verwerkte berichten worden meegeteld.<br/><br/>
Wanneer elk van de volgende gebeurtenissen plaatsvinden, wordt een tijdstempel gemaakt:

<ul>
<li>Bericht voert de gateway</li>
<li>Bericht wordt doorgestuurd naar de bestemming</li>
<li>Doel-antwoord wordt ontvangen</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>Met deze metriek wordt het resultaat van de berekening van de volgende:<br/><br/>
[Doel bevestiging antwoord verzonden naar de gateway] - [bericht voert de gateway] - [doel antwoord wordt ontvangen] + [bericht wordt doorgestuurd naar de bestemming]</td>
</tr>
<tr>
<td><strong>Fouten In verwerking</strong></td>
<td>Geeft het totale aantal berichten dat is mislukt tijdens de verwerking door de BizTalk-Service voor alle bruggen binnen een bepaalde periode.</td>
</tr>
<tr>
<td><strong>Berichten die worden verzonden</strong></td>
<td>Geeft het totale aantal berichten die via alle bruggen door de BizTalk-Service worden verzonden in een bepaalde periode. Met deze metriek wordt verhoogd wanneer een bericht van een pijplijn is de route bestemming bereikt. Met deze metriek geeft niet aan dat een bericht met succes is verwerkt.<br/><br/>
In een scenario voor aanvraag / antwoord, worden de metrische gegevens wordt verhoogd wanneer de bestemming route een ontvangstbevestiging terug naar de pijplijn verzendt.</td>
</tr>
<tr>
<td><strong>Berichten ontvangen</strong></td>
<td>Geeft het totale aantal berichten dat is ontvangen door de BizTalk-Service voor alle bruggen binnen een bepaalde periode. Met deze metriek wordt verhoogd wanneer er een nieuw bericht wordt ontvangen door de pijplijn.</td>
</tr>
<tr>
<td><strong>Berichten In proces</strong></td>
<td>Geeft het totale aantal berichten dat wordt verwerkt door de BizTalk-Service binnen een bepaalde periode.</td>
</tr>
<tr>
<td><strong>Verwerkte berichten</strong></td>
<td>Geeft het totale aantal berichten dat is verwerkt door de BizTalk Service in alle bruggen binnen een bepaalde periode. Met deze metriek wordt verhoogd wanneer een bericht wordt ontvangen door de pijplijn en is doorgestuurd naar de bestemming.</td>
</tr>
</table>


## <a name="scale"></a>Schalen
U kunt in het tabblad schalen toevoegen of verwijderen van het aantal eenheden die worden gebruikt door uw BizTalk Service. Standaard is er een eenheid geconfigureerd. Extra eenheden kunnen worden toegevoegd om uw BizTalk-Service te schalen. Als u de schaal verhoogt, wordt u om de doorvoer. De hoeveelheid resources ook stijgt, inclusief geïmplementeerde bruggen, overeenkomsten, LOB-verbindingen en verwerkingskracht. Bijvoorbeeld, verhogen u de schaal van 1 eenheid om 2 eenheden te. In dit geval kunt u dubbele het aantal bruggen implementeren, dubbele van de overeenkomsten, dubbele van de LOB-verbindingen en de verwerkingscapaciteit dubbele.

Sommige edities van BizTalk bieden een optie voor schaal. In dit geval is één eenheid toegestaan. Om te bepalen hoeveel eenheden uw editie kan worden geschaald, verwijzen naar [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md).

Het aantal eenheden mogelijk van invloed op prijzen. Als u de eenheden verhogen, selecteer **opslaan** wordt een bericht weergegeven waarin wordt aangegeven of de facturering wordt beïnvloed. U wordt dan kiezen om door te gaan. Wanneer u het aantal eenheden, de wijzigingen van de BizTalk Service-status van actief in bijwerken verhogen. In de status bijwerken blijft uw BizTalk Service om uit te voeren.

[BizTalk Services: Grafiek van edities](biztalk-editions-feature-chart.md) definieert een 'eenheid'.

## <a name="configure"></a>Configureren
Geldt niet voor hybride verbindingen.

Hiermee stelt u de Status van de back-up op None of automatisch. Wanneer ingesteld op None, worden geen back-ups automatisch gemaakt. Wanneer ingesteld op automatisch, configureert u de back-uplocatie, de frequentie van de back-up, en hoe lang de back-upbestanden houden. 

[BizTalk Services: Back-up en herstellen](biztalk-backup-restore.md) bevat de details. 

## <a name="HybridConnections"></a>Hybride verbindingen
Hybride verbindingen van verbinden een Azure-toepassing, zoals Web-Apps of Mobile Apps in Azure App Service met een on-premises resource die gebruikmaakt van een statische TCP-poort, zoals SQL Server, MySQL, HTTP-Web-API's en de meeste aangepaste webservices. Hybride verbindingen worden beheerd in BizTalk Services.

Als u wilt maken of beheren van hybride verbindingen in Azure BizTalk Services, Zie [hybride verbindingen](integration-hybrid-connection-overview.md).

## <a name="next"></a>Volgende
Nu dat u bekend met de verschillende tabbladen bent, kunt u meer informatie over de functies van Azure BizTalk Services:

* [BizTalk Services: beperking](biztalk-throttling-thresholds.md)  
* [BizTalk Services: naam en sleutel van verlener](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: back-ups maken en herstellen](biztalk-backup-restore.md)

## <a name="see-also"></a>Zie ook
* [Hybride verbindingen](integration-hybrid-connection-overview.md)  
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](biztalk-editions-feature-chart.md)  
* [BizTalk Services: inrichten](biztalk-provision-services.md)  
* [BizTalk Services: Grafiek van de status van de BizTalk-Service](biztalk-service-state-chart.md)  
* [De Azure BizTalk Services SDK gaan gebruiken](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

