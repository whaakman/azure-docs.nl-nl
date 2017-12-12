---
title: Azure Stack telemetrie | Microsoft Docs
description: Beschrijft hoe u configureert Azure Stack telemetrie-instellingen met behulp van PowerShell.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/08/2017
ms.author: jeffgilb
ms.reviewer: comartin
ms.openlocfilehash: 5cd8d4045764b753c5fdd81ade98d69c72709881
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-stack-telemetry"></a>Azure Stack-telemetrie

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack systeemgegevens of telemetrie, wordt automatisch naar Microsoft via de gebruikerservaring verbonden geüpload. Gegevens die worden verzameld van de Azure-Stack telemetriegegevens wordt gebruikt door Microsoft-teams voornamelijk voor het verbeteren van onze gebruikerservaringen en voor analyse van beveiliging, status, kwaliteit en prestaties.

Als operator Azure Stack telemetrie waardevolle inzichten in bedrijfsimplementaties kan bieden en biedt u een stem waarmee toekomstige versies van de vorm van Azure-Stack.

> [!NOTE]
> Azure-Stack kan ook worden geconfigureerd voor informatie over het gebruik van doorsturen naar Azure voor facturering. Dit is vereist voor meerdere knooppunten Azure Stack-klanten betalen-als-gebruik facturering kiezen. Rapportage over het gebruik van telemetrie onafhankelijk wordt beheerd en is niet vereist voor meerdere knooppunten klanten die kiezen voor het capaciteitsmodel of voor gebruikers van Azure Stack Development Kit. Voor deze scenario's, rapportage over het gebruik kan worden uitgeschakeld [met het registratiescript](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting). 

Azure Stack telemetrie is gebaseerd op het onderdeel Windows Server 2016 verbonden gebruikerservaring en telemetrie dat gebruikmaakt van de [Event Tracing voor Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) trace-technologie van logboekregistratie om te verzamelen en telemetrische gebeurtenissen en gegevens opslaan. Azure Stack-onderdelen gebruiken dezelfde logboekregistratie technologie voor het publiceren van gebeurtenissen en de gegevens die zijn verzameld met behulp van openbare besturingssysteem logboekregistratie en tracering van API's. Voorbeelden van Azure-Stack-onderdelen zijn Netwerkresourceprovider, Storage Resource Provider, Resource Provider bewaking en Update Resource Provider. Het onderdeel verbonden gebruikerservaring en telemetrie versleutelt gegevens met behulp van SSL en gebruikt vastmaken certificaat voor het verzenden van telemetriegegevens via HTTPS naar de Microsoft Data Management-service.

> [!NOTE]
> Ter ondersteuning van de gegevensstroom telemetrie moet poort 443 (HTTPS) openen in uw netwerk. Het onderdeel verbonden gebruikerservaring en telemetrie verbindt met de Microsoft Data Management-service op https://v10.vortex-win.data.microsoft.com. Het onderdeel verbonden gebruikerservaring en telemetrie is ook maakt verbinding met https://settings-win.data.microsoft.com voor het downloaden van configuratie-informatie.

