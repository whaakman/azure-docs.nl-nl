---
title: DPM back-up werkbelastingen naar Azure-portal gebruiken | Microsoft Docs
description: Een inleiding tot de back-ups van DPM-servers via de Azure Backup-service
services: backup
documentationcenter: 
author: adigan
manager: nkolli
editor: 
keywords: System Center Data Protection Manager, data protection manager, back-up van dpm
ms.assetid: c8c322cf-f5eb-422c-a34c-04a4801bfec7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: 3422c8d57bdd786ce5d1a41fbb4c12cc4efffddd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Met DPM voorbereiden op het maken van back-ups van workloads in Azure
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup-Server (klassiek)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klassiek)](backup-azure-dpm-introduction-classic.md)
>
>

Dit artikel bevat een inleiding tot Microsoft Azure Backup gebruiken om uw System Center Data Protection Manager (DPM)-servers en werkbelastingen te beveiligen. Deze leest, moet u het volgende weten:

* De werking van Azure DPM-server back-up
* De vereisten voor een goede back-ervaring
* De typische fouten zijn opgetreden en het omgaan met hen
* Ondersteunde scenario 's

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel bevat de informatie en procedures voor het herstellen van virtuele machines die zijn geïmplementeerd met behulp van de Resource Manager-model.
>
>

System Center DPM een back-up bestand-en toepassingsgegevens. Gegevensback-ups naar DPM worden opgeslagen op tape op schijf of een back-up naar Azure met Microsoft Azure Backup. DPM communiceert met Azure Backup als volgt:

* **DPM geïmplementeerd als een fysieke server of on-premises virtuele machine** — als DPM wordt geïmplementeerd als een fysieke server of als een lokale Hyper-V virtuele machine, u kunt back-ups naar een Recovery Services-kluis naast de schijf en tape back-up.
* **DPM geïmplementeerd als een virtuele machine van Azure** : van System Center 2012 R2 met Update 3, kan DPM worden geïmplementeerd als een virtuele machine van Azure. Als DPM wordt geïmplementeerd als een Azure virtuele machine die u kunt back-ups naar Azure-schijven gekoppeld aan de DPM Azure virtuele machine of u de opslag van gegevens kan offloaden door een back-up naar een Recovery Services-kluis.

## <a name="why-backup-from-dpm-to-azure"></a>Waarom back-up van DPM naar Azure?
De zakelijke voordelen van het gebruik van Azure Backup voor back-ups van DPM-servers zijn onder andere:

* Voor on-premises DPM-implementatie, kunt u Azure gebruiken als alternatief voor implementatie van de lange termijn op tape.
* Voor implementaties van DPM in Azure kunt Azure Backup u de schijf van de Azure-opslag-offload zodat u kunt opschalen door oudere gegevens zijn opgeslagen in de Recovery Services-kluis en nieuwe gegevens op schijf.

## <a name="prerequisites"></a>Vereisten
Azure back-up naar back-up van DPM-gegevens als volgt voorbereiden:

1. **Een Recovery Services-kluis maken** : een kluis maken in Azure portal.
2. **Kluisreferenties downloaden** : downloaden van de referenties op die u met de DPM-server naar een Recovery Services-kluis registreren.
3. **Installeer de Azure Backup Agent** : de agent op elke DPM-server installeren en uit Azure Backup.
4. **Registreer de server** : de DPM-server naar een Recovery Services-kluis registreren.

### <a name="1-create-a-recovery-services-vault"></a>1. Een Recovery Services-kluis maken
Een Recovery Services-kluis maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    De lijst met Recovery Services-kluizen wordt weergegeven.
3. Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    De blade Recovery Services-kluis wordt geopend en u wordt gevraagd een **naam**, **abonnement**, **resourcegroep** en **locatie** in te voeren.

    ![Een Recovery Services-kluis maken, stap 5](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)
4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die tussen 2 en 50 tekens bevat. De naam moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.
5. Klik op **Abonnement** om de beschikbare lijst met abonnementen te bekijken. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaard- (of voorgestelde) abonnement. Er zijn alleen meerdere mogelijkheden als uw organisatieaccount is gekoppeld aan meerdere Azure-abonnementen.
6. Klik op **Resourcegroep** om de lijst met beschikbare resourcegroepen te bekijken of klik op **Nieuw** om een nieuwe resourcegroep te maken. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.
7. Klik op **Locatie** om de geografische regio voor de kluis te selecteren.
8. Klik op **Create**. Het kan even duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal.
   Zodra uw kluis is gemaakt, wordt het in de portal geopend.

### <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Laat de optie ingesteld op geografisch redundante opslag als dit uw primaire back-up is. Kies lokaal redundante opslag als u een goedkopere optie wilt die niet zo duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy.md#geo-redundant-storage) en [lokaal redundante](../storage/common/storage-redundancy.md#locally-redundant-storage) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer de kluis om het kluisdashboard en de blade Instellingen te openen. Als de blade **Instellingen** niet wordt geopend, klikt u op **Alle instellingen** op het kluisdashboard.
2. Klik op de blade **Instellingen** op **Back-upinfrastructuur** > **Back-upconfiguratie** om de blade **Back-upconfiguratie** te openen. Kies op de blade **Back-upconfiguratie** de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om de VM aan de kluis te koppelen. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

