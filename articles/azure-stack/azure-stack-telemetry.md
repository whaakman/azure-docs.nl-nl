---
title: Azure Stack telemetrie | Microsoft Docs
description: Beschrijft hoe u configureert Azure Stack telemetrie-instellingen met behulp van PowerShell.
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
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: comartin
ms.openlocfilehash: ed3f09f942bdaa803ae8024d5c02230173190107
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248194"
---
# <a name="azure-stack-telemetry"></a>Azure Stack-telemetrie

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Systeemgegevens Azure Stack telemetrie automatisch naar Microsoft via de gebruikerservaring verbonden geüpload. Microsoft-teams gebruiken de gegevens die Azure-Stack telemetrie verzamelt voor een betere gebruikerservaringen. Deze gegevens worden ook gebruikt voor beveiliging, status, kwaliteit en analyse van prestaties.

Voor een Azure-Stack-operator telemetrie waardevolle inzichten in bedrijfsimplementaties kan bieden en biedt u een stem waarmee toekomstige versies van de vorm van Azure-Stack.

> [!NOTE]
> U kunt ook Azure Stack voor het doorsturen van gebruiksgegevens naar Azure voor facturering configureren. Dit is vereist voor meerdere knooppunten Azure Stack-klanten betalen-als-gebruik facturering kiezen. Rapportage over het gebruik van telemetrie onafhankelijk wordt beheerd en is niet vereist voor meerdere knooppunten klanten die kiezen voor het capaciteitsmodel of voor gebruikers van Azure Stack Development Kit. Voor deze scenario's, rapportage over het gebruik kan worden uitgeschakeld [met het registratiescript](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure Stack telemetrie is gebaseerd op het onderdeel Windows Server 2016 verbonden gebruikerservaring en telemetrie dat gebruikmaakt van de [Event Tracing voor Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) TraceLogging technologie voor het verzamelen en opslaan van gebeurtenissen en de gegevens. Azure Stack-onderdelen gebruikt dezelfde technologie voor het publiceren van gebeurtenissen en gegevens die worden verzameld met behulp van openbare besturingssysteem logboekregistratie en tracering API's. Voorbeelden van deze onderdelen Azure Stack zijn deze providers: netwerkbron, Opslagresource Resource bewaking en Update-Resource. Het onderdeel verbonden gebruikerservaring en telemetrie versleutelt gegevens met behulp van SSL en gebruikt vastmaken certificaat voor het verzenden van gegevens via HTTPS naar de Microsoft Data Management-service.

> [!IMPORTANT]
> Schakel telemetrie gegevensstroom door moet poort 443 (HTTPS) openen in uw netwerk. Het onderdeel verbonden gebruikerservaring en telemetrie verbindt met de Microsoft Data Management-service op https://v10.vortex-win.data.microsoft.com. Het onderdeel verbonden gebruikerservaring en telemetrie ook verbinding maakt met https://settings-win.data.microsoft.com voor het downloaden van configuratie-informatie.

## <a name="privacy-considerations"></a>Privacyoverwegingen

