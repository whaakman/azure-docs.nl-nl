---
title: Publiceren van een Cloudservice met de Azure-hulpprogramma's | Microsoft Docs
description: Meer informatie over het publiceren van Azure cloud service-projecten met behulp van Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: a60f00e1df994988bfeae112db10bffa43f81642
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969494"
---
# <a name="publishing-a-cloud-service-using-visual-studio"></a>Publiceren van een cloudservice met Visual Studio

Visual Studio te een toepassing rechtstreeks naar Azure, met ondersteuning voor zowel fasering en productie-omgevingen van een cloudservice publiceren. Bij het publiceren, selecteert u de implementatieomgeving en een opslagaccount dat tijdelijk wordt gebruikt voor het implementatiepakket.

Wanneer u ontwikkelen en testen van een Azure-toepassing, kunt u Web Deploy gebruiken om wijzigingen te publiceren incrementeel voor uw web-rollen. Nadat u uw toepassing in een implementatieomgeving publiceren, kunt Web Deploy u wijzigingen worden geïmplementeerd met de virtuele machine die wordt uitgevoerd de Webrol. U hoeft niet kunnen worden verpakt en uw gehele Azure-toepassing telkens wanneer die u wilt bijwerken van uw Webrol voor het testen van de wijzigingen te publiceren. Met deze methode kunt u uw web-rol wijzigingen die beschikbaar zijn in de cloud voor testen zonder te wachten op uw toepassing die is gepubliceerd naar een implementatieomgeving hebt.

Gebruik de volgende procedures voor het publiceren van uw Azure-toepassing en een Webrol bijwerken met behulp van Web Deploy:

- Publiceren of het pakket van een Azure-toepassing vanuit Visual Studio
- Een Webrol als onderdeel van de ontwikkeling en een testcyclus bijwerken

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publiceren of het pakket van een Azure-toepassing vanuit Visual Studio

Wanneer u uw Azure-toepassing publiceert, kunt u een van de volgende taken kunt doen:

- Een servicepakket maken: U kunt dit pakket en het configuratiebestand van de service gebruiken voor het publiceren van uw toepassing in een implementatieomgeving van de [Azure-portal](https://portal.azure.com).

- Uw Azure-project in Visual Studio publiceren: voor het publiceren van uw toepassing rechtstreeks naar Azure, gebruikt u de Wizard publiceren. Zie voor meer informatie, [Wizard voor Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Een servicepakket maken vanuit Visual Studio

1. Wanneer u klaar om uw toepassing te publiceren bent, Solution Explorer openen, opent u het snelmenu voor het Azure-project met de rollen en kies publiceren.

1. Volg deze stappen voor het maken van alleen een servicepakket:

   a. In het snelmenu voor het Azure-project, kies **pakket**.

   b. In de **pakket Azure-toepassing** in het dialoogvenster, kiest u de configuratie van de service waarvoor u wenst te maken van een pakket en kies vervolgens de buildconfiguratie van de.

   c. (Optioneel) Als u wilt inschakelen op de extern bureaublad voor de cloudservice nadat u deze hebt gepubliceerd, selecteert u **extern bureaublad inschakelen voor alle rollen**, en selecteer vervolgens **instellingen** extern bureaublad-referenties configureren. Zie voor meer informatie, [extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

      Als u fouten opsporen in uw cloudservice wilt nadat u deze hebt gepubliceerd, schakelt u over foutopsporing op afstand door te selecteren **externe foutopsporing inschakelen voor alle rollen**.

   d. Kies voor het maken van het pakket, de **pakket** koppeling.

      Verkenner toont de locatie van het bestand van het nieuwe pakket. U kunt deze locatie kopiëren zodat u deze vanuit Azure portal gebruiken kunt.

   e. Als u wilt publiceren dit pakket naar een implementatieomgeving, moet u deze locatie gebruiken als de locatie van het pakket wanneer u een cloudservice maken en implementeren van dit pakket in een omgeving met de Azure-portal.

1. (Optioneel) Als u wilt annuleren van het implementatieproces, in het snelmenu voor het regelitem in het activiteitenlogboek, kies **annuleren en verwijderen**. Met deze opdracht stopt het implementatieproces en verwijdert u de implementatieomgeving van Azure. Als u wilt verwijderen van de omgeving na de implementatie, gebruikt u de Azure portal.

1. (Optioneel) Nadat uw rolinstanties hebt gestart, Visual Studio automatisch ziet u de implementatieomgeving in de **Cloudservices** knooppunt in Server Explorer. Hier ziet u de status van de afzonderlijke rolinstanties. Zie [beheren Azure-resources met Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md). De volgende afbeelding ziet u de rolinstanties terwijl ze nog steeds in de status Bezig met initialiseren worden:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Een Webrol als onderdeel van de ontwikkeling en een testcyclus bijwerken

Als de back-end-infrastructuur van uw app stabiel is, maar de webrollen meer regelmatig moeten worden bijgewerkt, kunt u Web Deploy kunt gebruiken om bij te werken alleen een Webrol in uw project. Web Deploy is handig als u niet wilt opnieuw maken en implementeren van de back-end-werkrollen, of als u meerdere web-functies hebt en u wilt bijwerken slechts één van de webrollen.

### <a name="requirements-for-using-web-deploy"></a>Vereisten voor het gebruik van Web Deploy

- **Voor ontwikkeling en testen uitsluitend bedoeld**: de wijzigingen zijn aangebracht rechtstreeks aan de virtuele machine waarop de Webrol wordt uitgevoerd. Als deze virtuele machine moet worden gerecycled, gaan de wijzigingen verloren omdat het oorspronkelijke pakket die u hebt gepubliceerd, wordt gebruikt om de virtuele machine voor de rol opnieuw te maken. Uw toepassing om op te halen van de meest recente wijzigingen voor de Webrol opnieuw publiceren.

- **Alleen webrollen kunnen worden bijgewerkt**: werkrollen kunnen niet worden bijgewerkt. Bovendien kunt u niet bijwerken de `RoleEntryPoint` in `web role.cs`.

- **Biedt alleen ondersteuning voor één exemplaar van een Webrol**: U kan niet meerdere exemplaren van een Webrol hebt in uw implementatieomgeving. Meerdere webrollen elke met slechts één instantie worden echter ondersteund.

- **Verbindingen met extern bureaublad inschakelen**: deze vereiste kunt Web Deploy op de gebruiker en het wachtwoord gebruiken voor verbinding met de virtuele machine waarop de wijzigingen naar de server die Internet Information Services (IIS) wordt uitgevoerd. Bovendien moet u mogelijk verbinding maken met de virtuele machine een vertrouwd certificaat toevoegt aan IIS op deze virtuele machine. (Dit certificaat zorgt ervoor dat de externe verbinding voor IIS dat wordt gebruikt door Web Deploy beveiligd is.)

De volgende procedure wordt ervan uitgegaan dat u gebruikmaakt van de **Publish Azure Application** wizard.

### <a name="enable-web-deploy-when-you-publish-your-application"></a>Web Deploy inschakelen wanneer u uw toepassing publiceren

1. Om in te schakelen de **Web Deploy inschakelen voor alle webrollen** optie, moet u eerst Extern bureaublad-verbindingen configureren. Selecteer **extern bureaublad inschakelen** voor alle rollen en geef vervolgens de referenties die wordt gebruikt voor verbinding op afstand in de **configuratie voor extern bureaublad** vak dat wordt weergegeven. Zie [extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

1. Als u Web Deploy voor de webrollen in uw toepassing, schakelt **Web Deploy inschakelen voor alle webrollen**.

    Een gele driehoek van de waarschuwing wordt weergegeven. Web Deploy maakt gebruik van een niet-vertrouwd, zelf-ondertekend certificaat standaard, wat niet wordt aanbevolen voor het uploaden van gevoelige gegevens. Als u nodig hebt voor het beveiligen van dit proces voor gevoelige gegevens, kunt u een SSL-certificaat moet worden gebruikt voor verbindingen met Web Deploy toevoegen. Dit certificaat moet een vertrouwd certificaat. Zie voor meer informatie, [web implementeren beveiligen](#make-web-deploy-secure).

1. Kies **volgende** om weer te geven de **samenvatting** scherm en kies vervolgens **publiceren** om de cloudservice te implementeren.

    De cloudservice is gepubliceerd. De virtuele machine die is gemaakt, heeft externe verbindingen zijn ingeschakeld voor IIS zodat Web Deploy kan worden gebruikt om uw webrollen bijwerken zonder dat ze opnieuw te publiceren.

   > [!NOTE]
   > Hebt u meer dan één exemplaar dat is geconfigureerd voor een Webrol, verschijnt er een waarschuwingsbericht wordt weergegeven waarin wordt vermeld dat elke Webrol beperkt tot één exemplaar alleen in het pakket dat gemaakt is voor het publiceren van uw toepassing. Selecteer **OK** om door te gaan. Zoals vermeld in de sectie vereisten, beschikt u over meer dan één Webrol, maar slechts één exemplaar van elke rol weer te geven.

### <a name="update-your-web-role-by-using-web-deploy"></a>Uw Webrol bijwerken met behulp van Web Deploy

1. Voor het gebruik van Web Deploy, kunt u codewijzigingen aanbrengen aan het project voor het gebruik van uw web-rollen in Visual Studio die u wilt publiceren, en vervolgens met de rechtermuisknop op dit projectknooppunt in uw oplossing en wijs **publiceren**. De **webpublicatie** in het dialoogvenster wordt weergegeven.

1. (Optioneel) Als u een vertrouwd SSL-certificaat te gebruiken voor externe verbindingen voor IIS hebt toegevoegd, schakelt u de **toestaan niet-vertrouwd certificaat** selectievakje. Zie de sectie voor informatie over het toevoegen van een certificaat voor het beveiligen van Web Deploy **te maken Web implementeren beveiligde** verderop in dit artikel.

1. Voor het gebruik van Web Deploy, moet het mechanisme voor publiceren de gebruikersnaam en het wachtwoord dat u zich voor uw extern bureaublad-verbinding instellen wanneer het pakket voor het eerst wordt gepubliceerd.

   a. In **gebruikersnaam**, voer de gebruikersnaam in.

   b. In **wachtwoord**, voert u het wachtwoord.

   c. (Optioneel) Als u dit wachtwoord opslaan in dit profiel wilt, kiest u **wachtwoord opslaan**.

1. Kies voor het publiceren van de wijzigingen aan uw Webrol, **publiceren**.

    De statusregel weergegeven **publiceren gestart**. Wanneer het publiceren is voltooid, **publiceren is voltooid** wordt weergegeven. Nu zijn de wijzigingen die geïmplementeerd op de Webrol op uw virtuele machine. U kunt nu uw Azure-toepassing starten in de Azure-omgeving om uw wijzigingen te testen.

### <a name="make-web-deploy-secure"></a>Web implementeren beveiligen

1. Web Deploy maakt gebruik van een niet-vertrouwd, zelf-ondertekend certificaat standaard, wat niet wordt aanbevolen voor het uploaden van gevoelige gegevens. Als u nodig hebt voor het beveiligen van dit proces voor gevoelige gegevens, kunt u een SSL-certificaat moet worden gebruikt voor verbindingen met Web Deploy toevoegen. Dit certificaat moet een vertrouwd certificaat, die u van een certificeringsinstantie (CA ontvangt).

    Web Deploy om veilig te maken voor elke virtuele machine voor elk van uw web-rollen, moet u het vertrouwde certificaat dat u wilt gebruiken voor web uploaden implementeren naar Azure portal. Dit certificaat zorgt ervoor dat het certificaat is toegevoegd aan de virtuele machine die voor de Webrol wordt gemaakt wanneer u uw toepassing.

1. Een vertrouwd SSL-certificaat toevoegen aan IIS moet worden gebruikt voor externe verbindingen, de volgende stappen uit:

   a. Selecteer het exemplaar van de Webrol in voor verbinding met de virtuele machine die wordt uitgevoerd de Webrol, **Cloud Explorer** of **Server Explorer**, en kies vervolgens de **verbinding maken met behulp van extern bureaublad**  opdracht. Zie voor gedetailleerde stappen over het verbinding maken met de virtuele machine [extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md). Uw browser wordt gevraagd om u voor het downloaden van een `.rdp` bestand.

   b. Als u wilt toevoegen een SSL-certificaat, opent u de management-service in IIS-beheer. In de IIS-beheer SSL inschakelen door het openen van de **bindingen** herstelkoppeling in de **actie** deelvenster. De **sitebinding toevoegen** in het dialoogvenster wordt weergegeven. Kies **toevoegen**, en kies vervolgens HTTPS in de **Type** vervolgkeuzelijst. In de **SSL-certificaat** kiest u het SSL-certificaat dat u had ondertekend door een CA en dat u hebt geüpload naar de Azure-portal. Zie voor meer informatie, [verbindingsinstellingen configureren voor de beheerservice](http://go.microsoft.com/fwlink/?LinkId=215824).

      > [!NOTE]
      > Als u een vertrouwd SSL-certificaat toevoegt, wordt de gele driehoek van de waarschuwing niet meer weergegeven in de **Wizard publiceren**.

## <a name="include-files-in-the-service-package"></a>Bestanden opnemen in het servicepakket

Mogelijk moet u specifieke bestanden opnemen in uw service-pakket, zodat ze beschikbaar zijn op de virtuele machine die is gemaakt voor een rol. Bijvoorbeeld, u mogelijk wilt toevoegen een `.exe` of een `.msi` -bestand dat wordt gebruikt door een opstartscript naar uw servicepakket. Of u moet mogelijk om toe te voegen een assembly die een project web of worker-rol is vereist. Als u wilt bestanden bevatten, moeten ze worden toegevoegd aan de oplossing voor uw Azure-toepassing.

1. Als u wilt een assembly toevoegen aan een servicepakket, gebruikt u de volgende stappen uit:

   a. In **Solution Explorer**, opent u het projectknooppunt van het project dat de assembly waarnaar wordt verwezen, ontbreekt.
   b. De assembly toevoegen aan het project, open het snelmenu voor het **verwijzingen** map en kies vervolgens **verwijzing toevoegen**. Het dialoogvenster verwijzing toevoegen wordt weergegeven.
   c. De verwijzing die u wilt toevoegen en kies vervolgens kies **OK**. De verwijzing wordt toegevoegd aan de lijst onder de **verwijzingen** map.
   d. Open het snelmenu voor de assembly die u hebt toegevoegd en kies **eigenschappen**. De **eigenschappen** venster wordt weergegeven.

      Deze assembly in het servicepakket opnemen in de **lokale kopie lijst** Kies **waar**.
1. In **Solution Explorer** opent u het projectknooppunt van het project dat de assembly waarnaar wordt verwezen, ontbreekt.

1. De assembly toevoegen aan het project, open het snelmenu voor het **verwijzingen** map en kies vervolgens **verwijzing toevoegen**. De **verwijzing toevoegen** dialoogvenster wordt weergegeven.

1. Kies de referentie die u wilt toevoegen en kies vervolgens de **OK** knop.

    De verwijzing wordt toegevoegd aan de lijst onder de **verwijzingen** map.

1. Open het snelmenu voor de assembly die u hebt toegevoegd en kies **eigenschappen**. Het venster Eigenschappen wordt weergegeven.

1. Deze assembly in het servicepakket opnemen in de **lokale kopie** Kies **waar**.

1. Als u wilt bestanden opnemen in het servicepakket die zijn toegevoegd aan uw webproject rol, open het snelmenu voor het bestand en kies vervolgens **eigenschappen**. Uit de **eigenschappen** venster, kiest u **inhoud** uit de **Buildactie** keuzelijst.

1. Als u wilt bestanden opnemen in het servicepakket die zijn toegevoegd aan uw werkrolproject, open het snelmenu voor het bestand en kies vervolgens **eigenschappen**. Uit de **eigenschappen** venster, kiest u **kopiëren indien nieuwer** uit de **naar uitvoermap kopiëren** keuzelijst.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het publiceren naar Azure vanuit Visual Studio, [Wizard voor Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md).
