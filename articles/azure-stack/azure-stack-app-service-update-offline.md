---
title: Bijwerken van de Azure App Service Offline | Microsoft Docs
description: Gedetailleerde richtlijnen voor het bijwerken van Azure App Service in Azure Stack offline
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: anwestg
ms.openlocfilehash: 740a3ce36dde953cb92c52a10cbebf12544adae0
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968149"
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Offline-update van Azure App Service in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!IMPORTANT]
> De update 1809 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service 1.4 implementeert.
>
>

Door de instructies in dit artikel te volgen, kunt u upgraden de [App Service-resourceprovider](azure-stack-app-service-overview.md) in een Azure Stack-omgeving die is geïmplementeerd:

* niet verbonden met Internet
* beveiligd door Active Directory Federation Services (AD FS).

> [!IMPORTANT]
> Vóór de upgrade uitvoert, zorg ervoor dat u al hebt voltooid de [implementatie van de Azure App Service op Azure Stack-Resourceprovider](azure-stack-app-service-deploy-offline.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Voer het installatieprogramma van App Service resource provider

Als u de App Service-resourceprovider in een Azure Stack-omgeving bijwerken, moet u deze taken uitvoeren:

1. Download de [App Service-installatieprogramma](https://aka.ms/appsvcupdate4installer)
2. Maak een upgradepakket voor offline.
3. Voer het App Service-installatieprogramma (appservice.exe) en de upgrade is voltooid.

Tijdens dit proces wordt de upgrade:

* Detecteren van eerdere implementatie van App Service
* Uploaden naar Storage
* Upgrade van alle App Service-rollen (domeincontrollers, beheer, front-end, uitgever en werknemer rollen)
* Definities App Service-schaalset bijwerken
* Manifest van Resourceprovider App Service bijwerken

## <a name="create-an-offline-upgrade-package"></a>Maak een offlinepakket bijwerken

Als u App Service bijwerken in een omgeving zonder verbinding, moet u eerst een upgradepakket voor offline maken op een computer die verbonden met Internet.

1. Appservice.exe uitvoeren als beheerder

    ![App Service-installatieprogramma][1]

2. Klik op **Geavanceerd** > **offlinepakket maken**

    ![App Service-installatieprogramma geavanceerde][2]

3. Het App Service-installatieprogramma maakt een offline-upgradepakket en het pad naar deze wordt weergegeven.  U kunt klikken op **map openen** de map in de Verkenner te openen.

4. Kopieer het installatieprogramma (AppService.exe) en het offline-upgradepakket naar uw Azure Stack-hostmachine.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Voltooi de upgrade van App Service in Azure Stack

> [!IMPORTANT]
> Het installatieprogramma van de App Service moet worden uitgevoerd op een computer die de Azure Stack beheerder van Azure Resource Manager-eindpunt kan bereiken.
>
>

1. Appservice.exe uitvoeren als beheerder.

    ![App Service-installatieprogramma][1]

2. Klik op **Geavanceerd** > **uitvoeren van offline-installatie of upgrade**.

    ![App Service-installatieprogramma geavanceerde][2]

3. Blader naar de locatie van het offline upgradepakket die u eerder hebt gemaakt en klik vervolgens op **volgende**.

4. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

5. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

6. Zorg ervoor dat de Azure Stack Azure Resource Manager-eindpunt en de Active Directory-Tenant informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast tijdens de implementatie van Azure Stack, moet u de waarden in dit venster bewerken. Bijvoorbeeld, als u het domeinachtervoegsel *mycloud.com*, uw Azure Stack Azure Resource Manager-eindpunt moet wijzigen in *management.region.mycloud.com*. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![Informatie over de Cloud van Azure Stack][3]

7. Op de volgende pagina:

   1. Klik op de **Connect** naast de **Azure Stack-abonnementen** vak.
        * Als u Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure Stack. Klik op **aanmelden**.
        * Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld _cloudadmin@azurestack.local_. Voer uw wachtwoord in en klikt u op **aanmelden**.
   2. In de **Azure Stack-abonnementen** Schakel de **Providerabonnement standaard**.
   3. In de **Azure Stack-locaties** vak, selecteert u de locatie die overeenkomt met de regio die u naar implementeert. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure Stack Development Kit.
   4. Als een bestaande App Service-implementatie wordt gedetecteerd, klikt u vervolgens de resource-account voor groep en de opslag ingevuld en grijs.
   5. Klik op **volgende** om te controleren van de samenvatting van de upgrade.

    ![App Service-installatie gedetecteerd][4]

8. Op de pagina Samenvatting:
   1. Controleer of de selecties die u hebt gemaakt. Als u wilt wijzigen, gebruikt u de **vorige** knoppen om naar de vorige pagina's te gaan.
   2. Als de configuratie juist zijn, selecteert u het selectievakje in.
   3. De upgrade starten, klikt u op **volgende**.

       ![Samenvatting van de App Service-Upgrade][5]

9. De voortgangspagina van upgrade:
    1. Voortgang van de upgrade bijhouden. De duur van de upgrade van de App Service in Azure Stack varieert afhankelijk van het aantal rolexemplaren die zijn geïmplementeerd.
    2. Nadat de upgrade is voltooid, klikt u op **afsluiten**.

        ![App Service de voortgang van Upgrade][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

* [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)