### <a name="2-download-vault-credentials"></a>2. Kluisreferenties downloaden
Het kluisreferentiebestand is een certificaat dat de portal voor elke back-upkluis genereert. De portal uploadt de openbare sleutel vervolgens naar de Access Control Service (ACS). De persoonlijke sleutel van het certificaat wordt beschikbaar gesteld aan de gebruiker als onderdeel van de werkstroom die is opgegeven als invoer in de werkstroom van de registratie machine. Hiermee verifieert de machine voor het verzenden van back-upgegevens naar een geïdentificeerde kluis in de Azure Backup-service.

De kluisreferenties worden alleen gebruikt tijdens de registratiewerkstroom. Het is de verantwoordelijkheid van de gebruiker om ervoor te zorgen dat het kluisreferentiebestand niet worden gecompromitteerd. Als het kluisreferentiebestand in handen valt van een rogue-gebruiker, kan het bestand worden gebruikt om andere machines bij dezelfde kluis te registreren. Als de back-upgegevens worden versleuteld met een wachtwoordzin die deel uitmaakt van de klant, kan niet u bestaande back-upgegevens geschonden. Om dit probleem verhelpen, de kluisreferenties verlopen na 48hrs. U kunt de kluisreferenties een recovery Services downloaden vaak – maar alleen het meest recente kluisreferentiebestand is van toepassing tijdens de registratiewerkstroom.

