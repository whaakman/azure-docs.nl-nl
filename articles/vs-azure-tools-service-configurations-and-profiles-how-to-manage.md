---
title: -Configuraties en -profielen beheren | Microsoft Docs
description: Meer informatie over het werken met de service en -profielen configuratiebestanden | die instellingen voor de implementatieomgevingen opslaan en publicatie-instellingen voor cloud-services.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/11/2017
ms.author: kraigb
ms.openlocfilehash: af1205f8c3e477d123d4835c80a68b3afd6ee5ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-service-configurations-and-profiles"></a>Het beheren van configuraties en -profielen
## <a name="overview"></a>Overzicht
Wanneer u een service in de cloud publiceert, Visual Studio configuratie-informatie opslaat in twee soorten configuratiebestanden: service en -profielen. Instellingen voor de implementatie-omgevingen voor een Azure cloudservice opslaan,-configuraties (.cscfg-bestanden). Azure maakt gebruik van deze configuratiebestanden bij het beheren van uw cloudservices. Aan de andere kant profielen (.azurePubxml-bestanden) store publicatie-instellingen voor cloud-services. Deze instellingen zijn een record van wat u kiest wanneer u de wizard publiceren, en lokaal worden gebruikt door Visual Studio. Dit onderwerp wordt uitgelegd hoe u werkt met beide typen configuratiebestanden.

## <a name="service-configurations"></a>-Configuraties
U kunt serviceconfiguraties met meerdere moet worden gebruikt voor elk van uw implementatieomgevingen maken. U kunt bijvoorbeeld de configuratie van een service voor de lokale omgeving die u gebruikt voor het uitvoeren en testen van een Azure-toepassing en een andere serviceconfiguratie voor uw productieomgeving maken.

U kunt toevoegen, verwijderen, wijzigen en wijzigen van deze serviceconfiguraties op basis van uw vereisten. Zoals weergegeven in de volgende afbeelding, kunt u deze serviceconfiguraties beheren vanuit Visual Studio.

