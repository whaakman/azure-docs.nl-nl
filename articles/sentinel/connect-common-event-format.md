---
title: CEF-gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Informatie over het verbinden met Azure Sentinel CEF-gegevens.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cbf5003b-76cf-446f-adb6-6d816beca70f
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: rkarlin
ms.openlocfilehash: f9435c4b7649e9b97c209fb554f62228cde95034
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612394"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Verbinding maken met de externe oplossing met behulp van Common Event Format

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt Azure Sentinel verbinden met een externe oplossing waarmee u logboekbestanden opslaan in Syslog. Als uw apparaat kunt u Logboeken opslaan als Syslog Common Event Format (CEF), wordt de integratie met Azure Sentinel kunt u eenvoudig uitvoeren van analyses en query's over de gegevens.

> [!NOTE] 
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="how-it-works"></a>Hoe werkt het?

De verbinding tussen Azure Sentinel en uw apparaat CEF vindt plaats in drie stappen:

1. Op het apparaat moet u deze waarden zo instellen dat het apparaat verzendt de benodigde logboeken in de indeling van de nodig op de Azure Sentinel Syslog-agent. Zolang u ze ook in de Syslog-daemon op de agent Azure Sentinel wijzigen, kunt u deze parameters in uw apparaat.
    - Protocol = UDP
    - Poort 514 =
    - Faciliteit = Local-4
    - Indeling CEF =
2. De Syslog-agent de gegevens verzamelt en verzendt ze veilig naar Log Analytics, waar deze wordt geparseerd en verrijkt.
3. De agent slaat de gegevens in een Log Analytics-werkruimte, zodat deze kan worden opgevraagd indien nodig, met behulp van analytics, correlatie-regels en dashboards.

> [!NOTE]
> De agent Logboeken kunt verzamelen uit meerdere bronnen, maar moet worden geïnstalleerd op machine toegewezen proxy.

## <a name="step-1-connect-to-your-cef-appliance-via-dedicated-azure-vm"></a>Stap 1: Verbinding maken met uw apparaat CEF via speciale Azure-VM

U moet een agent op een specifieke Linux-machine implementeren (virtuele machine of on-premises) om de communicatie tussen het apparaat en de Azure-Sentinel te ondersteunen. U kunt de agent automatisch of handmatig implementeren. Automatische implementatie is gebaseerd op Resource Manager-sjablonen en kan alleen worden gebruikt als de specifieke Linux-machine is een nieuwe virtuele machine die u in Azure maken wilt.

 ![CEF in Azure](./media/connect-cef/cef-syslog-azure.png)

U kunt ook kunt u de agent handmatig op een bestaande VM in Azure, op een virtuele machine in een andere cloud of op een on-premises machine implementeren. 

 ![CEF on-premises](./media/connect-cef/cef-syslog-onprem.png)

### <a name="deploy-the-agent-in-azure"></a>De agent in Azure implementeren


1. Klik in de portal voor Azure Sentinel **gegevensconnectors** en selecteer het apparaattype. 

