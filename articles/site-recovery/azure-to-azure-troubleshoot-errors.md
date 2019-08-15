---
title: Azure Site Recovery voor probleemoplossing voor problemen met replicatie van Azure naar Azure en fouten | Microsoft Docs
description: Het oplossen van fouten en problemen bij het repliceren van virtuele machines voor herstel na noodgevallen van Azure
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: asgang
ms.openlocfilehash: 4d8ba44cdd5161a1a5ff108837cb57af4cd98835
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69034791"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Problemen met Azure-naar-Azure-VM-replicatie oplossen

Dit artikel beschrijft de voorkomende problemen in Azure Site Recovery bij het repliceren en herstellen van virtuele machines van Azure van de ene regio naar een andere regio en wordt uitgelegd hoe u oplossingen voor deze problemen. Zie voor meer informatie over ondersteunde configuraties, de [ondersteuningsmatrix voor het repliceren van virtuele Azure-machines](site-recovery-support-matrix-azure-to-azure.md).

## <a name="list-of-errors"></a>Lijst met fouten
- **[Problemen met Azure-resource quota (fout code 150097)](#azure-resource-quota-issues-error-code-150097)**
- **[Vertrouwde basis certificaten (fout code 151066)](#trusted-root-certificates-error-code-151066)**
- **[Uitgaande connectiviteit voor Site Recovery (fout code 151195)](#issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br)**

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemen met Azure-resource quota (fout code 150097)
Uw abonnement moet worden ingeschakeld om te maken van virtuele Azure-machines in de doelregio die u van plan bent om te gebruiken als uw Dr-regio. Uw abonnement moet ook voldoende quotum ingeschakeld als u virtuele machines van specifieke grootte hebben. Standaard wordt met Site Recovery even groot is voor de doel-VM kiest als de bron-VM. Als de overeenkomende grootte niet beschikbaar is, wordt automatisch de dichtstbijzijnde grootte opgehaald. Als er geen overeenkomende grootte die ondersteuning biedt voor bron-VM-configuratie is, wordt dit foutbericht weergegeven:

**Foutcode** | **Mogelijke oorzaken** | **Aanbeveling**
--- | --- | ---
150097<br></br>**Bericht**: Kan replicatie niet inschakelen voor de VmName van de virtuele machine. | -Uw abonnements-ID kan niet worden ingeschakeld om te maken van virtuele machines op de doellocatie voor de regio.</br></br>-Uw abonnements-ID kan niet worden ingeschakeld of beschikt niet over voldoende quotum voor het maken van specifieke VM-grootten op de doellocatie voor de regio.</br></br>-Een geschikt doel-VM-grootte die overeenkomt met de bron-VM NIC aantal (2) is niet gevonden voor de abonnements-ID op de doellocatie voor de regio.| Neem contact op met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om te kunnen maken van de virtuele machine voor de vereiste VM-grootten op de doellocatie voor uw abonnement. Nadat deze ingeschakeld, moet u de mislukte bewerking opnieuw proberen.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
U kunt contact opnemen met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om in te schakelen van uw abonnement te maken van virtuele machines van de vereiste grootte in de doellocatie.

Als de doellocatie een beperking van capaciteit heeft, replicatie uitschakelen en inschakelen naar een andere locatie waar uw abonnement voldoende quotum voor het maken van virtuele machines van de vereiste grootte heeft.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrouwde basis certificaten (fout code 151066)

Als de meest recente vertrouwde basiscertificaten niet aanwezig op de virtuele machine, mislukken uw taak 'replicatie inschakelen'. Zonder de certificaten mislukt de verificatie en autorisatie van Site Recovery-serviceaanroepen van de virtuele machine. Het foutbericht voor de taak is mislukt 'replicatie inschakelen' Site Recovery wordt weergegeven:

**Foutcode** | **Mogelijke oorzaak** | **Aanbevelingen**
--- | --- | ---
151066<br></br>**Bericht**: Site Recovery configuratie is mislukt. | De vereiste vertrouwde basiscertificaten gebruikt voor autorisatie en verificatie zijn niet aanwezig op de machine. | -Voor een virtuele machine met het Windows-besturingssysteem, zorg ervoor dat de vertrouwde basiscertificaten aanwezig zijn op de machine. Zie voor meer informatie, [configureren van vertrouwde basiscertificaten en niet-toegestane certificaten](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Voor een virtuele machine met het Linux-besturingssysteem, volg de richtlijnen voor vertrouwde basiscertificaten gepubliceerd door de distributeur versie van Linux-besturingssysteem.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
**Windows**

De meest recente Windows updates installeren op de virtuele machine zo in dat de vertrouwde basiscertificaten aanwezig zijn op de machine. Als u zich in een omgeving zonder verbinding, volgt u de standaard Windows update-proces in uw organisatie om op te halen van de certificaten. Als de vereiste certificaten niet aanwezig op de virtuele machine, mislukt de aanroepen naar de Site Recovery-service voor opmaaktalen wordt om beveiligingsredenen.

Ga als volgt het typische Windows update management of het certificaat updatebeheerproces in uw organisatie om op te halen van de meest recente basiscertificaten en de bijgewerkte lijst met ingetrokken certificaten op de virtuele machines.

Om te controleren of het probleem opgelost is, gaat u naar login.microsoftonline.com vanuit een browser op uw virtuele machine.

**Linux**

Volg de richtlijnen bij uw Linux-leverancier om op te halen van de meest recente vertrouwde basiscertificaten en de meest recente certificaatintrekkingslijst op de virtuele machine.

Omdat symlinks SuSE Linux gebruikt voor het onderhouden van een lijst van certificaten, als volgt te werk:

1.  Meld u aan als een hoofdgebruiker.

2.  Voer deze opdracht uit de map wilt wijzigen.

      ``# cd /etc/ssl/certs``

1. Controleer of de basis-CA-certificaat van Symantec aanwezig is.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

2. Als de basis-CA-certificaat van Symantec niet wordt gevonden, voer de volgende opdracht om het bestand te downloaden. Controleer of er fouten en voer de aanbevolen actie voor netwerkfouten.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

3. Controleer of het CA-certificaat van het Baltimore-basiscertificaat aanwezig is.

      ``# ls Baltimore_CyberTrust_Root.pem``

4. Als het Baltimore basis-CA-certificaat niet wordt gevonden, downloadt u het certificaat.  

    ``# wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

5. Controleer of het certificaat DigiCert_Global_Root_CA aanwezig is.

    ``# ls DigiCert_Global_Root_CA.pem``

6. Als de DigiCert_Global_Root_CA niet wordt gevonden, voer de volgende opdrachten om het certificaat te downloaden.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

7. Voer rehash script voor het bijwerken van het certificaat onderwerp-hashes voor de zojuist gedownloade certificaten.

    ``# c_rehash``

8.  Controleer als onderwerp van de hashes als symlinks voor de certificaten die zijn gemaakt.

    - Opdracht

      ``# ls -l | grep Baltimore``

    - Uitvoer

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Opdracht

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Uitvoer

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Opdracht

      ``# ls -l | grep DigiCert_Global_Root``

    - Uitvoer

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

9.  Maak een kopie van het bestand VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem met filename b204d74a.0

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

10. Maak een kopie van het bestand Baltimore_CyberTrust_Root.pem met filename 653b494a.0

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Maak een kopie van het bestand DigiCert_Global_Root_CA.pem met filename 3513523f.0

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Controleer of de bestanden aanwezig zijn.  

    - Opdracht

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Uitvoer

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Uitgaande connectiviteit voor Site Recovery-URL's of IP-adresbereiken (foutcode 151037 of 151072)

Voor Site Recovery-replicatie met werk, uitgaande connectiviteit voor bepaalde URL's of IP-bereiken zijn van de virtuele machine. Als uw VM zich achter een firewall bevindt of regels voor network security group (NSG) gebruikt voor het beheren van uitgaande connectiviteit, kunt u een van deze problemen kan tegenkomen.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Probleem 1: Kan de virtuele machine van Azure niet registreren met Site Recovery (151195) </br>
- **Mogelijke oorzaak** </br>
  - Er kan geen verbinding tot stand worden gebracht met Site Recovery-eind punten vanwege een DNS-omzettings fout.
  - Dit wordt vaker gezien tijdens opnieuw beveiliging wanneer u de virtuele machine failover, maar de DNS-server niet bereikbaar vanaf de DR-regio is.

- **Resolutie**
   - Als u aangepaste DNS gebruikt, zorg ervoor dat is de DNS-server toegankelijk vanuit de regio voor herstel na noodgevallen. Om te controleren of u een aangepaste DNS-server gaat u naar de virtuele machine > herstel na noodgevallen netwerk > DNS-servers. Probeer toegang tot de DNS-server van de virtuele machine. Als deze niet toegankelijk is vervolgens toegankelijk maken door Failover-overschakeling uitvoeren van de DNS-server of het maken van de line-of-site tussen DR-netwerk en DNS.

    ![COM-fout](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Probleem 2: Configuratie van Site Recovery is mislukt (151196)
- **Mogelijke oorzaak** </br>
  - Verbinding kan niet worden gemaakt met Office 365-verificatie en identiteit IP4-eindpunten.

- **Resolutie**
  - Azure Site Recovery toegang tot Office 365-IP-adressen bereiken zijn vereist voor verificatie.
    Als u van Azure Network security group (NSG) regels/firewall-proxy gebruikmaakt voor het beheren van uitgaande netwerkverbindingen op de virtuele machine, controleert u of dat u communicatie met O365-IP-bereiken toestaan. Maak een [Azure Active Directory (AAD)-servicetag](../virtual-network/security-overview.md#service-tags) op basis van NSG-regel voor het toestaan van toegang tot alle IP-adressen die overeenkomen met AAD
      - Als er nieuwe adressen worden toegevoegd aan de Azure Active Directory (AAD) in de toekomst, moet u nieuwe NSG-regels maken.

> [!NOTE]
> Als de virtuele machines zich achter de **standaard** interne Load Balancer bevinden, heeft deze geen toegang tot O365 ip's, dat wil zeggen standaard login.microsoftonline.com. Wijzig deze in een **Basic** interne Load Balancer type of maak afhankelijke toegang zoals vermeld in het [artikel](https://aka.ms/lboutboundrulescli).

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Probleem 3: Configuratie van Site Recovery is mislukt (151197)
- **Mogelijke oorzaak** </br>
  - Kan geen verbinding maken met Azure Site Recovery service-eindpunten.

- **Resolutie**
  - Azure Site Recovery vereist toegang tot [Site Recovery-IP-adresbereiken](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) , afhankelijk van de regio. Zorg ervoor dat het ip-bereiken zijn toegankelijk is vanaf de virtuele machine vereist.


### <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Probleem 4: A2A-replicatie mislukt wanneer het netwerk verkeer via on-premises proxy server loopt (151072)
- **Mogelijke oorzaak** </br>
  - De aangepaste proxy instellingen zijn ongeldig en de Azure Site Recovery Mobility-Service agent heeft de proxy-instellingen niet automatisch gedetecteerd via IE


- **Resolutie**
  1. Mobility Service-agent detecteert de proxy-instellingen van Internet Explorer op Windows en /etc/environment op Linux.
  2. Als u liever proxy alleen instelt voor Azure Site Recovery Mobility-service, kunt u de proxy gegevens opgeven in ProxyInfo. conf op:</br>
     - ``/usr/local/InMage/config/`` op ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` op ***Windows***
  3. De ProxyInfo.conf moet de proxy-instellingen in de volgende INI-indeling hebben.</br>
                *[proxy]*</br>
                *Adres =http://1.2.3.4*</br>
                *Poort 567 =*</br>
  4. Azure Site Recovery Mobility Service-agent ondersteunt alleen ***niet-geverifieerde proxy's***.


### <a name="fix-the-problem"></a>Het probleem wordt opgelost
Volg de stappen in het [document voor netwerk](site-recovery-azure-to-azure-networking-guidance.md)ondersteuning om [de vereiste url's](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) of de [vereiste IP-bereiken](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges)toe te staan.

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Schijf niet gevonden in de machine (foutcode 150039)

Een nieuwe schijf die is gekoppeld aan de virtuele machine moet worden geïnitialiseerd.

**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
150039<br></br>**Bericht**: De Azure-gegevens schijf (DiskURI) met Logical Unit Number (LUN) (LUNValue) is niet toegewezen aan een overeenkomende schijf die wordt gerapporteerd vanuit de VM met dezelfde LUN-waarde. | -Een nieuwe gegevensschijf aan de VM is gekoppeld, maar deze is niet geïnitialiseerd.</br></br>-De gegevensschijf binnen de virtuele machine rapporteert de LUN-waarde waarop de schijf is gekoppeld aan de virtuele machine niet correct.| Zorg ervoor dat de gegevensschijven zijn geïnitialiseerd en voer de bewerking vervolgens opnieuw uit.</br></br>Voor Windows: [Een nieuwe schijf koppelen en initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Voor Linux: [Initialiseer een nieuwe gegevens schijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
Zorg ervoor dat de gegevensschijven zijn geïnitialiseerd en voer de bewerking vervolgens opnieuw uit:

- Voor Windows: [Een nieuwe schijf koppelen en initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Voor Linux: [toevoegen van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning.

## <a name="one-or-more-disks-are-available-for-protectionerror-code-153039"></a>Er zijn een of meer schijven beschikbaar voor beveiliging (fout code 153039)
- **Mogelijke oorzaak** </br>
  - Als er na de beveiliging onlangs een of meer schijven aan de virtuele machine zijn toegevoegd. 
  - Als een of meer schijven zijn geïnitialiseerd na de beveiliging van de virtuele machine.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
U kunt ervoor kiezen om de schijven te beveiligen of de waarschuwing te negeren om de replicatie status van de virtuele machine weer in orde te maken.</br>
1. Om de schijven te beveiligen. Navigeer naar gerepliceerde items > VM-> schijven > Klik op niet-beveiligde schijf > replicatie inschakelen.
 ![add_disks](./media/azure-to-azure-troubleshoot-errors/add-disk.png)
2. Om de waarschuwing te negeren. Ga naar gerepliceerde items > VM > Klik op het gedeelte waarschuwing negeren onder overzicht.
![dismiss_warning](./media/azure-to-azure-troubleshoot-errors/dismiss-warning.png)


## <a name="remove-the-virtual-machine-from-the-vault-completed-with-information--error-code-150225"></a>Verwijderen van de virtuele machine uit de kluis is voltooid met informatie (fout code 150225)
Op het moment van de beveiliging van de virtuele machine maakt Azure Site Recovery enkele koppelingen op de virtuele bron machine. Wanneer u de beveiliging verwijdert of replicatie uitschakelt, Azure Site Recovery deze koppelingen verwijderen als onderdeel van de opschoon taak. Als de virtuele machine een resource vergrendeling heeft, wordt de taak voltooid met de informatie. Hiermee wordt aangegeven dat de virtuele machine is verwijderd uit de Recovery Services-kluis, maar sommige verouderde koppelingen kunnen niet worden opgeruimd van de bron machine.

U kunt deze waarschuwing negeren als u deze virtuele machine niet meer wilt beveiligen in de toekomst. Als u deze virtuele machine echter later wilt beveiligen, moet u de koppelingen opschonen zoals vermeld in de volgende stappen. 

**Als u deze niet opschoont, gaat u als volgt te werk:**

1.  Tijdens het inschakelen van de replicatie via de Recovery Services-kluis wordt de virtuele machine niet weer gegeven. 
2.  Als u de virtuele machine probeert te beveiligen via de > instellingen voor het uitvoeren van de VM **> herstel na nood** geval mislukt met de fout ' de*replicatie kan niet worden ingeschakeld vanwege de bestaande verouderde bron koppelingen op de VM*'.


### <a name="fix-the-problem"></a>Het probleem wordt opgelost

>[!NOTE]
>
>Azure Site Recovery verwijdert de virtuele bron machine niet of heeft deze op geen enkele manier effect tijdens het uitvoeren van de onderstaande stappen.
>

1. Verwijder de vergren deling van de virtuele machine of VM-resource groep. Bijvoorbeeld: Onder de naam van de virtuele machine "MoveDemo" is de resource vergrendeling die moet worden verwijderd.

   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Script downloaden [verouderde Azure site Recovery configuratie verwijderen](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
3. Voer het script *Cleanup-stale-ASR-config-Azure-VM. ps1*uit.
4. Geef de abonnements-ID, VM-resource groep en VM-naam op als para meter.
5. Als Azure-referenties worden gevraagd, moet u ervoor zorgen dat het script zonder storingen wordt uitgevoerd. 


## <a name="replication-cannot-be-enabled-because-of-the-existing-stale-resource-links-on-the-vm-error-code-150226"></a>Replicatie kan niet worden ingeschakeld vanwege de bestaande verlopen bron koppelingen op de VM (fout code 150226)

**Oorzaak: De virtuele machine heeft een verouderde configuratie van de vorige Site Recovery beveiliging**

De verouderde configuratie kan worden blijven op een Azure-VM in de volgende gevallen:

- U hebt replicatie ingeschakeld voor de virtuele Azure-machine met behulp van Site Recovery en vervolgens de replicatie uitschakelen, maar de **bron-VM had een resource vergrendeling**.
- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de Site Recovery-kluis wordt verwijderd zonder expliciet uitschakelen van replicatie op de virtuele machine.
- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de resourcegroep die de Site Recovery-kluis bevat zonder expliciet uitschakelen van replicatie op de virtuele machine wordt verwijderd.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

>[!NOTE]
>
>Azure Site Recovery verwijdert de virtuele bron machine niet of heeft deze op geen enkele manier effect tijdens het uitvoeren van de onderstaande stappen.


1. Verwijder de vergren deling van de virtuele machine of VM-resource groep, indien aanwezig. *Bijvoorbeeld:* Onder de naam van de virtuele machine "MoveDemo" is de resource vergrendeling die moet worden verwijderd.
   
   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Script downloaden [verouderde Azure site Recovery configuratie verwijderen](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
3. Voer het script *Cleanup-stale-ASR-config-Azure-VM. ps1*uit.
4. Geef de abonnements-ID, VM-resource groep en VM-naam op als para meter.
5. Als Azure-referenties worden gevraagd, moet u ervoor zorgen dat het script zonder storingen wordt uitgevoerd.  

## <a name="unable-to-see-the-azure-vm-or-resource-group--for-selection-in-enable-replication"></a>Kan de virtuele machine van Azure of de resource groep niet zien voor selectie in replicatie inschakelen

 **Oorzaak 1:  De resource groep en de virtuele bron machine bevinden zich op een andere locatie**
 
Azure Site Recovery is momenteel verplicht om de resource groep en de virtuele machines van de bron regio op dezelfde locatie te vinden. Als dat niet het geval is, kunt u de virtuele machine of resource groep niet vinden tijdens het tijdstip van de beveiliging. 

**Als tijdelijke oplossing**kunt u replicatie van de virtuele machine inschakelen in plaats van de Recovery Services-kluis. Ga naar de > Eigenschappen van de bron-VM > nood herstel te herstellen en de replicatie in te scha kelen.

**Oorzaak 2: De resource groep maakt geen deel uit van het geselecteerde abonnement**

U kunt mogelijk niet vinden van de resourcegroep op het moment van beveiliging, als deze geen deel uitmaakt van het betreffende abonnement. Zorg ervoor dat de resourcegroep deel uitmaakt van het abonnement dat wordt gebruikt.

 **Oorzaak 3: Verouderde configuratie**
 
Als u de virtuele machine die u wilt inschakelen voor de replicatie niet ziet, wordt mogelijk vanwege een verouderde configuratie van Site Recovery blijven op de virtuele machine van Azure. De verouderde configuratie kan worden blijven op een Azure-VM in de volgende gevallen:

- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de Site Recovery-kluis wordt verwijderd zonder expliciet uitschakelen van replicatie op de virtuele machine.
- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de resourcegroep die de Site Recovery-kluis bevat zonder expliciet uitschakelen van replicatie op de virtuele machine wordt verwijderd.

- U hebt replicatie ingeschakeld voor de virtuele Azure-machine met behulp van Site Recovery en vervolgens de replicatie uitschakelen, maar de bron-VM had een resource vergrendeling.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

> [!NOTE]
>
> Zorg ervoor dat u de module ' AzureRM. resources ' bijwerkt voordat u het onderstaande script gebruikt. Azure Site Recovery verwijdert de virtuele bron machine niet of heeft deze op geen enkele manier effect tijdens het uitvoeren van de onderstaande stappen.
>

1. Verwijder de vergren deling van de virtuele machine of VM-resource groep, indien aanwezig. *Bijvoorbeeld:* Onder de naam van de virtuele machine "MoveDemo" is de resource vergrendeling die moet worden verwijderd.

   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Script downloaden [verouderde configuratie verwijderen](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
3. Voer het script *Cleanup-stale-ASR-config-Azure-VM. ps1*uit.
4. Geef de abonnements-ID, VM-resource groep en VM-naam op als para meter.
5. Als Azure-referenties worden gevraagd, moet u ervoor zorgen dat het script zonder storingen wordt uitgevoerd.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Kan geen virtuele machine voor beveiliging selecteert
 **Oorzaak 1:  Voor de virtuele machine is een uitbrei ding in een mislukte of niet-reagerende status geïnstalleerd** <br>
 Ga naar virtuele machines > instelling > extensies en controleer of er geen extensies in een foutstatus zijn. Verwijder de extensie is mislukt en probeer het opnieuw beveiligen van de virtuele machine.<br>
 **Oorzaak 2:  [De inrichtings status van de virtuele machine is niet geldig](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>De Inrichtingsstatus van de virtuele machine is niet geldig (foutcode 150019)

Inschakelen van replicatie op de virtuele machine, de Inrichtingsstatus moet **geslaagd**. U kunt de status van de virtuele machine controleren door de onderstaande stappen te volgen.

1.  Selecteer de **Resource Explorer** van **alle Services** in Azure portal.
2.  Vouw de **abonnementen** lijst en selecteer uw abonnement.
3.  Vouw de **ResourceGroups** lijst en selecteer de resourcegroep van de virtuele machine.
4.  Vouw de **Resources** lijst en selecteer uw virtuele machine
5.  Controleer de **provisioningState** veld in de weergave van de instantie op aan de rechterzijde.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

- Als **provisioningState** is **mislukt**, neem contact op met ondersteuning voor meer informatie om op te lossen.
- Als **provisioningState** is **bijwerken**, kan ophalen van een andere extensie geïmplementeerd. Controleer of er zijn actieve bewerkingen op de virtuele machine, wacht tot ze voltooid en probeer de mislukte Site Recovery **inschakelen replicatie** taak.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>Kan geen virtueel netwerk - tabblad voor netwerk-selectie is grijs doel selecteren.

**Oorzaak 1: Als uw virtuele machine is gekoppeld aan een netwerk dat al is toegewezen aan een Doelnet netwerk.**
- Als de bron-VM deel uit van een virtueel netwerk maakt en een andere virtuele machine in hetzelfde virtuele netwerk is al met een netwerk in de doelresourcegroep die is toegewezen, wordt klikt u vervolgens door de standaardselectie netwerk vervolgkeuzelijst uitgeschakeld.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**Oorzaak 2: Als u de virtuele machine eerder hebt beveiligd met Azure Site Recovery en de replicatie is uitgeschakeld.**
 - Uitschakelen van de replicatie van een virtuele machine verwijdert niet de netwerktoewijzing. Er moet worden verwijderd uit de recovery Services-kluis waarin de virtuele machine is beveiligd. </br>
 Navigeer naar de recovery Services-kluis > Site Recovery-infrastructuur > netwerktoewijzing. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - Doelnetwerk geconfigureerd tijdens de installatie van noodhersteladapter kan worden gewijzigd na de eerste instellen nadat de virtuele machine is beveiligd. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Houd er rekening mee dat als u netwerktoewijzing wijzigt van invloed op alle beveiligde virtuele machines die gebruikmaken van dat specifieke netwerktoewijzing.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ fout van de Volume Shadow Copy service (foutcode 151025)

**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
151025<br></br>**Bericht**: Kan de Site Recovery-extensie niet installeren | -, COM + System Application' service uitgeschakeld.</br></br>-'Volume Shadow Copy-service is uitgeschakeld.| 'COM + System Application' en 'Volume Shadow Copy' services ingesteld op automatische of handmatige opstartmodus.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

U kunt 'Services'-console opent en zorg ervoor dat de ' COM + System Application' en 'Volume Shadow Copy' zijn niet ingesteld op 'Uitgeschakeld' voor 'Opstarttype'.
  ![COM-fout](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Niet-ondersteunde beheerde schijf grootte (fout code 150172)


**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
150172<br></br>**Bericht**: De beveiliging kan niet worden ingeschakeld voor de virtuele machine, omdat deze de grootte (DiskSize) heeft die kleiner is dan de mini maal ondersteunde grootte van 1024 MB. | -De schijf is kleiner dan de ondersteunde grootte van 1024 MB| Zorg ervoor dat de schijf grootten binnen het ondersteunde bereik liggen en voer de bewerking opnieuw uit.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>Het inschakelen van de beveiliging is mislukt als de apparaatnaam die wordt vermeld in de GRUB-configuratie in plaats van de UUID (fout code 151126)

**Mogelijke oorzaak:** </br>
De GRUB-configuratie bestanden ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" of "/etc/default/grub") bevatten mogelijk de waarde voor de **root** van de para meters en worden **hervat** als de werkelijke APPARAATNAMEN in plaats van uuid. Site Recovery is een UUID-benadering omdat de naam van apparaten kan veranderen tijdens het opnieuw opstarten van de virtuele machine, omdat VM mogelijk niet wordt bereikt met dezelfde naam als de failover, waardoor er problemen ontstaan. Bijvoorbeeld: </br>


- De volgende regel is afkomstig uit het GRUB-bestand **/boot/grub2/grub.cfg**. <br>
  *linux/boot/vmlinuz-3.12.49-11-default **root =/dev/sda2** $ {extra_cmdline} **Resume =/dev/sda1** begin = Silent quiet showopts*


- De volgende regel wordt uit het bestand WORMGATEN **/boot/grub/menu.lst**
  *kernel /boot/vmlinuz-3.0.101-63-default **hoofdmap = / dev/sda2** **= / dev/sda1 hervatten** splash = op de achtergrond crashkernel 256M-:128M showopts vga = 0x314 =*

Als u de vetgedrukte teken reeks hierboven ziet, heeft GRUB werkelijke apparaatnamen voor de para meters ' root ' en ' Resume ' in plaats van UUID.

**Oplossen:**<br>
De apparaatnamen moeten worden vervangen door de bijbehorende UUID.<br>


1. Zoek de UUID van het apparaat door de opdracht ' blkid \<device name > ' uit te voeren. Bijvoorbeeld:<br>
   ```
   blkid /dev/sda1
   ```<br>
   ```/dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap" ```<br>
   ```blkid /dev/sda2```<br>
   ```/dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```<br>
   ```



1. Vervang nu de naam van het apparaat door de bijbehorende uuid in de notatie zoals root =\<uuid = uuid >. Als we bijvoorbeeld de apparaatnaam vervangen door UUID voor de para meter root en resume die hierboven wordt genoemd in de bestanden "/boot/grub2/grub.cfg", "/boot/grub2/grub.cfg" of "/etc/default/grub:, zien de regels in de bestanden er als volgt uit. <br>
   *kernel/boot/vmlinuz-3.0.101-63-default **root = uuid = 62927e85-f7ba-40bc-9993-cc1feeb191e4** **Resume = uuid = 6f614b44-433b-431b-9ca1-4dd2f6f74f6b** begin = Silent crashkernel = 256M-: 128M showopts VGA = 0x314*
1. De beveiliging opnieuw starten

## <a name="enable-protection-failed-as-device-mentioned-in-the-grub-configuration-doesnt-existerror-code-151124"></a>Het inschakelen van de beveiliging is mislukt omdat het apparaat dat wordt vermeld in de GRUB-configuratie niet bestaat (fout code 151124)
**Mogelijke oorzaak:** </br>
De GRUB-configuratie bestanden ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" of "/etc/default/grub") kunnen de para meters ' rd.lvm.lv ' of ' rd_LVM_LV ' bevatten om aan te geven dat het LVM-apparaat moet worden gedetecteerd op het moment van opstarten. Als deze LVM-apparaten niet bestaan, wordt het beveiligde systeem zelf niet opgestart en vastgelopen in het opstart proces. Ook wordt de failover-VM in acht genomen. Hieronder ziet u enkele voor beelden:

Enkele voor beelden: </br>

1. De volgende regel is uit het GRUB-bestand **/boot/grub2/grub.cfg** op RHEL7. </br>
   *linux16/vmlinuz-3.10.0-957.EL7.x86_64 root =/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel = 128M\@64M **rd. lvm. lv = rootvg/root RD. lvm. lv = rootvg/swap** rhgb stille lang = en_US. UTF-8*</br>
   Hier ziet u dat de GRUB twee LVM-apparaten met de namen **' root '** en **' swap '** in de volume groep ' rootvg ' moet detecteren.
1. De volgende regel is uit het GRUB-bestand **/etc/default/grub** op RHEL7 </br>
   *GRUB_CMDLINE_LINUX = "crashkernel = auto **rd. lvm. lv = rootvg/root RD. lvm. lv = rootvg/swap** rhgb quiet"*</br>
   Hier ziet u dat de GRUB twee LVM-apparaten met de namen **' root '** en **' swap '** in de volume groep ' rootvg ' moet detecteren.
1. De volgende regel is uit het GRUB-bestand **/boot/grub/menu.lst** op RHEL6 </br>
   *kernel/vmlinuz-2.6.32-754.el6.x86_64 ro root = UUID = 36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG = en_US. UTF-8 rd_NO_MD SYSFONT = latarcyrheb-sun16 crashkernel = auto rd_LVM_LV = rootvg/lv_root KEYBOARDTYPE = PC-tabel = US rd_LVM_LV = rootvg/lv_swap rd_NO_DM rhgb quiet* </br>
   Hier ziet u dat de GRUB twee LVM-apparaten met de namen **' root '** en **' swap '** in de volume groep ' rootvg ' moet detecteren.<br>

**Oplossen:**<br>

Als het LVM-apparaat niet bestaat, kunt u dit oplossen door het te maken of de para meter uit de GRUB-configuratie bestanden te verwijderen en de beveiliging opnieuw in te scha kelen. </br>

## <a name="site-recovery-mobility-service-update-completed-with-warnings--error-code-151083"></a>De update van de Site Recovery Mobility-service is voltooid met waarschuwingen (fout code 151083)
Site Recovery Mobility-service heeft veel onderdelen, een van de het filter stuur programma genoemd. Filter stuur programma wordt in het systeem geheugen alleen geladen op het moment dat het systeem opnieuw wordt opgestart. Wanneer er Site Recovery Mobility service-updates met wijzigingen in het filter stuur programma, worden de computer bijgewerkt, maar krijgt u een waarschuwing dat sommige oplossingen opnieuw moeten worden opgestart. Dit betekent dat de correcties van het filter stuur programma alleen kunnen worden gerealiseerd wanneer een nieuw filter stuur programma wordt geladen dat alleen kan optreden wanneer het systeem opnieuw wordt opgestart.<br>
**Houd er rekening mee** dat dit slechts een waarschuwing is en dat bestaande replicaties blijven werken, zelfs na de nieuwe update van de agent. U kunt ervoor kiezen om op elk gewenst moment opnieuw op te starten om de voor delen van het nieuwe filter stuur programma te krijgen, maar als u de computer niet opnieuw opstart dan ook het oude filter stuur programma blijft actief. Naast het filter stuur programma worden **de voor delen van andere verbeteringen en oplossingen in Mobility service gerealiseerd zonder dat de computer opnieuw hoeft te worden opgestart wanneer de agent wordt bijgewerkt.**  


## <a name="protection-couldnt-be-enabled-as-replica-managed-disk-diskname-replica-already-exists-without-expected-tags-in-the-target-resource-group-error-code-150161"></a>De beveiliging kan niet worden ingeschakeld omdat de door de replica beheerde schijf ' schijfwitness-replica ' al bestaat, zonder de verwachte labels in de doel resource groep (fout code 150161

**Oorzaak**: Dit kan gebeuren als de virtuele machine eerder in het verleden is beveiligd en tijdens het uitschakelen van de replicatie, de replica schijf om een of andere reden niet is opgeschoond.</br>
**Oplossen:** Verwijder de vermelde replica schijf in het fout bericht en start de mislukte beveiligings taak opnieuw.

## <a name="next-steps"></a>Volgende stappen
[Virtuele Azure-machines repliceren](site-recovery-replicate-azure-to-azure.md)
