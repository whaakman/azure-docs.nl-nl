---
title: Een virtuele SQL Server-machine voor Windows maken in Azure Portal | Microsoft Docs
description: Deze zelfstudie laat zien hoe u een virtuele SQL Server 2017-machine voor Windows in Azure Portal kunt maken.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c9e62cf3dfc259d7064be9fd6b80cad0140336eb
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549266"
---
# <a name="quickstart-create-a-sql-server-2017-windows-virtual-machine-in-the-azure-portal"></a>Quickstart: Een virtuele SQL Server 2017-machine voor Windows maken in Azure Portal

> [!div class="op_single_selector"]
> * [Windows](quickstart-sql-vm-create-portal.md)
> * [Linux](../../linux/sql/provision-sql-server-linux-virtual-machine.md)

In deze quickstart gaat u een virtuele SQL Server-machine maken in Azure Portal.


  > [!TIP]
  > - Deze snelstartgids biedt een pad voor het snel inrichten van en verbinding maken met een SQL-VM. Bekijk [Provisioning guide for Windows SQL Server VMs in the Azure portal](virtual-machines-windows-portal-sql-server-provision.md) (Inrichtingshandleiding voor Windows SQL Server-VM's in Azure Portal) voor andere inrichtingskeuzes voor SQL-VM's.
  > - Als u vragen hebt over virtuele machines met SQL Server, raadpleegt u [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).

## <a id="subscription"></a> Een Azure-abonnement nemen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a id="select"></a> Een installatiekopie selecteren voor uw virtuele SQL Server-machine

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met uw account.

1. Selecteer in Azure Portal **Een resource maken**. 

1. Typ in het zoekveld `SQL Server 2017 Developer on Windows Server 2016`, en druk op ENTER.

1. Selecteer de installatiekopie met de naam **Gratis licentie voor SQL Server: SQL Server 2017 Developer op Windows Server 2016**. In deze zelfstudie maken we gebruik van de Developer Edition omdat deze een complete versie van de SQL Server is die gratis gebruikt kan worden voor ontwikkelings-/testdoeleinden. U betaalt alleen voor de kosten van het uitvoeren van de virtuele machine. Raadpleeg [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor virtuele SQL Server-machines in Azure) voor uitgebreide prijsinformatie.

   ![Nieuw zoekvenster](./media/quickstart-sql-vm-create-portal/newsearch.png)


1. Selecteer **Maken**.

## <a id="configure"></a> Algemene gegevens opgeven

Op de **basisbeginselen** tabblad, geef de volgende informatie:

1. In de **projectdetails** sectie, selecteer uw Azure-abonnement en selecteer vervolgens **nieuw** om een nieuwe resourcegroep te maken. Type _SQLVM-RG_ voor de naam.

   ![Abonnement](media/quickstart-sql-vm-create-portal/basics-project-details.png)

1. Onder **exemplaar details**:
    1. Type _SQLVM_ voor de **virtuele-machinenaam**. 
    1. Kies een locatie voor uw **regio**. 
    1. Omwille van deze snelstartgids laat **Beschikbaarheidsopties** ingesteld op _geen infrastructuur redundantie vereist_. Zie voor meer informatie over opties voor beschikbaarheid, [Azure-regio's en beschikbaarheid](../../windows/regions-and-availability.md). 
    1. In de **installatiekopie** in de lijst met _gratis licentie voor SQL Server: SQL Server 2017 Developer op WindowsServer 2016_. 
    1. Ervoor kiezen om te **grootte wijzigen** voor de **grootte** van de virtuele machine en selecteer de **A2 Basic** bieden. Zorg ervoor dat uw resources opschonen wanneer u klaar bent met eventuele onverwachte kosten voorkomen. 

   ![Exemplaardetails](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

1. Onder **Administrator-account**, Geef een gebruikersnaam, zoals _azureuser_ en een wachtwoord. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

   ![Administrator-account](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

1. Onder **regels voor binnenkomende poort**, kiest u **geselecteerde poorten toestaat** en selecteer vervolgens **RDP (3389)** in de vervolgkeuzelijst. 

   ![Regels voor binnenkomende poort](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)

## <a name="sql-server-settings"></a>SQL Server-instellingen

Op de **SQL Server-instellingen** tabblad, configureer dan de volgende opties:

1. Onder **beveiligings- en netwerken**, selecteer _openbaar (Internet_) voor **SQL-verbinding** en wijzig de poort op `1401` om te voorkomen dat met behulp van een bekende poortnummer in het openbare scenario. 
1. Onder **SQL-verificatie**, selecteer **inschakelen**. De SQL-aanmeldingsgegevens zijn ingesteld op dezelfde combinatie van gebruikersnaam en wachtwoord als u voor de virtuele machine hebt geconfigureerd. Gebruik de standaardinstellingen voor **Azure Key Vault-integratie** en **opslagconfiguratie**.  

   ![SQL server-beveiligingsinstellingen](media/quickstart-sql-vm-create-portal/sql-server-settings.png)

1. Alle andere instellingen desgewenst wijzigen en selecteer vervolgens **revisie + maken**. 

   ![Beoordelen en maken](media/quickstart-sql-vm-create-portal/review-create.png)


## <a name="create-the-sql-server-vm"></a>De virtuele SQL Server-machine maken

Op de **bekijken + maken** tabblad, controleert u de samenvatting en selecteert u **maken** om SQL Server, resourcegroep en resources die zijn opgegeven voor deze virtuele machine te maken.

U kunt de implementatie bewaken vanuit Azure Portal. Met de knop **Meldingen** boven aan het scherm kunt u de algemene status van de implementatie weergeven. Implementatie kan enkele minuten duren. 

## <a name="connect-to-sql-server"></a>Verbinding maken met SQL Server

1. Zoek in de portal de **openbaar IP-adres** van uw SQL Server-machine in de **overzicht** sectie van de eigenschappen van de virtuele machine.

1. Open op een andere computer die is verbonden met Internet, [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms).


1. Bewerk in het dialoogvenster **Verbinding maken met server** of **Verbinding maken met Database-engine** de waarde voor **Servernaam**. Typ het openbare IP-adres van de virtuele machine. Typ nu een komma en vervolgens **1401**. Dit is het aangepaste poortnummer dat u hebt opgegeven tijdens de configuratie van de nieuwe virtuele machine. Bijvoorbeeld `11.22.33.444,1401`.

1. Kies in het vak **Verificatie** **SQL Server-verificatie**.

1. Typ in het vak **Aanmelden** een geldige SQL-aanmeldingsnaam.

1. Typ in het vak **Wachtwoord** het wachtwoord van de aanmelding.

1. Selecteer **Verbinden**.

    ![ssms verbinden](./media/quickstart-sql-vm-create-portal/ssms-connect.png)

## <a id="remotedesktop"></a> Vanaf een externe locatie aanmelden bij de virtuele machine

Doorloop de volgende stappen om via Extern bureaublad verbinding te maken met de virtuele SQL Server-machine:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Nadat u verbinding hebt gemaakt met de virtuele SQL Server-machine, kunt u SQL Server Management Studio starten en verbinding maken met Windows-verificatie met behulp van de lokale beheerdersreferenties. Als u SQL Server-verificatie inschakelt, kunt u ook verbinding maken met SQL-verificatie. Dit kan met behulp van de SQL-aanmeldingsnaam en het wachtwoord die u hebt geconfigureerd tijdens het inrichten.

Met toegang tot de machine kunt u rechtstreeks de instellingen voor de machine en de SQL Server wijzigen op basis van uw vereisten. U kunt bijvoorbeeld de firewallinstellingen configureren of de SQL Server-configuratie-instellingen wijzigen.

## <a name="clean-up-resources"></a>Resources opschonen

Als het niet nodig is dat de virtuele SQL-machine continu wordt uitgevoerd, kunt u overbodige kosten voorkomen door de virtuele machine te stoppen wanneer deze niet in gebruik is. U kunt ook alle resources die aan de virtuele machine zijn gekoppeld, definitief verwijderen. Dit doet u door in de portal de gekoppelde resourcegroep te verwijderen. Hiermee verwijdert u ook de virtuele machine zelf definitief. Bedenk dus van tevoren goed of u dit wel wilt. Raadpleeg [Manage Azure resources through portal](../../../azure-resource-manager/manage-resource-groups-portal.md) (Azure-resources via de portal beheren) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een SQL Server 2017-machine in Azure portal gemaakt. Raadpleeg het volgende artikel voor meer informatie over hoe u uw gegevens migreert naar de nieuwe SQL-server.

> [!div class="nextstepaction"]
> [Een database migreren naar een SQL-VM](virtual-machines-windows-migrate-sql.md)
