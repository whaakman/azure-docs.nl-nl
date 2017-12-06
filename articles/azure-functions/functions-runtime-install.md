---
title: Runtime-installatie van Azure Functions | Microsoft Docs
description: Het installeren van de Azure Functions-Runtime-preview 2
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: f8ce27bf28f73818932f2ac9056d4fdd573679e8
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="install-the-azure-functions-runtime-preview-2"></a>Installeer de Azure Functions-Runtime-preview 2

Als u installeren van de Azure Functions-Runtime-preview 2 wilt, volg deze stappen:

1. Zorg ervoor dat uw machine voldoet aan de minimale vereisten voldoet.
1. Download de [Runtime Preview installatieprogramma van Azure Functions](https://aka.ms/azafrv2).
1. De Azure Functions-Runtime-preview 1 verwijderen.
1. De Azure Functions-Runtime-preview 2 installeren.
1. Voltooi de configuratie van de Azure Functions-Runtime-preview 2.
1. Uw eerste functie maken in Azure Functions-Runtime-Preview

## <a name="prerequisites"></a>Vereisten

Voordat u de Azure Functions-Runtime-preview installeert, moet u de volgende beschikbare bronnen hebben:

1. Een computer waarop Microsoft Windows Server 2016 of Microsoft Windows 10 auteurs Update (Professional of Enterprise Edition) wordt uitgevoerd.
1. Een exemplaar van SQL Server die in uw netwerk worden uitgevoerd.  Minimale versie vereist is SQL Server Express.

## <a name="uninstall-previous-version"></a>Vorige versie verwijderen

Als u eerder de evaluatieversie van Azure Functions-Runtime hebt geïnstalleerd, moet u deze verwijderen voordat u de meest recente versie installeert.  De Azure Functions-Runtime-preview verwijderen door het programma te verwijderen in programma's toevoegen/verwijderen in Windows.

## <a name="install-the-azure-functions-runtime-preview"></a>Installeer de Azure Functions-Runtime-Preview

Het installatieprogramma Azure Functions-Runtime Preview begeleidt u bij de installatie van de Azure Functions-Runtime preview beheer- en werkrollen.  Het is mogelijk de beheer- en werkrollen-functie installeren op dezelfde machine.  Als u meer apps van de functie toevoegt, moet u echter meer werkrollen op extra computers te schalen van uw functies naar meerdere werknemers kunnen implementeren.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Het beheer en de Werkrol op dezelfde computer installeren

1. Start het installatieprogramma van Azure Functions-Runtime Preview.

    ![Installatieprogramma van Azure Functions-Runtime-preview][1]

1. Klik op **Volgende**.
1. Zodra u de gebruiksvoorwaarden hebt gelezen de **overeenkomst**, **het selectievakje** aan de voorwaarden accepteren en klik op **volgende** om door te gaan.
1. Selecteer de rollen die u wilt installeren op deze machine **functies Beheerrol** en/of **functies Werkrol** en klik op **volgende**.

    ![Preview-installatieprogramma Azure Functions-Runtime - selectie van Systeemrol][3]

    > [!NOTE]
    > U kunt installeren de **Werkrol functies** op veel andere computers. Volg deze instructies om dit te doen, en selecteer alleen de **functies Werkrol** in het installatieprogramma.

1. Klik op **volgende** hebben de **Azure Functions-Runtime-installatiewizard** beginnen met de installatie op uw computer.
1. De wizard setup wordt gestart als u klaar is, de **Azure Functions-Runtime** hulpprogramma voor serverconfiguratie.

    ![Azure Functions-Runtime preview installatieprogramma voltooien][6]

    > [!NOTE]
    > Als u wilt installeren op **Windows 10** en de **Container** functie heeft niet eerder is ingeschakeld, de **Azure Functions-Runtime Setup** vraagt u de computer opnieuw opstarten de installatie voltooien.

## <a name="configure-the-azure-functions-runtime"></a>Configureren van de Azure Functions-Runtime

Om de Azure Functions-Runtime-installatie, moet u de configuratie te voltooien.

1. De **Azure Functions-Runtime** hulpprogramma voor serverconfiguratie ziet u welke functies op uw computer zijn geïnstalleerd.

    ![Azure Functions-Runtime-configuratieprogramma voor preview][7]

1. Klik op de **Database** tabblad, voert u de verbindingsgegevens voor uw SQL Server-exemplaar opgeven waaronder een [databasehoofdsleutel](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine), en klik op **toepassen**.  Verbinding met een SQL Server-exemplaar is vereist om de Azure Functions-Runtime voor het maken van een database voor de ondersteuning van de Runtime.

    ![Azure Functions-Runtime preview-databaseconfiguratie][8]

1. Klik op de **referenties** tabblad.  Hier moet u twee nieuwe referenties voor gebruik met een bestandsshare voor het hosten van alle functie-apps.  Opgeven **gebruikersnaam** en **wachtwoord** combinaties voor de **share bestandseigenaar** en voor de **file share gebruiker**, klikt u vervolgens op **Toepassen**.

    ![Azure Functions-Runtime preview-referenties][9]

1. Klik op de **bestandsshare** tabblad.  Hier moet u de details van de bestandsshare-locatie.  De bestandsshare worden gemaakt voor u of u kunt een bestaande bestandsshare gebruiken en klik op **toepassen**.  Als u een nieuwe bestandsshare-locatie selecteert, moet u een map voor gebruik door de Azure Functions-Runtime.

    ![Azure Functions-Runtime preview-bestandsshare][10]

1. Klik op de **IIS** tabblad.  Op dit tabblad ziet de details van de websites in IIS die het hulpprogramma Azure Functions-Runtime-configuratie wordt gemaakt.  U kunt aangepaste DNS-hier opgeven een naam voor de preview-portal voor Azure Functions-Runtime.  Klik op **toepassen** te voltooien.

    ![Azure Functions-Runtime preview IIS][11]

1. Klik op de **Services** tabblad.  Op dit tabblad toont de status van de services in uw Azure Functions-Runtime-hulpprogramma voor serverconfiguratie.  Als de **Activation-Service van Azure Functions Host** is niet uitgevoerd na de eerste configuratie, klikt u op **Service starten**.

    ![Azure Functions-Runtime preview-configuratie voltooien][12]

1. Blader naar de **van Azure Functions-Runtime-Portal** als `https://<machinename>.<domain>/`.

    ![Azure Functions-Runtime preview-portal][13]

## <a name="create-your-first-function-in-azure-functions-runtime-preview"></a>Uw eerste functie maken in Azure Functions-Runtime-preview

Uw eerste functie maken in Azure Functions-Runtime-preview

1. Blader naar de **van Azure Functions-Runtime-Portal** als https://<machinename>.<domain> bijvoorbeeld https://mycomputer.mydomain.com
1. U wordt gevraagd te **aanmelden**als geïmplementeerd in een domein gebruiken dat uw account domeingebruikersnaam en wachtwoord, anders gebruiken de lokale gebruikersnaam en het wachtwoord voor aanmelding bij de portal.

![Azure Functions-Runtime preview-portal aanmelding][14]

1. Voor het maken van de functie apps, moet u een abonnement maken.  Klik in het bovenste linkerbenedenhoek weergegeven van de portal op de  **+**  optie naast de abonnementen

![Azure Functions-Runtime preview-portal abonnementen][15]

1. Kies **DefaultPlan**, voer een naam voor uw abonnement en klikt u op **maken**.

![Azure Functions-Runtime preview portal abonnementen en de naam][16]

1. Alle van de functie-apps worden weergegeven in het linkerdeelvenster van de portal.  Voor het maken van een nieuwe functie-App, selecteer de kop **functie Apps** en klik op de  **+**  optie.

1. Voer een naam voor de functie-app, selecteert u het juiste abonnement, kiezen welke versie van de Azure Functions-runtime die u wilt programmeren in en klik op **maken**

![Azure Functions-Runtime preview portal nieuwe functie-app][17]

1. Uw nieuwe functie-app wordt vermeld in het linkerdeelvenster van de portal.  Functies selecteren en klik vervolgens op **nieuwe functie** boven in het middelste deelvenster van de portal.

![Azure Functions-Runtime preview-sjablonen][18]

1. Selecteert u de functie Timertrigger, in het rechter doel naam op voor de functie en het wijzigen van de planning voor `*/5 * * * * *` (deze expressie cron zorgt ervoor dat de timerfunctie uit te voeren om de vijf seconden), en klik op **maken**

![Azure Functions-Runtime preview nieuwe timer functie configuratie][19]

1. De functie is gemaakt.  U kunt het uitvoeringslogboek van uw app functie weergeven door het uitbreiden van de **logboek** deelvenster aan de onderkant van de portal.

![Azure Functions-Runtime preview functie uitvoeren][20]

<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-Progress.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer6-InstallComplete.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[13]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png
[14]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Login.png
[15]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions.png
[16]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions1.png
[17]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewFunctionApp.png
[18]: ./media/functions-runtime-install/AzureFunctionsRuntime_v1FunctionsTemplates.png
[19]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewTimerFunction.png
[20]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_RunningV2Function.png
