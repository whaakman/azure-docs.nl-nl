---
title: 'Azure Portal: een SQL Managed Instance maken | Microsoft Docs'
description: Een SQL Managed Instance, een netwerkomgeving en een client-VM maken om toegang te krijgen.
keywords: zelfstudie sql-database, een sql managed instance maken
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: quickstart
ms.date: 08/13/2018
ms.author: jovanpop-msft
ms.openlocfilehash: cb378c2d2773096992ef688653fd77b2625f8754
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42023806"
---
# <a name="create-an-azure-sql-managed-instance"></a>Een Azure SQL Managed Instance maken

Deze snelstart helpt u bij het maken van een SQL Managed Instance in Azure. Azure SQL Database Managed Instance is een Platform-as-a-Service (PaaS) SQL Server Database Engine-exemplaar waarmee u maximaal beschikbare SQL Server-databases in de Azure-cloud kunt uitvoeren en schalen. In deze snelstart ziet u hoe u aan de slag gaat door een SQL Managed Instance te maken.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="prepare-network-environment"></a>De netwerkomgeving voorbereiden

SQL Managed Instance is een beveiligde service die wordt geplaatst in uw eigen Azure Virtual Network (VNet). Als u een beheerd exemplaar wilt maken, moet u de Azure-netwerkomgeving voorbereiden, waaronder:
 - Azure VNet waar het beheerde exemplaar wordt geplaatst.
 - Subnet in uw Azure VNet waar beheerde exemplaren worden geplaatst.
 - Door de gebruiker gedefinieerde route waarmee Managed Instance kan communiceren met de Azure-services die het exemplaar controleren en beheren.

Het subnet is toegewezen aan beheerde exemplaren en u kunt geen andere resources (bijvoorbeeld Azure Virtual Machines) in dat subnet maken. In deze snelstart worden twee subnetten in uw Azure VNet aangemaakt, zodat u beheerde exemplaren in het subnet kunt plaatsen dat is toegewezen aan Managed Instances, en andere resources in het standaardsubnet.

1. Implementeer een Azure-netwerkomgeving die is voorbereid voor Azure SQL Database Managed Instance door te klikken op de volgende knop:

    <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener"> <img src="http://azuredeploy.net/deploybutton.png"> </a>

    Met deze knop opent u een formulier in de Azure Portal waar u uw netwerkomgeving kunt configureren voordat u deze implementeert.

