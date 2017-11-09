---
title: Maak een klassieke Azure-VM met MySQL | Microsoft Docs
description: Maak een Azure virtuele machine met Windows Server 2012 R2 en de MySQL-database met het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 98fa06d2-9b92-4d05-ac16-3f8e9fd4feaa
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: cynthn
ms.openlocfilehash: e4135f96027cc25e1e0a149857ace1672570fc4b
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2016"></a>MySQL installeren op een virtuele machine gemaakt met het klassieke implementatiemodel met Windows Server 2016
[MySQL](https://www.mysql.com) is een populaire open-source, SQL-database. Deze zelfstudie ziet u het installeren en voer de **community-versie van MySQL 5.7.18** als een MySQL-Server op een virtuele machine met **Windows Server 2016**. Uw ervaring mogelijk enigszins verschillen voor andere versies van MySQL of Windows Server.

Zie voor instructies over het installeren van MySQL op Linux: [MySQL installeren op Azure](../../linux/mysql-install.md).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="create-a-virtual-machine-running-windows-server-2016"></a>Maak een virtuele machine met Windows Server 2016
Als u een virtuele machine met Windows Server 2016 nog geen hebt, kunt u dit [zelfstudie](./tutorial.md) voor het maken van de virtuele machine.

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Nadat de virtuele machine is gemaakt, kunt u eventueel een gegevensschijf koppelen. Toevoegen van dat een gegevensschijf wordt aanbevolen voor productieworkloads en om te voorkomen met de beschikbare ruimte op de OS-schijf (C:), waaronder het besturingssysteem.

Zie [hoe een gegevensschijf koppelen aan een virtuele Windows-machine](../attach-managed-disk-portal.md) en volg de instructies voor het koppelen van een lege schijf. De host-cache-instelling ingesteld op **geen** of **alleen-lezen**.

## <a name="log-on-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine
Vervolgens moet u [Meld u aan bij de virtuele machine](./connect-logon.md) zodat u MySQL kunt installeren.

## <a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Installeren en uitvoeren van MySQL-Community-Server op de virtuele machine
Volg deze stappen als u wilt installeren, configureren en uitvoeren van de Community-versie van MySQL-Server:

> [!NOTE]
> Bij het downloaden van items die met Internet Explorer kunt u de Internet Explorer instellen **verbeterde beveiliging** op uitgeschakeld, en het downloadproces vereenvoudigen. Vanuit het menu Start hulpprogramma's of de Server Manager/lokale beheerserver Klik IE **verbeterde beveiliging** en de configuratie op Off instellen).
>
>

1. Nadat u verbinding hebt gemaakt met de virtuele machine via Extern bureaublad, klikt u op **Internet Explorer** vanuit het startscherm.
2. Selecteer de **extra** knop in de rechterbovenhoek (het pictogram cogged wheel) en klik vervolgens op **Internetopties**. Klik op de **beveiliging** en klik op de **vertrouwde websites** pictogram en klik vervolgens op de **Sites** knop. Http://*.mysql.com toevoegen aan de lijst met vertrouwde sites. Klik op **sluiten**, en klik vervolgens op **OK**.
3. In de adresbalk van Internet Explorer, typ https://dev.mysql.com/downloads/mysql/.
4. De MySQL-site gebruiken om te zoeken en download de nieuwste versie van het MySQL-installatieprogramma voor Windows. Als u het installatieprogramma MySQL, download u de versie die de volledige set (bijvoorbeeld de mysql-installatieprogramma-community-5.7.18.0.msi met een bestandsgrootte van 352.8 MB)-bestand en sla het installatieprogramma heeft.
5. Wanneer het installatieprogramma downloaden is voltooid, klikt u op **uitvoeren** setup te starten.
6. Op de **License Agreement** pagina, accepteer de gebruiksrechtovereenkomst en klik op **volgende**.
7. Op de **kiezen van een installatietype** pagina, klikt u op het installatietype dat u wilt en klik vervolgens op **volgende**. De volgende stappen wordt ervan uitgegaan dat de selectie van de **alleen Server** type installatie.
8. Als de **vereisten controleren** pagina, klikt u op **Execute** zodat het installatieprogramma ontbrekende onderdelen te installeren. Volg de instructies die wordt weergegeven, zoals de C++ Redistributable runtime.
9. Op de **installatie** pagina, klikt u op **Execute**. Wanneer de installatie is voltooid, klikt u op **volgende**.

10. Op de **productconfiguratie** pagina, klikt u op **volgende**.

11. Op de **Type en netwerken** pagina, geeft u het gewenste type en de connectiviteit opties, waaronder de TCP-poort indien nodig. Selecteer **geavanceerde opties weergeven**, en klik vervolgens op **volgende**.
    ![](./media/mysql-2008r2/MySQL_TypeNetworking.png)

12. Op de **Accounts en rollen** pagina, Geef een sterk wachtwoord van de MySQL-hoofdmap. Aanvullende MySQL-gebruikersaccounts toevoegen, indien nodig, en klik vervolgens op **volgende**.

    ![](./media/mysql-2008r2/MySQL_AccountsRoles_Filled.png)
13. Op de **Windows-Service** pagina en klik vervolgens op wijzigingen in de standaardinstellingen opgeven voor het uitvoeren van de MySQL-Server als een Windows-service naar behoefte **volgende**.

    ![](./media/mysql-2008r2/MySQL_WindowsService.png)
14. De opties op de **invoegtoepassingen en -extensies** pagina zijn optioneel. Klik op **volgende** om door te gaan.
15. Op de **geavanceerde opties** pagina, geef de gewenste wijzigingen aan de opties voor logboekregistratie op en klik vervolgens op **volgende**.

    ![](./media/mysql-2008r2/MySQL_AdvOptions.png)
16. Op de **serverconfiguratie toepassen** pagina, klikt u op **Execute**. Wanneer u de configuratiestappen zijn voltooid, klikt u op **voltooien**.
17. Op de **productconfiguratie** pagina, klikt u op **volgende**.
18. Op de **Installation Complete** pagina, klikt u op **logboek kopiëren naar Klembord** desgewenst later onderzoeken en klik vervolgens op **voltooien**.
19. Typ in het startscherm **mysql**, en klik vervolgens op **MySQL 5.7 Command-Line Client**.
20. Voer het root-wachtwoord die u hebt opgegeven in stap 12 en krijgt u een prompt waar kunt u opdrachten voor het configureren van MySQL uitgeven. Zie voor een overzicht van opdrachten en syntaxis, [MySQL verwijzing handleidingen](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html).

    ![](./media/mysql-2008r2/MySQL_CommandPrompt.png)
21. U kunt ook configuratie standaardserverinstellingen, zoals de basis- en -mappen en stations configureren. Zie voor meer informatie [6.1.2 Server Configuration standaard](https://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Eindpunten configureren

Voor de MySQL-service moet beschikbaar zijn voor clientcomputers op Internet, moet u een eindpunt configureert voor de TCP-poort en maakt u een Windows Firewall-regel. De standaardwaarde van de poort op waarop de MySQL-Server-service voor clients van MySQL luistert is 3306. U kunt een andere poort opgeven als de poort consistent met de waarde die is geleverd is op de **Type en netwerken** (stap 11 van de vorige procedure).

> [!NOTE]
> Voor gebruik in productieomgevingen, kunt u de gevolgen voor de beveiliging van de MySQL-Server-service beschikbaar te maken op alle computers op het Internet. U kunt de reeks IP-adressen die zijn toegestaan op het eindpunt te gebruiken met een lijst met ACL (Access Control) definiëren. Zie voor meer informatie [ingesteld van eindpunten aan een virtuele Machine](setup-endpoints.md).
>
>

Een eindpunt voor de MySQL-Server-service configureren:

1. Klik in de Azure-portal op **virtuele Machines (klassiek)**, klik op de naam van uw MySQL virtuele machine en klik vervolgens op **eindpunten**.
2. Klik in de opdrachtbalk op **toevoegen**.
3. Op de **eindpunt toevoegen** pagina, typt u een unieke naam voor **naam**.
4. Selecteer **TCP** als protocol.
5. Typ het poortnummer zoals **3306**, in beide **openbare poort** en **particuliere poort**, en klik vervolgens op **OK**.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Een Windows Firewall-regel voor MySQL verkeer toevoegen
Voer de volgende opdracht op voor het toevoegen van een Windows Firewall-regel waarmee MySQL-verkeer van het Internet, een _opdrachtprompt van Windows PowerShell_ op de virtuele machine voor de MySQL-server.

    New-NetFirewallRule -DisplayName "MySQL57" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public

## <a name="test-your-remote-connection"></a>Test de externe verbinding
Als u wilt testen uw externe verbinding met de Azure VM waarop de MySQL-Server-service wordt uitgevoerd, moet u de DNS-naam van de cloudservice met de VN opgeven.

1. Klik in de Azure-portal op **virtuele Machines (klassiek)**, klik op de naam van de virtuele machine van uw MySQL-server en klik vervolgens op **overzicht**.
2. Let in het dashboard virtuele machine op de **DNS-naam** waarde. Hier volgt een voorbeeld:

   ![](media/mysql-2008r2/MySQL_DNSName.png)
3. Voer de volgende opdracht als een MySQL-gebruiker aanmelden vanaf een lokale computer met MySQL of de MySQL-client.

     MySQL -u <yourMysqlUsername> - p -h<yourDNSname>

   Bijvoorbeeld, met behulp van de gebruikersnaam MySQL _dbadmin3_ en de _testmysql.cloudapp.net_ DNS-naam voor de virtuele machine, MySQL met de volgende opdracht kan worden gestart:

     MySQL -u dbadmin3 -p -h testmysql.cloudapp.net

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het uitvoeren van MySQL, de [MySQL documentatie](http://dev.mysql.com/doc/).
