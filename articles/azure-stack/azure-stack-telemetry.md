---
title: Telemetrie van Azure Stack | Microsoft Docs
description: Beschrijft hoe u Azure Stack met behulp van PowerShell telemetrie-instellingen configureert.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: comartin
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 873fc5f6dd4647f032a40ec2aa3c4527671cd92c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249803"
---
# <a name="azure-stack-telemetry"></a>Azure Stack-telemetrie

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack-telemetrie wordt automatisch systeemgegevens geüpload naar Microsoft via de gebruikerservaring verbonden. Microsoft-teams gebruiken de gegevens die door Azure Stack telemetrie wordt verzameld voor het verbeteren van klantervaring. Deze gegevens wordt ook gebruikt voor beveiliging, status, kwaliteit en analyse van prestaties.

Telemetrie-voor Azure Stack-operators, krijgt u inzicht in de enterprise-implementaties en biedt u een stem waarmee toekomstige versies van de vorm van Azure Stack.

> [!NOTE]
> U kunt ook Azure Stack voor het doorsturen van gebruiksgegevens naar Azure voor facturering. Dit is vereist voor meerdere knooppunten Azure Stack-klanten die betalen als u-gebruik facturering. Rapportage over het gebruik van de telemetriegegevens onafhankelijk van elkaar wordt beheerd en is niet vereist zijn voor klanten met meerdere knooppunten die het capaciteitsmodel of voor gebruikers van Azure Stack Development Kit. Voor deze scenario's, rapportage over het gebruik kan worden uitgeschakeld [met behulp van het script voor de registratie](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure Stack-telemetrie is gebaseerd op de verbonden gebruikerservaring van Windows Server 2016 en telemetrie-onderdeel dat gebruikmaakt van de [Event Tracing voor Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) TraceLogging technologie voor het verzamelen en opslaan van gegevens over gebeurtenissen en. Azure Stack-onderdelen gebruikt dezelfde technologie voor het publiceren van gebeurtenissen en gegevens die worden verzameld met behulp van openbare besturingssysteem logboekregistratie en tracering van API's. Voorbeelden van deze Azure Stack-onderdelen zijn deze providers: Het netwerk van Resource, Opslagresource, bewaking Resource en Resource bijwerken. Het onderdeel gebruikerservaring verbonden en telemetrie versleutelt gegevens met behulp van SSL en maakt gebruik van certificaten vast te maken voor het verzenden van gegevens via HTTPS naar de Microsoft Data Management-service.

> [!IMPORTANT]
> Om in te schakelen telemetriegegevensstroom, moet poort 443 (HTTPS) openen in uw netwerk. Het onderdeel gebruikerservaring verbonden en telemetrie maakt verbinding met de Microsoft Data Management-service op https://v10.vortex-win.data.microsoft.com. Het onderdeel gebruikerservaring verbonden en telemetrie ook verbinding maakt met https://settings-win.data.microsoft.com voor het downloaden van configuratie-informatie.

## <a name="privacy-considerations"></a>Privacy-overwegingen

