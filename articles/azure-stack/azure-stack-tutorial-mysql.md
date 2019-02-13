---
title: Bieden van maximaal beschikbare MySQL-databases in Azure Stack | Microsoft Docs
description: Meer informatie over het maken van een resourceprovider van MySQL-Server-hostcomputer en maximaal beschikbare MySQL-databases met Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: 8fc3246c76d7d09ea4518b5c548d5dbe05cc7a84
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164257"
---
# <a name="tutorial-offer-highly-available-mysql-databases"></a>Zelfstudie: MySQL-databases met hoge beschikbaarheid bieden

Als een Azure Stack-Operator, kunt u server-VM's databases MySQL-Server te configureren. Na een MySQL-cluster met succes is gemaakt en beheerd door Azure Stack, gebruikers die zijn geabonneerd op de MySQL-services eenvoudig maximaal beschikbare MySQL-databases kunnen maken.

Deze zelfstudie laat zien hoe Azure Stack marketplace-items gebruiken om te maken van een [MySQL met replicatie cluster](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Deze oplossing maakt gebruik van meerdere virtuele machines worden gerepliceerd van de databases van het hoofdknooppunt naar een configureerbaar aantal replica's. Nadat u hebt gemaakt, het cluster kan vervolgens worden toegevoegd als een Azure Stack MySQL die als host fungeert Server en vervolgens gebruikers een maximaal beschikbare MySQL-databases kunnen maken.

> [!IMPORTANT]
> De **MySQL met replicatie** Azure Stack marketplace-item mogelijk niet beschikbaar voor alle Azure-cloud-abonnement omgevingen. Controleer of de marketplace-item is beschikbaar in uw abonnement voordat u probeert te volgen in de rest van dit tutoral.

Wat u leert:

> [!div class="checklist"]
> * Een MySQL-Server-cluster maken vanuit de marketplace-items
> * Maak een Azure Stack-MySQL Server die als host fungeert
> * Een maximaal beschikbare MySQL-database maken

In deze zelfstudie wordt een cluster met drie VM MySQL-Server wordt gemaakt en geconfigureerd met behulp van de beschikbare Azure Stack marketplace-items. 

Voordat u de stappen in deze zelfstudie begint, zorg ervoor dat de [resourceprovider van MySQL-Server](azure-stack-mysql-resource-provider-deploy.md) is geïnstalleerd en dat de volgende items beschikbaar in de Azure Stack marketplace zijn:

> [!IMPORTANT]
> Alle van de volgende opties zijn vereist voor het maken van de MySQL-cluster.

- [MySQL met replicatie](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Dit is de sjabloon van de Bitnami-oplossing die wordt gebruikt voor de implementatie van de MySQL-cluster.
- [Debian 8 "Jessie"](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.Debian). Debian 8 "Jessie" backports kernel voor Microsoft Azure die worden geleverd door credativ. Debian GNU/Linux is een van de meest populaire Linux-distributies.
- [Aangepast script voor linux 2.0](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft.custom-script-linux?tab=Overview). Aangepaste Scriptextensie is een hulpprogramma voor het uitvoeren van uw virtuele machine aanpassen taken post VM inrichten. Wanneer deze extensie wordt toegevoegd aan een virtuele Machine, kan deze scripts downloaden uit Azure storage en voer ze uit op de virtuele machine. Aangepaste Scriptextensie taken kunnen ook worden geautomatiseerd met behulp van de Azure PowerShell-cmdlets en Azure platformoverschrijdende Command-Line Interface (xPlat CLI).
- VM-toegang voor Linux-extensie 1.4.7. De extensie voor VM-toegang kunt u het wachtwoord, SSH-sleutel of de configuraties SSH opnieuw instellen, zodat u weer toegang met uw virtuele machine krijgen kan. U kunt ook een nieuwe gebruiker met wachtwoord of SSH-sleutel toevoegen of verwijderen van een gebruiker die deze extensie. Deze extensie is bedoeld voor virtuele Linux-machines.

Zie voor meer informatie over het toevoegen van items aan de Azure Stack marketplace, de [overzicht van Azure Stack Marketplace](azure-stack-marketplace.md).

U moet ook een SSH-client, zoals [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) aan te melden bij de virtuele Linux-machines, nadat ze zijn geïmplementeerd.

## <a name="create-a-mysql-server-cluster"></a>Een MySQL-Server-cluster maken 
Gebruik de stappen in deze sectie voor het implementeren van de MySQL-Server-cluster met behulp van de [MySQL met replicatie](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster) marketplace-item. Deze sjabloon implementeert drie MySQL-Server-exemplaren in een maximaal beschikbare MySQL-cluster is geconfigureerd. Standaard wordt gemaakt in de volgende bronnen:

- Een virtueel netwerk
- Een netwerkbeveiligingsgroep
- Een storage-account
- Een beschikbaarheidsset
- Drie netwerkinterfaces (één voor elk van de standaard-VM's)
- Een openbaar IP-adres (voor de primaire MySQL cluster VM)
- Drie Linux-VM's voor het hosten van de MySQL-cluster

1. 
[!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Selecteer **\+** **een resource maken** > **Compute**, en vervolgens **MySQL met replicatie**.

   ![Implementatie van de aangepaste sjabloon](media/azure-stack-tutorial-mysqlrp/1.png)

3. Eenvoudige implementatie-informatie bieden over de **basisbeginselen** pagina. Controleer de standaardwaarden en indien nodig wijzigen en klik op **OK**.<br><br>Ten minste bieden het volgende:
   - Naam van de implementatie (de standaardinstelling is mymysql)
   - Hoofdwachtwoord van de toepassing. Geef een alfanumerieke wachtwoord 12 tekens met **geen speciale tekens**
   - Toepassingsnaam-database (de standaardinstelling is bitnami)
   - Aantal replica virtuele machines te maken van de MySQL-database (de standaardwaarde is 2)
   - Selecteer het abonnement te gebruiken
   - Selecteer de resourcegroep te gebruiken of een nieuw wachtwoord maken
   - Selecteer de locatie (de standaardinstelling is lokaal voor ASDK)

   [![](media/azure-stack-tutorial-mysqlrp/2-sm.PNG "Grondbeginselen van implementatie")](media/azure-stack-tutorial-mysqlrp/2-lg.PNG#lightbox)

4. Op de **configuratie omgeving** pagina, geef de volgende informatie en klik vervolgens op **OK**: 
   - Wachtwoord of SSH openbare sleutel te gebruiken voor verificatie van SSH (secure shell). Als u een wachtwoord gebruikt, moet deze letters, cijfers bevatten en **kunt** speciale tekens bevatten
   - VM-grootte (de standaardinstelling is Standard D1 v2 VM's)
   - Gegevens schijfgrootte in GB Klik **OK**

   [![](media/azure-stack-tutorial-mysqlrp/3-sm.PNG "Configuratie van omgeving")](media/azure-stack-tutorial-mysqlrp/3-lg.PNG#lightbox)

5. Controleer de implementatie **samenvatting**. (Optioneel) u kunt de aangepaste sjabloon en parameters downloaden, en klik vervolgens op **OK**.

   [![](media/azure-stack-tutorial-mysqlrp/4-sm.PNG "Samenvatting")](media/azure-stack-tutorial-mysqlrp/4-lg.PNG#lightbox)

6. Klik op **maken** op de **kopen** pagina om de implementatie te starten.

   ![Kopen](media/azure-stack-tutorial-mysqlrp/5.png)

    > [!NOTE]
    > De implementatie duurt ongeveer een uur. Zorg ervoor dat de implementatie is voltooid en de MySQL-cluster volledig is geconfigureerd voordat u doorgaat. 

7. Nadat alle implementaties zijn voltooid, bekijkt u de resource-group-items en selecteert u de **mysqlip** openbaar IP-adres-item. Noteer het openbare IP-adres en de volledige FQDN-naam van het openbare IP-adres voor het cluster.<br><br>U moet dit aan een Azure Stack-Operator bieden, zodat ze een MySQL-server host gebruik te maken van dit cluster MySQL kunnen maken.


### <a name="create-a-network-security-group-rule"></a>Regel voor netwerkbeveiligingsgroep maken
Er zijn geen openbare toegang is standaard geconfigureerd voor MySQL in de host-VM. Een inkomende netwerkbeveiligingsgroepen (NSG) regel voor de netwerkbeveiligingsgroep moet worden gemaakt voor de Azure Stack-MySQL-resourceprovider te verbinden en beheren van de MySQL-cluster.

1. In de beheerdersportal voor de, gaat u naar de resourcegroep hebt gemaakt bij het implementeren van de MySQL-cluster en selecteert u de netwerkbeveiligingsgroep (**standaard-subnet-sg**):

   ![open](media/azure-stack-tutorial-mysqlrp/6.png)

2. Selecteer **inkomende beveiligingsregels** en klik vervolgens op **toevoegen**.<br><br>Voer **3306** in de **doelpoortbereik** en geef eventueel een beschrijving in de **naam** en **beschrijving** velden. Klik op toevoegen om de inkomende regel dialoogvenster te sluiten.

   ![open](media/azure-stack-tutorial-mysqlrp/7.png)

### <a name="configure-external-access-to-the-mysql-cluster"></a>Externe toegang tot de MySQL-cluster configureren
Voordat de MySQL-cluster kan worden toegevoegd als een Azure Stack MySQL Server-host, moet externe toegang worden ingeschakeld.

1. In dit voorbeeld wordt met behulp van een SSH-client, [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), meld u aan bij de primaire machine MySQL op een computer die toegang hebben tot het openbare IP-adres. De naam van de primaire VM MySQL heeft meestal **0** en heeft een openbaar IP-adres toegewezen.<br><br>Gebruik het openbare IP- en het logboek in de virtuele machine met de gebruikersnaam van **bitnami** en het wachtwoord voor u eerder hebt gemaakt zonder speciale tekens.

   ![LinuxLogin](media/azure-stack-tutorial-mysqlrp/bitnami1.png)


2. Gebruik de volgende opdracht uit zodat u dat de bitnami-service is actief en wordt uitgevoerd in het venster SSH-client. Het bitnami-wachtwoord opnieuw opgeven wanneer u hierom wordt gevraagd:

   `sudo service bitnami status`

   ![Controleer service](media/azure-stack-tutorial-mysqlrp/bitnami2.png)

3. Maak een gebruikersaccount voor externe toegang moet worden gebruikt door de Azure Stack MySQL die als host fungeert Server verbinding maken met MySQL en sluit de SSH-client.<br><br>Voer de volgende opdrachten voor aanmelding bij MySQL als basis, met behulp van het hoofdwachtwoord eerder hebt gemaakt, en maak een nieuwe gebruiker met beheerdersrechten, Vervang *\<gebruikersnaam\>* en *\<wachtwoord\>* zoals vereist voor uw omgeving. In dit voorbeeld wordt de gebruiker moet worden gemaakt met de naam **sqlsa** en een sterk wachtwoord wordt gebruikt:

   ```mysql
   mysql -u root -p
   create user <username>@'%' identified by '<password>';
   grant all privileges on *.* to <username>@'%' with grant option;
   flush privileges;
   ```
   ![De gebruiker met beheerdersrechten maken](media/azure-stack-tutorial-mysqlrp/bitnami3.png)


4. Noteer de gegevens van de nieuwe MySQL-gebruiker.<br><br>U moet voor deze gebruikersnaam en wachtwoord, samen met het openbare IP-adres of de volledige FQDN-naam van het openbare IP-adres voor het cluster voor een Azure Stack-operators, zodat ze een MySQL-server host met behulp van deze MySQL-cluster kunnen maken.


## <a name="create-an-azure-stack-mysql-hosting-server"></a>Maak een Azure Stack-MySQL Server die als host fungeert
Nadat de MySQL-Server-cluster is gemaakt en geconfigureerd, moet een Azure Stack MySQL die als host fungeert Server zodat de capaciteit beschikbaar voor gebruikers om databases te maken in een Azure Stack-Operator maken. 

Zorg ervoor dat u het openbare IP-adres of de volledige FQDN voor het openbare IP-adres van de MySQL-cluster dat primaire VM eerder vastgelegd wanneer de resourcegroep van de MySQL-cluster is gemaakt (**mysqlip**). Bovendien moet de operator de MySQL-Server weten verificatiereferenties die u hebt gemaakt voor externe toegang tot de cluster-database voor MySQL.

> [!NOTE]
> Deze stap moet worden uitgevoerd vanaf de Azure Stack-beheerportal door een Azure Stack-Operator.

Met behulp van de MySQL-cluster openbaar IP-adres- en MySQL-verificatie aanmeldingsgegevens, een Azure Stack-Operator kunnen nu [een MySQL host-Server met behulp van de nieuwe MySQL-cluster maken](azure-stack-mysql-resource-provider-hosting-servers.md#connect-to-a-mysql-hosting-server). 

Zorg er ook voor dat u hebt gemaakt plannen en aanbiedingen voor het maken van de MySQL-database beschikbaar maken voor gebruikers. Een operator moet toevoegen de **Microsoft.MySqlAdapter** service naar een plan en maak een nieuw quotum specifiek voor databases met hoge beschikbaarheid. Zie voor meer informatie over het maken van abonnementen [Plan, aanbieding, quota en abonnementen overzicht](azure-stack-plan-offer-quota-overview.md).

> [!TIP]
> De **Microsoft.MySqlAdapter** service niet meer beschikbaar zijn om toe te voegen aan plannen totdat de [resourceprovider van MySQL-Server is geïmplementeerd](azure-stack-mysql-resource-provider-deploy.md).



## <a name="create-a-highly-available-mysql-database"></a>Een maximaal beschikbare MySQL-database maken
Nadat de MySQL-cluster is gemaakt, geconfigureerd, en toegevoegd als een Azure Stack MySQL die als host fungeert Server door een Azure Stack-Operator, kunt een tenant-gebruiker met een abonnement met inbegrip van de mogelijkheden van de MySQL-Server-database door maximaal beschikbare MySQL-databases maken de stappen in dit gedeelte te volgen. 

> [!NOTE]
> Deze stappen van de gebruikersportal van Azure Stack uitgevoerd als een tenant-gebruiker met een abonnement van mogelijkheden voor MySQL-Server (Microsoft.MySQLAdapter service).

1. 
[!INCLUDE [azs-user-portal](../../includes/azs-user-portal.md)]

2. Selecteer **\+** **een resource maken** > **gegevens \+ opslag**, en vervolgens **MySQL-Database** .<br><br>Geef de vereiste database-eigenschapsgegevens zoals naam, sortering, het abonnement dat u wilt gebruiken en de locatie waar u wilt gebruiken voor de implementatie. 

   ![MySQL-database maken](./media/azure-stack-tutorial-mysqlrp/createdb1.png)

3. Selecteer **SKU** en kies vervolgens de juiste MySQL die als host fungeert Server SKU om het te gebruiken. In dit voorbeeld wordt de Azure Stack-Operator heeft gemaakt de **MySQL-HA** SKU voor de ondersteuning van hoge beschikbaarheid voor cluster-MySQL-databases.

   ![Selecteer de SKU](./media/azure-stack-tutorial-mysqlrp/createdb2.png)

4. Selecteer **aanmelding** > **Maak een nieuwe aanmelding** en geef vervolgens de referenties voor de MySQL-verificatie moet worden gebruikt voor de nieuwe database. Wanneer u klaar bent, klikt u op **OK** en vervolgens **maken** om te beginnen met het proces van de database.

   ![Aanmelding toevoegen](./media/azure-stack-tutorial-mysqlrp/createdb3.png)

5. Wanneer de implementatie van de MySQL-database voltooid is, Controleer de eigenschappen van de database voor het detecteren van de verbindingsreeks te gebruiken om verbinding te maken met de nieuwe database maximaal beschikbaar. 

   ![Verbindingsreeks weergeven](./media/azure-stack-tutorial-mysqlrp/createdb4.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een MySQL-Server-cluster maken vanuit de marketplace-items
> * Maak een Azure Stack-MySQL Server die als host fungeert
> * Een maximaal beschikbare MySQL-database maken

Ga verder met de volgende zelfstudie voor meer informatie over het:
> [!div class="nextstepaction"]
> [Aanbieding WebApps](azure-stack-tutorial-app-service.md)
