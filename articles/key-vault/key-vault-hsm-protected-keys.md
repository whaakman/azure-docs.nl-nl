---
title: Genereren en overdragen van met HSM beveiligde sleutels voor Azure Sleutelkluis | Microsoft Docs
description: Gebruik dit artikel voor hulp bij het plannen en brengt u uw eigen HSM beveiligde sleutels voor gebruik met Azure Sleutelkluis genereren. Ook wel BYOK of breng uw eigen sleutel.
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: barclayn
ms.openlocfilehash: 0d34a19658ae67a9c98d6f31aaca35e67add5beb
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Genereren en overdragen HSM beveiligde sleutels voor Azure Sleutelkluis
## <a name="introduction"></a>Inleiding
Voor de zekerheid wanneer u Azure Sleutelkluis, gebruikt kunt u importeren of genereren van sleutels in hardware security modules (HSM's) die de HSM-grens nooit verlaten. Dit scenario wordt vaak aangeduid als *Breng uw eigen sleutel*, of BYOK. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Azure Sleutelkluis gebruikt Thales nShield-familie van HSM's voor het beveiligen van uw sleutels.

Gebruik de informatie in dit onderwerp voor hulp bij het plannen en brengt u uw eigen HSM beveiligde sleutels voor gebruik met Azure Sleutelkluis genereren.

Deze functionaliteit is niet beschikbaar voor Azure China.

> [!NOTE]
> Zie voor meer informatie over Azure Sleutelkluis [wat is Azure Sleutelkluis?](key-vault-whatis.md)  
>
> Zie voor een zelfstudie aan de slag, waaronder het maken van een sleutelkluis voor met HSM beveiligde sleutels, [aan de slag met Azure Key Vault](key-vault-get-started.md).
>
>

Meer informatie over het genereren en overdragen van een HSM beveiligde sleutel via Internet:

* U kunt de sleutel genereren van een offline werkstation, wat minder gevoelig voor aanvallen.
* De sleutel is gecodeerd met een KEK Key Exchange Key (), die versleuteld blijft totdat deze worden overgedragen naar de Azure Key Vault HSM's. Alleen de versleutelde versie van uw sleutel verlaat het oorspronkelijke werkstation.
* De toolset stelt de eigenschappen van uw tenantsleutel die wordt uw sleutel aan de beveiligingswereld van Azure Sleutelkluis gebonden. Dus nadat de Azure Key Vault HSM's ontvangen en uw sleutel ontsleuteld, kunnen deze HSM's gebruiken. Uw sleutel kan niet worden geëxporteerd. Deze binding wordt afgedwongen door de Thales HSM's.
* De KEK Key Exchange Key () die wordt gebruikt voor het versleutelen van uw sleutel wordt gegenereerd binnen de Azure Key Vault HSM's en kan niet worden geëxporteerd. De HSM's dwingen af dat er geen versie van de KEK buiten de HSM's kunnen worden. Daarnaast bevat de toolset attest van Thales dat de KEK kan niet worden geëxporteerd en is gegenereerd binnen een legitieme door Thales vervaardigde HSM.
* De toolset bevat een attest van Thales dat de Azure Sleutelkluis-beveiligingswereld ook is gegeneerd met een legitieme HSM, door Thales vervaardigde. Deze verklaring is uw bewijs dat Microsoft legitieme hardware wordt gebruikt.
* Microsoft gebruikt afzonderlijke kek's en afzonderlijke Beveiligingswerelden in elke geografische regio. Deze scheiding zorgt ervoor dat uw sleutel kan alleen worden gebruikt in datacenters in de regio waarin u het versleuteld. Bijvoorbeeld, kan niet een sleutel van een Europese klant worden gebruikt in datacenters in Noord-Amerika of Azië.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>Meer informatie over Thales HSM's en Microsoft-services
Thales e-Security is een toonaangevende, wereldwijde leverancier van gegevensversleuteling en cyberbeveiliging beveiligingsoplossingen voor de financiële, hightech productie, overheid en technologiesector. Met een 40 jaar bogen van het beveiligen van bedrijfs- en overheidsgegevens worden oplossingen van Thales gebruikt door vier van de vijf grootste energie en ruimtevaart bedrijven. Hun oplossingen worden ook gebruikt door 22 NAVO-landen en meer dan 80 procent van de betalingstransacties wereldwijd beveiligen.

Microsoft heeft samengewerkt met Thales voor het verbeteren van de status van de illustraties HSM's. Deze verbeteringen kunnen u profiteren van de gebruikelijke voordelen van gehoste services, zonder verliest controle over uw sleutels. In het bijzonder kan deze uitbreidingen Microsoft de HSM's zodat u niet te hoeft beheren. Als een cloudservice schaalt Azure Key Vault op korte termijn om te voldoen aan de gebruikspieken binnen uw organisatie. Op hetzelfde moment uw sleutel beschermd binnen de HSM's van Microsoft: U behoudt controle over de levenscyclus van de sleutel omdat u de sleutel te genereren en naar HSM's van Microsoft overdragen.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementatie van uw eigen BYOK (bring key) voor Azure Sleutelkluis
Gebruik de volgende informatie en procedures als u uw eigen met HSM beschermde sleutel genereren en vervolgens naar Azure Sleutelkluis overbrengen, het bring uw eigen scenario key (BYOK).

## <a name="prerequisites-for-byok"></a>Vereisten voor BYOK
Zie de volgende tabel voor een lijst met vereisten voor uw eigen BYOK (bring key) voor Azure Sleutelkluis.

| Vereiste | Meer informatie |
| --- | --- |
| Een abonnement op Azure |Voor het maken van een Azure Sleutelkluis, moet u een Azure-abonnement: [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) |
| De Azure Key Vault Premium servicecategorie ter ondersteuning van met HSM beveiligde sleutels |Zie voor meer informatie over de servicecategorieën en mogelijkheden voor Azure Sleutelkluis de [prijzen van Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) website. |
| Thales HSM, smartcards en ondersteunende software |U moet toegang hebben tot een Thales Hardware Security Module en operationele basiskennis hebben van Thales HSM's. Zie [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) voor de lijst met compatibele modellen of om aan te schaffen van een HSM, als u nog geen abonnement hebt. |
| De volgende hardware en software:<ol><li>Een offline x64 werkstation met een besturingssysteem minimaal Windows 7 en Thales nShield-software die ten minste versie 11.50.<br/><br/>Als dit werkstation Windows 7 wordt uitgevoerd, moet u [Installeer Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Een werkstation dat is verbonden met Internet en een Windows-besturingssysteem minimaal Windows 7 en [Azure PowerShell](/powershell/azure/overview) **minimaal versie 1.1.0** geïnstalleerd.</li><li>Een USB-station of ander draagbaar opslagapparaat met ten minste 16 MB vrije ruimte heeft.</li></ol> |Uit veiligheidsoverwegingen is het raadzaam dat de eerste werkstation niet is verbonden met een netwerk. Deze aanbeveling is echter niet via een programma afgedwongen.<br/><br/>Houd er rekening mee dat in de volgende instructies wordt dit werkstation aangeduid als niet-verbonden werkstation.</p></blockquote><br/>Bovendien, als uw tenantsleutel bedoeld voor een productienetwerk is, raden wij aan dat u een tweede, afzonderlijk werkstation gebruiken om te downloaden van de toolset en de tenantsleutel te uploaden. Maar voor testdoeleinden kunt u hetzelfde werkstation gebruiken als het eerste.<br/><br/>Houd er rekening mee dat in de volgende instructies wordt dit tweede werkstation aangeduid als het met Internet verbonden werkstation.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Genereren en overdragen van uw sleutel naar Azure Key Vault HSM
Gebruik de volgende vijf stappen voor het genereren en uw sleutel overdraagt naar een Azure Key Vault HSM:

* [Stap 1: Uw met Internet verbonden werkstation voorbereiden](#step-1-prepare-your-internet-connected-workstation)
* [Stap 2: Uw niet-verbonden werkstation voorbereiden](#step-2-prepare-your-disconnected-workstation)
* [Stap 3: Uw sleutel genereren](#step-3-generate-your-key)
* [Stap 4: De sleutel voor de overdracht voorbereiden](#step-4-prepare-your-key-for-transfer)
* [Stap 5: Uw sleutel overdraagt naar Azure Sleutelkluis](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Stap 1: Uw met Internet verbonden werkstation voorbereiden
Deze eerste stap komen de volgende procedures op uw werkstation dat is verbonden met Internet.

### <a name="step-11-install-azure-powershell"></a>Stap 1.1: Azure PowerShell installeren
Vanaf het Internet verbonden werkstation, download en installeer de Azure PowerShell-module met de cmdlets voor het beheren van Azure Sleutelkluis. Hiervoor moet een minimumversie van 0.8.13.

Zie voor installatie-instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>Stap 1.2: Uw Azure-abonnement-ID ophalen
Start een Azure PowerShell-sessie en meld u aan bij uw Azure-account met behulp van de volgende opdracht:

```Powershell
   Add-AzureAccount
```
Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Gebruik vervolgens de [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) opdracht:

```powershell
   Get-AzureSubscription
```
Zoek de ID voor het abonnement dat u voor Azure Sleutelkluis gebruiken wilt van de uitvoer. U kunt deze abonnement-ID wordt later nodig.

De Azure PowerShell-venster niet sluiten.

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Stap 1.3: Download de BYOK-toolset voor Azure Sleutelkluis
Ga naar het Microsoft Download Center en [download de Azure Key Vault BYOK-hulpmiddelenset](http://www.microsoft.com/download/details.aspx?id=45345) voor uw geografische regio of het exemplaar van Azure. Gebruik de volgende informatie om te identificeren, de naam van het pakket downloaden en de bijbehorende SHA-256-pakket-hash:

- - -
**Verenigde Staten:**

KeyVault-BYOK-hulpprogramma's-Verenigd States.zip

2E8C00320400430106366A4E8C67B79015524E4EC24A2D3A6DC513CA1823B0D4

- - -
**Europa:**

KeyVault-BYOK-hulpprogramma's-Europe.zip

9AAA63E2E7F20CF9BB62485868754203721D2F88D300910634A32DFA1FB19E4A

- - -
**Azië:**

KeyVault-BYOK-hulpprogramma's-AsiaPacific.zip

4BC14059BF0FEC562CA927AF621DF665328F8A13616F44C977388EC7121EF6B5

- - -
**Latijns-Amerika:**

KeyVault-BYOK-hulpprogramma's-LatinAmerica.zip

E7DFAFF579AFE1B9732C30D6FD80C4D03756642F25A538922DD1B01A4FACB619

- - -
**Japan:**

KeyVault-BYOK-hulpprogramma's-Japan.zip

3933C13CC6DC06651295ADC482B027AF923A76F1F6BF98B4D4B8E94632DEC7DF

- - -
**Korea:**

KeyVault-BYOK-hulpprogramma's-Korea.zip

71AB6BCFE06950097C8C18D532A9184BEF52A74BB944B8610DDDA05344ED136F

- - -
**Australië:**

KeyVault-BYOK-hulpprogramma's-Australia.zip

CD0FB7365053DEF8C35116D7C92D203C64A3D3EE2452A025223EEB166901C40A

- - -
[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-hulpprogramma's-USGovCloud.zip

F8DB2FC914A7360650922391D9AA79FF030FD3048B5795EC83ADC59DB018621A

- - -
**Amerikaanse overheid DOD:**

KeyVault-BYOK-hulpprogramma's-USGovernmentDoD.zip

A79DD8C6DFFF1B00B91D1812280207A205442B3DDF861B79B8B991BB55C35263

- - -
**Canada:**

KeyVault-BYOK-hulpprogramma's-Canada.zip

61BE1A1F80AC79912A42DEBBCC42CF87C88C2CE249E271934630885799717C7B

- - -
**Duitsland:**

KeyVault-BYOK-hulpprogramma's-Germany.zip

5385E615880AAFC02AFD9841F7BADD025D7EE819894AA29ED3C71C3F844C45D6

- - -
**India:**

KeyVault-BYOK-hulpprogramma's-India.zip

49EDCEB3091CF1DF7B156D5B495A4ADE1CFBA77641134F61B0E0940121C436C8

- - -
**Verenigd Koninkrijk:**

KeyVault-BYOK-hulpprogramma's-UnitedKingdom.zip

432746BD0D3176B708672CCFF19D6144FCAA9E5EB29BB056489D3782B3B80849

- - -

U kunt controleren de integriteit van de gedownloade BYOK-toolset van uw Azure PowerShell-sessie gebruiken de [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.

   ```powershell
   Get-FileHash KeyVault-BYOK-Tools-*.zip
   ```

De toolset bevat het volgende:

* Een KEK Key Exchange Key ()-pakket met een naam die begint met **BYOK-KEK - pkg-.**
* Een beveiligingswereldpakket met een naam die begint met **BYOK-SecurityWorld - pkg-.**
* Een pythonscript met de naam **verifykeypackage.py.**
* Een uitvoerbaar opdrachtregelbestand met de naam **KeyTransferRemote.exe** en bijbehorende DLL-bestanden.
* Een herdistrueerbare pakket Visual C++, met de naam **vcredist_x64.exe.**

Kopieer het pakket naar een USB-station of ander draagbaar opslagmedium.

## <a name="step-2-prepare-your-disconnected-workstation"></a>Stap 2: Uw niet-verbonden werkstation voorbereiden
Voer de volgende procedures op het werkstation dat niet is verbonden met een netwerk (Internet of het interne netwerk) voor deze tweede stap.

### <a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Stap 2.1: De niet-verbonden werkstation met Thales HSM voorbereiden
De (Thales) nCipher-ondersteuningssoftware installeren op een Windows-computer en vervolgens een Thales HSM aan die computer te koppelen.

Zorg ervoor dat de Thales-hulpprogramma's in het pad (**%nfast_home%\bin**). Typ bijvoorbeeld het volgende:

  ```cmd
  set PATH=%PATH%;"%nfast_home%\bin"
  ```

Zie voor meer informatie de handleiding inbegrepen bij de Thales HSM.

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Step 2.2: Installeer de BYOK-toolset op niet-verbonden werkstation
Kopieer het BYOK-toolset pakket van het USB-station of ander draagbaar opslagmedium en doe het volgende:

1. Pak de bestanden uit het gedownloade pakket naar een map.
2. Voer vcredist_x64.exe uit in die map.
3. Volg de instructies voor de installatie het Visual C++ runtime-onderdelen voor Visual Studio 2013.

## <a name="step-3-generate-your-key"></a>Stap 3: Uw sleutel genereren
Voer de volgende procedures op de niet-verbonden werkstation voor deze derde stap. Voltooi deze stap moet uw HSM initialisatie-modus. 


### <a name="step-31-change-the-hsm-mode-to-i"></a>Stap 3.1: De HSM-modus wijzigen in 'I'
Als u Thales nShield Microsoft Edge wordt gebruikt om de modus te wijzigen: 1. Gebruik de knop modus om de vereiste modus te markeren. 2. Binnen een paar seconden en houdt u de knop wissen van een paar seconden. Als de modus wordt gewijzigd, wordt de nieuwe modus LED niet meer knippert en blijft branden. De Status LED onregelmatig mogelijk flash een paar seconden en vervolgens knippert regelmatig wanneer het apparaat gereed is. Anders wordt het apparaat blijft in de huidige modus, met de juiste modus LED branden.

### <a name="step-32-create-a-security-world"></a>Stap 3.2: Maak een beveiligingswereld
Start een opdrachtprompt en voer het nieuwe-wereld-programma van Thales.

   ```cmd
    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3
   ```

Dit programma maakt een **Beveiligingswereld** bestand op % NFAST_KMDATA%\local\world, wat overeenkomt met de map C:\ProgramData\nCipher\Key Management Settings\User. U kunt andere waarden gebruiken voor het quorum, maar in ons voorbeeld wordt u gevraagd drie lege kaarten en pincodes voor elk adres invoeren. Vervolgens wordt door elke twee kaarten volledige toegang geven tot de beveiligingswereld. Deze kaarten worden de **Beheerderskaartenset** voor de nieuwe beveiligingswereld.

Ga daarna als volgt te werk:

* Back-up van het wereldbestand. Secure en beveiligen van het wereldbestand, de Beheerderskaarten en de bijbehorende pincodes en zorg ervoor dat niemand toegang tot meer dan één kaart heeft.

### <a name="step-33-change-the-hsm-mode-to-o"></a>Stap 3.3: Wijzig de HSM-modus ' o '
Als u Thales nShield Microsoft Edge wordt gebruikt om de modus te wijzigen: 1. Gebruik de knop modus om de vereiste modus te markeren. 2. Binnen een paar seconden en houdt u de knop wissen van een paar seconden. Als de modus wordt gewijzigd, wordt de nieuwe modus LED niet meer knippert en blijft branden. De Status LED onregelmatig mogelijk flash een paar seconden en vervolgens knippert regelmatig wanneer het apparaat gereed is. Anders wordt het apparaat blijft in de huidige modus, met de juiste modus LED branden.


### <a name="step-34-validate-the-downloaded-package"></a>Stap 3.4: Het gedownloade pakket valideren
Deze stap is optioneel maar aanbevolen zodat u het volgende kunt valideren:

* De uitwisselingssleutel van de sleutel die is opgenomen in de hulpmiddelenset, is gegenereerd met een legitieme Thales HSM.
* De hash van de Beveiligingswereld die is opgenomen in de hulpmiddelenset, is gegenereerd met een legitieme Thales HSM.
* De Key Exchange is niet exporteerbaar.

> [!NOTE]
> Als u wilt het gedownloade pakket valideren, moet de HSM moet worden verbonden, zijn ingeschakeld, en een beveiligingswereld (zoals de groep die u zojuist hebt gemaakt).
>
>

Het gedownloade pakket valideren:

1. Voer het script door één van de volgende, afhankelijk van uw geografische regio of het exemplaar van Azure te typen:

   * Voor Noord-Amerika:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * Voor Europa:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * Voor Azië:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * Voor Latijns-Amerika:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * Voor Japan:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * Voor Korea:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * Voor Australië:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * Voor [Azure Government](https://azure.microsoft.com/features/gov/), waarbij het US government-exemplaar van Azure wordt gebruikt:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * Voor de overheid van de VS DOD:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Voor Canada:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Voor Duitsland:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Voor India:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
   * Voor Verenigd Koninkrijk:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UK-1 -w BYOK-SecurityWorld-pkg-UK-1

     > [!TIP]
     > De Thales-software bevat python in %NFAST_HOME%\python\bin
     >
     >
2. Controleer of u het volgende, waarmee wordt aangegeven validatie is geslaagd: **resultaat: geslaagd**

Dit script valideert de ondertekenaarsketen tot de Thales-hoofdsleutel. De hash van deze hoofdsleutel is ingesloten in het script en de waarde moet **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. U kunt deze waarde ook afzonderlijk controleren in via de [Thales-website](http://www.thalesesec.com/).

U kunt nu een nieuwe sleutel te maken.

### <a name="step-35-create-a-new-key"></a>Stap 3.5: Maak een nieuwe sleutel
Een sleutel genereren met behulp van de Thales **generatekey** programma.

Voer de volgende opdracht om de sleutel te genereren:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

* De parameter *beveiligen* moet worden ingesteld op de waarde **module**, zoals wordt weergegeven. Hiermee maakt u een modulair beveiligde sleutel. De BYOK-toolset biedt geen ondersteuning voor OCS beveiligde sleutels.
* Vervang de waarde van *contosokey* voor de **ident** en **plainname** met een tekenreekswaarde. Om administratieve overhead te minimaliseren en verlaagt de kans op fouten, is het raadzaam dat u dezelfde waarde voor beide gebruiken. De **ident** waarde mag alleen cijfers, streepjes en kleine letters bestaan.
* De pubexp is leeg (standaard) in dit voorbeeld, maar u kunt specifieke waarden opgeven. Zie de Thales-documentatie voor meer informatie.

Deze opdracht maakt u een Tokenized sleutelbestand in uw map %NFAST_KMDATA%\local met een naam die begint met **key_simple_**, gevolgd door de **ident** dat is opgegeven in de opdracht. Bijvoorbeeld: **key_simple_contosokey**. Dit bestand bevat een versleutelde sleutel.

Back-up van deze Tokenized sleutelbestand op een veilige locatie.

> [!IMPORTANT]
> Wanneer u uw sleutel later naar Azure Sleutelkluis overdraagt, Microsoft kan sleutel niet exporteren dit voor u dus is het erg belangrijk back-up van uw sleutel en beveiligingswereld veilig. Neem contact op met Thales voor richtlijnen en aanbevolen procedures voor back-ups van uw sleutel.
>
>

U bent nu klaar om over te dragen van uw sleutel naar Azure Sleutelkluis.

## <a name="step-4-prepare-your-key-for-transfer"></a>Stap 4: De sleutel voor de overdracht voorbereiden
Voer de volgende procedures op de niet-verbonden werkstation voor deze vierde stap.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Stap 4.1: Maak een kopie van uw sleutel met beperkte machtigingen

Open een nieuw opdrachtpromptvenster en wijzig de huidige map naar de locatie waar u de BYOK-zip-bestand hebt uitgepakt. Als u de machtigingen voor uw sleutel vanaf een opdrachtprompt, voert u een van de volgende, afhankelijk van uw geografische regio of het exemplaar van Azure:

* Voor Noord-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* Voor Europa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* Voor Azië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* Voor Latijns-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* Voor Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* Voor Korea:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* Voor Australië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* Voor [Azure Government](https://azure.microsoft.com/features/gov/), waarbij het US government-exemplaar van Azure wordt gebruikt:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* Voor de overheid van de VS DOD:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Voor Canada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Voor Duitsland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Voor India:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1
* Voor Verenigd Koninkrijk:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1

Wanneer u deze opdracht uitvoert, vervangt *contosokey* door dezelfde waarde als u hebt opgegeven in **stap 3.5: Maak een nieuwe sleutel** van de [uw sleutel genereren](#step-3-generate-your-key) stap.

U wordt gevraagd om uw security world admin-kaarten.

Wanneer de opdracht is voltooid, ziet u **resultaat: SUCCES** en de kopie van uw sleutel met beperkte machtigingen zich in het bestand met de naam key_xferacId_<contosokey>.

U kunt inspecteert de ACL's met behulp van volgende opdrachten met de Thales-hulpprogramma's:

* aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Wanneer u deze opdrachten uitvoert, vervangt u contosokey met dezelfde waarde die u hebt opgegeven in **stap 3.5: Maak een nieuwe sleutel** van de [uw sleutel genereren](#step-3-generate-your-key) stap.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Stap 4.2: Versleutel de sleutel met behulp van Microsoft sleutel voor sleuteluitwisseling
Voer een van de volgende opdrachten uit, afhankelijk van uw geografische regio of het exemplaar van Azure:

* Voor Noord-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Europa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Azië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Latijns-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Korea:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Australië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor [Azure Government](https://azure.microsoft.com/features/gov/), waarbij het US government-exemplaar van Azure wordt gebruikt:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor de overheid van de VS DOD:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Canada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Duitsland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor India:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Verenigd Koninkrijk:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

* Vervang *contosokey* met de id die u gebruikt voor het genereren van de sleutel in **stap 3.5: Maak een nieuwe sleutel** van de [uw sleutel genereren](#step-3-generate-your-key) stap.
* Vervang *SubscriptionID* met de ID van de Azure-abonnement dat de sleutelkluis bevat. U hebt deze waarde opgehaald in **stap 1.2: uw Azure-abonnement-ID ophalen** van de [uw met Internet verbonden werkstation voorbereiden](#step-1-prepare-your-internet-connected-workstation) stap.
* Vervang *ContosoFirstHSMKey* met een label dat wordt gebruikt voor naam van uw uitvoerbestand.

Wanneer dit succesvol is voltooid, wordt weergegeven **resultaat: SUCCES** en er is een nieuw bestand in de huidige map met de volgende naam: KeyTransferPackage -*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Stap 4.3: Kopieer uw pakket voor sleuteloverdracht naar het Internet verbonden werkstation
Gebruik een USB-station of ander draagbaar opslagmedium kopiëren van het bestand voor uitvoer van de vorige stap (KeyTransferPackage-ContosoFirstHSMkey.byok) naar uw met Internet verbonden werkstation.

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a>Stap 5: Uw sleutel overdraagt naar Azure Sleutelkluis
Voor deze laatste stap op het Internet verbonden werkstation maakt gebruik van de [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet voor het uploaden van het sleuteloverdrachtpakket die u hebt gekopieerd uit de niet-verbonden werkstation naar Azure Key Vault HSM:

   ```powershell
        Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'
   ```

Als het uploaden voltooid is, ziet u de eigenschappen van de sleutel die u zojuist hebt toegevoegd wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen
U kunt deze met HSM beschermde sleutel nu gebruiken in de sleutelkluis. Zie voor meer informatie de **als u wilt gebruiken een hardware security module (HSM)** sectie het [aan de slag met Azure Key Vault](key-vault-get-started.md) zelfstudie.
