---
title: 'Azure Portal: een beheerd exemplaar voor SQL Database maken | Microsoft Docs'
description: Maak een beheerd exemplaar voor Azure SQL Database in een VNet en gebruik SQL Server Management Studio om een back-up van de Wide World Importers-database te herstellen.
keywords: zelfstudie sql database, een beheerd exemplaar voor sql database maken
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 03/14/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: 774a761465cfd886b85378a35dd43ac656a7ee48
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="create-an-azure-sql-database-managed-instance-in-the-azure-portal"></a>Een beheerd exemplaar voor Azure SQL Database maken in Azure Portal

In deze zelfstudie wordt uitgelegd hoe u in Azure Portal een beheerd exemplaar voor Azure SQL Database (preview) maakt in een toegewijd subnet van een virtueel netwerk (VNet). Vervolgens wordt uitgelegd hoe u verbinding maakt met het beheerde exemplaar via SQL Server Management Studio op een virtuele machine in hetzelfde VNet en een back-up herstelt van een database die is opgeslagen in Azure-blobopslag in het beheerde exemplaar.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!IMPORTANT]
> Zie [Uw databases migreren naar een volledig beheerde service met Azure SQL Database Managed Instance](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/) voor een lijst met regio's waarin MI momenteel beschikbaar is.
 
## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/#create/Microsoft.SQLManagedInstance).

## <a name="whitelist-your-subscription"></a>Uw abonnement aan de whitelist toevoegen

