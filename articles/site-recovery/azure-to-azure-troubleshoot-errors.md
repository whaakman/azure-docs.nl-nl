---
title: Azure Site Recovery voor probleemoplossing voor problemen met replicatie van Azure naar Azure en fouten | Microsoft Docs
description: Het oplossen van fouten en problemen bij het repliceren van virtuele machines voor herstel na noodgevallen van Azure
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sujayt
ms.openlocfilehash: b4359a90bb511b538499848effea7be22a23fac0
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570620"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Problemen met Azure-naar-Azure-VM-replicatie oplossen

Dit artikel beschrijft de voorkomende problemen in Azure Site Recovery bij het repliceren en herstellen van virtuele machines van Azure van de ene regio naar een andere regio en wordt uitgelegd hoe u oplossingen voor deze problemen. Zie voor meer informatie over ondersteunde configuraties, de [ondersteuningsmatrix voor het repliceren van virtuele Azure-machines](site-recovery-support-matrix-azure-to-azure.md).

## <a name="list-of-errors"></a>Lijst met fouten
- **[Quotumproblemen met he Azure-resource (foutcode 150097)](#azure-resource-quota-issues-error-code-150097)** 
- **[Vertrouwde basiscertificaten (foutcode 151066)](#trusted-root-certificates-error-code-151066)** 
- **[Uitgaande connectiviteit voor Site Recovery (foutcode 151195)](#issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br)** 

## <a name="azure-resource-quota-issues-error-code-150097"></a>Quotumproblemen met he Azure-resource (foutcode 150097)
Uw abonnement moet worden ingeschakeld om te maken van virtuele Azure-machines in de doelregio die u van plan bent om te gebruiken als uw Dr-regio. Uw abonnement moet ook voldoende quotum ingeschakeld als u virtuele machines van specifieke grootte hebben. Standaard wordt met Site Recovery even groot is voor de doel-VM kiest als de bron-VM. Als de overeenkomende grootte niet beschikbaar is, wordt automatisch de dichtstbijzijnde grootte opgehaald. Als er geen overeenkomende grootte die ondersteuning biedt voor bron-VM-configuratie is, wordt dit foutbericht weergegeven:

**Foutcode** | **Mogelijke oorzaken** | **Aanbeveling**
--- | --- | ---
150097<br></br>**Bericht**: Replicatie kan niet worden ingeschakeld voor de virtuele machine VmName. | -Uw abonnements-ID kan niet worden ingeschakeld om te maken van virtuele machines op de doellocatie voor de regio.</br></br>-Uw abonnements-ID kan niet worden ingeschakeld of beschikt niet over voldoende quotum voor het maken van specifieke VM-grootten op de doellocatie voor de regio.</br></br>-Een geschikt doel-VM-grootte die overeenkomt met de bron-VM NIC aantal (2) is niet gevonden voor de abonnements-ID op de doellocatie voor de regio.| Neem contact op met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om te kunnen maken van de virtuele machine voor de vereiste VM-grootten op de doellocatie voor uw abonnement. Nadat deze ingeschakeld, moet u de mislukte bewerking opnieuw proberen.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
U kunt contact opnemen met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om in te schakelen van uw abonnement te maken van virtuele machines van de vereiste grootte in de doellocatie.

Als de doellocatie een beperking van capaciteit heeft, replicatie uitschakelen en inschakelen naar een andere locatie waar uw abonnement voldoende quotum voor het maken van virtuele machines van de vereiste grootte heeft.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrouwde basiscertificaten (foutcode 151066)

Als de meest recente vertrouwde basiscertificaten niet aanwezig op de virtuele machine, mislukken uw taak 'replicatie inschakelen'. Zonder de certificaten mislukt de verificatie en autorisatie van Site Recovery-serviceaanroepen van de virtuele machine. Het foutbericht voor de taak is mislukt 'replicatie inschakelen' Site Recovery wordt weergegeven:

**Foutcode** | **Mogelijke oorzaak** | **Aanbevelingen**
--- | --- | ---
151066<br></br>**Bericht**: Configuratie van site Recovery is mislukt. | De vereiste vertrouwde basiscertificaten gebruikt voor autorisatie en verificatie zijn niet aanwezig op de machine. | -Voor een virtuele machine met het Windows-besturingssysteem, zorg ervoor dat de vertrouwde basiscertificaten aanwezig zijn op de machine. Zie voor meer informatie, [configureren van vertrouwde basiscertificaten en niet-toegestane certificaten](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Voor een virtuele machine met het Linux-besturingssysteem, volg de richtlijnen voor vertrouwde basiscertificaten gepubliceerd door de distributeur versie van Linux-besturingssysteem.

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

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

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

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Probleem 1: Kan niet registreren van virtuele machine van Azure met Site Recovery (151195) </br>
- **Mogelijke oorzaak** </br>
  - Kan geen verbinding maken met site recovery-eindpunten vanwege een probleem met de DNS-omzetting.
  - Dit wordt vaker gezien tijdens opnieuw beveiliging wanneer u de virtuele machine failover, maar de DNS-server niet bereikbaar vanaf de DR-regio is.
  
- **Resolutie**
   - Als u aangepaste DNS gebruikt, zorg ervoor dat is de DNS-server toegankelijk vanuit de regio voor herstel na noodgevallen. Om te controleren of u een aangepaste DNS-server gaat u naar de virtuele machine > herstel na noodgevallen netwerk > DNS-servers. Probeer toegang tot de DNS-server van de virtuele machine. Als deze niet toegankelijk is vervolgens toegankelijk maken door Failover-overschakeling uitvoeren van de DNS-server of het maken van de line-of-site tussen DR-netwerk en DNS.
  
    ![COM-fout](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)
 

### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Probleem 2: Configuratie van site Recovery is mislukt (151196)
- **Mogelijke oorzaak** </br>
  - Verbinding kan niet worden gemaakt met Office 365-verificatie en identiteit IP4-eindpunten.

- **Resolutie**
  - Azure Site Recovery toegang tot Office 365-IP-adressen bereiken zijn vereist voor verificatie.
    Als u van Azure Network security group (NSG) regels/firewall-proxy gebruikmaakt voor het beheren van uitgaande netwerkverbindingen op de virtuele machine, controleert u of dat u communicatie met O365-IP-bereiken toestaan. Maak een [Azure Active Directory (AAD)-servicetag](../virtual-network/security-overview.md#service-tags) op basis van NSG-regel voor het toestaan van toegang tot alle IP-adressen die overeenkomen met AAD
        - Als er nieuwe adressen worden toegevoegd aan de Azure Active Directory (AAD) in de toekomst, moet u nieuwe NSG-regels maken.
>[!NOTE]
> Als de virtuele machines achter **Standard** interne load balancer dan geen toegang tot O365-IP-adressen Internet Explorer Login.micorsoftonline.com standaard. Wijzig het **Basic** interne type load balancer of uitgaande toegang maken, zoals vermeld in de [artikel] (https://aka.ms/lboutboundrulescli).
>

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Probleem 3: Configuratie van site Recovery is mislukt (151197)
- **Mogelijke oorzaak** </br>
  - Kan geen verbinding maken met Azure Site Recovery service-eindpunten.

- **Resolutie**
  - Azure Site Recovery vereist toegang tot [Site Recovery-IP-adresbereiken](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) , afhankelijk van de regio. Zorg ervoor dat het ip-bereiken zijn toegankelijk is vanaf de virtuele machine vereist.
    

### <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premise-proxy-server-151072"></a>Probleem met 4: A2A-replicatie is mislukt tijdens het netwerkverkeer via on-premises proxy-server (151072 verloopt)
 - **Mogelijke oorzaak** </br>
   - De aangepaste proxy-instellingen zijn ongeldig en ASR Mobility Service-agent heeft geen automatische detectie de proxy-instellingen van Internet Explorer


 - **Resolutie**
   1.   Mobility Service-agent detecteert de proxy-instellingen van Internet Explorer op Windows en /etc/environment op Linux.
   2.  Als u liever proxy alleen voor ASR Mobility-Service instellen, kunt u de proxygegevens in ProxyInfo.conf dat zich bevindt in opgeven:</br>
       - ``/usr/local/InMage/config/`` op ***Linux***
       - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` op ***Windows***
   3.   De ProxyInfo.conf moet de proxy-instellingen in de volgende INI-indeling hebben.</br>
                   *[proxy]*</br>
                   *Adres =http://1.2.3.4*</br>
                   *Poort 567 =*</br>
   4. ASR Mobility Service-agent ondersteunt alleen ***niet-geverifieerde proxy's***.
 

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
Aan lijst met geaccepteerde [de vereiste URL's](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) of de [vereiste IP-adresbereiken](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), volg de stappen in de [netwerken document met richtlijnen](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Schijf niet gevonden in de machine (foutcode 150039)

Een nieuwe schijf die is gekoppeld aan de virtuele machine moet worden geïnitialiseerd.

**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
150039<br></br>**Bericht**: Azure-gegevensschijf (DiskName) (DiskURI) met logische unit number (LUN) (LUNValue) is niet toegewezen aan een overeenkomende schijf die wordt doorgegeven vanuit de virtuele machine met dezelfde LUN-waarde. | -Een nieuwe gegevensschijf aan de VM is gekoppeld, maar deze is niet geïnitialiseerd.</br></br>-De gegevensschijf binnen de virtuele machine rapporteert de LUN-waarde waarop de schijf is gekoppeld aan de virtuele machine niet correct.| Zorg ervoor dat de gegevensschijven zijn geïnitialiseerd en voer de bewerking vervolgens opnieuw uit.</br></br>Voor Windows: [Koppelen en een nieuwe schijf initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Voor Linux: [Initialiseren van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
Zorg ervoor dat de gegevensschijven zijn geïnitialiseerd en voer de bewerking vervolgens opnieuw uit:

- Voor Windows: [Koppelen en een nieuwe schijf initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Voor Linux: [toevoegen van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Kan niet zien van de Azure-VM voor selectie in 'replicatie inschakelen'

 **1 oorzaak:  Resourcegroep en de virtuele bronmachine zich in andere locatie** <br>
Azure Site Recovery voorgeschreven momenteel dat bronresourcegroep regio en virtuele machines moeten zich in dezelfde locatie. Als dat niet het geval is klikt zou u niet kunnen vinden van de virtuele machine tijdens het ophalen van beveiliging.

**2 oorzaak: Resourcegroep maakt geen deel uit van het geselecteerde abonnement** <br>
U kunt mogelijk niet vinden van de resourcegroep op het moment van beveiliging, als deze geen deel uitmaakt van het betreffende abonnement. Zorg ervoor dat de resourcegroep deel uitmaakt van het abonnement dat wordt gebruikt.

 **3 oorzaak: Verouderde configuratie** <br>
Als u de virtuele machine die u wilt inschakelen voor de replicatie niet ziet, wordt mogelijk vanwege een verouderde configuratie van Site Recovery blijven op de virtuele machine van Azure. De verouderde configuratie kan worden blijven op een Azure-VM in de volgende gevallen:

- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de Site Recovery-kluis wordt verwijderd zonder expliciet uitschakelen van replicatie op de virtuele machine.
- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de resourcegroep die de Site Recovery-kluis bevat zonder expliciet uitschakelen van replicatie op de virtuele machine wordt verwijderd.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

>[!NOTE] 
>
>Zorg ervoor dat u de module "" AzureRM.Resources"" bijwerken voordat u het onderstaande script. 

U kunt [verwijderen van verouderde ASR-configuratiescript](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) en verwijdert de verouderde Site Recovery-configuratie op de virtuele machine van Azure. U zou het mogelijk om te zien van de virtuele machine na het verwijderen van de verouderde configuratie.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Kan geen virtuele machine voor beveiliging selecteert 
 **1 oorzaak:  Virtuele machine uitbreidings-sommige in de status van een mislukte of reageert niet geïnstalleerd** <br>
 Ga naar virtuele machines > instelling > extensies en controleer of er geen extensies in een foutstatus zijn. Verwijder de extensie is mislukt en probeer het opnieuw beveiligen van de virtuele machine.<br>
 **2 oorzaak:  [De Inrichtingsstatus van de virtuele machine is niet geldig](#vms-provisioning-state-is-not-valid-error-code-150019)**

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

**1 oorzaak: Als uw virtuele machine is gekoppeld aan een netwerk dat al aan een doelnetwerk toegewezen is.**
- Als de bron-VM deel uit van een virtueel netwerk maakt en een andere virtuele machine in hetzelfde virtuele netwerk is al met een netwerk in de doelresourcegroep die is toegewezen, wordt klikt u vervolgens door de standaardselectie netwerk vervolgkeuzelijst uitgeschakeld.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**2 oorzaak: Als u eerder hebt beveiligd van de virtuele machine met Azure Site Recovery en de replicatie uitgeschakeld.**
 - Uitschakelen van de replicatie van een virtuele machine verwijdert niet de netwerktoewijzing. Er moet worden verwijderd uit de recovery Services-kluis waarin de virtuele machine is beveiligd. </br>
 Navigeer naar de recovery Services-kluis > Site Recovery-infrastructuur > netwerktoewijzing. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - Doelnetwerk geconfigureerd tijdens de installatie van noodhersteladapter kan worden gewijzigd na de eerste instellen nadat de virtuele machine is beveiligd. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Houd er rekening mee dat als u netwerktoewijzing wijzigt van invloed op alle beveiligde virtuele machines die gebruikmaken van dat specifieke netwerktoewijzing.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ fout van de Volume Shadow Copy service (foutcode 151025)
**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
151025<br></br>**Bericht**: Kan de extensie voor site recovery niet installeren | -, COM + System Application' service uitgeschakeld.</br></br>-'Volume Shadow Copy-service is uitgeschakeld.| 'COM + System Application' en 'Volume Shadow Copy' services ingesteld op automatische of handmatige opstartmodus.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

U kunt 'Services'-console opent en zorg ervoor dat de ' COM + System Application' en 'Volume Shadow Copy' zijn niet ingesteld op 'Uitgeschakeld' voor 'Opstarttype'.
  ![COM-fout](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Niet-ondersteunde beheerd schijfgrootte (foutcode 150172)


**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
150172<br></br>**Bericht**: Beveiliging kan niet worden ingeschakeld voor de virtuele machine omdat er (DiskName) met grootte (DiskSize) die lager is dan de minimaal ondersteunde grootte van 1024 MB. | -De schijf is kleiner dan de ondersteunde grootte van 1024 MB| Ervoor zorgen dat de schijfgrootten binnen het ondersteunde bereik valt en probeer het opnieuw. 

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>Schakel de beveiliging is mislukt omdat de apparaatnaam die worden vermeld in de GRUB-configuratie in plaats van de UUID (foutcode 151126)

**Mogelijke oorzaak:** </br>
De GRUB-configuratiebestanden (' / boot/grub/menu.lst ', ' / boot/grub/grub.cfg ', ' / boot/grub2/grub.cfg ' of '/ standaard/etc/wormgaten') kan de waarde voor de parameters bevatten **hoofdmap** en **hervatten** als de werkelijke apparaatnamen in plaats van UUID. Site Recovery mandaten UUID benadering, zoals de naam van de apparaten voor opnieuw opstarten van de virtuele machine verschilt mogelijk als virtuele machine mogelijk niet afkomstig is-up met dezelfde naam leidt tot problemen met failover. Bijvoorbeeld: </br>


- De volgende regel wordt uit het bestand WORMGATEN **/boot/grub2/grub.cfg**. <br>
*linux   /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2**  ${extra_cmdline} **resume=/dev/sda1** splash=silent quiet showopts*


- De volgende regel wordt uit het bestand WORMGATEN **/boot/grub/menu.lst**
*kernel /boot/vmlinuz-3.0.101-63-default **hoofdmap = / dev/sda2** **= / dev/sda1 hervatten ** splash = op de achtergrond crashkernel 256M-:128M showopts vga = 0x314 =*

Als u de bovenstaande vet tekenreeks ziet, bevat WORMGATEN daadwerkelijk apparaatnamen voor de parameters "root" en "Doorgaan" in plaats van UUID.
 
**Over het oplossen van:**<br>
De apparaatnamen moeten worden vervangen door de bijbehorende UUID.<br>


1. De UUID van het apparaat vinden door het uitvoeren van de opdracht ' blkid <device name>'. Bijvoorbeeld:<br>
```
blkid /dev/sda1 
```<br>
```/dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap" ```<br>
```blkid /dev/sda2```<br> 
```/dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3" 
```<br>



1. Now replace the device name with its UUID in the format like "root=UUID=<UUID>". For example, if we replace the device names with UUID for root and resume parameter mentioned above in the files "/boot/grub2/grub.cfg", "/boot/grub2/grub.cfg" or "/etc/default/grub: then the lines in the files looks like. <br>
*kernel /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4** **resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b** splash=silent crashkernel=256M-:128M showopts vga=0x314*
1. Restart the protection again

## Enable protection failed as device mentioned in the GRUB configuration doesn't exist(error code 151124)
**Possible Cause:** </br>
The GRUB configuration files ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" or "/etc/default/grub") may contain the parameters "rd.lvm.lv" or "rd_LVM_LV" to indicate the LVM device that should be discovered at the time of booting. If these LVM devices doesn't exist, then the protected system itself will not boot and stuck in the boot process. Even the same will be observed with the failover VM. Below are few examples: 

Few examples: </br>

1. The following line is from the GRUB file **"/boot/grub2/grub.cfg"** on RHEL7. </br>
*linux16 /vmlinuz-3.10.0-957.el7.x86_64 root=/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel=128M@64M **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet LANG=en_US.UTF-8*</br>
Here the highlighted portion shows that the GRUB has to detect two LVM devices with names **"root"** and **"swap"** from the volume group "rootvg". 
1. The following line is from the GRUB file **"/etc/default/grub"** on RHEL7 </br>
 *GRUB_CMDLINE_LINUX="crashkernel=auto **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet"*</br>
Here the highlighted portion shows that the GRUB has to detect two LVM devices with names **"root"** and **"swap"** from the volume group "rootvg". 
1. The following line is from the GRUB file **"/boot/grub/menu.lst"** on RHEL6 </br>
*kernel /vmlinuz-2.6.32-754.el6.x86_64 ro root=UUID=36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=rootvg/lv_root  KEYBOARDTYPE=pc KEYTABLE=us rd_LVM_LV=rootvg/lv_swap rd_NO_DM rhgb quiet* </br>
 Here the highlighted portion shows that the GRUB has to detect two LVM devices with names **"root"** and **"swap"** from the volume group "rootvg".<br>

**How to Fix:**<br>

If the LVM device doesn't exist, fix either by creating it or remove the parameter for the same from the GRUB configuration files and then retry the enable protection. </br>

## Site recovery mobility service update completed with warnings ( error code 151083)
Site Recovery mobility service has many components, one of which is called filter driver. Filter driver gets loaded into system memory only at a time of system reboot. Whenever there are  site recovery mobility service updates that has filter driver changes, we update the machine but still gives you warning that some fixes require a reboot. It means that the filter driver fixes can only be realized when a new filter driver is loaded which can happen only at the time of system reboot.<br>
**Please note** that this is just a warning and existing replication keeps on working even after the new agent update. You can choose to reboot anytime you want to get the benefits of new filter driver but if you don't reboot than also old filter driver keeps on working. Apart from filter driver, **benefits of  any other enhancements and fixes in mobility service get realized without any reboot when the agent gets updated.**  


## Protection couldn't be enabled as replica managed disk 'diskname-replica' already exists without expected tags in the target resource group( error code 150161

**Cause**: It can occur if the  virtual machine was protected earlier in the past and during disabling the replication, replica disk was not cleaned due to some reason.</br>
**How to fix:** 
Delete the mentioned replica disk in the error message and restart the failed protection job again. 

## Next steps
[Replicate Azure virtual machines](site-recovery-replicate-azure-to-azure.md)