Het kluisreferentiebestand is gedownload via een beveiligd kanaal vanuit de Azure-portal. De Azure Backup-service is niet op de hoogte van de persoonlijke sleutel van het certificaat en de persoonlijke sleutel is niet permanent in de portal of de service. Gebruik de volgende stappen voor het downloaden van het kluisreferentiebestand naar een lokale computer.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Open Recovery Services-kluis waarnaar machine waarop u wilt registreren van DPM.
3. Instellingenblade wordt standaard zijn geopend. Als dit is gesloten, klikt u op **instellingen** op kluisdashboard om de instellingenblade te openen. Op de blade instellingen, klikt u op **eigenschappen**.

    ![Blade Kluis openen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
4. Klik op de pagina eigenschappen van **downloaden** onder **back-up referenties**. De portal genereert het kluisreferentiebestand wordt beschikbaar gesteld voor downloaden.

    ![Downloaden](./media/backup-azure-dpm-introduction/vault-credentials.png)

De portal genereert een kluisreferentie met een combinatie van de kluisnaam van de en de huidige datum. Klik op **opslaan** de kluisreferenties downloaden naar de map downloads van het lokale account in of selecteer OpslaanAls in het menu opslaan naar een locatie opgeven voor de kluisreferenties. Het duurt om een minuut voor het bestand moet worden gegenereerd.

### <a name="note"></a>Opmerking
* Zorg ervoor dat het kluisreferentiebestand wordt opgeslagen op een locatie die toegankelijk is vanaf uw computer. Als deze is opgeslagen in een bestand share/SMB, controleert de toegangsmachtigingen.
* Het kluisreferentiebestand wordt alleen gebruikt tijdens de registratiewerkstroom.
* Het kluisreferentiebestand na 48hrs verlopen en kan worden gedownload vanuit de portal.

### <a name="3-install-backup-agent"></a>3. Back-Agent installeren
Nadat de Azure Backup-kluis is gemaakt, moet een agent worden geïnstalleerd op elk van uw Windows-machines (Windows Server, Windows-client, System Center Data Protection Manager-server of Azure Backup-Server-machine) waarmee een back-up van gegevens en toepassingen naar Azure.

1. Open Recovery Services-kluis waarnaar machine waarop u wilt registreren van DPM.
2. Instellingenblade wordt standaard zijn geopend. Als dit is gesloten, klikt u op **instellingen** om de instellingenblade te openen. Op de blade instellingen, klikt u op **eigenschappen**.

    ![Blade Kluis openen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Klik op de pagina instellingen **downloaden** onder **Azure Backup Agent**.

    ![Downloaden](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Nadat de agent wordt gedownload, dubbelklikt u op MARSAgentInstaller.exe voor het starten van de installatie van de Azure Backup agent. Kies de installatiemap en de tijdelijke map is vereist voor de agent. De opgegeven locatie van de cache moet vrije schijfruimte die ten minste 5% van de back-upgegevens bevatten.
4. Als u een proxyserver gebruiken om te verbinden met het internet, in de **proxyconfiguratie** scherm, voert u de gegevens van de proxy-server. Als u een geverifieerde proxyserver gebruikt, voert u de gegevens van de gebruiker en het wachtwoord in dit scherm.
5. De Azure Backup agent installeert u .NET Framework 4.5 en Windows PowerShell (indien deze nog niet beschikbaar is) om de installatie te voltooien.
6. Nadat de agent is geïnstalleerd, **sluiten** het venster.

   ![Sluiten](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)
7. Naar **de DPM-Server registreren** naar de kluis in de **Management** tabblad, klik op **Online**. Selecteer **registreren**. De Wizard Setup registreren wordt geopend.
8. Als u een proxyserver gebruiken om te verbinden met het internet, in de **proxyconfiguratie** scherm, voert u de gegevens van de proxy-server. Als u een geverifieerde proxyserver gebruikt, voert u de gegevens van de gebruiker en het wachtwoord in dit scherm.

    ![Proxy-configuratie](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. In het scherm van de referenties kluis Blader naar en selecteer het kluisreferentiebestand die eerder zijn gedownload.

    ![Referenties voor de kluis](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Het kluisreferentiebestand is alleen geldig voor 48 uur (nadat deze gedownload vanuit de portal). Als er een fout in dit scherm (bijvoorbeeld ' kluisreferenties opgegeven bestand is verlopen), meld u aan bij de Azure portal en download die de kluisreferenties bestand opnieuw optreden.

    Zorg ervoor dat het kluisreferentiebestand beschikbaar is op een locatie die toegankelijk zijn voor het installatieprogramma. Als u toegang tot gerelateerde fouten het kluisreferentiebestand te kopiëren naar een tijdelijke locatie op deze machine en probeer het opnieuw.

    Als er een ongeldige kluis referentie-fout optreedt (bijvoorbeeld ' kluis ongeldige referenties opgegeven') het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld met de service voor herstel. Probeer het opnieuw nadat het downloaden van een nieuw kluisreferentiebestand via de portal. Deze fout wordt doorgaans te zien wanneer de gebruiker op de **kluisreferentie downloaden** optie in de Azure-portal snel achter elkaar. In dit geval wordt is alleen het tweede kluisreferentiebestand geldig.
10. Als u het gebruik van netwerkbandbreedte tijdens het werk en niet-werkuren, in de **Beperkingsinstelling** scherm, stelt u de limieten voor bandbreedtegebruik en definiëren van de afdelingen en niet-werk uur.

    ![Beperking van de instelling](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)
11. In de **herstel mapinstelling** scherm, bladeren naar de map waarin de bestanden gedownload van Azure tijdelijk tijdelijk opgeslagen.

    ![Instellingen van de map Recovery](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)
12. In de **versleutelingsinstelling** scherm kunt u een wachtwoordzin genereren of geef een wachtwoordzin (minimaal 16 tekens). Moet u de wachtwoordzin op een veilige locatie opslaan.

    ![Versleuteling](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Als u de wachtwoordzin kwijtraakt of vergeet; Microsoft kan niet helpen bij het herstellen van de back-upgegevens. De gebruiker eigenaar is van de wachtwoordzin voor versleuteling en Microsoft heeft geen zichtbaarheid van de wachtwoordzin die wordt gebruikt door de eindgebruiker. Sla het bestand op een veilige locatie als dit nodig tijdens een herstelbewerking wordt uitgevoerd is.
    >
    >
13. Nadat u op de **registreren** knop, de machine naar de kluis is geregistreerd en u bent nu klaar om back-ups op Microsoft Azure.
14. Als u Data Protection Manager, kunt u de instellingen die zijn opgegeven tijdens de registratiewerkstroom door te klikken op de **configureren** optie door te selecteren **Online** onder de **Management** tabblad.

## <a name="requirements-and-limitations"></a>Vereisten (en beperkingen)
* DPM kan worden uitgevoerd als een fysieke server of een Hyper-V virtuele machine is geïnstalleerd op System Center 2012 SP1 of System Center 2012 R2. Kan ook worden uitgevoerd als een virtuele machine van Azure met System Center 2012 R2 met ten minste DPM 2012 R2 updatepakket 3 of een virtuele Windows-machine in VMWare uitgevoerd op System Center 2012 R2 met ten minste updatepakket 5.
* Als u DPM met System Center 2012 SP1 uitvoert moet u installeren Update Roll up 2 voor System Center Data Protection Manager SP1. Dit is vereist voordat u de Azure Backup Agent kunt installeren.
* De DPM-server moet Windows PowerShell en .net Framework 4.5 geïnstalleerd.
* DPM kan back-up meeste workloads naar Azure Backup. De Azure Backup ondersteuning voor een volledige lijst met wat is er ondersteund Zie onderstaande items.
* Gegevens die zijn opgeslagen in Azure Backup kunnen niet worden hersteld met de optie 'kopiëren naar tape'.
* U moet een Azure-account met de Azure Backup-functie ingeschakeld. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Meer informatie over [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Azure Backup, moet de Azure Backup-Agent moet worden geïnstalleerd op de servers die u back wilt-up. Elke server moet ten minste 5% van de grootte van de gegevens die worden back-up, beschikbaar als lokale vrije opslagruimte hebben. Bijvoorbeeld, vereist een back-up 100 GB aan gegevens een minimum van 5 GB vrije ruimte op de nieuwe locatie.
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
> Van in System Center 2012 DPM met SP1 en hoger u kunt back-up van de werkbelasting die zijn beveiligd door DPM naar Azure met Microsoft Azure Backup.
>
>
