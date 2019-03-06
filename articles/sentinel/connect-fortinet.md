---
title: Verzamelen van gegevens in Azure Sentinel Preview Fortinet | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens in Azure Sentinel Fortinet.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 8992847ed6d6c25f5384da9b74b8ddaab063ed45
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57455408"
---
# <a name="connect-your-fortinet-appliance"></a>Verbinding maken met uw apparaat Fortinet 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt Azure Sentinel verbinden met een apparaat Fortinet door op te slaan van de logboekbestanden als Syslog CEF. De integratie met Azure Sentinel kunt u gemakkelijk uitvoeren op analytics en query's voor gegevens van het logboekbestand van Fortinet. Zie voor meer informatie over hoe Azure Sentinel gegevens op CEF neemt. [CEF verbinding maken met apparaten](connect-common-event-format.md).

> [!NOTE]
> - Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="step-1-connect-your-fortinet-appliance-using-an-agent"></a>Stap 1: Verbinding maken met uw Fortinet-apparaat met behulp van een agent

Als u wilt verbinden met uw apparaat Fortinet Sentinel van Azure, moet u een agent op een specifieke virtuele machine implementeren (virtuele machine of on-premises) om de communicatie tussen het apparaat en de Azure-Sentinel te ondersteunen. U kunt de agent deploly automatisch of handmatig. Automatische implementatie is alleen beschikbaar als uw toegewezen machine is een nieuwe virtuele machine die u in Azure maken wilt. 

U kunt ook kunt u de agent handmatig op een bestaande VM in Azure, op een virtuele machine in een andere cloud of op een on-premises machine implementeren.

