---
title: Het genereren en overdragen van met HSM beveiligde sleutels voor Azure Key Vault - Azure Key Vault | Microsoft Docs
description: Gebruik dit artikel om te plannen, te genereren en vervolgens over te dragen van uw eigen HSM beveiligde sleutels gebruiken met Azure Key Vault. Ook wel BYOK of uw eigen sleutel.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: barclayn
ms.openlocfilehash: 49bc3c22eecf804e6930899478fe3be189677382
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985900"
---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Het genereren en overdragen met HSM beveiligde sleutels voor Azure Key Vault

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor extra zekerheid, wanneer u Azure Key Vault, gebruikt kunt u importeren of genereren van sleutels in hardware security modules (HSM's) die de HSM-grens nooit verlaten. In dit scenario wordt vaak aangeduid als *uw eigen sleutel*, of byok genoemd. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Azure Key Vault gebruikt Thales nShield-familie van HSM's voor het beveiligen van uw sleutels.

Gebruik de informatie in dit onderwerp om te plannen, te genereren en vervolgens over te dragen van uw eigen HSM beveiligde sleutels gebruiken met Azure Key Vault.

Deze functionaliteit is niet beschikbaar voor Azure China.

> [!NOTE]
> Zie voor meer informatie over Azure Key Vault [wat is Azure Key Vault?](key-vault-whatis.md)  
> Zie voor een aan de slag-zelfstudie, zoals het maken van een sleutelkluis voor HSM beschermde sleutels, [wat is Azure Key Vault?](key-vault-overview.md).

Meer informatie over het genereren en overdragen van een met HSM beveiligde sleutel via Internet:

* U kunt de sleutel genereren op een offline werkstation, waardoor de kwetsbaarheid voor aanvallen.
* De sleutel is gecodeerd met een sleutel (KEK-Exchange Key), die versleuteld blijft totdat deze zijn overgebracht naar de Azure Key Vault HSM's. Alleen de versleutelde versie van de sleutel verlaat het oorspronkelijke werkstation.
* De toolset stelt de eigenschappen van uw tenantsleutel die wordt uw sleutel aan de beveiligingswereld van Azure Key Vault gebonden. Dus nadat de Azure Key Vault HSM's ontvangen en de sleutel te ontsleutelen, kunnen deze HSM's gebruiken. De sleutel kan niet worden geëxporteerd. Deze binding wordt afgedwongen door de Thales HSM's.
* De sleutel (KEK-Key Exchange) dat wordt gebruikt voor het versleutelen van uw sleutel wordt gegenereerd in de Azure Key Vault HSM's en kan niet worden geëxporteerd. De HSM's dwingen af dat er geen versie van de KEK buiten de HSM's kunnen worden. De toolset bevat bovendien attest van Thales dat de KEK-sleutel kan niet worden geëxporteerd en is gegenereerd binnen een legitieme door Thales vervaardigde HSM.
* De toolset bevat een attest van Thales dat de Azure Key Vault-beveiligingswereld ook is gegeneerd met een legitieme HSM, door Thales vervaardigde. Deze verklaring is uw bewijs dat Microsoft legitieme hardware wordt gebruikt.
* Microsoft gebruikt afzonderlijke kek's en afzonderlijke Beveiligingswerelden in elke geografische regio. Dankzij deze scheiding zorgt ervoor dat de sleutel kan worden gebruikt alleen in datacenters in de regio waarin u hebt versleuteld. Bijvoorbeeld, kan niet een sleutel van een Europese klant worden gebruikt in datacenters in Noord-Amerika of Azië.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>Meer informatie over Thales HSM's en Microsoft-services

Thales e-Security is een toonaangevende wereldwijde leverancier van gegevensversleuteling en cyberbeveiliging beveiligingsoplossingen voor de financiële, hightech, productie, overheid en technologiesector. Met een 40 jaar bijhouden record van de bescherming van zakelijke en overheidsgegevens worden oplossingen van Thales gebruikt door vier van de vijf grootste energie- en ruimtevaart bedrijven. Hun oplossingen worden ook gebruikt door 22 NAVO-landen, en meer dan 80 procent van de betalingstransacties wereldwijd beveiligen.

Microsoft heeft samengewerkt met Thales om de status van illustraties voor HSM's. Deze verbeteringen kunnen u profiteren van de gebruikelijke voordelen van gehoste services, zonder verliest controle over uw sleutels. Om precies kan deze uitbreidingen Microsoft de HSM's zodat u niet te hoeft beheren. Als een cloudservice schaalt Azure Key Vault op korte termijn opschaalbaar om te voldoen aan de gebruikspieken van uw organisatie. Op hetzelfde moment, uw sleutel beschermd binnen de HSM's van Microsoft: U behoudt de controle over de levenscyclus van de omdat u de sleutel genereren en naar HSM's van Microsoft overdragen.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementatie van uw eigen sleutel (BYOK) brengen voor Azure Key Vault

Gebruik de volgende informatie en procedures als u uw eigen HSM beschermde sleutel genereren en vervolgens naar Azure Key Vault overbrengen, het uw eigen scenario key (BYOK).

## <a name="prerequisites-for-byok"></a>Vereisten voor BYOK

Zie de volgende tabel voor een lijst met vereisten voor het meenemen van uw eigen sleutel (BYOK) voor Azure Key Vault.

| Vereiste | Meer informatie |
| --- | --- |
| Een abonnement op Azure |Voor het maken van een Azure Key Vault, moet u een Azure-abonnement: [Aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) |
| De Azure Key Vault Premium-servicelaag voor de ondersteuning van met HSM beveiligde sleutels |Zie voor meer informatie over de service-lagen en mogelijkheden voor Azure Key Vault, de [prijzen van Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) website. |
| Thales HSM, smartcards en voor ondersteuningssoftware |U moet toegang hebben tot een Thales Hardware Security Module en operationele basiskennis hebben van Thales HSM's. Zie [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) voor de lijst met compatibele modellen of om aan te schaffen van een HSM, als u niet hebt. |
| De volgende hardware en software:<ol><li>Een offline x64 werkstation met een minimale Windows-besturingssysteem Windows 7 en Thales nShield-software ten minste versie 11.50.<br/><br/>Als dit werkstation Windows 7 wordt uitgevoerd, moet u [Microsoft .NET Framework 4.5 installeren](https://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Een werkstation dat is verbonden met Internet en heeft een minimumversie Windows-besturingssysteem van Windows 7 en [Azure PowerShell](/powershell/azure/overview?view=azps-1.2.0) **minimaal versie 1.1.0** geïnstalleerd.</li><li>Een USB-station of ander draagbaar opslagapparaat met ten minste 16 MB vrije ruimte.</li></ol> |Uit veiligheidsoverwegingen, wordt u aangeraden dat de eerste werkstation niet is verbonden met een netwerk. Deze aanbeveling is echter niet via een programma afgedwongen.<br/><br/>In de volgende instructies wordt dit werkstation aangeduid als niet-verbonden werkstation.</p></blockquote><br/>Bovendien, als uw tenantsleutel bedoeld voor een productienetwerk is, raden wij u een tweede, afzonderlijk werkstation gebruiken voor het downloaden van de toolset en uploaden van de tenant-sleutel. Maar voor testdoeleinden kunt u hetzelfde werkstation gebruiken als het eerste.<br/><br/>In de volgende instructies wordt dit tweede werkstation aangeduid als het met Internet verbonden werkstation.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Genereren en uw sleutel overdragen naar Azure Key Vault HSM

U kunt de volgende vijf stappen wilt gebruiken voor het genereren en uw sleutel overdragen naar een Azure Key Vault HSM:

* [Stap 1: Uw met Internet verbonden werkstation voorbereiden](#step-1-prepare-your-internet-connected-workstation)
* [Stap 2: Uw niet-verbonden werkstation voorbereiden](#step-2-prepare-your-disconnected-workstation)
* [Stap 3: De sleutel genereren](#step-3-generate-your-key)
* [Stap 4: Uw sleutel voorbereiden voor overdracht](#step-4-prepare-your-key-for-transfer)
* [Stap 5: Uw sleutel overdragen naar Azure Key Vault](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Stap 1: Uw met Internet verbonden werkstation voorbereiden

Voer de volgende procedures op uw werkstation dat is verbonden met Internet voor deze eerste stap.

### <a name="step-11-install-azure-powershell"></a>Stap 1.1: Azure PowerShell installeren

Vanuit het met Internet verbonden werkstation, download en installeer de Azure PowerShell-module met de cmdlets voor het beheren van Azure Key Vault. Zie voor installatie-instructies [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>Stap 1.2: Uw Azure-abonnement-ID ophalen

Start een Azure PowerShell-sessie en aanmelden bij uw Azure-account met behulp van de volgende opdracht uit:

```Powershell
   Connect-AzAccount
```
Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Vervolgens gebruikt u de [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) opdracht:

```powershell
   Get-AzSubscription
```
Zoek in de uitvoer de ID voor het abonnement dat u voor Azure Key Vault gebruiken wilt. U kunt deze abonnements-ID wordt later nodig.

Sluit de Azure PowerShell-venster niet.

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Stap 1.3: Download de BYOK-toolset voor Azure Key Vault

Ga naar het Microsoft Download Center en [download de Azure Key Vault BYOK-hulpmiddelenset](https://www.microsoft.com/download/details.aspx?id=45345) voor uw geografische regio of het exemplaar van Azure. Gebruik de volgende informatie om te identificeren van de naam van het pakket downloaden en de bijbehorende SHA-256-pakket-hash:

- - -
**Verenigde Staten:**

KeyVault-BYOK-Tools-UnitedStates.zip

2E8C00320400430106366A4E8C67B79015524E4EC24A2D3A6DC513CA1823B0D4

- - -
**Europa:**

KeyVault-BYOK-Tools-Europe.zip

9AAA63E2E7F20CF9BB62485868754203721D2F88D300910634A32DFA1FB19E4A

- - -
**Azië:**

KeyVault-BYOK-Tools-AsiaPacific.zip

4BC14059BF0FEC562CA927AF621DF665328F8A13616F44C977388EC7121EF6B5

- - -
**Latijns-Amerika:**

KeyVault-BYOK-Tools-LatinAmerica.zip

E7DFAFF579AFE1B9732C30D6FD80C4D03756642F25A538922DD1B01A4FACB619

- - -
**Japan:**

KeyVault-BYOK-Tools-Japan.zip

3933C13CC6DC06651295ADC482B027AF923A76F1F6BF98B4D4B8E94632DEC7DF

- - -
**Korea:**

KeyVault-BYOK-Tools-Korea.zip

71AB6BCFE06950097C8C18D532A9184BEF52A74BB944B8610DDDA05344ED136F

- - -
**Zuid-Afrika:**

KeyVault-BYOK-Tools-SouthAfrica.zip

C41060C5C0170AAAAD896DA732E31433D14CB9FC83AC3C67766F46D98620784A

- - -
**VAE:**

KeyVault-BYOK-Tools-UAE.zip

FADE80210B06962AA0913EA411DAB977929248C65F365FD953BB9F241D5FC0D3

- - -
**Australië:**

KeyVault-BYOK-Tools-Australia.zip

CD0FB7365053DEF8C35116D7C92D203C64A3D3EE2452A025223EEB166901C40A

- - -
[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Tools-USGovCloud.zip

F8DB2FC914A7360650922391D9AA79FF030FD3048B5795EC83ADC59DB018621A

- - -
**Amerikaanse overheid DOD:**

KeyVault-BYOK-Tools-USGovernmentDoD.zip

A79DD8C6DFFF1B00B91D1812280207A205442B3DDF861B79B8B991BB55C35263

- - -
**Canada:**

KeyVault-BYOK-Tools-Canada.zip

61BE1A1F80AC79912A42DEBBCC42CF87C88C2CE249E271934630885799717C7B

- - -
**Duitsland:**

KeyVault-BYOK-Tools-Germany.zip

5385E615880AAFC02AFD9841F7BADD025D7EE819894AA29ED3C71C3F844C45D6

- - -
**India:**

KeyVault-BYOK-Tools-India.zip

49EDCEB3091CF1DF7B156D5B495A4ADE1CFBA77641134F61B0E0940121C436C8

- - -
**Frankrijk:**

KeyVault-BYOK-Tools-France.zip

5C9D1F3E4125B0C09E9F60897C9AE3A8B4CB0E7D13A14F3EDBD280128F8FE7DF

- - -
**Verenigd Koninkrijk:**

KeyVault-BYOK-Tools-UnitedKingdom.zip

432746BD0D3176B708672CCFF19D6144FCAA9E5EB29BB056489D3782B3B80849

- - -

Voor het valideren van de integriteit van uw gedownloade BYOK-toolset, vanuit de Azure PowerShell-sessie, gebruikt u de [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.

   ```powershell
   Get-FileHash KeyVault-BYOK-Tools-*.zip
   ```

De toolset bevat:

* Een KEK Key Exchange Key ()-pakket met een naam die begint met **BYOK-KEK - pkg-.**
* Een beveiligingswereldpakket met een naam die met begint **BYOK-SecurityWorld - pkg-.**
* Een python-script met de naam **verifykeypackage.py.**
* Een uitvoerbaar opdrachtregelbestand met de naam **KeyTransferRemote.exe** en bijbehorende DLL-bestanden.
* Een Visual C++ Redistributable-pakket, met de naam **vcredist_x64.exe.**

Kopieer het pakket naar een USB-station of ander draagbaar opslagmedium.

## <a name="step-2-prepare-your-disconnected-workstation"></a>Stap 2: Uw niet-verbonden werkstation voorbereiden

Voor deze tweede stap doen in de volgende procedures op het werkstation dat niet is verbonden met een netwerk (Internet of uw interne netwerk).

### <a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Stap 2.1: Voorbereiden van de niet-verbonden werkstation met Thales HSM

De (Thales) nCipher-ondersteuningssoftware installeren op een Windows-computer, en voeg vervolgens een Thales HSM aan die computer.

Zorg ervoor dat de Thales-hulpprogramma's in het pad (**%nfast_home%\bin**). Bijvoorbeeld, typ het volgende:

  ```cmd
  set PATH=%PATH%;"%nfast_home%\bin"
  ```

Zie voor meer informatie de handleiding inbegrepen bij de Thales HSM.

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Stap 2.2: Installeer de BYOK-toolset op niet-verbonden werkstation

Kopieer het pakket met BYOK-toolset van het USB-station of ander draagbaar opslagmedium en doet u het volgende:

1. Pak de bestanden uit het gedownloade pakket naar een map.
2. Voer vcredist_x64.exe uit in die map.
3. Volg de instructies voor de installatie de Visual C++ runtime-onderdelen voor Visual Studio 2013.

## <a name="step-3-generate-your-key"></a>Stap 3: De sleutel genereren

Voor deze derde stap, voer de volgende procedures op de niet-verbonden werkstation. Voor deze stap moet uw HSM zijn in de modus voor de initialisatie. 

### <a name="step-31-change-the-hsm-mode-to-i"></a>Stap 3.1: De HSM-modus wijzigen in 'Ik'

Als u Thales nShield Edge, de modus te wijzigen: 1. Gebruik de knop modus om de vereiste modus te markeren. 2. Binnen een paar seconden en houdt u de knop wissen voor een paar seconden. Als de modus wijzigt, wordt de nieuwe modus LED niet meer knippert en branden blijft. De Status-LED onregelmatig voor een paar seconden mogelijk flash en vervolgens knippert regelmatig wanneer het apparaat gereed is. Anders wordt het apparaat blijft in de huidige modus, met de juiste modus LED belicht.

### <a name="step-32-create-a-security-world"></a>Stap 3.2: Maak een beveiligingswereld

Start een opdrachtprompt en voer het nieuwe-wereld-programma van Thales.

   ```cmd
    new-world.exe --initialize --cipher-suite=DLf3072s256mRijndael --module=1 --acs-quorum=2/3
   ```

Dit programma maakt een **Beveiligingswereld** bestand op % NFAST_KMDATA%\local\world, wat overeenkomt met de map C:\ProgramData\nCipher\Key Management Settings\User. U kunt andere waarden gebruiken voor het quorum, maar in ons voorbeeld u gevraagd drie lege kaarten en pincodes voor elk adres invoeren. Klik, twee kaarten volledige toegang geven tot de beveiligingswereld. Deze kaarten worden de **Beheerderskaartenset** voor de nieuwe beveiligingswereld.

> [!NOTE]
> Als uw HSM biedt geen ondersteuning voor de nieuwere ineens suite DLf3072s256mRijndael, kunt u vervangen--coderingssuite DLf3072s256mRijndael = met--coderingssuite DLf1024s160mRijndael =

Ga daarna als volgt te werk:

* Back-up van het wereldbestand. Beveilig het wereldbestand, de Beheerderskaarten en de bijbehorende pincodes, en zorg ervoor dat niemand toegang tot meer dan één kaart heeft.

### <a name="step-33-change-the-hsm-mode-to-o"></a>Stap 3.3: De HSM-modus te wijzigen ' o '

Als u Thales nShield Edge, de modus te wijzigen: 1. Gebruik de knop modus om de vereiste modus te markeren. 2. Binnen een paar seconden en houdt u de knop wissen voor een paar seconden. Als de modus wijzigt, wordt de nieuwe modus LED niet meer knippert en branden blijft. De Status-LED onregelmatig voor een paar seconden mogelijk flash en vervolgens knippert regelmatig wanneer het apparaat gereed is. Anders wordt het apparaat blijft in de huidige modus, met de juiste modus LED belicht.

### <a name="step-34-validate-the-downloaded-package"></a>Stap 3.4: Het gedownloade pakket valideren

Deze stap is optioneel, maar aanbevolen zodat u het volgende kunt valideren:

* De uitwisselingssleutel van de sleutel die is opgenomen in de hulpmiddelenset is, gegenereerd met een legitieme Thales HSM.
* De hash van de Beveiligingswereld die is opgenomen in de hulpmiddelenset is, gegenereerd met een legitieme Thales HSM.
* De uitwisselingssleutel van de sleutel is niet exporteerbaar maken.

> [!NOTE]
> Voor het valideren van het gedownloade pakket, moet de HSM moet worden verbonden, zijn ingeschakeld, en een beveiligingswereld (zoals de groep die u zojuist hebt gemaakt).

Het gedownloade pakket valideren:

1. Voer het script verifykeypackage.py door een van de volgende, afhankelijk van uw geografische regio of het exemplaar van Azure te typen:

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
   * Voor Zuid-Afrika:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-SA-1 -w BYOK-SecurityWorld-pkg-SA-1
   * Voor VAE:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UAE-1 -w BYOK-SecurityWorld-pkg-UAE-1
   * Voor Australië:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * Voor [Azure Government](https://azure.microsoft.com/features/gov/), waarbij de US government-exemplaar van Azure worden gebruikt:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * Voor de Amerikaanse overheid DOD:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Canada:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Voor Duitsland:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Voor India:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
   * Frankrijk:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-FRANCE-1 -w BYOK-SecurityWorld-pkg-FRANCE-1
   * Voor het Verenigd Koninkrijk:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UK-1 -w BYOK-SecurityWorld-pkg-UK-1

     > [!TIP]
     > De Thales-software bevat python in %NFAST_HOME%\python\bin
     >
     >
2. Bevestig dat u het volgende, waarmee wordt aangegeven validatie is geslaagd: **Resultaat: GESLAAGD**

Met dit script wordt de ondertekenaarsketen tot de Thales-hoofdsleutel gevalideerd. De hash van deze hoofdsleutel is ingesloten in het script en de waarde moet **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. U kunt deze waarde ook afzonderlijk controleren door naar de pagina de [Thales-website](http://www.thalesesec.com/).

U kunt nu een nieuwe sleutel te maken.

### <a name="step-35-create-a-new-key"></a>Stap 3.5: Een nieuwe sleutel maken

Genereer een sleutel met de Thales **generatekey** programma.

Voer de volgende opdracht uit om de sleutel te genereren:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

* De parameter *beveiligen* moet worden ingesteld op de waarde **module**, zoals wordt weergegeven. Hiermee maakt u een modulair beveiligde sleutel. De BYOK-toolset biedt geen ondersteuning voor met OCS beveiligde sleutels.
* Vervang de waarde van *contosokey* voor de **ident** en **plainname** met een string-waarde. Administratieve overhead te minimaliseren en de kans op fouten te verminderen, wordt u aangeraden dat u dezelfde waarde voor beide gebruiken. De **ident** waarde mag alleen cijfers, streepjes en kleine letters.
* De pubexp is leeg (standaard) in dit voorbeeld, maar u kunt specifieke waarden opgeven. Zie de Thales-documentatie voor meer informatie.

Deze opdracht maakt u een Tokenized sleutelbestand in uw map %NFAST_KMDATA%\local met een naam die begint met **key_simple_**, gevolgd door de **ident** die is opgegeven in de opdracht. Bijvoorbeeld: **key_simple_contosokey**. Dit bestand bevat een versleutelde sleutel.

Back-up van deze Tokenized sleutelbestand op een veilige locatie.

> [!IMPORTANT]
> Wanneer u uw sleutel later naar Azure Key Vault overdraagt, exporteren niet Microsoft deze sleutel naar u terug, dus het erg belangrijk is dat u back-up uw sleutel en beveiligingswereld veilig. Neem contact op met Thales voor richtlijnen en aanbevolen procedures voor het back-ups van uw sleutel.
>


U bent nu klaar om over te dragen van uw sleutel naar Azure Key Vault.

## <a name="step-4-prepare-your-key-for-transfer"></a>Stap 4: Uw sleutel voorbereiden voor overdracht

Voer de volgende procedures op de niet-verbonden werkstation voor deze vierde stap.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Stap 4.1: Een kopie van uw sleutel met beperkte machtigingen maken

Open een nieuwe opdrachtprompt en wijzig de huidige map naar de locatie waar u de BYOK-zip-bestand hebt uitgepakt. Als u wilt beperken de machtigingen voor uw sleutel vanaf een opdrachtprompt, voert u een van de volgende, afhankelijk van uw geografische regio of het exemplaar van Azure:

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
* Voor Zuid-Afrika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SA-1
* Voor VAE:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UAE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UAE-1
* Voor Australië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* Voor [Azure Government](https://azure.microsoft.com/features/gov/), waarbij de US government-exemplaar van Azure worden gebruikt:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* Voor de Amerikaanse overheid DOD:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Canada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Voor Duitsland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Voor India:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1
* Frankrijk:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-FRANCE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-FRANCE-1
* Voor het Verenigd Koninkrijk:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1

Wanneer u deze opdracht uitvoert, vervangt *contosokey* door dezelfde waarde als u hebt opgegeven in **stap 3.5: Maak een nieuwe sleutel** uit de [de sleutel genereren](#step-3-generate-your-key) stap.

U wordt gevraagd om uw kaarten security world-beheerder.

Wanneer de opdracht is voltooid, ziet u **resultaat: SUCCES** en de kopie van uw sleutel met beperkte machtigingen zich in het bestand met de naam key_xferacId_<contosokey>.

U kunt inspecteert de ACL's van de volgende opdrachten met de Thales-hulpprogramma's:

* aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Wanneer u deze opdrachten uitvoert, vervangt u contosokey met dezelfde waarde die u hebt opgegeven in **stap 3.5: Maak een nieuwe sleutel** uit de [de sleutel genereren](#step-3-generate-your-key) stap.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>4.2 stap: Uw sleutel versleutelt met behulp van Microsoft uitwisselingssleutel van de sleutel

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
* Voor Zuid-Afrika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor VAE:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UAE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UAE-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Australië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor [Azure Government](https://azure.microsoft.com/features/gov/), waarbij de US government-exemplaar van Azure worden gebruikt:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor de Amerikaanse overheid DOD:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Canada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor Duitsland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor India:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Frankrijk:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-France-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-France-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Voor het Verenigd Koninkrijk:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

* Vervang *contosokey* met de id die u gebruikt voor het genereren van de sleutel in **stap 3.5: Maak een nieuwe sleutel** uit de [de sleutel genereren](#step-3-generate-your-key) stap.
* Vervang *SubscriptionID* met de ID van de Azure-abonnement met uw key vault. U hebt deze waarde hebt opgehaald in **stap 1.2: Uw Azure-abonnement-ID ophalen** uit de [uw met Internet verbonden werkstation voorbereiden](#step-1-prepare-your-internet-connected-workstation) stap.
* Vervang *ContosoFirstHSMKey* met een label dat wordt gebruikt voor de naam van uw uitvoerbestand.

Wanneer dit voltooid is, wordt de pagina **resultaat: SUCCES** en er is een nieuw bestand in de huidige map met de volgende naam: KeyTransferPackage-*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Stap 4.3: De overdracht van de sleutel-pakket kopiëren naar het met Internet verbonden werkstation

Gebruik een USB-station of ander draagbaar opslagmedium om te kopiëren van het uitvoerbestand uit de vorige stap (KeyTransferPackage-ContosoFirstHSMkey.byok) naar uw met Internet verbonden werkstation.

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a>Stap 5: Uw sleutel overdragen naar Azure Key Vault

Voor deze laatste stap op het met Internet verbonden werkstation gebruiken de [toevoegen AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet voor het uploaden van het pakket voor sleuteloverdracht die u hebt gekopieerd uit de niet-verbonden werkstation naar de Azure Key Vault HSM:

   ```powershell
        Add-AzKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'
   ```

Als de upload voltooid is, ziet u de eigenschappen van de sleutel die u zojuist hebt toegevoegd wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen

U kunt nu deze met HSM beveiligde sleutel gebruiken in uw key vault. Zie voor meer informatie de **als u wilt gebruiken van een hardware security module (HSM)** sectie de [aan de slag met Azure Key Vault](key-vault-overview.md) zelfstudie.
