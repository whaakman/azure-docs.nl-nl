---
title: Implementeren van de lokale agent | Microsoft Docs
description: De lokale agent voor Azure Stack-validatie als een Service implementeren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: cfea454b20b010148eba063ec724e55134944ac3
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482926"
---
# <a name="deploy-the-local-agent"></a>De lokale agent implementeren

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Informatie over het gebruik van de validatie als een lokale agent Service (VaaS) om validatietests uit te voeren. De lokale agent moet worden geïmplementeerd voordat u validatietests uitvoert.

> [!Note]  
> Zorg ervoor dat de computer waarop de lokale agent wordt uitgevoerd geen uitgaande geen toegang meer tot het internet. Deze machine moet uitsluitend toegankelijk voor gebruikers die zijn gemachtigd voor het gebruik van VaaS namens uw tenant.

De lokale agent implementeren:

1. De lokale agent installeren.
2. Bevestigingen uitvoeren.
3. De lokale agent worden uitgevoerd.

## <a name="download-and-start-the-local-agent"></a>Downloaden en de lokale agent starten

Download de agent op een computer die voldoet aan de vereisten in uw datacenter en toegang heeft tot alle Azure Stack-eindpunten. Deze computer mag geen deel uit van de Azure Stack-systeem of gehost in de Azure Stack-cloud.

### <a name="machine-prerequisites"></a>Vereisten voor de machines

Controleer of uw computer voldoet aan de volgende criteria:

- Toegang tot alle Azure Stack-eindpunten
- .NET 4.6 en PowerShell 5.0 geïnstalleerd
- Ten minste 8 GB RAM-geheugen
- Minimaal 8-coreprocessors
- Minimaal 200 GB schijfruimte
- Stabiele netwerkverbinding met het internet

### <a name="download-and-install-the-agent"></a>De agent downloaden en installeren

1. Open Windows PowerShell in een opdrachtprompt op de computer die u gebruiken wilt voor de tests uitvoeren.
2. Voer de volgende opdracht voor het downloaden van de lokale agent:

    ```powershell
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "OnPremAgent.zip"
    Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent -Force
    Set-Location VaaSOnPremAgent\lib\net46
    ```

3. Voer de volgende opdracht voor het installeren van de lokale agent-afhankelijkheden:

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

    **Parameters**

    | Parameter | Description |
    | --- | --- |
    | aadServiceAdminUser | De globale beheerder voor uw Azure AD-tenant. Zo kan het zijn, vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | Het wachtwoord voor de globale beheerder. |
    | AadTenantId | Azure AD-tenant-ID voor het Azure-account is geregistreerd bij validatie uit als een Service. |
    | ExternalFqdn | U kunt de volledig gekwalificeerde domeinnaam van het configuratiebestand. Zie voor instructies, [werkstroom algemene parameters in de Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md). |
    | Regio | De regio van uw Azure AD-tenant. |

De opdracht downloadt een installatiekopie van de openbare opslagplaats (PIR)-installatiekopie (OS VHD) en een kopie van een Azure blob-opslag naar uw Azure Stack-opslag.

![Vereisten downloaden](media/installingprereqs.png)

> [!Note]
> Als u langzame netwerksnelheid ondervindt bij het downloaden van deze installatiekopieën, ze afzonderlijk te downloaden naar een lokale share en geef de parameter **- LocalPackagePath** *FileShareOrLocalPath*. U kunt meer hulp vinden op het PIR downloaden in de sectie [ingang langzame netwerkverbinding](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) van [validatie oplossen als een Service](azure-stack-vaas-troubleshoot.md).

## <a name="checks-before-starting-the-tests"></a>Controles voordat u begint met de tests

De tests zijn uitgevoerd externe bewerkingen. De machine waarop de tests moet toegang hebben tot de Azure Stack-eindpunten, anders de tests werkt niet. Als u van de lokale agent VaaS gebruikmaakt, gebruikt u de computer waar de agent wordt uitgevoerd. U kunt controleren of uw computer toegang tot de Azure Stack-eindpunten heeft door het uitvoeren van controles op de volgende:

1. Controleer of de basis-URI kan worden bereikt. Open een opdrachtprompt of bash-shell en voer de volgende opdracht, vervangt `<EXTERNALFQDN>` met de externe FQDN-naam van uw omgeving:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Open een webbrowser en Ga naar `https://adminportal.<EXTERNALFQDN>` om te controleren dat de MAS-Portal kan worden bereikt.

3. Meld u aan met de Azure AD-service administrator-waarden en het wachtwoord opgegeven tijdens het maken van de testronde.

4. Controleer de status van de van het systeem door het uitvoeren van de **Test AzureStack** PowerShell-cmdlet, zoals beschreven in [een validatietest uitvoeren voor Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test). Alle waarschuwingen en fouten oplossen voordat u een test start.

## <a name="run-the-agent"></a>De agent uitvoeren

1. Open Windows PowerShell in een opdrachtprompt.

2. Voer de volgende opdracht uit:

    ```powershell
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ```

      **Parameters**  

    | Parameter | Description |
    | --- | --- |
    | VaaSUserId | Gebruikers-ID gebruikt om aan te melden bij de VaaS Portal (bijvoorbeeld gebruikersnaam\@Contoso.com) |
    | VaaSTenantId | Azure AD-tenant-ID voor het Azure-account is geregistreerd bij validatie uit als een Service. |

    > [!Note]  
    > Wanneer u de agent uitvoert, de huidige werkmap moet de locatie van de uitvoerbare bestanden, taak-engine host **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

Als u eventuele fouten die niet ziet, is klikt u vervolgens de lokale agent voltooid. Bijvoorbeeld de volgende tekst wordt weergegeven in het consolevenster.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Gestarte agent](media/startedagent.png)

Een agent wordt uniek geïdentificeerd door de naam ervan. Standaard maakt het gebruik van de naam van de FQDN-naam (Fully Qualified Domain Name) van de machine vanaf waar deze werd gestart. U moet de minimaliseren om te voorkomen dat een per ongeluk wordt geselecteerd in het venster als de focus wordt onderbroken alle andere acties.

## <a name="next-steps"></a>Volgende stappen

- [Validatie as a Service oplossen](azure-stack-vaas-troubleshoot.md)
- [Validatie uit als een Service-belangrijkste concepten](azure-stack-vaas-key-concepts.md)
- [Snelstart: De validatie gebruiken als een serviceportal voor het plannen van uw eerste test](azure-stack-vaas-schedule-test-pass.md)