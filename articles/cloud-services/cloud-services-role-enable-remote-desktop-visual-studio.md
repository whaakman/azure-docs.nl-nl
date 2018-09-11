---
title: Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen
description: Uw Azure-cloud service-toepassing om toe te staan van verbindingen met extern bureaublad configureren
services: cloud-services
author: ghogen
manager: douge
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.workload: azure-vs
ms.date: 03/06/2018
ms.author: ghogen
ms.openlocfilehash: 703e969fe31def329be60037cceba27864063b4e
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304047"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-visual-studio"></a>Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van Visual Studio

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Extern bureaublad kunt u toegang tot het bureaublad van een functie die wordt uitgevoerd in Azure. U kunt een extern bureaublad-verbinding oplossen en diagnosticeren van problemen met uw toepassing terwijl deze wordt uitgevoerd.

De wizard publiceren die Visual Studio voor cloudservices biedt bevat een optie voor het inschakelen van extern bureaublad gedurende het publicatieproces, met behulp van referenties die u opgeeft. Met deze optie is geschikt wanneer u Visual Studio 2017 versie 15.4 en eerdere versies.

Met Visual Studio 2017 versie 15.5 of hoger, maar het verdient aanbeveling dat u extern bureaublad inschakelen via de wizard publiceren, tenzij u alleen als een ontwikkelaar van één werkt vermijden. Voor een situatie waarin het project kan worden geopend door andere ontwikkelaars, schakel u extern bureaublad in plaats daarvan via Azure portal, via PowerShell of via een release-pijplijn in een werkstroom voor continue implementatie. Deze aanbeveling is vanwege een wijziging in hoe Visual Studio met extern bureaublad op de cloudservice-VM, communiceert zoals in dit artikel wordt uitgelegd.

## <a name="configure-remote-desktop-through-visual-studio-2017-version-154-and-earlier"></a>Configureren van extern bureaublad via Visual Studio 2017 versie 15.4 en lager

Wanneer u Visual Studio 2017 versie 15.4 of eerder gebruikt, kunt u de **extern bureaublad inschakelen voor alle rollen** in de wizard publiceren. U kunt nog steeds gebruik van de wizard met Visual Studio 2017 versie 15.5 of hoger, maar niet de optie Extern bureaublad gebruiken.

1. In Visual Studio, start u de wizard Publiceren met de rechtermuisknop op uw cloud service-project in Solution Explorer en kies **publiceren**.

2. Meld u aan bij uw Azure-abonnement, indien nodig en selecteer **volgende**.

3. Op de **instellingen** weergeeft, schakelt **extern bureaublad inschakelen voor alle rollen**en selecteer vervolgens de **instellingen...**  koppeling te openen de **configuratie voor extern bureaublad** in het dialoogvenster.

4. Selecteer aan de onderkant van het dialoogvenster **meer opties**. Deze opdracht wordt een vervolgkeuzelijst die u maakt of een certificaat kiezen zodat u informatie over beveiligingsreferenties versleutelen kunt wanneer u verbinding maakt via Extern bureaublad weergegeven.

   > [!Note]
   > De certificaten die u nodig hebt voor een verbinding met extern bureaublad verschillen van de certificaten die u voor andere Azure-bewerkingen gebruikt. De RAS-certificaat moet een persoonlijke sleutel hebben.

5. Selecteer een certificaat in de lijst of kies  **&lt;maken... &gt;**. Als u een nieuw certificaat maakt, Geef een beschrijvende naam voor het nieuwe certificaat wanneer u hierom wordt gevraagd en selecteert u **OK**. Het nieuwe certificaat wordt weergegeven in de vervolgkeuzelijst lijst.

6. Geef een gebruikersnaam en wachtwoord. U kunt een bestaand account niet gebruiken. Gebruik niet 'Beheerder' als de naam van de gebruiker voor het nieuwe account.

7. Kies een datum op waarop het account verloopt en na welke Extern bureaublad-verbindingen worden geblokkeerd.

8. Nadat u de vereiste gegevens hebt opgegeven, selecteert u **OK**. Visual Studio voegt de extern bureaublad-instellingen toe aan van uw project `.cscfg` en `.csdef` bestanden, met inbegrip van het wachtwoord dat versleuteld met behulp van het gekozen certificaat.

9. Alle overige stappen met behulp van de **volgende** knop en selecteer vervolgens **publiceren** wanneer u bent gereed voor het publiceren van uw cloudservice. Als u niet klaar om te publiceren bent, selecteert u **annuleren** en -antwoordsessie **Ja** wanneer hierom wordt gevraagd wijzigingen op te slaan. U kunt uw cloudservice later publiceren met deze instellingen.

## <a name="configure-remote-desktop-when-using-visual-studio-2017-version-155-and-later"></a>Extern bureaublad configureren bij gebruik van Visual Studio 2017 versie 15.5 of hoger

Met Visual Studio 2017 versie 15.5 of hoger, kunt u nog steeds gebruik van de wizard Publiceren met een cloud service-project. U kunt ook de **extern bureaublad inschakelen voor alle rollen** optie als u alleen als een één-ontwikkelaar werkt.

