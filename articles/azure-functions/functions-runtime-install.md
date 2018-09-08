---
title: Azure Functions-Runtime-installatie | Microsoft Docs
description: Het installeren van de Azure Functions-Runtime preview 2
services: functions
author: apwestgarth
manager: stefsch
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 1ad1d2c74be97afcb62f3f8e8161111f4938f645
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093665"
---
# <a name="install-the-azure-functions-runtime-preview-2"></a>Installeer de Azure Functions-Runtime preview 2

Als u de preview van Azure Functions-Runtime 2 hebt geïnstalleerd wilt, volgt u deze stappen:

1. Zorg ervoor dat de machine voldoet aan de minimale vereisten voldoet.
1. Download de [Azure Functions-Runtime Preview installatieprogramma](https://aka.ms/azafrv2).
1. Verwijder de Azure Functions-Runtime preview 1.
1. Installeer de Azure Functions-Runtime preview 2.
1. Voltooi de configuratie van de preview van Azure Functions-Runtime 2.
1. Uw eerste functie maken in Azure Functions Runtime-Preview

## <a name="prerequisites"></a>Vereisten

Voordat u de Azure Functions-Runtime-preview hebt geïnstalleerd, moet u de volgende resources beschikbaar hebben:

1. Een machine met Microsoft Windows Server 2016 of Microsoft Windows 10 Creators Update (Professional of Enterprise Edition).
1. Een exemplaar van SQL Server in uw netwerk.  Minimale versie vereist is SQL Server Express.

## <a name="uninstall-previous-version"></a>Vorige versie verwijderen

Als u eerder de Azure Functions-Runtime-preview hebt geïnstalleerd, moet u deze verwijderen voordat u de meest recente versie installeert.  Verwijder de preview van Azure Functions-Runtime door het verwijderen van het programma in programma's toevoegen/verwijderen in Windows.

## <a name="install-the-azure-functions-runtime-preview"></a>De Preview van Azure Functions Runtime installeren

Het installatieprogramma van Azure Functions Runtime Preview helpt u bij de installatie van de Azure Functions-Runtime preview beheer- en werkrollen.  Het is mogelijk de beheer- en werkrollen-functie installeren op dezelfde computer.  Als u meer functie-apps toevoegt, moet u echter meer werkrollen op extra computers voor het schalen van uw functies naar meerdere werknemers kunnen implementeren.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Het beheer en de Werkrol op dezelfde computer installeren

1. Voer het installatieprogramma van Azure Functions Runtime Preview-versie.

    ![Installatieprogramma voor Azure Functions-Runtime-preview][1]

1. Klik op **Volgende**.
1. Zodra u lees de voorwaarden van de **overeenkomst**, **Schakel het selectievakje in** de voorwaarden accepteren en klik op **volgende** om door te gaan.
1. Selecteer de rollen die u wilt installeren op deze machine **functies Beheerrol** en/of **functies Werkrol** en klikt u op **volgende**.

    ![Azure Functions-Runtime preview installer - cloudrol kiezen][3]

    > [!NOTE]
    > U kunt installeren de **functies Werkrol** op veel andere computers. Volg deze instructies om dit te doen, en selecteer alleen de **functies Werkrol** in het installatieprogramma.

1. Klik op **volgende** hebben de **Azure Functions-Runtime-installatiewizard** beginnen met de installatie op uw computer.
1. Als u klaar bent met de wizard setup wordt gestart de **Azure Functions-Runtime** hulpprogramma voor serverconfiguratie.

    ![Installatieprogramma voor Azure Functions-Runtime preview voltooien][6]

    > [!NOTE]
    > Als u wilt installeren op **Windows 10** en de **Container** functie is niet eerder is ingeschakeld, de **installatie van Azure Functions Runtime** vraagt u de computer opnieuw opstarten de installatie kunnen voltooien.

## <a name="configure-the-azure-functions-runtime"></a>De Azure Functions-Runtime configureren

Als u wilt de Azure Functions-Runtime-installatie hebt voltooid, moet u de configuratie te voltooien.

1. De **Azure Functions-Runtime** hulpprogramma voor serverconfiguratie laat zien welke functies zijn geïnstalleerd op uw computer.

    ![Azure Functions-Runtime-configuratieprogramma voor Preview-versie][7]

1. Klik op de **Database** tabblad, voert u de verbindingsgegevens voor uw SQL Server-exemplaar, met inbegrip van op te geven een [databasehoofdsleutel](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine), en klikt u op **toepassen**.  Verbinding met een SQL Server-exemplaar is vereist in de volgorde voor de Azure Functions-Runtime een database te maken ter ondersteuning van de Runtime.

    ![Azure Functions-Runtime databaseconfiguratie in de Preview-versie][8]

1. Klik op de **referenties** tabblad.  Hier moet u twee nieuwe referenties voor gebruik met een bestandsshare voor het hosten van al uw functie-apps maken.  Geef **gebruikersnaam** en **wachtwoord** combinaties van de **eigenaar van de bestandsshare** en voor de **gebruiker van de bestandsshare**, klikt u vervolgens op **Toepassen**.

    ![Referenties voor Azure Functions-Runtime-preview][9]

1. Klik op de **bestandsshare** tabblad.  Hier moet u de details van de bestandsshare-locatie.  De bestandsshare kan worden gemaakt voor u of u kunt een bestaande bestandsshare gebruiken en klikt u op **toepassen**.  Als u een nieuwe bestandsshare-locatie hebt geselecteerd, moet u een map voor gebruik door de Azure Functions-Runtime.

    ![Azure Functions-Runtime bestandsshare in de Preview-versie][10]

1. Klik op de **IIS** tabblad.  Op dit tabblad worden de details van de websites in IIS die het configuratieprogramma van Azure Functions-Runtime wordt gemaakt.  U kunt een aangepaste DNS-naam hier voor de preview-portal van Azure Functions-Runtime opgeven.  Klik op **toepassen** om te voltooien.

    ![Voorbeeld van Azure Functions-Runtime IIS][11]

1. Klik op de **Services** tabblad.  Op dit tabblad toont de status van de services in uw Azure Functions-Runtime-configuratieprogramma.  Als de **Activation-Service van Azure Functions Host** is niet uitgevoerd na de initiële configuratie, klikt u op **Service starten**.

    ![Azure Functions-Runtime preview-configuratie voltooid][12]

1. Blader naar de **Azure Functions-Runtime Portal** als `https://<machinename>.<domain>/`.

    ![Azure Functions-Runtime preview-portal][13]

## <a name="create-your-first-function-in-azure-functions-runtime-preview"></a>Uw eerste functie maken in Azure Functions-Runtime-preview

Uw eerste functie maken in Azure Functions-Runtime-preview

1. Blader naar de **Azure Functions-Runtime Portal** als https://<machinename>.<domain> Bijvoorbeeld https://mycomputer.mydomain.com
1. U wordt gevraagd te **aanmelden**, als geïmplementeerd in een domein gebruiken die uw account domeingebruikersnaam en wachtwoord, anders gebruiken uw lokale gebruikersnaam en het wachtwoord voor aanmelding bij de portal.

![Azure Functions-Runtime preview-portal aanmelding][14]

1. Voor het maken van functie-apps, moet u een abonnement maken.  In de bovenste links hoek van de portal, klikt u op de **+** optie naast de abonnementen

![Azure Functions-Runtime preview-portal abonnementen][15]

1. Kies **DefaultPlan**, voer een naam in voor uw abonnement en op **maken**.

![Azure Functions-Runtime preview portal abonnement-abonnement en de naam][16]

1. Al uw functie-apps worden weergegeven in het linkerdeelvenster van de portal.  Voor het maken van een nieuwe functie-App, selecteer de kop **functie-Apps** en klikt u op de **+** optie.

1. Voer een naam in voor uw functie-app, selecteert u het juiste abonnement, kiest u welke versie van de Azure Functions-runtime die u wilt programmeren met en klikt u op **maken**

![Azure Functions-Runtime preview portal nieuwe functie-app][17]

1. Uw nieuwe functie-app wordt weergegeven in het linkerdeelvenster van de portal.  Functies selecteren en klik vervolgens op **nieuwe functie** aan de bovenkant van het middelste deelvenster in de portal.

![Sjablonen van Azure Functions-Runtime-preview][18]

1. Selecteer de Trigger voor Timer-functie, in de rechter flyout naam voor de functie en het schema te wijzigen `*/5 * * * * *` (deze cron-expressie zorgt ervoor dat uw timerfunctie om uit te voeren om de vijf seconden), en klikt u op **maken**

![Azure Functions-Runtime preview nieuwe timer functie configuratie][19]

1. De functie is nu gemaakt.  U kunt het uitvoeringslogboek van uw functie-app weergeven door het uitbreiden van de **log** deelvenster aan de onderkant van de portal.

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
