---
title: Voorbereiden van de DPM-server back-up workloads naar Azure
description: Een inleiding tot de back-ups van DPM-gegevens naar een Azure Recovery Services-kluis.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: f119d128b35b93d7e18d514c09d187689d8dffe9
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57306898"
---
# <a name="prepare-to-back-up-workloads-to-azure-with-system-center-dpm"></a>Voorbereiden op back-up van workloads naar Azure met System Center DPM

In dit artikel wordt uitgelegd hoe u voorbereiden voor System Center Data Protection Manager (DPM) back-ups naar Azure met behulp van de Azure Backup-service.

Dit artikel bevat:

- Een overzicht van het implementeren van DPM met Azure Backup.
- Vereisten en beperkingen voor het gebruik van Azure Backup met DPM.
- Stappen voor het voorbereiden van Azure, inclusief het instellen van een back-up van de Recovery Services-kluis en (optioneel) wijzigen van het type van de Azure storage voor de kluis.
- Stappen voor het voorbereiden van de DPM-server, inclusief downloaden kluis referenties, de Azure backup-agent installeren en registreren van de DPM-server in de kluis.
- Tips voor probleemoplossing voor algemene fouten.


## <a name="why-back-up-dpm-to-azure"></a>Waarom back-up van DPM naar Azure?

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) back-ups van gegevens van bestands- en toepassingsgegevens. DPM communiceert met Azure Backup als volgt:

* **DPM wordt uitgevoerd op een fysieke server of de on-premises virtuele machine** — u kunt back-up van gegevens naar een back-upkluis in Azure, ook naar schijf en tape back-up.
* **DPM wordt uitgevoerd op een Azure VM** : van System Center 2012 R2 met Update 3 of hoger, kunt u DPM op een Azure-VM implementeren. U kunt back-ups naar Azure-schijven die zijn gekoppeld aan de virtuele machine of back-up van de gegevens naar een back-upkluis met back-up van Azure.

De zakelijke voordelen van het back-ups van DPM-servers naar Azure zijn:

* Voor on-premises DPM biedt Azure Backup een alternatief voor de implementatie van op de lange termijn op tape.
* Voor DPM uit te voeren op een Azure VM, kunt Azure Backup u voor de offload van de Azure disk-opslag. Opslaan van oudere gegevens in een Backup-kluis kunt u uw bedrijf kan worden uitgebreid door nieuwe gegevens naar schijf te slaan.

## <a name="prerequisites-and-limitations"></a>Vereisten en beperkingen

**Instelling** | **Vereiste**
--- | ---
DPM op een Azure-VM | System Center 2012 R2 met DPM 2012 R2 updatepakket 3 of hoger.
DPM op een fysieke server | System Center 2012 SP1 of hoger. System Center 2012 R2.
DPM op een Hyper-V-VM | System Center 2012 SP1 of hoger. System Center 2012 R2.
DPM op een VMware-VM | System Center 2012 R2 met Update Rollup 5 of hoger.
Onderdelen | De DPM-server moet Windows PowerShell en .NET Framework 4.5 geïnstalleerd hebben.
Ondersteunde apps | [Ontdek](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix) waar DPM een back-up van kan maken.
Ondersteunde bestandstypen | Deze bestandstypen kunnen een back-worden gemaakt met Azure Backup: Versleuteld (volledige back-ups alleen); Gecomprimeerd (incrementele back-ups ondersteund). Sparse (incrementele back-ups ondersteund); Gecomprimeerd en sparse (behandeld als sparse).
Niet-ondersteunde bestandstypen | Servers op hoofdlettergevoelige bestandssystemen; vaste koppelingen (overgeslagen); reparsepunten (overgeslagen); versleuteld en gecomprimeerd (overgeslagen); versleuteld en sparse (overgeslagen); Gecomprimeerde stream; stream parseren.
Lokale opslag | Elke machine die u back wilt-up moet lokale vrije opslagruimte die ten minste 5% van de grootte van de gegevens die back-up hebben. Bijvoorbeeld, vereist back-ups van 100 GB aan gegevens een minimum van 5 GB aan vrije ruimte in de nieuwe locatie.
Kluis-opslag | Er is geen limiet voor de hoeveelheid gegevens die u kunt back-up naar een Azure Backup-kluis, maar de grootte van een gegevensbron (bijvoorbeeld een virtuele machine of een database) mag niet groter zijn dan 54400 GB.
Azure Backup-agent | Als DPM wordt uitgevoerd op System Center 2012 SP1, installeert u updatepakket 2 of hoger voor DPM SP1. Dit is vereist voor de installatie van agent.<br/><br/> In dit artikel wordt beschreven hoe u de nieuwste versie van de Azure Backup-agent, ook wel bekend als de Microsoft Azure Recovery Service agent (MARS) implementeren. Hebt u een eerdere versie is geïmplementeerd, kunt u bijwerken naar de nieuwste versie om ervoor te zorgen dat back-up werkt zoals verwacht.

