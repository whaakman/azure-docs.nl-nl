---
title: Wizard Azure-toepassing met Visual Studio publiceren | Microsoft Docs
description: Informatie over het configureren van de verschillende instellingen in de Visual Studio Azure Wizard toepassing publiceren
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 25b3ca9af2639860d9cfcb1492aef745fb47beb9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-visual-studio-publish-azure-application-wizard"></a>Wizard Azure-toepassing met Visual Studio publiceren
Nadat u een webtoepassing in Visual Studio ontwikkelen, kunt u deze toepassing naar een Azure cloudservice publiceren met behulp van de **Publish Azure Application** wizard. 

> [!NOTE]
> Dit onderwerp gaat over het implementeren van cloud-services, niet naar websites. Zie voor meer informatie over het implementeren van websites [het implementeren van een Azure-website](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).
> 
> 

## <a name="accessing-the-publish-azure-application-wizard"></a>Toegang tot de wizard Azure-toepassing publiceren

De wizard Publish Azure Application op twee manieren afhankelijk van het type van de Visual Studio-project hebt, kunt u openen.

**Als u een Azure-cloud service-project hebt:**

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **publiceren**.

**Als u een toepassing webproject dat niet is ingeschakeld voor Azure hebt:**

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **converteren** > **converteren naar Azure-Cloudserviceproject**. 

1. In **Solution Explorer**, met de rechtermuisknop op de zojuist gemaakte Azure-project en selecteer in het contextmenu **publiceren**.

## <a name="sign-in-page"></a>Aanmeldingspagina