1. Onder **Linux Syslog-agentconfiguratie**:
   - Kies **automatische implementatie** als u maken van een nieuwe machine die vooraf is geïnstalleerd met de Azure-Sentinel-agent en bevat alle configuratie nodig wilt, zoals hierboven is beschreven. Selecteer **automatische implementatie** en klikt u op **automatische agentimplementatie**. Hiermee gaat u naar de pagina kopen voor een specifieke Linux-VM die automatisch is verbonden met uw werkruimte. De virtuele machine is een **standard D2s v3 (2 vcpu's, 8 GB geheugen)** en heeft een openbaar IP-adres.
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
            
            2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Waar {0} moet worden vervangen door de GUID van uw werkruimte.
            
            1. De syslog-daemon opnieuw starten `sudo service rsyslog restart`<br> Zie voor meer informatie de [rsyslog-documentatie](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html)
           
          - Als u syslog-ng het volgende hebt geselecteerd:

              1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Waar {0} moet worden vervangen door de GUID van uw werkruimte.

              3. De syslog-daemon opnieuw starten `sudo service syslog-ng restart` <br>Zie voor meer informatie de [syslog-ng-documentatie](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/mutual-authentication-using-tls/2)
      2. Start opnieuw op de Syslog-agent met de volgende opdracht: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Bevestig dat er geen fouten in het logboek van de agent zijn door het uitvoeren van deze opdracht: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

 Zoek voor het gebruik van de relevante schema in Log Analytics voor de CEF-gebeurtenissen, `CommonSecurityLog`.


### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>De agent op een op lokale Linux-server implementeren

Als u Azure niet gebruikt, moet u handmatig de agent Azure Sentinel om uit te voeren op een eigen Linux-server implementeren.


1. Klik in de portal voor Azure Sentinel **gegevensconnectors** en selecteer het apparaattype.
1. Maken van een specifieke Linux-VM, onder **Linux Syslog-agentconfiguratie** Kies **handmatige implementatie**.
   1. Onder **de Syslog-agent downloaden en installeren**, selecteer **niet-Azure Linux-machine**. 
   1. In de **Direct agent** scherm die wordt geopend, selecteert **-Agent voor Linux** om te downloaden van de agent of voer deze opdracht uit om het te downloaden op uw Linux-machine:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. In het scherm connector onder **en Syslog doorsturen configureren**, instellen of uw Syslog-daemon is **rsyslog.d** of **syslog-ng het volgende**. 
      1. Kopieer deze opdrachten en voer ze uit op uw apparaat:
         - Als u rsyslog hebt geselecteerd:
           1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Waar {0} moet worden vervangen door de GUID van uw werkruimte.
           3. De syslog-daemon opnieuw starten `sudo service rsyslog restart`
         - Als u syslog-ng het volgende hebt geselecteerd:
            1. Laat de Syslog-daemon op faciliteit local_4 luisteren en de Syslog-berichten te verzenden naar de Azure-Sentinel agent met behulp van poort 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Download en installeer de [security_events configuratiebestand](https://aka.ms/asi-syslog-config-file-linux) die configureert u de Syslog-agent om te luisteren op poort 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Waar {0} moet worden vervangen door de GUID van uw werkruimte.
            3. De syslog-daemon opnieuw starten `sudo service syslog-ng restart`
      1. Start opnieuw op de Syslog-agent met de volgende opdracht: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Bevestig dat er geen fouten in het logboek van de agent zijn door het uitvoeren van deze opdracht: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
  
 Zoek voor het gebruik van de relevante schema in Log Analytics voor de CEF-gebeurtenissen, `CommonSecurityLog`.

## <a name="step-2-forward-common-event-format-cef-logs-to-syslog-agent"></a>Stap 2: Common Event Format (CEF) Logboeken door te sturen naar de Syslog-agent

Stel uw beveiligingsoplossing voor het verzenden van Syslog-berichten in CEF-indeling naar uw Syslog-agent. Zorg ervoor dat u dezelfde parameters die worden weergegeven in de configuratie van de agent. Meestal zijn:

- Poort 514
- Faciliteit local4

## <a name="step-3-validate-connectivity"></a>Stap 3: Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 

1. Zorg ervoor dat u de juiste functie gebruikt. De functie moet hetzelfde zijn in uw apparaat en in Azure Sentinel. U kunt controleren welke faciliteit bestand dat u gebruikt in Azure Sentinel en wijzig dit in het bestand `security-config-omsagent.conf`. 

2. Zorg ervoor dat uw logboeken zijn ophalen met de juiste poort in de Syslog-agent. Voer deze opdracht uit op de computer van de Syslog-agent: `tcpdump -A -ni any  port 514 -vv` Met deze opdracht ziet u de logboeken die gegevensstromen van het apparaat naar de Syslog-machine. Zorg ervoor dat Logboeken zijn ontvangen van de bron-apparaat op de juiste poort en de juiste faciliteit.

3. Zorg ervoor dat de logboeken die u verzendt aan de voldoet [RFC 5424](https://tools.ietf.org/html/rfc542).

4. Op de computer waarop de Syslog-agent wordt uitgevoerd, zorg ervoor dat deze poorten 514, 25226 zijn open en luistert, met de opdracht `netstat -a -n:`. Zie voor meer informatie over het gebruik van deze opdracht [netstat(8) - pagina voor Linux-man](https://linux.die.net/man/8/netstat). Als het goed luistert, ziet u dit:

   ![Azure Sentinel poorten](./media/connect-cef/ports.png) 

5. Zorg ervoor dat de daemon is ingesteld om te luisteren op poort 514, waarop u de logboeken verzenden.
    - Voor rsyslog het volgende:<br>Zorg ervoor dat het bestand `/etc/rsyslog.conf` deze configuratie omvat:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Zie voor meer informatie, [imudp: UDP-Syslog-Module voor invoer](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) en [imtcp: TCP-invoer van de Syslog-Module](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Voor syslog-ng het volgende:<br>Zorg ervoor dat het bestand `/etc/syslog-ng/syslog-ng.conf` deze configuratie omvat:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Zie voor meer informatie [imudp: UDP-Syslog-invoer Module] (Zie voor meer informatie de [syslog-ng het volgende Open Source Edition 3,16 - Administration Guide](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Controleer of er communicatie tussen de Syslog-daemon en de agent. Voer deze opdracht uit op de computer van de Syslog-agent: `tcpdump -A -ni any  port 25226 -vv` Met deze opdracht ziet u de logboeken die gegevensstromen van het apparaat naar de Syslog-machine. Zorg ervoor dat de logboeken ook op de agent ontvangen worden.

6. Als beide van deze opdrachten geslaagde resultaten hebt opgegeven, controleert u Log Analytics om te zien als uw logboeken binnenkomen. Alle gebeurtenissen die worden gestreamd vanaf deze apparaten worden weergegeven in onbewerkte vorm in Log Analytics onder `CommonSecurityLog` type.

7. Controleer of er fouten zijn of als de logboeken worden niet binnenkomen, kijkt u in `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Als deze aangeeft er zijn fouten in het verschil indeling dat, gaat u naar `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` en bekijk het bestand `security_events.conf`en zorg ervoor dat uw logboeken overeenkomt met de regex-indeling die u in dit bestand ziet.

8. Zorg ervoor dat de standaardgrootte van uw Syslog-bericht beperkt tot 2048 bytes (2KB is). Als u Logboeken zijn te lang, werkt u de security_events.conf met de volgende opdracht: `message_length_limit 4096`


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u CEF-apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