## <a name="privacy-considerations"></a>Privacyoverwegingen
De ETW-service stuurt telemetrische gegevens terug naar de beveiligde cloud-opslag. Het principe van laagst handleidingen toegang tot telemetriegegevens. Alleen medewerkers van Microsoft met een geldige zakelijke behoeften hebben toegang tot de telemetriegegevens. Microsoft deelt niet persoonlijke gegevens van onze klanten met derden, met uitzondering van de klant goeddunken of voor het beperkte doel dat wordt beschreven in de [Azure Stack-privacyverklaring](http://windows.microsoft.com/windows/preview-privacy-statement). We zakelijke rapporten delen met de OEM's en partners met wie telemetrie verzamelde, geanonimiseerde gegevens bevatten. Beslissingen delen van gegevens worden gemaakt door een interne Microsoft-team, met inbegrip van privacy, juridische en data management belanghebbenden.

Microsoft gelooft en procedures, afhankelijk van de informatie. We willen verzamelen alleen de informatie die we nodig hebt, en we deze alleen zolang deze nodig is om een service te bieden of voor analyse opslaan. Veel van de informatie over hoe de Azure-Stack-systeem en de Azure-services werken wordt binnen zes maanden verwijderd. Samengevat of cumulatieve gegevens voor een langere periode worden behouden.

We begrijpen dat de privacy en beveiliging van gegevens voor onze klanten is erg belangrijk.  Privacy van klanten en de bescherming van gegevens van de klant met de Azure-Stack hebben we een benadering attente en uitgebreide genomen. IT-beheerders hebben besturingselementen voor het aanpassen van functies en privacy-instellingen op elk gewenst moment. Onze inzet voor transparantie en een vertrouwensrelatie is uitgeschakeld:
- We kunnen openen met klanten over de typen gegevens die we verzamelen.
- We enterprise-klanten plaatsen in besturingselement: zij hun eigen privacy-instellingen kunnen aanpassen.
- We plaatsen klant privacy en beveiliging eerst.
- We zijn transparante over het gebruik telemetrie opgehaald.
- We gebruiken telemetrie gebruikerservaringen verbeteren.

Microsoft is niet van plan voor het verzamelen van vertrouwelijke gegevens, zoals creditcardnummers, gebruikersnamen en wachtwoorden, e-mailadressen of andere manier vertrouwelijke informatie. Als we bepalen dat gevoelige informatie per ongeluk is ontvangen, wordt deze verwijderd.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Voorbeelden van hoe de telemetriegegevens maakt gebruik van Microsoft
Telemetrie speelt een belangrijke rol bij je hulp bij het snel opsporen en oplossen van kritieke betrouwbaarheidsproblemen in implementaties en configuraties van onze klanten. Inzicht in de telemetriegegevens die we verzamelen helpt ons snel problemen met services of hardwareconfiguraties. De mogelijkheid van Microsoft om deze gegevens van klanten en verbeteringen in het ecosysteem station helpt verhogen van de balk voor de kwaliteit van onze geïntegreerde Azure Stack-oplossingen. 

Telemetrie kunt u ook Microsoft beter inzicht in hoe de onderdelen voor het implementeren van klanten, gebruikmaken van de functies en services gebruiken om hun zakelijke doelstellingen te realiseren. Inzichten verkrijgen van die gegevens helpt ons prioriteren van onze technische investeringen in de gebieden die direct voor onze klanten ervaringen en werkbelastingen gevolgen kunnen.

Voorbeelden zijn gebruik van de klant van containers, opslag- en netwerkconfiguraties die gekoppeld aan Azure-Stack-rollen zijn. We gebruiken ook de inzichten station verbeteringen en intelligence in enkele van onze beheer en controle van oplossingen.  Dit helpt klanten om kwaliteitsproblemen te diagnosticeren en geld doordat minder ondersteuning besparen aanroepen naar Microsoft.

## <a name="manage-telemetry-collection"></a>Verzameling van telemetrie beheren
We raden niet dat u telemetrie in uw organisatie uitschakelen zoals telemetrie bevat gegevens die de functionaliteit van het verbeterde product en stabiliteit stations. We echter erkennen dat in sommige scenario's dit kan nodig zijn. 

In dergelijke gevallen kunt kunt u het niveau van de telemetrie naar Microsoft verzonden met behulp van Register instellingen voorafgaand aan de implementatie of met de telemetrie-eindpunten post-implementatie configureren.

### <a name="asdk-set-telemetry-level-in-the-windows-registry"></a>ASDK: telemetrie niveau instellen in het Windows-register
De Register-Editor van Windows wordt gebruikt om handmatig de telemetrie-niveau instellen op de fysieke computer voordat u Azure-Stack implementeert. Als een management-beleid al, zoals Groepsbeleid bestaat, overschrijft deze registerinstelling. 

Voordat u Azure-Stack implementeert op de host van de kit ontwikkeling, start de CloudBuilder.vhdx op en voer het volgende script in een PowerShell-venster met verhoogde bevoegdheid:

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

De telemetrie-niveaus zijn cumulatieve en gecategoriseerd in vier niveaus (0-3):

**0 (beveiliging)**. Beveiligingsgegevens. Gegevens die vereist zijn om te voorkomen dat het besturingssysteem beveiligd, met inbegrip van gegevens over de instellingen van het onderdeel verbonden gebruikerservaring en Telemetrie en Windows Defender. Er is geen specifieke telemetrie van Azure-Stack is verzonden op dit niveau.

**1 (basis)**. Beveiligingsgegevens en Basisstatus en -kwaliteit gegevens. Basic apparaatgegevens, met inbegrip van: gegevens met betrekking tot de kwaliteit, app-compatibiliteit, app-gebruiksgegevens en gegevens van het beveiligingsniveau. Uw telemetrie-niveau wordt ingesteld op Basic schakelt Azure Stack telemetrie. De gegevens die worden verzameld op dit niveau omvat:

- **Basic apparaatgegevens** die zorgt voor een goed begrip over de typen en configuraties van systeemeigen als gevirtualiseerde Windows Server 2016-exemplaren in het ecosysteem, waaronder:
 - Machine kenmerken, zoals de OEM-model 
 - Kenmerken, netwerken, zoals het aantal en de snelheid van netwerkadapters,
 - Processor en geheugenkenmerken, zoals het aantal kernen, geheugengrootte 
 - Opslagkenmerken, zoals het aantal stations, type en de grootte.
- **Telemetrie-functionaliteit**, waaronder procent van het geüploade gebeurtenissen, Verwijderde gebeurtenissen en de laatste keer uploaden.
- **Quality-gerelateerde informatie** die helpt Microsoft bij het ontwikkelen van een basiskennis van hoe Azure-Stack is uitvoert. Een voorbeeld is het aantal kritieke waarschuwingen op een specifieke hardwareconfiguratie.
- **Compatibiliteitsgegevens** waarmee bieden een goed begrip over welke Resourceproviders zijn geïnstalleerd op een systeem en de virtuele machine en mogelijke compatibiliteitsproblemen identificeert.

**2 (uitgebreid)**. Als u meer inzicht, waaronder: informatie over hoe het besturingssysteem en andere Azure-Stack-services worden gebruikt, hoe ze uitvoeren, geavanceerde betrouwbaarheidsgegevens en gegevens uit zowel de Basic- en beveiligingsniveaus. 

**3 (volledig)**. Alle gegevens die nodig zijn om te bepalen en te helpen bij het oplossen van problemen, plus gegevens uit de **beveiliging**, **Basic**, en **uitgebreid** niveaus.

> [!NOTE]
> De standaardwaarde van het niveau van telemetrie is 2 (uitgebreid).

Houd er rekening mee dat Windows- en Azure Stack telemetrie uitschakelen ook SQL telemetrie uitschakelen wordt. Voor meer informatie over de gevolgen van de Windows Server-telemetrie instellingen verwijzing neemt de [Windows telemetrie technisch document](https://aka.ms/winservtelemetry). 

> [!IMPORTANT]
> Deze niveaus telemetrie zijn alleen van toepassing op Microsoft Azure-Stack-onderdelen. Niet-Microsoft-software-onderdelen en services die worden uitgevoerd in de Host van de levenscyclus van Hardware van Azure-Stack hardwarepartners kunnen communiceren met hun cloud-services buiten deze niveaus telemetrie. U moet samenwerken met uw Azure-Stack hardware solution provider over hun beleid Telemetrie en hoe u kunt aanmelden of opt-out. 

### <a name="asdk-and-multinode-enable-or-disable-telemetry-after-deployment"></a>ASDK en MultiNode: in- of uitschakelen van telemetrie na implementatie

Als u wilt in- of uitschakelen van telemetrie na de implementatie, moet u aan de bevoorrechte eindpunt (PEP) die wordt weergegeven op de ERCS virtuele machines toegang hebben.
1.  Om in te schakelen:`Set-Telemetry -Enable`
2.  Uitschakelen:`Set-Telemetry -Disable`

Details van de PARAMETER: 
> . PARAMETER Enable - inschakelen telemetrie uploaden van gegevens 

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
[Het implementatiepakket Azure Stack development kit downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure-Stack development kit implementeren](azure-stack-run-powershell-script.md)