![Aanmeldingspagina](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Account** : Selecteer een account of selecteer **account toevoegen** in de vervolgkeuzelijst van het account.

**Kies uw abonnement** -kiest u het abonnement moet worden gebruikt voor uw implementatie.
   
## <a name="settings-page---common-settings-tab"></a>Voor instellingenpagina - tabblad algemene instellingen   

![Algemene instellingen](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Cloudservice** -gebruik van de vervolgkeuzelijst, selecteer een bestaande cloud service of selecteer  **&lt;Nieuw >**, en maak een cloudservice. Het datacenter wordt tussen haakjes weergegeven voor elke cloudservice. Het wordt aanbevolen dat de datacentrum locatie voor de cloudservice niet hetzelfde zijn als de locatie van de center voor het opslagaccount (geavanceerde instellingen).  

**Omgeving** -optie **productie** of **fasering**. Kies de faseringsomgeving als u wilt implementeren van uw toepassing in een testomgeving. 

**Configuratie bouwen** -optie **Debug** of **Release**.

**Configuratie van de service** -optie **Cloud** of **lokale**.
   
**Extern bureaublad inschakelen voor alle functies** -Schakel deze optie als u wilt kunnen op afstand verbinding maken met de service. Deze optie wordt voornamelijk gebruikt voor het oplossen van problemen. Wanneer u dit selectievakje inschakelt, de **configuratie voor extern bureaublad** dialoogvenster wordt weergegeven. Kies de **instellingen** koppeling om de configuratie te wijzigen.
   
**Web Deploy inschakelen voor alle functies van web** -Schakel deze optie, web-implementatie voor de service moet worden ingeschakeld. U moet selecteren de **extern bureaublad inschakelen voor alle rollen** optie voor het gebruik van deze functie. Zie voor meer informatie [ [publiceren van een Azure-cloudservice met Visual Studio](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). 

## <a name="settings-page---advanced-settings-tab"></a>Voor instellingenpagina - instellingen tabblad Geavanceerd

![Geavanceerde instellingen](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Implementatielabel** -accepteer de standaardnaam of voer een naam van uw keuze. Als u wilt de datum toevoegen aan het implementatielabel, laat het selectievakje ingeschakeld. 
   
**Storage-account** -Selecteer het opslagaccount moet worden gebruikt voor deze implementatie **&lt;Nieuw > om een opslagaccount te maken. Het datacenter wordt tussen haakjes weergegeven voor elke storage-account. Het wordt aanbevolen dat de datacentrum locatie voor het opslagaccount niet hetzelfde zijn als de locatie van de center voor de cloudservice (algemene instellingen).  
   
De Azure storage-account slaat het pakket voor de toepassingsimplementatie. Nadat de toepassing is geïmplementeerd, wordt het pakket verwijderd uit het storage-account.

**Verwijderen van implementatie bij fout** -Selecteer deze optie om de implementatie verwijderd als er fouten tijdens de publicatie optreden. Dit moet worden uitgeschakeld als u wilt behouden constante virtueel IP-adres voor de cloudservice.

**Implementatie-update** -Selecteer deze optie als u implementeren wilt, alleen de bijgewerkte componenten heeft. Dit type implementatie verloopt waarschijnlijk sneller dan een volledige implementatie. Dit moet worden gecontroleerd als u wilt behouden constante virtueel IP-adres voor de cloudservice. 

**Implementatie-update - instellingen** -dit dialoogvenster wordt gebruikt om verdere geven hoe u wilt dat de rollen worden bijgewerkt. Als u ervoor kiest **incrementele update**, elk exemplaar van uw toepassing wordt bijgewerkt na elkaar, zodat de toepassing altijd beschikbaar is. Als u ervoor kiest **gelijktijdig bijwerken**, alle exemplaren van uw toepassing tegelijk worden bijgewerkt. Gelijktijdige bijwerken is sneller, maar de service mogelijk niet beschikbaar tijdens het updateproces. 

![Implementatie-instellingen](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**Inschakelen van IntelliTrace** -opgeven als u wilt inschakelen van IntelliTrace. U kunt uitgebreide foutopsporingsinformatie voor een rolexemplaar van met IntelliTrace registreren wanneer deze wordt uitgevoerd in Azure. Als u nodig hebt om de oorzaak van een probleem te achterhalen, kunt u de IntelliTrace-logboeken doorlopen uw code vanuit Visual Studio als in Azure werd uitgevoerd. Zie voor meer informatie over het gebruik van IntelliTrace [foutopsporing van een gepubliceerde Azure-cloudservice met Visual Studio en IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md). 

**Schakel profilering** -opgeven als u wilt inschakelen profileren van de prestaties. De profiler Visual Studio kunt u een gedetailleerde analyse van de rekenkundige aspecten van hoe uw cloudservice wordt uitgevoerd. Zie voor meer informatie over het gebruik van de Visual Studio-profiler [Test de prestaties van een Azure cloudservice](./vs-azure-tools-performance-profiling-cloud-services.md).

**Externe foutopsporing inschakelen voor alle rollen** -opgeven of u wilt foutopsporing op afstand inschakelen. Zie voor meer informatie over foutopsporing van cloudservices met Visual Studio [foutopsporing van een Azure-cloudservice of virtuele machine in Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Diagnostische instellingen voor pagina

![Instellingen voor diagnostische gegevens](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnostische gegevens kunt u problemen met een Azure-cloudservice (of de virtuele machine van Azure). Zie voor meer informatie over diagnostische gegevens [diagnostische gegevens configureren voor Azure Cloud Services en virtuele Machines](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Zie voor meer informatie over Application Insights [wat is Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Overzichtspagina

![Samenvatting](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Profiel als doel** -u kunt een publicatieprofiel maken van de instellingen die u hebt gekozen. U kunt bijvoorbeeld een profiel voor een testomgeving en één voor productie maken. Sla dit profiel op. Kies de **opslaan** pictogram. De wizard maakt het profiel en opgeslagen in de Visual Studio-project. Als u wilt de profielnaam wijzigen, opent u de **profiel als doel** lijst en kies vervolgens **< beheren... >**.
   
   > [!NOTE]
   > Het publicatieprofiel wordt weergegeven in Solution Explorer in Visual Studio en instellingen van het profiel naar een bestand met de extensie .azurePubxml worden geschreven. Instellingen worden opgeslagen als kenmerken van XML-codes.
   > 
   > 

## <a name="publishing-your-application"></a>Uw toepassing te publiceren

Als u de instellingen voor uw project implementatie configureert, selecteert u **publiceren** aan de onderkant van het dialoogvenster. U kunt de processtatus van het in bewaken de **uitvoer** venster in Visual Studio.

## <a name="next-steps"></a>Volgende stappen
- [Migreer en publiceren van webtoepassingen met een Azure-cloud-service vanuit Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)
- [Informatie over het gebruik van Visual Studio voor het publiceren van een Azure cloudservice](./vs-azure-tools-publishing-a-cloud-service.md)
- [Een gepubliceerde Azure-cloudservice met Visual Studio en IntelliTrace-foutopsporing](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)
- [De prestaties van een Azure cloudservice testen](./vs-azure-tools-performance-profiling-cloud-services.md)
- [Diagnostische gegevens configureren voor Azure-Cloudservices en virtuele Machines](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). 
- [Wat is Application Insights?](./application-insights/app-insights-overview.md)