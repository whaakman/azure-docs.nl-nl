---
title: Aanpassen van de beveiligingsconfiguraties OS in Azure Security Center (Preview) | Microsoft Docs
description: In dit artikel ziet u hoe u security center-evaluaties aanpassen
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
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: c0c37724e61490c8c33b5e2d37879549bbc6d7ce
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402113"
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>Aanpassen van de beveiligingsconfiguraties OS in Azure Security Center (Preview)

In dit scenario laat zien hoe het aanpassen van OS-configuratie worden uitgevoerd in Azure Security Center.

## <a name="what-are-os-security-configurations"></a>Wat zijn de OS-beveiligingsconfiguraties?

Azure Security Center bewaakt beveiligingsconfiguraties door het toepassen van een set [meer dan 150 aanbevolen regels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor het beperken van het besturingssysteem, met inbegrip van regels die betrekking hebben op firewalls, controle, wachtwoordbeleid en meer. Als een virtuele machine hebben een kwetsbare configuratie wordt gevonden, wordt door Security Center een beveiligingsaanbeveling gegenereerd.

Aanpassen van de regels, kunnen organisaties bepalen welke opties zijn meer geschikt is voor hun omgeving. U kunt een evaluatie van aangepaste beleid instellen en vervolgens toepassen op alle computers die van toepassing in het abonnement.

> [!NOTE]
> - Aanpassing van de OS-beveiligingsconfiguratie is momenteel beschikbaar voor Windows Server 2008, 2008 R2, 2012, 2012 R2 en 2016-versies alleen besturingssystemen.
> - De configuratie is van toepassing op alle virtuele machines en computers die zijn verbonden met alle werkruimten onder het geselecteerde abonnement.
> - Aanpassing van het besturingssysteem beveiliging-configuratie is alleen beschikbaar op de prijscategorie standard van Security Center.
>
>

U kunt de regels voor beveiligingsconfiguratie OS aanpassen door het inschakelen en uitschakelen van een specifieke regel, de gewenste instelling voor een bestaande regel te wijzigen of toevoegen van een nieuwe regel die gebaseerd op de ondersteunde typen (register, beleid voor beveiligingscontrole en beveiligingsbeleid). De gewenste instelling moet op dit moment een exacte waarde.

Nieuwe regels moeten zich in dezelfde indeling en dezelfde structuur als andere bestaande regels die van hetzelfde type.

> [!NOTE]
> Voor het aanpassen van OS-beveiligingsconfiguraties, moet u zijn toegewezen de rol van *Abonnementhouder*, *Inzender*, of *beveiligingsbeheerder*.
>
>

## <a name="customize-the-default-os-security-configuration"></a>De standaard OS-beveiligingsconfiguratie aanpassen

Voor het aanpassen van de standaard OS-beveiligingsconfiguratie in Security Center, het volgende doen:

1.  Open het dashboard van **Security Center**.

2.  Selecteer in het linkerdeelvenster **beveiligingsbeleid**.      

    ![Lijst met beveiligingsbeleid](media/security-center-customize-os-security-config/manual-provision.png)

3.  In de rij van het abonnement dat u wilt aanpassen, klikt u op **instellingen bewerken**.

4. Selecteer **beveiligingsconfiguraties bewerken**.  

    ![Het venster 'Beveiligingsconfiguraties bewerken'](media/security-center-customize-os-security-config/blade.png)

5. Volg de stappen om te downloaden, bewerken en het gewijzigde bestand uploaden.

   > [!NOTE]
   > Het configuratiebestand dat u hebt gedownload is standaard *json* indeling. Ga voor instructies over het wijzigen van dit bestand naar [het configuratiebestand aanpassen](#customize-the-configuration-file).
   >

6. Selecteer voor het doorvoeren van de wijziging, **opslaan**. Anders wordt wordt het beleid niet opgeslagen.

    ![De knop Opslaan](media/security-center-customize-os-security-config/save-successfully.png)

   Nadat u het bestand hebt opgeslagen, wordt de configuratie wordt toegepast op alle virtuele machines en computers die zijn verbonden met de werkruimten onder het abonnement. Het proces is meestal duurt een paar minuten maar duurt langer, afhankelijk van de grootte van de infrastructuur.

U kunt op elk gewenst moment de configuratie van het huidige beleid opnieuw op de standaardstatus heeft. Om dit te doen in de **regels voor beveiligingsconfiguratie bewerken OS** venster **opnieuw**. Deze optie om te bevestigen **Ja** in het pop-upvenster bevestiging.

![Het bevestigingsvenster opnieuw instellen](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Het configuratiebestand aanpassen

In het aanpassingsbestand heeft elke ondersteunde versie van het besturingssysteem een set regels of ruleset. Elke ruleset heeft een eigen naam en de unieke ID, zoals aangegeven in het volgende voorbeeld:

![Het ruleset-naam en ID](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> In dit voorbeeld van een bestand is bewerkt in Visual Studio, maar u kunt ook Kladblok gebruiken als u de JSON-Viewer invoegtoepassing geïnstalleerd hebt.
>
>

Wanneer u de aanpassingsbestand hebt bewerkt, kunt u een regel of alle mappen wijzigen. Elke ruleset bevat een *regels* sectie die wordt onderverdeeld in drie categorieën: Register, beleid voor beveiligingscontrole en beveiligingsbeleid, zoals hier wordt weergegeven:

![Drie ruleset-categorieën](media/security-center-customize-os-security-config/rules-section.png)

Elke categorie heeft een eigen set kenmerken. U kunt de volgende kenmerken wijzigen:

- **expectedValue**: Het gegevenstype veld van dit kenmerk moet overeenkomen met de ondersteunde waarden per *regeltype*, bijvoorbeeld:

  - **baselineRegistryRules**: De waarde moet overeenkomen met de [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) die gedefinieerd in die regel.

  - **baselineAuditPolicyRules**: Gebruik een van de volgende tekenreekswaarden:

    - *Geslaagd en mislukt*

    - *Geslaagd*

  - **baselineSecurityPolicyRules**: Gebruik een van de volgende tekenreekswaarden:

    - *Niemand*

    - Lijst met toegestane gebruikersgroepen, bijvoorbeeld: *Beheerders*, *back-upoperators*

-   **status**: De tekenreeks mag de opties *uitgeschakelde* of *ingeschakeld*. Voor deze release zijn de tekenreeks is hoofdlettergevoelig.

Dit zijn alleen de velden die kunnen worden geconfigureerd. Als u de indeling of de grootte schenden, kunt u zich niet de wijziging op te slaan. U ontvangt een foutmelding weergegeven die u moet een geldig JSON-configuratiebestand uploaden.

Zie voor een lijst van andere mogelijke fouten, [foutcodes](#error-codes).

De volgende drie secties bevatten voorbeelden van de vorige regels. De *expectedValue* en *status* kenmerken kunnen worden gewijzigd.

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

Sommige regels worden voor de verschillende typen besturingssystemen gedupliceerd. Dubbele regels hebben dezelfde *originalId* kenmerk.

## <a name="create-custom-rules"></a>Aangepaste regels maken

U kunt ook nieuwe regels maken. Voordat u een nieuwe regel maakt, houd rekening met de volgende beperkingen:

-   Schemaversie, *baselineId* en *baselineName* kan niet worden gewijzigd.

-   RuleSet kan niet worden verwijderd.

-   RuleSet kan niet worden toegevoegd.

-   Het maximum aantal regels dat is toegestaan (met inbegrip van standaardregels) is 1000.

Nieuwe aangepaste regels worden gemarkeerd met een nieuwe aangepaste bron (! = "Microsoft"). De *ruleId* veld kan niet null of leeg zijn. Als deze leeg is, genereert Microsoft een. Als deze niet leeg is, moet een geldige GUID die is uniek voor alle regels (standaard en aangepaste) hebben. Bekijk de volgende beperkingen voor de velden core:

-   **originalId**: Kan niet null of leeg zijn. Als *originalId* is niet leeg is, moet dit een geldige GUID zijn.

-   **cceId**: Kan niet null of leeg zijn. Als *cceId* is niet leeg is, deze moet uniek zijn.

-   **ruleType**: (Selecteer een optie) register, AuditPolicy of SecurityPolicy.

-   **Ernst**: (Selecteer een optie) onbekend, kritiek, waarschuwing of ter informatie.

-   **analyzeOperation**: Moet *gelijk is aan*.

-   **auditPolicyId**: Moet een geldige GUID zijn.

-   **regValueType**: (Selecteer een optie) Int, Long, String en MultipleString.

> [!NOTE]
> Hive moet *LocalMachine /*.
>
>

Voorbeeld van een nieuwe aangepaste regel:

**Register**:
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
**Beveiligingsbeleid**:
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
**Controlebeleid**:
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

## <a name="file-upload-failures"></a>Fouten bij het uploaden van bestand

Als het opgegeven configuratiebestand ongeldig vanwege fouten in waarden of de opmaak is, een fout wordt weergegeven, zoals **Opslagactie is mislukt**. U kunt een rapport met gedetailleerde fouten CSV te herstellen en corrigeer de fouten voordat u een gecorrigeerde configuratiebestand opnieuw downloaden.

Voorbeeld van een fout bij het bestand:

![Voorbeeld van een fout](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Foutcodes

Alle mogelijke fouten worden weergegeven in de volgende tabel:

| **Fout**                                | **Beschrijving**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfigurationSchemaVersionError  | De eigenschap *schemaVersion* is ongeldig of leeg zijn gevonden. De waarde moet worden ingesteld op *{0}*.                                                         |
| BaselineInvalidStringError               | De eigenschap *{0}* mag niet  *\\n*.                                                                                                         |
| BaselineNullRuleError                    | Lijst met de regels van de basislijn-configuratie bevat een regel met de waarde *null*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | De CCE-ID *{0}* is niet uniek.                                                                                                                  |
| BaselineRuleEmptyProperty                | De eigenschap *{0}* is ongeldig of ontbreekt.                                                                                                       |
| BaselineRuleIdNotInDefault               | De regel is een broneigenschap *Microsoft* , maar het is niet gevonden in de regelset van de standaard Microsoft.                                                   |
| BaselineRuleIdNotUniqueError             | De regel-ID is niet uniek.                                                                                                                       |
| BaselineRuleInvalidGuid                  | De eigenschap *{0}* ongeldig is gevonden. De waarde is geen geldige GUID.                                                                             |
| BaselineRuleInvalidHive                  | De component moet LocalMachine /.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Naam van de regel is niet uniek.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | De regel is niet gevonden in de nieuwe configuratie. Regel kan niet worden verwijderd.                                                                     |
| BaselineRuleNotFoundError                | De regel is niet gevonden in standaard basislijn ruleset.                                                                                        |
| BaselineRuleNotInPlace                   | De regel komt overeen met een standaard-regel met het type {0} en wordt vermeld in {1} lijst.                                                                       |
| BaselineRulePropertyTooLong              | De eigenschap *{0}* is te lang. Maximaal toegestane lengte: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | De verwachte waarde *{0}* komt niet overeen met het type van registerwaarde die is gedefinieerd.                                                              |
| BaselineRulesetAdded                     | Het ruleset met de ID *{0}* is niet gevonden in de standaardconfiguratie. Het ruleset kan niet worden toegevoegd.                                               |
| BaselineRulesetIdMustBeUnique            | Het ruleset basislijn *{0}* moet uniek zijn.                                                                                           |
| BaselineRulesetNotFound                  | Het ruleset met id *{0}* en de naam *{1}* is niet gevonden in de opgegeven configuratie. Het ruleset kan niet worden verwijderd.                                |
| BaselineRuleSourceNotMatch               | De regel met de ID *{0}* is al gedefinieerd.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Het regeltype is *{0}*.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Het werkelijke type van de regel is *{0}*, maar de *ruleType* eigenschap *{1}*.                                                                          |
| BaselineRuleUnpermittedChangesError      | Alleen *expectedValue* en *status* eigenschappen kunnen worden gewijzigd.                                                                       |
| BaselineTooManyRules                     | Het maximum aantal toegestane aangepaste regels is {0} regels. De opgegeven configuratie bevat {1} regels, {2} standaardregels, en {3} aangepaste regels. |
| ErrorNoConfigurationStatus               | Er is geen configuratie-status is gevonden. Status van de van gewenste configuratiestatus: *Standaard* of *aangepaste*.                                    |
| ErrorNonEmptyRulesetOnDefault            | De status van de configuratie is ingesteld op standaard. De *BaselineRulesets* lijst moet null of leeg zijn.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | De status van de opgegeven configuratie *aangepaste* , maar de *baselineRulesets* eigenschap is null of leeg zijn.                                             |
| ErrorParsingBaselineConfig               | De opgegeven configuratie is ongeldig. Een of meer van de gedefinieerde waarden hebben een null-waarde of een ongeldig type.                                  |
| ErrorParsingIsDefaultProperty            | De opgegeven *configurationStatus* waarde *{0}* is ongeldig. De waarde mag alleen *standaard* of *aangepaste*.                                         |
| InCompatibleViewVersion                  | De weergaveversie *{0}* is *niet* ondersteund op dit werkruimtetype.                                                                                   |
| InvalidBaselineConfigurationGeneralError | De configuratie van de basislijn is gevonden met een of meer validatiefouten van het type.                                                          |
| ViewConversionError                      | De weergave is een oudere versie dan de werkruimte wordt ondersteund. Weergeven van de conversie is mislukt: {0}.                                                                 |

Als u niet voldoende machtigingen hebt, krijgt u mogelijk een algemene fout is opgetreden, zoals hier wordt weergegeven:

!['Opslaan actie is mislukt"foutbericht](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel besproken over het aanpassen van OS-configuratie worden uitgevoerd in Security Center. Zie voor meer informatie over de van configuratieregels en herstel:

- [Security Center common configuration identifiers en baseline rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Unieke id's toewijzen aan configuratieregels gebruikt Security Center common configuration enumeration (CCE). Zie voor meer informatie, [CCE](https://nvd.nist.gov/config/cce/index).
- Raadpleeg beveiligingsproblemen oplossen wanneer de configuratie van het besturingssysteem komt niet overeen met de aanbevolen configuratie van beveiligingsregels [beveiligingsconfiguraties herstellen](security-center-remediate-os-vulnerabilities.md).
