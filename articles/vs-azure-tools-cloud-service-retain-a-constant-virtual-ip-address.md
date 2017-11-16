---
title: Hoe een constante virtuele IP-adres voor een Azure cloudservice behouden | Microsoft Docs
description: Informatie over het om ervoor te zorgen dat het virtuele IP-adres (VIP) van uw Azure-cloud-service niet wijzigen.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 96c4ed88db5e24f439825aee5afe457a1e8c81d7
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Een constante virtuele IP-adres voor een Azure cloudservice behouden
Wanneer u een cloudservice die wordt gehost in Azure bijwerkt, moet u mogelijk om ervoor te zorgen dat het virtuele IP-adres (VIP) van de service niet wijzigen. Domain Name System (DNS) veel management domeinservices gebruiken voor het registreren van domeinnamen. DNS werkt alleen als het VIP hetzelfde is gebleven. U kunt de **Wizard publiceren** in hulpprogramma's van Azure om ervoor te zorgen dat het VIP van uw cloudservice niet wanneer wijzigen u deze bijwerken. Zie voor meer informatie over het gebruik van DNS-domein management voor cloudservices [configureren van een aangepaste domeinnaam voor een Azure cloudservice](cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Een cloudservice zonder te wijzigen van de VIP publiceren
Het VIP van een cloudservice wordt toegewezen wanneer u eerst deze naar Azure in een bepaalde omgeving, zoals de productie-omgeving implementeren. Het VIP verandert alleen als u de implementatie expliciet verwijderen of de implementatie wordt impliciet verwijderd door het implementatieproces van de update. Als u wilt behouden het VIP, moet u uw implementatie niet verwijderen, en moet u ervoor zorgen dat Visual Studio automatisch de implementatie niet verwijderen. 

U kunt opgeven dat implementatie-instellingen in de **Wizard publiceren**, die ondersteuning biedt voor verschillende implementatieopties. U kunt een nieuwe implementatie of een update-implementatie, kan dit incrementele of gelijktijdige opgeven. Beide soorten update-implementatie behouden de VIP. Zie voor definities van deze verschillende soorten implementatie [Publish Azure Application Wizard](vs-azure-tools-publish-azure-application-wizard.md). Bovendien kunt u bepalen of de vorige implementatie van een cloudservice is verwijderd als er een fout optreedt. Als u deze optie niet correct hebt ingesteld, is het mogelijk dat het VIP onverwacht gewijzigd.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Een cloudservice bijwerken zonder te wijzigen van de VIP
1. Maak of open een Azure-cloud service-project in Visual Studio. 

2. In **Solution Explorer**, met de rechtermuisknop op het project. Selecteer in het snelmenu **publiceren**.

    ![Het menu Publish](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. In de **Publish Azure Application** dialoogvenster Selecteer het Azure-abonnement waaraan u wilt implementeren. Meld u aan als nodig en selecteer **volgende**.

    ![Azure Application aanmeldingspagina publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. Op de **algemene instellingen** tabblad, controleert u of dat de naam van de cloud-service op die u implementeert, wordt de **omgeving**, wordt de **buildconfiguratie**, en de **serviceconfiguratie** juist zijn.

    ![Tabblad algemene instellingen voor Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. Op de **geavanceerde instellingen** tabblad, Controleer de **implementatielabel** en de **opslagaccount** juist zijn. Controleer de **verwijderen van de implementatie mislukt** selectievakje is uitgeschakeld en controleer of de **implementatie-update** selectievakje is ingeschakeld. Door het selectievakje de **verwijderen van de implementatie mislukt** selectievakje u ervoor zorgen dat uw VIP niet verloren gaan als er een fout optreedt tijdens de implementatie. U selecteert de **implementatie-update** selectievakje, zorgt u ervoor dat uw implementatie wordt niet verwijderd en uw VIP niet verloren wanneer u uw toepassing opnieuw publiceren. 

    ![Tabblad Geavanceerde instellingen voor Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. U kunt meer opgeven hoe u wilt dat de rollen worden bijgewerkt, **instellingen** naast **implementatie-update**. Selecteer een **incrementele update** of **gelijktijdig bijwerken**, en selecteer **OK**. Kies **incrementele update** elk exemplaar van uw toepassing bijwerken achter elkaar, zodat de toepassing altijd beschikbaar is. Kies **gelijktijdig bijwerken** om bij te werken van alle exemplaren van uw toepassing op hetzelfde moment. Gelijktijdige bijwerken is sneller, maar de service mogelijk niet beschikbaar tijdens het updateproces. Wanneer u klaar bent, selecteert u **volgende**.

    ![Pagina implementatie-instellingen van Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. In de **Publish Azure Application** dialoogvenster, **volgende** totdat de **samenvatting** pagina wordt weergegeven. Controleer de instellingen en selecteer vervolgens **publiceren**.
   
    ![Pagina overzicht van de Azure-toepassing publiceren](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Volgende stappen
- [Wizard Azure-toepassing met Visual Studio publiceren](vs-azure-tools-publish-azure-application-wizard.md)

