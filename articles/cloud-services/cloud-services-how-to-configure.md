---
title: Het configureren van een cloudservice (klassieke portal) | Microsoft Docs
description: Informatie over het configureren van cloud-services in Azure. Informatie over het bijwerken van de configuratie van de cloud-service en het configureren van externe toegang tot de rolinstanties.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 39bb294c96ce0c12d91cf8b3488ac3e1a7b2f7b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-cloud-services"></a>Het configureren van Cloud-Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-configure-portal.md)
> * [Klassieke Azure Portal](cloud-services-how-to-configure.md)
> 
> 

U kunt de meest gebruikte instellingen voor een cloudservice kunt configureren in de klassieke Azure portal. Het is ook mogelijk uw configuratiebestand rechtstreeks bij te werken. Download in dat geval een serviceconfiguratiebestand om bij te werken, upload vervolgens het bijgewerkte bestand en werk de cloudservice bij met de configuratiewijzigingen. Welke methode u ook gebruikt, de configuratie-updates worden doorgegeven aan alle rolinstanties.

De klassieke Azure portal kunt u ook [verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)

Azure zorgt alleen servicebeschikbaarheid 99,95% tijdens de configuratie-updates hebt u ten minste twee rolexemplaren voor elke rol. Waarmee één virtuele machine voor het verwerken van aanvragen van clients, terwijl de andere wordt bijgewerkt. Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Een cloudservice wijzigen
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**, klik op de naam van de cloudservice en klik vervolgens op **configureren**.
   
    ![Configuratiepagina](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    Op de **configureren** pagina die u kunt bewaking, update-functie-instellingen configureren en selecteer het gastbesturingssysteem en de familie voor rolinstanties. 
2. In **bewaking**, het niveau van de controle op uitgebreide of minimale ingesteld en de verbindingsreeksen voor diagnostische gegevens die vereist voor uitgebreide bewaking zijn configureren.
3. Voor service-rollen (gegroepeerd per functie), kunt u de volgende instellingen bijwerken:
   
    * **Instellingen** wijzigen de waarden van verschillende configuratie-instellingen die zijn opgegeven in de *ConfigurationSettings* elementen van het service-configuratiebestand (.cscfg).

    * **Certificaten**  
        De vingerafdruk van het certificaat dat wordt gebruikt in SSL-versleuteling voor een rol wijzigen. Als u wilt wijzigen van een certificaat, moet u eerst het nieuwe certificaat te uploaden (op het **certificaten** pagina). Werk vervolgens de vingerafdruk van het in de weergegeven in de instellingen van de certificaattekenreeks.
4. In **besturingssysteem**, kunt u de besturingssysteemgroep of de versie voor rolinstanties wijzigen of kies **automatische** automatische updates van de huidige versie van besturingssysteem inschakelen. De instellingen van het besturingssysteem van toepassing op webrollen en werkrollen, maar hebben geen invloed op de virtuele Machines.
   
    Tijdens de implementatie van de meest recente versie van besturingssysteem is geïnstalleerd op alle rolinstanties en de besturingssystemen standaard automatisch worden bijgewerkt. 
   
    Als u nodig hebt voor uw cloudservice kunnen worden uitgevoerd op een andere besturingssysteemversie vanwege compatibiliteitsvereisten in uw code, kunt u een besturingssysteem familie en versie. Wanneer u een specifieke besturingssysteemversie kiest, worden automatische besturingssysteemupdates voor de cloudservice onderbroken. U moet zorgen dat de besturingssystemen updates ontvangen.
   
    Als u alle compatibiliteitsproblemen die uw apps met de meest recente versie van besturingssysteem hebt opgelost, kunt u automatische besturingssysteemupdates inschakelen door de versie van besturingssysteem in te stellen op **automatische**. 
   
    ![OS-instellingen](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. Als u uw configuratie-instellingen opslaan en toepassen op de rolexemplaren, klikt u op **opslaan**. (Klik op **negeren** om de wijzigingen te annuleren.) **Sla** en **negeren** worden toegevoegd aan de opdrachtbalk klikken nadat u een instelling wijzigt.

## <a name="update-a-cloud-service-configuration-file"></a>Een configuratiebestand voor cloud-service bijwerken
1. Download een cloud serviceconfiguratiebestand (.cscfg) met de huidige configuratie. Op de **configureren** pagina voor de cloudservice, klikt u op **downloaden**. Klik vervolgens op **opslaan**, of klik op **OpslaanAls** het bestand wilt opslaan.
2. Na het bijwerken van het configuratiebestand van de service, uploaden en updates voor de configuratie van toepassing:
   
   1. Op de **configureren** pagina, klikt u op **uploaden**.
      
       ![Configuratie uploaden](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. In **configuratiebestand**, gebruik **Bladeren** om de bijgewerkte .cscfg-bestand te selecteren.
   3. Als uw cloudservice geen rollen waarvoor slechts één exemplaar bevat, selecteert u de **configuratie toepassen, zelfs als een of meer rollen één exemplaar bevatten** selectievakje in zodat de updates voor de configuratie voor de rollen om door te gaan.
      
       Tenzij u ten minste twee exemplaren van elke rol definiëren, ten minste 99,95% kan niet worden gegarandeerd door Azure beschikbaarheid van uw cloudservice tijdens het bijwerken van de service-configuratie. Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).
   4. Klik op **OK** (vinkje). 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name.md).
* [Beheren van uw cloudservice](cloud-services-how-to-manage.md).
* [Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen](cloud-services-role-enable-remote-desktop.md)
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate.md).

