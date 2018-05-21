---
title: Bijwerken op Azure Stack-Azure App Service | Microsoft Docs
description: Gedetailleerde richtlijnen voor het bijwerken van Azure App Service op Azure-Stack
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
ms.date: 05/18/2018
ms.author: anwestg
ms.openlocfilehash: c822f25e25953b8709f481e51d6a63e6a912a60a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="update-azure-app-service-on-azure-stack"></a>Op Azure Stack-Azure App Service bijwerken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

> [!IMPORTANT]
> De update 1804 toepassen op uw Azure-Stack geïntegreerd systeem of de nieuwste Azure-Stack development kit implementeren voordat u Azure App Service 1.2 implementeert.
>
>

Volg de instructies in dit artikel, kunt u upgraden de [App Service-resourceprovider](azure-stack-app-service-overview.md) geïmplementeerd in een Azure-Stack-omgeving die is verbonden met Internet.

> [!IMPORTANT]
> Voordat u de upgrade uitvoert, zorg ervoor dat u al hebt voltooid de [implementatie van de Azure App Service op Azure Stack van Resource Provider](azure-stack-app-service-deploy.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Voer het App Service resource provider-installatieprogramma

Tijdens dit proces wordt de upgrade:

* Detecteren van eerdere implementatie van App Service
* Alle updatepakketten en nieuwe versies van alle OSS-bibliotheken voor implementatie voorbereiden
* Uploaden naar de opslag
* Upgraden van alle functies van de App Service (domeincontrollers, beheer, front-, uitgever en Worker rollen)
* Definities App Service-schaalset bijwerken
* App Service Provider Bronmanifest bijwerken

> [!IMPORTANT]
> Het installatieprogramma van de App Service moet worden uitgevoerd op een computer die het Azure Stack beheerder Azure Resource Manager-eindpunt kan bereiken.
>
>

Upgrade van uw implementatie van App-Service op Azure-Stack moet de volgende stappen uit:

1. Download de [App Service-installatie](https://aka.ms/appsvcupdate2installer)

2. Appservice.exe als administrator uitvoeren

    ![App Service-installatie][1]

3. Klik op **of upgrade naar de nieuwste versie van de App Service implementeren.**

4. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

5. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

6. Zorg ervoor dat het Azure Stack Azure Resource Manager-eindpunt en de Active Directory-Tenant informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast wanneer u Azure-Stack geïmplementeerd, moet u de waarden in dit venster om weer te geven die bewerken. Bijvoorbeeld, als u het domeinachtervoegsel *mycloud.com*, uw Azure Stack Azure Resource Manager-eindpunt moet wijzigen in *management.region.mycloud.com*. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![Informatie over de Stack van Azure-Cloud][2]

7. Op de volgende pagina:

   1. Klik op de **Connect** naast de **Stack-Azure-abonnementen** vak.
        * Als u gebruikmaakt van Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure-Stack. Klik op **aanmelden**.
        * Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld: *cloudadmin@azurestack.local*. Voer uw wachtwoord in en klikt u op **aanmelden**.
   2. In de **Stack-Azure-abonnementen** de optie de **Provider-abonnement standaard**.
   3. In de **Azure stacklocaties** Selecteer de locatie die overeenkomt met de regio die u implementeert op. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure-Stack Development Kit.
   4. Als een bestaande App Service-implementatie wordt gedetecteerd, vervolgens de resource-account voor groep en opslag ingevuld en grijs.
   5. Klik op **volgende** naar het overzicht van de upgrade.

    ![App Service-installatie gedetecteerd][3]

8. Op de pagina overzicht:
   1. Controleer of de selecties die u hebt aangebracht. Als u wilt wijzigen, gebruikt u de **vorige** knoppen naar vorige pagina's te gaan.
   2. Als de configuraties correct zijn, schakel het selectievakje in.
   3. Voor het starten van de upgrade, klikt u op **volgende**.

       ![App Service-Upgradeoverzicht][4]

9. De voortgangspagina van een upgrade uitvoeren:
    1. Voortgang van de upgrade bijhouden. De duur van de upgrade van App-Service op Azure-Stack varieert afhankelijk van het aantal rolexemplaren die zijn geïmplementeerd.
    2. Nadat de upgrade is voltooid, klikt u op **afsluiten**.

        ![Voortgang van de App Service-Upgrade][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

* [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)
