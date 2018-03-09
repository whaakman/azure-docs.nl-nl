---
title: Azure App Service Offline bijwerken | Microsoft Docs
description: Gedetailleerde richtlijnen voor het bijwerken van Azure App Service op Azure-Stack offline
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: anwestg
ms.openlocfilehash: 5b71c9fd58636e9871bf592904d5dca4bc0ec966
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Offline bijwerken van de Azure App Service op Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Volg de instructies in dit artikel, kunt u upgraden de [App Service-resourceprovider](azure-stack-app-service-overview.md) in een Azure-Stack-omgeving die is geïmplementeerd:

* niet verbonden met Internet
* beveiligd door Active Directory Federation Services (AD FS).

> [!IMPORTANT]
> Voordat u de upgrade uitvoert, zorg ervoor dat u al hebt voltooid de [implementatie van de Azure App Service op Azure Stack van Resource Provider](azure-stack-app-service-deploy-offline.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Voer het App Service resource provider-installatieprogramma

Als u de App Service-resourceprovider in een Azure-Stack-omgeving upgraden, moet u deze taken uitvoeren:

1. Download de [App Service-installatie](https://aka.ms/appsvcupdate1installer)
2. Een upgradepakket voor het offline maken.
3. Voer het App Service-installatieprogramma (appservice.exe) en de upgrade is voltooid.

Tijdens dit proces wordt de upgrade:

* Detecteren van eerdere implementatie van App Service
* Uploaden naar de opslag
* Upgraden van alle functies van de App Service (domeincontrollers, beheer, front-, uitgever en Worker rollen)
* App Service-scale set definities bijwerken
* App Service Provider Bronmanifest bijwerken

## <a name="create-an-offline-upgrade-package"></a>Een upgradepakket voor het offline maken

Als u wilt upgraden App Service in een omgeving zonder verbinding, moet u eerst een upgradepakket voor het offline maken op een computer die verbonden met Internet.

1. Appservice.exe als administrator uitvoeren

    ![App Service-installatie][1]

2. Klik op **Geavanceerd** > **offline pakket maken**

    ![Geavanceerde App Service-installatieprogramma][2]

3. Het App Service-installatieprogramma maakt een upgradepakket voor het offline en geeft u het pad weer aan.  U kunt klikken op **map openen** de map in de Verkenner openen.

4. Kopieer het installatieprogramma (AppService.exe) en het offline upgradepakket naar uw Azure-Stack-hostcomputer.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Voltooi de upgrade van App-Service op Azure-Stack

> [!IMPORTANT]
> Het installatieprogramma van de App Service moet worden uitgevoerd op een computer die het Azure-Stack beheerder Azure Resource Manager-eindpunt kan bereiken.
>
>

1. Appservice.exe uitvoeren als beheerder.  

    ![App Service-installatie][1]

2. Klik op **Geavanceerd** > **offline-installatie of upgrade voltooien**.

    ![Geavanceerde App Service-installatieprogramma][2]

3. Blader naar de locatie van het offline upgradepakket die u eerder hebt gemaakt en klik vervolgens op **volgende**.

4. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

5. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

6. Zorg ervoor dat het Azure Stack Azure Resource Manager-eindpunt en de Active Directory-Tenant informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast wanneer u Azure-Stack geïmplementeerd, moet u de waarden in dit venster om weer te geven die bewerken. Bijvoorbeeld, als u het domeinachtervoegsel *mycloud.com*, uw Azure Stack Azure Resource Manager-eindpunt moet wijzigen in *management.region.mycloud.com*. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![Informatie over de Stack van Azure-Cloud][3]

7. Op de volgende pagina:

   1. Klik op de **Connect** naast de **Stack-Azure-abonnementen** vak.
        * Als u gebruikmaakt van Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure-Stack. Klik op **aanmelden**.
        * Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld:  *cloudadmin@azurestack.local* . Voer uw wachtwoord in en klikt u op **aanmelden**.
   2. In de **Stack-Azure-abonnementen** Selecteer uw abonnement.
   3. In de **Azure stacklocaties** Selecteer de locatie die overeenkomt met de regio die u implementeert op. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure-Stack Development Kit.
   4. Als een bestaande App Service-implementatie wordt gedetecteerd, vervolgens de resource-account voor groep en opslag ingevuld en grijs.
   5. Klik op **volgende** naar het overzicht van de upgrade.

    ![App Service-installatie gedetecteerd][4]

8. Op de pagina overzicht:
   1. Controleer of de selecties die u hebt aangebracht. Als u wilt wijzigen, gebruikt u de **vorige** knoppen naar vorige pagina's te gaan.
   2. Als de configuraties correct zijn, schakel het selectievakje in.
   3. Voor het starten van de upgrade, klikt u op **volgende**.

       ![App Service-Upgradeoverzicht][5]

9. De voortgangspagina van een upgrade uitvoeren:
    1. Voortgang van de upgrade bijhouden. De duur van de upgrade van App-Service op Azure-Stack varieert afhankelijk van het aantal rolexemplaren die zijn geïmplementeerd.
    2. Nadat de upgrade is voltooid, klikt u op **afsluiten**.

        ![Voortgang van de App Service-Upgrade][6]

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
