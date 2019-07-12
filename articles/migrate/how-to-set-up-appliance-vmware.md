---
title: Instellen van een apparaat voor Azure Migrate evaluatie/servermigratie voor virtuele VMware-machines | Microsoft Docs
description: Beschrijft hoe u voor het instellen van een apparaat voor detectie, beoordeling en zonder agents migratie van virtuele VMware-machines met behulp van Azure Migrate evaluatie/servermigratie.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/08/2019
ms.author: raynew
ms.openlocfilehash: fe190381df346278e75a3e6fd9876b80c33bd86b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811724"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>Een apparaat instellen voor VMware-VM 's

Dit artikel wordt beschreven hoe u het apparaat Azure Migrate instelt als u beoordeling van de virtuele VMware-machines met het hulpprogramma Azure Migrate-Server-evaluatie, of u virtuele VMware-machines naar Azure migreert met de zonder agent migratie met behulp van het hulpprogramma voor migratie van Azure-Server migreren.

De VMware-VM-apparaat is een lichtgewicht apparaat die wordt gebruikt door Azure Migrate evaluatie/servermigratie het volgende doen:

- On-premises VMware-VM's detecteren.
- Metagegevens-en prestatiegegevens voor gedetecteerde virtuele machines naar Azure migreren evaluatie/servermigratie verzenden.

[Meer informatie](migrate-appliance.md) over het apparaat Azure Migrate.


## <a name="appliance-deployment-steps"></a>Stappen voor de implementatie van apparaat

Instellen van het apparaat u:
- Een sjabloon voor OVA-bestanden downloaden en importeren naar de vCenter-Server.
- Maken van het apparaat en controleer dat deze verbinding met Azure Migrate-Server-evaluatie maken kan. 
- Het apparaat voor het eerst configureren en registreren bij de Azure Migrate-project.

## <a name="download-the-ova-template"></a>Het OVA-sjabloon downloaden

1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Server-evaluatie**, klikt u op **ontdekken**.
2. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , klikt u op **Ja, met VMWare vSphere-hypervisor**.
3. Klik op **downloaden** voor het downloaden van de. Sjabloon voor OVA-bestanden.



### <a name="verify-security"></a>Beveiliging controleren

Controleer of het OVA-bestand beveiligd is voordat u deze implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Voer de volgende opdracht, voor het genereren van de hash voor het ova-bestand:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Voor versie 1.0.0.5 van het toestel, de gegenereerde hash moet overeenkomen met deze instellingen. 

  **Algoritme** | **Hash-waarde**
  --- | ---
  MD5 | ddfdf21c64af02a222ed517ce300c977


## <a name="create-the-appliance-vm"></a>De VM-toepassing maken

Importeer het gedownloade bestand en een virtuele machine maken.

1. Klik in de Client vSphere-console op **Bestand** > **OVF-sjabloon implementeren**.
2. In de Wizard OVF-sjabloon implementeren > **bron**, geef de locatie van het OVA-bestand.
3. In **naam** en **locatie**, Geef een beschrijvende naam voor de virtuele machine. Selecteer de inventaris-object waarin de virtuele machine wordt gehost.
5. In **Host/Cluster**, geeft u de host of cluster op waarmee de virtuele machine wordt uitgevoerd.
6. In **opslag**, geef de opslaglocatie voor de virtuele machine.
7. Geef in **Schijfindeling** het schijftype en de schijfgrootte op.
8. In **netwerktoewijzing**, waarmee de virtuele machine verbinding maken via het netwerk opgeven. Het netwerk moet verbinding met internet, metagegevens verzenden naar Azure Migrate-Server-evaluatie.
9. Controleer en bevestig de instellingen en klik op **Voltooien**.


### <a name="verify-appliance-access-to-azure"></a>Apparaat toegang tot Azure controleren