Zie voor een diagram van een van beide opties [verbinding maken met gegevensbronnen](connect-data-sources.md#agent-options).

### <a name="deploy-the-agent-in-azure"></a>De agent in Azure implementeren

1. Klik in de portal voor Azure Sentinel **gegevensverzameling** en selecteer het apparaattype. 

1. Onder **Linux Syslog-agentconfiguratie**:
   - Kies **automatische implementatie** als u maken van een nieuwe machine die vooraf is geïnstalleerd met de Azure-Sentinel-agent en bevat alle configuratie nodig wilt, zoals hierboven is beschreven. Selecteer **automatische implementatie** en klikt u op **automatische agentimplementatie**. Hiermee gaat u naar de pagina kopen voor een specifieke virtuele machine die automatisch is verbonden met uw werkruimte, is. De virtuele machine is een **standard D2s v3 (2 vcpu's, 8 GB geheugen)** en heeft een openbaar IP-adres.
      1. In de **aangepaste implementatie** pagina, geef de details en kiest u een gebruikersnaam en wachtwoord en als u akkoord met de voorwaarden en bepalingen gaat, koopt u de virtuele machine.
      1. Configureer uw apparaat voor het verzenden van logboeken met behulp van de instellingen die worden vermeld in de pagina. Gebruik deze instellingen voor de algemene Common Event Format-connector:
         - Protocol = UDP
         - Poort 514 =
         - Faciliteit = Local-4
         - Indeling CEF =
   - Kies **handmatige implementatie** als u wilt gebruiken van een bestaande virtuele machine als de specifieke Linux-machine waarop de agent Azure Sentinel moet worden geïnstalleerd. 
      1. Onder **de Syslog-agent downloaden en installeren**, selecteer **virtuele Azure Linux-machine**. 
      1. In de **virtuele machines** scherm dat wordt geopend, selecteert u de computer die u wilt gebruiken en klikt u op **Connect**.
      1. In het scherm connector onder **en Syslog doorsturen configureren**, instellen of uw Syslog-daemon is **rsyslog.d** of **syslog-ng het volgende**. 
      1. Kopieer deze opdrachten en voer ze uit op uw apparaat:
          - Als u rsyslog.d hebt geselecteerd:
              
            1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`
            
            1. De syslog-daemon opnieuw starten `sudo service rsyslog restart`
             
          - Als u syslog-ng het volgende hebt geselecteerd:

              1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`

              3. De syslog-daemon opnieuw starten `sudo service syslog-ng restart`
      2. Start opnieuw op de Syslog-agent met de volgende opdracht: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Bevestig dat er geen fouten in het logboek van de agent zijn door het uitvoeren van deze opdracht: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>De agent op een op lokale Linux-server implementeren

Als u Azure niet gebruikt, moet u handmatig de agent Azure Sentinel om uit te voeren op een eigen Linux-server implementeren.


1. Klik in de portal voor Azure Sentinel **gegevensverzameling** en selecteer het apparaattype.
1. Maken van een specifieke Linux-VM, onder **Linux Syslog-agentconfiguratie** Kies **handmatige implementatie**.
   1. Onder **de Syslog-agent downloaden en installeren**, selecteer **niet-Azure Linux-machine**. 
   1. In de **Direct agent** scherm die wordt geopend, selecteert **-Agent voor Linux** om te downloaden van de agent of voer deze opdracht uit om het te downloaden op uw Linux-machine:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
    3. In het scherm connector onder **en Syslog doorsturen configureren**, instellen of uw Syslog-daemon is **rsyslog.d** of **syslog-ng het volgende**. 
    4. Kopieer deze opdrachten en voer ze uit op uw apparaat:
       - Als u rsyslog hebt geselecteerd:
          1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
          2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`
          3. De syslog-daemon opnieuw starten `sudo service rsyslog restart`
       - Als u syslog-ng het volgende hebt geselecteerd:
            1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`
            3. De syslog-daemon opnieuw starten `sudo service syslog-ng restart`
    5. Start opnieuw op de Syslog-agent met de volgende opdracht: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
    6. Bevestig dat er geen fouten in het logboek van de agent zijn door het uitvoeren van deze opdracht: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-fortinet-logs-to-the-syslog-agent"></a>Stap 2: De Fortinet logboeken doorsturen naar de Syslog-agent

De Fortinet voor het doorsturen van Syslog-berichten in CEF-indeling naar uw Azure-werkruimte via de Syslog-agent configureren:

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
    - Stel de **facility_name** gebruik van de faciliteit die u hebt geconfigureerd in de agent. Standaard stelt de agent dit op local4.
    - Stel de **syslog-poort** naar **514**, of de poort die is ingesteld op de agent.
    - Om in te schakelen CEF-indeling in eerdere versies van FortiOS, moet u mogelijk de opdrachtenset uitvoeren **csv uitschakelen**.
 
   > [!NOTE] 
   > Voor meer informatie gaat u naar de [Fortinet documentbibliotheek](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary). Kies de versie en gebruik de **handboek voor** en **bericht-logboek: naslag**.

## <a name="step-3-validate-connectivity"></a>Stap 3: Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 

1. Zorg ervoor dat uw logboeken zijn ophalen met de juiste poort in de Syslog-agent. De computer van de Syslog-agent van deze opdracht uitvoeren: `tcpdump -A -ni any  port 514 -vv` Met deze opdracht ziet u de logboeken die gegevensstromen van het apparaat naar de Syslog-machine. Zorg ervoor dat Logboeken zijn ontvangen van de bron-apparaat op de juiste poort en de juiste faciliteit.

2. Controleer of er communicatie tussen de Syslog-daemon en de agent. De computer van de Syslog-agent van deze opdracht uitvoeren: `tcpdump -A -ni any  port 25226 -vv` Met deze opdracht ziet u de logboeken die gegevensstromen van het apparaat naar de Syslog-machine. Zorg ervoor dat de logboeken ook op de agent ontvangen worden.

1. Als uw Fortinet-logboeken worden niet door de agent wordt ontvangen, moet u deze opdracht, afhankelijk van welk type u gebruikt, moet de functie ingesteld en de logboeken om te zoeken naar het woord Fortinet in de logboeken van Syslog-daemon uitvoeren:
   - rsyslog.d: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`

     De Syslog-daemon opnieuw: `sudo service rsyslog restart`
   - syslog-ng: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      
     De Syslog-daemon opnieuw: `sudo service syslog-ng restart`
1. Als beide van deze opdrachten geslaagde resultaten hebt opgegeven, controleert u Log Analytics om te zien als uw logboeken binnenkomen. Alle gebeurtenissen die worden gestreamd vanaf deze apparaten worden weergegeven in onbewerkte vorm in Log Analytics onder `CommonSecurityLog ` type.
1. Als u wilt controleren als er fouten zijn of als de logboeken worden niet binnenkomen, zoeken in `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
1. Zorg ervoor dat de standaardgrootte van uw Syslog-bericht beperkt tot 2048 bytes (2KB is). Als u Logboeken zijn te lang, werkt u de security_events.conf met de volgende opdracht: `message_length_limit 4096`


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u Fortinet apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

