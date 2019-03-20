---
title: Micro Focus CICS BankDemo instellen voor Micro Focus Enterprise Developer 4.0 in Azure | Microsoft Docs
description: Voer de toepassing Micro Focus BankDemo op Azure voor meer informatie over het gebruik van Micro Focus Enterprise Server en Enterprise-ontwikkelaars.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 733649c2b7f27ec5e80cb77adba708a158fb2c22
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58099464"
---
# <a name="set-up-micro-focus-cics-bankdemo-for-micro-focus-enterprise-developer-40-in-azure"></a>Micro Focus CICS BankDemo instellen voor Micro Focus Enterprise Developer 4.0 in Azure

Bij het instellen van Micro Focus Enterprise Server 4.0 en Enterprise Developer 4.0 op Azure, kunt u implementaties van IBM z/OS-werkbelastingen kunt testen. In dit artikel laat zien hoe het instellen van CICS BankDemo, een voorbeeldtoepassing die wordt geleverd met Enterprise-ontwikkelaars.

CICs staat voor het systeem voor klantinformatie, de transactie-platform dat door veel van de online mainframe-toepassingen. De toepassing BankDemo is erg handig voor het leren van de werking van Enterprise-Server en Enterprise Developer en over het beheren en implementeren van een werkelijke toepassing is voltooid met groene scherm terminals.

## <a name="prerequisites"></a>Vereisten

- Een virtuele machine met [Enterprise Developer](set-up-micro-focus-on-azure.md). Houd er rekening mee dat ontwikkelaar een compleet exemplaar van de Enterprise-Server erop voor ontwikkelings- en testdoeleinden heeft. Dit is het exemplaar van de Enterprise-Server die wordt gebruikt voor de demo.

- [SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express). Download en installeer deze op de Enterprise Developer-VM. Enterprise-Server vereist een database voor het beheer van CICS regio's en de toepassing BankDemo gebruikt ook een SQL Server-database met de naam BANKDEMO. In deze demo wordt ervan uitgegaan dat u SQL Server Express gebruikt voor beide databases. Wanneer u installeert, selecteert u de standaardinstallatie.

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) (SSMS). SSMS wordt gebruikt voor het beheren van de databases en een T-SQL-script is uitgevoerd. Download en installeer deze op de Enterprise Developer-VM.

