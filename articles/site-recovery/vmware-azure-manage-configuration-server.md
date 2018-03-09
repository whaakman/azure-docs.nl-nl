---
title: Beheren van de configuratieserver voor noodherstel van VMware met Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voor het beheren van een bestaande configuratieserver voor noodherstel van VMware naar Azure met Azure Site Recovery.
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: ddb1c9426874634fea54f1b67509d4ac2af70eba
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>De configuratieserver voor VMware-virtuele machines beheren

Instellen van een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratieserver coördineert de communicatie tussen de on-premises VMware en Azure en beheert gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de configuratieserver nadat deze geïmplementeerd.


## <a name="modify-vmware-settings"></a>VMware-instellingen wijzigen

Instellingen wijzigen voor de VMware-server waarmee de configuratieserver verbinding maakt.

1. Aanmelden bij de computer uitgevoerd van de configuratieserver.
2. Azure Site Recovery Configuration Manager start vanuit de snelkoppeling op het bureaublad. Of open [deze koppeling](https://configuration-server-name/IP:44315).
3. Selecteer **beheren vCenter-Server/vSPhere ESXi server**, en vervolgens de volgende handelingen uit:

    * Selecteer een andere VMware server koppelt u de configuratieserver, **toevoegen vCenter-Server/vSphere ESXi server**. Geef de Serverdetails.

    * Selecteer voor het bijwerken van de referenties waarmee verbinding met de VMware-server voor automatische detectie van virtuele VMware-machines **bewerken**. Geef de nieuwe referenties op en selecteer vervolgens **OK**.

    ![VMware wijzigen](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Referenties voor de installatie van de Mobility-Service wijzigen

Wijzig de referenties voor de Mobility-Service automatisch worden geïnstalleerd op de VMware-machines dat u voor replicatie inschakelt.

1. Aanmelden bij de computer uitgevoerd van de configuratieserver.
2. Site Recovery Configuration Manager start vanuit de snelkoppeling op het bureaublad. Of open [deze koppeling](https://configuration-server-name/IP:44315).
3. Selecteer **referenties van de virtuele machine beheren**, en geef nieuwe referenties. Selecteer vervolgens **OK** om de instellingen te werken.

    ![Referenties van de Mobility-Service wijzigen](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Proxy-instellingen wijzigen

Wijzig de proxyinstellingen die worden gebruikt door de server-machine van de configuratie voor toegang tot het internet naar Azure. Als u een machine proces server naast de standaard processerver uitgevoerd op de servercomputer van de configuratie hebt, kunt u de instellingen op beide computers wijzigen.

1. Aanmelden bij de computer uitgevoerd van de configuratieserver.
2. Site Recovery Configuration Manager start vanuit de snelkoppeling op het bureaublad. Of open [deze koppeling](https://configuration-server-name/IP:44315).
3. Selecteer **connectiviteit beheren**, en werk de proxy-waarden. Selecteer vervolgens **opslaan** om de instellingen te werken.

## <a name="add-a-network-adapter"></a>Een netwerkadapter toevoegen

De sjabloon Open Virtualization Format (OVF) implementeert de configuratieserver VM met één netwerkadapter. U kunt [een extra adapter toevoegen aan de virtuele machine)](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), maar u moet het toe te voegen voordat u de configuratieserver in de kluis registreren.

Toevoegen als een adapter toevoegen nadat u de configuratieserver in de kluis registreren, de adapter in de VM-eigenschappen. De server in de kluis vervolgens opnieuw te registreren.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Een configuratieserver in dezelfde kluis te registreren

Als u wilt, kunt u de configuratieserver in dezelfde kluis opnieuw registreren. Als u hebt een extra proces server-machine, naast de standaard processerver uitgevoerd op de servercomputer van de configuratie opnieuw registreren beide computers.


  1. Open in de kluis **beheren** > **Site Recovery-infrastructuur** > **configuratieservers**.
  2. In **Servers**, selecteer **downloaden registratiesleutel** het kluisreferentiebestand te downloaden.
  3. Aanmelden bij de configuratie van server-machine.
  4. In **%ProgramData%\ASR\home\svagent\bin**, open **cspsconfigtool.exe**.
  5. Op de **kluis registratie** tabblad **Bladeren** en zoek het kluisreferentiebestand die u hebt gedownload.
  6. Indien nodig, bieden u proxy-server-gegevens. Selecteer vervolgens **Registreren**.
  7. Open een PowerShell-opdrachtvenster beheerder en voer de volgende opdracht:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Upgrade de configuratieserver

Updatepakketten voor het bijwerken van de configuratieserver worden uitgevoerd. Updates kunnen worden toegepast voor maximaal N-4 versies. Bijvoorbeeld:

- Als u 9.7, 9,8, 9,9 of 9.10 uitvoert, kunt u rechtstreeks naar 9.11 bijwerken.
- Als u 9,6 of eerder uitvoert en u wilt upgraden naar 9.11, moet u eerst upgraden naar versie 9.7. voordat u 9.11.

Koppelingen naar de updatepakketten voor het upgraden van alle versies van de configuratieserver zijn beschikbaar in de [wiki updates pagina](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

De server als volgt bijwerken:

1. Download het installatiebestand van de update voor de configuratieserver.
2. Dubbelklik erop om het installatieprogramma uitvoert.
3. Het installatieprogramma detecteert de huidige versie op de machine.
4. Selecteer **OK** om te bevestigen en de upgrade uitvoert. 


## <a name="delete-or-unregister-a-configuration-server"></a>Verwijderen of de registratie van een configuratieserver

1. Schakel [Schakel de beveiliging](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) voor alle VM's onder de configuratieserver.
2. [Loskoppelen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) en [verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) alle beleidsregels voor replicatie van de configuratieserver.
3. [Verwijder](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) alle vCenter-servers/vSphere-hosts die gekoppeld aan de configuratieserver zijn.
4. Open in de kluis **Site Recovery-infrastructuur** > **configuratieservers**.
5. Selecteer de configuratieserver die u wilt verwijderen. Klik op de **Details** pagina **verwijderen**.

    ![Configuratieserver verwijderen](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Verwijderen met PowerShell

U kunt eventueel de configuratieserver verwijderen met behulp van PowerShell.

1. [Installeer](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) de Azure PowerShell-module.
2. Meld u aan bij uw Azure-account met deze opdracht:
    
    `Login-AzureRmAccount`
3. Selecteer het abonnement van de kluis.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Hiermee stelt u de context van de kluis.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. De configuratieserver worden opgehaald.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Verwijder de configuratieserver.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> U kunt de **-Force** optie in Remove-AzureRmSiteRecoveryFabric voor het geforceerd verwijderen van de configuratieserver.
 


## <a name="renew-ssl-certificates"></a>SSL-certificaten vernieuwen

De configuratieserver heeft een ingebouwde webserver, die de activiteiten van de Mobility-Service, processervers en hoofddoelservers verbonden ingedeeld. Een SSL-certificaat de webserver gebruikmaakt om clients te verifiëren. Het certificaat verloopt na drie jaar en op elk gewenst moment kan worden vernieuwd.

### <a name="check-expiry"></a>Controleer de vervaldatum

Voor implementaties van configuration server vóór mei 2016, is verloopdatum voor certificaten ingesteld op één jaar. Als u een certificaat dat verloopt hebben, gebeurt het volgende:

- Wanneer de vervaldatum is twee maanden of minder, door de service wordt gestart verzenden van meldingen in de portal en per e-mail (als u zich hebt geabonneerd op Site Recovery meldingen).
- Er verschijnt een meldingsbanner op de pagina van de resource kluis. Selecteer de banner voor meer informatie.
- Als u ziet een **nu bijwerken** knop, betekent dit dat sommige onderdelen in uw omgeving nog niet hebt bijgewerkt naar 9.4.xxxx.x of hogere versies. De onderdelen upgraden voordat u het certificaat vernieuwen. Bij oudere versies kunt u niet vernieuwen.

### <a name="renew-the-certificate"></a>Het certificaat vernieuwen

1. Open in de kluis **Site Recovery-infrastructuur** > **configuratieserver**. Selecteer de vereiste configuratie-server.
2. De vervaldatum weergegeven onder **configuratieserver health**.
3. Selecteer **certificaten vernieuwen**. 


## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor het instellen van herstel na noodgevallen van [virtuele VMware-machines](vmware-azure-tutorial.md) naar Azure.
