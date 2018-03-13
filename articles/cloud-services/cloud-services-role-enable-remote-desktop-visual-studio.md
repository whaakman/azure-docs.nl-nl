---
title: Inschakelen van extern bureaublad-Connection voor een rol in Azure-Cloudservices | Microsoft Docs
description: Het configureren van uw Azure-cloud-servicetoepassing voor het toestaan van extern bureaublad-verbindingen
services: cloud-services
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: kraigb
ms.openlocfilehash: bcb7d24a302b57a1739c9c98763cd658c29b17bd
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-visual-studio"></a>Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met Visual Studio

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Extern bureaublad kunt u toegang tot het bureaublad van een rol in Azure wordt uitgevoerd. U kunt een verbinding met extern bureaublad gebruiken oplossen en analyseren van problemen met uw toepassing, terwijl deze wordt uitgevoerd.

De wizard Publiceren met Visual Studio voor cloud-services bevat een optie voor het extern bureaublad inschakelen tijdens het publicatieproces met referenties die u opgeeft. Met deze optie is geschikt als u Visual Studio 2017 versie 15,4 en eerdere versies.

Met Visual Studio 2017 versie 15.5 en hoger, wordt dit echter aanbevolen te voorkomen dat inschakelen van extern bureaublad met de wizard publiceren, tenzij u alleen als één ontwikkelaar werkt. Een situatie waarin het project kan worden geopend door andere ontwikkelaars vindt inschakelen u in plaats daarvan Extern bureaublad via de Azure portal via PowerShell of vanuit een release-definitie in een werkstroom continue implementatie. Deze aanbeveling is vanwege een wijziging in hoe Visual Studio met extern bureaublad op de cloudservice VM, communiceert zoals in dit artikel wordt uitgelegd.

## <a name="configure-remote-desktop-through-visual-studio-2017-version-154-and-earlier"></a>Extern bureaublad via Visual Studio 2017 versie 15,4 en ouder configureren

Wanneer u Visual Studio 2017 versie 15,4 en ouder gebruikt, kunt u de **extern bureaublad inschakelen voor alle rollen** optie in de wizard publiceren. U kunt nog steeds gebruik van de wizard met Visual Studio 2017 versie 15.5 en hoger, maar niet de optie Extern bureaublad gebruiken.

1. In Visual Studio, start u de wizard Publiceren met de rechtermuisknop op uw cloud service-project in Solution Explorer kiezen **publiceren**.

2. Meld u aan bij uw Azure-abonnement, indien nodig, en selecteer **volgende**.

3. Op de **instellingen** pagina **extern bureaublad inschakelen voor alle rollen**, selecteer vervolgens de **instellingen...**  koppeling openen de **configuratie voor extern bureaublad** in het dialoogvenster.

4. Selecteer onderaan in het dialoogvenster **meer opties**. Deze opdracht wordt een vervolgkeuzelijst die u maakt of Selecteer een certificaat dat u referenties gegevens versleutelen kunt om verbinding te maken via Extern bureaublad weergegeven.

   > [!Note]
   > De certificaten die u nodig hebt voor een verbinding met extern bureaublad wijken af van de certificaten die u voor andere Azure-bewerkingen gebruikt. De RAS-certificaat moet een persoonlijke sleutel hebben.

5. Selecteer een certificaat in de lijst of kies  **&lt;maken... &gt;**. Als u een nieuw certificaat maakt, Geef een beschrijvende naam voor het nieuwe certificaat wanneer u wordt gevraagd en selecteert u **OK**. Het nieuwe certificaat wordt weergegeven in het vak vervolgkeuzelijst.

6. Geef een gebruikersnaam en wachtwoord. U kunt een bestaand account niet gebruiken. Gebruik niet 'Administrator' als de gebruikersnaam op voor het nieuwe account.

7. Kies een datum op waarop het account verloopt en na welke Extern bureaublad-verbindingen worden geblokkeerd.

8. Nadat u de vereiste gegevens hebt opgegeven, selecteert u **OK**. Visual Studio voegt de extern bureaublad-instellingen toe aan uw project `.cscfg` en `.csdef` bestanden, met inbegrip van het wachtwoord dat versleuteld met behulp van het gekozen.

9. Alle resterende stappen met behulp van de **volgende** knop en selecteer vervolgens **publiceren** wanneer u klaar bent voor het publiceren van uw cloudservice. Als u niet gereed om te publiceren bent, selecteert u **annuleren** en het antwoord **Ja** desgevraagd wijzigingen op te slaan. Met deze instellingen kunt u uw cloudservice later publiceren.

## <a name="configure-remote-desktop-when-using-visual-studio-2017-version-155-and-later"></a>Extern bureaublad configureren bij gebruik van Visual Studio 2017 versie 15.5 en hoger

Met Visual Studio 2017 versie 15.5 en hoger, kunt u de wizard Publiceren met een cloudserviceproject. U kunt ook de **extern bureaublad inschakelen voor alle rollen** optie als u alleen als één ontwikkelaar werkt.