De ETW-service stuurt telemetrische gegevens terug naar een beveiligde cloud-opslag. De principal van minimale bevoegdheden ligt door toegang tot de telemetrische gegevens. Alleen Microsoft-personeel met een geldige zakelijke behoeften toegang krijgen tot de telemetrische gegevens. Microsoft biedt geen persoonlijke klantgegevens met derden, delen, met uitzondering van de klant goeddunken of voor de beperkte toepassing beschreven in de [privacyverklaring van Microsoft](https://privacy.microsoft.com/PrivacyStatement). Zakelijke rapporten die worden gedeeld met OEM's en partners bevatten verzamelde, geanonimiseerde gegevens. Beslissingen voor het delen van gegevens worden gemaakt door een interne Microsoft-team, met inbegrip van privacy, juridische en management belanghebbenden.

Microsoft streeft ernaar en informatie tot procedures. We streven ernaar om het verzamelen van alleen de informatie die nodig, en op te slaan voor alleen zo lang die nodig is voor het leveren van een service of voor analyse. Veel van de informatie over hoe de Azure Stack-systeem en de Azure-services werken binnen zes maanden verwijderd. Samengevatte of geaggregeerde gegevens worden bewaard voor een langere periode.

We begrijpen dat de privacy en beveiliging van informatie over de klant is belangrijk.  Microsoft hanteert een doordachte en uitgebreide benadering privacy van klanten en de bescherming van gegevens van de klant in Azure Stack. IT-beheerders hebben besturingselementen voor het aanpassen van functies en privacy-instellingen op elk gewenst moment. Onze toewijding aan transparantie en vertrouwen is duidelijk:

- We openen met klanten over de typen gegevens die we verzamelen.
- We plaatsen enterprise-klanten in besturingselement, kunnen ze hun eigen privacy-instellingen aanpassen.
- We plaats privacy van klanten en beveiliging eerst.
- We transparante over hoe telemetrische gegevens wordt gebruikt.
- We telemetriegegevens gebruikt om betere klantervaringen.

Microsoft niet van plan bent voor het verzamelen van gevoelige gegevens, zoals creditcardnummers, gebruikersnamen en wachtwoorden, e-mailadressen of vergelijkbare gevoelige informatie. Als we ontdekken dat gevoelige informatie per ongeluk is ontvangen, wordt het verwijderen.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Voorbeelden van hoe Microsoft de telemetriegegevens die zijn gebruikt

Telemetrie speelt een belangrijke rol bij het helpen om snel te identificeren en oplossen van problemen voor betrouwbaarheid in implementaties van klanten en configuraties. Inzichten op basis van telemetrische gegevens kunt identificeren van problemen met de services of hardwareconfiguraties. Microsoft kan deze gegevens ophalen van klanten en station verbeteringen aan het ecosysteem, genereert de balk voor de kwaliteit van de geïntegreerde Azure Stack-oplossingen.

Telemetrie helpt ook bij Microsoft om beter te begrijpen hoe de onderdelen voor het implementeren van klanten, gebruik van functies en services gebruiken om hun zakelijke doelstellingen te realiseren. Deze inzichten kunnen prioriteiten op de belangrijkste gebieden direct klantervaringen en werkbelastingen gevolgen kunnen.

Enkele voorbeelden zijn het gebruik van de klant van containers, opslag- en netwerkconfiguraties die gekoppeld aan Azure Stack-rollen zijn. We gebruiken ook de inzichten station verbeteringen en intelligentie in Azure Stack-beheer en controle van oplossingen. Deze verbeteringen maken het gemakkelijker voor klanten om problemen te diagnosticeren en geld door minder ondersteuning kunnen besparen aanroepen naar Microsoft.

## <a name="manage-telemetry-collection"></a>Verzamelen van telemetriegegevens beheren

We niet aanbevolen het uitschakelen van telemetrie in uw organisatie. Echter, in sommige scenario's dit kan nodig zijn.

In deze scenario's kunt u het telemetrieniveau van de naar Microsoft verzonden met behulp van de instellingen in het register voordat u implementeert in Azure Stack, of met behulp van de telemetrie-eindpunten, nadat u Azure Stack implementeren configureren.

### <a name="telemetry-levels-and-data-collection"></a>Telemetrie-niveaus en gegevensverzameling

Voordat u telemetrie-instellingen wijzigt, moet u de telemetrie-niveaus en welke gegevens worden verzameld op elk niveau begrijpen.

De telemetrie-instellingen zijn gegroepeerd in vier niveaus (0-3) die cumulatief en gecategoriseerd als de volgende manier zijn:

**0 (beveiliging)**</br>
Alleen beveiligingsgegevens. De informatie die vereist zijn ter beveiliging van het besturingssysteem. Dit omvat gegevens over de instellingen voor gebruikerservaring verbonden en telemetrie-onderdeel en de Windows Defender. Er is geen specifiek voor Azure Stack telemetrie wordt verzonden op dit niveau.

**1 (Basic)**</br>
Beveiligingsgegevens, en Basisbewaking van statussen en kwaliteit. Basic apparaatgegevens, met inbegrip van: gegevens met betrekking tot kwaliteit, app-compatibiliteit, basisgegevens, en de gegevens uit de **Security** niveau. Uw telemetrieniveau instellen op Basic kunnen Azure Stack telemetrie. De gegevens die zijn verzameld op dit niveau bevat:

- *Basic apparaatgegevens* die zorgt voor een goed begrip over de typen en -configuraties van systeemeigen en virtuele Windows Server 2016-instanties in het ecosysteem. Dit omvat:

  - Machine-kenmerken, zoals de OEM, en het model.
  - Kenmerken, zoals het aantal netwerkadapters en de snelheid van toegang.
  - Processor en geheugen kenmerken, zoals het aantal kernen en de hoeveelheid geheugen die is geïnstalleerd.
  - Opslagkenmerken, zoals het aantal schijven, type station en grootte van de schijf.

- *Telemetrie-functionaliteit*, met inbegrip van de hoeveelheid geüploade gebeurtenissen, Verwijderde gebeurtenissen en de meest recente gegevens uploadtijd.
- *Informatie met betrekking tot kwaliteit* die helpt Microsoft bij het ontwikkelen van een basiskennis hebt van hoe Azure Stack wordt uitgevoerd. Bijvoorbeeld, het aantal kritieke waarschuwingen op een specifieke hardwareconfiguratie.
- *Compatibiliteit* die zorgt voor een goed begrip over welke Resource-Providers geïnstalleerd op een systeem- en een virtuele machine. Hiermee wordt aangegeven voor mogelijke compatibiliteitsproblemen.

**2 (Enhanced)**</br>
Extra inzichten, met inbegrip van: hoe het besturingssysteem en Azure Stack-services worden gebruikt, hoe deze services uitvoeren, geavanceerde betrouwbaarheid gegevens en gegevens van de **Security** en **Basic** niveaus.

> [!NOTE]
> Dit is de standaardinstelling voor telemetrie.

**3 (volledig)**</br>
Alle gegevens die nodig zijn om te identificeren en te helpen problemen op te lossen, plus gegevens uit de **Security**, **Basic**, en **uitgebreid** niveaus.

> [!IMPORTANT]
> Deze niveaus telemetrie zijn alleen van toepassing op Microsoft Azure Stack-onderdelen. Niet-Microsoft-software-onderdelen en services die worden uitgevoerd in de Host van de levenscyclus van Hardware van Azure Stack-hardware-partners kunnen communiceren met hun cloud-services buiten deze telemetrie-niveaus. U moet werken met uw Azure Stack-solution-provider voor hardware om te begrijpen van hun beleid telemetrie, en hoe u kunt ervoor kiezen of opt-out.

Het uitschakelen van Windows-en Azure Stack, schakelt ook SQL telemetrie. Zie voor meer informatie over de gevolgen van de telemetrie-instellingen voor Windows Server, de [technisch document Windows-telemetrie](https://aka.ms/winservtelemetry).

### <a name="asdk-set-the-telemetry-level-in-the-windows-registry"></a>ASDK: Stel het telemetrieniveau van de in de Windows-register

De Windows-register-Editor kunt u het telemetrieniveau van de handmatig instellen op de fysieke hostcomputer voordat u Azure Stack implementeren. Als een management-beleid al, zoals Groepsbeleid bestaat, overschrijft deze instelling in het register.

Voordat u implementeert Azure Stack op de host van development kit, opstarten naar CloudBuilder.vhdx en voer het volgende script in een PowerShell-venster met verhoogde bevoegdheid:

```powershell
### Get current AllowTelemetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0' # Set this value to 0,1,2,or3.  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

### <a name="asdk-and-multi-node-enable-or-disable-telemetry-after-deployment"></a>ASDK en meerdere knooppunten: in- of uitschakelen telemetrie na de implementatie

Als u wilt in- of uitschakelen telemetrie na de implementatie, moet u beschikken over naar de bevoegde eindpunt (PEP) die wordt weergegeven op de ERCS virtuele machines.

1. Om in te schakelen: `Set-Telemetry -Enable`
2. Om uit te schakelen: `Set-Telemetry -Disable`

Details van de PARAMETER:
> . Inschakelen van de PARAMETER - inschakelen telemetrie gegevens uploaden</br>
> . Uitschakelen van de PARAMETER - uitschakelen telemetrie gegevens uploaden  

**Script telemetrie inschakelen:**

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Enable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

**Script voor het uitschakelen van telemetrie:**

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Disable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

## <a name="next-steps"></a>Volgende stappen

[Azure Stack registreren bij Azure](azure-stack-registration.md)