---
title: Een Cloudservice met de Azure-hulpprogramma's publiceren | Microsoft Docs
description: Meer informatie over hoe Azure-cloud service-projecten publiceren met behulp van Visual Studio.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2017
ms.author: kraigb
ms.openlocfilehash: 933d274406951416c0e1f83dcc0d72b7f2bed527
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="publishing-a-cloud-service-using-visual-studio"></a>Publiceren van een cloudservice met Visual Studio

Visual Studio te een toepassing rechtstreeks naar Azure, met ondersteuning voor zowel fasering en productie-omgevingen van een cloudservice publiceren. Wanneer u publiceert, selecteert u de implementatieomgeving en een opslagaccount die tijdelijk wordt gebruikt voor het implementatiepakket.

Wanneer u ontwikkelen en testen van een Azure-toepassing, kunt u Web Deploy gebruiken om wijzigingen te publiceren stapsgewijs voor uw web-rollen. Nadat u uw toepassing in een implementatieomgeving publiceren, kunt Web Deploy u wijzigingen rechtstreeks op de virtuele machine die wordt uitgevoerd de Webrol implementeren. U beschikt niet over het pakket en publiceren van uw volledige Azure-toepassing elke keer dat u bijwerken van uw Webrol wilt voor het testen van de wijzigingen. Met deze aanpak hebt u uw wijzigingen van web-functie beschikbaar in de cloud voor het testen zonder te wachten op uw toepassing naar een implementatieomgeving gepubliceerd.

Gebruik de volgende procedures voor het publiceren van uw Azure-toepassing en een Webrol bijwerken met behulp van Web Deploy:

* Publiceren of een Azure-toepassing vanuit Visual Studio-pakket
* Een Webrol als onderdeel van de ontwikkeling en tests cyclus bijwerken

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publiceren of een Azure-toepassing vanuit Visual Studio-pakket

Wanneer u uw Azure-toepassing publiceert, kunt u een van de volgende taken kunt doen:

