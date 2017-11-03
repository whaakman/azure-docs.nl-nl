---
title: Maak een back-up van VMware-servers met Azure Backup-Server | Microsoft Docs
description: Azure Backup Server gebruiken om back-up van een VMware vCenter/ESXi-servers naar Azure of de schijf. In dit artikel wordt stap voor-stap instructies voor back-ups maken (of beveiligen) = uw VMware-werkbelastingen.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
ms.assetid: 6b131caf-de85-4eba-b8e6-d8a04545cd9d
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: markgal;
ms.openlocfilehash: ad331dffb7c31d12290f4223967c568e4535fe3c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-a-vmware-server-to-azure"></a>Back-up van een VMware-server naar Azure

In dit artikel wordt uitgelegd hoe Azure back-up-Server configureren om VMware server-werkbelasting beveiligen. In dit artikel wordt ervan uitgegaan dat u hebt al Azure Backup-Server is geïnstalleerd. Als u geen Azure Backup-Server is geïnstalleerd, raadpleegt u [voorbereiden op back-up van werkbelastingen met Azure Backup-Server](backup-azure-microsoft-azure-backup.md).

Azure Backup-Server kunnen back-up maken of te beschermen, VMware vCenter-serverversie 6.5, 6.0 en 5.5.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Maken van een beveiligde verbinding met de vCenter-Server

Standaard communiceert Azure Backup-Server met elk vCenter-Server via een HTTPS-kanaal. Als u wilt inschakelen op de beveiligde communicatie, wordt u aangeraden om het certificaat VMware certificeringsinstantie (CA) van de computer te installeren op Azure Backup-Server. Als u geen veilige communicatie vereist en u gebruiken wilt om de vereiste HTTPS te schakelen, Zie [uitschakelen beveiligde communicatieprotocol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). Importeer het vertrouwde certificaat op de Azure Backup-Server voor het maken van een beveiligde verbinding tussen Azure Backup-Server en de vCenter-Server.

Doorgaans kunt u een browser op de machine van Azure Backup-Server verbinding maken met de vCenter-Server via de vSphere webclient. De eerste keer dat u de browser van de Azure Backup-Server gebruiken voor verbinding met de vCenter-Server, de verbinding is niet beveiligd. De volgende afbeelding toont de niet-beveiligde verbinding.

