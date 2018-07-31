---
title: De configuratieserver voor VMware-noodherstel met Azure Site Recovery beheren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een bestaande configuratieserver voor VMware naar Azure met Azure Site RecoveryS een noodgeval beheren.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: df5b2ecce2a5c9d7c263ee0acc3a49b859b93f7f
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346117"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>De configuratieserver voor VMware-VM's beheren

Instellen van een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratieserver coördineert de communicatie tussen on-premises VMware- en Azure en beheert de gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de configuratieserver nadat deze geïmplementeerd.



## <a name="modify-vmware-settings"></a>VMware-instellingen wijzigen

De configuratieserver kunt u als volgt openen:
    - Meld u aan de virtuele machine waarop deze geïmplementeerd, en Start Azure Site Recovery Configuration Manager op de snelkoppeling op het bureaublad.
    - U kunt ook de configuratieserver van op afstand openen **https://*ConfigurationServerName*/:44315 /**. Meld u aan met beheerdersreferenties.
   
### <a name="modify-vmware-server-settings"></a>Instellingen voor VMware-server wijzigen

1. Als u wilt een andere VMware-server koppelen aan de configuratieserver, na het aanmelden, selecteer **vCenter Server/vSphere ESXi-server toevoegen**.
2. Voer de details in en selecteer vervolgens **OK**.


### <a name="modify-credentials-for-automatic-discovery"></a>Referenties wijzigen voor automatische detectie

1. Selecteer voor het bijwerken van de referenties waarmee verbinding wordt gemaakt met de VMware-server voor automatische detectie van virtuele VMware-machines na het aanmelden, **bewerken**.
2. Geef de nieuwe referenties op en selecteer vervolgens **OK**.

    ![Wijzigen van VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)


## <a name="modify-credentials-for-mobility-service-installation"></a>Referenties wijzigen voor de installatie van Mobility Service

Wijzig de referenties die worden gebruikt voor het installeren van Mobility Service automatisch op de VMware-VM's die u voor replicatie inschakelen.

