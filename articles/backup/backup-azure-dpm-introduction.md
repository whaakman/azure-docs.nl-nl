---
title: DPM gebruiken om te back-up van workloads naar Azure
description: Een inleiding tot de back-ups van DPM-gegevens naar een Azure Recovery Services-kluis.
services: backup
author: adigan
manager: nkolli
keywords: System Center Data Protection Manager, data protection manager, dpm back-up
ms.service: backup
ms.topic: conceptual
ms.date: 8/22/2018
ms.author: adigan
ms.openlocfilehash: 2da5b04f56a5746fb77de6bc954bb5971eb4664b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885167"
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Met DPM voorbereiden op het maken van back-ups van workloads in Azure
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

In dit artikel wordt uitgelegd hoe u back-ups System Center Data Protection Manager (DPM) naar Azure gebruikt; inclusief:

* Hoe u back-up van DPM-servergegevens naar Azure
* De vereisten voor het bereiken van een goede back-ervaring
* De typische fouten gevonden en hoe u omgaat met hen
* Ondersteunde scenario's

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel bevat de informatie en procedures voor het herstellen van virtuele machines die zijn geïmplementeerd met behulp van de Resource Manager-model.
>
>

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) back-ups van gegevens van bestands- en toepassingsgegevens. Meer informatie over ondersteunde werkbelastingen vindt [hier](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix). Gegevensback-up naar DPM kan worden opgeslagen op tape op de schijf of een back-up naar Azure met Microsoft Azure Backup. DPM communiceert met Azure Backup als volgt:

* **DPM geïmplementeerd als een fysieke server of on-premises virtuele machine** : DPM geïmplementeerd als een fysieke server of een on-premises Hyper-V virtuele machine back-ups van gegevens naar een Recovery Services-kluis naast de schijf en tape back-up.
* **DPM geïmplementeerd als een virtuele machine van Azure** : van System Center 2012 R2 met Update 3 op, kunt u DPM implementeren op een Azure-machine. Als DPM wordt geïmplementeerd als een virtuele machine van Azure, kunt u back-ups naar Azure-schijven die zijn gekoppeld aan de virtuele machine of offload van de opslag van gegevens met back-ups op een Recovery Services-kluis.

## <a name="why-back-up-dpm-to-azure"></a>Waarom back-up van DPM naar Azure?
De zakelijke voordelen van het back-ups van DPM-servers naar Azure zijn:

* Voor on-premises DPM-implementatie, moet u Azure gebruiken als alternatief voor de implementatie van op de lange termijn op tape.
* Voor het implementeren van DPM op een virtuele machine in Azure, de Azure disk-opslag-offload. Opslaan van oudere gegevens in uw Recovery Services-kluis kunt u uw bedrijf kan worden uitgebreid door nieuwe gegevens naar schijf te slaan.

## <a name="prerequisites"></a>Vereisten
Azure Backup back-up van DPM-gegevens als volgt voorbereiden:

1. **Maak een Recovery Services-kluis** : een kluis maken in Azure portal.
2. **Download de kluisreferenties** : downloaden van de referenties die u kunt de DPM-server registreren bij de Recovery Services-kluis.
3. **Installeer de Azure Backup-Agent** — Installeer de agent op elke DPM-server.
4. **Registreer de server** : de DPM-server registreren bij de Recovery Services-kluis.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="key-definitions"></a>Belangrijke definities
Hier volgen enkele belangrijke definities voor back-up naar Azure voor DPM:

1. **Kluisreferenties** : Kluisreferenties nodig zijn om te verifiëren van de computer voor het verzenden van back-upgegevens naar een geïdentificeerde kluis in de Azure Backup-service. Het kan worden gedownload uit de kluis en is geldig tot 48 uur.
2. **Wachtwoordzin** : wachtwoordzin wordt gebruikt voor het versleutelen van de back-ups naar de cloud. Sla het bestand op een veilige locatie als dit nodig tijdens een herstelbewerking wordt uitgevoerd is.
3. **Beveiligingspincode** — als u hebt ingeschakeld de [beveiligingsinstellingen](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) van de kluis BEVEILIGINGSPINCODE is vereist voor het uitvoeren van kritieke back-upbewerkingen. Deze multi-factor-authentication voegt een extra beveiligingslaag toe. 
4. **Herstelmap** , is de woordgroep die de back-ups van cloud tijdelijk naar cloud herstelbewerkingen zijn gedownload. De grootte moet ongeveer gelijk is aan de grootte van de back-items die u wilt herstellen parallel.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="edit-storage-replication"></a>Opslagreplicatie bewerken

