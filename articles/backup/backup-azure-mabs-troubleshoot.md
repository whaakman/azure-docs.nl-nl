---
title: Problemen met Azure Backup-Server | Microsoft Docs
description: Problemen met het oplossen van de installatie, de registratie van Azure Backup-Server, en back-up en herstel van toepassingsworkloads.
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: e9517672138a4ea7577af1295dea13771733983e
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Problemen met Azure Backup Server oplossen

Gebruik de informatie in de volgende tabellen om het oplossen van fouten die optreden tijdens het gebruik van Azure Backup-Server.

## <a name="invalid-vault-credentials-provided"></a>Ongeldige kluisreferenties opgegeven 

U lost dit probleem, volg [bovenstaande stappen voor probleemoplossing](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues).

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>De agentbewerking is mislukt vanwege een communicatiefout met de service DPM agent coordinator op de server 

U lost dit probleem, volg [bovenstaande stappen voor probleemoplossing](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues). 

## <a name="setup-could-not-update-registry-metadata"></a>Setup kan de metagegevens van het register niet bijwerken

U lost dit probleem, volg [bovenstaande stappen voor probleemoplossing](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#installation-issues).




## <a name="installation-issues"></a>Problemen met de installatie

| Bewerking | Foutdetails | Tijdelijke oplossing |
|-----------|---------------|------------|
|Installeren | Setup kan de metagegevens van het register niet bijwerken. Deze update kan leiden tot overusage opslag verbruikt. Om dit te voorkomen, werk de registervermelding ReFS bijsnijden. | De registersleutel aanpassen **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. De Dword-waarde ingesteld op 1. |
|Installeren | Setup kan de metagegevens van het register niet bijwerken. Deze update kan leiden tot overusage opslag verbruikt. Om dit te voorkomen, werk de registervermelding SnapOptimization Volume. | Maak de registersleutel **SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds** met de waarde van een lege tekenreeks. |
## <a name="registration-and-agent-related-issues"></a>Registratie en problemen met de agent
| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Registreren in een kluis | Ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld met de service voor herstel. | Deze fout oplossen: <ul><li> Download de meest recente referentiebestand uit de kluis en probeer het opnieuw. <br>(OR)</li> <li> Als de vorige actie niet werkt, probeert te downloaden van de referenties naar een andere lokale map of een nieuwe kluis maken. <br>(OR)</li> <li> Probeer het bijwerken van de datum- en tijdinstellingen zoals beschreven in [deze blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>(OR)</li> <li> Controleer of c:\windows\temp meer dan 65000 zijn bestanden heeft. Verouderde bestanden verplaatsen naar een andere locatie of de items in de map Temp verwijderen. <br>(OR)</li> <li> Controleer de status van certificaten. <br> a. Open **computercertificaten beheren** (in het Configuratiescherm). <br> b. Vouw de **persoonlijke** en de onderliggende knooppunten **certificaten**.<br> c.  Verwijder het certificaat **hulpprogramma's van Windows Azure**. <br> d. Probeer de registratie in de Azure Backup-client. <br> (OR) </li> <li> Controleer als Groepsbeleid geïmplementeerd is. </li></ul> |
| De agents worden gepusht op beveiligde servers | De agentbewerking is mislukt vanwege een communicatiefout met de service DPM Agent Coordinator op \<ServerName >. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, voert u de volgende stappen uit**: <ul><li> Als u een computer uit een niet-vertrouwd domein toevoegen wilt, voert u de [deze stappen](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx). <br> (OR) </li><li> Als u een computer van een vertrouwd domein toevoegen wilt, problemen oplossen met behulp van de stappen in [deze blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>(OR)</li><li> Schakelt u antivirus het oplossen van problemen. Als het probleem is opgelost, de antivirussoftware instellingen zoals voorgesteld in wijzigen [in dit artikel](https://technet.microsoft.com/library/hh757911.aspx).</li></ul> |
| De agents worden gepusht op beveiligde servers | De referenties die zijn opgegeven voor de server zijn ongeldig. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, voert u de volgende stappen uit**: <br> Probeer de beveiligingsagent handmatig installeren op de productieserver zoals opgegeven in [in dit artikel](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual).|
| Azure Backup-Agent is geen verbinding maken met de Azure Backup-service (ID: 100050) | De Azure Backup Agent is kan geen verbinding maken met de Azure Backup-service. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, voert u de volgende stappen uit**: <br>1. Voer de volgende opdracht vanaf een opdrachtprompt: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Hiermee opent u Internet Explorer-venster. <br/> 2. Ga naar **extra** > **Internetopties** > **verbindingen** > **LAN-instellingen**. <br/> 3. Controleer of de proxy-instellingen voor het account. Stel Proxy IP-adres en poort. <br/> 4. Sluit Internet Explorer.|
| Installatie van Azure Backup-Agent is mislukt | De Microsoft Azure Recovery Services-installatie is mislukt. Alle wijzigingen die door de Microsoft Azure Recovery Services-installatie zijn aangebracht in het systeem zijn teruggedraaid. (ID: 4024) | Azure-Agent handmatig installeren.


## <a name="configuring-protection-group"></a>Configureren van beveiligingsgroep
| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Beveiligingsgroepen configureren | DPM kan niet opsommen voor het toepassingsonderdeel op de beveiligde computer (beveiligde computernaam). | Selecteer **vernieuwen** op het scherm configureren bescherming groep gebruikersinterface op het niveau van de relevante datasource-component. |
| Beveiligingsgroepen configureren | Kan geen beveiliging configureren | Als de beveiligde server een SQL-server is, controleert u of dat de rol sysadmin zijn verleend aan het systeemaccount (NTAuthority\System) op de beveiligde computer zoals beschreven in [in dit artikel](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx).
| Beveiligingsgroepen configureren | Er is onvoldoende beschikbare ruimte in de opslaggroep voor deze beveiligingsgroep. | De schijven die zijn toegevoegd aan de opslaggroep [mag niet een partitie](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Verwijder alle bestaande volumes op de schijven. Voeg ze toe aan de opslaggroep.|
| Beleid wijzigen |Het back-upbeleid kan niet worden gewijzigd. Fout: De huidige bewerking is mislukt vanwege een interne servicefout [0x29834]. Probeer de bewerking na enige tijd is verstreken. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft ondersteuning. |**Oorzaak:**<br/>Deze fout treedt op onder drie voorwaarden: Wanneer beveiligingsinstellingen zijn ingeschakeld, wanneer u probeert te verkort de bewaartermijn onder de minimumwaarden eerder hebt opgegeven, en wanneer u zich op een niet-ondersteunde versie. (Niet-ondersteunde versies zijn die onder Microsoft Azure Backup-Server versie 2.0.9052 en de Azure Backup-Server update 1). <br/>**Aanbevolen actie:**<br/> Stel de bewaarperiode boven de minimale periode opgegeven bewaarperiode om door te gaan met het beleid gerelateerde updates. (De minimale bewaarperiode is zeven dagen voor dagelijks, vier weken voor de week, drie weken voor maandelijks of één jaar voor jaarlijks.) <br><br>Desgewenst voorkeur een andere aanpak is het bijwerken van de backup-agent en de Azure Backup-Server wilt gebruiken voor alle beveiligingsupdates. |

## <a name="backup"></a>Back-up
| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Back-up | De replica is inconsistent | Controleer of de optie Automatische consistentiecontroles controle in de Wizard beveiligingsgroep is ingeschakeld. Zie het Microsoft TechNet-artikel voor meer informatie over de oorzaken van replica inconsistentie en relevante suggesties [de Replica is inconsistent](https://technet.microsoft.com/library/cc161593.aspx).<br> <ol><li> In geval van een back-up van systeemstatus/BMR, Controleer of Windows Server back-up is geïnstalleerd op de beveiligde server.</li><li> Controleren op problemen met ruimte in de DPM-opslaggroep op de DPM/Microsoft Azure Backup-Server en opslag waar nodig toewijzen.</li><li> Controleer de status van de Volume Shadow Copy-Service op de beveiligde server. Als het is uitgeschakeld, stelt u deze handmatig te starten. Start de service op de server. Vervolgens gaat u terug naar de console DPM/Microsoft Azure Backup-Server en de synchronisatie met de consistentiecontrole gestart.</li></ol>|
| Back-up | Een onverwachte fout opgetreden bij het uitvoeren van de taak. Het apparaat is niet gereed. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, voert u de volgende stappen uit:** <br> <ul><li>De ruimte van de opslagruimte op schaduwkopie ingesteld op onbeperkt op de items in de beveiligingsgroep en voer de consistentiecontrole.<br></li> (OR) <li>Verwijder de bestaande beveiliging groep en het maken van meerdere nieuwe groepen. Elke nieuwe beveiligingsgroep moet bevatten een afzonderlijke item.</li></ul> |
| Back-up | Als u een back-up alleen systeemstatus, controleert u of er is onvoldoende vrije ruimte op de beveiligde computer voor het opslaan van de systeemstatusback-up. | <ol><li>Controleer of Windows Server back-up is geïnstalleerd op de beveiligde machine.</li><li>Controleer of er voldoende ruimte is op de beveiligde computer voor de systeemstatus. De eenvoudigste manier om te controleren of dit is de beveiligde computer naar Windows Server back-up openen, klikt u op via de selecties en selecteer vervolgens de BMR. De gebruikersinterface vervolgens vertelt u hoeveel ruimte is vereist. Open **WSB** > **lokale back-up** > **back-upschema** > **back-upconfiguratie selecteren**  >  **Volledige server** (de grootte wordt weergegeven). Deze grootte wordt gebruikt voor verificatie.</li></ol>
| Back-up | Online herstelpunt is mislukt | Als het foutbericht ' Windows Azure Backup-Agent kan niet worden gemaakt van een momentopname van het geselecteerde volume is ' vergroot de schijfruimte in de replica en herstelpuntvolume.
| Back-up | Online herstelpunt is mislukt | Als het foutbericht 'De Windows Azure Backup-Agent kan geen verbinding met de OBEngine-service' staat, moet u controleren of de OBEngine in de lijst bestaat met services op de computer uitgevoerd. Als de OBEngine-service niet wordt uitgevoerd, gebruikt u de opdracht 'net start OBEngine' de OBEngine-service te starten.
| Back-up | Online herstelpunt is mislukt | Als het foutbericht 'de wachtwoordzin voor versleuteling voor deze server is niet ingesteld. Configureer een wachtwoordzin voor versleuteling,' Probeer te configureren van een wachtwoordzin voor versleuteling. Als dit mislukt, moet u de volgende stappen uitvoeren: <br> <ol><li>Controleer of de nieuwe locatie bestaat. Dit is de locatie die wordt vermeld in het register **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config**, met de naam **ScratchLocation** moet aanwezig zijn.</li><li> Als de nieuwe locatie bestaat, probeert u opnieuw te registreren met behulp van de oude wachtwoordzin. *Wanneer u een wachtwoordzin voor versleuteling configureert, kunt u dit in een veilige locatie opslaan.*</li><ol>
| Back-up | Back-upfouten voor BMR | Als het BMR groot is, bepaalde toepassingsbestanden verplaatsen naar het station van het besturingssysteem en probeer het opnieuw. |
| Back-up | De optie voor een VM VMware op een nieuw Microsoft Azure Backup-Server opnieuw te beveiligen, wordt niet weergegeven als beschikbaar om toe te voegen. | VMware-eigenschappen zijn op een oude, buiten gebruik gestelde exemplaar van Microsoft Azure Backup-Server gericht. Dit probleem oplossen:<br><ol><li>In VCenter (SC-VMM-equivalent), gaat u naar de **samenvatting** tabblad en klik vervolgens op **aangepaste kenmerken**.</li>  <li>Verwijder de oude naam van de Microsoft Azure Backup-Server van de **DPMServer** waarde.</li>  <li>Ga terug naar de nieuwe Microsoft Azure Backup-Server en de pagina wijzigen  Nadat u hebt geselecteerd de **vernieuwen** knop, de virtuele machine wordt weergegeven met een selectievakje in als beschikbaar om toe te voegen aan de beveiliging.</li></ol> |
| Back-up | Fout bij het openen van gedeelde bestanden/mappen | Probeer de antivirussoftware te wijzigen zoals voorgesteld in het TechNet-artikel [antivirussoftware uitvoeren op de DPM-server](https://technet.microsoft.com/library/hh757911.aspx).|
| Back-up | Taken voor het maken van online herstelpunten voor VMware VM is mislukt. Er is een fout opgetreden in VMware in DPM opgetreden tijdens ChangeTracking informatie ophalen. ErrorCode - FileFaultFault (ID 33621) |  <ol><li> De CTK in VMware voor de betrokken virtuele machines opnieuw instellen.</li> <li>Controleer dat onafhankelijke schijf is niet aanwezig in VMware.</li> <li>Beveiliging voor de betrokken virtuele machines stoppen en opnieuw te beveiligen met de **vernieuwen** knop. </li><li>Voer een CC voor de betrokken virtuele machines.</li></ol>|


## <a name="change-passphrase"></a>De wachtwoordzin wijzigen
| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| De wachtwoordzin wijzigen |De beveiliging PINCODE die is ingevoerd, is onjuist. Geef de juiste beveiliging PINCODE om deze bewerking te voltooien. |**Oorzaak:**<br/> Deze fout treedt op wanneer u een ongeldige of verlopen van PINCODE beveiliging tijdens het uitvoeren van een kritieke bewerking (zoals het wijzigen van een wachtwoordzin). <br/>**Aanbevolen actie:**<br/> U moet een geldige beveiligings PINCODE invoeren voor het voltooien van de bewerking. Als u de PINCODE, meld u aan bij de Azure portal en gaat u naar de Recovery Services-kluis. Ga vervolgens naar **instellingen** > **eigenschappen** > **BEVEILIGINGSCODE genereren**. Met deze PINCODE kunt u de wachtwoordzin. |
| De wachtwoordzin wijzigen |Bewerking is mislukt. ID: 120002 |**Oorzaak:**<br/>Deze fout treedt op wanneer de beveiligingsinstellingen zijn ingeschakeld, of wanneer u probeert te wijzigen van de wachtwoordzin voor wanneer u een niet-ondersteunde versie.<br/>**Aanbevolen actie:**<br/> Als u wilt wijzigen van de wachtwoordzin, moet u eerst de backup-agent bijwerken naar de minimumversie 2.0.9052 is. U moet ook Azure Backup-Server bijwerken naar het minimum van update 1 en voer een geldige PINCODE-beveiliging. Als u de PINCODE, meld u aan bij de Azure-portal en gaat u naar de Recovery Services-kluis. Ga vervolgens naar **instellingen** > **eigenschappen** > **BEVEILIGINGSCODE genereren**. Met deze PINCODE kunt u de wachtwoordzin. |


## <a name="configure-email-notifications"></a>E-mailmeldingen configureren
| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| E-mailmeldingen met een Office 365-account instellen |Fout-ID: 2013| **Oorzaak:**<br> Het gebruik van Office 365-account <br>**Aanbevolen actie:**<ol><li> Het eerste wat om ervoor te zorgen is 'Toestaan anonieme Relay op een Ontvangconnector' voor uw DPM-server is ingesteld op Exchange. Zie voor meer informatie over het configureren van dit [toestaan anonieme Relay op een verbindingslijn ontvangen](http://technet.microsoft.com/en-us/library/bb232021.aspx) op TechNet.</li> <li> Als u kan niet een interne SMTP-relay gebruiken en instellen moet met behulp van uw Office 365-server, kunt u IIS instellen om te worden van een relay. De DPM-server configureren [doorsturen van de SMTP O365 met behulp van IIS](https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx).<br><br> **Belangrijk:** Zorg ervoor dat u de user@domain.com indeling en *niet* domein\gebruiker.<br><br><li>DPM-punt aan de lokale server-naam gebruiken als de SMTP-server-poort 587. Wijs deze het e-mailadres voor een gebruiker die de e-mailberichten moeten voortkomen uit.<li> De gebruikersnaam en wachtwoord op de pagina van de installatie van DPM SMTP-, moet voor een domeinaccount in het domein dat DPM ingeschakeld is. </li><br> **Opmerking**: wanneer u het adres van de SMTP-server wijzigen wilt, de wijziging aanbrengt in de nieuwe instellingen, instellingen te sluiten en weer openen om er zeker van te zijn toont de nieuwe waarde.  Eenvoudig wijzigen en testen mogelijk niet altijd tot de nieuwe instellingen van kracht, zodat het testen van het op deze manier wordt aanbevolen.<br><br>Op elk gewenst moment tijdens dit proces kunt u deze instellingen wissen door de DPM-console sluiten en het bewerken van de volgende registersleutels: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword verwijderen en SMTPUserName sleutels**. U kunt deze toevoegen terug naar de gebruikersinterface wanneer u deze opnieuw te starten.
