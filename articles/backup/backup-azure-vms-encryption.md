---
title: Back-up en herstel van versleutelde Azure virtuele machines met Azure Backup
description: Beschrijft hoe u een back-up en herstel van versleutelde Azure-VM's met de Azure Backup-service.
services: backup
author: geetha
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 4/3/2019
ms.author: geetha
ms.openlocfilehash: 893a22fb9f325625707869c8f6571d572b8f6b33
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358227"
---
# <a name="back-up-and-restore-encrypted-azure-vm"></a>Back-up en herstel van versleutelde virtuele Azure-machine

Dit artikel wordt beschreven hoe u back-up en herstellen van Windows of Linux Azure virtuele machines (VM's) met versleutelde schijven met behulp van de [Azure Backup](backup-overview.md) service.

Als u meer informatie wilt over hoe Azure Backup met Azure-VM's communiceert voordat u begint, moet u deze resources controleren:

- [Beoordeling](backup-architecture.md#architecture-direct-backup-of-azure-vms) de architectuur van Azure VM-back-up.
- [Meer informatie over](backup-azure-vms-introduction.md) back-up van virtuele Azure-machine en de Azure Backup-extensie.

## <a name="encryption-support"></a>Ondersteuning voor versleuteling

Azure Backup biedt ondersteuning voor back-up van virtuele Azure-machines waarvoor hun OS/gegevensschijven versleuteld met Azure Disk Encryption (ADE). ADE maakt gebruik van BitLocker voor het versleutelen van de Windows-VM's en de functie dm-crypt voor Linux-VM's. ADE kan worden geïntegreerd met Azure Key Vault om versleutelingssleutels op de schijf en geheimen te beheren. Versleutelingssleutels voor Key Vault-sleutel (kek's) kunnen worden gebruikt om een extra beveiligingslaag toevoegen encryption geheimen versleutelen voordat ze worden geschreven naar Key Vault.

Azure Backup kunt back-up en herstellen van virtuele Azure-machines met behulp van ADE met en zonder de Azure AD-app, zoals samengevat in de volgende tabel.

**Type VM-schijf** | **ADE (BEK/dm-crypt)** | **ADE en KEK-sleutel**
--- | --- | ---
**Unmanaged** | Ja | Ja
**Managed**  | Ja | Ja

- Meer informatie over [ADE](../security/azure-security-disk-encryption-overview.md), [Key Vault](../key-vault/key-vault-overview.md), en [kek's](https://blogs.msdn.microsoft.com/cclayton/2017/01/03/creating-a-key-encrypting-key-kek/).
- Lees de [Veelgestelde vragen over](../security/azure-security-disk-encryption-faq.md) voor versleuteling van de Azure-VM-schijf.



### <a name="limitations"></a>Beperkingen

- U kunt een back-up en herstel van versleutelde virtuele machines binnen hetzelfde abonnement en regio.
- Azure Backup ondersteunt versleuteld met sleutels die zelfstandige virtuele machines. Een willekeurige toets die deel uitmaakt van een certificaat dat wordt gebruikt voor het versleutelen van een virtuele machine wordt momenteel niet ondersteund.
- U kunt een back-up en herstel van versleutelde virtuele machines binnen hetzelfde abonnement en dezelfde regio bevinden als de back-up van de Recovery Services-kluis.
- Versleutelde virtuele machines kunnen niet worden hersteld op het niveau van het bestand/map. U moet de volledige virtuele machine voor het herstellen van bestanden en mappen herstellen.
- Bij het herstellen van een virtuele machine, u kunt geen gebruiken de [vervangen bestaande virtuele machine](backup-azure-arm-restore-vms.md#restore-options) optie voor versleutelde virtuele machines. Deze optie wordt alleen ondersteund voor niet-versleutelde beheerde schijven.




## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, het volgende doen:

1. Zorg ervoor dat u hebt een of meer [Windows](../security/azure-security-disk-encryption-windows.md) of [Linux](../security/azure-security-disk-encryption-linux.md) VM's met ADE ingeschakeld.
2. [Bekijk de ondersteuningsmatrix](backup-support-matrix-iaas.md) voor Azure VM backup
3. [Maak](backup-azure-arm-vms-prepare.md#create-a-vault) een back-up van de Recovery Services-kluis als u dit niet hebt.
4. Als u versleuteling voor virtuele machines die al is ingeschakeld voor back-up inschakelt, moet u alleen back-up bieden met machtigingen voor toegang tot de Key Vault zodat back-ups zonder onderbreking toegang houden. [Meer informatie](#provide-permissions) over het toewijzen van deze machtigingen.

Er zijn bovendien een aantal dingen die u moet uitvoeren in sommige gevallen:

- **De VM-agent installeren op de virtuele machine**: Azure Backup back-ups van virtuele Azure-machines door het installeren van een extensie op de Azure VM-agent wordt uitgevoerd op de machine. Als uw virtuele machine vanuit een Azure marketplace-installatiekopie is gemaakt, wordt de agent is geïnstalleerd en worden uitgevoerd. Als u een aangepaste VM maakt, of u een on-premises machine migreert, moet u mogelijk [de agent handmatig installeren](backup-azure-arm-vms-prepare.md#install-the-vm-agent).
- **Uitgaand verkeer expliciet toestaan**: Over het algemeen moet u niet expliciet uitgaand netwerkverkeer toegang toestaan voor een Azure-VM om te communiceren met Azure Backup. Echter, enkele VM's mogelijk ondervindt problemen met verbindingen, met de **ExtensionSnapshotFailedNoNetwork** fout bij de poging om verbinding te maken. Als dit gebeurt, moet u [uitgaand verkeer expliciet toestaan](backup-azure-arm-vms-prepare.md#explicitly-allow-outbound-access), zodat de Azure Backup-extensie met Azure openbare IP-adressen voor back-upverkeer communiceren kan.



## <a name="configure-a-backup-policy"></a>Een back-upbeleid configureren

1. Als u een Recovery Services-kluis voor back-up nog niet hebt gemaakt, voert u de [deze instructies](backup-azure-arm-vms-prepare.md#create-a-vault)
2. Open de kluis in de portal en selecteer **back-up** in de **aan de slag** sectie.

    ![Blade back-up](./media/backup-azure-vms-encryption/select-backup.png)

3. In **back-updoel** > **waar wordt uw werkbelasting uitgevoerd?** Selecteer **Azure**.
4. In **wat wilt u een back-up?** Selecteer **virtuele machine** > **OK**.

      ![Blade scenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

5. In **back-upbeleid** > **back-upbeleid kiezen**, selecteert u het beleid dat u wilt koppelen aan de kluis. Klik vervolgens op **OK**.
    - Een back-upbeleid geeft aan wanneer de back-ups worden gemaakt en hoe lang ze worden opgeslagen.
    - De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven.

    ![Blade Scenario openen](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

6. Als u niet wilt gebruiken van het standaardbeleid, selecteert u **nieuw**, en [een aangepast beleid maken](backup-azure-arm-vms-prepare.md#create-a-custom-policy).


7. Kies de versleutelde virtuele machines die u back wilt-up met behulp van de optie beleid en selecteer **OK**.

      ![Versleutelde virtuele machines selecteren](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

8. Als u Azure Key Vault, op de pagina van de kluis, ziet u een bericht dat Azure Backup nodig alleen-lezen toegang tot de sleutels en geheimen in de Key Vault heeft.

    - Als u dit bericht ontvangt, is geen actie vereist.

        ![Toegang OK](./media/backup-azure-vms-encryption/access-ok.png)

    - Als u dit bericht ontvangt, moet u machtigingen instellen, zoals beschreven in de [onderstaande procedure](#provide-permissions).

        ![Waarschuwing voor toegang](./media/backup-azure-vms-encryption/access-warning.png)

9. Klik op **back-up inschakelen** voor het implementeren van het back-upbeleid in de kluis en back-up inschakelen voor de geselecteerde virtuele machines.


## <a name="trigger-a-backup-job"></a>Een back-uptaak activeert

De eerste back-up wordt uitgevoerd volgens de planning, maar u kunt deze onmiddellijk als volgt uitvoeren:

1. Klik in het kluismenu op **back-up items**.
2. In **back-Upitems** klikt u op **virtuele Azure-Machine**.
3. In de **back-Upitems** lijst, klikt u op het beletselteken (...).
4. Klik op **back-up nu**.
5. In **nu back-up**, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag waarop het herstelpunt dat moet worden bewaard. Klik vervolgens op **OK**.
6. Controleer de portal. U kunt de voortgang van de taak op het kluisdashboard bewaken > **back-uptaken** > **Bezig**. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.


## <a name="provide-permissions"></a>Machtigingen verlenen

Azure-VM moet alleen-lezen toegang tot de back-up van de sleutels en geheimen, samen met de bijbehorende virtuele machines.

- Uw Key Vault is gekoppeld aan de Azure AD-tenant van de Azure-abonnement. Als u bent een **gebruiker lid**, back-up van Azure krijgt toegang tot de Key Vault zonder verdere actie.
- Als u bent een **gastgebruiker**, moet u machtigingen voor Azure Backup voor toegang tot de key vault opgeven.

Machtigingen instellen:

1. Selecteer in de Azure portal, **alle services**, en zoek naar de **Key vaults**.
2. Selecteer de sleutelkluis die is gekoppeld aan de versleutelde VM u back-up.
3. Selecteer **toegangsbeleid** > **nieuwe toevoegen**.
4. Selecteer **Selecteer principal**, en typ vervolgens **back-up Management**.
5. Selecteer **back-up van Managementservice** > **Selecteer**.

    ![Selectie van de Backup-service](./media/backup-azure-vms-encryption/select-backup-service.png)

6. In **toegangsbeleid toevoegen** > **configureren met sjabloon (optioneel)**, selecteer **Azure Backup**.
    - De vereiste machtigingen zijn ingevuld voor **sleutelmachtigingen** en **geheime machtigingen**.
    - Als uw virtuele machine is versleuteld met behulp van **BEK alleen**, verwijdert u de selectie voor **sleutelmachtigingen** omdat u alleen machtigingen voor geheimen nodig hebt.

    ![Azure back-up selecteren](./media/backup-azure-vms-encryption/select-backup-template.png)

6. Klik op **OK**. **Back-up van Service Management** wordt toegevoegd aan **toegangsbeleid**.

    ![Toegangsbeleid](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

7. Klik op **opslaan** voor Azure Backup met de machtigingen.

## <a name="restore-an-encrypted-vm"></a>Een versleutelde VM herstellen

U herstellen versleutelde virtuele machines als volgt:

1. [De VM-schijf herstellen](backup-azure-arm-restore-vms.md#restore-disks).
2. Voer een van de volgende handelingen uit:
    - Gebruik de sjabloon die wordt gegenereerd tijdens de herstelbewerking, VM-instellingen aanpassen en activeren van de VM-implementatie. [Meer informatie](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm).
    - Maak een nieuwe virtuele machine van de herstelde schijven met behulp van Powershell. [Meer informatie](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="next-steps"></a>Volgende stappen

Wanneer u problemen ondervindt, controleert u

- [Veelvoorkomende fouten](backup-azure-vms-troubleshoot.md) wanneer back-up en herstellen van virtuele Azure-machines versleuteld.
- [Azure VM agent/back-up-extensie](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) problemen.
