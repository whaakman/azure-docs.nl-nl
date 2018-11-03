---
title: Hoe u een constante virtuele IP-adres voor een Azure-cloud-service behouden | Microsoft Docs
description: Leer hoe u om ervoor te zorgen dat het virtuele IP-adres (VIP) van uw Azure-cloud-service niet wijzigen.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 348e13c919b85018388c8032ea697c8ad69f70f9
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969249"
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Een constante virtuele IP-adres voor een Azure-cloud-service behouden
Wanneer u een cloudservice die wordt gehost in Azure hebt bijgewerkt, moet u mogelijk om ervoor te zorgen dat het virtuele IP-adres (VIP) van de service niet wijzigen. Veel domain management services gebruiken de Domain Name System (DNS) voor het registreren van domeinnamen. DNS werkt alleen als het VIP-adres hetzelfde blijft. U kunt de **Wizard publiceren** in Azure-hulpprogramma's om ervoor te zorgen dat het VIP van uw service in de cloud niet wanneer verandert u deze bijwerken. Zie voor meer informatie over het gebruik van DNS-domein-beheer voor cloudservices, [configureren van een aangepaste domeinnaam voor een Azure-cloudservice](cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Een service in de cloud publiceren zonder dat bijbehorende VIP worden gewijzigd
Het VIP van een service in de cloud wordt toegewezen wanneer u eerst deze op Azure in een bepaalde omgeving, zoals de productie-omgeving implementeren. Het VIP-adres verandert alleen als u de implementatie expliciet verwijderen of de implementatie impliciet door het proces van de update wordt verwijderd. Als u wilt het VIP-adres behouden, moet u de implementatie niet verwijderen, en moet u ervoor zorgen dat Visual Studio automatisch de implementatie niet verwijderen. 

U kunt opgeven dat implementatie-instellingen in de **Wizard publiceren**, die ondersteuning biedt voor verschillende implementatieopties. U kunt een nieuwe implementatie of een update-implementatie, die incrementele of gelijktijdige worden kan opgeven. Beide soorten update-implementatie behouden het VIP. Zie voor definities van deze verschillende typen implementatie [Wizard voor Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md). Bovendien kunt u bepalen of de vorige implementatie van een cloudservice is verwijderd als er een fout optreedt. Als u niet goed deze optie instelt, kan dat het VIP onverwacht worden gewijzigd.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Een cloudservice bijwerken zonder dat bijbehorende VIP worden gewijzigd
1. Maak of open een Azure-cloud service-project in Visual Studio. 

2. In **Solution Explorer**, met de rechtermuisknop op het project. Selecteer in het snelmenu **publiceren**.

    ![Het menu Publish](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. In de **Publish Azure Application** dialoogvenster vak, selecteert u het Azure-abonnement dat u wilt implementeren. Meld u indien nodig, en selecteer **volgende**.

    ![Azure-toepassing aanmelden pagina publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. Op de **algemene instellingen die u** tabblad, controleert u of dat de naam van de cloud service op die u implementeert, wordt de **omgeving**, wordt de **buildconfiguratie**, en de **Serviceconfiguratie** juist zijn.

    ![Tabblad algemene instellingen voor Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. Op de **geavanceerde instellingen** tabblad, Controleer de **implementatielabel** en de **opslagaccount** juist zijn. Controleer de **implementatie bij fout verwijderen** selectievakje is uitgeschakeld en controleer de **implementatie update** selectievakje is ingeschakeld. Door uit te schakelen de **implementatie bij fout verwijderen** selectievakje, zorgt u ervoor dat uw VIP niet verloren gaan als er een fout optreedt tijdens de implementatie. Door het selecteren van de **implementatie update** selectievakje, die u ervoor zorgen dat uw implementatie wordt niet verwijderd en de VIP niet verloren wanneer u uw toepassing opnieuw publiceert. 

    ![Tabblad Geavanceerde instellingen voor Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. Als u meer hoe u wilt dat de rollen worden bijgewerkt, schakelt **instellingen** naast **implementatie update**. Selecteer een **incrementele update** of **gelijktijdig bijwerken**, en selecteer **OK**. Kies **incrementele update** elk exemplaar van uw toepassing bijwerken na elkaar, zodat de toepassing altijd beschikbaar is. Kies **gelijktijdig bijwerken** om bij te werken van alle exemplaren van uw toepassing op hetzelfde moment. Gelijktijdige bijwerken is sneller, maar de service mogelijk niet beschikbaar tijdens het updateproces. Wanneer u klaar bent, selecteert u **volgende**.

    ![Pagina implementatie-instellingen voor Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. In de **Publish Azure Application** in het dialoogvenster, selecteer **volgende** totdat de **samenvatting** pagina wordt weergegeven. Controleer de instellingen en selecteer vervolgens **publiceren**.
   
    ![De pagina overzicht van Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Volgende stappen
- [Wizard Azure-toepassing met Visual Studio publiceren](vs-azure-tools-publish-azure-application-wizard.md)

