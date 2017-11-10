---
title: Dashboard, bewaken, schaal, configureren, en hybride verbindingen in BizTalk Services | Microsoft Docs
description: Meer informatie over de besturingselementen en prestaties voor BizTalk Services controleren
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 351809cd5f165a863dc02bfadf78fa59cbaabfd7
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>De tabbladen Dashboard, Controleren, Schaal, Configureren en Hybride verbinding controleren

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Nadat u uw BizTalk Service maken en implementeren van uw toepassing, kunt u enkele van de BizTalk Service-instellingen wijzigen en controleren van de toepassingsprestaties. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Hiermee wordt een nieuw venster geopend met de volgende tabbladen. Dit onderwerp beschrijft deze tabbladen.

## <a name="quickstart-quickstartquickstart"></a>Quick Start)![Snelstartgids][Quickstart])
Afhankelijk van de editie van BizTalk Services, alle vermelde opties mogelijk niet beschikbaar. 

<table border="1">
    <tr>
        <td><strong>Download de hulpprogramma 's</strong></td>
        <td>Download de BizTalk Services SDK voor het installeren van de Visual Studio-projectsjablonen op de lokale computer. Deze sjablonen maken het <strong>BizTalk Services</strong> (brug) en de <strong>BizTalk serviceartefacten</strong> (transformatie) Visual Studio-projecten die zijn geïmplementeerd voor uw BizTalk Service.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hoe Start ik met de Azure BizTalk Services SDK </a> en <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installeren van de Azure BizTalk Services SDK</a> vermeldt de stappen om te beginnen.
        </td>
    </tr>
    <tr>
        <td><strong>Partner overeenkomsten maken</strong></td>
        <td>Hiermee opent u de Azure BizTalk Services-Portal gehost op Azure, waarbij het toevoegen van partners en X12, AS2, maken en EDIFACT EDI-overeenkomsten.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configureren van onderdelen voor EDI-berichten op BizTalk Services Portal</a> vermeldt de stappen om te beginnen.
        </td>
    </tr>

<tr>
        <td><strong>Meer informatie over BizTalk Services</strong></td>
        <td>Ga naar de <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> voor meer informatie over Azure BizTalk Services.</td>
</tr>
</table>


U kunt in de taakbalk onder:

<table border="1">

<tr>
<td><strong>Beheren</strong> implementatie van uw toepassing</td>
<td>Hiermee opent u de Azure BizTalk Services-portal. De BizTalk Services-Portal is de toegang tot EDI-configuratie, inclusief partners toevoegen en X12, AS2, maken en EDIFACT-overeenkomsten.
<br/><br/>
Dit is hetzelfde als <strong>partner overeenkomsten maken</strong> op de <strong>Quick Start</strong> tabblad.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configureren van onderdelen voor EDI-berichten op BizTalk Services Portal</a> bevat meer informatie over de BizTalk Services-Portal.</td>
</tr>

<tr>
<td><strong>Verbindingsgegevens</strong> van de Access Control-Namespace</td>
<td>Wanneer u de verbindingsgegevens selecteert, klikt u vervolgens de Access Control Namespace, standaard verlener en sleutel standaard weergegeven. U kunt deze waarden kopiëren.
<br/><br/>
U kunt ook de Access Control-beheerportal openen. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Maken van een toegangsbeheer Namespace</a> bevat meer informatie over de Access Control-beheerportal.</td>
</tr>

<tr>
<td><strong>Sleutels synchroniseren</strong> in het Opslagaccount</td>
<td>Wanneer u een opslagaccount maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze versleutelingssleutels beheren toegang tot uw Storage-Account. Uw BizTalk Service gebruikt automatisch de primaire sleutel. <strong>Sleutels synchroniseren</strong> kunnen gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.
<br/><br/>
U wilt dat de BizTalk Service met een nieuwe primaire sleutel voor het Opslagaccount. Om dit te doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer de secundaire sleutel. Als u dit doet, wordt de BizTalk Service wordt gestart met behulp van de secundaire sleutel.</li>
<li>Selecteer uw Storage-account en de primaire sleutel opnieuw genereren. Vergeet niet uw BizTalk Service met behulp van de secundaire sleutel.</li>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>Selecteer uw Storage-account en de secundaire sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'rollover voor sleutels' genoemd. Het doel is om gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.</td>
</tr>

