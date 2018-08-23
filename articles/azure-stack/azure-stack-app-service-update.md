---
title: Bijwerken van de Azure App Service op Azure Stack | Microsoft Docs
description: Gedetailleerde richtlijnen voor het bijwerken van Azure App Service in Azure Stack
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
ms.date: 08/15/2018
ms.author: anwestg
ms.openlocfilehash: f12eb7d74d9c47567c72cddda8d2813f394fb1de
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42055071"
---
# <a name="update-azure-app-service-on-azure-stack"></a>Bijwerken van de Azure App Service op Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!IMPORTANT]
> De update 1807 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service 1.3 implementeert.
>
>

Door de instructies in dit artikel te volgen, kunt u upgraden de [App Service-resourceprovider](azure-stack-app-service-overview.md) geïmplementeerd in een Azure Stack-omgeving die is verbonden met Internet.

> [!IMPORTANT]
> Vóór de upgrade uitvoert, zorg ervoor dat u al hebt voltooid de [implementatie van de Azure App Service op Azure Stack-Resourceprovider](azure-stack-app-service-deploy.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Voer het installatieprogramma van App Service resource provider

Tijdens dit proces wordt de upgrade:

* Detecteren van eerdere implementatie van App Service
* Alle updatepakketten en nieuwe versies van alle OSS-bibliotheken voor implementatie voorbereiden
* Uploaden naar Storage
* Upgrade van alle App Service-rollen (domeincontrollers, beheer, front-end, uitgever en werknemer rollen)
* Definities App Service-schaalset bijwerken
* Manifest van Resourceprovider App Service bijwerken

> [!IMPORTANT]
> Het installatieprogramma van de App Service moet worden uitgevoerd op een computer die de Azure Stack beheerder van Azure Resource Manager-eindpunt kan bereiken.
>
>

Als u wilt upgraden van uw implementatie van App Service in Azure Stack, de volgende stappen uit:

1. Download de [App Service-installatieprogramma](https://aka.ms/appsvcupdate3installer)

2. Appservice.exe uitvoeren als beheerder

    ![App Service-installatieprogramma][1]

3. Klik op **of upgrade naar de nieuwste versie van de App Service implementeren.**

4. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

5. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

6. Zorg ervoor dat de Azure Stack Azure Resource Manager-eindpunt en de Active Directory-Tenant informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast tijdens de implementatie van Azure Stack, moet u de waarden in dit venster om weer te geven die bewerken. Bijvoorbeeld, als u het domeinachtervoegsel *mycloud.com*, uw Azure Stack Azure Resource Manager-eindpunt moet wijzigen in *management.region.mycloud.com*. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![Informatie over de Cloud van Azure Stack][2]

7. Op de volgende pagina:

   1. Klik op de **Connect** naast de **Azure Stack-abonnementen** vak.
        * Als u Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure Stack. Klik op **aanmelden**.
        * Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld *cloudadmin@azurestack.local*. Voer uw wachtwoord in en klikt u op **aanmelden**.
   2. In de **Azure Stack-abonnementen** Schakel de **Providerabonnement standaard**.
   3. In de **Azure Stack-locaties** vak, selecteert u de locatie die overeenkomt met de regio die u naar implementeert. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure Stack Development Kit.
   4. Als een bestaande App Service-implementatie wordt gedetecteerd, klikt u vervolgens de resource-account voor groep en de opslag ingevuld en grijs.
   5. Klik op **volgende** om te controleren van de samenvatting van de upgrade.

    ![App Service-installatie gedetecteerd][3]

8. Op de pagina Samenvatting:
   1. Controleer of de selecties die u hebt gemaakt. Als u wilt wijzigen, gebruikt u de **vorige** knoppen om naar de vorige pagina's te gaan.
   2. Als de configuratie juist zijn, selecteert u het selectievakje in.
   3. De upgrade starten, klikt u op **volgende**.

       ![Samenvatting van de App Service-Upgrade][4]

9. De voortgangspagina van upgrade:
    1. Voortgang van de upgrade bijhouden. De duur van de upgrade van de App Service in Azure Stack varieert afhankelijk van het aantal rolexemplaren die zijn geïmplementeerd.
    2. Nadat de upgrade is voltooid, klikt u op **afsluiten**.

        ![App Service de voortgang van Upgrade][5]

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