- [Visual Studio 2017](https://azure.microsoft.com/downloads/) met het nieuwste servicepack of [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/), die u gratis kunt downloaden.

- Bureaublad of een andere Rumba 3270 emulator.

## <a name="configure-the-windows-environment"></a>De Windows-omgeving configureren

Nadat u Enterprise Developer 4.0 hebt geïnstalleerd op de virtuele machine, moet u het exemplaar van de Enterprise-Server die wordt geleverd met het configureren. Om dit te doen, moet u als volgt een aantal extra functies voor Windows installeren.

1. Extern bureaublad gebruiken voor aanmelding bij de Enterprise Server 4.0-VM wordt gebruikt, u hebt gemaakt.

2. Klik op de **zoeken** pictogram naast de **Start** knop en het type **Windows functies**. De Wizard Server Manager toevoegen functies en onderdelen wordt geopend.

3. Selecteer **rol webserver (IIS)**, en controleert u het volgende:

    - Hulpprogramma's voor webbeheer
    - Compatibiliteit met IIS 6-beheer (Selecteer alle beschikbare functies)
    - IIS-beheerconsole
    - IIS Management Scripts en hulpprogramma 's
    - IIS-beheerservice

4. Selecteer **World Wide Web Services**, en controleer het volgende:

     Onderdelen voor toepassingsontwikkeling:
    - .NET-uitbreidbaarheid
    - ASP.NET
    - Veelvoorkomende HTTP-functies: Alle beschikbare functies toevoegen
    - Status en diagnostische gegevens: Alle beschikbare functies toevoegen
    - Beveiliging:
        - Basisverificatie
        - Windows-verificatie

5. Selecteer **Windows Process Activation-Service** en alle onderliggende items.

6. Voor **functies**, Controleer **Microsoft .NET framework 3.5.1**, en controleer het volgende:

    - Windows Communication Foundation HTTP Activation
    - Windows Communication Foundation Non-HTTP Activation

7. Voor **functies**, Controleer **Microsoft .NET framework 4.6**, en controleer het volgende:

   - Named Pipe-activering
   - TCP Activation
   - TCP-poort delen

     ![Wizard functies en onderdelen toevoegen: Functieservices](media/01-demo-roles.png)

8. Wanneer u alle opties hebt geselecteerd, klikt u op **volgende** te installeren.

9. Na de Windows-functies, Ga naar **Configuratiescherm \> systeem en beveiliging \> Systeembeheer**, en selecteer **Services**. Schuif naar beneden en zorg ervoor dat de volgende services worden uitgevoerd en ingesteld op **automatische**:

    - **NetTcpPortSharing**
    - **Listener-Adapter Net.Pipe**
    - **Net.tcp Listener Adapter**

10. Voor het configureren van ondersteuning voor IIS en WAS in het menu Zoeken **Micro Focus Enterprise opdrachtprompt voor ontwikkelaars (64 bits)** en uitvoeren als **beheerder**.

11. Type **wassetup – i** en druk op **Enter**.

12. Nadat het script wordt uitgevoerd, kunt u het venster sluiten.

## <a name="configure-the-local-system-account-for-sql-server"></a>Het lokale systeemaccount gebruikt voor SQL Server configureren

Bepaalde processen Enterprise-Server moeten kunnen aanmelden bij SQL Server en het maken van databases en andere objecten. Deze processen gebruiken het lokale systeemaccount gebruikt, zodat u sysadmin-instantie aan dat account geven moet.

1. Start de **SSMS** en klikt u op **Connect** verbinding maken met de lokale SQLEXPRESS-Server met behulp van Windows-verificatie. Deze moet beschikbaar zijn in de **servernaam** lijst.

2. Aan de linkerkant, vouw de **Security** map en selecteer **aanmeldingen**.

3. Selecteer **NT AUTHORITY\\SYSTEM** en selecteer **eigenschappen**.

4. Selecteer **serverfuncties** en Controleer **sysadmin**.

     ![SSMS-Object Explorer-venster: Aanmeldingseigenschappen](media/02-demo-explorer.png)

## <a name="create-the-bankdemo-database-and-all-its-objects"></a>De database BankDemo en alle bijbehorende objecten maken

1. Open **Windows Explorer** en navigeer naar **C:\\gebruikers\\openbare\\documenten\\Micro Focus\\Enterprise Developer\\ Voorbeelden\\Mainframe\\CICS\\DotNet\\BankDemo\\SQL**.

2. Kopieer de inhoud van **BankDemoCreateAll.SQL** -bestand in het Klembord.

3. Open **SSMS**. Aan de rechterkant, klikt u op **Server** en selecteer **nieuwe Query**.

4. Plak de inhoud van het Klembord in de **nieuwe Query** vak.

5. De SQL-instructies uitvoeren door te klikken op **Execute** uit de **opdracht** tabblad boven de query.

De query moet worden uitgevoerd zonder fouten. Wanneer deze voltooid is, hebt u de voorbeelddatabase voor de toepassing BankDemo.

![SQLQuery1.sql output](media/03-demo-query.png)

## <a name="verify-that-the-database-tables-and-objects-have-been-created"></a>Controleer of de database-tabellen en objecten is gemaakt

1. Met de rechtermuisknop op de **BANKDEMO** database en selecteer **vernieuwen**.

2. Vouw de **Database** en selecteer **tabellen**. U ziet er ongeveer als volgt uit.

     ![BANKDEMO tabel uitgevouwen in Object Explorer](media/04-demo-explorer.png)

## <a name="build-the-application-in-enterprise-developer"></a>Maken van de toepassing in Enterprise Developer

1. Open Visual Studio en meld u aan.

2. Onder de **bestand** Selecteer menuoptie **Open Project/oplossing**, gaat u naar **C:\\gebruikers\\openbare\\documenten\\Micro Focus\\Enterprise Developer\\voorbeelden\\Mainframe\\CICS\\DotNet\\BankDemo**, en selecteer de **sln**bestand.

3. Even om te controleren van de objecten. COBOL programma's worden weergegeven in Solution Explorer met de extensie koppelingsbestand voor samen met CopyBooks (CPY) en JCL.

4. Met de rechtermuisknop op de **BankDemo2** Project en selecteer **Set as Startup Project**.

    > [!NOTE]
    > Het BankDemo Project maakt gebruik van HCOSS (Host optie voor toepassingscompatibiliteit voor SQL Server), dat niet wordt gebruikt voor deze demo.

5. In **Solution Explorer**, met de rechtermuisknop op de **BankDemo2** Project en selecteer **bouwen**.

    > [!NOTE]
    > Bouwen op het niveau van de oplossing resulteert in fouten, zoals HCOSS is niet geconfigureerd.

6. Wanneer het Project is gemaakt, bekijkt u de **uitvoer** venster. Het ziet er als de onderstaande afbeelding.

     ![Venster van de uitvoer van geslaagde build](media/05-demo-output.png)

## <a name="deploy-the-bankdemo-application-into-the-region-database"></a>De toepassing BankDemo in de regio-database implementeren

1. Open een opdrachtprompt voor ontwikkelaars van de Enterprise (64 bits) als beheerder.

2. Navigeer naar de **openbare %\\documenten\\Micro Focus\\Enterprise Developer\\voorbeelden\\Mainframe\\CICS\\DotNet\\ BankDemo**.

3. Uitvoeren bij de opdrachtprompt **bankdemodbdeploy** en voegt u de parameter toe om de database te implementeren, bijvoorbeeld:

    ```
    bankdemodbdeploy (local)/sqlexpress
    ```

> [!NOTE]
> Zorg ervoor dat u een slash (/) een backslash (\\). Met dit script wordt uitgevoerd voor een tijdje.

![Beheer: Enterprise Developer-opdrachtpromptvenster](media/06-demo-cmd.png)

## <a name="create-the-bankdemo-region-in-enterprise-administrator-for-net"></a>Maken van de regio BankDemo in Enterprise-beheerder voor .NET

1. Open de **Enterprise-Server voor het beheer van .NET** gebruikersinterface.

2. De MMC-module starten vanuit de Windows **Start** in het menu kiezen **Micro Focus Enterprise Developer \> configuratie \> Enterprise-Server voor .NET-beheerder**. (Voor Windows Server, kiest u **Micro Focus Enterprise Developer \> Enterprise-Server voor .NET-beheerder**).

3. Vouw de **regio's** -container in het linkerdeelvenster en klik met de rechtermuisknop **CICS**.

4. Selecteer **definiëren regio** te maken van een nieuwe CICS regio met de naam **BANKDEMO**, gehost in de database (lokaal).

5. Het database-server-exemplaar opgeven, klikt u op **volgende**, en voer vervolgens de regionaam van de **BANKDEMO**.

     ![In het dialoogvenster van de regio bepalen](media/07-demo-cics.png)

6. Als u wilt het definitiebestand van de regio voor de regio-overschrijdende database selecteert, zoek **regio\_bankdemo\_db.config** in **C:\\gebruikers\\openbare\\ Documenten\\Micro Focus\\Enterprise Developer\\voorbeelden\\Mainframe\\CICS\\DotNet\\BankDemo**.

     ![Regio - regionaam definiëren: BANKDEMO](media/08-demo-cics.png)

7. Klik op **Voltooien**.

## <a name="create-xa-resource-definitions"></a>Definities van XA-Resource maken

1. In het linkerdeelvenster van de **Enterprise-Server voor het beheer van .NET** gebruikersinterface, vouw **System**, en vervolgens **XA Resourcedefinities**. Deze instelling bepaalt hoe de regio werkt samen met Enterprise-Server en de toepassingsdatabases.

2. Met de rechtermuisknop op **XA Resourcedefinities** en selecteer **Server-exemplaar toevoegen**.

3. Selecteer in de vervolgkeuzelijst **Database-Service-exemplaar**. Dit is de lokale computer SQLEXPRESS.

4. Selecteer het exemplaar van onder de **definities van XA-Resource (machinename\\sqlexpress)** container en klik op **toevoegen**.

5. Selecteer **Database XA-Resource Definition** en typ vervolgens **BANKDEMO** voor de **naam** en **regio**.

     ![Nieuwe Database XA-Resource Definition scherm](media/09-demo-xa.png)

6. Klik op het beletselteken (**...** ) om de wizard Connection String. Voor **servernaam**, type **(lokaal)\\SQLEXPRESS**. Voor **aanmelding**, selecteer **Windows-verificatie**. Typ voor de databasenaam, **BANKDEMO**

     ![Verbindingsreeks-scherm voor bewerken](media/10-demo-string.png)

7. Test de verbinding.

## <a name="start-the-bankdemo-region"></a>De regio BANKDEMO starten

> [!NOTE]
> De eerste stap is het belangrijk: De regio voor gebruik van de XA-resourcedefinitie die u zojuist hebt gemaakt, moet u instellen.

1. Navigeer naar de **BANDEMO CICS regio** onder de **regio's Container**, en selecteer vervolgens **regio opstartbestand bewerken** uit de **acties** deelvenster. Schuif omlaag naar de SQL-eigenschappen en voer **bankdemo** voor de **XA-resourcenaam** , of gebruikt u het beletselteken om deze te selecteren.

2. Klik op de **opslaan** pictogram uw wijzigingen op te slaan.

3. Met de rechtermuisknop op **BANKDEMO CICS regio** in de **Console** deelvenster en selecteer **starten/stoppen regio**.

4. In de onderkant van de **starten/stoppen regio** vak dat wordt weergegeven in het middelste deelvenster, selecteer **Start**. Na enkele seconden, de regio wordt gestart.

     ![Vak SQL starten/stoppen](/media/11-demo-sql.png)

     ![CICS regio BANKDEMO - gestart scherm](media/12-demo-cics.png)

## <a name="create-a-listener"></a>Maak een listener

U moet een listener voor de TN3270-sessies die toegang hebben tot de toepassing BankDemo maken.

1. Vouw in het linkerdeelvenster **configuratie Editors** en selecteer **Listener**.

2. Klik op de **bestand openen** pictogram en selecteer de **seelistener.exe.config** bestand. Dit bestand worden gewijzigd en wordt geladen telkens als Enterprise-Server wordt gestart.

3. U ziet dat de twee regio's eerder gedefinieerd (ESDEMO en JCLDEMO).

4. Voor het maken van een nieuwe regio voor BANKDEMO, met de rechtermuisknop op **regio's**, en selecteer **regio toevoegen**.

5. Selecteer **BANKDEMO regio**.

6. Een kanaal TN3270 toevoegen met de rechtermuisknop op **BANKDEMO regio** en selecteren **kanaal toevoegen**.

7. Voor **naam**, voer **TN3270**. Voor **poort**, voer **9024**. (Houd er rekening mee dat de toepassing ESDEMO gebruikmaakt van poort 9230 dus moet u een andere poort gebruikt.)

8. Om het bestand hebt opgeslagen, klikt u op de **opslaan** pictogram of kies **bestand** \> **opslaan**.

9. Voor het starten van de listener, klikt u op de **Listener starten** pictogram of kies **opties** \> **Listener starten**.

     ![Configuratie-Editor voor listener-windows](media/13-demo-listener.png)


## <a name="configure-rumba-to-access-the-bankdemo-application"></a>Rumba voor toegang tot de toepassing BankDemo configureren

Het laatste wat dat u moet doen is een 3270-sessie met behulp van Rumba, een emulator 3270 configureren. Deze stap kunt u de toepassing BankDemo openen via de listener die u zojuist hebt gemaakt.

1. Vanuit de Windows **Start** in het menu Start Rumba Desktop.

2. Onder de **verbindingen** menu-item, selecteer **TN3270**.

3. Klik op **invoegen** en het type **127.0.0.1** voor het IP-adres en **9024** voor de gebruiker gedefinieerde poort.

4. Aan de onderkant van het dialoogvenster klikt u op **Connect**. Een zwart CICS scherm wordt weergegeven.

5. Type **bank** om het eerste scherm 3270 voor de toepassing BankDemo weer te geven.

6. Voor gebruikers-ID, typt u **B0001** en voor het wachtwoord, typt u. Het eerste scherm BANK20 wordt geopend.

![Mainframe weergave welkomstscherm](media/14-demo.png)
![Mainframe-weergave - Rumba - subsysteem demonstratie scherm](media/15-demo.png)

Gefeliciteerd! U nu uitvoert een CICS toepassing in Azure met behulp van Micro Focus Enterprise-Server.

## <a name="learn-more"></a>Meer informatie

- [Mainframe-migratie - Portal](https://blogs.msdn.microsoft.com/azurecat/2018/11/16/mainframe-migration-to-azure-portal/)
- [Virtuele machines](https://docs.microsoft.com/azure/virtual-machines/linux/overview)
- [Problemen oplossen](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Mainframe ontrafelen aan Azure-migratie](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)
