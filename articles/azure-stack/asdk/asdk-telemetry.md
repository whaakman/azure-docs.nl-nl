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
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: e50cb33acc95567a24afb19f88f88a3f586e0124
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224747"
---
# <a name="azure-stack-telemetry"></a>Azure Stack-telemetrie

Azure Stack-systeemgegevens of telemetrie, wordt automatisch geüpload naar Microsoft via de gebruikerservaring die zijn verbonden. Gegevens die worden verzameld van de telemetriegegevens van de Azure Stack wordt gebruikt door Microsoft teams voornamelijk bedoeld voor het verbeteren van onze klantervaringen en voor beveiliging, status, kwaliteit en prestaties analysis.

Telemetrie-als Azure Stack-operators, krijgt u inzicht in de enterprise-implementaties en biedt u een stem waarmee toekomstige versies van de vorm van Azure Stack.

> [!NOTE]
> Azure Stack kan ook worden geconfigureerd voor informatie over het gebruik van doorsturen naar Azure voor facturering. Dit is vereist voor meerdere knooppunten Azure Stack-klanten die betalen als u-gebruik facturering. Rapportage over het gebruik van de telemetriegegevens onafhankelijk van elkaar wordt beheerd en is niet vereist zijn voor klanten met meerdere knooppunten die het capaciteitsmodel of voor gebruikers van Azure Stack Development Kit. Voor deze scenario's, rapportage over het gebruik kan worden uitgeschakeld [met behulp van het script voor de registratie](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure Stack-telemetrie is gebaseerd op de verbonden gebruikerservaring van Windows Server 2016 en telemetrie-onderdeel dat gebruikmaakt van de [Event Tracing voor Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) logboekregistratie technologie om te verzamelen en opslaan van telemetriegebeurtenissen en gegevens traceren. Azure Stack-onderdelen gebruiken dezelfde logboekregistratie technologie voor het publiceren van gebeurtenissen en de gegevens die zijn verzameld met behulp van openbare besturingssysteem logboekregistratie en tracering van API's. Voorbeelden van Azure Stack-onderdelen zijn Netwerkresourceprovider, Storage Resource Provider Resourceprovider bewaking en Provider voor Update-Resource. Het onderdeel gebruikerservaring verbonden en telemetrie versleutelt gegevens met behulp van SSL en maakt gebruik van certificaten vast te maken voor het verzenden van telemetriegegevens via HTTPS naar de Microsoft Data Management-service.

> [!NOTE]
> Ter ondersteuning van telemetriegegevensstroom, moet poort 443 (HTTPS) openen in uw netwerk. Het onderdeel gebruikerservaring verbonden en telemetrie maakt verbinding met de Microsoft Data Management-service op https://v10.vortex-win.data.microsoft.com. Het onderdeel gebruikerservaring verbonden en telemetrie ook verbinding maakt met https://settings-win.data.microsoft.com voor het downloaden van configuratie-informatie.