![-Configuraties beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

U kunt ook openen de **configuraties beheren** in het dialoogvenster van de eigenschappenpagina van de rol. Als de eigenschappen voor een functie in uw Azure-project, open het snelmenu voor die functie en kies vervolgens **eigenschappen**. Op de **instellingen** tabblad uit, vouw de **serviceconfiguratie** lijst, en selecteer vervolgens **beheren** openen de **configuraties beheren** in het dialoogvenster.

### <a name="to-add-a-service-configuration"></a>De configuratie van een service toevoegen
1. Open het snelmenu voor het Azure-project in Solution Explorer en selecteer **configuraties beheren**.
   
    De **beheren-configuraties** dialoogvenster wordt weergegeven.
2. Als u wilt toevoegen in de configuratie van een service, moet u een kopie van een bestaande configuratie te maken. Om dit te doen, kiest u de configuratie die u wilt kopiëren uit de lijst en selecteer vervolgens **kopie maken**.
3. (Optioneel) Als u de configuratie van de service een andere naam geven, kiest u de nieuwe serviceconfiguratie in de lijst met de naam en selecteer vervolgens **naam**. In de **naam** tekst typt u de naam die u wilt gebruiken voor deze serviceconfiguratie en selecteer vervolgens **OK**.
   
    Een nieuwe serviceconfiguratiebestand met de naam serviceconfiguration zijn. [Nieuwe Name] cscfg-bestand wordt toegevoegd aan de Azure-project in Solution Explorer.

### <a name="to-delete-a-service-configuration"></a>Een serviceconfiguratie verwijderen
1. Open het snelmenu voor het Azure-project in Solution Explorer en selecteer **configuraties beheren**.
   
    De **beheren-configuraties** dialoogvenster wordt weergegeven.
2. Als u wilt een serviceconfiguratie verwijderen, kiest u de configuratie die u verwijderen wilt uit de **naam** lijst en selecteer vervolgens **verwijderen**. Er verschijnt een dialoogvenster om te controleren dat u wilt verwijderen van deze configuratie.
3. Selecteer **Verwijderen**.
   
     Het configuratiebestand van de service is verwijderd uit de Azure-project in Solution Explorer.

### <a name="to-rename-a-service-configuration"></a>Naam wijzigen van de configuratie van een service
1. Open het snelmenu voor het Azure-project in Solution Explorer en selecteer vervolgens **configuraties beheren**.
   
    De **beheren-configuraties** dialoogvenster wordt weergegeven.
2. Wijzig de naam van de configuratie van een service, kiest u de nieuwe serviceconfiguratie van de **naam** lijst, en selecteer vervolgens **naam**. In de **naam** tekst typt u de naam die u wilt gebruiken voor deze serviceconfiguratie en selecteer vervolgens **OK**.
   
    De naam van het configuratiebestand van de service is gewijzigd in de Azure-project in Solution Explorer.

### <a name="to-change-a-service-configuration"></a>Een serviceconfiguratie wijzigen
* Als u een serviceconfiguratie wijzigen wilt, opent u het snelmenu voor het specifieke rol die u wilt wijzigen in de Azure-project en selecteer vervolgens **eigenschappen**. Zie [hoe: de rollen configureren voor een Azure Cloud Service met Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) voor meer informatie.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Verschillende combinaties instellen met behulp van profielen maken
Met behulp van een profiel automatisch vult u de **Wizard publiceren** met verschillende combinaties van instellingen voor verschillende doeleinden. Bijvoorbeeld, u kunt een profiel hebt voor foutopsporing en een andere versie is gebaseerd. In dat geval uw **Debug** profiel zou hebben **IntelliTrace** ingeschakeld en de **Debug** configuratie die is geselecteerd, en uw **Release** profiel zou hebben **IntelliTrace** uitgeschakeld en de **Release** configuratie die is geselecteerd. U kunt ook verschillende profielen voor het implementeren van een service met een ander opslagaccount.

Wanneer u de wizard voor het eerst uitvoert, wordt een standaard-profiel gemaakt. Visual Studio slaat u het profiel in een bestand met een extensie .azurePubXml, die wordt toegevoegd aan uw Azure-project onder de **profielen** map. Als u verschillende mogelijkheden handmatig opgeven wanneer u later de wizard uitvoert, wordt het bestand automatisch bijgewerkt. Voordat u de volgende procedure uitvoert, moet u al hebt gepubliceerd uw cloudservice ten minste één keer.

### <a name="to-add-a-profile"></a>Een profiel toevoegen
1. Open het snelmenu voor uw Azure-project en selecteer vervolgens **publiceren**.
2. Naast de **profiel als doel** lijst, selecteert de **profiel opslaan** knop als de volgende afbeelding ziet. Hiermee maakt u een profiel voor u.
   
    ![Een nieuw profiel maken](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Nadat het profiel is gemaakt, selecteert u **< beheren... >** in de **profiel als doel** lijst.
   
    De **profielen beheren** dialoogvenster wordt weergegeven, als de volgende afbeelding ziet.
   
    ![Dialoogvenster profielen beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. In de **naam** lijst, kiest u een profiel en selecteer vervolgens **kopie maken**.
5. Kies de **sluiten** knop.
   
    Het nieuwe profiel wordt weergegeven in de lijst met doelservers profiel.
6. In de **doel profiel** , selecteert u het profiel dat u zojuist hebt gemaakt. De instellingen van de Wizard publiceren worden gevuld met de opties in het profiel dat u hebt geselecteerd.
7. Selecteer de **vorige** en **volgende** knoppen weer te geven van elke pagina van de Wizard publiceren en vervolgens de instellingen aanpassen voor dit profiel. Zie [Publish Azure Application Wizard](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor meer informatie.
8. Nadat u de instellingen aanpassen, selecteert u **volgende** om terug te gaan naar de instellingenpagina. Het profiel wordt opgeslagen wanneer u de service publiceert met behulp van deze instellingen of als u selecteert **opslaan** naast de lijst met profielen.

### <a name="to-rename-or-delete-a-profile"></a>Wijzig de naam of een profiel verwijderen
1. Open het snelmenu voor uw Azure-project en selecteer vervolgens **publiceren**.
2. In de **profiel als doel** selecteert **beheren**.
3. In de **profielen beheren** in het dialoogvenster, selecteer het profiel dat u wilt verwijderen en selecteer vervolgens **verwijderen**.
4. Selecteer in het bevestigingsdialoogvenster **OK**.
5. Selecteer **sluiten**.

### <a name="to-change-a-profile"></a>Een profiel wijzigen
1. Open het snelmenu voor uw Azure-project en selecteer vervolgens **publiceren**.
2. In de **doel profiel** , selecteert u het profiel dat u wilt wijzigen.
3. Selecteer de **vorige** en **volgende** knoppen wilt weergeven van elke pagina van de Wizard publiceren en wijzig vervolgens de gewenste instellingen. Zie [Publish Azure Application Wizard](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor meer informatie.
4. Nadat u de instellingen wijzigen, selecteert u **volgende** wilt teruggaan naar de **instellingen** pagina.
5. (Optioneel) Selecteer **publiceren** voor het publiceren van de cloudservice met de nieuwe instellingen. Als u niet wilt dat uw cloudservice op dit moment publiceren en sluiten van de Wizard publiceren, vraagt Visual Studio als u wilt de wijzigingen opslaan op het profiel.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het configureren van andere onderdelen van uw Azure-project vanuit Visual Studio, [configureren van een Azure-Project](http://go.microsoft.com/fwlink/p/?LinkID=623075)