* Een servicepakket maken: U kunt dit pakket en het configuratiebestand van de service gebruiken voor het publiceren van uw toepassing in een implementatieomgeving met van de [Azure-portal](https://portal.azure.com).
* Uw Azure-project vanuit Visual Studio publiceren: voor het publiceren van uw toepassing rechtstreeks in Azure, gebruikt u de Wizard publiceren. Zie voor informatie [Publish Azure Application Wizard](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Een servicepakket maken vanuit Visual Studio

1. Wanneer u klaar bent voor uw toepassing publiceren, open Solution Explorer, open het snelmenu voor het Azure-project met uw rollen, en kies publiceren.
1. Volg deze stappen voor het maken van alleen servicepakket:

   a. Kies in het snelmenu voor het Azure-project **pakket**.
   b. In de **pakket Azure toepassing** in het dialoogvenster kiest u de configuratie van de service waarvan u wilt maken van een pakket en kies vervolgens de buildconfiguratie.
   c. (optioneel) Als u extern bureaublad voor de cloudservice nadat u deze hebt gepubliceerd, selecteer de **extern bureaublad inschakelen voor alle rollen** selectievakje en selecteer vervolgens **instellingen** voor het configureren van extern bureaublad. Als u fouten opsporen in uw cloudservice wilt nadat u deze hebt gepubliceerd, schakelt u foutopsporing op afstand door te selecteren **externe foutopsporing inschakelen voor alle rollen**.

      Zie voor meer informatie [met behulp van extern bureaublad met de Azure-rollen](vs-azure-tools-remote-desktop-roles.md).
   d. Kies voor het maken van het pakket, de **pakket** koppeling.

      Verkenner toont de bestandslocatie van het nieuwe pakket. U kunt deze locatie kopiëren zodat u deze vanuit de Azure-portal gebruiken kunt.
   e. Voor het publiceren van dit pakket met een implementatieomgeving, moet u deze locatie gebruiken als de locatie van het pakket wanneer u een cloudservice maken en implementeren van dit pakket in een omgeving met de Azure-portal.
1. (Optioneel) Als u wilt annuleren in het snelmenu voor het artikel in het gebeurtenissenlogboek van het implementatieproces kiezen **annuleren en verwijder**. Met deze opdracht het implementatieproces stopt en verwijdert deze de implementatieomgeving van Azure. U kunt de omgeving na de implementatie, met de Azure-portal.

1. (Optioneel) Nadat uw rolinstanties hebt gestart, Visual Studio automatisch de implementatieomgeving in toont de **Cloudservices** knooppunt in Server Explorer. Hier ziet u de status van de afzonderlijke rolinstanties. Zie [het beheer van Azure-resources met Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md). De volgende afbeelding ziet u de rolexemplaren als ze nog steeds in de status Bezig met initialiseren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Een Webrol als onderdeel van de ontwikkeling en tests cyclus bijwerken

Als uw app back-end-infrastructuur stabiel is, maar de webrollen moeten vaker worden bijgewerkt, kunt u Web Deploy alleen een Webrol bijwerken in uw project. Web Deploy is handig wanneer u niet wilt bouwen en implementeren van de back-end-werkrollen, of als u meerdere web-functies hebt en u wilt bijwerken slechts één van de web-rollen.

### <a name="requirements"></a>Vereisten

Hier volgen de vereisten voor het gebruik van Web Deploy uw Webrol bijwerken:

* **Voor het ontwikkelen en testen van uitsluitend:** de wijzigingen zijn aangebracht rechtstreeks aan de virtuele machine waarop de Webrol wordt uitgevoerd. Als deze virtuele machine moet worden gerecycled, gaan de wijzigingen verloren omdat het oorspronkelijke pakket die u hebt gepubliceerd wordt gebruikt om de virtuele machine voor de rol opnieuw te maken. Uw toepassing om op te halen van de meest recente wijzigingen voor de Webrol opnieuw publiceren.
* **Alleen webrollen kunnen worden bijgewerkt:** werkrollen kunnen niet worden bijgewerkt. U kunt de RoleEntryPoint in web role.cs bovendien niet bijwerken.
* **Biedt slechts ondersteuning voor één exemplaar van een Webrol:** kunt u meerdere exemplaren van een Webrol hebben in uw implementatieomgeving. Meerdere webrollen elke met slechts één exemplaar worden echter ondersteund.
* **Inschakelen van extern bureaublad-verbindingen:** deze vereiste kunt Web Deploy op de gebruiker en het wachtwoord gebruiken voor verbinding met de virtuele machine voor het implementeren van de wijzigingen op de server waarop Internet Information Services (IIS). Bovendien moet u mogelijk verbinding met de virtuele machine een vertrouwd certificaat toevoegen aan IIS op deze virtuele machine. (Dit certificaat zorgt ervoor dat de externe verbinding voor IIS die wordt gebruikt door Web Deploy beveiligd is.)

De volgende procedure wordt ervan uitgegaan dat u de **Publish Azure Application** wizard.

### <a name="enable-web-deploy-when-you-publish-your-application"></a>Web Deploy te schakelen wanneer u uw toepassing publiceren

1. Om in te schakelen de **inschakelen Web Deploy** voor alle web-rollen selectievakje inschakelt, moet u eerst Extern bureaublad-verbindingen configureren. Selecteer **extern bureaublad inschakelen** voor alle rollen en geeft vervolgens de referenties die wordt gebruikt om op afstand in de **configuratie voor extern bureaublad** vak dat wordt weergegeven. Zie [met extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md).
1. Als u Web Deploy voor de webrollen in uw toepassing, schakelt **Web Deploy inschakelen voor alle webrollen**.

    Een gele driehoek van de waarschuwing wordt weergegeven. Web Deploy gebruikt een niet-vertrouwd, zelf-ondertekend certificaat standaard niet aanbevolen wordt voor het uploaden van gevoelige gegevens. Als u dit proces voor gevoelige gegevens te beveiligen moet, kunt u een SSL-certificaat moet worden gebruikt voor verbindingen Web Deploy toevoegen. Dit certificaat moet een vertrouwd certificaat. Zie voor meer informatie [web implementeren beveiligen](#make-web-deploy-secure).
1. Kies **volgende** om weer te geven de **samenvatting** scherm en kies vervolgens **publiceren** voor het implementeren van de cloudservice.

    De cloudservice is gepubliceerd. De virtuele machine die wordt gemaakt, heeft externe verbindingen zijn ingeschakeld voor IIS zodat Web Deploy kan worden gebruikt voor het bijwerken van uw web-rollen zonder deze opnieuw te publiceren.

   > [!NOTE]
   > Als u meer dan één exemplaar is geconfigureerd voor een Webrol hebt, wordt een waarschuwing weergegeven, waarin staat dat elke Webrol is beperkt tot één exemplaar alleen in het pakket dat gemaakt als uw toepassing wilt publiceren. Selecteer **OK** om door te gaan. Zoals vermeld in de sectie vereisten, kunt u meer dan een Webrol, maar slechts één exemplaar van elke rol hebben.

### <a name="update-your-web-role-by-using-web-deploy"></a>Bijwerken van uw Webrol met behulp van Web Deploy

1. Voor het gebruik van Web Deploy, kunt u codewijzigingen aanbrengen aan het project voor een van uw web-rollen in Visual Studio die u wilt publiceren, en vervolgens met de rechtermuisknop op dit projectknooppunt in uw oplossing en wijs **publiceren**. De **webpublicatie** dialoogvenster wordt weergegeven.
1. (Optioneel) Als u een vertrouwd certificaat voor SSL te gebruiken voor externe verbindingen voor IIS hebt toegevoegd, kunt u het selectievakje de **toestaan niet-vertrouwd certificaat** selectievakje. Zie de sectie voor informatie over het toevoegen van een certificaat voor het beveiligen van Web Deploy **te maken Web implementeren beveiligde** verderop in dit artikel.
1. Voor het gebruik van Web Deploy, moet het mechanisme voor publiceren de gebruikersnaam en het wachtwoord dat u zich ingesteld voor verbinding met extern bureaublad wanneer u het pakket voor het eerst hebt gepubliceerd.

   a. In **gebruikersnaam**, geef de gebruikersnaam op.
   b. In **wachtwoord**, voer het wachtwoord.
   c. (Optioneel) Als u dit wachtwoord opslaan in dit profiel wilt, kiest u **wachtwoord opslaan**.
1. Kies voor het publiceren van de wijzigingen aan uw Webrol, **publiceren**.

    Geeft de statusregel **publiceren gestart**. Wanneer het publiceren is voltooid, **publiceren is voltooid** wordt weergegeven. De wijzigingen zijn nu aan de Webrol op uw virtuele machine geïmplementeerd. U kunt nu uw Azure-toepassing starten in de Azure-omgeving om uw wijzigingen te testen.

### <a name="make-web-deploy-secure"></a>Web implementeren beveiligen

1. Web Deploy gebruikt een niet-vertrouwd, zelf-ondertekend certificaat standaard niet aanbevolen wordt voor het uploaden van gevoelige gegevens. Als u dit proces voor gevoelige gegevens te beveiligen moet, kunt u een SSL-certificaat moet worden gebruikt voor verbindingen Web Deploy toevoegen. Dit certificaat moet een vertrouwd certificaat, die u van een certificeringsinstantie (CA ontvangt).

    Web implementeren om veilig te maken voor elke virtuele machine voor elk van uw web-rollen, moet u het vertrouwde certificaat dat u wilt gebruiken voor webtoepassingen uploaden implementeren op de Azure-portal. Dit certificaat zorgt ervoor dat het certificaat is toegevoegd aan de virtuele machine die wordt gemaakt voor de Webrol wanneer u uw toepassing publiceren.
1. Als een vertrouwd certificaat voor SSL wilt IIS moet worden gebruikt voor externe verbindingen toevoegen, de volgende stappen uit:

  a. Voor verbinding met de virtuele machine die wordt uitgevoerd de Webrol, selecteer het exemplaar van de Webrol in **Cloud Explorer** of **Server Explorer**, en kies vervolgens de **verbinding maken met behulp van extern bureaublad**  opdracht. Zie voor gedetailleerde stappen voor het verbinding maken met de virtuele machine [met behulp van extern bureaublad met de Azure-rollen](vs-azure-tools-remote-desktop-roles.md). Uw browser vraagt u voor het downloaden van een `.rdp` bestand.
   b. Als u wilt een SSL-certificaat toevoegen, open de management-service in IIS-beheer. SSL in IIS-beheer inschakelen via de **bindingen** koppelen de **actie** deelvenster. De **sitebinding toevoegen** dialoogvenster wordt weergegeven. Kies **toevoegen**, en kies vervolgens HTTPS in de **Type** vervolgkeuzelijst. In de **SSL-certificaat** kiest u het SSL-certificaat dat u heeft ondertekend door een CA en dat u hebt geüpload naar de Azure-portal. Zie voor meer informatie [verbindingsinstellingen voor de beheerservice configureren](http://go.microsoft.com/fwlink/?LinkId=215824).

      > [!NOTE]
      > Als u een vertrouwd certificaat voor SSL toevoegt, wordt de gele driehoek van de waarschuwing niet meer weergegeven in de **Wizard publiceren**.

## <a name="include-files-in-the-service-package"></a>Bestanden in het servicepakket opnemen

Mogelijk moet u specifieke bestanden opnemen in het servicepakket zodat ze beschikbaar zijn op de virtuele machine die is gemaakt voor een rol. Bijvoorbeeld, u mogelijk wilt toevoegen een `.exe` of een `.msi` -bestand dat wordt gebruikt door een opstartscript aan uw servicepakket. Of moet u mogelijk een assembly waarvoor een rol of de werknemer webrolproject toevoegen. Als bestanden wilt opnemen, moeten ze worden toegevoegd aan de oplossing voor uw Azure-toepassing.

1. Als u wilt een assembly toevoegen aan een servicepakket, gebruik de volgende stappen:

   a. In **Solution Explorer**, opent het projectknooppunt voor het project dat de assembly waarnaar wordt verwezen ontbreekt.
   b. Als de assembly toevoegen aan het project, open het snelmenu voor het **verwijzingen** map en kies vervolgens **verwijzing toevoegen**. Het dialoogvenster verwijzing toevoegen wordt weergegeven.
   c. Kies de referentie die u wilt toevoegen en kies vervolgens **OK**. De verwijzing wordt toegevoegd aan de lijst onder de **verwijzingen** map.
   d. Open het snelmenu voor de assembly die u hebt toegevoegd en kies **eigenschappen**. De **eigenschappen** venster wordt weergegeven.

      Als deze assembly in het servicepakket in de **lokale kopie lijst** kiezen **True**.
1. In **Solution Explorer** opent het projectknooppunt voor het project dat de assembly waarnaar wordt verwezen ontbreekt.
1. Als de assembly toevoegen aan het project, open het snelmenu voor het **verwijzingen** map en kies vervolgens **verwijzing toevoegen**. De **verwijzing toevoegen** dialoogvenster wordt weergegeven.
1. Kies de referentie die u wilt toevoegen en kies vervolgens de **OK** knop.

    De verwijzing wordt toegevoegd aan de lijst onder de **verwijzingen** map.
1. Open het snelmenu voor de assembly die u hebt toegevoegd en kies **eigenschappen**. Het venster Eigenschappen wordt weergegeven.
1. Als deze assembly in het servicepakket in de **lokale kopie** Kies **True**.
1. Het snelmenu voor het bestand openen als bestanden wilt opnemen in het servicepakket die zijn toegevoegd aan uw webproject rol, en kies vervolgens **eigenschappen**. Van de **eigenschappen** venster kiezen **inhoud** van de **Build Action** keuzelijst met invoervak.
1. Om bestanden in het servicepakket die zijn toegevoegd aan uw werkrolproject, open het snelmenu voor het bestand en kies vervolgens **eigenschappen**. Van de **eigenschappen** venster kiezen **kopiëren indien nieuwer** van de **naar uitvoermap kopiëren** keuzelijst met invoervak.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over publiceren naar Azure vanuit Visual Studio, [Publish Azure Application Wizard](vs-azure-tools-publish-azure-application-wizard.md).
