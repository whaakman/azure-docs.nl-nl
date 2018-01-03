---
title: Back-up en herstel virtuele machines versleuteld met behulp van Azure Backup
description: In dit artikel wordt gesproken over de ervaring van de back-up en herstel voor virtuele machines die zijn versleuteld met behulp van Azure Disk Encryption.
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 8387f186-7d7b-400a-8fc3-88a85403ea63
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/13/2017
ms.author: pajosh;markgal;trinadhk; sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 509e891207d1469ed244eab4512ec66420284fd5
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Back-up en herstel van versleutelde virtuele machines met Azure Backup
In dit artikel wordt gesproken over de stappen voor de back-up en herstellen van virtuele machines (VM's) met behulp van Azure Backup. Het bevat ook informatie over ondersteunde scenario's, vereisten en stappen voor probleemoplossing voor foutgevallen.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

 * Back-up en herstel van versleutelde virtuele machines wordt alleen ondersteund voor virtuele machines die gebruikmaken van het Azure Resource Manager-implementatiemodel. Dit wordt niet ondersteund voor virtuele machines die gebruikmaken van het klassieke implementatiemodel. <br>
 * Back-up en herstel van versleutelde VM's wordt ondersteund voor Windows- en Linux-VM's die gebruikmaken van Azure Disk Encryption. Schijfversleuteling wordt de branche standaard BitLocker-functies van Windows en dm-crypt van Linux gebruikt voor versleuteling van schijven. <br>
 
 De volgende tabel bevat de ondersteunde scenario's voor BitLocker-versleutelingssleutel (BEK) - sleutel en alleen versleutelingssleutel (KEK-sleutel) - gecodeerd virtuele machines:
 
 
   |  | BEK + KEK virtuele machines | Virtuele machines BEK alleen-lezen |
   | --- | --- | --- |
   | **Niet-beheerde virtuele machines**  | Ja | Ja  |
   | **Beheerde virtuele machines**  | Ja | Ja  |

## <a name="prerequisites"></a>Vereisten
* De virtuele machine is versleuteld met behulp van [Azure Disk Encryption](../security/azure-security-disk-encryption.md).

* Een Recovery Services-kluis is gemaakt en storage-replicatie is ingesteld door de stappen in [uw omgeving voorbereiden voor back-up](backup-azure-arm-vms-prepare.md).

* Back-up is opgegeven [machtigingen voor toegang tot een sleutelkluis](#provide-permissions-to-azure-backup) met sleutels en geheimen voor virtuele machines versleuteld.

## <a name="backup-encrypted-vm"></a>Back-up versleuteld VM
Gebruik de volgende stappen voor instellen van een back-updoel, beleid, items configureren en activeren van een back-up.

### <a name="configure-backup"></a>Back-up configureren
1. Als u al een Recovery Services-kluis openen, gaat u verder met de volgende stap. Als u een Recovery Services-kluis openen niet hebt, maar u zich in de Azure-portal op de **Hub** selecteert u **Bladeren**.

   a. Typ in de lijst met resources **Recovery Services**.

   b. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer er **Recovery Services-kluizen**, selecteert u deze.

      ![Recovery Services-kluis](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer een kluis in de lijst.

     Het geselecteerde kluisdashboard wordt geopend.
2. Selecteer in de lijst met items die onder de kluis wordt weergegeven, **back-up** back-ups van de versleutelde VM starten.

      ![Back-blade](./media/backup-azure-vms-encryption/select-backup.png)
3. Op de **back-up** tegel, selecteer **back-updoel**.

      ![Blade scenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Onder **waar wordt uw workload uitgevoerd?**, selecteer **Azure**. Onder **wat wilt u back-up maken?**, selecteer **virtuele machine**. Selecteer vervolgens **OK**.

   ![Blade Scenario openen](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Onder **back-upbeleid kiezen**, selecteert u het back-upbeleid dat u wilt toepassen op de kluis. Selecteer vervolgens **OK**.

      ![Back-upbeleid selecteren](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden weergegeven. Als u een beleid maken wilt, selecteert u **nieuw** uit de vervolgkeuzelijst. Nadat u hebt geselecteerd **OK**, het back-upbeleid is gekoppeld aan de kluis.

6. Kies de versleutelde virtuele machines om te koppelen aan het opgegeven beleid en selecteer **OK**.

      ![Selecteer versleutelde virtuele machines](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Deze pagina bevat een bericht over sleutelkluizen die is gekoppeld aan de versleutelde virtuele machines die u hebt geselecteerd. Back-up vereist alleen-lezen toegang tot de sleutels en geheimen in de sleutelkluis. Deze machtigingen wordt gebruikt voor back-up van de sleutels en geheimen, samen met de bijbehorende virtuele machines.<br>
Als u een **lid gebruiker**, inschakelen back-upproces wordt naadloos toegang tot de sleutelkluis te verkrijgen back-up van virtuele machines versleuteld zonder tussenkomst van de gebruiker.

   ![Versleutelde VMs bericht](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   Voor een **gastgebruiker**, moet u machtigingen voor de Backup-service voor toegang tot de sleutelkluis voor back-ups werken opgeven. U kunt deze machtigingen opgeven door de [stappen die worden vermeld in de volgende sectie](#provide-permissions-to-backup)

   ![Versleutelde VMs bericht](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)
 
    Nu dat u alle instellingen voor de kluis hebt gedefinieerd, selecteer **back-up inschakelen** aan de onderkant van de pagina. **Back-up inschakelen** implementeert het beleid op de kluis en de virtuele machines.
  
8. De volgende fase in de voorbereiding van de VM-Agent wordt geïnstalleerd of alleen voor zorgen dat de VM-Agent is geïnstalleerd. Volg de stappen in dezelfde doet [uw omgeving voorbereiden voor back-up](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Een back-uptaak wordt geactiveerd
Volg de stappen in [back-Azure-machines naar een Recovery Services-kluis](backup-azure-arm-vms.md) voor het activeren van een back-uptaak.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Back-ups van virtuele machines al een back-up gaan met versleuteling ingeschakeld  
Als u virtuele machines al back-up gemaakt in een Recovery Services-kluis die zijn ingeschakeld voor versleuteling later hebt, moet u machtigingen geven voor de back-up voor toegang tot de sleutelkluis voor back-ups om door te gaan. U kunt deze machtigingen opgeven door de [stappen in de volgende sectie](#provide-permissions-to-azure-backup). Of u kunt de stappen PowerShell in de sectie 'Back-up' van de [PowerShell documentatie](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-backup"></a>Machtigingen verlenen aan back-up
Gebruik de volgende stappen uit om relevante machtigingen aan back-up voor toegang tot de sleutelkluis en back-up van versleutelde virtuele machines.
1. Selecteer **meer services**, en zoek naar **kluizen sleutel**.

    ![Sleutelkluizen](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Selecteer in de lijst van sleutelkluizen, de sleutelkluis die zijn gekoppeld aan de versleutelde virtuele machine die u een back moet-up.

     ![Sleutelkluis-selectie](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Selecteer **toegangsbeleid**, en selecteer vervolgens **nieuwe toevoegen**.

    ![Nieuwe toevoegen](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Selecteer **Selecteer principal**, en typ vervolgens **back-up-beheerservice** in het zoekvak. 

    ![Backup-service zoeken](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Selecteer **back-up-beheerservice**, en selecteer vervolgens **Selecteer**.

    ![Selectie van de Backup-service](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Onder **configureren van sjabloon (optioneel)**, selecteer **Azure Backup**. De vereiste machtigingen worden gevuld voor **sleutel machtigingen** en **geheime machtigingen**. Als uw VM is versleuteld met behulp van **BEK alleen**, machtigingen alleen voor geheimen zijn vereist, dus moet u de selectie voor **sleutel machtigingen**.

    ![Azure Backup-selectie](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Selecteer **OK**. U ziet dat **back-up-beheerservice** wordt toegevoegd **toegangsbeleid**. 

    ![Toegangsbeleid](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Selecteer **opslaan** zodat de vereiste machtigingen tot de back-up.

    ![Back-up-beleid](./media/backup-azure-vms-encryption/save-access-policy.png)

Nadat de machtigingen zijn is opgegeven, kunt u doorgaan met het inschakelen van back-up voor versleutelde virtuele machines.

## <a name="restore-an-encrypted-vm"></a>Een versleutelde virtuele machine herstellen
Voor het herstellen van een versleutelde VM eerst herstellen schijven volgens de stappen in de sectie 'Een back-up schijven herstellen' in [kiest u de configuratie van een virtuele machine herstellen](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Daarna kunt u een van de volgende opties:

* Volg de stappen in de PowerShell [een virtuele machine maken van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) voor het maken van een volledige virtuele machine van herstelde schijven.
* Of, [sjablonen gebruiken voor het aanpassen van een herstelde virtuele machine](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) voor het maken van virtuele machines van herstelde schijven. Sjablonen kunnen alleen worden gebruikt voor herstelpunten die zijn gemaakt na 26 April 2017.

## <a name="troubleshooting-errors"></a>Het oplossen van problemen
| Bewerking | Foutdetails | Oplossing |
| --- | --- | --- |
|Back-up maken | Back-up niet voldoende machtigingen hebben voor de sleutelkluis voor back-up van versleutelde virtuele machines. | Back-up moet deze machtigingen worden opgegeven door de volgende de [stappen in de vorige sectie](#provide-permissions-to-azure-backup). Of u kunt de PowerShell-stappen in de sectie 'Beveiliging inschakelen' van de PowerShell-documentatie op volgen [gebruik AzureRM.RecoveryServices.Backup-cmdlets voor back-up van virtuele machines](backup-azure-vms-automation.md#back-up-azure-vms). |  
| Herstellen |U kunt deze versleutelde virtuele machine niet herstellen omdat de sleutelkluis die zijn gekoppeld aan deze virtuele machine bestaat niet. |Een sleutelkluis maken met behulp van [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md). Zie [een sleutelkluis-sleutel en een geheim herstellen met behulp van Azure Backup](backup-azure-restore-key-secret.md) voor het herstellen van een sleutel en een geheim als ze niet aanwezig is. |
| Herstellen |U kunt deze versleutelde virtuele machine niet herstellen omdat de sleutel en het geheim die is gekoppeld aan deze virtuele machine niet bestaat. |Zie [een sleutelkluis-sleutel en een geheim herstellen met behulp van Azure Backup](backup-azure-restore-key-secret.md) voor het herstellen van een sleutel en een geheim als ze niet aanwezig is. |
| Herstellen |Back-up beschikt niet over de autorisatie voor toegang tot resources in uw abonnement. |Zoals eerder vermeld, schijven eerst herstellen door de stappen in de sectie 'Een back-up schijven herstellen' in [kiest u de configuratie van een virtuele machine herstellen](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Daarna gebruikt u PowerShell om te [een virtuele machine maken van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
