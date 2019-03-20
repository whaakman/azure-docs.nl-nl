---
title: Back-up van virtuele VMware-machines met Azure Backup Server
description: Azure Backup Server gebruiken voor back-up van virtuele VMware-machines die worden uitgevoerd op een VMware vCenter-/ ESXi-server.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: raynew
ms.openlocfilehash: b323836ff804d9e4af3765a2d4ea713a34bcf284
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226518"
---
# <a name="back-up-vmware-vms-with-azure-backup-server"></a>Back-up van virtuele VMware-machines met Azure Backup Server

In dit artikel wordt uitgelegd hoe u back-up van virtuele VMware-machines waarop VMware ESXi-hosts/vCenter-Server naar Azure met Azure Backup Server.

Dit artikel wordt uitgelegd hoe u:

- Een beveiligd kanaal zo instellen dat Azure Backup Server met de VMware-servers via HTTPS communiceren kan.
- Instellen van een VMware-account met Azure Backup Server gebruikt voor toegang tot de VMware-server.
- Referenties van het account toevoegen aan Azure Backup.
- De vCenter of ESXi-server toevoegen aan Azure Backup Server.
- Instellen van een beveiligingsgroep met de VMware-VM's die u wilt back-up, back-instellingen opgeven en de back-up plannen.

## <a name="before-you-start"></a>Voordat u begint
- Controleer of dat u een versie van de vCenter-/ ESXi die wordt ondersteund voor back-up - versie 6.5, 6.0 of 5.5 uitvoert.
- Zorg ervoor dat u Azure Backup Server hebt ingesteld. Als u nog niet hebt, [hiervoor](backup-azure-microsoft-azure-backup.md) voordat u begint. U moet Azure Backup Server uitvoeren met de nieuwste updates.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Maken van een beveiligde verbinding met de vCenter-Server

Standaard communiceert Azure Backup Server via HTTPS met VMware-servers. Als u de HTTPS-verbinding instelt, de VMware certificeringsinstantie (CA)-certificaat downloaden en importeren op de Azure Backup-Server.


### <a name="before-you-start"></a>Voordat u begint

