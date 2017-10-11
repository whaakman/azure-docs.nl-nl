---
title: Bewaking van de VMware-oplossing in Log Analytics | Microsoft Docs
description: Meer informatie over hoe de oplossing VMware bewaking kunt logboeken beheren en controleren van ESXi-hosts.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: 17072c4b6e4fdf6e4dc2b7a6a4ded7fa9f9f6fde
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>VMware Monitoring (Preview)-oplossing in Log Analytics

![VMware symbool](./media/log-analytics-vmware/vmware-symbol.png)

De oplossing VMware bewaking in Log Analytics is een oplossing waarmee u een centrale logboekregistratie en controle benadering voor grote VMware logboeken maken. Dit artikel wordt beschreven hoe u kunt oplossen, vastleggen en beheren van de ESXi-hosts op één locatie met de oplossing. Met de oplossing kunt u gedetailleerde gegevens bekijken voor uw ESXi-hosts op één locatie. U ziet de bovenste gebeurtenis aantallen, status en trends van VM- en ESXi-hosts via de logboeken van de ESXi-host. Als u problemen met het weergeven en zoeken naar Logboeken van gecentraliseerde ESXi-host. En u waarschuwingen op basis van het logboek zoekquery's kunt maken.

De oplossing maakt gebruik van systeemeigen syslog-functionaliteit van de ESXi-host om gegevens te pushen naar een doel-virtuele machine, met OMS-Agent. De oplossing schrijven niet echter bestanden in syslog binnen een virtuele machine. De OMS-agent wordt poort 1514 geopend en luistert naar dit. Wanneer deze de gegevens ontvangt, stuurt de OMS-agent de gegevens in OMS.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om te installeren en configureren van de oplossing.

* De oplossing VMware bewaking toevoegen aan uw OMS-werkruimte met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Ondersteunde VMware ESXi-hosts
vSphere ESXi-Host 5.5 en 6.0

