---
title: Back-up en herstel van versleutelde virtuele machines van Azure met behulp van Azure Backup-services
description: In dit artikel vertelt de ervaring van de back-up en herstel voor virtuele machines die met behulp van Azure Disk Encryption zijn versleuteld.
services: backup
author: geetha
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: geetha
ms.openlocfilehash: 28126df0dfd9a03e93a76fa5071331603c4819a4
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851015"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Back-up en herstel van versleutelde virtuele machines met Azure Backup
In dit artikel vertelt de stappen voor het back-up en herstellen van virtuele machines (VM's) met behulp van Azure Backup. Het bevat ook informatie over ondersteunde scenario's, vereisten en stappen voor probleemoplossing voor foutgevallen.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

 Back-up en herstel van versleutelde virtuele machines wordt alleen ondersteund voor virtuele machines die gebruikmaken van het Azure Resource Manager-implementatiemodel. Het wordt niet ondersteund voor virtuele machines die gebruikmaken van het klassieke implementatiemodel. Back-up en herstel van versleutelde virtuele machines wordt ondersteund voor Windows en Linux-VM's die gebruikmaken van Azure Disk Encryption. Schijfversleuteling maakt gebruik van de branche standaard BitLocker-functie van Windows en de dm-crypt-functie van Linux voor versleuteling van schijven. De volgende tabel ziet u versleutelingstype en ondersteuning voor VM's.

   |  | BEK + KEK-VM 's | Alleen-BEK VM 's |
   | --- | --- | --- |
   | **Niet-beheerde virtuele machines**  | Ja | Ja  |
   | **Beheerde VM 's**  | Ja | Ja  |

   > [!NOTE]
   > Azure Backup ondersteunt versleuteld met sleutels die zelfstandige virtuele machines. Een willekeurige toets die deel uitmaakt van een certificaat dat wordt gebruikt voor het versleutelen van een virtuele machine momenteel niet ondersteund.
   >

## <a name="prerequisites"></a>Vereisten
* De virtuele machine is versleuteld met behulp van [Azure Disk Encryption](../security/azure-security-disk-encryption.md).

* Een Recovery Services-kluis is gemaakt en storage-replicatie is ingesteld door de stappen in [uw omgeving voorbereidt voor back-up](backup-azure-arm-vms-prepare.md).

* Back-up is opgegeven machtigingen voor toegang tot een key vault sleutels en geheimen met voor versleutelde virtuele machines.

## <a name="back-up-an-encrypted-vm"></a>Back-up van een versleutelde VM
Gebruik de volgende stappen uit op een back-doel ingesteld, een beleid definiëren, items configureren en activeren van een back-up.

### <a name="configure-backup"></a>Back-up configureren
1. Als u al een Recovery Services-kluis openen, gaat u verder met de volgende stap. Als u een Recovery Services-kluis openen geen hebt, maar u zich in de Azure portal, selecteer **alle services**.

   a. Typ in de lijst met resources **Recovery Services**.

   b. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer de melding **Recovery Services-kluizen**, selecteert u deze.

      ![Recovery Services-kluis](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer een kluis in de lijst.

     Het geselecteerde kluisdashboard wordt geopend.
1. Selecteer in de lijst met items dat wordt weergegeven op de kluis, **back-up** back-ups van versleutelde VM starten.

      ![Blade back-up](./media/backup-azure-vms-encryption/select-backup.png)
1. Op de **back-up** tegel, selecteer **back-updoel**.

      ![Blade scenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
1. Onder **waar wordt uw werkbelasting uitgevoerd?**, selecteer **Azure**. Onder **wat wilt u een back-up?**, selecteer **virtuele machine**. Selecteer vervolgens **OK**.

   ![Blade Scenario openen](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
1. Onder **back-upbeleid kiezen**, selecteert u het back-upbeleid dat u wilt toepassen op de kluis. Selecteer vervolgens **OK**.

      ![Back-upbeleid selecteren](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden weergegeven. Als u een beleid maken wilt, selecteert u **nieuw** uit de vervolgkeuzelijst. Nadat u hebt geselecteerd **OK**, het back-upbeleid is gekoppeld aan de kluis.

1. Kies de versleutelde virtuele machines om te koppelen aan het opgegeven beleid en selecteer **OK**.

      ![Versleutelde virtuele machines selecteren](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
1. Deze pagina bevat een bericht over sleutelkluizen die is gekoppeld aan de versleutelde virtuele machines die u hebt geselecteerd. Back-up vereist alleen-lezen toegang tot de sleutels en geheimen in de key vault. Deze machtigingen gebruikt om back-up van de sleutels en geheimen, samen met de bijbehorende virtuele machines.<br>
Als u een **gebruiker lid**, back-up inschakelen-proces wordt naadloos verkrijgen van toegang tot de key vault nodig om back-up van versleutelde virtuele machines zonder tussenkomst van de gebruiker.

   ![Versleutelde virtuele machines-bericht](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   Voor een **gastgebruiker**, moet u machtigingen voor de Backup-service toegang heeft tot de sleutelkluis voor back-ups om te werken. U kunt deze machtigingen opgeven met de volgende stappen die worden vermeld in de volgende sectie

   ![Versleutelde virtuele machines-bericht](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)

    Nu dat u alle instellingen voor de kluis hebt gedefinieerd, selecteert u **back-up inschakelen** aan de onderkant van de pagina. **Back-up inschakelen** wordt het beleid geïmplementeerd naar de kluis en de virtuele machines.

1. De volgende fase ter voorbereiding op de VM-Agent wordt geïnstalleerd of te zorgen dat de VM-Agent is geïnstalleerd. Volg de stappen in hetzelfde doet [uw omgeving voorbereidt voor back-up](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Een back-uptaak activeert
Volg de stappen in [back-up Azure VM's naar een Recovery Services-kluis](backup-azure-arm-vms.md) voor het activeren van een back-uptaak.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Back-ups van virtuele machines al een back-up gaan met versleuteling ingeschakeld  
Als u virtuele machines al back-up in een Recovery Services-kluis die zijn ingeschakeld voor codering later hebt, moet u machtigingen om weer te geven tot toegang tot de sleutel-kluis voor back-ups om door te gaan. U kunt deze machtigingen opgeven door de [stappen in de volgende sectie](#provide-permissions). Of u kunt de stappen PowerShell in de sectie 'Back-up inschakelen' van de [PowerShell-documentatie](backup-azure-vms-automation.md).

## <a name="provide-permissions"></a>Machtigingen verlenen
Gebruik de volgende stappen uit voor de relevante machtigingen voor Azure Backup voor toegang tot de key vault en back-up van versleutelde virtuele machines.
1. Selecteer **alle services**, en zoek naar de **Key vaults**.

    ![Sleutelkluizen](./media/backup-azure-vms-encryption/search-key-vault.png)

1. Selecteer in de lijst van sleutelkluizen, de sleutelkluis die is gekoppeld aan de versleutelde VM die een back moet-up.

     ![Selectie van de sleutelkluis](./media/backup-azure-vms-encryption/select-key-vault.png)

1. Selecteer **toegangsbeleid**, en selecteer vervolgens **nieuwe toevoegen**.

    ![Nieuwe toevoegen](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)

1. Selecteer **Selecteer principal**, en typ vervolgens **Management-Service voor back-up** in het zoekvak in.

    ![Backup-service zoeken](./media/backup-azure-vms-encryption/search-backup-service.png)

1. Selecteer **Management-Service voor back-up**, en selecteer vervolgens **Selecteer**.

    ![Selectie van de Backup-service](./media/backup-azure-vms-encryption/select-backup-service.png)

1. Onder **configureren met sjabloon (optioneel)**, selecteer **Azure Backup**. De vereiste machtigingen zijn ingevuld voor **sleutelmachtigingen** en **geheime machtigingen**. Als uw virtuele machine is versleuteld met behulp van **BEK alleen**, machtigingen voor geheimen zijn vereist, dus moet u de selectie voor **sleutelmachtigingen**.

    ![Azure back-up selecteren](./media/backup-azure-vms-encryption/select-backup-template.png)

1. Selecteer **OK**. U ziet dat **Management-Service voor back-up** toegevoegd **toegangsbeleid**.

    ![Toegangsbeleid](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Selecteer **opslaan** back-up van de vereiste machtigingen bieden.

    ![Beleid voor back-toegang](./media/backup-azure-vms-encryption/save-access-policy.png)

Nadat de machtigingen zijn is opgegeven, kunt u doorgaan met het inschakelen van back-up voor versleutelde virtuele machines.

## <a name="restore-an-encrypted-vm"></a>Een versleutelde VM herstellen
Azure Backup nu ondersteunt herstel van [Azure een versleutelde VM zonder Azure AD](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-prerequisites-aad) naast de vorige aanbieding terugzetten versleuteld ondersteuning voor Azure VM met Azure AD.<br>

Als u een versleutelde VM herstellen, eerst schijven herstellen met de volgende stappen in de sectie 'Een back-up schijven herstellen' in [kiest u de configuratie van een virtuele machine terugzetten](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Hierna kunt u een van de volgende opties gebruiken:

* Volg de stappen in de PowerShell [maken van een VM van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) te maken van een volledige VM van herstelde schijven.
* Of, [sjablonen gebruiken voor het aanpassen van een herstelde VM](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) VM's van herstelde schijven maakt. Sjablonen kunnen alleen worden gebruikt voor herstelpunten die zijn gemaakt na 26 April 2017.

## <a name="troubleshooting-errors"></a>Fouten oplossen
| Bewerking | Foutdetails | Oplossing |
| --- | --- | --- |
|Backup | Foutcode: UserErrorKeyVaultPermissionsNotConfigured<br><br>Foutbericht: Azure Backup-Service beschikt niet over voldoende machtigingen voor Key Vault voor back-up van versleutelde virtuele Machines. | Back-up moet deze machtigingen worden opgegeven door de [stappen in de vorige sectie](#provide-permissions). Of u kunt de PowerShell-stappen in de sectie 'Beveiliging inschakelen' van het artikel volgen [PowerShell gebruiken om te back-up en herstellen van virtuele machines](backup-azure-vms-automation.md#enable-protection). |  
| Herstellen | U kunt deze versleutelde VM niet herstellen omdat de sleutelkluis die is gekoppeld aan deze virtuele machine bestaat niet. |Een sleutelkluis maken met behulp van [wat is Azure Key Vault?](../key-vault/key-vault-overview.md). Zie [herstellen van een key vault-sleutel en een geheim met behulp van Azure Backup](backup-azure-restore-key-secret.md) om terug te zetten van een sleutel en een geheim als ze niet aanwezig zijn. |
| Herstellen | Foutcode: UserErrorKeyVaultKeyDoesNotExist<br><br> Foutbericht: U kunt deze versleutelde VM niet herstellen omdat de sleutel die is gekoppeld aan deze virtuele machine bestaat niet. |Zie [herstellen van een key vault-sleutel en een geheim met behulp van Azure Backup](backup-azure-restore-key-secret.md) om terug te zetten van een sleutel en een geheim als ze niet aanwezig zijn. |
| Herstellen | Foutcode: ProviderAuthorizationFailed/UserErrorProviderAuthorizationFailed<br><br>Foutbericht: De Backup-service heeft geen toegang tot resources in uw abonnement. |Zoals eerder vermeld, herstel de schijven eerst met de volgende stappen in de sectie 'Een back-up schijven herstellen' in [kiest u de configuratie van een virtuele machine terugzetten](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Gebruik PowerShell om te daarna [maken van een VM van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
