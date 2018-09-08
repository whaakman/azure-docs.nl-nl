---
title: De lokale agent implementeren en testen van de installatiekopie van virtuele machines in Azure Stack-validatie als een service | Microsoft Docs
description: De lokale agent implementeren en testen van de installatiekopie van virtuele machines voor Azure Stack-validatie als een service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 78136ab00dcba2f8a99df36ba99d384b49995882
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159211"
---
# <a name="deploy-the-local-agent-and-test-virtual-machines"></a>De lokale agent en test virtuele machine implementeren

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Meer informatie over het gebruik van de validatie als een lokale agent service (VaaS) om te controleren of uw hardware. De lokale agent moet worden geïmplementeerd op de Azure Stack-oplossing vóór het uitvoeren van de validatietests worden gevalideerd.

> [!Note]  
> U moet ervoor zorgen dat de computer waarop de lokale agent wordt uitgevoerd, heeft geen toegang meer tot het Internet. De machine moet alleen toegankelijk zijn voor gebruikers die zijn gemachtigd voor het gebruik van Azure Stack VaaS.

Voor het testen van uw virtuele machines:

1. De lokale agent installeren
2. Fouten in uw systeem invoeren
3. De lokale agent uitvoeren

## <a name="download-and-start-the-local-agent"></a>Downloaden en de lokale agent starten

Download de agent op een computer die voldoet aan de vereisten in uw datacenter die geen deel uitmaakt van het Azure Stack-systeem, maar dat toegang heeft tot alle Azure Stack-eindpunten.

### <a name="machine-prerequisites"></a>Vereisten voor de machines

Controleer of uw computer voldoet aan de volgende criteria:

- Toegang tot alle Azure Stack-eindpunten
- .NET 4.6 en PowerShell 5.0 geïnstalleerd
- Ten minste 8 GB RAM-geheugen
- Minimaal 8-coreprocessors
- Minimaal 200 GB schijfruimte
- Stabiele netwerkverbinding met het internet

Azure Stack wordt het systeem onder testen. De computer mag geen deel uit van Azure Stack of gehost in de Azure Stack-cloud.

### <a name="download-and-install-the-agent"></a>De agent downloaden en installeren

1. Open Windows PowerShell in een opdrachtprompt op de computer die u gebruiken wilt voor de tests uitvoeren.
2. Voer de volgende opdracht voor het downloaden van de lokale agent:

    ```PowerShell  
        Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.3.2.0.nupkg" -outfile "OnPremAgent.zip"
        Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent.3.2.0 -Force
        Set-Location VaaSOnPremAgent.3.2.0\lib\net46
    ````

3. Voer de volgende opdracht voor het installeren van de lokale agent-afhankelijkheden:

    ```PowerShell  
        $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-VaaSPrerequisites -AadTenantId <AadTenantId> `
        -ServiceAdminCreds <ServiceAdminCreds> `
        -ArmEndpoint https://adminmanagement.<ExternalFqdn> `
        -Region <Region>
    ````

    **Parameters**

    | Parameter | Beschrijving |
    | --- | --- |
    | aadServiceAdminUser | De globale beheerder voor uw Azure AD-tenant. Zo kan het zijn, vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | Het wachtwoord voor de globale beheerder. |
    | AadTenantId | Azure AD-tenant-ID voor het Azure-account is geregistreerd bij validatie uit als een Service. |
    | ExternalFqdn | U kunt de volledig gekwalificeerde domeinnaam van het configuratiebestand. Zie voor instructies, [Webtestparameters voor validatie als een service van Azure Stack](azure-stack-vaas-parameters-test.md). |
    | Regio | De regio van uw Azure AD-tenant. |

De opdracht downloadt een installatiekopie van de openbare opslagplaats (PIR)-installatiekopie (OS VHD) en een kopie van een Azure blob-opslag naar uw Azure Stack-opslag. 

![Vereisten downloaden](media/installingprereqs.png)

> [!Note]  
> Als u langzame netwerksnelheid ondervindt bij het downloaden van deze installatiekopieën, ze afzonderlijk te downloaden naar een lokale share en geef de parameter **- LocalPackagePath** *FileShareOrLocalPath*. U kunt meer hulp vinden op het PIR downloaden in de sectie [ingang langzame netwerkverbinding](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) van [validatie uit als een service oplossen](azure-stack-vaas-troubleshoot.md).

## <a name="fault-injection"></a>Foutinjectie

Microsoft heeft Azure Stack ontworpen voor flexibiliteit en te tolereren meerdere typen van software en hardware-fouten. Foutinjectie verhoogt het aantal fouten in het systeem. Deze toename helpt u bij problemen eerder onthullen zodat u kunt Verminder het aantal incidenten die het systeem laat uitvallen.

Voer de volgende opdrachten voor het invoeren van fouten in uw systeem.

1. Open Windows PowerShell in een opdrachtprompt.

2. Voer de volgende opdracht uit:

    ```PowerShell  
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-ServiceFabricSDK Install-ServiceFabricSDK
    ```

## <a name="run-the-agent"></a>De agent uitvoeren

1. Open Windows PowerShell in een opdrachtprompt.

2. Voer de volgende opdracht uit:

    ````PowerShell  
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Parameters**  
    
    | Parameter | Beschrijving |
    | --- | --- |
    | VaaSUserId | Gebruikers-ID gebruikt om aan te melden bij de VaaS Portal (bijvoorbeeld UserName@Contoso.com) |
    | VaaSTenantId | Azure AD-tenant-ID voor het Azure-account is geregistreerd bij validatie uit als een Service. |

    > [!Note]  
    > Wanneer u de agent uitvoert, de huidige werkmap moet de locatie van de uitvoerbare bestanden, taak-engine host **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

Als u eventuele fouten die niet ziet, is klikt u vervolgens de lokale agent voltooid. Bijvoorbeeld de volgende tekst wordt weergegeven in het consolevenster.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Gestarte agent](media/startedagent.png)

Een agent wordt uniek geïdentificeerd door de naam ervan. Standaard maakt het gebruik van de naam van de FQDN-naam (Fully Qualified Domain Name) van de machine vanaf waar deze werd gestart. U moet de minimaliseren om te voorkomen dat een per ongeluk klikt op het venster als de focus wordt onderbroken alle andere acties.

## <a name="next-steps"></a>Volgende stappen

- [Een nieuwe Azure Stack-oplossing te valideren](azure-stack-vaas-validate-solution-new.md)  
- Als u traag of onregelmatige verbinding met Internet hebt, kunt u de installatiekopie van het PIR downloaden. Zie voor meer informatie, [ingang langzame netwerkverbinding](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity).
- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
