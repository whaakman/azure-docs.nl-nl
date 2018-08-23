---
title: Wizard Azure-toepassing met Visual Studio publiceren | Microsoft Docs
description: Informatie over het configureren van de verschillende instellingen in de Visual Studio Azure Wizard toepassing publiceren
services: visual-studio-online
author: ghogen
manager: douge
assetId: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 97d78bd61f7cf2a651fea1ac29e2a952a8f8ced3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059489"
---
# <a name="using-the-visual-studio-publish-azure-application-wizard"></a>Wizard Azure-toepassing met Visual Studio publiceren

Nadat u een web-App in Visual Studio ontwikkelen, kunt u deze toepassing in een Azure cloudservice met behulp van de **Publish Azure Application** wizard.

> [!Note]
> Dit artikel is over het implementeren van cloudservices, niet met websites. Zie voor meer informatie over het implementeren van websites [over het implementeren van een Azure-website](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="accessing-the-publish-azure-application-wizard"></a>Toegang tot de wizard Azure-toepassing publiceren

De wizard Azure-toepassing publiceren op twee manieren, afhankelijk van het type van Visual Studio-project hebt, kunt u openen.

**Als u een Azure-cloud service-project hebt:**

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **publiceren**.

**Als u een web application-project dat niet is ingeschakeld voor Azure hebt:**

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **converteren** > **converteren naar Azure-Cloudserviceproject**. 

1. In **Solution Explorer**, met de rechtermuisknop op de zojuist gemaakte Azure-project en selecteer in het contextmenu **publiceren**.

## <a name="sign-in-page"></a>Aanmeldingspagina opgeven

![Aanmeldingspagina opgeven](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Account** : Selecteer een account of selecteer **een account toevoegen** in de vervolgkeuzelijst van het account.

**Kies uw abonnement** -kiest u het abonnement moet worden gebruikt voor uw implementatie.

## <a name="settings-page---common-settings-tab"></a>Voor instellingenpagina - tabblad algemene instellingen

![Algemene instellingen](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Cloudservice** -met behulp van de vervolgkeuzelijst, selecteer een bestaande cloud service, of selecteer  **&lt;Nieuw >**, en maken van een service in de cloud. Het datacenter wordt tussen haakjes weergegeven voor elke service in de cloud. Het verdient aanbeveling dat het datacenter locatie voor de cloudservice niet hetzelfde zijn als de datacenterlocatie voor het opslagaccount (geavanceerde instellingen).

**Omgeving** -Selecteer **productie** of **fasering**. Kies de faseringsomgeving als u wilt implementeren van uw toepassing in een testomgeving. 

**Konfigurace buildu** -Selecteer **Debug** of **Release**.

**Configuratie van de service** -Selecteer **Cloud** of **lokale**.

**Extern bureaublad inschakelen voor alle rollen** : Selecteer deze optie als u wilt kunnen op afstand verbinding maken met de service. Deze optie wordt voornamelijk gebruikt voor het oplossen van problemen. Zie voor meer informatie, [extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

**Web Deploy voor alle web-functies inschakelen** : Selecteer deze optie om in te schakelen van webimplementatie voor de service. U moet ook selecteren de **extern bureaublad inschakelen voor alle rollen** optie voor het gebruik van deze functie. Zie voor meer informatie, [publiceren van een cloudservice met Visual Studio](vs-azure-tools-publishing-a-cloud-service.md).

## <a name="settings-page---advanced-settings-tab"></a>Voor instellingenpagina - tabblad Geavanceerd van instellingen

![Geavanceerde instellingen](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Implementatielabel** -accepteer de standaardnaam of voer een naam in van uw keuze. Als u wilt de datum aan het implementatielabel toevoegen, laat u het selectievakje ingeschakeld. 

**Storage-account** -Selecteer het opslagaccount dat moet worden gebruikt voor deze implementatie **&lt;Nieuw > om een opslagaccount te maken. Het datacenter wordt tussen haakjes weergegeven voor elke storage-account. Het wordt aanbevolen dat de datacenterlocatie voor het opslagaccount hetzelfde als de datacenterlocatie voor de cloudservice (algemene instellingen is).

De Azure storage-account wordt het pakket voor de implementatie van de toepassing opgeslagen. Nadat de toepassing is geïmplementeerd, wordt het pakket verwijderd uit het opslagaccount.

**Verwijder de implementatie bij fout** : Selecteer deze optie om de implementatie verwijderd als er fouten zijn opgetreden tijdens het publiceren. Dit moet worden uitgeschakeld als u wilt behouden een constante virtuele IP-adres voor de cloudservice.

**Implementatie-update** : Selecteer deze optie als u implementeren wilt, alleen de bijgewerkte componenten. Dit type implementatie kan worden sneller dan een volledige implementatie. Dit moet worden gecontroleerd als u wilt behouden een constante virtuele IP-adres voor de cloudservice. 

**Implementatie - instellingen bijwerken** -dit dialoogvenster wordt gebruikt om verdere geven hoe u wilt dat de rollen worden bijgewerkt. Als u ervoor kiest **incrementele update**, elk exemplaar van uw toepassing wordt bijgewerkt na elkaar, zodat de toepassing altijd beschikbaar is. Als u ervoor kiest **gelijktijdig bijwerken**, alle exemplaren van uw toepassing op hetzelfde moment worden bijgewerkt. Gelijktijdige bijwerken is sneller, maar de service mogelijk niet beschikbaar tijdens het updateproces.

![Implementatie-instellingen](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**IntelliTrace inschakelen** -opgeven of u wilt om in te schakelen van IntelliTrace. Met IntelliTrace, kunt u uitgebreide informatie over foutopsporing voor een rolinstantie registreren wanneer deze wordt uitgevoerd in Azure. Als u de oorzaak van een probleem wilt, kunt u de IntelliTrace-Logboeken kunt doorlopen uw code vanuit Visual Studio alsof deze worden uitgevoerd in Azure. Zie voor meer informatie over het gebruik van IntelliTrace [opsporen van fouten in een gepubliceerde Azure-cloudservice met Visual Studio en IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md).

**Schakel profilering** -opgeven of u wilt om in te schakelen prestaties profilering. De Visual Studio profiler kunt u een diepgaande analyse van de rekenkundige aspecten van hoe uw cloudservice wordt uitgevoerd. Zie voor meer informatie over het gebruik van de Visual Studio profiler [Test de prestaties van een Azure-cloudservice](./vs-azure-tools-performance-profiling-cloud-services.md).

**Externe foutopsporing inschakelen voor alle rollen** -opgeven als u foutopsporing op afstand inschakelen. Zie voor meer informatie over het opsporen van fouten in cloudservices met Visual Studio, [opsporen van fouten in een Azure-cloudservice of virtuele machine in Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>De pagina instellingen voor diagnostische gegevens

![Diagnostische instellingen](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnostics kunt u problemen met een Azure-cloudservice (of de virtuele machine van Azure). Zie voor meer informatie over diagnostische gegevens [diagnostische gegevens configureren voor Azure Cloud Services en Virtual Machines](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Zie voor meer informatie over Application Insights [wat is Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Overzichtspagina

![Samenvatting](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Doelprofiel** -u kunt kiezen om te maken van een publicatieprofiel van de instellingen die u hebt gekozen. U kunt bijvoorbeeld een profiel voor een testomgeving en één voor productie maken. Om dit profiel hebt opgeslagen, kiest u de **opslaan** pictogram. De wizard profiel wordt gemaakt en opgeslagen in de Visual Studio-project. Als u wilt de profielnaam wijzigen, opent u de **doelprofiel** lijst en kies vervolgens  **&lt;beheren... &gt;**.

   > [!Note]
   > Het publicatieprofiel dat wordt weergegeven in Solution Explorer in Visual Studio en instellingen van het profiel naar een bestand met de extensie .azurePubxml worden geschreven. Instellingen worden opgeslagen als kenmerken van XML-tags.

## <a name="publishing-your-application"></a>Uw toepassing te publiceren

Als u de instellingen voor de implementatie van uw project configureert, selecteert u **publiceren** aan de onderkant van het dialoogvenster. U kunt de processtatus in bewaken de **uitvoer** venster in Visual Studio.

## <a name="next-steps"></a>Volgende stappen

- [Migreren en publiceren van een webtoepassing in een Azure cloudservice van Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)

- [Informatie over het gebruik van Visual Studio voor het publiceren van een Azure-cloudservice](./vs-azure-tools-publishing-a-cloud-service.md)

- [Foutopsporing in een gepubliceerde Azure-cloudservice met Visual Studio en IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)

- [Test de prestaties van een Azure-cloudservice](./vs-azure-tools-performance-profiling-cloud-services.md)

- [Diagnostische gegevens configureren voor Azure-Cloudservices en virtuele Machines](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

- [Wat is Application Insights?](./application-insights/app-insights-overview.md)