2. (Optioneel) Wijzig de namen van VNet en subnetten en pas IP-adresbereiken aan die zijn gekoppeld aan uw netwerkresources. Druk vervolgens op de knop "Kopen" om uw omgeving te maken en configureren:

    ![managed instance-omgeving maken](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

    > [!Note]
    > Deze Azure Resource Manager-implementatie maakt in het VNet twee subnetten: één voor beheerde exemplaren met de naam **ManagedInstances**, en de andere met de naam **Standaard** voor andere resources, zoals een virtuele clientmachine die kan worden gebruikt om verbinding te maken met Managed Instance. Als u geen twee subnetten nodig hebt, kunt u later de standaardversie verwijderen. In dat geval is het voor u echter niet mogelijk om stap 3 in deze snelstart te voltooien: [client-computer voorbereiden](#prepare-client-machine).

    > [!Note]
    > Als u de namen van het VNet en de subnetten wijzigt, zorg er dan voor dat u de nieuwe namen onthoudt, omdat ze vereist zijn in de volgende secties. In de rest van de zelfstudie wordt aangenomen dat u het VNet hebt gemaakt met de naam **MyNewVNet**, het **ManagedInstances**-subnet voor SQL Managed Instances en het **Standaard**-subnet voor virtuele machines en andere resources.

## <a name="create-a-managed-instance"></a>Een beheerd exemplaar maken

In de volgende stappen wordt uitgelegd hoe u een beheerd exemplaar maakt nadat u goedkeuring voor preview hebt ontvangen.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek naar **Managed Instance** en selecteer vervolgens **Azure SQL Database Managed Instance (preview)**.
3. Klik op **Create**.

   ![Beheerd exemplaar maken](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

4. Selecteer uw abonnement en controleer of er onder Preview-voorwaarden **Geaccepteerd** staat.

   ![preview-voorwaarden managed instance geaccepteerd](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Vul het formulier voor het beheerde exemplaar in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam van het beheerde exemplaar**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Beheerdersaanmeldgegevens voor het beheerde exemplaar**|Een geldige gebruikersnaam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen. Maak geen gebruik van 'serverbeheerder' aangezien dit een rol is die op serverniveau is gereserveerd.| 
   |**Wachtwoord**|Een geldig wachtwoord|Het wachtwoord moet minstens 16 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Resourcegroep**|De resourcegroep die u eerder hebt gemaakt||
   |**Locatie**|De locatie die u eerder hebt geselecteerd|Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's.|
   |**Virtueel netwerk**|Het virtuele netwerk dat u eerder hebt gemaakt| Kies het item **MyNewVNet/ManagedInstances** als u in de vorige stap niet de namen hebt gewijzigd. Kies anders de VNet-naam en het beheerde exemplaar-subnet die u hebt ingevoerd in de vorige sectie. **Gebruik het Standaard-subnet niet, omdat dit niet is geconfigureerd voor het hosten van beheerde exemplaren**. |

   ![formulier beheerd exemplaar maken](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

6. Klik op **Prijscategorie** om reken- en opslagresources toe te wijzen en de opties voor prijscategorieën te bekijken. Standaard krijgt het exemplaar 32 GB gratis opslagruimte, **maar mogelijk is dit niet voldoende voor uw toepassingen**.
7. Gebruik de schuifregelaars of tekstvakken om de hoeveelheid opslagruimte en het aantal virtuele kernen op te geven. 
   ![prijscategorie beheerde instantie](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

8. Wanneer u klaar bent, klikt u op **Toepassen** om uw selectie op te slaan.  
9. Klik op **Maken** om het beheerde exemplaar te implementeren.
10. Klik op het pictogram voor **Meldingen** om de status van de implementatie te bekijken.
 
   ![voortgang implementatie](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

11. Klik op **Implementatie wordt uitgevoerd** om het venster Beheerd exemplaar te openen. Hier kunt u details van de voortgang van de implementatie controleren.
 
   ![voortgang implementatie 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Terwijl de implementatie wordt uitgevoerd, kunt u doorgaan met de volgende procedure.

> [!IMPORTANT]
> Het eerste exemplaar in een subnet kent doorgaans een veel langere implementatietijd dan verdere exemplaren. Het kan meer dan 24 uur duren voordat de implementatie is voltooid. Annuleer de implementatiebewerking niet omdat deze langer duurt dan verwacht. De implementatieduur voor uw eerste exemplaar is een tijdelijke situatie. U kunt kort na begin van de openbare preview een aanzienlijke verbetering van de implementatietijd verwachten. Het maken van het tweede beheerde exemplaar in het subnet duurt enkele minuten.

## <a name="prepare-client-machine"></a>Client-computer voorbereiden

Omdat SQL Managed Instance in uw persoonlijke virtuele netwerk wordt geplaatst, moet u een Azure-VM maken met een geïnstalleerd SQL-clienthulpprogramma, zoals SQL Server Management Studio of SQL Operations Studio om verbinding te maken met het beheerde exemplaar en om query's uit te voeren.

> [!Note]
> In plaats van de virtuele client-machine van Azure kunt u een [Point-to-Site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)-verbinding configureren en verbinding maken met het beheerde exemplaar van de lokale computer.

De eenvoudigste manier om een virtuele client-machine te maken met alle noodzakelijke hulpprogramma's is met de Azure Resource Manager-sjablonen.

1. Klik op de volgende knop (zorg ervoor dat u bent aangemeld bij de Azure Portal in een nieuw browsertabblad):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Voer in het formulier dat wordt geopend de naam van de virtuele machine, de gebruikersnaam van de beheerder en het wachtwoord in waarmee u verbinding maakt met de virtuele machine.

    ![client-vm maken](./media/sql-database-managed-instance-get-started/create-client-sql-vm.png)

    Als u de VNet-naam en het standaardsubnet niet hebt gewijzigd, hoeft u de laatste twee parameters niet te wijzigen. Anders moet u deze waarden wijzigen naar de waarden die u hebt ingevoerd bij het instellen van de netwerkomgeving.

3. Klik op de knop "Kopen" en de virtuele Azure-machine wordt geïmplementeerd in het netwerk dat u hebt voorbereid.

4. Maak verbinding met uw virtuele machine via Extern bureaublad en zoek SQL Server Management Studio of SQL Operation Studio, die automatisch zijn geïnstalleerd op de virtuele machine.

5. Open SQL Server Management Studio en voer de **hostnaam** voor uw beheerde exemplaar in het venster **Servernaam** in, selecteer **SQL Server-verificatie**, geef uw gebruikersnaam en wachtwoord op in het dialoogvenster  **Verbinding maken met server** en klik vervolgens op **Verbinden**.

    ![ssms verbinden](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Nadat u verbinding hebt gemaakt, kunt u uw systeem en gebruikersdatabases bekijken in het knooppunt Databases. Daarnaast kunt u diverse objecten bekijken in de knooppunten Beveiliging, Server-objecten, Replicatie, Beheer, SQL Server Agent en XEvent Profiler.

## <a name="next-steps"></a>Volgende stappen

 - [Uw toepassingen verbinding laten maken met Managed Instance](sql-database-managed-instance-connect-app.md).
 - [Uw databases naar Managed Instance migreren van on-premises](sql-database-managed-instance-migrate.md).


