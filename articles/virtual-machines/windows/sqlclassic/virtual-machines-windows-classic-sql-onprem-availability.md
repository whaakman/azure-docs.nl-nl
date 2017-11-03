---
title: On-premises altijd op beschikbaarheidsgroepen uitbreiden naar Azure | Microsoft Docs
description: Deze zelfstudie maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel en wordt beschreven hoe u de wizard Replica toevoegen in SQL Server Management Studio (SSMS) toe te voegen van de replica van een AlwaysOn-beschikbaarheidsgroep in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 50326a093adaf3558c56dfd0b38544f0e60be460
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>On-premises altijd op beschikbaarheidsgroepen uitbreiden naar Azure
Altijd op beschikbaarheidsgroepen bieden hoge beschikbaarheid voor groepen van database door secundaire replica's toe te voegen. Deze replica's staan failover wordt uitgevoerd de databases bij een fout. Ze kunnen ook worden gebruikt voor het offloaden van lezen werkbelastingen of back-uptaken.

Door een of meer virtuele Azure-machines met SQL Server-inrichting en vervolgens als replica's toevoegen aan de beschikbaarheid van lokale groepen kunt u beschikbaarheidsgroepen: lokale uitbreiden naar Microsoft Azure.

Deze zelfstudie wordt ervan uitgegaan dat u hebt het volgende:

