---
title: bestand opnemen
description: bestand opnemen
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 99ca7e82a11687d25355589e7ea539a14cdb493b
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420757"
---
1. Download de client SDK-snelstartgidsen voor de volgende platformen:
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > Als u het iOS-project moet u voor het downloaden van ' azuresdk-iOS -\*.zip "van [meest recente versie van GitHub](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest). Pak deze uit en voeg de `MicrosoftAzureMobile.framework` -bestand naar de hoofdmap van het project.
    >

2. U moet een databaseverbinding toevoegen of verbinding maken met een bestaande verbinding. Bepaal eerst of u moet maken van een gegevensarchief of gebruik een bestaande resourcegroep.

    - **Een nieuw gegevensarchief maken**: Als u maken van een gegevensarchief wilt, gebruikt u de volgende Quick Start:

        [Snelstart: Aan de slag met individuele databases in Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

    - **Bestaande gegevensbron**: Volg de onderstaande instructies als u wilt gebruiken van een bestaande databaseverbinding

        1. De verbindingsreeks voor de SQL-Database-indeling; `Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}**  Naam van de server, dit kan worden gevonden in de overzichtspagina voor uw database en bevindt zich doorgaans in de vorm van 'server_name.database.windows.net'.
            **{poort}**  meestal 1433.
            **{your_catalogue}**  Naam van de database.
            **{your_username}**  Gebruikersnaam voor toegang tot uw database.
            **{your_password}**  Wachtwoord voor toegang tot uw database.

            [Meer informatie over de indeling van de SQL-verbindingsreeks](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. Toevoegen van de verbindingsreeks voor uw **mobiele app** In App Service, u kunt beheren met verbindingsreeksen voor uw toepassing met behulp van de **configuratie** optie in het menu.

            Een verbindingsreeks toevoegen:

            1. Klik op de **toepassingsinstellingen** tabblad.

            2. Klik op **[+] nieuwe verbindingsreeks**.

            3. U moet opgeven **naam**, **waarde** en **Type** voor de verbindingsreeks.

            4. Type **naam** als `MS_TableConnectionString`

            5. Waarde moet de verbindende tekenreeks die u in de stap voordat gevormd.

            6. Als u een verbindingsreeks wilt aan een SQL Azure database toevoegen kiezen **systeemonderhoud plaatsvindt** onder **type**.

3. Azure Mobile Apps heeft SDK's voor back-ends voor .NET en Node.js.

   - **Node.js-back-end**
    
     Als u Node.js Quick Start-app gebruiken gaat, volgt u de onderstaande instructies.

     1. In de Azure-portal, gaat u naar **eenvoudige tabellen**, ziet u dit scherm.
      
        ![Knooppunt eenvoudige tabellen](./media/app-service-mobile-configure-new-backend/node-easy-tables.png)

     2. Zorg ervoor dat de SQL-verbindingsreeks is al toegevoegd aan de **configuratie** tabblad. Schakel het selectievakje van **ik erken dat hiermee alle site-inhoud wordt overschreven** en klikt u op de **takentabel maken** knop.
     
        ![Eenvoudige tabellen knooppuntconfiguratie](./media/app-service-mobile-configure-new-backend/node-easy-tables-configuration.png)

     3. In **eenvoudige tabellen**, klikt u op de **+ toevoegen** knop.
    
        ![Eenvoudige Knooppunttabellen toevoegen knop](./media/app-service-mobile-configure-new-backend/node-easy-tables-add.png)

     4. Maak een `TodoItem` tabel met anonieme toegang.
      
        ![Eenvoudige Knooppunttabellen toevoegen tabel](./media/app-service-mobile-configure-new-backend/node-easy-tables-table-add.png)

   - **.NET-back-end**
    
        Als u .NET Quick Start-app gebruiken gaat, volgt u de onderstaande instructies.

        1. Downloaden van het project Azure Mobile Apps .NET-server uit de [opslagplaats voor azure-mobile-apps-quickstarts](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart).

        2. Bouw het project .NET-server lokaal in Visual Studio.

        3. Open in Visual Studio Solution Explorer, met de rechtermuisknop op `ZUMOAPPNAMEService` project, klikt u op **publiceren**, ziet u een `Publish to App Service` venster. Als u op een Mac werkt, bekijk dan andere manieren om de app te implementeren [hier](https://docs.microsoft.com/en-us/azure/app-service/deploy-local-git).
        
           ![Visual studio publiceren](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Selecteer **App Service** publiceren doel, klik vervolgens op **bestaande selecteren**, klikt u vervolgens op de **publiceren** knop aan de onderkant van het venster.

        5. U moet zich eerst naar Visual Studio met uw Azure-abonnement. Selecteer de `Subscription`, `Resource Group`, en selecteer vervolgens de naam van uw app. Wanneer u klaar bent, klikt u op **OK**, hierdoor wordt de .NET-server-project dat u lokaal geïmplementeerd in de back-end van de App Service. Wanneer de implementatie is voltooid, wordt u omgeleid naar `http://{zumoappname}.azurewebsites.net/` in de browser.
        
           ![Back-end is](./media/app-service-mobile-configure-new-backend/backend-is-up.png)
