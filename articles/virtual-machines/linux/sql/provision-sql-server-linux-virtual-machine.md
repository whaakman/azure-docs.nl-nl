---
title: Een virtuele SQL Server 2017-machine voor Linux maken in Azure | Microsoft Docs
description: Deze zelfstudie laat zien hoe u een virtuele SQL Server 2017-machine voor Linux in Azure Portal kunt maken.
services: virtual-machines-linux
author: rothja
ms.author: jroth
manager: jhubbard
ms.date: 10/25/2017
ms.topic: hero-article
tags: azure-service-management
ms.devlang: na
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.technology: database-engine
ms.openlocfilehash: 8d714718e4fc27f0abbfede8fe6d0e2693698ec1
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="provision-a-linux-sql-server-virtual-machine-in-the-azure-portal"></a>Een virtuele SQL Server-machine inrichten in Azure Portal

> [!div class="op_single_selector"]
> * [Linux](provision-sql-server-linux-virtual-machine.md)
> * [Windows](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)

In deze snelstartzelfstudie gaat u Azure Portal gebruiken om een virtuele Linux-machine te maken waarop SQL Server 2017 is geïnstalleerd.

In deze zelfstudie leert u het volgende:

* [Een virtuele Linux SQL-machine vanuit de galerie te maken](#create)
* [Verbinding te maken met de nieuwe virtuele machine via ssh](#connect)
* [Het SA-wachtwoord te wijzigen](#password)
* [Voor externe verbindingen te configureren](#remote)

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a id="create"></a> Een virtuele Linux-machine maken waarop SQL Server is geïnstalleerd

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Klik in het linkerdeelvenster op **Nieuw**.

1. Klik in het deelvenster **Nieuw** op **Berekenen**.

1. Klik op **Alles bekijken** naast de kop **Aanbevolen**.

   ![Alle VM-installatiekopieën bekijken](./media/provision-sql-server-linux-virtual-machine/azure-compute-blade.png)

1. Typ in het zoekvak **SQL Server 2017** en druk op **Enter** om met zoeken te beginnen.

1. Klik op het pictogram met het **filter** om de zoekopdracht te beperken tot **installatiekopieën voor Linux** van **Microsoft** en klik vervolgens op **Gereed**.

    ![Zoekfilter voor installatiekopieën van virtuele SQL Server 2017-machines](./media/provision-sql-server-linux-virtual-machine/searchfilter.png)

1. Selecteer een installatiekopie van SQL Server 2017 voor Linux in de zoekresultaten. In deze zelfstudie wordt gebruikgemaakt van **gratis licentie voor SQL Server: SQL Server 2017 Developer op Red Hat Enterprise Linux 7.4**.

   > [!TIP]
   > De ontwikkelaarsversie kunt u gebruiken om de functies van de Enterprise-editie te testen of om er ontwikkeltaken mee uit te voeren, zonder dat er SQL Server-licentiekosten mee gemoeid zijn. U betaalt alleen om met de virtuele Linux-machine te werken.

1. Klik op **Create**.

1. Vul in het venster **Basisinformatie** de details in van uw virtuele Linux-machine. 

    ![Het venster Basisinformatie](./media/provision-sql-server-linux-virtual-machine/basics.png)

    > [!Note]
    > U hebt de keuze om voor de verificatie een openbare SSH-sleutel of een wachtwoord te gebruiken. SSH is veiliger. Zie [SSH-sleutels maken in Linux en Mac voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys) voor instructies over het maken van een SSH-sleutel.

1. Klik op **OK**.

1. Kies in het venster **Grootte** de grootte voor een machine. Selecteer **Alles bekijken** om de andere grootten te zien. Zie [Linux VM-grootten](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes) voor meer informatie over de grootte van VM-machines.

    ![Een VM-grootte kiezen](./media/provision-sql-server-linux-virtual-machine/vmsizes.png)

   > [!TIP]
   > Voor de ontwikkeling en het uitvoeren van functionele tests kunt u het beste een VM-grootte van **DS2** of groter kiezen. Gebruik **DS13** of groter als u prestatietests wilt uitvoeren.

1. Klik op **Selecteren**.

1. In het venster **Instellingen** kunt u wijzigingen aanbrengen in de instellingen, of u kunt de standaardinstellingen behouden.

1. Klik op **OK**.

1. Klik op de pagina **Samenvatting** op **Kopen** om de virtuele machine te maken.

## <a id="connect"></a> Verbinding maken met de virtuele Linux-machine

Als u al een BASH-shell gebruikt, moet u verbinding maken met de virtuele Azure-machine via de opdracht **ssh**. In de volgende opdracht vervangt u de VM-gebruikersnaam en het IP-adres om verbinding te maken met uw virtuele Linux-machine.

```bash
ssh azureadmin@40.55.55.555
```

U vindt het IP-adres van uw virtuele machine in Azure Portal.

![IP-adres in Azure Portal](./media/provision-sql-server-linux-virtual-machine/vmproperties.png)

Als u Windows gebruikt en geen BASH-shell hebt, kunt u een SSH-client installeren, zoals PuTTY.

1. [PuTTY downloaden en installeren](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

1. Voer PuTTY uit.

1. Voer het openbare IP-adres van de virtuele machine in op het configuratiescherm van PuTTY.

1. Klik op Openen en voer uw gebruikersnaam en wachtwoord in bij de prompts.

Zie [Een virtuele Linux-machine in Azure maken met behulp van de portal](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-portal#ssh-to-the-vm) voor meer informatie over verbinding maken met virtuele Linux-machines.

## <a id="password"></a>Het SA-wachtwoord wijzigen

Op de nieuwe virtuele machine wordt SQL Server geïnstalleerd met een willekeurig SA-wachtwoord. Voordat u met de SA-aanmeldingsgegevens verbinding kunt maken met SQL Server, moet u dit wachtwoord opnieuw instellen.

1. Nadat u verbinding hebt gemaakt met uw virtuele Linux-machine, opent u een nieuwe opdrachtenterminal.

1. Wijzig het SA-wachtwoord met de volgende opdrachten:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```

   Voer een nieuw SA-wachtwoord en een wachtwoordbevestiging in wanneer dat wordt gevraagd.

1. Start de SQL Server-service opnieuw.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="add-the-tools-to-your-path-optional"></a>De hulpprogramma's aan uw pad toevoegen (optioneel)

Meerdere SQL-Server-[pakketten](sql-server-linux-virtual-machines-overview.md#packages) worden standaard geïnstalleerd, met inbegrip van het pakket met opdrachtregelprogramma's van SQL Server. Het pakket bevat de hulpprogramma's **sqlcmd** en **bcp**. Voor meer gebruiksgemak kunt u ervoor kiezen het pad naar de hulpprogramma's `/opt/mssql-tools/bin/` toe te voegen aan de omgevingsvariabele **PAD**.

1. Voer de volgende opdrachten uit als u het **PAD** wilt wijzigen voor zowel aanmeldingssessies als interactieve/niet-aanmeldingssessies:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a id="remote"></a> Voor externe verbindingen configureren

Als u op afstand verbinding wilt maken met SQL Server op de virtuele Azure-machine, moet u een inkomende regel configureren in de netwerkbeveiligingsgroep. De regel staat verkeer op de poort toe waarop door SQL Server wordt geluisterd (standaard is dat 1433). De volgende stappen laten zien hoe Azure Portal voor deze stap kan worden gebruikt. 

1. Selecteer in de portal **Virtuele machines** en selecteer de virtuele SQL Server-machine.

1. Selecteer **Netwerken** in de lijst met eigenschappen.

1. Klik in het venster **Netwerken** op de knop **Toevoegen** onder **Regels voor binnenkomende poort**.

   ![Regels voor binnenkomende poort](./media/provision-sql-server-linux-virtual-machine/networking.png)

1. Selecteer **MS SQL** in de lijst **Service**.

    ![Regel voor MS SQL-beveiligingsgroep](./media/provision-sql-server-linux-virtual-machine/sqlnsgrule.png)

1. Klik op **OK** om de regel voor uw virtuele machine op te slaan.

### <a name="open-the-firewall-on-rhel"></a>De firewall op RHEL openen

Deze zelfstudie heeft u richtlijnen gegeven om een virtuele RHEL-machine (Red Hat Enterprise Linux) te maken. Als u extern verbinding wilt maken met virtuele RHEL-machines, moet u poort 1433 op de Linux-firewall ook openen.

1. [Maak verbinding](#connect) met uw virtuele RHEL-machine.

1. Voer de volgende opdrachten uit in de BASH-shell:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

## <a name="next-steps"></a>Volgende stappen

Nu u een virtuele SQL Server 2017-machine in Azure hebt, kunt u met behulp van **sqlcmd** lokaal verbinding maken om Transact-SQL-query's uit te voeren.

Als u de virtuele Azure-machine voor externe verbindingen met SQL Server hebt geconfigureerd, zou u ook op afstand verbinding moeten kunnen maken. Zie [Use SSMS on Windows to connect to SQL Server on Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-ssms) (SSMS gebruiken op Windows om verbinding te maken met SQL Server op Linux) als u een voorbeeld wilt zien van hoe u extern verbinding kunt maken met SQL Server op Linux vanuit Windows. Zie [Use Visual Studio Code to create and run Transact-SQL scripts for SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode) (Visual Studio Code gebruiken om Transact-SQL-scripts voor SQL Server te maken en uit te voeren) als u verbinding wilt maken met Visual Studio Code.

Zie [Overview of SQL Server 2017 on Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview) (Overzicht van SQL Server 2017 op Linux) voor meer algemene informatie over SQL Server op Linux. Zie [Overzicht van virtuele SQL Server 2017-machines in Azure](sql-server-linux-virtual-machines-overview.md) voor meer informatie over het gebruik van virtuele SQL Server 2017-machines voor Linux.