* Een actief Azure-abonnement. U kunt [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een bestaande altijd op beschikbaarheidsgroep on-premises bevinden. Zie voor meer informatie over beschikbaarheidsgroepen [altijd op beschikbaarheidsgroepen](https://msdn.microsoft.com/library/hh510230.aspx).
* De verbinding tussen de on-premises netwerk en uw virtuele Azure-netwerk. Zie voor meer informatie over het maken van dit virtuele netwerk [een Site-naar-Site verbinding maken met de Azure portal (klassiek)](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

## <a name="add-azure-replica-wizard"></a>Wizard Azure Replica toevoegen
In deze sectie leest u hoe u de **Azure Replica Wizard toevoegen** uitbreiden van uw oplossing AlwaysOn-beschikbaarheidsgroep zodanig dat Azure replica's.

> [!IMPORTANT]
> De **Azure Replica Wizard toevoegen** biedt alleen ondersteuning voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel. Nieuwe VM-implementaties moeten het nieuwere Resource Manager-model gebruiken. Als u virtuele machines met Resource Manager, moet u de secundaire Azure replica met behulp van Transact-SQL-commmands (hier niet weergegeven) handmatig toevoegen. Deze wizard werkt niet in het Resource Manager-scenario.

1. Uit binnen SQL Server Management Studio, vouw **altijd op hoge beschikbaarheid** > **beschikbaarheidsgroepen** > **[naam van de beschikbaarheidsgroep]**.
2. Met de rechtermuisknop op **Beschikbaarheidsreplica**, klikt u vervolgens op **Replica toevoegen**.
3. Standaard de **van de Replica toevoegen aan de Wizard beschikbaarheidsgroep** wordt weergegeven. Klik op **Volgende**.  Als u hebt geselecteerd de **deze pagina niet meer weergeven** optie onder aan de pagina tijdens het starten van een vorige van deze wizard, in dit scherm wordt niet weergegeven.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. U moet verbinding maken met alle bestaande secundaire replica's. U kunt klikken op **verbinden...** naast elke replica of u kunt klikken op **verbinding maken met alle...** aan de onderkant van het scherm. Nadat de verificatie, klikt u op **volgende** om door te gaan naar het volgende scherm.
5. Op de **replica's opgeven** pagina meerdere tabbladen worden weergegeven aan de bovenkant: **replica's**, **eindpunten**, **back-up voorkeuren**, en **Listener**. Van de **replica's** tabblad **Azure Replica toevoegen...** openen van de Wizard Azure Replica toevoegen.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Selecteer een bestaand Azure-Beheercertificaat uit het lokale certificaatarchief van Windows als u een voordat hebt geïnstalleerd. Selecteer of Voer de id van een Azure-abonnement als u een voordat hebt gebruikt. U kunt klikken op downloaden om te downloaden en installeren van een Azure-Beheercertificaat en downloaden van de lijst met abonnementen op basis van een Azure-account.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. U kunt elk veld op de pagina met waarden die worden gebruikt voor het maken van de Azure virtuele Machine (VM) die als host voor de replica fungeert wordt gevuld.
   
   | Instelling | Beschrijving |
   | --- | --- |
   | **Afbeelding** |Selecteer de gewenste combinatie van het besturingssysteem en SQL Server |
   | **VM-grootte** |Selecteer de grootte van virtuele machine die het beste past bij uw bedrijfsbehoeften |
   | **VM-naam** |Geef een unieke naam voor de nieuwe virtuele machine. De naam moet tussen 3 en 15 tekens bevatten, kunnen alleen letters, cijfers en afbreekstreepjes bevatten en moet beginnen met een letter en eindigen met een letter of een cijfer. |
   | **VM-gebruikersnaam** |Geef een naam op die als het administrator-account op de virtuele machine |
   | **Beheerderswachtwoord voor VM** |Geef een wachtwoord voor het nieuwe account |
   | **Wachtwoord bevestigen** |Bevestig het wachtwoord van het nieuwe account |
   | **Virtueel netwerk** |Geef het Azure-netwerk dat de nieuwe virtuele machine moet worden gebruikt. Zie voor meer informatie over virtuele netwerken [Virtual Network-overzicht](../../../virtual-network/virtual-networks-overview.md). |
   | **Virtueel netwerksubnet** |Geef het subnet van het virtuele netwerk dat de nieuwe virtuele machine moet worden gebruikt |
   | **Domein** |Controleer de vooraf ingestelde waarde voor het domein juist |
   | **Domein-gebruikersnaam** |Geef een account dat in de lokale groep administrators op de lokale clusterknooppunten |
   | **Wachtwoord** |Geef het wachtwoord voor de domeinnaam van de gebruiker |
8. Klik op **OK** valideren van de implementatie-instellingen.
9. Juridische voorwaarden worden vervolgens weergegeven. Lees en klik op **OK** als u akkoord met deze voorwaarden gaat.
10. De **replica's opgeven** pagina opnieuw wordt weergegeven. Controleer de instellingen voor de nieuwe Azure replica op de **replica's**, **eindpunten**, en **back-up voorkeuren** tabbladen. Instellingen wijzigen om te voldoen aan uw bedrijfsvereisten.  Zie voor meer informatie over de parameters die op deze tabbladen opgenomen [pagina opgeven voor replica's (nieuwe Wizard/Add Replica Wizard beschikbaarheidsgroep)](https://msdn.microsoft.com/library/hh213088.aspx). Houd er rekening mee dat listeners kunnen niet worden gemaakt met behulp van het tabblad Listener voor beschikbaarheidsgroepen die Azure replica's bevatten. Bovendien, als er al een listener is gemaakt vóór het starten van de Wizard, ontvangt u een bericht weergegeven dat aangeeft dat deze niet wordt ondersteund in Azure. Laten we zien hoe u maakt luisteraars in de **maken van een beschikbaarheidsgroep-Listener** sectie.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. Klik op **Volgende**.
12. Selecteer de methode voor synchronisatie u gebruiken wilt op de **eerste gegevenssynchronisatie selecteren** pagina en klik op **volgende**. Selecteer voor de meeste scenario **volledige gegevenssynchronisatie**. Zie voor meer informatie over synchronisatiemethoden gegevens [Selecteer initiële synchronisatie gegevenspagina (altijd op beschikbaarheid van groep Wizards)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Bekijk de resultaten op de **validatie** pagina. Corrigeer de problemen en voer de validatie opnieuw uit indien nodig. Klik op **Volgende**.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Controleer de instellingen op de **samenvatting** pagina en klik vervolgens op **voltooien**.
15. Tijdens het inrichtingsproces begint. Wanneer de wizard voltooid is, klikt u op **sluiten** om af te sluiten buiten de wizard.

> [!NOTE]
> De Wizard Azure Replica maakt een logbestand in Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Dit logboekbestand kan worden gebruikt om op te lossen mislukte Azure replica-implementaties. Als de Wizard niet kan geen acties uitvoeren, worden alle vorige bewerkingen teruggedraaid, met inbegrip van de ingerichte virtuele machine verwijderen.
> 
> 

## <a name="create-an-availability-group-listener"></a>Maken van een beschikbaarheidsgroep-listener
Nadat de beschikbaarheidsgroep is gemaakt, moet u een listener voor clients verbinding maken met de replica's maken. Binnenkomende verbindingen naar de primaire of een secundaire replica alleen-lezen-listeners doorgestuurd. Zie voor meer informatie over listeners [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Volgende stappen
Naast het gebruik van de **Azure Replica Wizard toevoegen** om uit te breiden de AlwaysOn-beschikbaarheidsgroep naar Azure, u mogelijk ook verplaatsen sommige SQL Server-workloads volledig in Azure. Om te beginnen, Zie [inrichten van een virtuele Machine van SQL Server op Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Zie voor andere onderwerpen die betrekking hebben op SQL Server wordt uitgevoerd in Azure VM's [SQL Server op Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