- Als u niet wilt dat HTTPS te gebruiken kunt u [uitschakelen van de standaardinstelling](backup-azure-backup-server-vmware.md).
- U doorgaans verbinding maken vanuit een browser op de Azure Backup Server-machine met de vCenter-/ ESXi-server met behulp van de vSphere-webclient. De eerste keer dat u hiervoor de verbinding is niet beveiligd en ziet u het volgende.
- Het is belangrijk om te begrijpen hoe Azure Backup Server omgaat met back-ups.
    - Als eerste stap Azure Backup Server back-ups van gegevens naar de opslag van de lokale schijf. Azure Backup-Server maakt gebruik van een opslaggroep, een set schijven en volumes waarop Azure Backup Server schijfherstelpunten voor de beveiligde gegevens worden opgeslagen. De opslaggroep kan direct gekoppelde opslag (DAS), een Fibre channel SAN- of iSCSI-opslagapparaat of SAN zijn. Het is belangrijk om ervoor te zorgen dat u voldoende opslagruimte voor de lokale back up van de gegevens van uw VMware-VM hebt.
    - Azure Backup-Server en back-ups maakt van de lokale schijfopslag naar Azure.
    - [Hulp bij het](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1807#figure-out-how-much-storage-space-you-need) om te achterhalen hoeveel opslagruimte u nodig hebt. De informatie is voor DPM, maar ook voor Azure Backup Server kan worden gebruikt.

### <a name="set-up-the-certificate"></a>Het certificaat instellen

Stel een beveiligd kanaal als volgt in:

1. In de browser op Azure Backup Server, voert u de vSphere Client-Web-URL. Als de aanmeldingspagina niet wordt weergegeven, controleert u of de verbinding en browser proxy-instellingen.

    ![vSphere-webclient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

2. Klik op de aanmeldingspagina voor vSphere-webclient **downloaden vertrouwde basis-CA-certificaten**.

    ![Vertrouwde basis-CA-certificaat downloaden](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

3. Een bestand met de naam **downloaden** wordt gedownload. Afhankelijk van uw browser ontvangt u een bericht waarin u wordt gevraagd of u wilt openen of sla het bestand.

    ![CA-certificaat downloaden](./media/backup-azure-backup-server-vmware/download-certs.png)

4. Sla het bestand op de Azure Backup Server-machine met de extensie .zip.

5. Met de rechtermuisknop op **download.zip** > **alle uitpakken**. Het ZIP-bestand haalt de inhoud ervan naar de **certificaten** map:
   - Bestand van het basiscertificaat met een extensie die met een genummerde volgorde, zoals.0 en.1 begint.
   - Het CRL-bestand heeft een extensie die met een reeks zoals .r0 of .r1 begint. Het CRL-bestand is gekoppeld aan een certificaat.

     ![Gedownloade certificaten](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. In de **certificaten** map met de rechtermuisknop op het basiscertificaatbestand > **naam**.

    ![Wijzig de naam van basiscertificaat](./media/backup-azure-backup-server-vmware/rename-cert.png)

6. Wijzigen van het basiscertificaat extensie .crt en bevestigen. Het pictogram van het bestand wordt gewijzigd in een waarde van een basiscertificaat.

7. Met de rechtermuisknop op het basiscertificaat en selecteer in het pop-upmenu **certificaat installeren**.

8. In **Wizard Certificaat importeren**, selecteer **lokale Machine** als bestemming voor het certificaat en klik vervolgens op **volgende**. Controleer of als u wordt gevraagd of u wilt toestaan dat de computer gewijzigd.

    ![Welkomstpagina van wizard](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)


9. Op de **certificaat Store** weergeeft, schakelt **alle certificaten in het onderstaande archief plaatsen**, en klik vervolgens op **Bladeren** kiezen van het certificaatarchief.

     ![Certificaatarchief](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

10. In **Selecteer certificaat Store**, selecteer **Trusted Root Certification Authorities** als de doelmap voor de certificaten en klik vervolgens op **OK**.

    ![Certificaat-doelmap](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

11. In **voltooien van de Wizard Certificaat importeren**, Controleer of de map en klik vervolgens op **voltooien**.

    ![Controleer of het certificaat bevindt zich in de juiste map](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)


12. Nadat het importeren van het certificaat is bevestigd, meld u met de vCenter-Server om te bevestigen dat de verbinding beveiligd is.




### <a name="disable-default-https"></a>Standaard HTTPS uitschakelen

Als u beveiligde grenzen binnen uw organisatie hebben en niet wilt gebruiken van het HTTPS-protocol tussen VMware-servers en de Azure Backup Server-machine, schakelt u HTTPS als volgt: u
1. Kopieer en plak de volgende tekst in een txt-bestand.

      ```
      Windows Registry Editor Version 5.00
      [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
      "IgnoreCertificateValidation"=dword:00000001
      ```

2. Sla het bestand op de Azure Backup Server-machine met de naam van de **DisableSecureAuthentication.reg**.

3. Dubbelklik op het bestand voor het activeren van het register-item.


## <a name="create-a-vmware-role"></a>Een VMware-rol maken

De Azure Backup-Server moet een gebruikersaccount met machtigingen voor toegang tot de Vcenter-Server/ESXi-host. Een VMware-rol maken met specifieke bevoegdheden en vervolgens een account koppelen aan de rol.

1. Aanmelden bij de vCenter-Server (of de ESXi-host als u geen vCenter-Server).
2. In de **Navigator** deelvenster, klikt u op **beheer**.

    ![Beheer](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

3. In **beheer** > **rollen**, klikt u op het pictogram van de rol toevoegen (het symbool +).

    ![Rol toevoegen](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)


4. In **rol maken** > **rolnaam**, voer *BackupAdminRole*. De rolnaam mag wat u maar wilt, maar moet wel voor doel van de rol te herkennen.

5. Selecteer de bevoegdheden, zoals samengevat in de onderstaande tabel en klik vervolgens op **OK**.  De nieuwe rol wordt weergegeven in de lijst van de **rollen** deelvenster.
   - Klik op het pictogram naast het bovenliggende label in op de bovenliggende uitvouwen en weergeven van de onderliggende bevoegdheden.
   - Als u wilt de bevoegdheden VirtualMachine selecteert, moet u verschillende niveaus met ingang van de bovenliggende/onderliggende hiërarchie.
   - U hoeft niet te selecteren van alle onderliggende bevoegdheden binnen een van de bovenliggende bevoegdheden.

     ![Bovenliggende/onderliggende hiërarchie bevoegdheden](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

### <a name="role-permissions"></a>Rolmachtigingen
**6.5/6.0** | **5.5**
--- | ---
Datastore.AllocateSpace | Datastore.AllocateSpace
Global.ManageCustomFields | Global.ManageCustomFields
Global.SetCustomField |
Host.Local.CreateVM | Network.Assign
Network.Assign |
Resource.AssignVMToPool |
VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   
VirtualMachine.Config.AdvancedConfig| VirtualMachine.Config.AdvancedConfig
VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking
VirtualMachine.Config.HostUSBDevice |
VirtualMachine.Config.QueryUnownedFiles |
VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement
VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff
VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create
VirtualMachine.Provisioning.DiskRandomAccess |
VirtualMachine.Provisioning.DiskRandomRead | VirtualMachine.Provisioning.DiskRandomRead
VirtualMachine.State.CreateSnapshot | VirtualMachine.State.CreateSnapshot
VirtualMachine.State.RemoveSnapshot | VirtualMachine.State.RemoveSnapshot




## <a name="create-a-vmware-account"></a>Maak een VMware-account

1. In VMware vCenter Server **Navigator** deelvenster, klikt u op **gebruikers en groepen**. Als u geen vCenter-Server gebruikt, maakt u het account op de juiste ESXi-host.

    ![Optie voor gebruikers en groepen](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    De **vCenter gebruikers en groepen** deelvenster wordt weergegeven.


2. In de **vCenter gebruikers en groepen** Configuratiescherm, selecteer de **gebruikers** tabblad en klik vervolgens op het pictogram van de gebruikers toevoegen (het symbool +).

     ![vCenter-gebruikers en groepen-Configuratiescherm](./media/backup-azure-backup-server-vmware/usersandgroups.png)


3. In **nieuwe gebruiker** dialoogvenster vak, voegt u de gebruikersinformatie > **OK**. De gebruikersnaam is in deze procedure BackupAdmin.

    ![Dialoogvenster Nieuwe gebruiker](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)


4. Het gebruikersaccount dat koppelen aan de rol, in de **Navigator** deelvenster, klikt u op **algemene machtigingen**. In de **algemene machtigingen** Configuratiescherm, selecteer de **beheren** tabblad en klik vervolgens op het pictogram toevoegen (het symbool +).

    ![Algemene machtigingen deelvenster](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)


5. In **globale machtiging Root - machtiging toevoegen**, klikt u op **toevoegen** kiezen van de gebruiker of groep.

    ![Gebruiker of groep kiezen](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

6. In **gebruikers/groepen selecteren**, kiest u **BackupAdmin** > **toevoegen**. In **gebruikers**, wordt de *domein\gebruikersnaam* indeling wordt gebruikt voor het gebruikersaccount. Als u gebruiken een ander domein wilt, kiest u uit de **domein** lijst. Klik op **OK** om toe te voegen van de geselecteerde gebruikers kunnen de **machtiging toevoegen** in het dialoogvenster.

    ![BackupAdmin gebruiker toevoegen](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)


7.  In **toegewezen rol**, selecteer in de vervolgkeuzelijst **BackupAdminRole** > **OK**.

    ![Gebruiker toewijzen aan rol](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)


Op de **beheren** tabblad de **algemene machtigingen** deelvenster, het nieuwe gebruikersaccount en de bijbehorende functieservices worden weergegeven in de lijst.


## <a name="add-the-account-on-azure-backup-server"></a>Het account toevoegen aan Azure Backup Server


1. Open Azure Backup Server. Als u het pictogram niet op het bureaublad vinden, opent u Microsoft Azure Backup uit de lijst met apps.

    ![Azure Backup Server-pictogram](./media/backup-azure-backup-server-vmware/mabs-icon.png)

2. Klik in de Azure Backup Server-console op **Management** >  **productieservers** > **VMware beheren**.

    ![Azure Backup Server-console](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)


3. In de **referenties beheren** in het dialoogvenster, klikt u op **toevoegen**.

    ![Het dialoogvenster Azure Backup Server beheren referenties](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

4. In **referentie toevoegen** , voer een naam en een beschrijving op voor de nieuwe referentie en geef de gebruikersnaam en het wachtwoord die u op de VMware-server hebt gedefinieerd. De naam van de *Contoso Vcenter referentie* wordt gebruikt voor het identificeren van de referentie op die in deze procedure. Als de VMware-server en Azure Backup Server niet in hetzelfde domein, moet u het domein opgeven in de naam van de gebruiker.

    ![In het dialoogvenster van Azure Backup-Server toevoegen referentie](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

5. Klik op **toevoegen** om toe te voegen van de nieuwe referentie.

    ![Het dialoogvenster Azure Backup Server beheren referenties](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)


## <a name="add-the-vcenter-server"></a>De vCenter-Server toevoegen

De vCenter-Server toevoegen aan Azure Backup Server.


1. Klik in de Azure Backup Server-console op **Management** > **productieservers** > **toevoegen**.

    ![Wizard voor het openen productie toevoegen](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)


2. In **productie-Wizard voor het toevoegen van Server** > **productieservertype selecteren** pagina, selecteert u **VMware-Servers**, en klik vervolgens op **volgende**.

     ![Wizard voor het productie toevoegen](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

3. In **Computers selecteren****naam/IP-serveradres**, de FQDN-naam of IP-adres van de VMware-server opgeven. Als alle ESXi-servers worden beheerd door dezelfde vCenter, moet u de vCenter-naam opgeven. Anders wordt de ESXi-host toevoegen.

    ![VMware-server opgeven](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. In **SSL-poort**, voer de poort die wordt gebruikt om te communiceren met de VMware-server. 443 is de standaardpoort, maar u kunt dit wijzigen als de VMware-server op een andere poort luistert.

5. In **referentie opgeven**, selecteert u de referentie op die u eerder hebt gemaakt.

    ![Referenties opgeven](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Klik op **toevoegen** de VMware-server toevoegen aan de serverlijst met. Klik op **Volgende**.

    ![VMWare-server en referentie toevoegen](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. In de **samenvatting** pagina, klikt u op **toevoegen** de VMware-server toevoegen aan Azure Backup Server. De nieuwe server wordt direct toegevoegd dat geen agent is vereist op de VMware-server.

    ![VMware-server toevoegen aan Azure Backup Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

8. Controleer de instellingen op de **voltooien** pagina.

   ![Voltooiingspagina](./media/backup-azure-backup-server-vmware/summary-screen.png)

Als u meerdere ESXi-hosts die niet worden beheerd door vCenter-server hebt, of u meerdere exemplaren van de vCenter-Server hebt, moet u de wizard om toe te voegen van de servers opnieuw uit te voeren.




## <a name="configure-a-protection-group"></a>Configureer een beveiligingsgroep

Virtuele VMware-machines toevoegen voor back-up. Beveiligingsgroepen voor het verzamelen van meerdere virtuele machines en het bewaren van dezelfde gegevens en back-upinstellingen toepassen op alle virtuele machines in de groep.


1. Klik in de Azure Backup Server-console op **Protection**, > **nieuw**.

    ![Open de wizard nieuwe beveiligingsgroep maken](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. In de **nieuwe beveiligingsgroep maken** welkomstpagina van de wizard, klikt u op **volgende**.

    ![Dialoogvenster voor wizard nieuwe beveiligingsgroep maken](./media/backup-azure-backup-server-vmware/protection-wizard.png)

1. Op de **Selecteer type beveiligingsgroep** pagina, selecteert u **Servers** en klik vervolgens op **volgende**. De **groepsleden selecteren** pagina wordt weergegeven.

1. In **groepsleden selecteren** > Selecteer de VM's (of mappen van de virtuele machine) die u wilt back-up. Klik op **Volgende**.

    - Wanneer u een map selecteert of virtuele machines of mappen in deze map ook voor back-up zijn geselecteerd. U kunt mappen of virtuele machines die u niet wilt dat back-up uitschakelen.
1. Als een virtuele machine of de map is al back-up, kunt u deze niet selecteren. Dit Zorg ervoor dat er dubbele herstelpunten zijn niet gemaakt voor een virtuele machine. .

     ![Groepsleden selecteren](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)


1. In **methode voor gegevensbeveiliging selecteren** pagina, typ een naam voor de beveiligingsgroep en beveiligingsinstellingen. Terug naar Azure, beveiliging op korte termijn instellen op **schijf** en online beveiliging inschakelt. Klik op **Volgende**.

    ![Methode voor gegevensbeveiliging selecteren](./media/backup-azure-backup-server-vmware/name-protection-group.png)

1. In **Kortetermijndoelen opgeven**, opgeven hoe lang u gegevens wilt bewaren back-ups op schijf.
   - In **bewaartermijn**, geef het aantal dagen schijfherstelpunten moeten worden opgeslagen.
   - In **Synchronisatiefrequentie**, Geef op hoe vaak herstelpunten van de schijf worden gemaakt.
       - Als u niet wilt dat het instellen van een vorige van interval kunt u controleren **net vóór een herstelpunt** zodat een back-up wordt uitgevoerd net voordat elk herstelpunt is gepland.
       - Op korte termijn een back-ups zijn volledige back-ups en geen incrementele.
       - Klik op **wijzigen** wijzigen van de tijden/datums wanneer op korte termijn een back-ups uitgevoerd.

     ![Korte-termijndoelstellingen opgeven](./media/backup-azure-backup-server-vmware/short-term-goals.png)

1. In **Schijftoewijzing controleren**, Controleer de schijfruimte die is opgegeven voor de virtuele machine back-ups. voor de virtuele machines.

   - De aanbevolen schijftoewijzingen zijn gebaseerd op de bewaartermijn die u hebt opgegeven, het type werkbelasting en de grootte van de beveiligde gegevens. Vereiste wijzigingen, en klik vervolgens op **volgende**.
   - **Gegevensgrootte:** De grootte van de gegevens in de beveiligingsgroep.
   - **Schijfruimte:** De aanbevolen hoeveelheid schijfruimte voor de beveiligingsgroep. Als u wilt om deze instelling te wijzigen, kunt u de totale ruimte die groter is dan het bedrag dat u een schatting maken van dat elke gegevensbron groeit moet toewijzen.
   - **Collocatie gegevens:** Als u collocatie inschakelt, worden meerdere gegevensbronnen in de beveiliging kunnen toewijzen aan een enkele replica en herstelpuntvolume. CO-locatie wordt niet ondersteund voor alle werkbelastingen.
   - **Automatisch vergroten:** Als u deze instelling inschakelen en gegevens in de beveiligingsgroep groter worden dan de aanvankelijke toewijzing, wordt Azure Backup Server probeert om de schijfgrootte met 25 procent.
   - **Opslaggroepdetails:** Toont de status van de opslaggroep weer, waaronder de totale en resterende schijfgrootte.

     ![Schijftoewijzing controleren](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

1. In **Replica methode voor maken** pagina, op te geven hoe u wilt de eerste back-up maken en klik vervolgens op **volgende**.
   - De standaardwaarde is **automatisch via het netwerk** en **nu**.
   - Als u de standaardwaarde, raden wij aan dat u een tijdstip buiten de piekuren opgeeft. Kies **Later** en geeft u een dag en tijd.
   - Voor grote hoeveelheden gegevens of minder dan optimale netwerkomstandigheden, kunt u de gegevens offline repliceren met behulp van verwisselbare media.

     ![Methode voor het maken van replica selecteren](./media/backup-azure-backup-server-vmware/replica-creation.png)

1. In **opties voor consistentiecontrole**, selecteer hoe en wanneer de consistentiecontroles automatiseren. Klik op **Volgende**.
      - U kunt consistentiecontroles uitvoeren wanneer de gerepliceerde gegevens inconsistent of volgens een vast schema.
      - Als u niet dat automatische consistentiecontroles configureren wilt, kunt u een handmatige controle uitvoeren. U doet dit door met de rechtermuisknop op de beveiligingsgroep > **consistentiecontrole uitvoeren**.

1. In **gegevens voor Online beveiliging opgeven** pagina, selecteert u de virtuele machines of virtuele machine mappen die u back wilt-up. U kunt de leden afzonderlijk selecteren of klik op **Alles selecteren** kiezen alle leden. Klik op **Volgende**.

      ![Gegevens voor online beveiliging opgeven](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. Op de **Online back-upschema opgeven** pagina, Geef op hoe vaak u wilt back-up van gegevens uit de lokale opslag naar Azure.

    - Cloudherstelpunten voor de gegevens worden gegenereerd op basis van de planning. Klik op **Volgende**.
    - Nadat het herstelpunt dat is gegenereerd, wordt deze overgebracht naar de Recovery Services-kluis in Azure.

      ![Onlineback-upschema opgeven](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. Op de **Online bewaarbeleid opgeven** pagina, geef aan hoe lang u wilt behouden van de herstelpunten die zijn gemaakt op basis van de dagelijkse/wekelijkse/maandelijkse/jaarlijkse back-ups naar Azure. Klik vervolgens op **volgende**.

    - Er is geen beperking voor hoe lang u gegevens in Azure behouden kunt.
    - De enige beperking is dat er geen meer dan 9999 herstelpunten per beveiligd exemplaar. In dit voorbeeld is het beveiligde exemplaar de VMware-server.

      ![Onlinebewaarbeleid opgeven](./media/backup-azure-backup-server-vmware/retention-policy.png)


1. Op de **samenvatting** pagina, Controleer de instellingen en klik vervolgens op **groep maken**.

     ![Een lid van beveiligingsgroep en samenvatting van instelling](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="vmware-vsphere-67"></a>VMWare vSphere 6.7

Back-up vSphere 6.7 doet het volgende:

- TLS 1.2 op DPM-Server in te schakelen
  >[!Note]
  >VMWare 6.7 en hoger had TLS als protocol voor communicatie ingeschakeld.

- De registersleutels instellen, als volgt:  

  Windows-register-Editor versie 5.00

  [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft.NETFramework\v2.0.50727] "SystemDefaultTlsVersions"=dword:00000001 "SchUseStrongCrypto"=dword:00000001

  [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft.NETFramework\v4.0.30319] "SystemDefaultTlsVersions"=dword:00000001 "SchUseStrongCrypto"=dword:00000001

  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft.NETFramework\v2.0.50727] "SystemDefaultTlsVersions"=dword:00000001 "SchUseStrongCrypto"=dword:00000001

  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft.NETFramework\v4.0.30319] "SystemDefaultTlsVersions"=dword:00000001 s"SchUseStrongCrypto"=dword:00000001


## <a name="next-steps"></a>Volgende stappen

Voor het oplossen van problemen bij het instellen van back-ups, raadpleegt u de [probleemoplossingsgids voor Azure Backup Server](./backup-azure-mabs-troubleshoot.md).
