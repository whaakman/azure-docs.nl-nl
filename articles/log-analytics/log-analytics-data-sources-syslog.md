---
title: Verzamelen en analyseren van Syslog-berichten in OMS Log Analytics | Microsoft Docs
description: Syslog is een gebeurtenis logboekregistratie-protocol die geldt voor Linux. In dit artikel wordt beschreven hoe verzameling van Syslog-berichten configureren voor logboekanalyse en details van de records die ze in de OMS-opslagplaats maken.
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 061c32fe39530f8b67899b1b9e1104e7fe006380
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a>Syslog-gegevensbronnen in Log Analytics
Syslog is een gebeurtenis logboekregistratie-protocol die geldt voor Linux.  Toepassingen worden verzonden berichten die kunnen worden opgeslagen op de lokale computer of naar een Syslog-collector geleverd.  Wanneer de OMS-Agent voor Linux is geïnstalleerd, configureert het de lokale Syslog-daemon voor het doorsturen van berichten naar de agent.  De agent verzendt vervolgens het bericht met logboekanalyse waarop een record in de OMS-opslagplaats is gemaakt.  

> [!NOTE]
> Log Analytics biedt ondersteuning voor verzameling van berichten is verzonden door rsyslog of syslog-ng, waarbij rsyslog de standaard-daemon is. De standaard syslog-daemon op versie 5 van Red Hat Enterprise Linux en CentOS, Oracle Linux-versie (sysklog) wordt niet ondersteund voor de verzameling van syslog-gebeurtenis. Syslog-gegevens te verzamelen van deze versie van deze verdelingen de [rsyslog daemon](http://rsyslog.com) moet worden geïnstalleerd en geconfigureerd ter vervanging van sysklog.
>
>

![Syslog-verzameling](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Syslog configureren
De OMS-Agent voor Linux worden alleen verzameld van gebeurtenissen met de faciliteiten en de ernst die zijn opgegeven in de configuratie.  U kunt Syslog configureren via de OMS-portal of met het beheren van configuratiebestanden op uw Linux-agents.

### <a name="configure-syslog-in-the-oms-portal"></a>Syslog configureren in de OMS-portal
Configureer Syslog uit de [menu Data in logboekanalyse-instellingen](log-analytics-data-sources.md#configuring-data-sources).  Deze configuratie wordt doorgegeven naar het configuratiebestand op elke Linux-agent.

U kunt een nieuwe opslagruimte toevoegen door in de naam te typen en op  **+** .  Voor elke faciliteit worden alleen berichten met de geselecteerde ernst verzameld.  Controleer de ernst voor de bepaalde opslagruimte die u wilt verzamelen.  U kunt aanvullende criteria om berichten te filteren kan niet opgeven.

![Syslog configureren](media/log-analytics-data-sources-syslog/configure.png)

Standaard worden alle configuratiewijzigingen automatisch naar alle agents gepusht.  Als u wilt Syslog handmatig configureren op elke Linux-agent, schakel het selectievakje *toepassen op de onderstaande configuratie op mijn Linux-machines*.

### <a name="configure-syslog-on-linux-agent"></a>Syslog op Linux-agent configureren
Wanneer de [OMS-agent is geïnstalleerd op een client voor Linux](log-analytics-linux-agents.md), het installeren van een standaard syslog-configuratiebestand dat definieert de opslagruimte en de ernst van de berichten die worden verzameld.  U kunt dit bestand als u de configuratie wijzigen.  Het configuratiebestand is verschillend, afhankelijk van de Syslog-daemon die op de client is geïnstalleerd.

> [!NOTE]
> Als u de syslog-configuratie hebt bewerkt, moet u opnieuw de syslog-daemon om de wijzigingen van kracht te laten worden.
>
>

#### <a name="rsyslog"></a>Rsyslog
Het configuratiebestand voor rsyslog bevindt zich op **/etc/rsyslog.d/95-omsagent.conf**.  De Standaardinhoud worden hieronder weergegeven.  Hiermee verzamelt syslog-berichten is verzonden vanaf de lokale agent voor alle installaties met een niveau van de waarschuwing of hoger.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

U kunt een faciliteit verwijderen door het verwijderen van de sectie van het configuratiebestand.  U kunt de ernst die door het wijzigen van deze faciliteit vermelding voor een bepaalde opslagruimte worden verzameld beperken.  Bijvoorbeeld, als u wilt beperken van de installatie van de gebruiker voor de berichten met een ernst van de fout of hoger moet u die regel van het configuratiebestand als volgt wijzigen:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng
Het configuratiebestand voor de syslog-ng is de locatie op **/etc/syslog-ng/syslog-ng.conf**.  De Standaardinhoud worden hieronder weergegeven.  Dit verzamelt syslog-berichten is verzonden vanaf de lokale agent voor alle installaties en alles.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

U kunt een faciliteit verwijderen door het verwijderen van de sectie van het configuratiebestand.  U kunt de ernst die door deze te verwijderen uit de lijst voor een bepaalde opslagruimte worden verzameld beperken.  Bijvoorbeeld, om te beperken van de faciliteit gebruiker alleen waarschuwingen en kritieke berichten, moet u die sectie van het configuratiebestand als volgt wijzigen:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Verzamelen van gegevens van aanvullende Syslog-poorten
De OMS-agent luistert naar de Syslog-berichten op de lokale client op poort 25224.  Wanneer de agent is geïnstalleerd, wordt een standaardconfiguratie syslog toegepast en gevonden in de volgende locatie:

* Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`

U kunt het poortnummer wijzigen door het maken van twee configuratiebestanden: een configuratiebestand FluentD en een bestand rsyslog of syslog-ng, afhankelijk van de Syslog-daemon die u hebt geïnstalleerd.  

* Het configuratiebestand FluentD moet een nieuw bestand in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` en vervang de waarde in de **poort** vermelding met het nummer van uw aangepaste poort.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* Voor rsyslog, moet u een nieuw configuratiebestand in maken: `/etc/rsyslog.d/` en vervang de waarde % SYSLOG_PORT % met uw aangepaste poortnummer.  

    > [!NOTE]
    > Als u deze waarde in het configuratiebestand wijzigt `95-omsagent.conf`, wordt deze overschreven wanneer de agent van toepassing een standaardconfiguratie is.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* De syslog-ng config moet worden gewijzigd door te kopiëren van de voorbeeldconfiguratie van de hieronder wordt weergegeven en de aangepaste gewijzigde instellingen toe te voegen aan het einde van het configuratiebestand van de syslog-ng.conf te vinden in `/etc/syslog-ng/`.  Voer **niet** gebruiken het standaardlabel **% WORKSPACE_ID % _oms** of **% WORKSPACE_ID_OMS**, aangepast etiket om uw wijzigingen onderscheid te kunnen definiëren.  

    > [!NOTE]
    > Als u de standaardwaarden in het configuratiebestand wijzigt, wordt deze overschreven wanneer de agent van toepassing een standaardconfiguratie is.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Na het voltooien van de wijzigingen, de Syslog en de OMS-agent moet-service opnieuw worden opgestart zodat de configuratiewijzigingen van kracht.   

## <a name="syslog-record-properties"></a>Eigenschappen van de record Syslog
Syslog-records hebben een soort **Syslog** en de eigenschappen in de volgende tabel hebben.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |Computer waarop de gebeurtenis is verzameld. |
| Opslagruimte |Definieert het deel van het systeem dat het bericht wordt gegenereerd. |
| HostIP |IP-adres van het systeem het bericht te verzenden. |
| Hostnaam |Naam van het systeem het bericht te verzenden. |
| Foutcode |Ernst van de gebeurtenis. |
| SyslogMessage |Tekst van het bericht. |
| Proces-id |ID van het proces dat het bericht wordt gegenereerd. |
| eventTime |Datum en tijd waarop de gebeurtenis is gegenereerd. |

## <a name="log-queries-with-syslog-records"></a>Logboek-query's met Syslog-records
De volgende tabel bevat voorbeelden van logboek-query's die Syslog-records ophalen.

| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| Syslog |Alle Syslogs. |
| Syslog &#124; waar foutcode == "error" |Alle Syslog-records met de ernst van de fout. |
| Syslog &#124; overzicht van AggregatedValue = count() door Computer |Telling van Syslog-records op de computer. |
| Syslog &#124; overzicht van AggregatedValue count() door faciliteit = |Telling van Syslog-records door de faciliteit. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.
* Gebruik [aangepaste velden](log-analytics-custom-fields.md) parseren van gegevens van syslog-records in afzonderlijke velden.
* [Linux-agents configureren](log-analytics-linux-agents.md) andere typen gegevens verzamelen.
