---
title: Runtime-installatie van Azure Functions | Microsoft Docs
description: Het installeren van de Azure Functions-Runtime
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
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 1e4188313a87d07f396e5f8edc8969dd5da2c436
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-functions-runtime-preview"></a>Installeer de Azure Functions-Runtime-Preview

Als u installeren van de preview van Azure Functions-Runtime wilt, moet u deze stappen volgen:

1. Zorg ervoor dat uw machine voldoet aan de minimale vereisten
1. Download de [Runtime Preview installatieprogramma van Azure Functions](https://aka.ms/azafr). 
1. Installeer de Azure Functions-Runtime-preview
1. Voltooi de configuratie van de Azure Functions-Runtime-preview

## <a name="prerequisites"></a>Vereisten

Voordat u de Azure Functions-Runtime-preview installeert, moet u het volgende hebben:

1. Een computer waarop Microsoft Windows Server 2016 of Microsoft Windows 10 auteurs Update (Professional of Enterprise Edition) wordt uitgevoerd.
1. Een exemplaar van SQL Server die in uw netwerk worden uitgevoerd.  Minimale versie vereiste is SQL Server Express.

## <a name="install-the-azure-functions-runtime-preview"></a>Installeer de Azure Functions-Runtime-Preview

Het installatieprogramma van Azure Functions-Runtime preview begeleidt u bij de installatie van de Azure Functions-Runtime preview beheer- en werkrollen.  Het is mogelijk de beheer- en werkrollen-functie installeren op dezelfde machine.  Als u meer functies toevoegt, moet u echter meer werkrollen op extra computers te schalen van uw functies naar meerdere werknemers kunnen implementeren.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Het beheer en de Werkrol op dezelfde computer installeren

1. Start het installatieprogramma van Azure Functions-Runtime Preview.

    ![Azure Functions-Runtime Preview-installatieprogramma][1]

1. **Klik op volgende** vooraf voorbij de eerste fase van het installatieprogramma
1. Zodra u lees de voorwaarden van de **overeenkomst**, **het selectievakje** de voorwaarden accepteren en **Klik op volgende** om door te gaan.
1. Nu selecteren de rollen die u wilt installeren op deze machine **functies Beheerrol** en/of **functies Werkrol** en **Klik op volgende**

    ![Azure Functions-Runtime Preview Installer - selectie van Systeemrol][3]

    > [!NOTE]
    > U kunt installeren de **functies Werkrol** op veel andere computers om dit te doen, volgt u deze instructies en selecteer alleen de **functies Werkrol** in het installatieprogramma.

1. **Klik op volgende** hebben de **Azure Functions-Runtime-installatieprogramma** installeren op uw computer.
1. Nadat het installatieprogramma start de **Azure Functions-Runtime configuratiehulpprogramma**.

    ![Azure Functions-Runtime Preview installatieprogramma voltooid][5]

    > [!NOTE]
    > Als u wilt installeren op **Windows 10** en de **Container** functie heeft niet eerder is ingeschakeld, de **Azure Functions-Runtime** installatieprogramma vraagt u te starten om uw de machine om de installatie te voltooien.

## <a name="configure-the-azure-functions-runtime"></a>Configureren van de Azure Functions-Runtime

U moet de configuratie te voltooien om de Azure Functions-Runtime-installatie te voltooien.

1. De **Azure Functions-Runtime-configuratieprogramma** ziet u welke functies op uw computer zijn geïnstalleerd.

    ![Azure Functions-Runtime Preview Configuration Tool][6]

1. Klik op de **Database** tabblad, voert u de **Verbindingsdetails voor uw SQL Server-exemplaar** en **Klik op toepassen**.  Dit is vereist in volgorde van de Azure Functions-Runtime voor het maken van een database voor de ondersteuning van de Runtime.
    
    ![Azure Functions-Runtime Preview databaseconfiguratie][7]

1. Klik op de **referenties** tabblad.  In dit scherm moet u twee nieuwe referenties voor gebruik met een bestandsshare maken voor het hosten van uw Azure Functions.  **Geef een gebruikersnaam en wachtwoord** combinaties voor de **Share bestandseigenaar** en voor de **File Share gebruiker** en klik op **toepassen**.

    ![Azure Functions-Runtime Preview referenties][8]

1. Klik op de **bestandsshare** tabblad.  In dit scherm moet u de details van de **bestandsshare-locatie**.  Dit kan worden gemaakt voor u of u kunt een bestaande bestandsshare gebruiken en klik op **toepassen**.  Als u een nieuwe bestandsshare-locatie selecteert, moet u een map voor gebruik door de Azure Functions-Runtime.
    
    ![Azure Functions-Runtime Preview-bestandsshare][9]

1. Klik op de **IIS** tabblad.  Op dit tabblad ziet de details van de websites in IIS die de Azure Functions-Runtime-installatie maakt.  **Klik op toepassen** te voltooien.

    ![Preview van Azure Functions-Runtime IIS][10]

1. Klik op de **Services** tabblad.  Op dit tabblad toont de status van de services in uw Azure Functions-Runtime-installatie.  Als na de initiële configuratie de **Activation-Service van Azure Functions Host** wordt niet uitgevoerd Klik **Service starten**

    ![Azure Functions-Runtime Preview Configruation voltooid][11]

1. Ten slotte bladert u naar de **Azure Functions-Runtime Portal** als`https://<machinename>/`

    ![Preview-Portal voor Azure Functions-Runtime][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png