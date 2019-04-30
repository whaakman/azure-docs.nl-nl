---
title: On-premises AlwaysOn-beschikbaarheidsgroepen uitbreiden naar Azure | Microsoft Docs
description: Deze zelfstudie maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel, en wordt beschreven hoe u de wizard Replica toevoegen in SQL Server Management Studio (SSMS) om toe te voegen van de replica van een AlwaysOn-beschikbaarheidsgroep in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: d3e56f1741a9cfd3f2d9f786c2ce22eb6a946ef2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481585"
---
# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>On-premises AlwaysOn-beschikbaarheidsgroepen uitbreiden naar Azure
Always On Availability Groups bieden hoge beschikbaarheid voor groepen van database door secundaire replica's toe te voegen. Deze replica's staan Failover-overschakeling uitvoeren databases bij een storing. Ze kunnen ook worden gebruikt voor de offload van werkbelastingen voor lezen of back-uptaken.

U kunt on-premises-beschikbaarheidsgroepen uitbreiden naar Microsoft Azure door een of meer virtuele Azure-machines met SQL Server inrichten en deze vervolgens als replica's toevoegen aan uw on-premises Availability Groups.

In deze zelfstudie wordt ervan uitgegaan dat u hebt het volgende:

* Een actief Azure-abonnement. U kunt [zich registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een bestaande AlwaysOn-beschikbaarheidsgroep on-premises. Zie voor meer informatie over Availability Groups [Always On Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx).
* De verbinding tussen de on-premises netwerk en uw Azure-netwerk. Zie voor meer informatie over het maken van dit virtuele netwerk [maken van een Site-naar-Site-verbinding met de Azure portal (klassiek)](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

## <a name="add-azure-replica-wizard"></a>Wizard Azure Replica toevoegen
Deze sectie leest u hoe u de **Wizard Azure Replica toevoegen** om uit te breiden van uw oplossing AlwaysOn-beschikbaarheidsgroep zodanig dat Azure replica's.

> [!IMPORTANT]
> De **Wizard Azure Replica toevoegen** biedt alleen ondersteuning voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel. Nieuwe VM-implementaties moeten het nieuwere model van Resource Manager gebruiken. Als u virtuele machines met Resource Manager, moet u de secundaire Azure-replica met behulp van Transact-SQL-commmands (hier niet weergegeven) handmatig toevoegen. Met deze wizard werkt niet in het Resource Manager-scenario.

1. Uit SQL Server Management Studio, vouw **Always On hoge beschikbaarheid** > **beschikbaarheidsgroepen** > **[naam van de beschikbaarheidsgroep]**.
2. Met de rechtermuisknop op **Beschikbaarheidsreplica**, klikt u vervolgens op **Replica toevoegen**.
3. Standaard de **Replica toevoegen aan de Wizard beschikbaarheidsgroep** wordt weergegeven. Klik op **volgende**.  Als u hebt geselecteerd de **deze pagina niet meer weergeven** optie aan de onderkant van de pagina tijdens een eerdere starten van deze wizard, in dit scherm niet wordt weergegeven.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. U moet verbinding maken met alle bestaande secundaire replica's. U kunt klikken op **verbinden...** naast elke replica of u kunt klikken op **alle verbinden...** aan de onderkant van het scherm. Na de verificatie, klikt u op **volgende** om naar het volgende scherm te gaan.
5. Op de **replica's opgeven** pagina meerdere tabbladen worden weergegeven aan de bovenzijde: **Replica's**, **eindpunten**, **back-up van voorkeuren**, en **Listener**. Uit de **replica's** tabblad **Azure-Replica toevoegen...** starten van de Wizard Azure Replica toevoegen.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Selecteer een bestaande Azure-Beheercertificaat uit het lokale Windows-certificaatarchief als u een voordat hebt geïnstalleerd. Selecteer of Voer de id van een Azure-abonnement als u een voordat u hebt gebruikt. U kunt klikken op downloaden om te downloaden en installeren van een Azure-Beheercertificaat en downloaden van de lijst met abonnementen met een Azure-account.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. U kunt elk veld op de pagina met waarden die worden gebruikt voor het maken van de Azure Virtual Machine (VM) die als voor de replica host wordt gevuld.
   
   | Instelling | Description |
   | --- | --- |
   | **Installatiekopie** |Selecteer de gewenste combinatie van OS- en SQL Server |
   | **VM-grootte** |Selecteer de grootte van virtuele machine die het beste bij uw bedrijfsbehoeften |
   | **VM-naam** |Geef een unieke naam voor de nieuwe virtuele machine. De naam moet tussen 3 en 15 tekens bevatten, kan alleen letters, cijfers en afbreekstreepjes bevatten en moet beginnen met een letter en eindigen met een letter of een cijfer. |
   | **VM-gebruikersnaam** |Geef een gebruikersnaam die het administrator-account op de virtuele machine wordt |
   | **VM-beheerderswachtwoord** |Geef een wachtwoord op voor het nieuwe account |
   | **Wachtwoord bevestigen** |Bevestig het wachtwoord van het nieuwe account |
   | **Virtueel netwerk** |Geef het Azure-netwerk dat de nieuwe virtuele machine moet worden gebruikt. Zie voor meer informatie over virtuele netwerken [overzicht van Virtual Network](../../../virtual-network/virtual-networks-overview.md). |
   | **Virtual Network Subnet** |Geef een subnet van het virtuele netwerk dat de nieuwe virtuele machine moet worden gebruikt |
   | **Domein** |Controleer of de vooraf ingestelde waarde voor het domein juist |
   | **Domeingebruikersnaam** |Geef een account dat in de lokale beheerdersgroep op de knooppunten van het lokale |
   | **Wachtwoord** |Geef het wachtwoord voor de domeinnaam van de gebruiker |
8. Klik op **OK** om de implementatie-instellingen te valideren.
9. Juridische voorwaarden worden vervolgens weergegeven. Lees en klik op **OK** als u akkoord met deze voorwaarden gaat.
10. De **replica's opgeven** pagina opnieuw wordt weergegeven. Controleer de instellingen voor de nieuwe Azure replica op de **replica's**, **eindpunten**, en **back-up voorkeuren** tabbladen. Instellingen wijzigen om te voldoen aan uw zakelijke vereisten.  Zie voor meer informatie over de parameters die op deze tabbladen [pagina opgeven voor replica's (Wizard Nieuwe beschikbaarheidsgroep Wizard toevoegen/Replica)](https://msdn.microsoft.com/library/hh213088.aspx). Houd er rekening mee dat listeners kunnen niet worden gemaakt met behulp van het tabblad Listener voor beschikbaarheidsgroepen die Azure replica's bevatten. Als u al een listener is gemaakt vóór het starten van de Wizard, ontvangt u bovendien een bericht weergegeven dat aangeeft dat deze niet wordt ondersteund in Azure. Er wordt beschreven hoe u het maken van listeners in de **maken van een beschikbaarheidsgroep-Listener** sectie.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. Klik op **volgende**.
12. Selecteer de methode voor synchronisatie u gebruiken wilt op de **eerste gegevenssynchronisatie selecteren** pagina en klik op **volgende**. Selecteer voor de meeste scenario's, **volledige gegevenssynchronisatie**. Zie voor meer informatie over synchronisatiemethoden gegevens [initiële gegevens synchronisatie pagina selecteren (altijd op beschikbaarheid van groep Wizards)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Bekijk de resultaten op de **validatie** pagina. Openstaande problemen te corrigeren en voer de validatie opnieuw uit indien nodig. Klik op **volgende**.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Controleer de instellingen op de **samenvatting** pagina en klik vervolgens op **voltooien**.
15. Het inrichtingsproces wordt gestart. Wanneer de wizard voltooid is, klikt u op **sluiten** om buiten de wizard af te sluiten.

> [!NOTE]
> De Wizard Azure Replica wordt een logboekbestand gemaakt in Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Dit logboekbestand kan worden gebruikt om op te lossen mislukte Azure replica-implementaties. Als de Wizard is mislukt voor het uitvoeren van een actie, worden alle vorige bewerkingen teruggedraaid, met inbegrip van de ingerichte virtuele machine verwijderen.
> 
> 

## <a name="create-an-availability-group-listener"></a>Maken van een beschikbaarheidsgroep-listener
Nadat de beschikbaarheidsgroep is gemaakt, moet u een listener voor clients verbinding maken met de replica's maken. Beschikbaarheidsgroeplisteners leiden inkomende verbindingen naar de primaire of een secundaire replica alleen-lezen. Zie voor meer informatie over listeners [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Volgende stappen
Naast het gebruik van de **Wizard Azure Replica toevoegen** als u wilt de AlwaysOn-beschikbaarheidsgroep uitbreiden naar Azure, u mogelijk ook verplaatsen sommige SQL Server-workloads volledig in Azure. Als u wilt beginnen, Zie [inrichten van een SQL Server-Machine in Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Zie voor andere onderwerpen met betrekking tot SQL Server wordt uitgevoerd in Azure VM's, [SQL Server op Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