Storage-replicatie kunt u kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Als de kluis uw primaire back-up is, laat u de optie is ingesteld op geografisch redundante opslag. Als u een goedkopere optie die niet erg als duurzame wilt, gebruikt u de volgende procedure om lokaal redundante opslag te configureren. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante](../storage/common/storage-redundancy-lrs.md) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

> [!NOTE] 
> Configureer de storage-replicatie voordat de eerste back-up wordt geactiveerd. Als u besluit de opslagconfiguratie voor de replicatie na de back-ups van het beveiligde item wijzigen, moet u beveiliging voor de kluis stoppen voordat u overschakelt van de opslagconfiguratie.
>  

1. Selecteer uw kluis en open het dashboard van de kluis.

2. In de **beheren** sectie, klikt u op **back-upinfrastructuur**.

3. Op de **back-upconfiguratie** menu, kiest u de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

    Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om de VM aan de kluis te koppelen. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="download-vault-credentials"></a>Kluisreferenties downloaden
Het kluisreferentiebestand is een certificaat dat de portal voor elke back-upkluis genereert. De portal uploadt de openbare sleutel vervolgens naar de Access Control Service (ACS). Tijdens de registratiewerkstroom machine wordt de persoonlijke sleutel van het certificaat beschikbaar gesteld aan de gebruiker, de machine verifieert. De Azure Backup-service verzendt op basis van de verificatie, gegevens naar de geïdentificeerde kluis.

De kluisreferenties worden alleen gebruikt tijdens de registratiewerkstroom. Het is de verantwoordelijkheid van de gebruiker om ervoor te zorgen dat het bestand met kluisreferenties niet worden gecompromitteerd. Als de controle van de referenties verloren is gegaan, kunnen de kluisreferenties worden gebruikt om andere machines naar de kluis te registreren. Echter worden back-upgegevens versleuteld met een wachtwoordzin die deel uitmaakt van de klant, zodat bestaande back-upgegevens kunnen niet worden aangetast. Als u wilt dit probleem verhelpen, de kluisreferenties verlopen na 48 uur. Download nieuwe kluisreferenties zo vaak als nodig is. Alleen de meest recente kluisreferentiebestand kan echter worden gebruikt tijdens de registratiewerkstroom.

Het kluisreferentiebestand is gedownload via een beveiligd kanaal uit de Azure-portal. De Azure Backup-service niet op de hoogte van de persoonlijke sleutel van het certificaat en de persoonlijke sleutel niet beschikbaar is in de portal of de service. Gebruik de volgende stappen uit om te downloaden van het kluisreferentiebestand naar een lokale computer.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Open de Recovery Services-kluis die u wilt registreren bij een DPM-server.

