---
title: Het oplossen van problemen met de Log Analytics-agent voor Windows | Microsoft Docs
description: Beschrijf de symptomen, de oorzaken en de oplossing voor de meest voorkomende problemen met de Log Analytics-agent voor Windows in Azure Monitor.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2019
ms.author: magoedte
ms.openlocfilehash: afa4483677336e9a887908a8cccf9590eed27af3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67120108"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-windows"></a>Het oplossen van problemen met de Log Analytics-agent voor Windows 

Dit artikel bevat informatie over het oplossen van fouten die u mogelijk ondervindt met de Log Analytics-agent voor Windows in Azure Monitor en mogelijke oplossingen om op te lossen ze stelt.

Als geen van deze stappen voor u werkt, zijn ook de volgende ondersteuningskanalen beschikbaar:

* Klanten met Premier-ondersteuning voordelen kunt opent u een ondersteuningsaanvraag met [Premier](https://premier.microsoft.com/).
* Klanten met Azure support-overeenkomsten kunnen een ondersteuningsaanvraag openen [in Azure portal](https://manage.windowsazure.com/?getsupport=true).
* Ga naar de pagina Log Analytics Feedback om te controleren ingediende ideeën en bugs [ https://aka.ms/opinsightsfeedback ](https://aka.ms/opinsightsfeedback) of een nieuw bestand. 

## <a name="important-troubleshooting-sources"></a>Belangrijke bronnen voor het oplossen van problemen

 Om te helpen bij het oplossen van problemen met betrekking tot de Log Analytics-agent voor Windows, de agent legt gebeurtenissen vast in het Windows-gebeurtenislogboek specifiek onder *toepassings- en Services\Operations Manager*.  

## <a name="connectivity-issues"></a>Connectiviteitsproblemen

Als de agent via een proxyserver of firewall communiceert, kunnen er beperkingen verhindert communicatie van de broncomputer en de Azure Monitor-service. Als de communicatie wordt geblokkeerd, onjuiste configuratie, registratie bij een werkruimte mislukken terwijl de agent wilt installeren configureren van de agent na de installatie om te rapporteren aan een andere werkruimte of communicatie met de agent na een succesvolle registratie mislukt. Deze sectie beschrijft de methoden voor het oplossen van dit type probleem met de Windows-agent. 

Controleer dat de firewall of proxyserver is geconfigureerd om toe te staan de volgende poorten en URL's in de volgende tabel beschreven. Ook Bevestig HTTP-controle is niet ingeschakeld voor webverkeer te genereren, zoals het voorkomen een veilig TLS-kanaal tussen de agent en de Azure Monitor dat kan.  

|Agentresource|Poorten |Richting |HTTPS-controle overslaan|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Poort 443 |Uitgaande|Ja |  
|*.oms.opinsights.azure.com |Poort 443 |Uitgaande|Ja |  
|*.blob.core.windows.net |Poort 443 |Uitgaande|Ja |  
|*.azure-automation.net |Poort 443 |Uitgaande|Ja |  

Zie voor firewall-informatie is vereist voor Azure Government, [beheer van Azure Government](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Er zijn verschillende manieren kunt u controleren of als de agent met Azure Monitor communiceert.

- Schakel de [evaluatie van de status van Azure Log Analytics-Agent](../insights/solution-agenthealth.md) in de werkruimte. Vanuit het dashboard status van Agent weergeven de **aantal agents niet heeft gereageerd** kolom om snel te zien als de agent wordt vermeld.  

- De volgende query uit om te bevestigen dat de agent verzendt een heartbeat naar de werkruimte die is geconfigureerd voor het rapport om te worden uitgevoerd. Vervang <ComputerName> door de werkelijke naam van de machine.

    ```
    Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer 
    ```

    Als de computer met de service communiceert, moet de query een resultaat retourneren. Als de query heeft geen een resultaat geretourneerd, Controleer eerst of dat de agent is geconfigureerd om te rapporteren aan de juiste werkruimte. Als deze juist is geconfigureerd, gaat u verder met stap 3 en zoeken naar het Windows-gebeurtenislogboek om te bepalen of de agent aanmeldt met het probleem mogelijk worden zich voorkomen dat communicatie met Azure Monitor.

- Een andere methode voor het identificeren van een probleem met de netwerkverbinding is door het uitvoeren van de **TestCloudConnectivity** hulpprogramma. Het hulpprogramma wordt standaard geïnstalleerd met de agent in de map *%SystemRoot%\Program Files\Microsoft Monitoring Agent\Agent*. Navigeer naar de map en het hulpprogramma uitvoeren vanaf een opdrachtprompt met verhoogde bevoegdheid. Het hulpprogramma retourneert de resultaten en belangrijke functies waar de test is mislukt (bijvoorbeeld, als deze is gerelateerd aan een bepaalde poort/URL die is geblokkeerd). 

    ![Resultaten van het hulpprogramma uitvoeren TestCloudConnection](./media/agent-windows-troubleshoot/output-testcloudconnection-tool-01.png)

- Filter de *Operations Manager* gebeurtenislogboek door **gebeurtenisbronnen** - *Health-servicemodules*, *HealthService*, en *Serviceconnector* en filteren op **gebeurtenisniveau** *waarschuwing* en *fout* om te controleren als deze gebeurtenissen van geschreven de onderstaande tabel. Als ze zich bevinden, raadpleegt u de stappen die voor elke gebeurtenis die mogelijk zijn opgenomen.

    |Gebeurtenis-id |source |Description |Oplossing |
    |---------|-------|------------|-----------|
    |2133 & 2129 |Health-Service |Verbinding met de service van de agent is mislukt |Deze fout kan optreden wanneer de agent kan niet rechtstreeks of via een firewall/proxy-server naar de service Azure Monitor communiceren. Controleer of dat de netwerk-firewall/proxy staat de TCP-verkeer van de computer met de service of agent-proxy-instellingen.|
    |2138 |Health-servicemodules |Proxy verificatie is vereist |Configureer de agent-proxy-instellingen en geef de gebruikersnaam en wachtwoord vereist om te verifiëren met de proxyserver. |
    |2129 |Health-servicemodules |Mislukte verbinding mislukt SSL-onderhandeling |Controleer uw instellingen voor netwerkadapters TCP/IP- en agent-proxy-instellingen.|
    |2127 |Health-servicemodules |Fout bij het verzenden van gegevens van foutcode |Als dit alleen tijdens de dag regelmatig gebeurt, kan het zojuist een willekeurige afwijkingen die kan worden genegeerd zijn. Monitor om te begrijpen hoe vaak dit gebeurt. Als dit vaak gedurende de dag gebeurt, controleert u eerst uw netwerkconfiguratie en proxy-instellingen. Als de beschrijving van de HTTP-foutcode 404 bevat en is de eerste keer is dat de agent probeert gegevens te verzenden naar de service, bevat deze een 500 fout opgetreden bij een binnenste 404-fout op te geven. 404-fout betekent dat niet wordt gevonden, wat aangeeft dat het opslaggebied voor de nieuwe werkruimte wordt nog ingericht. Volgende opnieuw wordt uitgevoerd, wordt met succes schrijven van gegevens naar de werkruimte zoals verwacht. Een HTTP-fout 403 kan duiden op een machtiging of referenties probleem. Er is informatie opgenomen in de 403-fout bij het oplossen van het probleem.|
    |4000 |Service-Connector |DNS-naamomzetting is mislukt |De machine kan niet omzetten voor de Internet-adres dat wordt gebruikt bij het verzenden van gegevens naar de service. Dit wordt mogelijk DNS-resolverinstellingen op uw computer, onjuist proxy-instellingen of mogelijk een tijdelijke DNS-probleem met uw provider. Als dit regelmatig gebeurt, kan dit worden veroorzaakt door een tijdelijk netwerkprobleem.|
    |4001 |Service-Connector |Kan geen verbinding maken met de service. |Deze fout kan optreden wanneer de agent kan niet rechtstreeks of via een firewall/proxy-server naar de service Azure Monitor communiceren. Controleer of dat de netwerk-firewall/proxy staat de TCP-verkeer van de computer met de service of agent-proxy-instellingen.|
    |4002 |Service-Connector |De service heeft HTTP-statuscode 403 geretourneerd in antwoord op een query. Neem contact op met de servicebeheerder de status van de service. De query wordt later opnieuw geprobeerd. |Deze fout tijdens de registratie-fase van de agent wordt geschreven en ziet u een vergelijkbaar met de volgende URL: *https://<workspaceID>.oms.opinsights.azure.com/AgentService.svc/AgentTopologyRequest*. Een fout 403 betekent dat het is niet toegestaan code en kan worden veroorzaakt door een verkeerd getypt werkruimte-ID of sleutel, of de datum en tijd op de computer onjuist is. Als de tijd +/-15 minuten na de huidige tijd is, mislukt onboarding. Bijgewerkt om dit te corrigeren, de datum en/of de tijdzone van uw Windows-computer.|

## <a name="data-collection-issues"></a>Problemen met het verzamelen van gegevens

Nadat de agent is geïnstalleerd en de rapporten naar de geconfigureerde werkruimte of werkruimten, het mogelijk niet ontvangen configuratie, verzamelen of doorsturen van prestaties, Logboeken of andere gegevens naar de service, afhankelijk van wat is ingeschakeld en die gericht is op de computer. Dit is nodig om te bepalen of:

- Is het gegevenstype van een bepaalde of alle gegevens die in de werkruimte niet beschikbaar is?
- Is het gegevenstype opgegeven door een oplossing of als onderdeel van de werkruimte-configuratie voor het verzamelen van gegevens?
- Hoeveel computers zijn beïnvloed? Is het een of meerdere computers die rapporteren aan de werkruimte?
- Is dit werkt en is deze niet op een bepaald tijdstip van de dag of heeft deze nooit zijn verzameld? 
- Is het log-zoekquery u syntactisch juiste? 
- Er is de agent ooit ontvangen de configuratie van Azure Monitor?

De eerste stap bij het oplossen van problemen is om te bepalen als de computer een heartbeat-gebeurtenis wordt verzonden.

```
Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer
```

Als de query resultaten worden geretourneerd, moet u vaststellen of een bepaald type niet wordt verzameld en doorgestuurd naar de service. Dit wordt mogelijk veroorzaakt door de agent ontvangt geen bijgewerkte configuratie van de service of een ander symptoom zo wordt voorkomen dat de agent werken normaal. Voer de volgende stappen voor het verder worden opgelost.

1. Open een opdrachtprompt met verhoogde bevoegdheid op de computer en de agentservice opnieuw starten door te typen `net stop healthservice && net start healthservice`.
2. Open de *Operations Manager* gebeurtenislogboek en zoek naar **gebeurtenis-id's** *7023, 7024, 7025, 7028* en *1210* van **gebeurtenis bron** *HealthService*.  Deze gebeurtenissen geven aan de agent is configuratie van Azure Monitor ontvangt en ze zijn actief bewaken van de computer. De beschrijving van de gebeurtenis voor gebeurtenis-ID 1210 ook op de laatste geeft regel alle oplossingen en inzichten die zijn opgenomen in het bereik van de bewaking op de agent.  

    ![Beschrijving van gebeurtenis-ID 1210](./media/agent-windows-troubleshoot/event-id-1210-healthservice-01.png)

3. Als u na enkele minuten u niet de verwachte gegevens in de queryresultaten of visualisatie ziet, afhankelijk van of u bekijkt de gegevens van een oplossing of inzichten te verkrijgen, uit de *Operations Manager* gebeurtenislogboek en zoek naar **gebeurtenis bronnen** *HealthService* en *Health-servicemodules* en filteren op **gebeurtenisniveau** *waarschuwing* en *Fout* om te controleren als deze gebeurtenissen in de volgende tabel geschreven.

    |Gebeurtenis-id |source |Description |Oplossing |
    |---------|-------|------------|
    |8000 |HealthService |Deze gebeurtenis geeft als een werkstroom die betrekking hebben op prestaties, gebeurtenis, of een andere gegevens die worden verzameld, is niet kan worden doorgestuurd naar de service voor opname in de werkruimte. | Gebeurtenis-ID 2136 van bron Health-service, samen met deze gebeurtenis is geschreven en kan duiden op de agent kan niet communiceren met de service, mogelijk vanwege een onjuiste configuratie van de proxy- en -verificatie-instellingen, netwerkstoringen of de firewall van het netwerk is / proxy staat niet toe dat TCP-verkeer van de computer met de service.| 
    |10102 en 10103 |Health-servicemodules |Werkstroom kan gegevensbron niet omzetten. |Dit kan gebeuren als de opgegeven prestatiemeteritem of het exemplaar niet op de computer bestaat of onjuist is gedefinieerd in de werkruimte instellingen. Als dit een door de gebruiker gedefinieerde [prestatiemeteritem](data-sources-performance-counters.md#configuring-performance-counters), Controleer of de opgegeven informatie volgt de juiste indeling en bestaat op de doelcomputers. |
    |26002 |Health-servicemodules |Werkstroom kan gegevensbron niet omzetten. |Dit kan gebeuren als het opgegeven Windows-gebeurtenislogboek niet op de computer bestaat. Deze fout kan veilig worden genegeerd als de computer niet verwacht wordt dat dit gebeurtenislogboek geregistreerd, anders als dit een door de gebruiker gedefinieerde [gebeurtenislogboek](data-sources-windows-events.md#configuring-windows-event-logs), Controleer of de opgegeven informatie juist is. |