Managed Instance wordt oorspronkelijk beschikbaar gemaakt als gated openbare preview waarvoor uw abonnement in de whitelist moet zijn opgenomen. Als uw abonnement nog niet in de whitelist is opgenomen, volgt u onderstaande stappen om de preview-voorwaarden te bekijken en te accepteren, en vervolgens opname in de whitelist aan te vragen.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek naar **Managed Instance** en selecteer vervolgens **Azure SQL Database Managed Instance (preview)**.
3. Klik op **Create**.

   ![beheerd exemplaar maken](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Selecteer uw abonnement, klik op **Preview-voorwaarden** en geef vervolgens de gevraagde informatie op.

   ![preview-voorwaarden managed instance](./media/sql-database-managed-instance-tutorial/preview-terms.png)

5. Na voltooiing klikt u op **OK**.

   ![preview-voorwaarden managed instance](./media/sql-database-managed-instance-tutorial/preview-approval-pending.png)

   > [!NOTE]
   > Terwijl u wacht op preview-goedkeuring, kunt u de volgende gedeelten van deze zelfstudie alvast voltooien.

## <a name="configure-a-virtual-network-vnet"></a>Een virtueel netwerk (VNet) configureren

In de volgende stappen wordt uitgelegd hoe u een nieuw virtueel netwerk (VNet) maakt in [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) dat u kunt gebruiken voor uw beheerde exemplaar. Lees [Managed Instance VNet Configuration](sql-database-managed-instance-vnet-configuration.md) (VNet-configuratie voor beheerd exemplaar) voor meer informatie over VNet-configuratie.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek en klik op **Virtueel netwerk**, controleer of **Resource Manager** is geselecteerd als implementatiemodel en klik vervolgens op **Maken**.

   ![virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Vul het formulier voor het virtuele netwerk in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Adresruimte**|Een geldig adresbereik, bijvoorbeeld 10.14.0.0/24|De adresnaam van het virtuele netwerk in CIDR-notatie.|
   |**Abonnement**|Uw abonnement|Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen.|
   |**Resourcegroep**|Een geldige resourcegroep (nieuw of bestaand)|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Locatie**|Een geldige locatie| Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's.|
   |**Subnetnaam**|Een geldige subnetnaam, bijvoorbeeld mi_subnet|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Subnetadresbereik**|Een geldig subnetadres, bijvoorbeeld 10.14.0.0/28. Gebruik voor het subnet een adresruimte die kleiner is dan de adresruimte zelf, zodat er ruimte overblijft om andere subnetten in hetzelfde VNet te maken. Denk bijvoorbeeld aan een subnet voor het hosten van test-/client-apps of gatewaysubnetten om verbinding te maken via on-premises of andere virtuele netwerken.|Het adresbereik van het subnet in CIDR-notatie. Dit moet deel uitmaken van de adresruimte van het virtuele netwerk.|
   |**Service-eindpunten**|Uitgeschakeld|Schakel één of meer service-eindpunten in voor dit subnet.|
   ||||

   ![formulier virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Klik op **Create**.

## <a name="create-new-route-table-and-a-route"></a>Een nieuwe routetabel en route maken

In de volgende stappen wordt uitgelegd hoe u de route 0.0.0.0/0 Next Hop Internet maakt.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek en klik op **Routetabel** en klik vervolgens op de pagina Routetabel op **Maken**. 

   ![routetabel maken](./media/sql-database-managed-instance-tutorial/route-table-create.png)

3. Vul het formulier voor de routetabel in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Abonnement**|Uw abonnement|Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen.|
   |**Resourcegroep**|Selecteer de resourcegroep die u in de vorige procedure hebt gemaakt|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Locatie**|Selecteer de locatie die u in de vorige procedure hebt opgegeven| Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's.|
   |**Doorgave van BCP-route uitschakelen**|Uitgeschakeld||
   ||||

   ![formulier routetabel maken](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

4. Klik op **Create**.
5. Nadat de routetabel is gemaakt, opent u de zojuist gemaakte routetabel.

   ![routetabel](./media/sql-database-managed-instance-tutorial/route-table.png)

6. Klik op **Routes** en klik vervolgens op **Toevoegen**.

   ![toevoegen aan routetabel](./media/sql-database-managed-instance-tutorial/route-table-add.png)

7.  Voeg de route **0.0.0.0/0 Next Hop Internet** toe als **enige** route met behulp van de gegevens uit onderstaande tabel:

    | Instelling| Voorgestelde waarde | Beschrijving |
    | ------ | --------------- | ----------- |
    |**Routenaam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
    |**Adresvoorvoegsel**|0.0.0.0/0|Het IP-doeladres in CIDR-notatie waarop deze route van toepassing is.|
    |**Volgend hoptype**|Internet|De volgende hop verwerkt overeenkomende pakketten voor deze route.|
    |||

    ![route](./media/sql-database-managed-instance-tutorial/route.png)

8. Klik op **OK**.

## <a name="to-apply-the-route-table-to-the-managed-instance-subnet"></a>De routetabel toepassen op het subnet voor het beheerde exemplaar

In de volgende stappen wordt uitgelegd hoe u de nieuwe routetabel instelt op het subnet voor het beheerde exemplaar.

1. Als u de routetabel wilt instellen op het subnet voor het beheerde exemplaar opent u het virtuele netwerk dat u eerder hebt gemaakt.
2. Klik op **Subnetten** en klik vervolgens op het subnet voor het beheerde exemplaar (**mi_subnet** in onderstaande schermopname).

    ![subnet](./media/sql-database-managed-instance-tutorial/subnet.png)

11. Klik op **Routetabel** en selecteer vervolgens **myMI_route_tabel**.

    ![routetabel instellen](./media/sql-database-managed-instance-tutorial/set-route-table.png)

12. Klik op **Opslaan**.

    ![instelling routetabel opslaan](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)

## <a name="create-a-managed-instance"></a>Een beheerd exemplaar maken

In de volgende stappen wordt uitgelegd hoe u een beheerd exemplaar maakt nadat u goedkeuring voor preview hebt ontvangen.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek naar **Managed Instance** en selecteer vervolgens **Azure SQL Database Managed Instance (preview)**.
3. Klik op **Create**.

   ![beheerd exemplaar maken](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Selecteer uw abonnement en controleer of er onder Preview-voorwaarden **Geaccepteerd** staat.

   ![preview-voorwaarden managed instance geaccepteerd](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

4. Vul het formulier voor het beheerde exemplaar in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam van het beheerde exemplaar**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Beheerdersaanmeldgegevens voor het beheerde exemplaar**|Een geldige gebruikersnaam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.| 
   |**Wachtwoord**|Een geldig wachtwoord|Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Resourcegroep**|De resourcegroep die u eerder hebt gemaakt||
   |**Locatie**|De locatie die u eerder hebt geselecteerd|Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's.|
   |**Virtueel netwerk**|Het virtuele netwerk dat u eerder hebt gemaakt|

   ![formulier beheerd exemplaar maken](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

5. Klik op **Prijscategorie** om reken- en opslagresources toe te wijzen en de opties voor prijscategorieën te bekijken. Standaard krijgt het exemplaar 32 GB gratis opslagruimte, maar mogelijk is dit niet voldoende voor uw toepassingen.
6. Gebruik de schuifregelaars of tekstvakken om de hoeveelheid opslagruimte en het aantal virtuele kernen op te geven. 
   ![formulier beheerd exemplaar maken](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

7. Wanneer u klaar bent, klikt u op **Toepassen** om uw selectie op te slaan.  
8. Klik op **Maken** om het beheerde exemplaar te implementeren.
9. Klik op het pictogram voor **Meldingen** om de status van de implementatie te bekijken.
 
   ![voortgang implementatie](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

9. Klik op **Implementatie wordt uitgevoerd** om het venster Beheerd exemplaar te openen. Hier kunt u details van de voortgang van de implementatie controleren.
 
   ![voortgang implementatie 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Terwijl de implementatie wordt uitgevoerd, kunt u doorgaan met de volgende procedure.

> [!IMPORTANT]
> Het eerste exemplaar in een subnet kent doorgaans een veel langere implementatietijd dan verdere exemplaren. Het kan meer dan 24 uur duren voordat de implementatie is voltooid. Annuleer de implementatiebewerking niet omdat deze langer duurt dan verwacht. De implementatieduur voor uw eerste exemplaar is een tijdelijke situatie. U kunt kort na begin van de openbare preview een aanzienlijke verbetering van de implementatietijd verwachten.

## <a name="create-a-new-subnet-in-the-vnet-for-a-virtual-machine"></a>Een nieuw subnet in het VNet maken voor een virtuele machine

In de volgende stappen wordt uitgelegd hoe u een tweede subnet in het VNet maakt voor een virtuele machine waarin u SQL Server Management Studio installeert en verbinding maakt met uw beheerde exemplaar.

1. Open uw virtuele netwerk-resource.
 
   ![VNet](./media/sql-database-managed-instance-tutorial/vnet.png)

2. Klik op **Subnetten** en klik vervolgens op **+Subnet**.
 
   ![subnet toevoegen](./media/sql-database-managed-instance-tutorial/add-subnet.png)

3. Vul het formulier voor het subnet in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Adresbereik (CIDR-blok)**|Een geldig adresbereik binnen het VNet (gebruik de standaardinstelling)||
   |**Netwerkbeveiligingsgroep**|Geen||
   |**Routetabel**|None||
   |**Service-eindpunten**|None||

   ![subnetdetails vm](./media/sql-database-managed-instance-tutorial/vm-subnet-details.png)

4. Klik op **OK**.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Een virtuele machine maken in het nieuwe subnet in het VNet

In de volgende stappen wordt uitgelegd hoe u een virtuele machine maakt in hetzelfde VNet waarin het beheerde exemplaar wordt gemaakt. 

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Selecteer **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** of **Windows 10**. In dit gedeelte van de zelfstudie wordt Windows Server gebruikt. De configuratie voor Windows 10 is sterk vergelijkbaar. 

   ![compute](./media/sql-database-managed-instance-tutorial/compute.png)

3. Vul het formulier voor de virtuele machine in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   | **Type VM-schijf**|SSD|SSD's bieden de beste balans tussen prijs en prestaties.|   
   |**Gebruikersnaam**|Een geldige gebruikersnaam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.| 
   |**Wachtwoord**|Een geldig wachtwoord|Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).| 
   |**Abonnement**|Uw abonnement|Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen.|
   |**Resourcegroep**|De resourcegroep die u eerder hebt gemaakt||
   |**Locatie**|De locatie die u eerder hebt geselecteerd||
   |**Hebt u al een Windows Server-licentie?**|Nee|Als u over Windows Server-licenties met actieve Software Assurance (SA) beschikt, kunt u met Azure Hybrid Benefit kosten besparen.|
   ||||

   ![formulier virtuele machine maken](./media/sql-database-managed-instance-tutorial/virtual-machine-create-form.png)

3. Klik op **OK**.
4. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Voor deze zelfstudie hebt u slechts een kleine virtuele machine nodig.

    ![Formaten van virtuele machines](./media/sql-database-managed-instance-tutorial/virtual-machine-size.png)  

5. Klik op **Selecteren**.
6. Klik op het formulier **Instellingen** op **Subnet** en selecteer vervolgens **vm_subnet**. Kies niet het subnet waarin het beheerde exemplaar is ingericht, maar een ander subnet in hetzelfde VNet.

    ![instellingen VM](./media/sql-database-managed-instance-tutorial/virtual-machine-settings.png)  

7. Klik op **OK**.
8. Bekijk op de samenvattingspagina de details van de aanbieding en klik vervolgens op **Maken** om te beginnen met de implementatie van de virtuele machine.
 
## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

In de volgende stappen wordt uitgelegd hoe u verbinding maakt met uw nieuwe virtuele machine via verbinding met een extern bureaublad.

1. Nadat de implementatie is voltooid, gaat u naar de virtuele machine-resource.

    ![VM](./media/sql-database-managed-instance-tutorial/vm.png)  

2. Klik op de knop **Verbinden** in de eigenschappen van de virtuele machine. Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload.
3. Open het gedownloade RDP-bestand om verbinding met de VM te maken. 
4. Wanneer u hierom wordt gevraagd, klikt u op **Verbinden**. Op een Mac hebt u een RDP-client nodig, zoals deze [Extern-bureaubladclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) uit de Mac App Store.

5. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine en klik vervolgens op **OK**.

6. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

U bent verbonden met de virtuele machine in het dashboard Serverbeheer.

> [!IMPORTANT]
> Ga niet verder voordat het beheerde exemplaar is ingericht. Nadat het is ingericht, haalt u de hostnaam voor uw exemplaar op in het veld **Beheerd exemplaar** op het tabblad **Overzicht** van uw beheerde exemplaar. De naam lijkt op het volgende: **drfadfadsfd.tr23.westus1-a.worker.database.windows.net**.

## <a name="install-ssms-and-connect-to-the-managed-instance"></a>SQL Server Management Studio installeren en verbinding met het beheerde exemplaar maken

In de volgende stappen wordt uitgelegd hoe u SQL Server Management Studio downloadt en installeert en vervolgens verbinding maakt met uw beheerde exemplaar.

1. Klik in Serverbeheer in het linkerdeelvenster op **Lokale server**.

    ![eigenschappen serverbeheer](./media/sql-database-managed-instance-tutorial/server-manager-properties.png)  

2. Klik in het deelvenster **Eigenschappen** op **Aan** om de verbeterde beveiliging van IE te wijzigen.
3. In het dialoogvenster **Verbeterde beveiliging van Internet Explorer** klikt u in het gedeelte Beheerders op **Uit** en vervolgens op **OK**.

    ![verbeterde beveiliging van internet explorer](./media/sql-database-managed-instance-tutorial/internet-explorer-security-configuration.png)  
4. Open **Internet Explorer** via de taakbalk.
5. Selecteer **Aanbevolen instellingen voor beveiliging en compatibiliteit gebruiken** en klik vervolgens op **OK** om de instelling van Internet Explorer 11 te voltooien.
6. Voer https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms in het URL-adresvak in en druk op **Enter**. 
7. Download de meest recente versie van SQL Server Management Studio en klik op **Uitvoeren** wanneer u daarom wordt gevraagd.
8. Wanneer u daarom wordt gevraagd, klikt u op **Installeren** om de installatie te starten.
9. Wanneer de installatie is voltooid, klikt u op **Sluiten**.
10. Open SQL Server Management Studio.
11. In het dialoogvenster **Verbinding maken met server** geeft u de **hostnaam* voor uw beheerde exemplaar op in het vak **Servernaam**. Vervolgens selecteert u **SQL Server-verificatie**, voert u uw gebruikersnaam en wachtwoord in en klikt u op **Verbinding maken**.

    ![ssms verbinden](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Nadat u verbinding hebt gemaakt, kunt u uw systeem en gebruikersdatabases bekijken in het knooppunt Databases. Daarnaast kunt u diverse objecten bekijken in de knooppunten Beveiliging, Server-objecten, Replicatie, Beheer, SQL Server Agent en XEvent Profiler.

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Het standaard back-upbestand van Wide World Importers downloaden

Volg deze stappen om het standaard back-upbestand van Wide World Importers te downloaden.

Wanneer u Internet Explorer gebruikt, voer dan https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak in het URL-adresvak in en klik desgevraagd op **Opslaan** om dit bestand op te slaan in de map **Downloads**.

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Een Azure-opslagaccount maken en het back-upbestand uploaden

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek naar **opslag** en klik vervolgens op **Opslagaccount** om het formulier voor opslagaccounts te openen.

   ![opslagaccount](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Vul het formulier voor het opslagaccount in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Implementatiemodel**|Resourcemodel||
   |**Type account**|Blob Storage||
   |**Prestaties**|Standard of Premium|Magnetische schijven of SSD's|
   |**Replicatie**|Lokaal redundante opslag||
   |**Opslaglaag (standaard)|Statisch of dynamisch||
   |**Veilige overdracht vereist**|Uitgeschakeld||
   |**Abonnement**|Uw abonnement|Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen.|
   |**Resourcegroep**|De resourcegroep die u eerder hebt gemaakt|| 
   |**Locatie**|De locatie die u eerder hebt geselecteerd||
   |**Virtuele netwerken**|Uitgeschakeld||

4. Klik op **Create**.

   ![details opslagaccount](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Nadat de implementatie van het opslagaccount is voltooid, opent u het nieuwe opslagaccount.
6. Klik onder **Instellingen** op **Shared Access Signature** om het formulier voor Shared Access Signature (SAS) te openen.

   ![sas-formulier](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. Op het SAS-formulier kunt u de standaardwaarden naar wens aanpassen. Houd er rekening mee dat de vervaldatum/-tijd standaard is ingesteld op slechts 8 uur.
8. Klik op **SAS genereren**.

   ![sas-formulier voltooid](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. Kopieer de **SAS-token** en de **SAS-URL van de Blob-server** en sla deze op.
10. Klik onder **Instellingen** op **Containers**.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. Klik op **+Container** om een container voor uw back-upbestand te maken.
12. Vul het formulier voor de container in met behulp van de gegevens in onderstaande tabel:

    | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Openbaar toegangsniveau**|Privé (geen anonieme toegang)||

    ![containerdetails](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. Klik op **OK**.
14. Nadat de container is gemaakt, opent u de container.

    ![container](./media/sql-database-managed-instance-tutorial/container.png)

15. Klik op **Containereigenschappen** en kopieer de URL vervolgens naar de container.

    ![URL voor container](./media/sql-database-managed-instance-tutorial/container-url.png)

16. Klik op **Uploaden** om het formulier **Blob uploaden** te openen

    ![uploaden](./media/sql-database-managed-instance-tutorial/upload.png)

17. Blader naar uw downloadmap en selecteer het bestand **AdventureWorks2016.bak**.

    ![uploaden](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. Klik op **Uploaden**.
19. Ga niet verder voordat het uploaden is voltooid.

    ![uploaden voltooid](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>De Wide World Importers-database herstellen op basis van een back-upbestand

Volg deze stappen om in SQL Server Management Studio de Wide World Importers-database te herstellen naar uw beheerde exemplaar op basis van het back-upbestand.

1. Open een nieuw queryvenster in SQL Server Management Studio.
2. Gebruik het volgende script om SAS-referenties te maken. Geef daarbij de URL voor de container van het opslagaccount en de SAS-sleutel op, zoals aangegeven.

   ```
CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
, SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![referenties](./media/sql-database-managed-instance-tutorial/credential.png)

3. Gebruik het volgende script om de SAS-referenties en geldigheid van de back-up te controleren. Geef daarbij de URL voor de container met het back-upbestand op:

   ```
   RESTORE FILELISTONLY FROM URL = 
   'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![lijst met bestanden](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Gebruik het volgende script om de Adventure Works 2012-database te herstellen op basis van een back-upbestand. Geef daarbij de URL voor de container met het back-upbestand op:

   ```
   RESTORE DATABASE [Wide World Importers] FROM URL =
  'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![herstellen in uitvoering](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Als u de status van het herstellen wilt bijhouden, voert u de volgende query uit in een nieuwe querysessie:

   ```
SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
, dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
FROM sys.dm_exec_requests r 
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![percentage voltooid van herstellen](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Wanneer het herstellen is voltooid, bekijkt u het herstel in Objectverkenner. 

    ![herstellen voltooid](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over beheerde exemplaren leest u [What is a Managed Instance?](sql-database-managed-instance.md) (Wat is een beheerd exemplaar?).
- Lees [Managed Instance VNet Configuration](sql-database-managed-instance-vnet-configuration.md) (VNet-configuratie voor beheerd exemplaar) voor meer informatie over VNet-configuratie.
- Lees [Connect applications](sql-database-managed-instance-connect-app.md) (Verbinding maken met toepassingen) voor meer informatie over het maken van verbinding met toepassingen.
- Lees het artikel [Managed Instance migration using DMS](../dms/tutorial-sql-server-to-managed-instance.md) (Migratie van een beheerd exemplaar via DMS) voor een zelfstudie over gebruik van de Azure Database Migration Service (DMS).