![Voorbeeld van een niet-beveiligde verbinding met de VMware-server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

U kunt dit probleem oplossen en een beveiligde verbinding maken, downloaden de vertrouwde basis-CA-certificaten.

1. Voer de URL naar de vSphere webclient in de browser op Azure Backup-Server. De vSphere webclient-aanmeldingspagina wordt weergegeven.

    ![vSphere webclient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    Zoek aan de onderkant van de informatie voor beheerders en ontwikkelaars de **downloaden vertrouwde basis-CA-certificaten** koppeling.

    ![Koppeling voor het downloaden van de vertrouwde basis-CA-certificaten](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Als u de aanmeldingspagina van vSphere webclient niet ziet, controleert u de proxy-instellingen van uw browser.

2. Klik op **downloaden vertrouwde basis-CA-certificaten**.

    De vCenter-Server downloadt een bestand op uw lokale computer. Naam van het bestand met de naam **downloaden**. Afhankelijk van uw browser ontvangt u een bericht waarin wordt gevraagd of u wilt openen of opslaan van het bestand.

    ![bericht downloaden wanneer certificaten worden gedownload.](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Sla het bestand naar een locatie op de Azure Backup-Server. Wanneer u het bestand opslaat, voeg de extensie .zip.

    Het bestand is een ZIP-bestand met de informatie over certificaten. U kunt de extractie-hulpprogramma's gebruiken met de extensie .zip.

4. Met de rechtermuisknop op **download.zip**, en selecteer vervolgens **Alles uitpakken** om de inhoud te extraheren.

    Het ZIP-bestand extraheert inhoud naar een map met de naam ervan **certificaten**. Twee typen bestanden worden weergegeven in de map certificaten. Het basiscertificaatbestand heeft een extensie die met een genummerde volgorde zoals.0 en.1 begint.
    
    Het CRL-bestand heeft een extensie die met een reeks zoals .r0 of .r1 begint. Het CRL-bestand is gekoppeld aan een certificaat.

    ![Lokaal uitgepakt bestand downloaden ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. In de **certificaten** map met de rechtermuisknop op het bestand voor het basiscertificaat en klik vervolgens op **naam**.

    ![Wijzig de naam van basiscertificaat ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Wijzig het basiscertificaat uitbreiding in .crt. Als u wordt gevraagd als u zeker dat u wilt wijzigen van de extensie, klikt u op **Ja** of **OK**. Anders kunt u de juiste functie van het bestand wijzigen. Het pictogram voor het bestand is gewijzigd in een pictogram dat een basiscertificaat vertegenwoordigt.

6. Met de rechtermuisknop op het basiscertificaat en selecteer in het pop-upmenu **certificaat installeren**.

    De **Wizard Certificaat importeren** dialoogvenster wordt weergegeven.

7. In de **Wizard Certificaat importeren** dialoogvenster, **lokale Machine** als bestemming voor het certificaat en klik vervolgens op **volgende** om door te gaan.

    ![Certificaat bestemming opslagopties ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Als u wordt gevraagd als u toestaan dat de computer gewijzigd wilt, klikt u op **Ja** of **OK**, naar alle wijzigingen.

8. Op de **certificaatarchief** pagina **alle certificaten in het onderstaande archief plaatsen**, en klik vervolgens op **Bladeren** kiezen van het certificaatarchief.

    ![Certificaten in een bepaalde opslag-positie plaatsen](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    De **certificaatarchief selecteren** dialoogvenster wordt weergegeven.

    ![Map van de certificaathiërarchie opslag](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Selecteer **Trusted Root Certification Authorities** als doelmap voor de certificaten en klik vervolgens op **OK**.

    ![Certificaat-doelmap](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    De **Trusted Root Certification Authorities** map wordt bevestigd dat het certificaatarchief. Klik op **Volgende**.

    ![Archiefmap certificaat](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Op de **de wizard Certificaat importeren** pagina, Controleer of het certificaat is in de gewenste map en klik vervolgens op **voltooien**.

    ![Controleer of het certificaat bevindt zich in de juiste map](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    Een dialoogvenster wordt weergegeven, wordt bevestigd dat het importeren van het certificaat is geslaagd.

11. Meld u aan de vCenter-Server om te bevestigen dat de verbinding beveiligd is.

  Als het certificaat importeren niet geslaagd is en u geen beveiligde verbinding kan maken, raadpleegt u de VMware vSphere-documentatie op [servercertificaten](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Als u beveiligde grenzen binnen uw organisatie hebben en niet wilt dat het HTTPS-protocol inschakelen, gebruikt u de volgende procedure uitschakelen van de beveiligde communicatie.

### <a name="disable-secure-communication-protocol"></a>Beveiligde communicatieprotocol uitschakelen

Als uw organisatie kan het HTTPS-protocol niet vereist, gebruik de volgende stappen uit om uit te schakelen van HTTPS. Schakel het standaardgedrag een registersleutel die het standaardgedrag negeert te maken.

1. Kopieer en plak de volgende tekst in een txt-bestand.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Sla het bestand op uw computer Azure Backup-Server. Gebruik voor de bestandsnaam DisableSecureAuthentication.reg.

3. Dubbelklik op het bestand voor het activeren van het register-item.


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a>Een rol- en gebruikersaccount maakt op de vCenter-Server

Op de vCenter-Server is een rol een vooraf gedefinieerde set met rechten. Een beheerder van de vCenter-Server maakt de rollen. De beheerder paren als machtigingen wilt toewijzen, gebruikersaccounts met een rol. Een gebruikersrol maakt met bevoegdheden om vast te stellen de benodigde gebruikersreferenties naar de back-up van de vCenter-Server-computer, en vervolgens het gebruikersaccount wilt koppelen aan de rol.

Azure Backup-Server gebruikt een gebruikersnaam en wachtwoord voor verificatie met de vCenter-Server. Azure Backup-Server gebruikt deze referenties als verificatie voor alle back-upbewerkingen.

Een vCenter-Server-rol en de bevoegdheden voor een back-upadministrator toevoegen:

1. Meld u aan met de vCenter-Server en vervolgens in de vCenter-Server **Navigator** -scherm, klikt u op **beheer**.

    ![Beheeroptie in vCenter Server Navigator Configuratiescherm](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. In **beheer** Selecteer **rollen**, en klik vervolgens in de **rollen** Configuratiescherm klikt u op het pictogram van de rol toevoegen (het symbool +).

    ![Functie toevoegen](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    De **rol maken** dialoogvenster wordt weergegeven.

    ![Rol maken](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. In de **rol maken** het dialoogvenster de **rolnaam** Voer *BackupAdminRole*. De rolnaam kan niet wat u maar wilt, maar deze moet herkenbare voor doel van de rol.

4. Selecteer de bevoegdheden voor de juiste versie van vCenter en klik vervolgens op **OK**. De volgende tabel bevat de vereiste machtigingen voor vCenter 6.0 en vCenter 5.5.

  Wanneer u de bevoegdheden selecteert, klikt u op het pictogram naast het bovenliggende label uitbreiden van de bovenliggende en onderliggende bevoegdheden weergeven. Schakel de bevoegdheden van de virtuele machine, moet u verschillende niveaus in de bovenliggende/onderliggende hiërarchie gaan. U hoeft niet te selecteren van alle onderliggende bevoegdheden binnen een bovenliggende bevoegdheid.

  ![Bovenliggende/onderliggende hiërarchie bevoegdheden](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Nadat u op **OK**, de nieuwe functie in de lijst in het deelvenster functies weergegeven.

|Bevoegdheden voor vCenter 6.0| Bevoegdheden voor vCenter 5.5|
|--------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>Maak een gebruikersaccount van de vCenter-Server en machtigingen

Nadat de rol met bevoegdheden is ingesteld, moet u een gebruikersaccount maken. Het gebruikersaccount heeft een naam en wachtwoord waarmee u de referenties die worden gebruikt voor verificatie.

1. Maken van een gebruikersaccount in de vCenter-Server **Navigator** -scherm, klikt u op **gebruikers en groepen**.

    ![Optie voor gebruikers en groepen](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    De **vCenter gebruikers en groepen** deelvenster wordt weergegeven.

    ![vCenter gebruikers en groepen Configuratiescherm](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. In de **vCenter gebruikers en groepen** Configuratiescherm, selecteer de **gebruikers** tabblad en klik vervolgens op het pictogram van de gebruikers toevoegen (het symbool +).

    De **nieuwe gebruiker** dialoogvenster wordt weergegeven.

3. In de **nieuwe gebruiker** dialoogvenster vak, gegevens van de gebruiker toevoegen en klik vervolgens op **OK**. De gebruikersnaam is in deze procedure BackupAdmin.

    ![Dialoogvenster Nieuwe gebruiker](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    Het nieuwe gebruikersaccount wordt weergegeven in de lijst.

4. Het gebruikersaccount koppelen aan de rol, in de **Navigator** -scherm, klikt u op **algemene machtigingen**. In de **algemene machtigingen** Configuratiescherm, selecteer de **beheren** tabblad en klik vervolgens op het pictogram toevoegen (het symbool +).

    ![Globale machtigingen Configuratiescherm](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    De **globale machtigingen Root - machtiging toevoegen** dialoogvenster wordt weergegeven.

5. In de **globale machtiging Root - machtiging toevoegen** in het dialoogvenster, klikt u op **toevoegen** kiezen van de gebruiker of groep.

    ![Gebruiker of groep kiezen](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    De **gebruikers/groepen selecteren** dialoogvenster wordt weergegeven.

6. In de **gebruikers/groepen selecteren** dialoogvenster Kies **BackupAdmin** en klik vervolgens op **toevoegen**.

    In **gebruikers**, wordt de *domein\gebruikersnaam* indeling wordt gebruikt voor het gebruikersaccount. Als u gebruiken een ander domein wilt, kiest u uit de **domein** lijst.

    ![BackupAdmin gebruiker toevoegen](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Klik op **OK** de geselecteerde gebruikers toevoegen aan de **machtiging toevoegen** in het dialoogvenster.

7. Nu dat u hebt vastgesteld dat de gebruiker, wordt de gebruiker toewijzen aan de rol. In **toegewezen rol**, selecteer in de vervolgkeuzelijst **BackupAdminRole**, en klik vervolgens op **OK**.

    ![Gebruiker toewijzen aan rol](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  Op de **beheren** tabblad de **algemene machtigingen** deelvenster, het nieuwe gebruikersaccount en de bijbehorende functieservices weergegeven in de lijst.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Referenties van de vCenter-Server op Azure Backup-Server tot stand brengen

Installeer voordat u de VMware-server aan Azure Backup-Server toevoegen, [Update 1 voor Azure Backup-Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. Om te openen in Azure Backup-Server, dubbelklikt u op het pictogram op het bureaublad van de Azure Backup-Server.

    ![Pictogram voor Azure Backup-Server](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Als u het pictogram niet op het bureaublad vinden, opent u de Azure Backup-Server in de lijst met geïnstalleerde apps. De naam van de app Azure Backup-Server, Microsoft Azure Backup wordt genoemd.

2. Klik in de console Azure Backup-Server op **Management**, klikt u op **productieservers**, en klik vervolgens op het lint op **beheren VMware**.

    ![Azure Backup-Server-console](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    De **referenties beheren** dialoogvenster wordt weergegeven.

    ![Het dialoogvenster Azure Backup-Server beheren referenties](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. In de **referenties beheren** in het dialoogvenster, klikt u op **toevoegen** openen de **referentie toevoegen** in het dialoogvenster.

4. In de **referentie toevoegen** dialoogvenster Voer een naam en beschrijving voor de nieuwe referentie. Geef vervolgens de gebruikersnaam en wachtwoord. De naam van de *Contoso Vcenter referentie* wordt gebruikt voor het identificeren van de referentie in de volgende procedure. Gebruik dezelfde gebruikersnaam en wachtwoord dat wordt gebruikt voor de vCenter-Server. Als de vCenter-Server en de Azure Backup-Server zich niet in hetzelfde domein in **gebruikersnaam**, geef het domein.

    ![In het dialoogvenster van Azure referentie van back-up-Server toevoegen](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Klik op **toevoegen** nieuwe referentie toevoegen aan Azure Backup-Server. De nieuwe referentie wordt weergegeven in de lijst in de **referenties beheren** in het dialoogvenster.
    
    ![Het dialoogvenster Azure Backup-Server beheren referenties](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Sluiten de **referenties beheren** in het dialoogvenster, klikt u op de **X** in de rechterbovenhoek.


## <a name="add-the-vcenter-server-to-azure-backup-server"></a>De vCenter-Server toevoegen aan Azure Backup-Server

Wizard voor toevoegen van productie-Server wordt gebruikt voor de vCenter-Server toevoegen aan Azure Backup-Server.

Productie-Server toevoegen om Wizard te openen, voert u de volgende procedure:

1. Klik in de console Azure Backup-Server op **Management**, klikt u op **productieservers**, en klik vervolgens op **toevoegen**.

    ![De Wizard openen productie Server toevoegen](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    De **productie Server toevoeging Wizard** dialoogvenster wordt weergegeven.

    ![Wizard voor productie-Server toevoegen](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Op de **productieserver Selecteer type** pagina **VMware Servers**, en klik vervolgens op **volgende**.

3. In **Server naam of het IP-adres**, geef de volledig gekwalificeerde domeinnaam (FQDN) of IP-adres van de VMware-server. Als alle ESXi-servers worden beheerd met dezelfde vCenter, kunt u de vCenter-naam gebruiken.

    ![VMware server FQDN of IP-adres opgeven](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. In **SSL-poort**, geef de poort die wordt gebruikt om te communiceren met de VMware-server. Gebruik van poort 443, de standaardpoort, is tenzij u zeker weet dat een andere poort vereist is.

5. In **Geef referentie**, selecteert u de referenties die u eerder hebt gemaakt.

    ![Geef referenties](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Klik op **toevoegen** VMware server toevoegen aan de lijst met **VMware-Servers toegevoegd**, en klik vervolgens op **volgende** verplaatsen naar de volgende pagina in de wizard.

    ![VMWare-server en referentie toevoegen](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. In de **samenvatting** pagina, klikt u op **toevoegen** de opgegeven VMware-server toevoegen aan Azure Backup-Server.

    ![VMware-server toevoegen aan Azure Backup-Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  De VMware-server back-up is een zonder agent back-up en de nieuwe server onmiddellijk wordt toegevoegd. De **voltooien** pagina ziet u de resultaten.

  ![Voltooiingspagina](./media/backup-azure-backup-server-vmware/summary-screen.png)

  Herhaal de vorige stappen in deze sectie u meerdere exemplaren van de vCenter-Server toevoegen aan Azure Backup-Server door.

Nadat u de vCenter-Server aan Azure Backup-Server toevoegen, wordt de volgende stap is het maken van een beveiligingsgroep. De beveiligingsgroep bevat de verschillende details voor de korte of lange bewaarperiode en waar u definiëren en de back-upbeleid van toepassing is. Het back-upbeleid is de planning voor wanneer de back-ups uitgevoerd en waarvan een back-is gemaakt.


## <a name="configure-a-protection-group"></a>Configureer een beveiligingsgroep

Als u System Center Data Protection Manager of Azure Backup-Server voordat u niet hebt gebruikt, raadpleegt u [plannen voor back-ups van schijf](https://technet.microsoft.com/library/hh758026.aspx) voorbereiden van uw hardware-omgeving. Nadat u hebt gecontroleerd dat u de juiste opslag hebt, moet u de wizard nieuwe beveiligingsgroep maken gebruiken om toe te voegen virtuele VMware-machines.

1. Klik in de console Azure Backup-Server op **beveiliging**, en klik in het lint op **nieuw** om de wizard nieuwe beveiligingsgroep maken te openen.

    ![Open de wizard nieuwe beveiligingsgroep maken](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    De **nieuwe beveiligingsgroep maken** in het dialoogvenster wizard wordt weergegeven.

    ![Dialoogvenster voor wizard nieuwe beveiligingsgroep maken](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Klik op **volgende** om door te gaan naar de **type beveiligingsgroep selecteren** pagina.

2. Op de **Selecteer type beveiligingsgroep** pagina **Servers** en klik vervolgens op **volgende**. De **groepsleden selecteren** pagina wordt weergegeven.

3. Op de **groepsleden selecteren** pagina, de beschikbare leden en de geselecteerde leden worden weergegeven. Selecteer de leden die u wilt beveiligen en klik vervolgens op **volgende**.

    ![Groepsleden selecteren](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Wanneer u een lid, selecteert als u een map met andere mappen of virtuele machines selecteert, worden de mappen en virtuele machines ook geselecteerd. Het opnemen van de mappen en virtuele machines in de bovenliggende map heet mapniveau beveiliging. Schakel het selectievakje voor het verwijderen van een map of een virtuele machine.

    Als een virtuele machine of een map met een VM is al beveiligd naar Azure, niet worden geselecteerd die virtuele machine opnieuw. Dat wil zeggen, nadat een virtuele machine is beveiligd met Azure, worden niet beveiligd, waarmee wordt voorkomen dat dubbele herstelpunten worden gemaakt voor een virtuele machine. Als u zien welk exemplaar van Azure Backup-Server beveiligt al lid is wilt, wijst u het lid om te zien van de naam van de server beschermen.

4. Op de **methode voor gegevensbeveiliging selecteren** pagina, voer een naam voor de beveiligingsgroep. Beveiliging op korte termijn (naar schijf) en online beveiliging zijn geselecteerd. Als u wilt gebruiken van online beveiliging (naar Azure), moet u beveiliging op korte termijn op schijf. Klik op **volgende** om door te gaan naar de korte termijn beveiliging bereik.

    ![Methode voor gegevensbeveiliging selecteren](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Op de **Kortetermijndoelen opgeven** pagina voor **bewaartermijn**, geef het aantal dagen dat u wilt bewaren herstelpunten die zijn *opgeslagen op schijf*. Als u wilt wijzigen van de tijd en dagen wanneer herstelpunten worden gemaakt, klikt u op **wijzigen**. De korte termijn herstelpunten zijn volledige back-ups. Ze zijn geen incrementele back-ups. Wanneer u tevreden met de korte-termijndoelstellingen bent, klikt u op **volgende**.

    ![Korte-termijndoelstellingen opgeven](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Op de **Schijftoewijzing controleren** controleert en wijzig indien nodig de schijfruimte voor de virtuele machines. De aanbevolen schijftoewijzingen zijn gebaseerd op de bewaartermijn die is opgegeven in de **Kortetermijndoelen opgeven** pagina, het type werkbelasting en de grootte van de beveiligde gegevens (aangeduid in stap 3).  

  - **Gegevensgrootte:** grootte van de gegevens in de beveiligingsgroep.
  - **Schijfruimte:** de aanbevolen hoeveelheid schijfruimte voor de beveiligingsgroep. Als u wilt om deze instelling te wijzigen, moet u de totale ruimte die groter is dan het bedrag in dat u een schatting maken van dat elke gegevensbron groeit toewijzen.
  - **Gegevens plaatsen:** als u collocatie inschakelt, meerdere bronnen in de beveiliging aan een enkele replica en herstelpuntvolume toewijzen kunnen gegevenspuntvolume. Collocatie wordt niet ondersteund voor alle werkbelastingen.
  - **Automatisch vergroten:** als u voor deze instelling inschakelt als gegevens in de groep beveiligde groter worden dan de aanvankelijke toewijzing, System Center Data Protection Manager wordt geprobeerd om de schijfgrootte met 25 procent.
  - **Opslaggroepdetails:** toont de status van de opslaggroep, waaronder de totale en resterende schijfgrootte.

    ![Schijftoewijzing controleren](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Wanneer u tevreden bent met de toegewezen ruimte aan, klikt u op **volgende**.

7. Op de **methode voor maken van selecteren Replica** pagina, Geef op hoe u voor het genereren van de eerste kopie, of de replica van de beveiligde gegevens op Azure Backup-Server.

    De standaardwaarde is **automatisch via het netwerk** en **nu**. Als u de standaard gebruikt, raden wij aan dat u opgeeft dat een tijdstip buiten de piekuren. Kies **Later** en geef een dag en tijd.

    Voor grote hoeveelheden gegevens of minder dan optimale netwerkomstandigheden, kunt u de gegevens offline repliceren met behulp van verwijderbare media.

    Nadat u uw keuzes hebt aangebracht, klikt u op **volgende**.

    ![Methode voor het maken van replica selecteren](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Op de **opties voor consistentiecontrole** pagina, selecteer hoe en wanneer de consistentiecontroles automatiseren. Wanneer de gerepliceerde gegevens inconsistent, of volgens een vooropgezet schema, kunt u consistentiecontroles uitvoeren.

    Als u niet voor het configureren van automatische consistentiecontroles wilt, kunt u een handmatige controle uitvoeren. In het gebied van de beveiliging van de back-upserver van Azure-console, met de rechtermuisknop op de beveiligingsgroep en selecteer vervolgens **consistentiecontrole uitvoeren**.

    Klik op **volgende** verplaatsen naar de volgende pagina.

9. Op de **gegevens voor Online beveiliging opgeven** pagina, selecteer een of meer gegevensbronnen die u wilt beveiligen. U kunt de leden afzonderlijk selecteren of op **Alles selecteren** alle leden kiezen. Nadat u ervoor de leden kiest, klikt u op **volgende**.

    ![Gegevens voor online beveiliging opgeven](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Op de **onlineback-upschema opgeven** pagina, geeft u de planning voor het genereren van herstelpunten van de back-up van schijf. Nadat het herstelpunt dat is gegenereerd, wordt deze overgebracht naar de Recovery Services-kluis in Azure. Wanneer u tevreden met het online back-upschema bent, klikt u op **volgende**.

    ![Onlineback-upschema opgeven](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Op de **Online bewaarbeleid opgeven** pagina, geef aan hoe lang u wilt behouden van de back-upgegevens in Azure. Nadat het beleid is gedefinieerd, klikt u op **volgende**.

    ![Onlinebewaarbeleid opgeven](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Er is geen tijdslimiet voor hoe lang van gegevens in Azure bewaren. Wanneer u herstelpuntgegevens in Azure opslaat, is de enige limiet dat er meer dan 9999 herstelpunten per beveiligde exemplaar kan niet. In dit voorbeeld is het beveiligde exemplaar de VMware-server.

12. Op de **samenvatting** pagina, Controleer de details voor de leden van de beveiligingsgroep en de instellingen en klik vervolgens op **groep maken**.

    ![Een lid van beveiligingsgroep en samenvatting van de instelling](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Volgende stappen
Als u Azure Backup-Server gebruikt om VMware workloads te beschermen, hebt u mogelijk geïnteresseerd in beveiligen met Azure Backup-Server een [Microsoft Exchange-server](./backup-azure-exchange-mabs.md), een [Microsoft SharePoint-farm](./backup-azure-backup-sharepoint-mabs.md), of een [SQL Server-database](./backup-azure-sql-mabs.md).

Zie voor informatie over problemen met het registreren van de agent, configureert de beveiligingsgroep of back-ups van taken, [problemen met Azure Backup-Server](./backup-azure-mabs-troubleshoot.md).
