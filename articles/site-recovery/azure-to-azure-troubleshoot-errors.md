---
title: Azure Site Recovery voor probleemoplossing voor problemen met replicatie van Azure naar Azure en fouten | Microsoft Docs
description: Het oplossen van fouten en problemen bij het repliceren van virtuele machines voor herstel na noodgevallen van Azure
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: sujayt
ms.openlocfilehash: a41cd658060ef92efb0fc21a98ca616276378c5e
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113851"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Problemen met Azure-naar-Azure-VM-replicatie oplossen

Dit artikel beschrijft de voorkomende problemen in Azure Site Recovery bij het repliceren en herstellen van virtuele machines van Azure van de ene regio naar een andere regio en wordt uitgelegd hoe u oplossingen voor deze problemen. Zie voor meer informatie over ondersteunde configuraties, de [ondersteuningsmatrix voor het repliceren van virtuele Azure-machines](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Quotumproblemen met he Azure-resource (foutcode 150097)
Uw abonnement moet worden ingeschakeld om te maken van virtuele Azure-machines in de doelregio die u van plan bent om te gebruiken als uw Dr-regio. Uw abonnement moet ook voldoende quotum ingeschakeld als u virtuele machines van specifieke grootte hebben. Standaard wordt met Site Recovery even groot is voor de doel-VM kiest als de bron-VM. Als de overeenkomende grootte niet beschikbaar is, wordt automatisch de dichtstbijzijnde grootte opgehaald. Als er geen overeenkomende grootte die ondersteuning biedt voor bron-VM-configuratie is, wordt dit foutbericht weergegeven:

**Foutcode** | **Mogelijke oorzaken** | **Aanbeveling**
--- | --- | ---
150097<br></br>**Bericht**: replicatie kan niet worden ingeschakeld voor de virtuele machine VmName. | -Uw abonnements-ID kan niet worden ingeschakeld om te maken van virtuele machines op de doellocatie voor de regio.</br></br>-Uw abonnements-ID kan niet worden ingeschakeld of beschikt niet over voldoende quotum voor het maken van specifieke VM-grootten op de doellocatie voor de regio.</br></br>-Een geschikt doel-VM-grootte die overeenkomt met de bron-VM NIC aantal (2) is niet gevonden voor de abonnements-ID op de doellocatie voor de regio.| Neem contact op met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om te kunnen maken van de virtuele machine voor de vereiste VM-grootten op de doellocatie voor uw abonnement. Nadat deze ingeschakeld, moet u de mislukte bewerking opnieuw proberen.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
U kunt contact opnemen met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om in te schakelen van uw abonnement te maken van virtuele machines van de vereiste grootte in de doellocatie.

Als de doellocatie een beperking van capaciteit heeft, replicatie uitschakelen en inschakelen naar een andere locatie waar uw abonnement voldoende quotum voor het maken van virtuele machines van de vereiste grootte heeft.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrouwde basiscertificaten (foutcode 151066)

Als de meest recente vertrouwde basiscertificaten niet aanwezig op de virtuele machine, mislukken uw taak 'replicatie inschakelen'. Zonder de certificaten mislukt de verificatie en autorisatie van Site Recovery-serviceaanroepen van de virtuele machine. Het foutbericht voor de taak is mislukt 'replicatie inschakelen' Site Recovery wordt weergegeven:

**Foutcode** | **Mogelijke oorzaak** | **Aanbevelingen**
--- | --- | ---
151066<br></br>**Bericht**: Site Recovery-configuratie is mislukt. | De vereiste vertrouwde basiscertificaten gebruikt voor autorisatie en verificatie zijn niet aanwezig op de machine. | -Voor een virtuele machine met het Windows-besturingssysteem, zorg ervoor dat de vertrouwde basiscertificaten aanwezig zijn op de machine. Zie voor meer informatie, [configureren van vertrouwde basiscertificaten en niet-toegestane certificaten](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Voor een virtuele machine met het Linux-besturingssysteem, volg de richtlijnen voor vertrouwde basiscertificaten gepubliceerd door de distributeur versie van Linux-besturingssysteem.

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

3. Controleer of de basis-CA-certificaat van Symantec aanwezig is.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Als de basis-CA-certificaat van Symantec niet wordt gevonden, voer de volgende opdracht om het bestand te downloaden. Controleer of er fouten en voer de aanbevolen actie voor netwerkfouten.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Controleer of het CA-certificaat van het Baltimore-basiscertificaat aanwezig is.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Als het Baltimore basis-CA-certificaat niet wordt gevonden, downloadt u het certificaat.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. Controleer of het certificaat DigiCert_Global_Root_CA aanwezig is.

    ``# ls DigiCert_Global_Root_CA.pem``

8. Als de DigiCert_Global_Root_CA niet wordt gevonden, voer de volgende opdrachten om het certificaat te downloaden.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Voer rehash script voor het bijwerken van het certificaat onderwerp-hashes voor de zojuist gedownloade certificaten.

    ``# c_rehash``

10. Controleer als onderwerp van de hashes als symlinks voor de certificaten die zijn gemaakt.

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


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Uitgaande connectiviteit voor Site Recovery-URL's of IP-adresbereiken (foutcode 151037 of 151072)

Voor Site Recovery-replicatie met werk, uitgaande connectiviteit voor bepaalde URL's of IP-bereiken zijn van de virtuele machine. Als uw VM zich achter een firewall bevindt of regels voor network security group (NSG) gebruikt voor het beheren van uitgaande connectiviteit, ziet u mogelijk een van deze foutberichten weergegeven:

**Foutcodes** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
151037<br></br>**Bericht**: registratie van virtuele machine van Azure met Site Recovery is mislukt. | -U NSG voor het beheren van uitgaande toegang op de virtuele machine en de vereiste IP-bereiken worden niet opgenomen in de whitelist voor uitgaande toegang.</br></br>-U gebruikt firewallhulpprogramma's van derden en de vereiste IP-bereiken/URL's zijn niet opgenomen in de whitelist.</br>| -Als u firewallproxy gebruikt voor het beheren van uitgaande netwerkverbindingen op de virtuele machine, zorg ervoor dat de vereiste URL's of IP-datacenterbereiken in de whitelist opgenomen. Zie voor meer informatie, [firewall-proxy richtlijnen](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-Als u NSG-regels voor het beheren van uitgaande netwerkverbindingen op de virtuele machine gebruikt, zorg ervoor dat de vereiste datacenter-IP-adresbereiken in de whitelist opgenomen. Zie voor meer informatie, [network security group richtlijnen](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Bericht**: Site Recovery-configuratie is mislukt. | Kan geen verbinding maken met Site Recovery service-eindpunten. | -Als u firewallproxy gebruikt voor het beheren van uitgaande netwerkverbindingen op de virtuele machine, zorg ervoor dat de vereiste URL's of IP-datacenterbereiken in de whitelist opgenomen. Zie voor meer informatie, [firewall-proxy richtlijnen](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-Als u NSG-regels voor het beheren van uitgaande netwerkverbindingen op de virtuele machine gebruikt, zorg ervoor dat de vereiste datacenter-IP-adresbereiken in de whitelist opgenomen. Zie voor meer informatie, [network security group richtlijnen](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
Aan lijst met geaccepteerde [de vereiste URL's](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) of de [vereiste IP-adresbereiken](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), volg de stappen in de [netwerken document met richtlijnen](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Schijf niet gevonden in de machine (foutcode 150039)

Een nieuwe schijf die is gekoppeld aan de virtuele machine moet worden geïnitialiseerd.

**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
150039<br></br>**Bericht**: Azure-gegevensschijf (DiskName) (DiskURI) met logische unit number (LUN) (LUNValue) is niet toegewezen aan een overeenkomende schijf die wordt doorgegeven vanuit de virtuele machine met dezelfde LUN-waarde. | -Een nieuwe gegevensschijf aan de VM is gekoppeld, maar deze is niet geïnitialiseerd.</br></br>-De gegevensschijf binnen de virtuele machine rapporteert de LUN-waarde waarop de schijf is gekoppeld aan de virtuele machine niet correct.| Zorg ervoor dat de gegevensschijven zijn geïnitialiseerd en voer de bewerking vervolgens opnieuw uit.</br></br>Voor Windows: [koppelen en een nieuwe schijf initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Voor Linux: [initialiseren van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Het probleem wordt opgelost
Zorg ervoor dat de gegevensschijven zijn geïnitialiseerd en voer de bewerking vervolgens opnieuw uit:

- Voor Windows: [koppelen en een nieuwe schijf initialiseren](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Voor Linux: [toevoegen van een nieuwe gegevensschijf in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Kan niet zien van de Azure-VM voor selectie in 'replicatie inschakelen'

 **1 oorzaak: Resourcegroep en de virtuele bronmachine zijn in een andere locatie** <br>
Azure Site Recovery voorgeschreven momenteel dat bronresourcegroep regio en virtuele machines moeten zich in dezelfde locatie. Als dat niet het geval is klikt zou u niet kunnen vinden van de virtuele machine tijdens het ophalen van beveiliging.

**2 oorzaak: Resourcegroep maakt geen deel uit van het geselecteerde abonnement** <br>
U kunt mogelijk niet vinden van de resourcegroep op het moment van beveiliging, als deze geen deel uitmaakt van het betreffende abonnement. Zorg ervoor dat de resourcegroep deel uitmaakt van het abonnement dat wordt gebruikt.

 **3 oorzaak: De verouderde configuratie** <br>
Als u de virtuele machine die u wilt inschakelen voor de replicatie niet ziet, wordt mogelijk vanwege een verouderde configuratie van Site Recovery blijven op de virtuele machine van Azure. De verouderde configuratie kan worden blijven op een Azure-VM in de volgende gevallen:

- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de Site Recovery-kluis wordt verwijderd zonder expliciet uitschakelen van replicatie op de virtuele machine.
- U replicatie voor de Azure-VM ingeschakeld met behulp van Site Recovery en vervolgens de resourcegroep die de Site Recovery-kluis bevat zonder expliciet uitschakelen van replicatie op de virtuele machine wordt verwijderd.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

U kunt [verwijderen van verouderde ASR-configuratiescript](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) en verwijdert de verouderde Site Recovery-configuratie op de virtuele machine van Azure. U zou het mogelijk om te zien van de virtuele machine na het verwijderen van de verouderde configuratie.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Kan geen virtuele machine voor beveiliging selecteert 
 **1 oorzaak: de virtuele machine heeft een bepaalde extensie geïnstalleerd in de status van een mislukte of reageert niet** <br>
 Ga naar virtuele machines > instelling > extensies en controleer of er geen extensies in een foutstatus zijn. Verwijder de extensie is mislukt en probeer het opnieuw beveiligen van de virtuele machine.<br>
 **2 oorzaak: [VM de Inrichtingsstatus is niet geldig](#vms-provisioning-state-is-not-valid-error-code-150019)**

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



## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ fout van de Volume Shadow Copy service (foutcode 151025)
**Foutcode** | **Mogelijke oorzaken** | **Aanbevelingen**
--- | --- | ---
151025<br></br>**Bericht**: kan de extensie voor Site recovery niet installeren | -, COM + System Application' service uitgeschakeld.</br></br>-'Volume Shadow Copy-service is uitgeschakeld.| 'COM + System Application' en 'Volume Shadow Copy' services ingesteld op automatische of handmatige opstartmodus.

### <a name="fix-the-problem"></a>Het probleem wordt opgelost

U kunt 'Services'-console opent en zorg ervoor dat de ' COM + System Application' en 'Volume Shadow Copy' zijn niet ingesteld op 'Uitgeschakeld' voor 'Opstarttype'.
  ![COM-fout](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>Volgende stappen
[Virtuele Azure-machines repliceren](site-recovery-replicate-azure-to-azure.md)