Als u als onderdeel van een team werkt, moet u extern bureaublad op de Azure-cloud-service in plaats daarvan inschakelen met behulp van de [Azure-portal](cloud-services-role-enable-remote-desktop-new-portal.md) of [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

Deze aanbeveling is vanwege een wijziging in hoe Visual Studio 2017 versie 15.5 of hoger met de cloudservice-VM communiceert. Bij het inschakelen van extern bureaublad met de wizard publiceren, communiceren eerdere versies van Visual Studio met de virtuele machine via een zogeheten de "RDP-invoegtoepassing." Visual Studio 2017 versie 15.5 of hoger communiceert in plaats daarvan met de extensie' RDP' dat is veiliger en meer flexibiliteit. Deze wijziging is ook worden uitgelijnd met het feit dat de Azure-portal en PowerShell-methoden voor het inschakelen van extern bureaublad ook gebruik van de RDP-extensie.

Wanneer Visual Studio communiceert met de RDP-extensie, verzendt deze een wachtwoord als tekst zonder opmaak via SSL. De configuratiebestanden van het project wordt echter alleen een versleuteld wachtwoord, die kan worden ontsleuteld in tekst zonder opmaak alleen met het lokale certificaatarchief die oorspronkelijk is gebruikt voor het versleutelen van het opslaan.

Als u het cloudserviceproject op de dezelfde ontwikkelcomputer elke keer implementeert is dat certificaat lokaal beschikbaar. In dit geval kunt u de **extern bureaublad inschakelen voor alle rollen** in de wizard publiceren.

Als u of andere ontwikkelaars implementeren, het cloudserviceproject op verschillende computers wilt, maar vervolgens deze andere computers geen het vereiste certificaat voor het ontsleutelen van het wachtwoord. Als gevolg hiervan, ziet u de volgende strekking weergegeven:

```output
Applying remote desktop protocol (RDP) extension.
Certificate with thumbprint [thumbprint] doesn't exist.
```

U kunt het wachtwoord wijzigen telkens wanneer u de service in de cloud implementeren, maar deze actie wordt onhandig voor iedereen die moet extern bureaublad gebruiken.

Als u het project met een team deelt, vervolgens het is raadzaam om te wissen van de optie in de wizard Publiceren en in plaats daarvan inschakelen van extern bureaublad rechtstreeks via de [Azure-portal](cloud-services-role-enable-remote-desktop-new-portal.md) of met behulp van [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

### <a name="deploying-from-a-build-server-with-visual-studio-2017-version-155-and-later"></a>Implementeren vanaf een server maken met Visual Studio 2017 versie 15.5 of hoger

U kunt een cloud service-project van een buildserver (bijvoorbeeld met Azure DevOps-Services) op welke Visual Studio 2017 versie 15.5 of hoger is geïnstalleerd in de build-agent implementeren. Met deze overeenkomst plaats implementatie op de computer waarop het versleutelingscertificaat dat beschikbaar is.

Voor het gebruik van de RDP-extensie van Azure DevOps-Services, zijn onder andere de volgende gegevens in de build-pijplijn:

1. Opnemen `/p:ForceRDPExtensionOverPlugin=true` in uw MSBuild-argumenten om ervoor te zorgen dat de implementatie werkt met de RDP-extensie in plaats van de RDP-invoegtoepassing. Bijvoorbeeld:

    ```
    msbuild AzureCloudService5.ccproj /t:Publish /p:TargetProfile=Cloud /p:DebugType=None
        /p:SkipInvalidConfigurations=true /p:ForceRDPExtensionOverPlugin=true
    ```

1. Nadat uw build-stappen toevoegen de **Azure Cloud Service-implementatie** stap en de eigenschappen instellen.

1. Na de implementatiestap van de toevoegen een **Azure Powershell** stap, stelt u de **weergavenaam** eigenschap in op "Azure-implementatie: inschakelen RDP-extensie" (of een andere geschikte naam), en selecteer de juiste Azure het abonnement.

1. Stel **scripttype** aan als zijnde "Inline" en plak de onderstaande code in de **Inline Script** veld. (U kunt ook maken een `.ps1` -bestand in uw project met dit script, stelt u de **scripttype** naar "Pad naar scriptbestand" en stel **scriptpad** om te verwijzen naar het bestand.)

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

Nadat u uw cloudservice op Azure publiceren en extern bureaublad hebt ingeschakeld, kunt u Visual Studio Server Explorer kunt gebruiken voor aanmelding bij de cloudservice-VM:

1. Vouw in Server Explorer, de **Azure** knooppunt, en vouw het knooppunt voor een service in de cloud en een van de rollen om een lijst met exemplaren weer te geven.

2. Met de rechtermuisknop op een knooppunt van het exemplaar en selecteer **verbinding maken met behulp van extern bureaublad**.

3. Voer de gebruikersnaam en wachtwoord die u eerder hebt gemaakt. U bent nu aangemeld bij de externe sessie.

## <a name="additional-resources"></a>Aanvullende bronnen

[Cloud Services configureren](cloud-services-how-to-configure-portal.md)
