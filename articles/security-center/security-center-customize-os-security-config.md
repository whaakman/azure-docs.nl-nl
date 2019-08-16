---
title: Beveiligings configuraties van het besturings systeem aanpassen in Azure Security Center (preview) | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Security Center-evaluaties aanpast
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2019
ms.author: v-mohabe
ms.openlocfilehash: df4f51f97798048b28c0193dbc61e07fc55c9adc
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69535070"
---
# <a name="customize-os-security-configurations-in-azure-security-center-retired"></a>Beveiligings configuraties van besturings systemen in Azure Security Center aanpassen (buiten gebruik gesteld)

In deze walkthrough wordt gedemonstreerd hoe u de evaluatie van de beveiligings configuratie van het besturings systeem (preview) kunt aanpassen in Azure Security Center.

> [!NOTE]
> De mogelijkheid om de beveiligings configuraties van het besturings systeem (preview-functie) aan te passen, is op 31 juli 2019 buiten gebruik gesteld. Zie voor meer informatie en alternatieve Services de [buiten gebruiks telling van Security Center-functies (2019 juli)](security-center-features-retirement-july2019.md#menu_securityconfigurations).

## <a name="what-are-os-security-configurations"></a>Wat zijn de beveiligings configuraties van het besturings systeem?

Azure Security Center controleert beveiligingsconfiguraties door een set van [meer dan 150 aanbevolen regels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) toe te passen voor beveiliging van het besturingssysteem, met inbegrip van regels voor firewalls, controle, wachtwoordbeleid en meer. Als er wordt geconstateerd dat een computer een kwetsbare configuratie heeft, wordt er door Security Center een beveiligingsaanbeveling gegenereerd.

Door de regels aan te passen, kunnen organisaties bepalen welke configuratie opties geschikter zijn voor hun omgeving. U kunt een aangepast beoordelings beleid instellen en dit vervolgens Toep assen op alle toepasselijke computers in het abonnement.

> [!NOTE]
> - Momenteel is aanpassing van de besturingssysteem beveiligings configuratie alleen beschikbaar voor de besturings systemen Windows Server versie 2008, 2008 R2, 2012, 2012 R2 en 2016.
> - De configuratie is van toepassing op alle Vm's en computers die zijn verbonden met alle werk ruimten onder het geselecteerde abonnement.
> - De aanpassing van de besturingssysteem beveiligings configuratie is alleen beschikbaar op de laag Security Center Standard.
>
>

U kunt de regels voor de beveiligings configuratie van het besturings systeem aanpassen door een specifieke regel in te scha kelen, de gewenste instelling voor een bestaande regel te wijzigen of een nieuwe regel toe te voegen die is gebaseerd op de ondersteunde regel typen (REGI ster, controle beleid en beveiligings beleid). Op dit moment moet de gewenste instelling een exacte waarde zijn.

Nieuwe regels moeten dezelfde indeling en structuur hebben als andere bestaande regels van hetzelfde type.

> [!NOTE]
> Als u beveiligings configuraties voor het besturings systeem wilt aanpassen, moet u de rol van *abonnements eigenaar*, *abonnements bijdrager*of *beveiligings beheerder*toewijzen.
>
>

## <a name="customize-the-default-os-security-configuration"></a>De standaard beveiligings configuratie van het besturings systeem aanpassen

Ga als volgt te werk om de standaard beveiligings configuratie van het besturings systeem in Security Center aan te passen:

1.  Open het dashboard van **Security Center**.

2.  Selecteer in het linkerdeel venster de optie **prijzen & instellingen**.

    ![Lijst met beveiligings beleid](media/security-center-customize-os-security-config/manual-provision.png)

4. Selecteer het betreffende abonnement en selecteer **beveiligings configuraties bewerken**.  

    ![Het venster beveiligings configuraties bewerken](media/security-center-customize-os-security-config/blade.png)

5. Volg de stappen om het gewijzigde bestand te downloaden, te bewerken en te uploaden.

   > [!NOTE]
   > Standaard wordt het configuratie bestand dat u hebt gedownload in *JSON* -indeling. Ga voor instructies over het wijzigen van dit bestand naar [het aanpassen van het configuratie bestand](#customize-the-configuration-file).
   >

6. Selecteer **Opslaan**om de wijziging door te voeren. Anders wordt het beleid niet opgeslagen.

    ![De knop Opslaan](media/security-center-customize-os-security-config/save-successfully.png)

   Nadat u het bestand hebt opgeslagen, wordt de configuratie toegepast op alle Vm's en computers die zijn verbonden met de werk ruimten onder het abonnement. Het proces duurt doorgaans enkele minuten, maar kan langer duren, afhankelijk van de grootte van de infra structuur.

U kunt op elk gewenst moment de huidige beleids configuratie opnieuw instellen op de standaard status. Hiertoe selecteert u **opnieuw instellen**in het venster **regels voor beveiligings configuratie van besturings systeem bewerken** . Bevestig deze optie door **Ja** te selecteren in het pop-upvenster met de bevestiging.

![Het bevestigings venster opnieuw instellen](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Het configuratie bestand aanpassen

In het aanpassings bestand heeft elke ondersteunde versie van het besturings systeem een set regels, of ruleset. Elke ruleset heeft een eigen naam en unieke ID, zoals wordt weer gegeven in het volgende voor beeld:

![De ruleset-naam en-ID](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Dit voorbeeld bestand is bewerkt in Visual Studio, maar u kunt ook Klad blok gebruiken als u de JSON-Viewer-invoeg toepassing hebt geïnstalleerd.
>
>

Wanneer u het aanpassings bestand bewerkt, kunt u een of meer regels wijzigen. Elke ruleset bevat een *regel* sectie die is onderverdeeld in drie categorieën: REGI ster, controle beleid en beveiligings beleid, zoals hier wordt weer gegeven:

![Drie categorieën regelset](media/security-center-customize-os-security-config/rules-section.png)

Elke categorie heeft een eigen set kenmerken. U kunt de volgende kenmerken wijzigen:

- **expectedValue**: Het veld gegevens type van dit kenmerk moet overeenkomen met de ondersteunde waarden per *regel type*, bijvoorbeeld:

  - **baselineRegistryRules**: De waarde moet overeenkomen met de [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) die in die regel is gedefinieerd.

  - **baselineAuditPolicyRules**: Gebruik een van de volgende teken reeks waarden:

    - *Geslaagd en mislukt*

    - *Geleverd*

  - **baselineSecurityPolicyRules**: Gebruik een van de volgende teken reeks waarden:

    - *Niemand*

    - Lijst met toegestane gebruikers groepen, bijvoorbeeld: *Beheerders*, *back-* upoperators

-   **status**: De teken reeks kan de opties *uitgeschakeld* of *ingeschakeld*bevatten. De teken reeks voor deze versie is hoofdletter gevoelig.

Dit zijn de enige velden die kunnen worden geconfigureerd. Als u de bestands indeling of de grootte schendt, kunt u de wijziging niet opslaan. U krijgt een fout melding dat u een geldig JSON-configuratie bestand moet uploaden.

Zie [fout codes](#error-codes)voor een lijst met andere mogelijke fouten.

De volgende drie secties bevatten voor beelden van de voor gaande regels. De kenmerken *expectedValue* en *State* kunnen worden gewijzigd.

**baselineRegistryRules**
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

    }
```

**baselineAuditPolicyRules**
```json
    {
    "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
    "ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
    "originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
    "cceId": "CCE-11001-5",
    "ruleName": "Audit Policy: Account Management: Other Account Management Events",
    "ruleType": "AuditPolicy",
    "expectedValue": "Success and Failure",
    "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

**baselineSecurityPolicyRules**
```json
    {
    "sectionName": "Privilege Rights",
    "settingName": "SeIncreaseWorkingSetPrivilege",
    "ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
    "originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
    "cceId": "CCE-10548-6",
    "ruleName": "Increase a process working set",
    "ruleType": "SecurityPolicy",
    "expectedValue": "Administrators, Local Service",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

Er zijn enkele regels gedupliceerd voor de verschillende typen besturings systemen. Dubbele regels hebben hetzelfde *originalId* -kenmerk.

## <a name="create-custom-rules"></a>Aangepaste regels maken

U kunt ook nieuwe regels maken. Houd bij het maken van een nieuwe regel de volgende beperkingen in acht:

-   Schema versie, *baselineId* en *baselinenaam* kunnen niet worden gewijzigd.

-   RuleSet kan niet worden verwijderd.

-   De ruleset kan niet worden toegevoegd.

-   Het maximum aantal regels dat is toegestaan (met inbegrip van de standaard regels) is 1000.

Nieuwe aangepaste regels worden gemarkeerd met een nieuwe aangepaste bron (! = "micro soft"). Het veld *ruleId* kan null of leeg zijn. Als deze leeg is, wordt er een gegenereerd. Als deze niet leeg is, moet het een geldige GUID hebben die uniek is voor alle regels (standaard en aangepast). Bekijk de volgende beperkingen voor de belangrijkste velden:

-   **originalId**: Kan null of leeg zijn. Als *originalId* niet leeg is, moet dit een geldige GUID zijn.

-   **cceId**: Kan null of leeg zijn. Als *cceId* niet leeg is, moet het uniek zijn.

-   **ruleType**: (Selecteer één) REGI ster, Auditpolicy of SecurityPolicy.

-   **Ernst**: (Selecteer één) onbekend, kritiek, waarschuwing of informatief.

-   **analyzeOperation**: Moet *gelijk zijn aan*.

-   **auditPolicyId**: Moet een geldige GUID zijn.

-   **regValueType**: (Selecteer één) int, Long, String of MultipleString.

> [!NOTE]
> Hive moet *LocalMachine*zijn.
>
>

Voor beeld van een nieuwe aangepaste regel:

**REGI ster**:
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule", "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Beveiligings beleid**:
```json
   {
   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Controle beleid**:
```json
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Fouten bij uploaden van bestanden

Als het ingediende configuratie bestand ongeldig is vanwege fouten in waarden of opmaak, wordt een fout bericht weer gegeven, zoals de **actie opslaan is mislukt**. U kunt een gedetailleerd rapport met fouten. csv downloaden om de fouten te herstellen en op te lossen voordat u een gecorrigeerd configuratie bestand opnieuw verzendt.

Voor beeld van een fout bestand:

![Voor beeld van fout bestand](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Foutcodes

Alle mogelijke fouten worden weer gegeven in de volgende tabel:

| **Optreedt**                                | **Beschrijving**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfigurationSchemaVersionError  | De eigenschap *schemaVersion* is ongeldig of leeg. De waarde moet worden ingesteld op *{0}* .                                                         |
| BaselineInvalidStringError               | De eigenschap *{0}* mag geen  *\\n*bevatten.                                                                                                         |
| BaselineNullRuleError                    | De lijst basislijn configuratie regels bevat een regel met de waarde *Null*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | De CCE-id *{0}* is niet uniek.                                                                                                                  |
| BaselineRuleEmptyProperty                | De eigenschap *{0}* ontbreekt of is ongeldig.                                                                                                       |
| BaselineRuleIdNotInDefault               | De regel heeft de eigenschap source van *micro soft* maar deze is niet gevonden in de micro soft-standaard-regelset.                                                   |
| BaselineRuleIdNotUniqueError             | De regel-id is niet uniek.                                                                                                                       |
| BaselineRuleInvalidGuid                  | De eigenschap *{0}* is ongeldig. De waarde is geen geldige GUID.                                                                             |
| BaselineRuleInvalidHive                  | De Hive moet LocalMachine zijn.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | De regel naam is niet uniek.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | De regel is niet gevonden in de nieuwe configuratie. De regel kan niet worden verwijderd.                                                                     |
| BaselineRuleNotFoundError                | De regel is niet gevonden in de standaardsorteervolgorde van de basis lijn.                                                                                        |
| BaselineRuleNotInPlace                   | De regel komt overeen met een standaard regel {0} van het type en {1} wordt weer gegeven in de lijst.                                                                       |
| BaselineRulePropertyTooLong              | De eigenschap *{0}* is te lang. Maxi maal toegestane lengte {1}:.                                                                                        |
| BaselineRuleRegTypeInvalidError          | De verwachte waarde *{0}* komt niet overeen met het type register waarde dat is gedefinieerd.                                                              |
| BaselineRulesetAdded                     | De ruleset met de *{0}* id is niet gevonden in de standaard configuratie. De ruleset kan niet worden toegevoegd.                                               |
| BaselineRulesetIdMustBeUnique            | De opgegeven regelset *{0}* van de basislijn naam moet uniek zijn.                                                                                           |
| BaselineRulesetNotFound                  | De ruleset met *{0}* id en *{1}* naam is niet gevonden in de opgegeven configuratie. De ruleset kan niet worden verwijderd.                                |
| BaselineRuleSourceNotMatch               | De regel met de *{0}* id is al gedefinieerd.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Het standaard regel type is *{0}* .                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Het werkelijke type van de regel is *{0}* , maar de eigenschap *ruleType* is *{1}* .                                                                          |
| BaselineRuleUnpermittedChangesError      | Alleen *expectedValue* en *status* eigenschappen mogen worden gewijzigd.                                                                       |
| BaselineTooManyRules                     | Het Maxi maal toegestane aantal aangepaste regels is {0} regels. De opgegeven configuratie bevat {1} regels, {2} standaard regels en {3} aangepaste regels. |
| ErrorNoConfigurationStatus               | Er is geen configuratie status gevonden. Status van de gewenste configuratie status: *Standaard* of *aangepast*.                                    |
| ErrorNonEmptyRulesetOnDefault            | De configuratie status is ingesteld op standaard. De *BaselineRulesets* -lijst moet null of leeg zijn.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | De opgegeven configuratie status is *aangepast* , maar de eigenschap *baselineRulesets* is null of leeg.                                             |
| ErrorParsingBaselineConfig               | De opgegeven configuratie is ongeldig. Een of meer gedefinieerde waarden hebben een null-waarde of een ongeldig type.                                  |
| ErrorParsingIsDefaultProperty            | De opgegeven *configurationStatus* - *{0}* waarde is ongeldig. De waarde kan alleen *standaard* of *aangepast*zijn.                                         |
| InCompatibleViewVersion                  | De weergave versie *{0}* wordt *niet* ondersteund voor dit type werk ruimte.                                                                                   |
| InvalidBaselineConfigurationGeneralError | De opgegeven basislijn configuratie is gevonden met een of meer type validatie fouten.                                                          |
| ViewConversionError                      | De weer gave is een oudere versie dan de werk ruimte ondersteunt. Conversie van weer gave {0}is mislukt:.                                                                 |

Als u niet voldoende machtigingen hebt, krijgt u mogelijk een algemene fout fout, zoals hier wordt weer gegeven:

![Fout bericht ' actie opslaan is mislukt '](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt beschreven hoe u evaluaties van de besturingssysteem beveiliging kunt aanpassen in Security Center. Zie voor meer informatie over de configuratie regels en herstel:

- [Algemene configuratie-id's en basislijn regels Security Center](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Security Center maakt gebruik van algemene configuratie-inventarisatie (CCE) om unieke id's toe te wijzen aan configuratie regels. Zie [CCE](https://nvd.nist.gov/config/cce/index)voor meer informatie.
- Zie [beveiligings configuraties herstellen](security-center-virtual-machine-protection.md)voor meer informatie over het oplossen van beveiligings problemen wanneer de configuratie van uw besturings systeem niet overeenkomt met de aanbevolen beveiligings configuratie regels.
