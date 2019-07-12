---
title: De Fortinet gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens Fortinet Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: f3ab4861e874074e7de059c7c50064d53749748c
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611301"
---
# <a name="connect-your-fortinet-appliance"></a>Verbinding maken met uw apparaat Fortinet

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement. Wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie voor meer informatie, [aanvullende gebruiksvoorwaarden voor Microsoft Azure previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

U kunt Azure Sentinel verbinden met een apparaat Fortinet door op te slaan van de logboekbestanden als Syslog Common Event Format (CEF). Dankzij de integratie met Azure Sentinel, kunt u eenvoudig uitvoeren analyses en query's op gegevens van het logboekbestand van Fortinet. Zie voor meer informatie over hoe Azure Sentinel gegevens op CEF neemt. [CEF verbinding maken met apparaten](connect-common-event-format.md).

> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waar u uitgevoerd Sentinel van Azure.

## <a name="step-1-connect-your-fortinet-appliance-by-using-an-agent"></a>Stap 1: Verbinding maken met uw apparaat Fortinet met behulp van een agent

Als u wilt verbinden met uw apparaat Fortinet Azure Sentinel, een agent op een specifieke virtuele machine of on-premises computer ter ondersteuning van de communicatie tussen het apparaat en Azure Sentinel te implementeren. U kunt de agent automatisch of handmatig implementeren. Automatische implementatie is alleen beschikbaar als uw toegewezen machine is een nieuwe virtuele machine die u in Azure maakt.

U kunt ook de agent handmatig op een bestaande VM in Azure, op een virtuele machine in een andere cloud of op een on-premises machine implementeren.

Zie voor een diagram van een van beide opties [verbinding maken met gegevensbronnen](connect-data-sources.md#agent-options).

### <a name="deploy-the-agent-in-azure"></a>De agent in Azure implementeren

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en selecteer het apparaattype.

1. Onder **Linux Syslog-agentconfiguratie**:
   - Kies **automatische implementatie** als u maken van een nieuwe machine die vooraf geïnstalleerd met de Azure-Sentinel-agent en bevat alle de configuratie die nodig zijn wilt, zoals eerder is beschreven. Selecteer **automatische implementatie** > **automatische agentimplementatie**. De pagina kopen voor een specifieke virtuele machine die automatisch verbonden met uw werkruimte wordt weergegeven. De virtuele machine is een **standard D2s v3 (2 vcpu's, 8 GB geheugen)** en heeft een openbaar IP-adres.
      1. Op de **aangepaste implementatie** pagina, geef de details, voer een gebruikersnaam en wachtwoord en als u akkoord met de voorwaarden en bepalingen gaat, koopt u de virtuele machine.
      1. Configureer uw apparaat voor het verzenden van logboeken met behulp van de instellingen die worden vermeld op de pagina. Gebruik deze instellingen voor de algemene Common Event Format-connector:
         - Protocol = UDP
         - Poort 514 =
         - Faciliteit = Local-4
         - Indeling CEF =
   - Kies **handmatige implementatie** als u wilt gebruiken van een bestaande virtuele machine als de specifieke Linux-machine waarop de Azure-Sentinel-agent is geïnstalleerd. 
      1. Onder **de Syslog-agent downloaden en installeren**, selecteer **virtuele Azure Linux-machine**. 
      1. Op de **virtuele machines** scherm, selecteert u de computer die u wilt gebruiken en selecteer **Connect**.
      1. Op het scherm connector onder **en Syslog doorsturen configureren**, instellen of uw Syslog-daemon is **rsyslog.d** of **syslog-ng het volgende**. 
      1. Kopieer deze opdrachten en voer ze uit op uw apparaat:
          - Als u rsyslog.d hebt geselecteerd:
              
            1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten verzenden naar de agent Sentinel van Azure met behulp van poort 25226. Gebruik deze opdracht: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            1. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. Met deze opdracht gebruiken: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` waar {0} moet worden vervangen door de GUID van uw werkruimte.
            1. De syslog-daemon opnieuw starten met de volgende opdracht: `sudo service rsyslog restart`
             
          - Als u syslog-ng het volgende hebt geselecteerd:

              1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten verzenden naar de agent Sentinel van Azure met behulp van poort 25226. Gebruik deze opdracht: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              1. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. Met deze opdracht gebruiken: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` waar {0} moet worden vervangen door de GUID van uw werkruimte.
              1. De syslog-daemon opnieuw starten met de volgende opdracht: `sudo service syslog-ng restart`
      1. De Syslog-agent opnieuw starten door met de volgende opdracht: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Bevestig dat er geen fouten in het logboek van de agent zijn door het uitvoeren van deze opdracht: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>De agent op een on-premises Linux-server implementeren

Als u Azure niet gebruikt, moet u handmatig de agent Azure Sentinel om uit te voeren op een eigen Linux-server implementeren.

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en selecteer het apparaattype.
1. Maken van een specifieke Linux-VM, onder **Linux Syslog-agentconfiguratie** Selecteer **handmatige implementatie**.

    1. Onder **de Syslog-agent downloaden en installeren**, selecteer **niet-Azure Linux-machine**.
    1. In de **Direct agent** scherm die wordt geopend, selecteert **-Agent voor Linux** om te downloaden van de agent of voer deze opdracht uit om het te downloaden op uw Linux-machine: `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`

       1. In het scherm connector onder **en Syslog doorsturen configureren**, instellen of uw Syslog-daemon is **rsyslog.d** of **syslog-ng het volgende**.
       1. Kopieer deze opdrachten en voer ze uit op uw apparaat:

          - Als u rsyslog hebt geselecteerd:

            1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. Gebruik deze opdracht: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            1. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. Met deze opdracht gebruiken: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` waar {0} moet worden vervangen door de GUID van uw werkruimte.
            1. De syslog-daemon opnieuw starten met de volgende opdracht: `sudo service rsyslog restart`

          - Als u syslog-ng het volgende hebt geselecteerd:

            1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten verzenden naar de agent Sentinel van Azure met behulp van poort 25226. Gebruik deze opdracht: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            1. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. Met deze opdracht gebruiken: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` waar {0} moet worden vervangen door de GUID van uw werkruimte.
            1. De syslog-daemon opnieuw starten met de volgende opdracht: `sudo service syslog-ng restart`

      1. De Syslog-agent opnieuw starten door met de volgende opdracht: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Bevestig dat er geen fouten in het logboek van de agent zijn door het uitvoeren van deze opdracht: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-fortinet-logs-to-the-syslog-agent"></a>Stap 2: De Fortinet logboeken doorsturen naar de Syslog-agent

Configureer Fortinet voor het doorsturen van Syslog-berichten in CEF-indeling naar uw Azure-werkruimte via de Syslog-agent.

1. Open de CLI op uw apparaat Fortinet en voer de volgende opdrachten uit:

        config log syslogd setting
        set format cef
        set facility <facility_name>
        set port 514
        set reliable disable
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Vervang de server **ip-adres** met het IP-adres van de agent.
    - Stel **facility_name** gebruik van de faciliteit die u hebt geconfigureerd in de agent. Standaard stelt de agent dit op local4.
    - Stel de **syslog-poort** naar **514** of de poort die is ingesteld op de agent.
    - Om in te schakelen CEF-indeling in eerdere versies van FortiOS, moet u mogelijk de opdrachtenset uitvoeren **csv uitschakelen**.
 
   > [!NOTE] 
   > Voor meer informatie gaat u naar de [Fortinet documentbibliotheek](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary). Selecteer de versie en gebruik de **handboek voor** en **bericht-logboek: naslag**.

 Zoek voor het gebruik van de relevante schema in Azure Monitor Log Analytics voor de gebeurtenissen Fortinet, `CommonSecurityLog`.


## <a name="step-3-validate-connectivity"></a>Stap 3: Verbinding valideren

Het duurt maximaal 20 minuten totdat de logboeken in Log Analytics wordt weergegeven. 

1. Zorg ervoor dat u de juiste functie gebruikt. De functie moet hetzelfde zijn in uw apparaat en in Azure Sentinel. U kunt controleren welke faciliteit bestand dat u gebruikt in Azure Sentinel en wijzig dit in het bestand `security-config-omsagent.conf`. 

2. Zorg ervoor dat uw logboeken zijn ophalen met de juiste poort in de Syslog-agent. Voer deze opdracht uit op de computer van de Syslog-agent: `tcpdump -A -ni any  port 514 -vv`. Met deze opdracht ziet u de logboeken die van het apparaat naar de Syslog-machine streamen. Zorg ervoor dat Logboeken zijn ontvangen van de bron-apparaat op de juiste poort en de juiste installatie.

3. Zorg ervoor dat de logboeken die u verzendt aan de voldoet [RFC 5424](https://tools.ietf.org/html/rfc542).

4. Op de computer waarop de Syslog-agent wordt uitgevoerd. Zorg ervoor dat de poorten 514 en 25226 zijn open en luistert met behulp van de opdracht `netstat -a -n:`. Zie voor meer informatie over het gebruik van deze opdracht [netstat(8) - pagina voor Linux-man](https://linux.die.net/man/8/netstat). Als het goed luistert, moet u het volgende zien:

   ![Azure Sentinel poorten](./media/connect-cef/ports.png) 

5. Zorg ervoor dat de daemon is ingesteld om te luisteren op poort 514, waarop u de logboeken verzenden.
    - Voor rsyslog het volgende:<br>Zorg ervoor dat het bestand `/etc/rsyslog.conf` deze configuratie omvat:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Zie voor meer informatie, [imudp: Invoer UDP Syslog-module](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) en [imtcp: Invoer TCP, Syslog-module](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module).

   - Voor syslog-ng het volgende:<br>Zorg ervoor dat het bestand `/etc/syslog-ng/syslog-ng.conf` deze configuratie omvat:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Zie voor meer informatie, [imudp: Invoer UDP Syslog-module](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imudp.html) en [syslog-ng het volgende Open-source edition 3,16 - Administration guide](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Controleer of er communicatie tussen de Syslog-daemon en de agent. Voer deze opdracht uit op de computer van de Syslog-agent: `tcpdump -A -ni any  port 25226 -vv`. Met deze opdracht ziet u de logboeken die van het apparaat naar de Syslog-machine streamen. Zorg ervoor dat de logboeken ook op de agent ontvangen worden.

6. Als beide van deze opdrachten geslaagde resultaten hebt opgegeven, controleert u Log Analytics om te zien als uw logboeken binnenkomen. Alle gebeurtenissen die worden gestreamd vanaf deze apparaten worden weergegeven in onbewerkte vorm in Log Analytics onder `CommonSecurityLog` type.

7. Controleer of er fouten zijn of als de logboeken worden niet binnenkomen, kijkt u in `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Als deze aangeeft er zijn fouten in het verschil indeling dat, gaat u naar `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` en bekijk het bestand `security_events.conf`. Zorg ervoor dat uw logboeken overeenkomt met de regex-indeling die u in dit bestand ziet.

8. Zorg ervoor dat de standaardgrootte van uw Syslog-bericht beperkt tot 2048 bytes (2 KB is). Als de logboeken zijn te lang, werkt u security_events.conf met behulp van deze opdracht: `message_length_limit 4096`

10. Als uw Fortinet-logboeken worden niet door de agent wordt ontvangen, moet u deze opdracht, afhankelijk van welk type u gebruikt, moet de functie ingesteld en de logboeken om te zoeken naar het woord Fortinet in de logboeken van Syslog-daemon uitvoeren:
       - rsyslog.d: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`

     De Syslog-daemon opnieuw starten met de volgende opdracht: `sudo service rsyslog restart`
       - syslog-ng: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      
     De Syslog-daemon opnieuw starten met de volgende opdracht: `sudo service syslog-ng restart`

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u Fortinet apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

