---
title: -Configuraties en -profielen beheren | Microsoft Docs
description: Meer informatie over het werken met service-configuraties en -profielen-configuratiebestanden | welke Sla de instellingen voor de implementatieomgevingen en publicatie-instellingen voor cloudservices.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/11/2017
ms.author: ghogen
ms.openlocfilehash: 1acb13f8bdcb7f5a6f214a7425e13c72e21bd29d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057573"
---
# <a name="how-to-manage-service-configurations-and-profiles"></a>-Configuraties en -profielen beheren
## <a name="overview"></a>Overzicht
Wanneer u een cloudservice publiceert, Visual Studio worden configuratie-informatie opgeslagen in twee soorten configuratiebestanden: service-configuraties en -profielen. Instellingen voor de implementatie voor een Azure-cloudservice opslaan,-configuraties (.cscfg-bestanden). Azure maakt gebruik van deze configuratiebestanden bij het beheren van uw cloudservices. Aan de andere kant profielen (.azurePubxml bestanden) store publicatie-instellingen voor cloudservices. Deze instellingen zijn een record van wat u kiest wanneer u de wizard publiceren, en lokaal worden gebruikt door Visual Studio. In dit onderwerp wordt uitgelegd hoe u werkt met beide typen-configuratiebestanden.

## <a name="service-configurations"></a>-Configuraties
U kunt meerdere serviceconfiguraties gebruiken voor elk van uw implementatieomgevingen maken. U kunt bijvoorbeeld de serviceconfiguratie van een voor de lokale omgeving waarmee u kunt uitvoeren en testen van een Azure-toepassing en een andere serviceconfiguratie voor uw productie-omgeving maken.

U kunt toevoegen, verwijderen, wijzigen en aanpassen van deze serviceconfiguraties op basis van uw vereisten. Zoals wordt weergegeven in de volgende afbeelding, kunt u deze configuraties beheren vanuit Visual Studio.