Als u als onderdeel van een team werkt, moet u in plaats daarvan Extern bureaublad op de Azure-cloud-service inschakelen met behulp van de [Azure-portal](cloud-services-role-enable-remote-desktop-new-portal.md) of [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

Deze aanbeveling is vanwege een wijziging in hoe Visual Studio 2017 versie 15.5 en hoger met de cloudservice-VM communiceert. Bij het inschakelen van extern bureaublad met de wizard publish communiceren eerdere versies van Visual Studio met de virtuele machine via een zogeheten de 'RDP-invoegtoepassing." Visual Studio 2017 versie 15.5 en hoger communiceert in plaats daarvan met de extensie' RDP' die is veiliger en flexibeler. Deze wijziging is ook wordt uitgelijnd met het feit dat de Azure-portal en PowerShell-methoden voor het inschakelen van extern bureaublad ook gebruik van de RDP-extensie.

Wanneer Visual Studio communiceert met de RDP-extensie, verzendt het een wachtwoord als tekst zonder opmaak via SSL. De configuratiebestanden van het project opslaan echter alleen een gecodeerd wachtwoord, die kan worden ontsleuteld naar tekst zonder opmaak alleen met het lokale certificaatarchief die oorspronkelijk werd gebruikt om ze te versleutelen.

Als u op elke keer dat het cloudserviceproject van dezelfde ontwikkelingscomputer implementeert, zijn dat lokale certificaat is beschikbaar. In dit geval kunt u de **extern bureaublad inschakelen voor alle rollen** optie in de wizard publiceren.

Als u of andere ontwikkelaars wilt het cloudserviceproject van andere computers te implementeren, maar vervolgens deze andere computers geen het vereiste certificaat voor het ontsleutelen van het wachtwoord. Als gevolg hiervan, ziet u het volgende foutbericht weergegeven:

```output
Applying remote desktop protocol (RDP) extension.
Certificate with thumbprint [thumbprint] doesn't exist.
```

Telkens wanneer u de cloudservice implementeren, maar deze actie wordt onhandig voor iedereen die extern bureaublad gebruiken, kunt u het wachtwoord wijzigen.

Als u het project met een team deelt, wordt het aanbevolen Schakel de optie in de wizard Publiceren en in plaats daarvan inschakelen van extern bureaublad rechtstreeks via de [Azure-portal](cloud-services-role-enable-remote-desktop-new-portal.md) of met behulp van [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

### <a name="deploying-from-a-build-server-with-visual-studio-2017-version-155-and-later"></a>Implementeren vanaf een server bouwen met Visual Studio 2017 versie 15.5 en hoger

U kunt een cloudserviceproject van een buildserver (bijvoorbeeld met Visual Studio Team Services) op welke Visual Studio-2017 15.5 of hoger is geïnstalleerd in de build-agent implementeren. Met deze benadering plaats implementatie op de computer waarop het versleutelingscertificaat beschikbaar is.

Voor het gebruik van de RDP-extensie van Visual Studio Team Services, zijn de volgende gegevens in de definitie van de build:

1. Omvatten `/p:ForceRDPExtensionOverPlugin=true` in uw MSBuild-argumenten om ervoor te zorgen dat de implementatie werkt met de RDP-extensie in plaats van de RDP-invoegtoepassing. Bijvoorbeeld:

    ```
    msbuild AzureCloudService5.ccproj /t:Publish /p:TargetProfile=Cloud /p:DebugType=None
        /p:SkipInvalidConfigurations=true /p:ForceRDPExtensionOverPlugin=true
    ```

1. Nadat de stappen van de build toevoegen de **Azure Cloud Service-implementatie** stap en stel de eigenschappen.

1. Na de stap implementatie toevoegen een **Azure Powershell** stap, stelt u de **weergavenaam** eigenschap in op 'Azure-implementatie: inschakelen RDP-uitbreiding' (of een andere geschikte naam), en selecteer de juiste Azure abonnement.

1. Stel **scripttype** 'in 'inline en plak de onderstaande code in de **Inline-Script** veld. (U kunt ook maken een `.ps1` bestand in uw project met dit script **scripttype** 'Pad naar scriptbestand' en een set **scriptpad** om te verwijzen naar het bestand.)

    ```ps
    Param(
        [Parameter(Mandatory=$True)]
        [string]$username,

        [Parameter(Mandatory=$True)]
        [string]$password,

        [Parameter(Mandatory=$True)]
        [string]$serviceName,

        [Datetime]$expiry = ($(Get-Date).AddYears(1))
    )

    Write-Host "Service Name: $serviceName"
    Write-Host "User Name: $username"
    Write-Host "Expiry: $expiry"

    $securepassword = ConvertTo-SecureString -String $password -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential $username,$securepassword

    # Try to remote existing RDP Extensions
    try
    {
        $existingRDPExtension = Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
        if ($existingRDPExtension -ne $null)
        {
            Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
        }
    }
    catch
    {
    }

    Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry -Verbose
    ```

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Verbinding maken met een Azure-rol met behulp van extern bureaublad

Nadat u uw cloudservice op Azure publiceert en extern bureaublad hebt ingeschakeld, kunt u Visual Studio Server Explorer gebruiken voor aanmelding bij de cloudservice-VM:

1. Vouw in Server Explorer de **Azure** knooppunt, en vouw vervolgens het knooppunt voor een cloudservice en een van de rollen voor het weergeven van exemplaren.

2. Met de rechtermuisknop op een knooppunt van het exemplaar en selecteer **verbinding maken met behulp van extern bureaublad**.

3. Geef de gebruikersnaam en wachtwoord dat u eerder hebt gemaakt. U bent nu aangemeld bij de externe sessie.

## <a name="additional-resources"></a>Aanvullende resources

[Cloud Services configureren](cloud-services-how-to-configure-portal.md)