3. Instellingenmenu wordt standaard geopend. Als dit is gesloten, klikt u op **instellingen** op het dashboard van de kluis om het menu te openen. In de **instellingen** menu, klikt u op **eigenschappen**.

    ![Het menu Kluis openen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. Op de pagina eigenschappen onder **back-up referenties** klikt u op **downloaden**. De portal genereert het kluisreferentiebestand wordt beschikbaar gesteld voor downloaden.

    ![Downloaden](./media/backup-azure-dpm-introduction/vault-credentials.png)

De portal genereert een kluisreferentie met behulp van een combinatie van de naam van de kluis en de huidige datum. Klik op **opslaan** de kluisreferenties downloaden naar de map downloads van het lokale account, of selecteer OpslaanAls in het menu opslaan naar een locatie voor de kluisreferenties opgeven. Het duurt om een minuut voor het bestand moet worden gegenereerd.

### <a name="note"></a>Opmerking
* Zorg ervoor dat het bestand met kluisreferenties is opgeslagen in een locatie die toegankelijk is vanaf uw computer. Als deze is opgeslagen in een bestand share/SMB, controleert de toegangsmachtigingen.
* Het bestand met kluisreferenties wordt alleen gebruikt tijdens de registratiewerkstroom.
* Het bestand met kluisreferenties verlopen na 48hrs en kan worden gedownload vanaf de portal.

## <a name="install-backup-agent"></a>Backup-Agent installeren
Nadat de Azure Backup-kluis is gemaakt, moet een agent worden geïnstalleerd op elk van uw Windows-machines (Windows Server, Windows-client, System Center Data Protection Manager-server of Azure Backup Server-machine) die mogelijk maakt een back-up van gegevens en toepassingen naar Azure .

1. Open de Recovery Services-kluis die u wilt registreren van DPM-machine.
2. Instellingenmenu wordt standaard geopend. Als dit is gesloten, klikt u op **instellingen** om het instellingenmenu. Klik in het menu instellingen op **eigenschappen**.

    ![Het menu Kluis openen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Klik op de pagina instellingen **downloaden** onder **Azure Backup Agent**.

    ![Downloaden](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Nadat u de agent hebt gedownload, de MARSAgentInstaller.exe voor het starten van de installatie van de Azure backup-agent worden uitgevoerd. Kies de installatiemap en de scratchmap vereist voor de agent. De opgegeven locatie van de cache moet beschikbare ruimte ten minste 5% van de back-upgegevens hebben.

4. Als u een proxyserver gebruiken om te verbinden met het internet, in de **proxyconfiguratie** scherm, voert u de gegevens van de proxy-server. Als u gebruikmaakt van een geverifieerde proxyserver, voert u de gebruikersgegevens voor de naam en het wachtwoord in dit scherm.

5. De Azure backup-agent installeert u .NET Framework 4.5 en Windows PowerShell (als deze nog niet beschikbaar is) om de installatie te voltooien.

6. Nadat de agent is geïnstalleerd, **sluiten** het venster.

   ![Sluiten](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

7. Naar **de DPM-Server registreren** naar de kluis in de **Management** tabblad, klik op **Online**. Selecteer **registreren**. De Wizard Setup registreren wordt geopend.

8. Als u een proxyserver gebruiken om te verbinden met het internet, in de **proxyconfiguratie** scherm, voert u de gegevens van de proxy-server. Als u gebruikmaakt van een geverifieerde proxyserver, voert u de gebruikersgegevens voor de naam en het wachtwoord in dit scherm.

    ![Proxyconfiguratie](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. In het scherm kluis referenties, blader naar en selecteer het bestand met kluisreferenties die eerder zijn gedownload.

    ![Kluisreferenties](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Het bestand met kluisreferenties is alleen geldig voor 48 uur (nadat deze gedownload vanuit de portal). Als er een fout in dit scherm (bijvoorbeeld ' kluisreferenties opgegeven bestand is verlopen'), meld u aan bij Azure portal en download die de kluisreferenties bestand opnieuw uit.

    Zorg ervoor dat het bestand met kluisreferenties is beschikbaar in een locatie die toegankelijk is voor het installatieprogramma. Als u toegang tot gerelateerde fouten, het bestand met kluisreferenties kopiëren naar een tijdelijke locatie op deze machine en probeer het opnieuw.

    Als er een fout bij de ongeldige kluis referentie (bijvoorbeeld ' Ongeldige kluisreferenties opgegeven") het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan de recovery-service. Probeer het opnieuw nadat u hebt gedownload van een nieuw kluisreferentiebestand via de portal. Deze fout is doorgaans te zien als de gebruiker klikt op de **kluisreferentie downloaden** optie in de Azure-portal, snel achter elkaar. In dit geval wordt is alleen het tweede kluisreferentiebestand geldig.

10. Voor het beheren van het gebruik van de netwerkbandbreedte tijdens het werk, en niet-werkuren, in de **Beperkingsinstelling** scherm, die u kunt de limieten voor bandbreedtegebruik instellen en definiëren van de uur werk- en niet wordt gewerkt.

    ![Beperkingsinstelling](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

11. In de **instelling van de map herstel** scherm, bladeren naar de map waar de bestanden gedownload van Azure tijdelijk tijdelijk opgeslagen.

    ![Instelling van de map herstel](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

12. In de **versleutelingsinstelling** scherm, u kunt een wachtwoordzin gegenereerd of geef een wachtwoordzin (minimaal 16 tekens). Moet u de wachtwoordzin op een veilige locatie opslaan.

    ![Versleuteling](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Als de wachtwoordzin kwijtraakt of vergeet; Microsoft kan niet helpen bij het herstellen van de back-upgegevens. De gebruiker is eigenaar van de wachtwoordzin voor versleuteling en Microsoft biedt geen inzicht hebben in de wachtwoordzin die wordt gebruikt door de gebruiker. Sla het bestand op een veilige locatie als dit nodig tijdens een herstelbewerking wordt uitgevoerd is.
    >
    >

13. Nadat u op de **registreren** knop, de machine naar de kluis is geregistreerd en u bent nu klaar om te starten van back-ups op Microsoft Azure.

14. Wanneer u Data Protection Manager, kunt u de instellingen die zijn opgegeven tijdens de registratiewerkstroom door te klikken op de **configureren** optie hiervoor **Online** onder de **Management**  Tabblad.

## <a name="requirements-and-limitations"></a>Vereisten (en beperkingen)
* DPM kan worden uitgevoerd als een fysieke server of een Hyper-V virtuele machine is geïnstalleerd op de System Center 2012 SP1 of System Center 2012 R2. Kan ook worden uitgevoerd als een Azure virtuele machine die wordt uitgevoerd op System Center 2012 R2 met ten minste DPM 2012 R2 updatepakket 3 of een Windows-machine in VMware die ten minste wordt uitgevoerd op System Center 2012 R2 met Update Rollup 5.
* Als u DPM met System Center 2012 SP1 uitvoert moet u installeren Update Roll up 2 voor System Center Data Protection Manager SP1. Dit is vereist voordat u de Azure Backup Agent kunt installeren.
* De DPM-server moet Windows PowerShell en .net Framework 4.5 geïnstalleerd.
* DPM kan back-up de meeste werkbelastingen op Azure Backup. De back-up van Azure ondersteuning voor een volledige lijst van heeft wat wordt ondersteund, Zie onderstaande items.
* Gegevens die zijn opgeslagen in Azure Backup kan niet worden hersteld met de optie 'kopiëren naar tape'.
* U moet een Azure-account met de Azure Backup-functie is ingeschakeld. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Meer informatie over [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Azure Backup, moet de Azure Backup-Agent moet worden geïnstalleerd op de servers die u back wilt-up. Elke server moet ten minste 5% van de grootte van de gegevens die back-up, als lokale vrije opslagruimte beschikbaar hebben. Bijvoorbeeld, vereist back-ups van 100 GB aan gegevens een minimum van 5 GB aan vrije ruimte in de nieuwe locatie.
* Gegevens worden opgeslagen in de kluis van Azure-opslag. Er is geen limiet voor de hoeveelheid gegevens die u kunt back-up naar een Azure Backup-kluis, maar de grootte van een gegevensbron (bijvoorbeeld een virtuele machine of een database) mag niet groter zijn dan 54400 GB.

Deze bestandstypen worden ondersteund voor back-up naar Azure:

* Versleuteld (volledige back-ups alleen)
* Gecomprimeerd (incrementele back-ups ondersteund)
* Sparse (incrementele back-ups ondersteund)
* Gecomprimeerd en sparse (behandeld als Sparse)

En deze worden niet ondersteund:

* Servers op hoofdlettergevoelige bestandssystemen worden niet ondersteund.
* Vaste koppelingen (overgeslagen)
* Reparsepunten (overgeslagen)
* Versleuteld en gecomprimeerd (overgeslagen)
* Versleuteld en sparse (overgeslagen)
* Gecomprimeerde stream
* Sparse stream

> [!NOTE]
> In System Center 2012 DPM met SP1 en hoger, kunt u back-up van beveiligde werkbelastingen naar Azure.
>
>