<tr>
<td><strong>Verwijder</strong> uw toepassing</td>
<td>Wanneer u selecteert verwijderen, uw BizTalk Service en alle items die zijn geïmplementeerd, worden verwijderd.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Afhankelijk van de editie van BizTalk Services, alle vermelde opties mogelijk niet beschikbaar. 

Wanneer u de naam van uw BizTalk Service selecteert, wordt het tabblad Dashboard wordt weergegeven. In het Dashboard kunt u:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Overzicht van gebruik: Toont het aantal gebruikte hybride verbindingen
Het gebruik van de gegevens worden ook weergegeven in GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrische grafiek: Toont een vaste lijst maatstaven voor prestaties
Deze metrische gegevens opgeven realtime waarden met betrekking tot de status van de BizTalk Service. U kunt ook de **relatieve** of **Absolute** waarden en het tijdsbereik **Interval** van de metrische gegevens die worden weergegeven in de grafiek. 

Voor een beschrijving van deze maatstaven voor prestaties, gaat u naar [beschikbare metrische gegevens](#Metrics) in dit onderwerp.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Snelle weergave: Geeft een lijst van uw BizTalk Service-eigenschappen
<table border="1">

<tr>
<td><strong>Database bijhouden referenties bijwerken</strong></td>
<td>De gebruikersnaam en wachtwoord gebruikt voor aanmelding bij de Database voor het bijhouden van wijzigingen.</td>
</tr>
<tr>
<td><strong>SSL-certificaat bijwerken</strong></td>
<td>Kan de BizTalk Service voor het gebruik van een ander SSL-certificaat bijwerken. Een zelfondertekend SSL-certificaat wordt automatisch gemaakt wanneer u <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">de BizTalk Service maakt</a>.</td>
</tr>
<tr>
<td><strong>Certificaat downloaden</strong></td>
<td>U kunt het SSL-certificaat gebruikt door uw BizTalk Service naar een lokale computer downloaden.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Geeft de huidige status van uw BizTalk Service. Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Statusgrafiek Service</a>. </td>
</tr>
<tr>
<td><strong>Service-URL</strong></td>
<td>De URL voor uw BizTalk Service. Dit is hetzelfde als de <strong>domein-URL</strong> ingevoerd bij het maken van uw BizTalk Service.</td>
</tr>
<tr>
<td><strong>Adres van de openbare virtuele IP (VIP)</strong></td>
<td>Het IP-adres wordt toegewezen aan uw BizTalk Service. Het wordt gebruikt voor alle invoereindpunten en het bronadres voor uitgaand verkeer. Dit IP-adres behoort tot uw BizTalk Service wanneer deze is gemaakt. Als u de BizTalk Service verwijdert, wordt het IP-adres toegewezen aan een andere BizTalk Service.</td>
</tr>
<tr>
<td><strong>ACS-Namespace</strong></td>
<td>Verifieert met de BizTalk Service.</td>
</tr>
<tr>
<td><strong>Editie</strong></td>
<td>Een lijst met de editie ingevoerd bij het maken van de BizTalk Service.</td>
</tr>
<tr>
<td><strong>Locatie</strong></td>
<td>Geeft de geografische regio die als host fungeert voor uw BizTalk Service.</td>
</tr>
<tr>
<td><strong>Gemaakt</strong></td>
<td>Geeft de datum en tijd waarop die de BizTalk Service is gemaakt.</td>
</tr>
<tr>
<td><strong>Traceringsdatabase</strong></td>
<td>De naam van het Azure SQL Database die de Traceringstabellen gebruikt door uw BizTalk Service worden opgeslagen. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten wat</a> biedt details over de Database bijhouden.</td>
</tr>
<tr>
<td><strong>Opslag voor bewaken/archiveren</strong></td>
<td>De naam van de Azure Storage-account waarmee de uitvoer van de bewaking van uw BizTalk Service worden opgeslagen.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten wat</a> biedt details over de Storage-account.</td>
</tr>
<tr>
<td><strong>De naam van abonnement</strong></td>
<td>Het abonnement dat als host fungeert voor uw BizTalk Service bevat. Het abonnement bepaalt toegang.</td>
</tr>
<tr>
<td><strong>Abonnements-ID</strong></td>
<td>Wanneer een abonnement is gemaakt, wordt er automatisch een abonnements-ID gegenereerd. Wanneer u de REST-API's, moet u mogelijk opgeven de abonnement-ID.</td>
</tr>
</table>

[BizTalk Services: Inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280) vermeldt de stappen voor het maken van een BizTalk Service.

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Beheren van de verbindingsinformatie, de sleutels synchroniseren, en verwijder in de taakbalk:
<table border="1">

<tr>
<td><strong>Beheren</strong> implementatie van uw toepassing</td>
<td>Hiermee opent u de Azure BizTalk Services-Portal. De BizTalk Services-Portal is de toegang tot EDI-configuratie, inclusief partners toevoegen en X12, AS2, maken en EDIFACT-overeenkomsten.
<br/><br/>
Dit is hetzelfde als <strong>partner overeenkomsten maken</strong> op de <strong>Quick Start</strong> tabblad.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configureren van onderdelen voor EDI-berichten op BizTalk Services Portal</a> bevat meer informatie over de BizTalk Services-Portal.</td>
</tr>
<tr>
<td><strong>Verbindingsgegevens</strong> van de Access Control-Namespace</td>
<td>Geeft de Access Control Namespace, standaard verlener en standaard sleutelwaarden; die kunnen worden gekopieerd.
<br/><br/>
U kunt ook de Access Control-beheerportal openen. Deze Access Control-beheerportal is hetzelfde als wanneer u de optie Active Directory in het navigatiedeelvenster links.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Het beheren van uw ACS-Namespace</a> bevat meer informatie over de Access Control-beheerportal.</td>
</tr>
<tr>
<td><strong>Sleutels synchroniseren</strong> in het Opslagaccount</td>
<td>Wanneer u een opslagaccount maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze versleutelingssleutels beheren toegang tot uw Storage-Account. Uw BizTalk Service gebruikt automatisch de primaire sleutel. <strong>Sleutels synchroniseren</strong> kunnen gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.
<br/><br/>
U wilt dat de BizTalk Service met een nieuwe primaire sleutel voor het Opslagaccount. Om dit te doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer de secundaire sleutel. Als u dit doet, wordt de BizTalk Service wordt gestart met behulp van de secundaire sleutel.</li>
<li>Selecteer uw Storage-account en de primaire sleutel opnieuw genereren. Vergeet niet uw BizTalk Service met behulp van de secundaire sleutel.</li>
<li>Selecteer uw BizTalk Service en selecteer <strong>sleutels synchroniseren</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>Selecteer uw Storage-account en de secundaire sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'rollover voor sleutels' genoemd. Het doel is om gebruikers schakelen tussen de primaire sleutel en de secundaire sleutel zonder te onderbreken van de BizTalk Service.</td>
</tr>

<tr>
<td><strong>Verwijder</strong> uw toepassing</td>
<td>Uw BizTalk Service en alle items die zijn geïmplementeerd, worden verwijderd.</td>
</tr>
</table>


## <a name="monitor"></a>Bewaken
Niet van toepassing op de editie Free.

Wanneer u de naam van uw BizTalk Service selecteert, wordt het tabblad Monitor beschikbaar en wordt het volgende weergegeven:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrische grafiek: Geeft de geselecteerde prestatiewaarden
Deze metrische gegevens opgeven realtime waarden met betrekking tot de status van de BizTalk Service. U kiezen welke maatstaven voor prestaties worden weergegeven. Maximaal zes maatstaven voor prestaties kan tegelijkertijd worden weergegeven. 

U kunt ook de **relatieve** of **Absolute** waarden en het tijdsbereik **Interval** van de metrische gegevens die worden weergegeven. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Om te verwijderen of metrische gegevens weergeven in de grafiek:
1. Selecteer de **Monitor** tabblad.
2. Selecteer **toevoegen metrische gegevens** in de taakbalk:  
   ![Selecteer de metrische gegevens toevoegen][AddMetrics]
3. Controleer de maatstaven voor prestaties die u wilt weergeven.
4. Selecteer het vinkje om terug te keren naar de **Monitor** tabblad.
5. Selecteer de cirkel naast de metrische gegevens om weer te geven die metrische waarde in de grafiek.  
   
    Bijvoorbeeld, de **CPU-gebruik** metriek is niet beschikbaar; de uitvoer wordt niet weergegeven in de grafiek:  
   ![CPU-gebruik metriek is niet beschikbaar][GrayedMetric]  
   
    Selecteer de grijs uit cirkel om in te schakelen de **CPU-gebruik** metrische gegevens om de uitvoer ervan weergegeven in de grafiek weer te geven:  
   ![CPU-gebruik metrische gegevens is ingeschakeld][EnabledMetric]
6. Als een waarde van de grafiek weergeven en de lijst, selecteert u **metriek verwijderen** in de taakbalk. Als u wilt de metrische gegevens weer toe te voegen aan de lijst, selecteert u **toevoegen metrische gegevens** in de taakbalk de metrische gegevens controleren en selecteer het vinkje om terug te keren naar de **Monitor** tabblad. Selecteer de grijs uit cirkel om in te schakelen, de metriek.

## <a name="Metrics"></a>Beschikbare metrische gegevens
De volgende prestatiemeteritems/maatstaven voor prestaties zijn beschikbaar:

<table border="1">

<tr>
<td><strong>RountdTrip latentie</strong></td>
<td>Geeft de gemiddelde tijd in milliseconden (ms) voor het verwerken van een bericht van de tijd die het bericht wordt ontvangen totdat het bericht volledig is verwerkt door de BizTalk Service weer over alle bruggen. Alleen de verwerkte berichten worden meegeteld.<br/><br/>
Wanneer de volgende gebeurtenissen plaatsvinden, wordt een tijdstempel gemaakt:
<ul>
<li>Bericht krijgt de gateway</li>
<li>Bericht wordt doorgestuurd naar de bestemming</li>
<li>Bestemming reactie wordt ontvangen</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>
Met deze metriek wordt het resultaat van de volgende berekening:
<br/><br/>
[Bestemming bevestiging antwoord verzonden naar de gateway] - [bericht krijgt de gateway]</td>
</tr>
<tr>
<td><strong>Fouten bij de bron</strong></td>
<td>Geeft het totaal aantal berichten die niet door de BizTalk Service bij het ophalen van berichten van de bron-eindpunten.</td>
</tr>
<tr>
<td><strong>CPU-gebruik</strong></td>
<td>Geeft het gemiddelde percentage processortijd van alle rolinstanties.</td>
</tr>
<tr>
<td><strong>Latentie verwerken</strong></td>
<td>Geeft de gemiddelde tijd In milliseconden (ms) in een bericht te verwerken door de BizTalk Service weer over alle bruggen, met uitzondering van de tijd doorgebracht in bestemmingen. Alleen de verwerkte berichten worden meegeteld.<br/><br/>
Wanneer elk van de volgende gebeurtenissen plaatsvinden, wordt er een tijdstempel gemaakt:

<ul>
<li>Bericht krijgt de gateway</li>
<li>Bericht wordt doorgestuurd naar de bestemming</li>
<li>Bestemming reactie wordt ontvangen</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>Met deze metriek wordt het resultaat van de volgende berekening:<br/><br/>
[Bestemming bevestiging antwoord verzonden naar de gateway] - [bericht krijgt de gateway] - [bestemming reactie wordt ontvangen] + [bericht wordt doorgestuurd naar de bestemming]</td>
</tr>
<tr>
<td><strong>Fouten In het proces</strong></td>
<td>Geeft het totaal aantal berichten die zijn mislukt bij de verwerking door de BizTalk Service via de bruggen binnen een periode.</td>
</tr>
<tr>
<td><strong>Berichten die worden verzonden</strong></td>
<td>Geeft het totaal aantal berichten is verzonden door de BizTalk Service via alle bruggen binnen een periode. Met deze metriek wordt verhoogd wanneer er een bericht verzonden vanuit een pijplijn de route-bestemming heeft bereikt. Met deze metriek geeft niet aan een bericht is verwerkt.<br/><br/>
In een Request-Reply-scenario worden de metriek wordt verhoogd als de bestemming route een ontvangstbevestiging teruggestuurd naar de pijplijn wordt.</td>
</tr>
<tr>
<td><strong>Berichten ontvangen</strong></td>
<td>Geeft het totaal aantal berichten is ontvangen door de BizTalk Service via alle bruggen binnen een periode. Met deze metriek wordt verhoogd wanneer een nieuw bericht wordt ontvangen door de pijplijn.</td>
</tr>
<tr>
<td><strong>Berichten In proces</strong></td>
<td>Geeft het totaal aantal berichten wordt verwerkt door de BizTalk Service binnen een periode.</td>
</tr>
<tr>
<td><strong>Berichten die zijn verwerkt</strong></td>
<td>Geeft het totale aantal berichten dat is verwerkt door de BizTalk Service via alle bruggen binnen een periode. Met deze metriek wordt verhoogd wanneer er een bericht wordt ontvangen door de pijplijn en is doorgestuurd naar de bestemming.</td>
</tr>
</table>


## <a name="scale"></a>Schalen
U kunt toevoegen of verwijderen van het aantal eenheden dat door uw BizTalk Service gebruikt in het tabblad schaal. Er is standaard één eenheid worden geconfigureerd. Extra eenheden kunnen worden toegevoegd aan het schalen van uw BizTalk Service. Als u de schaal verhoogt, bent u doorvoer te verbeteren. De hoeveelheid resources verhoogt ook, inclusief geïmplementeerde bruggen, overeenkomsten, LOB-verbindingen en verwerkingskracht. Bijvoorbeeld, verhoogt u de schaal van 1 eenheid tot 2 eenheden. In dit geval kunt u dubbele het aantal bruggen implementeren, dubbelklik de overeenkomsten, dubbelklik de LOB-verbindingen en dubbelklik de verwerkingskracht.

Sommige edities van BizTalk bieden geen een optie voor schaal. In dit geval is één eenheid toegestaan. Om te bepalen hoeveel eenheden uw editie kan worden geschaald, verwijzen naar [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md).

Het aantal eenheden verhogen mogelijk van invloed prijzen. Als u de eenheden verhogen, selecteer **opslaan** wordt een bericht weergegeven dat aangeeft of facturering wordt beïnvloed. U wordt dan kiezen om door te gaan. Wanneer u het aantal eenheden, de BizTalk Service status is gewijzigd van actief in Updating verhogen. Uw BizTalk Service blijft uitvoeren in de status bijgewerkt.

[BizTalk Services: Grafiek van edities](biztalk-editions-feature-chart.md) definieert een 'eenheid'.

## <a name="configure"></a>Configureren
Geldt niet voor hybride verbindingen.

De back-Status wordt ingesteld op None of automatisch. Als op None is ingesteld, worden geen back-ups automatisch gemaakt. Als de waarde op automatisch, configureert u de back-uplocatie, de frequentie van de back-up, en hoe lang de back-bestanden moeten worden behouden. 

[BizTalk Services: Back-up en herstel](biztalk-backup-restore.md) bevat de details. 

## <a name="HybridConnections"></a>Hybride verbindingen
Hybride verbinding wordt een Azure-toepassing, zoals Web-Apps of mobiele Apps in Azure App Service met een on-premises resource die gebruikmaakt van een statische TCP-poort, zoals SQL Server, MySQL, HTTP-Web-API's en de meeste aangepaste webservices. Hybride verbindingen worden beheerd in BizTalk Services.

Als u wilt maken of beheren van hybride verbindingen in de Azure BizTalk Services, Zie [hybride verbindingen](integration-hybrid-connection-overview.md).

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
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

