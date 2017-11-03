---
title: FTP in App Service op Azure Stack inschakelen | Microsoft Docs
description: Stappen voor het voltooien om in te schakelen FTP in App Service op Azure-Stack
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: erikje
ms.openlocfilehash: 9cadc57831ac7f7e5d32b10a4a87dab3fac02958
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-ftp-in-app-service-on-azure-stack"></a>FTP inschakelen in App Service op Azure Stack

Zodra u hebt de App-Service op Azure-Stack is geïmplementeerd als u inschakelen, FTP-publicatie, wilt zodat uw tenants hun toepassingsbestanden en de inhoud kunnen uploaden, moet u er een aantal extra stappen die moeten worden voltooid zijn.  In toekomstige releases wordt deze stappen worden geautomatiseerd.

> [!NOTE]
> Deze stappen zijn voor de Service of een App Service configureren op Azure Stack van Resource Provider Ondernemingsadministrators.

## <a name="enable-ftp"></a>FTP inschakelen

1.  Meld u aan bij de Azure-Stack-portal als de servicebeheerder.
2.  Blader naar **netwerkinterfaces** en selecteer de **FTP-NIC** onder **resourcegroep** - **AppService-LOCAL**. ![Azure Stack-netwerkinterfaces][1]
3.  Opmerking de **openbaar IP-adres** van de **FTP-NIC**. 
![Details van Azure netwerkstack-Interface][2]
4.  Naast Blader naar **virtuele Machines** en selecteer de **FTP0 VM**. ![Azure Stack van virtuele Machines][3]
5.  Open een extern-bureaubladsessie met de virtuele machine via de **Connect** knop en meld u aan de sessie met de Administrator-referenties die u tijdens de implementatie van App Service instellen.  
![Virtuele Machine van Azure Stackdetails][4]
6.  Open **Internet Information Service (IIS) Manager** op de FTP-virtuele machine (VM FTP0).
7.  Onder **Sites** Selecteer **FTP-Site die als host fungeert**.
8.  Open **FTP-Firewallondersteuning**. ![IIS-beheer op App Service FTP0-VM][5]
9.  Geef het openbare IP-adres van de FTP-NIC en klik op **toepassen** ![IIS Manager FTP-Firewallondersteuning][6]

## <a name="validate-the-enabling-of-ftp"></a>Het inschakelen van FTP valideren

1.  Meld u aan bij de Azure-Stack-portal als de service-beheerder of als een tenant.
2.  Blader naar **App Services** en selecteert u een Web-, mobiele of API-App die u hebt gemaakt. ![App Services][7]
3.  In de toepassing details Opmerking de **FTP-hostnaam** en **FTP-/ implementatiegebruiker gebruikersnaam**. ![App Service-App-Details][8]
> [!NOTE]
> Als u een item onder niet ziet **FTP-/ implementatiegebruiker gebruikersnaam**, moet u de implementatie referenties eerste met behulp van de **Implementatiereferenties** Blade.

4.  Open Windows Verkenner, de hostnaam van de FTP-invoeren in de adresbalk bestand bijvoorbeeld ftp://ftp.appservice.azurestack.local
5.  Als u wordt gevraagd Voer de **implementatiereferenties** hebt genoteerd in stap 3, als de functie is ingeschakeld ziet u een mapweergave van de inhoud van de app service-toepassing. ![FTP-bestand weergeven][9]
<!--Image references-->
[1]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interfaces.png
[2]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interface-details.png
[3]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines.png
[4]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines-FTP0-VM.png
[5]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager.png
[6]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager-FTP-Firewall-Support.png
[7]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-services.png
[8]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-service-app-detail.png
[9]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-ftp-file-listing.png