![Configuraties beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

U kunt ook openen de **configuraties beheren** in het dialoogvenster van de eigenschappenpagina van de rol. Als u wilt de eigenschappen voor een rol in uw Azure-project openen, opent u het snelmenu voor die rol en kies vervolgens **eigenschappen**. Op de **instellingen** tabblad uit, vouw de **serviceconfiguratie** lijst en selecteer vervolgens **beheren** openen de **configuraties beheren** in het dialoogvenster.

### <a name="to-add-a-service-configuration"></a>De configuratie van een service toevoegen
1. Open in Solution Explorer het snelmenu voor het Azure-project en selecteer vervolgens **configuraties beheren**.
   
    De **-configuraties beheren** in het dialoogvenster wordt weergegeven.
2. Als u wilt toevoegen de configuratie van een service, moet u een kopie van een bestaande configuratie te maken. Om dit te doen, kiest u de configuratie die u wilt kopiëren uit de lijst en selecteer vervolgens **kopie maken**.
3. (Optioneel) Als u wilt de configuratie van de service een andere naam geven, de configuratie van de nieuwe service kiezen uit de lijst met de naam en selecteer vervolgens **naam**. In de **naam** tekst typt u de naam die u wilt gebruiken voor deze serviceconfiguratie en selecteer vervolgens **OK**.
   
    Een nieuw serviceconfiguratiebestand met de naam serviceconfiguration komt. [Nieuwe Name] cscfg-bestand wordt toegevoegd aan de Azure-project in Solution Explorer.

### <a name="to-delete-a-service-configuration"></a>Een serviceconfiguratie verwijderen
1. Open in Solution Explorer het snelmenu voor het Azure-project en selecteer vervolgens **configuraties beheren**.
   
    De **-configuraties beheren** in het dialoogvenster wordt weergegeven.
2. Als u wilt een serviceconfiguratie verwijderen, kiest u de configuratie die u verwijderen wilt uit de **naam** lijst en selecteer vervolgens **verwijderen**. Er verschijnt een dialoogvenster om te controleren of dat u wilt verwijderen van deze configuratie.
3. Selecteer **Verwijderen**.
   
     Het configuratiebestand van de service is verwijderd uit de Azure-project in Solution Explorer.

### <a name="to-rename-a-service-configuration"></a>Naam wijzigen van de configuratie van een service
1. Open het snelmenu voor het Azure-project in Solution Explorer en selecteer vervolgens **configuraties beheren**.
   
    De **-configuraties beheren** in het dialoogvenster wordt weergegeven.
2. Wijzig de naam van de configuratie van een service, kiest u de nieuwe serviceconfiguratie van de **naam** lijst en selecteer vervolgens **naam**. In de **naam** tekst typt u de naam die u wilt gebruiken voor deze serviceconfiguratie en selecteer vervolgens **OK**.
   
    De naam van het configuratiebestand van de service is gewijzigd in de Azure-project in Solution Explorer.

### <a name="to-change-a-service-configuration"></a>Een serviceconfiguratie wijzigen
* Als u een serviceconfiguratie wijzigen wilt, opent u het snelmenu voor het specifieke rol die u wilt wijzigen in de Azure-project en selecteer vervolgens **eigenschappen**. Zie [hoe: de rollen configureren voor een Azure-Cloudservice met Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) voor meer informatie.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Verschillende combinaties maken met behulp van profielen
Met behulp van een profiel, kunt u automatisch invullen de **Wizard publiceren** met verschillende combinaties van instellingen voor verschillende doeleinden. Bijvoorbeeld, kunt u een profiel voor het opsporen van fouten hebt en een andere voor release builds. In dat geval uw **Debug** profiel zou hebben **IntelliTrace** ingeschakeld en de **Debug** configuratie die is geselecteerd, en uw **Release** profiel zou hebben **IntelliTrace** uitgeschakeld en de **Release** configuratie die is geselecteerd. U kunt ook andere profielen gebruiken om een service met behulp van een ander opslagaccount te implementeren.

Wanneer u de wizard voor het eerst uitvoert, wordt een standaard-profiel wordt gemaakt. Visual Studio slaat het profiel in een bestand met een extensie .azurePubXml, die wordt toegevoegd aan uw Azure-project onder de **profielen** map. Als u verschillende mogelijkheden handmatig opgeven wanneer u later de wizard uitvoert, wordt het bestand automatisch bijgewerkt. Voordat u de volgende procedure uitvoert, moet u al hebt gepubliceerd uw cloudservice ten minste één keer.

### <a name="to-add-a-profile"></a>Een profiel toevoegen
1. Open het snelmenu voor uw Azure-project en selecteer vervolgens **publiceren**.
2. Naast de **doelprofiel** in de lijst met de **profiel opslaan** knop, zoals in de volgende afbeelding wordt weergegeven. Hiermee maakt u een profiel voor u.
   
    ![Een nieuw profiel maken](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Nadat het profiel is gemaakt, selecteert u **< beheren >** in de **doelprofiel** lijst.
   
    De **Spravovat Profily** in het dialoogvenster wordt weergegeven, zoals in de volgende afbeelding wordt weergegeven.
   
    ![Dialoogvenster profielen beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. In de **naam** lijst, kiest u een profiel en selecteer vervolgens **kopie maken**.
5. Kies de **sluiten** knop.
   
    Het nieuwe profiel wordt weergegeven in de lijst van de doel-profiel.
6. In de **doelprofiel** , selecteert u het profiel dat u zojuist hebt gemaakt. De Wizard publiceren-instellingen worden automatisch ingevuld met de opties van het profiel dat u hebt geselecteerd.
7. Selecteer de **vorige** en **volgende** knoppen aan elke pagina van de Wizard publiceren weergeven en pas vervolgens de instellingen voor dit profiel. Zie [Wizard voor Azure-toepassing publiceren](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor meer informatie.
8. Nadat u klaar bent met het aanpassen van de instellingen, selecteert u **volgende** om terug te gaan naar de pagina instellingen. Het profiel is opgeslagen wanneer u de service publiceren met behulp van deze instellingen of als u selecteert **opslaan** naast de lijst met profielen.

### <a name="to-rename-or-delete-a-profile"></a>Wijzig de naam of een profiel verwijderen
1. Open het snelmenu voor uw Azure-project en selecteer vervolgens **publiceren**.
2. In de **doelprofiel** in de lijst met **beheren**.
3. In de **Spravovat Profily** in het dialoogvenster, selecteer het profiel dat u wilt verwijderen en selecteer vervolgens **verwijderen**.
4. Selecteer in het bevestigingsdialoogvenster dat wordt weergegeven, **OK**.
5. Selecteer **sluiten**.

### <a name="to-change-a-profile"></a>Een profiel wijzigen
1. Open het snelmenu voor uw Azure-project en selecteer vervolgens **publiceren**.
2. In de **doelprofiel** , selecteert u het profiel dat u wilt wijzigen.
3. Selecteer de **vorige** en **volgende** knoppen wilt weergeven van elke pagina van de Wizard publiceren en vervolgens de instellingen die u wilt wijzigen. Zie [Wizard voor Azure-toepassing publiceren](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor meer informatie.
4. Nadat u klaar bent met het wijzigen van de instellingen, selecteert u **volgende** terugkeren naar de **instellingen** pagina.
5. (Optioneel) Selecteer **publiceren** voor het publiceren van de cloudservice met de nieuwe instellingen. Als u niet wilt dat uw cloudservice op dit moment publiceren en u de Wizard publiceren te sluiten, Visual Studio wordt gevraagd of u wilt opslaan van de wijzigingen aan het profiel.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het configureren van andere onderdelen van uw Azure-project in Visual Studio, [configureren van een Azure-Project](http://go.microsoft.com/fwlink/p/?LinkID=623075)