Zorg ervoor dat het apparaat VM verbinding met maken kan [Azure-URL's](migrate-support-matrix-vmware.md#assessment-url-access-requirements).


## <a name="configure-the-appliance"></a>Configureer het apparaat

Het apparaat voor de eerste keer instellen.

1. Klik in de vSphere Client-console met de rechtermuisknop op de VM > **Console openen**.
2. Geef de taal, de tijdzone en het wachtwoord voor het apparaat.
3. Open een browser op elke computer die u kunt verbinding maken met de virtuele machine en open de URL van de web-app van het apparaat: **https://*apparaatnaam of IP-adres*: 44368**.

   U kunt ook de app openen vanaf het bureaublad apparaat door te klikken op de snelkoppeling naar de app.
4. In de web-app > **vereisten instellen**, doet u het volgende:
    - **Licentie**: Accepteer de licentievoorwaarden en lees de informatie van derden.
    - **Connectiviteit**: De app controleert of de virtuele machine toegang heeft tot internet heeft. Als de virtuele machine gebruikmaakt van een proxy:
        - Klik op **Proxy-instellingen**, en geeft u het proxyadres en de luisterende poorten in het formulier http://ProxyIPAddress of http://ProxyFQDN.
        - Geef referenties op als de proxy verificatie nodig heeft.
        - Alleen HTTP-proxy wordt ondersteund.
    - **Tijd synchronisatie**: Tijd is geverifieerd. De tijd op het apparaat moet worden gesynchroniseerd met de tijd voor de detectie van goed te laten werken.
    - **Updates installeren**: Azure Migrate controleert dat de meest recente updates voor apparaat zijn geïnstalleerd.
    - **Installeer VDDK**: Azure Migrate wordt gecontroleerd of de VMWare vSphere virtuele schijf Development Kit (VDDK) is geïnstalleerd.
        - Azure Migrates gebruikt de VDDK voor virtuele machines tijdens de migratie naar Azure repliceren.
        - Download VDDK 6.7 van VMware en pak het gedownloade zip-inhoud naar de opgegeven locatie op het apparaat.

## <a name="register-the-appliance-with-azure-migrate"></a>Het apparaat registreren met Azure Migrate

1. Klik op **aanmelden**. Als deze niet wordt weergegeven, zorg er dan voor dat u uitgeschakeld hebt met het pop-upblokkering in de browser.
2. Meld u aan met uw Azure-referenties op het nieuwe tabblad. 
    - Meld u aan met uw gebruikersnaam en wachtwoord.
    - Meld u aan met een PINCODE wordt niet ondersteund.
3. Wanneer is aangemeld, gaat u terug naar de web-app.
2. Selecteer het abonnement waarin de Azure Migrate-project is gemaakt. Selecteer vervolgens het project.
3. Geef een naam voor het apparaat. De naam moet alfanumeriek met 14 tekens of minder.
4. Klik op **registreren**.


## <a name="start-continuous-discovery"></a>Continue detectie starten

Nu verbinding maken tussen het apparaat en vCenter-Server en start de VM-detectie. 

1. In **opgeven van de vCenter-Serverdetails**, geef de FQDN-naam of IP-adres van de vCenter-Server. U kunt laat de standaardpoort, of een aangepaste poort op waarop de vCenter-Server luistert.
2. In **gebruikersnaam** en **wachtwoord**, geef de alleen-lezen accountreferenties die het apparaat wordt gebruikt voor het detecteren van virtuele machines op de vCenter-server. Zorg ervoor dat het account heeft de [vereiste machtigingen voor de detectie van](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions).
3. Klik op **-verbinding valideren** om ervoor te zorgen dat het apparaat verbinding met vCenter-Server maken kan.
4. Nadat de verbinding tot stand is gebracht, klikt u op **opslaan en detectie starten**.


Hiermee start u detectie. Het duurt ongeveer 15 minuten voor metagegevens van de gedetecteerde virtuele machines in de portal worden weergegeven. 


## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor [VMware evaluatie](tutorial-assess-vmware.md) en [zonder agent migratie](tutorial-migrate-vmware.md).