## <a name="privacy-considerations"></a>Privacy-overwegingen
De ETW-service stuurt telemetrische gegevens terug naar een beveiligde cloud-opslag. Het principe van minste uitgebreide handleidingen toegang tot de telemetrische gegevens. Alleen Microsoft-personeel met een geldige zakelijke behoeften hebben toegang tot de telemetrische gegevens. Microsoft deelt niet persoonlijke gegevens van onze klanten met derden, met uitzondering van de klant goeddunken of voor de beperkte toepassing beschreven in de [privacyverklaring voor Azure Stack](https://privacy.microsoft.com/PrivacyStatement). We zakelijke rapporten delen met OEM's en partners die verzamelde, geanonimiseerde telemetriegegevens bevatten. Beslissingen voor het delen van gegevens worden gemaakt door een interne Microsoft-team, met inbegrip van privacy, juridische en management belanghebbenden.

Microsoft streeft ernaar en informatie tot procedures. We streven ernaar om het verzamelen van alleen de informatie die we nodig hebben en we slaan alleen zolang dat nodig is om een service te bieden of voor analyse. Veel van de informatie over hoe de Azure Stack-systeem en de Azure-services werken binnen zes maanden verwijderd. Samengevatte of geaggregeerde gegevens worden bewaard voor een langere periode.

We begrijpen dat de privacy en beveiliging van onze klanten gegevens belangrijk is. Aan de privacy van klanten en de bescherming van gegevens van de klant met Azure Stack, hebben we een benadering doordachte en uitgebreide genomen. IT-beheerders hebben besturingselementen voor het aanpassen van functies en privacy-instellingen op elk gewenst moment. Onze toewijding aan transparantie en vertrouwen is duidelijk:
- We zijn open met klanten over de typen gegevens die we verzamelen.
- We plaatsen enterprise-klanten in besturingselement, kunnen ze hun eigen privacy-instellingen aanpassen.
- We plaats privacy van klanten en beveiliging eerst.
- We zijn transparant over hoe telemetrie wordt gebruikt.
- We gebruiken telemetrie te verbeteren van klantervaring.

Microsoft is niet van plan bent voor het verzamelen van vertrouwelijke gegevens, zoals creditcardnummers, gebruikersnamen en wachtwoorden, e-mailadressen of andere op dezelfde manier gevoelige informatie. Als we ontdekken dat gevoelige informatie per ongeluk is ontvangen, wordt het verwijderen.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Voorbeelden van hoe Microsoft de telemetriegegevens die zijn gebruikt
Telemetrie speelt een belangrijke rol bij ons snel opsporen en oplossen van problemen met betrouwbaarheid in implementaties en configuraties van onze klanten. Inzicht in de telemetrische gegevens die we verzamelen, kunnen we snel identificeren van problemen met services of hardwareconfiguraties. De mogelijkheid van Microsoft om deze gegevens van klanten en verbeteringen in het ecosysteem station helpt de LAT voor de kwaliteit van onze geïntegreerde Azure Stack-oplossingen.

Telemetrie helpt ook bij Microsoft om beter te begrijpen hoe de onderdelen voor het implementeren van klanten, gebruik van functies en services gebruiken om hun zakelijke doelstellingen te realiseren. Inzichten krijgen van die gegevens prioriteiten kunt op de belangrijkste gebieden direct voor onze klanten ervaringen en werkbelastingen gevolgen kunnen.

Enkele voorbeelden zijn klant gebruik van containers, opslag- en netwerkconfiguraties die gekoppeld aan Azure Stack-rollen zijn. We gebruiken ook de inzichten station verbeteringen en intelligentie in enkele van onze beheer en controle van oplossingen. Dit helpt klanten bij het vaststellen van kwaliteitsproblemen met en geld door minder ondersteuning kunnen besparen aanroepen naar Microsoft.

## <a name="manage-telemetry-collection"></a>Verzamelen van telemetriegegevens beheren
We raden niet dat u telemetrie in uw organisatie uitschakelen als telemetrie biedt gegevens waar verbeterde productfunctionaliteit en stabiliteit. We echter herkent dat in sommige scenario's dit kan nodig zijn.

In dergelijke gevallen kunt kunt u het telemetrieniveau van de naar Microsoft verzonden met behulp van Register instellingen voorafgaand aan de implementatie of met behulp van de telemetrie-eindpunten na de implementatie configureren.

### <a name="set-telemetry-level-in-the-windows-registry"></a>Telemetrieniveau instellen in het Windows-register
De Windows-register-Editor wordt gebruikt om het telemetrieniveau van de handmatig instellen op de fysieke hostcomputer vóór de implementatie van Azure Stack. Als een management-beleid al, zoals Groepsbeleid bestaat, overschrijft deze instelling in het register.

Voordat u implementeert Azure Stack op de host van development kit, opstarten naar de CloudBuilder.vhdx en voer het volgende script in een PowerShell-venster met verhoogde bevoegdheid:

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

De telemetrie-niveaus zijn cumulatief en gecategoriseerd in vier niveaus (0-3):

**0 (beveiliging)**. Alleen beveiligingsgegevens. Informatie die vereist zijn om te zorgen dat het besturingssysteem beveiligen, met inbegrip van gegevens over de instellingen voor gebruikerservaring verbonden en telemetrie-onderdeel en de Windows Defender. Er zijn geen specifieke Azure Stack-telemetrie wordt verzonden op dit niveau.

**1 (basis)**. Beveiligingsgegevens, en Basisbewaking van statussen en kwaliteit. Basic apparaatgegevens, met inbegrip van: gegevens met betrekking tot kwaliteit, app-compatibiliteit, app-gebruiksgegevens en gegevens van het beveiligingsniveau. Uw telemetrieniveau instellen op Basic kunnen Azure Stack telemetrie. De gegevens die zijn verzameld op dit niveau bevat:

- **Basic apparaatgegevens** die zorgt voor een goed begrip over de typen en -configuraties van systeemeigen als gevirtualiseerde Windows Server 2016-instanties in het ecosysteem, waaronder:
  - Model voor machine kenmerken, zoals de OEM,
  - Kenmerken, zoals het aantal en de snelheid van netwerkadapters, netwerken
  - Processor en geheugenkenmerken, zoals het aantal kernen, geheugengrootte
  - Opslagkenmerken, zoals het aantal schijven, het type en grootte.
- **Telemetrie-functionaliteit**, waaronder procent van de geüploade gebeurtenissen, Verwijderde gebeurtenissen en de laatste keer uploaden.
- **Informatie met betrekking tot kwaliteit** die helpt Microsoft bij het ontwikkelen van een basiskennis hebt van hoe Azure Stack wordt uitgevoerd. Een voorbeeld is het aantal kritieke waarschuwingen op een specifieke hardwareconfiguratie.
- **Compatibiliteit**, die zorgt voor een goed begrip over welke Resource Providers zijn geïnstalleerd op een systeem en de virtuele machine en identificeert mogelijke compatibiliteitsproblemen.

**2 (uitgebreid)**. Extra inzichten, met inbegrip van: informatie over hoe het besturingssysteem en andere Azure Stack-services worden gebruikt, hoe ze uitvoeren, geavanceerde betrouwbaarheidsgegevens en gegevens van zowel de Basic- en beveiligingsniveaus.

**3 (volledig)**. Alle gegevens die nodig zijn om te identificeren en te helpen problemen op te lossen, plus gegevens uit de **Security**, **Basic**, en **uitgebreid** niveaus.

> [!NOTE]
> De standaardwaarde van het niveau van telemetrie is 2 (uitgebreid).

Hiermee schakelt u Windows- en Azure Stack telemetrie uitschakelen SQL telemetrie. Raadpleeg voor meer informatie over de gevolgen van de telemetrie-instellingen voor Windows Server, de [technisch document Windows-telemetrie](https://aka.ms/winservtelemetry).

> [!IMPORTANT]
> Deze niveaus telemetrie zijn alleen van toepassing op Microsoft Azure Stack-onderdelen. Niet-Microsoft-software-onderdelen en services die worden uitgevoerd in de Host van de levenscyclus van Hardware van Azure Stack-hardware-partners kunnen communiceren met hun cloud-services buiten deze telemetrie-niveaus. U moet werken met uw Azure Stack-solution-provider voor hardware om te begrijpen van hun beleid telemetrie, en hoe u kunt ervoor kiezen of opt-out.

### <a name="enable-or-disable-telemetry-after-deployment"></a>In- of uitschakelen telemetrie na de implementatie

Als u wilt in- of uitschakelen telemetrie na de implementatie, moet u beschikken over naar de bevoegde eindpunt (PEP) die wordt weergegeven op de ERCS virtuele machines.
1.  Om in te schakelen: `Set-Telemetry -Enable`
2.  Om uit te schakelen: `Set-Telemetry -Disable`

Details van de PARAMETER:
> . Inschakelen van de PARAMETER - inschakelen telemetrie gegevens uploaden

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
[Een marketplace-item toevoegen](asdk-marketplace-item.md)
