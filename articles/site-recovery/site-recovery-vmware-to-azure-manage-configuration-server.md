---
title: " Beheren van de configuratieserver voor noodherstel van VMware met Azure Site Recovery | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u een bestaande configuratieserver voor noodherstel VMware naar Azure, met de Azure Site Recovery-service beheert.
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2017
ms.author: anoopkv
ms.openlocfilehash: e9e4bfc86df2cae1facac62472c915d91fb8c84c
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="manage-the-configuration-server"></a>De configuratieserver beheren

Instellen van een on-premises configuratieserver wanneer u de [Azure Site Recovery](site-recovery-overview.md) service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratieserver coördineert de communicatie tussen de on-premises VMware en Azure en beheert gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de configuratieserver nadat deze geïmplementeerd.

## <a name="modify-vmware-settings"></a>VMware-instellingen wijzigen

Instellingen wijzigen voor de VMware-server waarmee de configuratieserver verbinding maakt.

1. Meld u bij de computer uitgevoerd van de configuratieserver.
2. Azure Site Recovery Configuration Manager start via de snelkoppeling op het bureaublad. Of open **https://configuration-server-name/IP:44315**.
3. Klik op **beheren vCenter-Server/vSPhere ESXi server**:
    - U kunt een andere VMware-server koppelen met de configuratieserver, **vCenter Server vSphere/ESXi-server toevoegen**, en geef de Serverdetails.
    - Klik op om de referenties waarmee verbinding met de VMware-server voor automatische detectie van virtuele VMware-machines **bewerken**. Geef de nieuwe referenties op en klik vervolgens op **OK**.

        ![VMware wijzigen](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Referenties voor de installatie van de Mobility-service wijzigen

Wijzig de referenties voor de Mobility-service automatisch worden geïnstalleerd op virtuele VMware-machines u voor replicatie inschakelen.

1. Meld u bij de computer uitgevoerd van de configuratieserver.
2. Azure Site Recovery Configuration Manager start via de snelkoppeling op het bureaublad. Of open **https://configuration-server-name/IP:44315**.
3. Klik op **referenties van de virtuele machine beheren**, en geef nieuwe referenties. Klik vervolgens op **OK** om de instellingen te werken.

    ![Wijzigen van de Mobility-Servicereferenties](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Proxy-instellingen wijzigen

Wijzig de proxyinstellingen die worden gebruikt door de server-machine van de configuratie voor toegang tot het internet naar Azure. Als u hebt een extra proces server-machine, naast de standaard processerver uitgevoerd op de servercomputer van de configuratie van de instellingen op beide computers wijzigen.

1. Meld u bij de computer uitgevoerd van de configuratieserver.
2. Azure Site Recovery Configuration Manager start via de snelkoppeling op het bureaublad. Of open **https://configuration-server-name/IP:44315**.
3. Klik op **connectiviteit beheren**, en werk de proxy-waarden. Klik vervolgens op **opslaan** om de instellingen te werken.

## <a name="add-a-network-adapter"></a>Een netwerkadapter toevoegen

De sjabloon OVF implementeert de configuratieserver VM met één netwerkadapter. U kunt [een extra adapter toevoegen aan de virtuele machine)](how-to-deploy-configuration-server.md#add-an-additional-adapter), maar u moet dit doen voordat u de configuratieserver in de kluis registreren.

Als u een adapter toevoegen wilt nadat u de configuratieserver in de kluis hebt geregistreerd, moet u de adapter in de VM-eigenschappen toevoegen en vervolgens de server in de kluis te registreren.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Een configuratieserver in dezelfde kluis te registreren

Als u wilt, kunt u de configuratieserver in dezelfde kluis opnieuw registreren. f die u hebt een extra proces server-machine, naast de standaard processerver uitgevoerd op de servercomputer van de configuratie opnieuw registreren van beide computers.

  1. Open in de kluis **beheren** > **Site Recovery-infrastructuur** > **configuratieservers**.
  2. In **Servers**, klikt u op **downloaden registratiesleutel**. Hiermee downloadt u het kluisreferentiebestand.
  3. Meld u bij de configuratie van server-machine.
  4. In **%ProgramData%\ASR\home\svagent\bin**, open **cspsconfigtool.exe**.
  5. Op de **kluis registratie** tabblad, klik op Bladeren en zich het kluisreferentiebestand die u hebt gedownload.
  6. Indien nodig, bieden u proxy-server-gegevens. Klik vervolgens op **registreren**.
  7. Open een opdrachtvenster Admin PowerShell en voer de volgende opdracht:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="delete-or-unregister-a-configuration-server"></a>Verwijderen of de registratie van een configuratieserver

1. Schakel [Schakel de beveiliging](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) voor alle VM's onder de configuratieserver.
2. [Loskoppelen](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) en [verwijderen](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) alle beleidsregels voor replicatie van de configuratieserver.
3. [Verwijder](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) alle Vcenter-servers/vSphere-hosts die gekoppeld aan de configuratieserver zijn.
4. Open in de kluis, **Site Recovery-infrastructuur** > **Configuration Servers**
5. Klik op de configuratieserver die u wilt verwijderen. Klik op de **Details** pagina, klikt u op **verwijderen**.

    ![Configuratieserver verwijderen](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Verwijderen met PowerShell

U kunt eventueel de configuratieserver met behulp van PowerShell verwijderen:

1. [Installeer](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell-module
2. Meld u aan bij aan uw Azure-account met de opdracht:
    
    `Login-AzureRmAccount`
3. Selecteer het abonnement kluis:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Stel de context van de kluis:
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. De configuratieserver worden opgehaald:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. De configuratieserver verwijderen:

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> U kunt de **-Force** optie in Remove-AzureRmSiteRecoveryFabric voor het geforceerd verwijderen van de configuratieserver.
 


## <a name="renew-ssl-certificates"></a>SSL-certificaten vernieuwen

De configuratieserver heeft een ingebouwde webserver, die de activiteiten van de Mobility-service, processervers en hoofddoelservers verbonden ingedeeld. Een SSL-certificaat de webserver gebruikmaakt om clients te verifiëren. Het certificaat verloopt na drie jaar en op elk gewenst moment kan worden vernieuwd.

### <a name="check-expiry"></a>Controleer de vervaldatum

Voor implementaties van configuration server vóór mei 2016, is verloopdatum voor certificaten ingesteld op één jaar. Als u hebt is een certificaat vereist, is verlopen, gebeurt het volgende:

- Wanneer de vervaldatum is twee maanden of minder, door de service wordt gestart verzenden van meldingen in de portal en per e-mail (als u zich hebt geabonneerd op Azure Site Recovery meldingen).
- Er verschijnt een meldingsbanner op de pagina van de resource kluis. Klik op de banner voor meer informatie.
- Als u ziet een **nu bijwerken** knop, dit geeft aan dat er een aantal onderdelen in uw omgeving die nog niet zijn bijgewerkt naar 9.4.xxxx.x of hogere versies. Onderdelen worden bijgewerkt voordat u het certificaat vernieuwen. Bij oudere versies kunt u niet vernieuwen.

### <a name="renew-the-certificate"></a>Het certificaat vernieuwen

1. Open in de kluis **Site Recovery-infrastructuur** > **configuratieserver**, en klik op de vereiste configuratie-server.
2. De vervaldatum weergegeven onder **configuratie-serverstatus**
3. Klik op **certificaten vernieuwen**. 


## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor het instellen van herstel na noodgevallen van [virtuele VMware-machines](tutorial-vmware-to-azure.md) en fysieke servers(tutorial-physical-to-azure.md) naar Azure.