De ETW-service stuurt telemetrische gegevens terug naar de beveiligde cloud-opslag. Het principe van minimale bevoegdheden begeleidt toegang tot telemetriegegevens. Alleen medewerkers van Microsoft met een geldige bedrijfsbehoefte toegang krijgen tot de telemetriegegevens. Microsoft geen persoonlijke klantgegevens met derden, deelt, met uitzondering van de klant goeddunken of voor het beperkte doel dat wordt beschreven in de [privacyverklaring van Microsoft](https://privacy.microsoft.com/PrivacyStatement). Zakelijke rapporten die worden gedeeld met de OEM's en partners bevatten verzamelde, geanonimiseerde gegevens. Beslissingen delen van gegevens worden gemaakt door een interne Microsoft-team, met inbegrip van privacy, juridische en data management belanghebbenden.

Microsoft gelooft en procedures, afhankelijk van de informatie. We willen verzamelen alleen de informatie die nodig, en opslaan voor alleen zolang als nodig om een service te bieden of voor analyse. Veel van de informatie over hoe de Azure-Stack-systeem en de Azure-services werken wordt binnen zes maanden verwijderd. Samengevat of cumulatieve gegevens voor een langere periode worden behouden.

We begrijpen dat de privacy en beveiliging van klantgegevens belangrijk is.  Microsoft gaat een benadering attente en uitgebreid naar de privacy van klanten en de bescherming van gegevens van de klant in Azure-Stack. IT-beheerders hebben besturingselementen voor het aanpassen van functies en privacy-instellingen op elk gewenst moment. Onze inzet voor transparantie en een vertrouwensrelatie is uitgeschakeld:

- We openen met klanten over de typen gegevens die we verzamelen.
- We enterprise-klanten plaatsen in besturingselement: zij hun eigen privacy-instellingen kunnen aanpassen.
- We plaatsen klant privacy en beveiliging eerst.
- We transparante over het gebruik telemetrische gegevens opgehaald.
- We gebruiken telemetriegegevens gebruikerservaringen verbeteren.

Microsoft biedt geen plan voor het verzamelen van gevoelige gegevens, zoals creditcardnummers, gebruikersnamen en wachtwoorden, e-mailadressen of vergelijkbare gevoelige informatie. Als we bepalen dat gevoelige informatie per ongeluk is ontvangen, wordt deze verwijderd.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Voorbeelden van hoe de telemetriegegevens maakt gebruik van Microsoft

Telemetrie speelt een belangrijke rol bij het snel vaststellen en oplossen van kritieke betrouwbaarheidsproblemen in implementaties van klanten en configuraties. Inzicht in telemetriegegevens kunt identificeren van problemen met services of hardwareconfiguraties. De mogelijkheid van Microsoft om deze gegevens van klanten en verbeteringen van de schijf met het ecosysteem wordt gegeven van de balk voor de kwaliteit van de geïntegreerde Azure Stack-oplossingen.

Telemetrie kunt u ook Microsoft beter inzicht in hoe de onderdelen voor het implementeren van klanten, gebruikmaken van de functies en services gebruiken om hun zakelijke doelstellingen te realiseren. Deze inzichten te prioriteren van technische investeringen in de gebieden die direct gebruikerservaringen en werkbelastingen gevolgen kunnen.

Voorbeelden zijn het gebruik van de klant van containers, opslag- en netwerkconfiguraties die gekoppeld aan Azure-Stack-rollen zijn. We gebruiken ook de inzichten station verbeteringen en intelligence in Azure Stack-beheer en controle van oplossingen. Deze verbeteringen maken het gemakkelijker voor klanten om problemen te diagnosticeren en geld doordat minder ondersteuning besparen aanroepen naar Microsoft.

## <a name="manage-telemetry-collection"></a>Verzameling van telemetrie beheren

Wordt niet aanbevolen het uitschakelen van telemetrie in uw organisatie. Echter, in sommige scenario's dit kan nodig zijn.

In deze scenario's, kunt u het niveau van de telemetrie naar Microsoft verzonden met behulp van registerinstellingen voordat u Azure-Stack implementeert of met behulp van de telemetrie-eindpunten nadat u Azure-Stack implementeert.

### <a name="telemetry-levels-and-data-collection"></a>Telemetrie-niveaus en gegevensverzameling

Voordat u de telemetrie-instellingen wijzigt, moet u de telemetrie-niveaus en welke gegevens worden verzameld op elk niveau begrijpen.

De telemetrie-instellingen zijn gegroepeerd in vier niveaus (0-3) die cumulatieve en gecategoriseerd als de wordt gevolgd zijn:

**0 (beveiliging)**</br>
Beveiligingsgegevens. Gegevens die vereist zijn om het besturingssysteem te beveiligen. Dit omvat gegevens over de instellingen van het onderdeel verbonden gebruikerservaring en Telemetrie en Windows Defender. Er is geen telemetrie die specifiek zijn voor Azure-Stack is verzonden op dit niveau.

**1 (basis)**</br>
Beveiligingsgegevens en Basisstatus en -kwaliteit gegevens. Basic apparaatgegevens, met inbegrip van: gegevens met betrekking tot de kwaliteit, app-compatibiliteit, app-gebruiksgegevens, en de gegevens uit de **beveiliging** niveau. Uw telemetrie-niveau wordt ingesteld op Basic schakelt Azure Stack telemetrie. De gegevens die worden verzameld op dit niveau omvat:

- *Basic apparaatgegevens* die zorgt voor een goed begrip over de typen en configuraties van systeemeigen en virtuele Windows Server 2016-exemplaren in het ecosysteem. Dit omvat:

  - Machine kenmerken, zoals de OEM en het model.
  - Kenmerken, netwerken, zoals het aantal netwerkadapters en de snelheid.
  - Kenmerken processor en geheugen, zoals het aantal kernen en de hoeveelheid geheugen is geïnstalleerd.
  - Opslagkenmerken, zoals het aantal stations, type station en de grootte van de schijf.

- *Telemetrie-functionaliteit*, met inbegrip van de hoeveelheid geüploade gebeurtenissen, Verwijderde gebeurtenissen en de meest recente gegevens uploaden tijd.
- *Informatie met betrekking tot kwaliteit* die helpt Microsoft bij het ontwikkelen van een basiskennis van hoe Azure-Stack is uitvoert. Bijvoorbeeld: de telling van kritieke waarschuwingen op een specifieke hardwareconfiguratie.
- *Compatibiliteitsgegevens* die zorgt voor een goed begrip over die Resource Providers worden geïnstalleerd op een systeem- en een virtuele machine. Hiermee wordt de mogelijke compatibiliteitsproblemen geïdentificeerd.

**2 (uitgebreid)**</br>
Als u meer inzicht, waaronder: hoe het besturingssysteem en de Azure-Stack-services worden gebruikt, hoe deze services uitvoeren, geavanceerde betrouwbaarheid gegevens en gegevens van de **beveiliging** en **Basic** niveaus.

> [!NOTE]
> Dit is de standaardinstelling voor telemetrie.

**3 (volledig)**</br>
Alle gegevens die nodig zijn om te bepalen en te helpen bij het oplossen van problemen, plus gegevens uit de **beveiliging**, **Basic**, en **uitgebreid** niveaus.

> [!IMPORTANT]
> Deze niveaus telemetrie zijn alleen van toepassing op Microsoft Azure-Stack-onderdelen. Niet-Microsoft-software-onderdelen en services die worden uitgevoerd in de Host van de levenscyclus van Hardware van Azure-Stack hardwarepartners kunnen communiceren met hun cloud-services buiten deze niveaus telemetrie. U moet samenwerken met uw Azure-Stack hardware solution provider over hun beleid Telemetrie en hoe u kunt aanmelden of opt-out.

Windows- en Azure Stack telemetrie uitschakelen, schakelt ook SQL telemetrie. Zie voor meer informatie over de gevolgen van de telemetrie-instellingen voor Windows Server, de [Windows telemetrie technisch document](https://aka.ms/winservtelemetry).

### <a name="asdk-set-the-telemetry-level-in-the-windows-registry"></a>ASDK: de telemetrie-niveau instellen in het Windows-register

De Windows-register-Editor kunt u handmatig de telemetrie-niveau instellen op de fysieke computer voordat u Azure-Stack implementeert. Als een management-beleid al, zoals Groepsbeleid bestaat, wordt deze registerinstelling overschreven.

Voordat u Azure-Stack implementeert op de host van de kit ontwikkeling, start CloudBuilder.vhdx op en voer het volgende script in een PowerShell-venster met verhoogde bevoegdheid:

```powershell
### Get current AllowTelmetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0' # Set this value to 0,1,2,or3.  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

### <a name="asdk-and-multi-node-enable-or-disable-telemetry-after-deployment"></a>ASDK en met meerdere knooppunten: in- of uitschakelen van telemetrie na implementatie

Als u wilt in- of uitschakelen van telemetrie na de implementatie, moet u aan de bevoorrechte eindpunt (PEP) die wordt weergegeven op de ERCS virtuele machines toegang hebben.

1. Om in te schakelen: `Set-Telemetry -Enable`
2. Uitschakelen: `Set-Telemetry -Disable`

Parameterdetails van de:
> . PARAMETER Enable - inschakelen telemetrie uploaden van gegevens</br>
> . Uitschakelen van de PARAMETER - uitschakelen telemetrie uploaden van gegevens  

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

- [Het implementatiepakket Azure Stack development kit downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

- [Azure-Stack development kit implementeren](azure-stack-run-powershell-script.md)