Voordat u begint, moet u een Azure-account met de Azure Backup-functie is ingeschakeld. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Meer informatie over [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-settings"></a>Opslaginstellingen wijzigen

U kunt kiezen tussen geografisch redundante opslag en lokaal redundante opslag.

- Uw kluis heeft standaard geografisch redundante opslag.
- Als de kluis uw primaire back-up is, laat u de optie is ingesteld op geografisch redundante opslag. Als u een goedkopere optie die niet erg als duurzame wilt, gebruikt u de volgende procedure om lokaal redundante opslag te configureren.
- Meer informatie over [Azure storage](../storage/common/storage-redundancy.md), en de [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante](../storage/common/storage-redundancy-lrs.md) opslagopties.
- Wijzigen van instellingen voor de opslag voor de eerste back-up. Als u al een item gebackupt hebt, stopt u de back-up in de kluis voordat u Opslaginstellingen wijzigen.

De instelling voor opslagreplicatie bewerken:

1. Open het dashboard van de kluis.

2. In **beheren**, klikt u op **back-upinfrastructuur**.

3. In **back-upconfiguratie** in het menu een opslag-optie voor de kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

## <a name="download-vault-credentials"></a>Kluisreferenties downloaden

U kluisreferenties gebruiken wanneer u de DPM-server in de kluis registreert.

- Het kluisreferentiebestand is een certificaat dat de portal voor elke back-upkluis genereert.
- De portal uploadt de openbare sleutel vervolgens naar de Access Control Service (ACS).
- Tijdens de registratiewerkstroom machine wordt de persoonlijke sleutel van het certificaat beschikbaar gesteld aan de gebruiker, de machine verifieert.
- De Azure Backup-service verzendt op basis van de verificatie, gegevens naar de geïdentificeerde kluis.

### <a name="best-practices-for-vault-credentials"></a>Aanbevolen procedures voor kluisreferenties

Voor de referenties kunt u het kluisreferentiebestand via een beveiligd kanaal downloaden vanuit de Azure-portal:

- De kluisreferenties worden gebruikt alleen tijdens de registratiewerkstroom.
- Het is uw verantwoordelijkheid om ervoor te zorgen dat het bestand met kluisreferenties veilig en niet waarmee is geknoeid is.
    - Als de controle van de referenties verloren is gegaan, kunnen de kluisreferenties worden gebruikt om andere machines naar de kluis te registreren.
    - Echter worden back-upgegevens versleuteld met een wachtwoordzin die deel uitmaakt van de klant, zodat bestaande back-upgegevens kunnen niet worden aangetast.
- Zorg ervoor dat bestand wordt opgeslagen in een locatie die toegankelijk is vanaf de DPM-server. Als deze is opgeslagen in een bestand share/SMB, controleert de toegangsmachtigingen.
- De kluisreferenties verlopen na 48 uur. U kunt download nieuwe kluisreferenties zo vaak als nodig is. Alleen de meest recente kluisreferentiebestand kan echter worden gebruikt tijdens de registratiewerkstroom.
- De Azure Backup-service niet op de hoogte van de persoonlijke sleutel van het certificaat en de persoonlijke sleutel niet beschikbaar is in de portal of de service.

Download het bestand met kluisreferenties als volgt naar een lokale computer:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Open de kluis waarin u wilt registreren van de DPM-server.
3. In **instellingen**, klikt u op **eigenschappen**.

    ![Het menu Kluis openen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. In **eigenschappen** > **back-up referenties**, klikt u op **downloaden**. De portal het kluisreferentiebestand is een combinatie van de huidige datum en de naam van de kluis genereert, en maakt deze beschikbaar voor downloaden.

    ![Downloaden](./media/backup-azure-dpm-introduction/vault-credentials.png)

5. Klik op **opslaan** de kluisreferenties downloaden naar map, of **OpslaanAls** en geef een locatie. Het duurt om een minuut voor het bestand moet worden gegenereerd.


## <a name="install-the-backup-agent"></a>De Backup-Agent installeren

Elke machine die wordt ondersteund door Azure Backup moet de backup-agent (ook wel bekend als de agent van Microsoft Azure Recovery Service (MARS)) is geïnstalleerd. Installeer de agent op de DPM-server als volgt:

1. Open de kluis die u wilt registreren van de DPM-server.
2. In **instellingen**, klikt u op **eigenschappen**.

    ![Het menu Kluis openen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Op de **eigenschappen** pagina, de Azure Backup Agent downloaden.

    ![Downloaden](./media/backup-azure-dpm-introduction/azure-backup-agent.png)


4. Nadat u hebt gedownload, MARSAgentInstaller.exe worden uitgevoerd. de agent installeren op de DPM-machine.
5. Selecteer een map voor installatie en de cachemap voor de agent. De vrije ruimte voor cache-locatie moet ten minste 5% van de back-upgegevens.
6. Als u een proxyserver gebruiken om te verbinden met het internet, in de **proxyconfiguratie** scherm, voert u de gegevens van de proxy-server. Als u gebruikmaakt van een geverifieerde proxyserver, voert u de gebruikersgegevens voor de naam en het wachtwoord in dit scherm.
7. De Azure backup-agent installeert .NET Framework 4.5 en Windows PowerShell (als deze zijn niet geïnstalleerd) om de installatie te voltooien.
8. Nadat de agent is geïnstalleerd, **sluiten** het venster.

    ![Sluiten](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

## <a name="register-the-dpm-server-in-the-vault"></a>De DPM-server in de kluis registreren

1. In de DPM Administrator-console > **Management**, klikt u op **Online**. Selecteer **Registreren**. De Wizard Server registreren wordt geopend.
2. In **proxyconfiguratie**, geef de proxy-instellingen waar nodig.

    ![Proxyconfiguratie](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. In **Backup-kluis**, blader naar en selecteer het bestand met kluisreferenties die u hebt gedownload.

    ![Kluisreferenties](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

10. In **Beperkingsinstelling**, optioneel kan worden ingeschakeld voor bandbreedteregeling voor back-ups. U kunt de snelheid limieten instellen voor werktijden en dagen opgeven.

    ![Beperkingsinstelling](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

11. In **instelling van de map herstel**, Geef een locatie die kan worden gebruikt tijdens het gegevensherstel.

    - Azure Backup gebruikt deze locatie als een tijdelijke wachtruimte voor de herstelde gegevens.
    - Azure Backup wordt na het voltooien gegevensherstel opschonen van de gegevens op dit gebied.
    - De locatie moet voldoende ruimte voor het opslaan van items die u verwacht te herstellen parallel hebben.

    ![Instelling van de map herstel](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

12. In **versleutelingsinstelling** genereren of geef een wachtwoordzin.

    - De wachtwoordzin die wordt gebruikt voor het versleutelen van de back-ups naar de cloud.
    - Geef ten minste 16 tekens.
    - Sla het bestand op een veilige locatie, het nodig voor herstel.

    ![Versleuteling](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > De wachtwoordzin voor versleuteling van jou en Microsoft biedt geen inzicht hebben in deze.
    > Als de wachtwoordzin kwijtraakt of vergeet; Microsoft kan niet helpen bij het herstellen van de back-upgegevens.

13. Klik op **registreren** de DPM-server naar de kluis registreren.

Nadat de server is geregistreerd, is naar bent de kluis en u nu klaar om te starten van back-ups op Microsoft Azure.

## <a name="troubleshoot-vault-credentials"></a>Kluisreferenties oplossen

### <a name="expiration-error"></a>Vervaldatum fout

Het bestand met kluisreferenties is alleen geldig voor 48 uur (nadat deze gedownload vanuit de portal). Als er een fout in dit scherm (bijvoorbeeld ' kluisreferenties opgegeven bestand is verlopen'), meld u aan bij Azure portal en download die de kluisreferenties bestand opnieuw uit.

### <a name="access-error"></a>Fout bij toegang

Zorg ervoor dat het bestand met kluisreferenties is beschikbaar in een locatie die toegankelijk is voor het installatieprogramma. Als u toegang tot gerelateerde fouten, het bestand met kluisreferenties kopiëren naar een tijdelijke locatie op deze machine en probeer het opnieuw.

### <a name="invalid-credentials-error"></a>Fout ongeldige referenties

Als er een fout bij de ongeldige kluis referentie (bijvoorbeeld ' Ongeldige kluisreferenties opgegeven") het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan de recovery-service.

- Probeer het opnieuw nadat u hebt gedownload van een nieuw kluisreferentiebestand via de portal.
- Deze fout is doorgaans te zien als u klikt op de **kluisreferentie downloaden** optie in de Azure portal, twee keer snel achter elkaar. In dit geval wordt is alleen het tweede kluisreferentiebestand geldig.