#### <a name="prepare-a-linux-server"></a>Bereid een server voor Linux
Maak een Linux-besturingssysteem VM alle syslog-gegevens ontvangen van de ESXi-hosts. De [OMS Linux-Agent](log-analytics-linux-agents.md) is de verzameling voor alle ESXi-host syslog-gegevens. U kunt meerdere ESXi-hosts gebruiken voor het doorsturen van logboeken met een enkel Linux-server, zoals in het volgende voorbeeld.  

   ![Syslog-stroom](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Syslog verzamelen configureren
1. Syslog-doorsturen voor VSphere instellen. Zie voor gedetailleerde informatie om u te helpen bij het opstellen van syslog doorsturen [syslog configureren op ESXi 5.x en 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Ga naar **ESXi-hostconfiguratie** > **Software** > **geavanceerde instellingen** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. In de *Syslog.global.logHost* veld, het toevoegen van de Linux-server en het poortnummer *1514*. Bijvoorbeeld, `tcp://hostname:1514` of`tcp://123.456.789.101:1514`
3. Open de hostfirewall ESXi voor syslog. **De hostconfiguratie ESXi** > **Software** > **beveiligingsprofiel** > **Firewall** en openen **Eigenschappen**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. Controleer de vSphere-Console om te controleren die syslog correct is ingesteld. Bevestig op de host ESXI die poort **1514** is geconfigureerd.
5. Download en installeer de OMS-Agent voor Linux op de Linux-server. Zie voor meer informatie de [documentatie voor de OMS-Agent voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. Nadat de OMS-Agent voor Linux is geïnstalleerd, gaat u naar de map /etc/opt/microsoft/omsagent/sysconf/omsagent.d en het vmware_esxi.conf-bestand kopiëren naar de map /etc/opt/microsoft/omsagent/conf/omsagent.d en de wijziging van de eigenaar of groep en machtigingen van het bestand. Bijvoorbeeld:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. Start de OMS-Agent opnieuw voor Linux met `sudo /opt/microsoft/omsagent/bin/service_control restart`.
8. Test de verbinding tussen de Linux-server en de ESXi-host met behulp van de `nc` opdracht op de ESXi-Host. Bijvoorbeeld:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. In de OMS-Portal te doorzoeken logboek voor `Type=VMware_CL`. Wanneer OMS syslog-gegevens worden verzameld, behoudt de syslog-indeling. In de portal voor bepaalde specifieke velden zijn vastgelegd, zoals *hostnaam* en *procesnaam*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Als weergave logboek zoekresultaten vergelijkbaar met de afbeelding hierboven zijn, bent u ingesteld op het dashboard van de oplossing OMS VMware bewaking gebruiken.  

## <a name="vmware-data-collection-details"></a>De verzameling Gegevensdetails VMware
De oplossing VMware bewaking verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van ESXi-hosts met behulp van de OMS-Agents voor Linux die u hebt ingeschakeld.

De volgende tabel bevat de methoden van de collectie en andere informatie over hoe gegevens worden verzameld.

| Platform | OMS-Agent voor Linux | SCOM-agents | Azure Storage | SCOM vereist? | SCOM-agent gegevens die worden verzonden via de beheergroep | Frequentie van de verzameling |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |om de 3 minuten |

De volgende tabel ziet u voorbeelden van gegevensvelden verzameld door de bewaking van de VMware-oplossing:

| Veldnaam | Beschrijving |
| --- | --- |
| Device_s |VMware-opslagapparaten |
| ESXIFailure_s |Fout-typen |
| EventTime_t |tijd waarop de gebeurtenis heeft plaatsgevonden |
| HostName_s |ESXi-hostnaam |
| Operation_s |virtuele machine maken of verwijderen van de virtuele machine |
| ProcessName_s |De naam van gebeurtenis |
| ResourceId_s |naam van de VMware-host |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware SCSI-status |
| SyslogMessage_s |Syslog-gegevens |
| UserName_s |gebruiker die zijn gemaakt of verwijderd van virtuele machine |
| VMName_s |VM-naam |
| Computer |hostcomputer |
| TimeGenerated |tijd die de gegevens is gegenereerd |
| DataCenter_s |VMware datacenter |
| StorageLatency_s |opslaglatentie (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Overzicht van de bewaking van de VMware-oplossing
De VMware-tegel wordt weergegeven in de OMS-portal. Het bevat een overzichtsweergave van storingen. Wanneer u op de tegel klikt, gaat u in een dashboardweergave.

![Tegel](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>De dashboardweergave navigeren
In de **VMware** dashboardweergave blades zijn gerangschikt op:

* Aantal mislukte-Status
* Telt bovenste Host door gebeurtenis
* Telt het aantal bovenste gebeurtenis
* Activiteiten van de virtuele Machine
* Gebeurtenissen van ESXi-Host schijf

![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klik op een blade als u wilt openen, logboekanalyse zoekvenster die specifiek is voor de blade ziet u gedetailleerde informatie.

Hier kunt kunt u de zoekopdracht om dit te wijzigen voor een bepaald bewerken. Bekijk voor een zelfstudie over de basisprincipes van OMS zoeken, de [OMS logboek search-zelfstudie.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Gebeurtenissen van ESXi-host vinden
Eén ESXi-host genereert meerdere logboeken, op basis van hun processen. De bewaking van de VMware-oplossing zijn ze gecentraliseerd en bevat een overzicht van het aantal gebeurtenissen. Deze centrale weergave helpt u begrijpen welke ESXi-host heeft een groot aantal gebeurtenissen en welke gebeurtenissen treden het vaakst in uw omgeving.

![Gebeurtenis](./media/log-analytics-vmware/events.png)

U kunt inzoomen verdere door te klikken op een ESXi-host of een gebeurtenistype.

Als u op de naam van een ESXi-host, kunt u gegevens vanaf deze host ESXi weergeven. Als u beperken resultaten met het gebeurtenistype wilt, voegt u `“ProcessName_s=EVENT TYPE”` in uw query. U kunt selecteren **procesnaam** in het zoekvak. Die de gegevens wordt beperkt.

![Inzoomen](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Activiteiten met hoge VM zoeken
Een virtuele machine worden gemaakt en op elke host ESXi verwijderd. Het is handig voor een beheerder om te bepalen hoeveel virtuele machines ESXi-host wordt gemaakt. Die beurt, kunt u inzicht in prestaties en capaciteitsplanning. Het bijhouden van activiteitsgebeurtenissen VM is van cruciaal belang bij het beheren van uw omgeving.

![Inzoomen](./media/log-analytics-vmware/vmactivities1.png)

Als u aanvullende ESXi host VM maken voor gegevens zien wilt, klikt u op de naam van een ESXi-host.

![Inzoomen](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Algemene zoekquery 's
De oplossing omvat andere nuttige query's die kunnen helpen bij het beheren van uw ESXi-hosts, zoals hoge opslagruimte opslaglatentie en path-fout.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Query 's](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Query's opslaan
Zoekopdrachten opslaan is een standaardfunctie in OMS en kunt u alle query's die u hebt gevonden nuttig. Nadat u een query die nuttig maakt, opslaan door te klikken op de **Favorieten**. Een opgeslagen query kunt u eenvoudig gebruiken vanuit de [mijn Dashboard](log-analytics-dashboards.md) pagina waar u uw eigen aangepaste dashboards kunt maken.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Waarschuwingen van query's maken
Nadat u uw query's hebt gemaakt, wilt u mogelijk de query's gebruiken om u te waarschuwen wanneer specifieke gebeurtenissen plaatsvinden. Zie [waarschuwingen in logboekanalyse](log-analytics-alerts.md) voor informatie over het maken van waarschuwingen. Zie voor voorbeelden van waarschuwingen, query's en andere voorbeelden van query's de [Monitor VMware met OMS Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blogbericht.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Wat moet ik doen op de ESXi hosten instelling? Wat zijn de gevolgen hebben deze op mijn huidige omgeving?
De oplossing maakt gebruik van de systeemeigen ESXi-Host Syslog mechanisme doorsturen. U hoeft geen aanvullende Microsoft-software op de ESXi Host om vast te leggen van de logboeken niet. Er moet een lage impact op uw bestaande omgeving. U hoeft echter doorsturen van syslog ESXI-functionaliteit is ingesteld.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Moet ik mijn ESXi-host starten?
Nee. Dit proces is niet opnieuw opstarten vereist. Soms vSphere niet correct bijgewerkt de syslog. In dat geval, meld u aan bij de ESXi-host en laad de syslog opnieuw. U moet niet opnieuw in en start opnieuw op de host, zodat dit proces is niet aan uw omgeving verstoren.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Kan ik verhogen of verlagen van het volume van logboekgegevens naar OMS verzonden?
U kunt Ja. U kunt de instellingen logboekniveau ESXi-Host in de vSphere. Logboekverzameling is gebaseerd op de *info* niveau. Dus als u controleren VM maken of verwijderen wilt, moet u houden de *info* niveau op Hostd. Zie voor meer informatie de [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Waarom Hostd niet-gegevens levert aan OMS? Mijn logboek is ingesteld op info.
Er is een fout ESXi-host voor de syslog-timestamp. Zie voor meer informatie de [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Nadat u de oplossing hebt toegepast, moeten Hostd normaal functioneren.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kan ik meerdere ESXi-hosts die syslog-gegevens voor een enkele virtuele machine met omsagent doorsturen hebben?
Ja. U kunt meerdere ESXi-hosts worden doorgestuurd naar een enkele virtuele machine met omsagent hebben.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Waarom zie ik niet gegevens die binnenkomen in OMS?
Er zijn meerdere redenen:

* De ESXi-host is niet correct gegevens worden gepusht naar de virtuele machine omsagent uitgevoerd. Als u wilt testen, moet u de volgende stappen uitvoeren:

  1. Om te bevestigen, meld u aan bij de ESXi-host met behulp van ssh en voer de volgende opdracht:`nc -z ipaddressofVM 1514`

      Als dit niet lukt, vSphere-instellingen in de geavanceerde configuratie zijn waarschijnlijk niet worden hersteld. Zie [syslog verzamelen configureren](#configure-syslog-collection) voor meer informatie over het instellen van de ESXi-host voor het doorsturen van syslog.
  2. Als syslog-poort verbinding geslaagd is, maar u geen gegevens nog steeds niet ziet, klikt u vervolgens opnieuw laden de syslog op de host ESXi met ssh de volgende opdracht uitvoeren:` esxcli system syslog reload`
* De virtuele machine met OMS-Agent is niet juist ingesteld. U kunt dit testen, moet u de volgende stappen uitvoeren:

  1. OMS luistert op poort 1514 en gegevens pushes in OMS. Om te controleren of dit geopend is, moet u de volgende opdracht uitvoeren:`netstat -a | grep 1514`
  2. U ziet poort `1514/tcp` openen. Als u dit niet doet, moet u controleren of de omsagent correct is geïnstalleerd. Als u de poortinformatie niet ziet, klikt u vervolgens is de syslog-poort niet geopend op de virtuele machine.

     1. Controleren of de OMS-Agent wordt uitgevoerd met behulp van `ps -ef | grep oms`. Als deze niet wordt uitgevoerd, het proces starten met de opdracht` sudo /opt/microsoft/omsagent/bin/service_control start`
     2. Open het `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf`-bestand.

         Controleer of de juiste gebruiker en groepsinstelling is geldig, vergelijkbaar met:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

         Als het bestand bestaat niet of de gebruiker en groepsinstelling is onjuist, los problemen door [voorbereiden van een Linux-server](#prepare-a-linux-server).

## <a name="next-steps"></a>Volgende stappen
* Gebruik [logboek zoekopdrachten](log-analytics-log-searches.md) in Log Analytics om gedetailleerde VMware weer te geven gegevens te hosten.
* [Maak uw eigen dashboards](log-analytics-dashboards.md) VMware hostgegevens weergegeven.
* [Waarschuwingen maken](log-analytics-alerts.md) wanneer specifieke VMware hostgebeurtenissen plaatsvinden.