1. Na het aanmelden, selecteer **referenties voor virtuele machine beheren**
2. Geef de nieuwe referenties op en selecteer vervolgens **OK**.

    ![Referenties van de Mobility-Service wijzigen](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Proxy-instellingen wijzigen

Wijzig de proxy-instellingen die worden gebruikt door de configuratie van server-machine voor internettoegang tot Azure. Als u een proces-servercomputer naast de standaard-processerver die wordt uitgevoerd op de servercomputer van de configuratie hebt, kunt u de instellingen op beide computers wijzigen.

1. Na het aanmelden bij de configuratieserver, selecteer **verbindingen beheren**.
2. Werk de proxy-waarden. Selecteer vervolgens **opslaan** om de instellingen te werken.

## <a name="add-a-network-adapter"></a>Een netwerkadapter toevoegen

De Open Virtualization Format (OVF)-sjabloon implementeert de configuratieserver VM met één netwerkadapter.

- U kunt [een extra adapter toevoegen aan de virtuele machine](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), maar moet u deze toevoegen voordat u de configuratieserver in de kluis registreert.
- Om toe te voegen een adapter nadat u de configuratieserver in de kluis registreert, moet u de adapter toevoegen in de VM-eigenschappen. Vervolgens moet u de server in de kluis registreren.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Een configuratieserver in dezelfde kluis registreren

Als u wilt, kunt u de configuratieserver in dezelfde kluis registreren. Hebt u een extra proces server virtuele machine, naast de standaard-processerver die wordt uitgevoerd op de servercomputer van de configuratie opnieuw registreren op beide machines.


  1. Open in de kluis **beheren** > **Site Recovery-infrastructuur** > **configuratieservers**.
  2. In **Servers**, selecteer **registratiesleutel downloaden** voor het downloaden van het bestand met kluisreferenties.
  3. Aanmelden bij de configuratie van server-machine.
  4. In **%ProgramData%\ASR\home\svsystems\bin**Open **cspsconfigtool.exe**.
  5. Op de **kluis registratie** tabblad **Bladeren**, en Ga naar het bestand met kluisreferenties die u hebt gedownload.
  6. Indien nodig, bieden u proxy-server-gegevens. Selecteer vervolgens **Registreren**.
  7. Open een admin PowerShell-opdrachtvenster en voer de volgende opdracht uit:
   ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

      >[!NOTE] 
      >Om **ophalen van de meest recente certificaten** Voer de opdracht uit vanaf de configuratieserver naar uitbreidbare processerver *"< installatie Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe >"--registermt uit*

  8. Ten slotte de obengine opnieuw door de volgende opdracht wordt uitgevoerd.
  ```
          net stop obengine
          net start obengine

## Upgrade the configuration server

You run update rollups to update the configuration server. Updates can be applied for up to N-4 versions. For example:

- If you run 9.7, 9.8, 9.9, or 9.10, you can upgrade directly to 9.11.
- If you run 9.6 or earlier and you want to upgrade to 9.11, you must first upgrade to version 9.7. before 9.11.

Links to update rollups for upgrading to all versions of the configuration server are available in the [wiki updates page](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Upgrade the server as follows:

1. In the vault, go to **Manage** > **Site Recovery Infrastructure** > **Configuration Servers**.
2. If an update is available, a link appears in the **Agent Version** > column.
    ![Update](./media/vmware-azure-manage-configuration-server/update2.png)
3. Download the update installer file to the configuration server.

    ![Update](./media/vmware-azure-manage-configuration-server/update1.png)

4. Double-click to run the installer.
5. The installer detects the current version running on the machine. Click **Yes** to start the upgrade.
6. When the upgrade completes the server configuration validates.

    ![Update](./media/vmware-azure-manage-configuration-server/update3.png)
    
7. Click **Finish** to close the installer.

## Delete or unregister a configuration server

1. [Disable protection](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) for all VMs under the configuration server.
2. [Disassociate](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) and [delete](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) all replication policies from the configuration server.
3. [Delete](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) all vCenter servers/vSphere hosts that are associated with the configuration server.
4. In the vault, open **Site Recovery Infrastructure** > **Configuration Servers**.
5. Select the configuration server that you want to remove. Then, on the **Details** page, select **Delete**.

    ![Delete configuration server](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### Delete with PowerShell

You can optionally delete the configuration server by using PowerShell.

1. [Install](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) the Azure PowerShell module.
2. Sign in to your Azure account by using this command:
    
    `Connect-AzureRmAccount`
3. Select the vault subscription.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Set the vault context.
    
    ```
    $vault = get-AzureRmRecoveryServicesVault-naam <name of your vault> Set AzureRmSiteRecoveryVaultSettings - ARSVault $vault
    ```
4. Retrieve the configuration server.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Delete the configuration server.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> You can use the **-Force** option in Remove-AzureRmSiteRecoveryFabric for forced deletion of the configuration server.
 
## Generate configuration server Passphrase

1. Sign in to your configuration server, and then open a command prompt window as an administrator.
2. To change the directory to the bin folder, execute the command **cd %ProgramData%\ASR\home\svsystems\bin**
3. To generate the passphrase file, execute **genpassphrase.exe -v > MobSvc.passphrase**.
4. Your passphrase will be stored in the file located at **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## Renew SSL certificates

The configuration server has an inbuilt web server, which orchestrates activities of the Mobility Service, process servers, and master target servers connected to it. The web server uses an SSL certificate to authenticate clients. The certificate expires after three years and can be renewed at any time.

### Check expiry

For configuration server deployments before May 2016, certificate expiry was set to one year. If you have a certificate that is going to expire, the following occurs:

- When the expiry date is two months or less, the service starts sending notifications in the portal, and by email (if you subscribed to Site Recovery notifications).
- A notification banner appears on the vault resource page. For more information, select the banner.
- If you see an **Upgrade Now** button, it indicates that some components in your environment haven't been upgraded to 9.4.xxxx.x or higher versions. Upgrade the components before you renew the certificate. You can't renew on older versions.

### Renew the certificate

1. In the vault, open **Site Recovery Infrastructure** > **Configuration Server**. Select the required configuration server.
2. The expiry date appears under **Configuration Server health**.
3. Select **Renew Certificates**. 


## Next steps

Review the tutorials for setting up disaster recovery of [VMware VMs](vmware-azure-tutorial.md) to Azure.
