---
title: Azure Site Recovery voor probleemoplossing voor problemen met de replicatie van Azure naar Azure en fouten | Microsoft Docs
description: Het oplossen van fouten en problemen bij het repliceren van virtuele machines voor herstel na noodgevallen in Azure
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: sujayt
ms.openlocfilehash: be43e34976682847c4756e062ec5b638ebc26063
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Oplossen van problemen met de replicatie van de virtuele machine van Azure naar Azure

Dit artikel wordt beschreven welke algemene problemen in de Azure Site Recovery wanneer repliceren en herstellen van virtuele machines in Azure vanaf één regio naar een andere regio en wordt uitgelegd hoe u ze problemen oplossen. Zie voor meer informatie over ondersteunde configuraties, de [ondersteuningsmatrix voor het repliceren van virtuele Azure-machines](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemen met Azure-resource-quotum (foutcode 150097)
Uw abonnement moet worden ingeschakeld voor het maken van virtuele Azure-machines in de doelregio die u wilt gebruiken als uw herstel na noodgevallen regio. Uw abonnement moet ook voldoende quota is ingeschakeld voor het maken van virtuele machines met een specifieke grootte hebben. Standaard hervat Site Recovery dezelfde grootte hebben voor het doel VM als de bron-VM. Als de grootte van de overeenkomende niet beschikbaar is, wordt de grootte van de dichtstbijzijnde mogelijk automatisch gekozen. Als er geen overeenkomende grootte die ondersteuning biedt voor VM-configuratie bron is, wordt dit foutbericht weergegeven:

Foutcode | **Mogelijke oorzaken** | Aanbeveling
--- | --- | ---
150097<br></br>**Bericht**: replicatie kan niet worden ingeschakeld voor de virtuele machine VmName. | -Uw abonnements-ID is mogelijk niet ingeschakeld voor het maken van alle virtuele machines in de doellocatie van de regio.</br></br>-Uw abonnements-ID is mogelijk niet ingeschakeld of beschikt niet over voldoende quotum voor het specifieke VM-grootten maken in de doellocatie van de regio.</br></br>-Een geschikte doel VM-grootte die overeenkomt met de bron-VM NIC aantal (2) is niet gevonden voor de abonnements-ID in de doellocatie van de regio.| Neem contact op met [ondersteuning voor facturering aan Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) voor het maken van de virtuele machine voor het vereiste VM-grootte in de doellocatie voor uw abonnement. Nadat deze ingeschakeld, moet u de mislukte bewerking opnieuw.

### <a name="fix-the-problem"></a>Los het probleem
U kunt contact opnemen met [ondersteuning voor facturering aan Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om in te schakelen van uw abonnement te maken van virtuele machines van de vereiste grootte in de doellocatie.

Als de doellocatie een beperking van capaciteit heeft, Schakel replicatie uit en naar een andere locatie waar uw abonnement voldoende quotum voor het maken van virtuele machines van de vereiste grootte heeft.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrouwde basiscertificaten (foutcode 151066)

Als de meest recente vertrouwde certificaten niet aanwezig op de virtuele machine, kan de taak 'replicatie inschakelen' mislukken. Zonder de certificaten mislukt de verificatie en autorisatie van Site Recovery serviceaanroepen van de virtuele machine. Het foutbericht voor de taak is mislukt 'replicatie inschakelen' Site Recovery wordt weergegeven:

Foutcode | **Mogelijke oorzaak** | **Aanbevelingen**
--- | --- | ---
151066<br></br>**Bericht**: Site Recovery-configuratie is mislukt. | De vereiste vertrouwde basiscertificaten wordt gebruikt voor autorisatie en verificatie niet beschikbaar zijn op de machine. | -Voor een virtuele machine met de Windows-besturingssysteem, zorg ervoor dat de vertrouwde basiscertificaten aanwezig is op de computer. Zie voor informatie [configureren van vertrouwde basiscertificaten en niet-toegestane certificaten](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Volg de richtlijnen voor vertrouwde basiscertificaten gepubliceerd door de Linux-besturingssysteem versie distributor voor een virtuele machine met het Linux-besturingssysteem.

### <a name="fix-the-problem"></a>Los het probleem
**Windows**

Installeer de meest recente Windows updates op de virtuele machine zodat de vertrouwde basiscertificaten aanwezig op de machine zijn. Als u zich in een omgeving zonder verbinding, volgt u de standaard Windows update-proces in uw organisatie de certificaten op te halen. Als de vereiste certificaten niet aanwezig op de virtuele machine, mislukken de aanroepen naar de Site Recovery-service uit veiligheidsoverwegingen.

Volg het normale Windows update management of certificaat updatebeheerproces in uw organisatie om op te halen van de meest recente basiscertificaten en de bijgewerkte lijst met ingetrokken certificaten op de virtuele machines.

Om te controleren of het probleem opgelost is, gaat u naar login.microsoftonline.com vanuit een browser op uw virtuele machine.

**Linux**

Volg de richtlijnen bij uw Linux-distributor ophalen van de meest recente vertrouwde basiscertificaten en de meest recente certificaatintrekkingslijst op de virtuele machine.

Omdat symlinks SuSE Linux gebruikt voor het onderhouden van een lijst van certificaten, als volgt te werk:

1.  Aanmelden als een hoofdgebruiker.

2.  Voer deze opdracht om te wijzigen van de map.

      ``# cd /etc/ssl/certs``

3. Controleer of de basis-CA-certificaat van Symantec aanwezig is.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Als de basis-CA-certificaat van Symantec niet wordt gevonden, voer de volgende opdracht om het bestand te downloaden. Controleer op fouten en volg de aanbevolen actie voor netwerkfouten.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Controleer of het Baltimore basis-CA-certificaat aanwezig is.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Als het Baltimore basis-CA-certificaat niet is gevonden, downloadt u het certificaat.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. Controleer of het certificaat DigiCert_Global_Root_CA aanwezig is.

    ``# ls DigiCert_Global_Root_CA.pem``

8. Als de DigiCert_Global_Root_CA niet wordt gevonden, voer de volgende opdrachten om het certificaat te downloaden.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Voer rehash script bijwerken van het certificaat onderwerp hashes voor de zojuist gedownloade certificaten.

    ``# c_rehash``

10. Controleer of het onderwerp hashes als symlinks zijn gemaakt voor de certificaten.

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

11. Maak een kopie van het bestand VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem met filename b204d74a.0

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Maak een kopie van het bestand Baltimore_CyberTrust_Root.pem met filename 653b494a.0

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


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Uitgaande verbinding voor de Site Recovery-URL's of het IP-adresbereiken (foutcode 151037 of 151072)

Voor replicatie van Site Recovery voor werk, uitgaande verbinding met een specifieke URL's of het IP-adresbereiken zijn van de virtuele machine. Als uw VM zich achter een firewall of (NSG) netwerkbeveiligingsgroepen waarmee uitgaande verbinding gebruikt, ziet u mogelijk een van deze foutberichten:

**Foutcodes** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
151037<br></br>**Bericht**: registratie van de virtuele machine van Azure met Site Recovery is mislukt. | -U NSG uitgaande toegang op de virtuele machine en de vereiste IP-adresbereiken zijn niet goedgekeurde lijst voor uitgaande toegang.</br></br>-U hulpprogramma's van derden firewall en de vereiste IP-adresbereiken/URL's zijn niet wilt plaatsen.</br>| -Als u uitgaande netwerkverbinding op de virtuele machine beheren firewallproxy gebruikt, zorg ervoor dat de vereiste URL's of datacenter IP-adresbereiken wilt plaatsen. Zie voor informatie [firewall-proxy richtlijnen](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-Als u NSG-regels waarmee uitgaande netwerkverbinding op de virtuele machine gebruikt, zorg ervoor dat de vereiste datacenter IP-adresbereiken wilt plaatsen. Zie voor informatie [groep beveiligingsrichtlijnen netwerk](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Bericht**: Site Recovery-configuratie is mislukt. | Kan geen verbinding maken met Site Recovery service-eindpunten. | -Als u uitgaande netwerkverbinding op de virtuele machine beheren firewallproxy gebruikt, zorg ervoor dat de vereiste URL's of datacenter IP-adresbereiken wilt plaatsen. Zie voor informatie [firewall-proxy richtlijnen](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-Als u NSG-regels waarmee uitgaande netwerkverbinding op de virtuele machine gebruikt, zorg ervoor dat de vereiste datacenter IP-adresbereiken wilt plaatsen. Zie voor informatie [groep beveiligingsrichtlijnen netwerk](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Los het probleem
Aan de lijst met geaccepteerde [de gewenste URL's](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) of de [IP-adresbereiken vereist](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), volg de stappen in de [leidraad voor netwerken](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Schijf niet gevonden in de machine (foutcode 150039)

Een nieuwe schijf die is gekoppeld aan de virtuele machine moet worden geïnitialiseerd.

Foutcode | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
150039<br></br>**Bericht**: Azure gegevensschijf (DiskName) (DiskURI) met de logische eenheid number (LUN) (LUNValue) is niet toegewezen aan een bijbehorende schijf wordt gerapporteerd door binnen de virtuele machine die dezelfde LUN-waarde heeft. | -Een nieuwe gegevensschijf is gekoppeld aan de virtuele machine, maar deze is niet geïnitialiseerd.</br></br>-De gegevensschijf in de virtuele machine is de LUN-waarde waarmee de schijf is gekoppeld aan de virtuele machine niet correct rapporteren.| Zorg dat de gegevensschijven worden geïnitialiseerd en probeer het opnieuw.</br></br>Voor Windows: [koppelen en een nieuwe schijf initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Voor Linux: [initialiseren van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Los het probleem
Zorg ervoor dat de gegevensschijven geïnitialiseerd en probeer het vervolgens opnieuw:

- Voor Windows: [koppelen en een nieuwe schijf initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Voor Linux: [toevoegen van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Kan niet voor een overzicht van de virtuele machine van Azure voor selectie in "replicatie inschakelen"

Als u de virtuele machine die u wilt inschakelen voor de replicatie niet ziet, deze mogelijk vanwege een verouderde configuratie van Site Recovery blijven op de virtuele machine in Azure. De verouderde configuratie kan worden links op een virtuele machine van Azure in de volgende gevallen:

- U replicatie is ingeschakeld voor de virtuele machine in Azure met behulp van Site Recovery en vervolgens de Site Recovery-kluis verwijderd zonder expliciet uitschakeling van replicatie op de virtuele machine.
- U replicatie is ingeschakeld voor de virtuele machine in Azure met behulp van Site Recovery en vervolgens de resourcegroep met de Site Recovery-kluis zonder expliciet uitschakeling van replicatie op de virtuele machine wordt verwijderd.

### <a name="fix-the-problem"></a>Los het probleem

U kunt [verwijderen van verouderde ASR configuratiescript](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) en verwijdert u de verouderde Site Recovery-configuratie op de virtuele machine in Azure. U moet mogelijk zijn om te zien van de virtuele machine na het verwijderen van verouderde configuratie.

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>De Inrichtingsstatus van de virtuele machine is niet geldig (foutcode 150019)

Schakel replicatie op de virtuele machine door de Inrichtingsstatus moet **geslaagd**. U kunt de VM-status controleren door de onderstaande stappen te volgen.

1.  Selecteer de **Resource Explorer** van **alle Services** in Azure-portal.
2.  Vouw de **abonnementen** lijst en uw abonnement te selecteren.
3.  Vouw de **ResourceGroups** lijst en selecteert u de resourcegroep van de virtuele machine.
4.  Vouw de **Resources** lijst en selecteer de virtuele machine
5.  Controleer de **provisioningState** veld in de weergave op de rechterzijde exemplaar.

### <a name="fix-the-problem"></a>Los het probleem

- Als **provisioningState** is **mislukt**, contact op met de ondersteuning met details om op te lossen.
- Als **provisioningState** is **Updating**, kan ophalen van een andere extensie geïmplementeerd. Controleer of er zijn geen actieve bewerkingen op de virtuele machine, wachten totdat ze zijn voltooid en probeer opnieuw het herstel van de mislukte Site **replicatie inschakelen** taak.

## <a name="next-steps"></a>Volgende stappen
[Virtuele Azure-machines repliceren](site-recovery-replicate-azure-to-azure.md)
