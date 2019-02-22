---
title: Databaseweergaven in Azure Blockchain Workbench
description: Overzicht van Azure Blockchain Workbench SQL DB-databaseweergaven.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 02/21/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 06b7fb678bc79203589cfa75e8afb457d6ed344f
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594318"
---
# <a name="database-views-in-azure-blockchain-workbench"></a>Databaseweergaven in Azure Blockchain Workbench

Azure Blockchain Workbench levert gegevens van gedistribueerde grootboeken naar een *buiten de chain* SQL DB-database. De database buiten de chain maakt het mogelijk het gebruik van SQL en bestaande hulpprogramma's, zoals [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017), om te communiceren met blockchain-gegevens.

Azure Blockchain Workbench biedt een set databaseweergaven die toegang tot gegevens die nuttig zijn bieden bij het uitvoeren van uw query's. Deze weergaven zijn sterk gedenormaliseerde om gemakkelijk snel gestart het samenstellen van rapporten, analyses, en anders blockchain-gegevens met bestaande programma's en zonder opnieuw trainen van medewerkers van de database te gebruiken.

Deze sectie bevat een overzicht van de databaseweergaven en de gegevens die ze bevatten.

> [!NOTE]
> Direct voor het gebruik van de database-tabellen gevonden in de database buiten deze weergaven, tijdens het mogelijk is, wordt niet ondersteund.
>

## <a name="vwapplication"></a>vwApplication

In deze weergave biedt details over **toepassingen** die zijn geüpload naar Azure Blockchain Workbench.

| Name                             | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                                                   |
|----------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                    | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName                  | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDescription           | nvarchar(255) | Ja         | Een beschrijving van de toepassing |
| ApplicationDisplayName           | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled               | bits           | Nee          | Wordt geïdentificeerd als de toepassing is momenteel ingeschakeld.<br /> **Opmerking:** Hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten blijven op de blockchain en gegevens over deze opdrachten niet uit de database. |
| UploadedDtTm                     | datetime2(7)  | Nee          | De datum en tijd die een contract is geüpload |
| UploadedByUserId                 | int           | Nee          | De ID van de gebruiker die de toepassing uploaden |
| UploadedByUserExternalId         | nvarchar(255) | Nee          | De externe id voor de gebruiker die de toepassing wordt geüpload. Deze ID is standaard de gebruiker van de Azure Active Directory voor het consortium.                                                                                                |
| UploadedByUserProvisioningStatus | int           | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor de gebruiker. Mogelijke waarden zijn: <br />0: gebruiker is gemaakt door de API<br />1: een sleutel is gekoppeld aan de gebruiker in de database<br />2: de gebruiker is volledig is ingericht                         |
| UploadedByUserFirstName          | nvarchar(50)  | Ja         | De voornaam van de gebruiker die het contract geüpload |
| UploadedByUserLastName           | nvarchar(50)  | Ja         | De achternaam van de gebruiker die het contract geüpload |
| UploadedByUserEmailAddress       | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die het contract geüpload |

## <a name="vwapplicationrole"></a>vwApplicationRole

In deze weergave biedt details over de functies die zijn gedefinieerd in toepassingen met Azure Blockchain Workbench.

In een *Asset Transfer* toepassing, bijvoorbeeld functies zoals *koper* en *verkoper* rollen kunnen worden gedefinieerd.

| Name                   | Type             | Kan niet null zijn | Description                                       |
|------------------------|------------------|-------------|---------------------------------------------------|
| ApplicationId          | int              | Nee          | Een unieke id voor de toepassing           |
| ApplicationName        | nvarchar(50)     | Nee          | De naam van de toepassing                       |
| ApplicationDescription | nvarchar(255)    | Ja         | Een beschrijving van de toepassing                  |
| ApplicationDisplayName | nvarchar(255)    | Nee          | De naam moet worden weergegeven in een gebruikersinterface      |
| RoleId                 | int              | Nee          | Een unieke id voor een rol in de toepassing |
| RoleName               | nvarchar50)      | Nee          | De naam van de rol                              |
| RoleDescription        | Description(255) | Ja         | Een beschrijving van de rol                         |

## <a name="vwapplicationroleuser"></a>vwApplicationRoleUser

In deze weergave biedt details over de functies die zijn gedefinieerd in Azure Blockchain Workbench toepassingen en de gebruikers die zijn gekoppeld aan deze.

In een *Asset Transfer* toepassing, bijvoorbeeld *John Smith* kan worden gekoppeld aan de *koper* rol.

| Name                       | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                           |
|----------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId              | int           | Nee          | Een unieke id voor de toepassing                                                                                                                                                                                               |
| ApplicationName            | nvarchar(50)  | Nee          | De naam van de toepassing                                                                                                                                                                                                           |
| ApplicationDescription     | nvarchar(255) | Ja         | Een beschrijving van de toepassing                                                                                                                                                                                                      |
| ApplicationDisplayName     | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface                                                                                                                                                                                          |
| ApplicationRoleId          | int           | Nee          | Een unieke id voor een rol in de toepassing                                                                                                                                                                                     |
| ApplicationRoleName        | nvarchar50)   | Nee          | De naam van de rol                                                                                                                                                                                                                  |
| ApplicationRoleDescription | nvarchar(255) | Ja         | Een beschrijving van de rol                                                                                                                                                                                                             |
| UserId                     | int           | Nee          | De ID van de gebruiker die is gekoppeld aan de rol |
| UserExternalId             | nvarchar(255) | Nee          | De externe id voor de gebruiker die gekoppeld aan de rol is. Deze ID is standaard de gebruiker van de Azure Active Directory voor het consortium.                                                                     |
| UserProvisioningStatus     | int           | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor de gebruiker. Mogelijke waarden zijn: <br />0: gebruiker is gemaakt door de API<br />1: een sleutel is gekoppeld aan de gebruiker in de database<br />2: de gebruiker is volledig is ingericht |
| UserFirstName              | nvarchar(50)  | Ja         | De voornaam van de gebruiker die gekoppeld aan de rol is |
| UserLastName               | nvarchar(255) | Ja         | De achternaam van de gebruiker die gekoppeld aan de rol is |
| UserEmailAddress           | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die gekoppeld aan de rol is |

## <a name="vwconnectionuser"></a>vwConnectionUser

Deze weergave bevat details over de verbindingen die zijn gedefinieerd in Azure Blockchain Workbench en de gebruikers die zijn gekoppeld aan deze. In deze weergave bevat de volgende gegevens voor elke verbinding:

-   Details van de gekoppelde grootboek
-   Informatie van de gebruiker is gekoppeld

| Name                     | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                           |
|--------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ConnectionId             | int           | Nee          | De unieke id voor een verbinding maakt in Azure Blockchain Workbench |
| ConnectionEndpointUrl    | nvarchar(50)  | Nee          | De eindpunt-url voor een verbinding |
| ConnectionFundingAccount | nvarchar(255) | Ja         | De financiering account dat is gekoppeld aan een verbinding, indien van toepassing |
| LedgerId                 | int           | Nee          | De unieke id voor een grootboek |
| LedgerName               | nvarchar(50)  | Nee          | De naam van het grootboek |
| LedgerDisplayName        | nvarchar(255) | Nee          | De naam van het grootboek om weer te geven in de gebruikersinterface |
| UserId                   | int           | Nee          | De ID van de gebruiker die is gekoppeld aan de verbinding |
| UserExternalId           | nvarchar(255) | Nee          | De externe id voor de gebruiker die gekoppeld aan de verbinding is. Deze ID is standaard de gebruiker van de Azure Active Directory voor het consortium. |
| UserProvisioningStatus   | int           | Nee          |Hiermee geeft u de huidige status van het inrichtingsproces voor de gebruiker. Mogelijke waarden zijn: <br />0: gebruiker is gemaakt door de API<br />1: een sleutel is gekoppeld aan de gebruiker in de database<br />2: de gebruiker is volledig is ingericht |
| UserFirstName            | nvarchar(50)  | Ja         | De voornaam van de gebruiker die gekoppeld aan de verbinding is |
| UserLastName             | nvarchar(255) | Ja         | De achternaam van de gebruiker die gekoppeld aan de verbinding is |
| UserEmailAddress         | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die gekoppeld aan de verbinding is |

## <a name="vwcontract"></a>vwContract

Deze weergave bevat details over geïmplementeerde contracten. In deze weergave bevat de volgende gegevens voor elke opdracht:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Implementatie van de gekoppelde grootboek voor de functie
-   Details voor de gebruiker die de actie heeft gestart
-   Gegevens die betrekking hebben op de blockchain-blok- en transactiekosten

| Name                                     | Type           | Kan niet null zijn | Description                                                                                                                                                                                                                                                   |
|------------------------------------------|----------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ConnectionId                             | int            | Nee          | De unieke id voor een verbinding in Azure Blockchain Workbench.                                                                                                                                                                                         |
| ConnectionEndpointUrl                    | nvarchar(50)   | Nee          | De eindpunt-url voor een verbinding |
| ConnectionFundingAccount                 | nvarchar(255)  | Ja         | De financiering account dat is gekoppeld aan een verbinding, indien van toepassing |
| LedgerId                                 | int            | Nee          | De unieke id voor een grootboek |
| LedgerName                               | nvarchar(50)   | Nee          | De naam van het grootboek |
| LedgerDisplayName                        | nvarchar(255)  | Nee          | De naam van het grootboek om weer te geven in de gebruikersinterface |
| ApplicationId                            | int            | Nee          | Een unieke id voor de toepassing |
| ApplicationName                          | nvarchar (50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName                   | nvarchar (255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled                       | bits            | Nee          | Wordt geïdentificeerd als de toepassing is momenteel ingeschakeld.<br /> **Opmerking:** Hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten blijven op de blockchain en gegevens over deze opdrachten niet uit de database.  |
| WorkflowId                               | int            | Nee          | Een unieke id voor de werkstroom die is gekoppeld aan een contract |
| WorkflowName                             | nvarchar(50)   | Nee          | De naam van de werkstroom die is gekoppeld aan een contract |
| WorkflowDisplayName                      | nvarchar(255)  | Nee          | De naam van de werkstroom die is gekoppeld aan het contract weergegeven in de gebruikersinterface |
| WorkflowDescription                      | nvarchar(255)  | Ja         | De beschrijving van de werkstroom die is gekoppeld aan een contract |
| ContractCodeId                           | int            | Nee          | Een unieke id voor de Contractcode die is gekoppeld aan het contract |
| ContractFileName                         | int            | Nee          | De naam van het bestand met de slimme Contractcode voor deze werkstroom. |
| ContractUploadedDtTm                     | int            | Nee          | De datum en tijd die de Contractcode is geüpload |
| ContractId                               | int            | Nee          | De unieke id voor het contract |
| ContractProvisioningStatus               | int            | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor het contract. Mogelijke waarden zijn: <br />0: het contract is gemaakt door de API in de database<br />1: het contract is verzonden naar het grootboek<br />2: het contract is geïmplementeerd op het grootboek<br />3 of 4: de opdracht kan niet worden geïmplementeerd op het grootboek<br />5 - het contract is geïmplementeerd op het grootboek <br /><br />Vanaf versie 1.5, worden 0 tot en met 5 waarden ondersteund. Voor achterwaartse compatibiliteit in de huidige release, weergave **vwContractV0** beschikbaar dat ondersteunt alleen waarden van 0 tot en met 2. |
| ContractLedgerIdentifier                 | nvarchar (255) |             | Het e-mailadres van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserId                 | int            | Nee          | Een externe id voor de gebruiker die het contract wordt geïmplementeerd. Deze ID is standaard de guid die de Azure Active Directory-ID voor de gebruiker.                                                                                                          |
| ContractDeployedByUserExternalId         | nvarchar(255)  | Nee          | Een externe id voor de gebruiker die het contract wordt geïmplementeerd. Deze ID is standaard de guid die de Azure Active Directory-ID voor de gebruiker.                                                                                                         |
| ContractDeployedByUserProvisioningStatus | int            | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor de gebruiker. Mogelijke waarden zijn: <br />0: gebruiker is gemaakt door de API<br />1: een sleutel is gekoppeld aan de gebruiker in de database <br />2: de gebruiker is volledig is ingericht                     |
| ContractDeployedByUserFirstName          | nvarchar(50)   | Ja         | De voornaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserLastName           | nvarchar(255)  | Ja         | De achternaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserEmailAddress       | nvarchar(255)  | Ja         | Het e-mailadres van de gebruiker die het contract geïmplementeerd |

## <a name="vwcontractaction"></a>vwContractAction

In deze weergave vertegenwoordigen de meerderheid van informatie met betrekking tot de acties die worden uitgevoerd op contracten en is ontworpen om gemakkelijk algemene scenario's voor rapportage mogelijk te maken. Voor elke uitgevoerde actie bevat in deze weergave de volgende gegevens:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Slimme contract functie en de parameter-definitie die is gekoppeld
-   Implementatie van de gekoppelde grootboek voor de functie
-   Specifieke waarden opgegeven voor de parameters
-   Details voor de gebruiker die de actie heeft gestart
-   Gegevens die betrekking hebben op de blockchain-blok- en transactiekosten

| Name                                     | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                                                                                                    |
|------------------------------------------|---------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                            | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName                          | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName                   | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled                       | bits           | Nee          | Dit veld wordt geïdentificeerd als de toepassing is momenteel ingeschakeld. Opmerking: hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten op de blockchain blijven en gegevens over deze opdrachten niet uit de database.                                                  |
| WorkflowId                               | int           | Nee          | Een unieke id voor een werkstroom |
| WorkflowName                             | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName                      | nvarchar(255) | Nee          | De naam van de werkstroom om weer te geven in een gebruikersinterface |
| WorkflowDescription                      | nvarchar(255) | Ja         | De beschrijving van de werkstroom |
| ContractId                               | int           | Nee          | Een unieke id voor het contract |
| ContractProvisioningStatus               | int           | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor het contract. Mogelijke waarden zijn: <br />0: het contract is gemaakt door de API in de database<br />1: het contract is verzonden naar het grootboek<br />2: het contract is geïmplementeerd op het grootboek<br />3 of 4: de opdracht kan niet worden geïmplementeerd op het grootboek<br />5 - het contract is geïmplementeerd op het grootboek <br /><br />Vanaf versie 1.5, worden 0 tot en met 5 waarden ondersteund. Voor achterwaartse compatibiliteit in de huidige release, weergave **vwContractActionV0** beschikbaar dat ondersteunt alleen waarden van 0 tot en met 2. |
| ContractCodeId                           | int           | Nee          | Een unieke id voor de implementatie van de code van de overeenkomst |
| ContractLedgerIdentifier                 | nvarchar(255) | Ja         | Een unieke id die is gekoppeld aan de geïmplementeerde versie van een slimme contract voor een specifieke gedistribueerd grootboek. Bijvoorbeeld, Ethereum. |
| ContractDeployedByUserId                 | int           | Nee          | De unieke id van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserFirstName          | nvarchar(50)  | Ja         | De voornaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserLastName           | nvarchar(255) | Ja         | De achternaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserExternalId         | nvarchar(255) | Nee          | Externe id van de gebruiker die het contract wordt geïmplementeerd. Deze ID is standaard de guid die hun identiteit in de Azure Active Directory-consortium vertegenwoordigt.                                                                                                                                                |
| ContractDeployedByUserEmailAddress       | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die het contract geïmplementeerd |
| WorkflowFunctionId                       | int           | Nee          | Een unieke id voor een Werkstroomfunctie |
| WorkflowFunctionName                     | nvarchar(50)  | Nee          | De naam van de functie |
| WorkflowFunctionDisplayName              | nvarchar(255) | Nee          | De naam van een functie moet worden weergegeven in de gebruikersinterface |
| WorkflowFunctionDescription              | nvarchar(255) | Nee          | De beschrijving van de functie |
| ContractActionId                         | int           | Nee          | De unieke id voor een overeenkomst-actie |
| ContractActionProvisioningStatus         | int           | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor de actie contract. Mogelijke waarden zijn: <br />0: de actie van het contract is gemaakt door de API in de database<br />1: de actie contract is verzonden naar het grootboek<br />2: de actie van het contract is geïmplementeerd op het grootboek<br />3 of 4: de opdracht kan niet worden geïmplementeerd op het grootboek<br />5 - het contract is geïmplementeerd op het grootboek <br /><br />Vanaf versie 1.5, worden 0 tot en met 5 waarden ondersteund. Voor achterwaartse compatibiliteit in de huidige release, weergave **vwContractActionV0** beschikbaar dat ondersteunt alleen waarden van 0 tot en met 2. |
| ContractActionTimestamp                  | DATETIME(2,7) | Nee          | De tijdstempel van de overeenkomst-actie |
| ContractActionExecutedByUserId           | int           | Nee          | De unieke id van de gebruiker die de overeenkomst-actie uitgevoerd |
| ContractActionExecutedByUserFirstName    | int           | Ja         | De voornaam van de gebruiker die de overeenkomst-actie uitgevoerd |
| ContractActionExecutedByUserLastName     | nvarchar(50)  | Ja         | De achternaam van de gebruiker die de overeenkomst-actie uitgevoerd |
| ContractActionExecutedByUserExternalId   | nvarchar(255) | Ja         | Externe id van de gebruiker die de overeenkomst-actie uitgevoerd. Deze ID is standaard de guid die hun identiteit in de Azure Active Directory-consortium vertegenwoordigt. |
| ContractActionExecutedByUserEmailAddress | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die de overeenkomst-actie uitgevoerd |
| WorkflowFunctionParameterId              | int           | Nee          | Een unieke id voor een parameter van de functie |
| WorkflowFunctionParameterName            | nvarchar(50)  | Nee          | De naam van een parameter van de functie |
| WorkflowFunctionParameterDisplayName     | nvarchar(255) | Nee          | De naam van een parameter van de functie moet worden weergegeven in de gebruikersinterface |
| WorkflowFunctionParameterDataTypeId      | int           | Nee          | De unieke id voor het gegevenstype dat is gekoppeld aan een functie werkstroomparameter |
| WorkflowParameterDataTypeName            | nvarchar(50)  | Nee          | De naam van een gegevenstype dat is gekoppeld aan een functie werkstroomparameter |
| ContractActionParameterValue             | nvarchar(255) | Nee          | De waarde voor de parameter die zijn opgeslagen in de slimme contract |
| BlockHash                                | nvarchar(255) | Ja         | De hash van het blok |
| BlockNumber                              | int           | Ja         | Het nummer van het blok op het grootboek |
| BlockTimestamp                           | DATETIME(2,7) | Ja         | De tijdstempel van het blok |
| TransactionId                            | int           | Nee          | Een unieke id voor de transactie |
| TransactionFrom                          | nvarchar(255) | Ja         | De partij die afkomstig zijn van de transactie |
| TransactionTo                            | nvarchar(255) | Ja         | De partij die is verwerkt met |
| TransactionHash                          | nvarchar(255) | Ja         | De hash van een transactie |
| TransactionIsWorkbenchTransaction        | bits           | Ja         | Een bit die wordt geïdentificeerd als de transactie is een transactie Azure Blockchain Workbench |
| TransactionProvisioningStatus            | int           | Ja         | Hiermee geeft u de huidige status van het inrichtingsproces voor de transactie. Mogelijke waarden zijn: <br />0: de transactie is gemaakt door de API in de database<br />1: de transactie is verzonden naar het grootboek<br />2: de transactie is geïmplementeerd op het grootboek                 |
| TransactionValue                         | Decimal(32,2) | Ja         | De waarde van de transactie |

## <a name="vwcontractproperty"></a>vwContractProperty

In deze weergave vertegenwoordigen de meerderheid van informatie met betrekking tot de eigenschappen die zijn gekoppeld aan een contract en is ontworpen om gemakkelijk algemene scenario's voor rapportage mogelijk te maken. In deze weergave bevat voor elke eigenschap die wordt uitgevoerd, de volgende gegevens:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Details voor de gebruiker die de werkstroom wordt geïmplementeerd
-   Definitie van de eigenschap slimme contract dat is gekoppeld
-   Specifieke waarden van eigenschappen
-   Details voor de eigenschap state van het contract

| Name                               | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                                                                        |
|------------------------------------|---------------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                      | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName                    | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName             | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled                 | bits           | Nee          | Wordt geïdentificeerd als de toepassing is momenteel ingeschakeld.<br />**Opmerking:** Hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten blijven op de blockchain en gegevens over deze opdrachten niet uit de database.                      |
| WorkflowId                         | int           | Nee          | De unieke id voor de werkstroom |
| WorkflowName                       | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName                | nvarchar(255) | Nee          | De naam van de werkstroom weergegeven in de gebruikersinterface |
| WorkflowDescription                | nvarchar(255) | Ja         | De beschrijving van de werkstroom |
| ContractId                         | int           | Nee          | De unieke id voor het contract |
| ContractProvisioningStatus         | int           | Nee          | Hiermee geeft u de huidige status van het inrichtingsproces voor het contract. Mogelijke waarden zijn: <br />0: het contract is gemaakt door de API in de database<br />1: het contract is verzonden naar het grootboek<br />2: het contract is geïmplementeerd op het grootboek<br />3 of 4: de opdracht kan niet worden geïmplementeerd op het grootboek<br />5 - het contract is geïmplementeerd op het grootboek <br /><br />Vanaf versie 1.5, worden 0 tot en met 5 waarden ondersteund. Voor achterwaartse compatibiliteit in de huidige release, weergave **vwContractPropertyV0** beschikbaar dat ondersteunt alleen waarden van 0 tot en met 2. |
| ContractCodeId                     | int           | Nee          | Een unieke id voor de implementatie van de code van de overeenkomst |
| ContractLedgerIdentifier           | nvarchar(255) | Ja         | Een unieke id die is gekoppeld aan de geïmplementeerde versie van een slimme contract voor een specifieke gedistribueerd grootboek. Bijvoorbeeld, Ethereum. |
| ContractDeployedByUserId           | int           | Nee          | De unieke id van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserFirstName    | nvarchar(50)  | Ja         | De voornaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserLastName     | nvarchar(255) | Ja         | De achternaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserExternalId   | nvarchar(255) | Nee          | Externe id van de gebruiker die het contract wordt geïmplementeerd. Deze ID is standaard de guid die staat voor de identiteit in het consortium Azure Active Directory |
| ContractDeployedByUserEmailAddress | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die het contract geïmplementeerd |
| WorkflowPropertyId                 | int           |             | Een unieke id voor een eigenschap van een werkstroom |
| WorkflowPropertyDataTypeId         | int           | Nee          | De ID van het gegevenstype van de eigenschap |
| WorkflowPropertyDataTypeName       | nvarchar(50)  | Nee          | De naam van het gegevenstype van de eigenschap |
| WorkflowPropertyName               | nvarchar(50)  | Nee          | De naam van de werkstroom-eigenschap |
| WorkflowPropertyDisplayName        | nvarchar(255) | Nee          | De weergavenaam van de werkstroom-eigenschap |
| WorkflowPropertyDescription        | nvarchar(255) | Ja         | Een beschrijving van de eigenschap |
| ContractPropertyValue              | nvarchar(255) | Nee          | De waarde voor een eigenschap van het contract |
| StateName                          | nvarchar(50)  | Ja         | Als deze eigenschap de status van het contract bevat, is het de weergavenaam voor de status. Als deze niet gekoppeld aan de status is, is de waarde niet null zijn. |
| StateDisplayName                   | nvarchar(255) | Nee          | Als deze eigenschap de status bevat, is het de weergavenaam voor de status. Als deze niet gekoppeld aan de status is, is de waarde niet null zijn. |
| StateValue                         | nvarchar(255) | Ja         | Als deze eigenschap de status bevat, is de waarde van de status. Als deze niet gekoppeld aan de status is, is de waarde niet null zijn. |

## <a name="vwcontractstate"></a>vwContractState

In deze weergave vertegenwoordigen de meerderheid van informatie met betrekking tot de status van een specifieke overeenkomst en is ontworpen om gemakkelijk algemene scenario's voor rapportage mogelijk te maken. Elke record in deze weergave bevat de volgende gegevens:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Details voor de gebruiker die de werkstroom wordt geïmplementeerd
-   Definitie van de eigenschap slimme contract dat is gekoppeld
-   Details voor de eigenschap state van het contract

| Name                               | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                                                                        |
|------------------------------------|---------------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                      | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName                    | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName             | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled                 | bits           | Nee          | Wordt geïdentificeerd als de toepassing is momenteel ingeschakeld.<br />**Opmerking:** Hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten blijven op de blockchain en gegevens over deze opdrachten niet uit de database. |
| WorkflowId                         | int           | Nee          | Een unieke id voor de werkstroom |
| WorkflowName                       | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName                | nvarchar(255) | Nee          | De naam die wordt weergegeven in de gebruikersinterface |
| WorkflowDescription                | nvarchar(255) | Ja         | De beschrijving van de werkstroom |
| ContractLedgerImplementationId     | nvarchar(255) | Ja         | Een unieke id die is gekoppeld aan de geïmplementeerde versie van een slimme contract voor een specifieke gedistribueerd grootboek. Bijvoorbeeld, Ethereum. |
| ContractId                         | int           | Nee          | Een unieke id voor het contract |
| ContractProvisioningStatus         | int           | Nee          |Hiermee geeft u de huidige status van het inrichtingsproces voor het contract. Mogelijke waarden zijn: <br />0: het contract is gemaakt door de API in de database<br />1: het contract is verzonden naar het grootboek<br />2: het contract is geïmplementeerd op het grootboek<br />3 of 4: de opdracht kan niet worden geïmplementeerd op het grootboek<br />5 - het contract is geïmplementeerd op het grootboek <br /><br />Vanaf versie 1.5, worden 0 tot en met 5 waarden ondersteund. Voor achterwaartse compatibiliteit in de huidige release, weergave **vwContractStateV0** beschikbaar dat ondersteunt alleen waarden van 0 tot en met 2. |
| ConnectionId                       | int           | Nee          | Een unieke id voor de blockchain-exemplaar dat de werkstroom wordt geïmplementeerd op |
| ContractCodeId                     | int           | Nee          | Een unieke id voor de implementatie van de code van de overeenkomst |
| ContractDeployedByUserId           | int           | Nee          | De unieke id van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserExternalId   | nvarchar(255) | Nee          | Externe id van de gebruiker die het contract wordt geïmplementeerd. Deze ID is standaard de guid die hun identiteit in de Azure Active Directory-consortium vertegenwoordigt. |
| ContractDeployedByUserFirstName    | nvarchar(50)  | Ja         | De voornaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserLastName     | nvarchar(255) | Ja         | De achternaam van de gebruiker die het contract geïmplementeerd |
| ContractDeployedByUserEmailAddress | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker die het contract geïmplementeerd |
| WorkflowPropertyId                 | int           | Nee          | Een unieke id voor de eigenschap van een werkstroom |
| WorkflowPropertyDataTypeId         | int           | Nee          | De ID van het gegevenstype van de werkstroom-eigenschap |
| WorkflowPropertyDataTypeName       | nvarchar(50)  | Nee          | De naam van het gegevenstype van de werkstroom-eigenschap |
| WorkflowPropertyName               | nvarchar(50)  | Nee          | De naam van de werkstroom-eigenschap |
| WorkflowPropertyDisplayName        | nvarchar(255) | Nee          | De weergavenaam van de eigenschap om weer te geven in een gebruikersinterface |
| WorkflowPropertyDescription        | nvarchar(255) | Ja         | De beschrijving van de eigenschap |
| ContractPropertyValue              | nvarchar(255) | Nee          | De waarde voor een eigenschap die zijn opgeslagen in het contract |
| StateName                          | nvarchar(50)  | Ja         | Als deze eigenschap bevat de status, het de weergavenaam voor de status. Als deze niet gekoppeld aan de status is, is de waarde niet null zijn. |
| StateDisplayName                   | nvarchar(255) | Nee          | Als deze eigenschap de status bevat, is het de weergavenaam voor de status. Als deze niet gekoppeld aan de status is, is de waarde niet null zijn. |
| StateValue                         | nvarchar(255) | Ja         | Als deze eigenschap de status bevat, is de waarde van de status. Als deze niet gekoppeld aan de status is, is de waarde niet null zijn. |

## <a name="vwuser"></a>vwUser

In deze weergave biedt details over de consortium-leden die zijn ingericht voor het gebruik van Azure Blockchain Workbench. Standaard is via de eerste inrichting van de gebruiker gegevens ingevuld.

| Name               | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                               |
|--------------------|---------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Id                 | int           | Nee          | Een unieke id voor een gebruiker |
| externalID         | nvarchar(255) | Nee          | Een externe id voor een gebruiker. Deze ID is standaard de guid die de Azure Active Directory-ID voor de gebruiker. |
| ProvisioningStatus | int           | Nee          |Hiermee geeft u de huidige status van het inrichtingsproces voor de gebruiker. Mogelijke waarden zijn: <br />0: gebruiker is gemaakt door de API<br />1: een sleutel is gekoppeld aan de gebruiker in de database<br />2: de gebruiker is volledig is ingericht |
| FirstName          | nvarchar(50)  | Ja         | De voornaam van de gebruiker |
| LastName           | nvarchar(50)  | Ja         | De achternaam van de gebruiker |
| EmailAddress       | nvarchar(255) | Ja         | Het e-mailadres van de gebruiker |

## <a name="vwworkflow"></a>vwWorkflow

In deze weergave vertegenwoordigt de details van core werkstroom metagegevens, evenals de functies en parameters van de werkstroom. Is ontworpen voor het melden van en bevat het ook metagegevens over de toepassing die is gekoppeld aan de werkstroom. In deze weergave bevat gegevens uit meerdere onderliggende tabellen te vergemakkelijken rapportage over werkstromen. In deze weergave bevat de volgende gegevens voor elke werkstroom:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Informatie over de status van de werkstroom starten die is gekoppeld

| Name                              | Type          | Kan niet null zijn | Description                                                                                                                                |
|-----------------------------------|---------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                     | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName                   | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName            | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled                | bits           | Nee          | Wordt geïdentificeerd als de toepassing is ingeschakeld |
| WorkflowId                        | int           | Ja         | Een unieke id voor een werkstroom |
| WorkflowName                      | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName               | nvarchar(255) | Nee          | De naam die wordt weergegeven in de gebruikersinterface |
| WorkflowDescription               | nvarchar(255) | Ja         | De beschrijving van de werkstroom. |
| WorkflowConstructorFunctionId     | int           | Nee          | De id van de Werkstroomfunctie die als de constructor voor de werkstroom fungeert |
| WorkflowStartStateId              | int           | Nee          | Een unieke id voor de status |
| WorkflowStartStateName            | nvarchar(50)  | Nee          | De naam van de status |
| WorkflowStartStateDisplayName     | nvarchar(255) | Nee          | De naam moet worden weergegeven in de gebruikersinterface voor de status |
| WorkflowStartStateDescription     | nvarchar(255) | Ja         | Een beschrijving van de werkstroomstatus |
| WorkflowStartStateStyle           | nvarchar(50)  | Ja         | Deze waarde geeft het percentage voltooid van te zijn dat de werkstroom wordt in deze status |
| WorkflowStartStateValue           | int           | Nee          | De waarde van de status |
| WorkflowStartStatePercentComplete | int           | Nee          | Een tekstbeschrijving waarmee een hint aan clients in deze status in de gebruikersinterface voor het renderen. Ondersteunde statussen bevatten *succes* en *fout* |

## <a name="vwworkflowfunction"></a>vwWorkflowFunction

In deze weergave vertegenwoordigt de details van core werkstroom metagegevens, evenals de functies en parameters van de werkstroom. Is ontworpen voor het melden van en bevat het ook metagegevens over de toepassing die is gekoppeld aan de werkstroom. In deze weergave bevat gegevens uit meerdere onderliggende tabellen te vergemakkelijken rapportage over werkstromen. In deze weergave bevat de volgende gegevens voor elke Werkstroomfunctie:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Details van de werkstroom-functie

| Name                                 | Type          | Kan niet null zijn | Description                                                                          |
|--------------------------------------|---------------|-------------|--------------------------------------------------------------------------------------|
| ApplicationId                        | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName                      | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName               | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled                   | bits           | Nee          | Wordt geïdentificeerd als de toepassing is ingeschakeld |
| WorkflowId                           | int           | Nee          | Een unieke id voor een werkstroom |
| WorkflowName                         | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName                  | nvarchar(255) | Nee          | De naam van de werkstroom weergegeven in de gebruikersinterface |
| WorkflowDescription                  | nvarchar(255) | Ja         | De beschrijving van de werkstroom |
| WorkflowFunctionId                   | int           | Nee          | Een unieke id voor een functie |
| WorkflowFunctionName                 | nvarchar(50)  | Ja         | De naam van de functie |
| WorkflowFunctionDisplayName          | nvarchar(255) | Nee          | De naam van een functie moet worden weergegeven in de gebruikersinterface |
| WorkflowFunctionDescription          | nvarchar(255) | Ja         | De beschrijving van de Werkstroomfunctie |
| WorkflowFunctionIsConstructor        | bits           | Nee          | Als de Werkstroomfunctie de constructor voor de werkstroom is identificeert |
| WorkflowFunctionParameterId          | int           | Nee          | Een unieke id voor een parameter van een functie |
| WorkflowFunctionParameterName        | nvarchar(50)  | Nee          | De naam van een parameter van de functie |
| WorkflowFunctionParameterDisplayName | nvarchar(255) | Nee          | De naam van een parameter van de functie moet worden weergegeven in de gebruikersinterface |
| WorkflowFunctionParameterDataTypeId  | int           | Nee          | Een unieke id voor het gegevenstype dat is gekoppeld aan een functie werkstroomparameter |
| WorkflowParameterDataTypeName        | nvarchar(50)  | Nee          | De naam van een gegevenstype dat is gekoppeld aan een functie werkstroomparameter |

## <a name="vwworkflowproperty"></a>vwWorkflowProperty

In deze weergave geeft de eigenschappen gedefinieerd voor een werkstroom. In deze weergave bevat de volgende gegevens voor elke eigenschap:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Details van de eigenschap workflow

| Name                         | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                                                   |
|------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName              | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName       | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| ApplicationEnabled           | bits           | Nee          | Wordt geïdentificeerd als de toepassing is momenteel ingeschakeld.<br />**Opmerking:** Hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten blijven op de blockchain en gegevens over deze opdrachten niet uit de database. |
| WorkflowId                   | int           | Nee          | Een unieke id voor de werkstroom |
| WorkflowName                 | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName          | nvarchar(255) | Nee          | De naam moet worden weergegeven voor de werkstroom in een gebruikersinterface |
| WorkflowDescription          | nvarchar(255) | Ja         | Een beschrijving van de werkstroom |
| WorkflowPropertyID           | int           | Nee          | Een unieke id voor een eigenschap van een werkstroom |
| WorkflowPropertyName         | nvarchar(50)  | Nee          | De naam van de eigenschap |
| WorkflowPropertyDescription  | nvarchar(255) | Ja         | Een beschrijving van de eigenschap |
| WorkflowPropertyDisplayName  | nvarchar(255) | Nee          | De naam moet worden weergegeven in een gebruikersinterface |
| WorkflowPropertyWorkflowId   | int           | Nee          | De ID van de werkstroom die gekoppeld aan deze eigenschap is |
| WorkflowPropertyDataTypeId   | int           | Nee          | De ID van het gegevenstype voor de eigenschap |
| WorkflowPropertyDataTypeName | nvarchar(50)  | Nee          | De naam van het gegevenstype voor de eigenschap |
| WorkflowPropertyIsState      | bits           | Nee          | Dit veld wordt geïdentificeerd als deze eigenschap werkstroom de status van de werkstroom bevat |

## <a name="vwworkflowstate"></a>vwWorkflowState

In deze weergave geeft de eigenschappen die zijn gekoppeld aan een werkstroom. In deze weergave bevat de volgende gegevens voor elke opdracht:

-   De definitie van de bijbehorende toepassing
-   Gekoppelde werkstroomdefinitie
-   Informatie over de status van de werkstroom

| Name                         | Type          | Kan niet null zijn | Description                                                                                                                                                                                                                                                   |
|------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                | int           | Nee          | Een unieke id voor de toepassing |
| ApplicationName              | nvarchar(50)  | Nee          | De naam van de toepassing |
| ApplicationDisplayName       | nvarchar(255) | Nee          | Een beschrijving van de toepassing |
| ApplicationEnabled           | bits           | Nee          | Wordt geïdentificeerd als de toepassing is momenteel ingeschakeld.<br />**Opmerking:** Hoewel een toepassing kan worden weergegeven als uitgeschakeld in de database, gekoppelde contracten blijven op de blockchain en gegevens over deze opdrachten niet uit de database. |
| WorkflowId                   | int           | Nee          | De unieke id voor de werkstroom |
| WorkflowName                 | nvarchar(50)  | Nee          | De naam van de werkstroom |
| WorkflowDisplayName          | nvarchar(255) | Nee          | De naam die wordt weergegeven in de gebruikersinterface voor de werkstroom |
| WorkflowDescription          | nvarchar(255) | Ja         | De beschrijving van de werkstroom |
| WorkflowStateID              | int           | Nee          | De unieke id voor de status |
| WorkflowStateName            | nvarchar(50)  | Nee          | De naam van de status |
| WorkflowStateDisplayName     | nvarchar(255) | Nee          | De naam moet worden weergegeven in de gebruikersinterface voor de status |
| WorkflowStateDescription     | nvarchar(255) | Ja         | Een beschrijving van de werkstroomstatus |
| WorkflowStatePercentComplete | int           | Nee          | Deze waarde geeft het percentage voltooid van te zijn dat de werkstroom wordt in deze status |
| WorkflowStateValue           | nvarchar(50)  | Nee          | Waarde van de status |
| WorkflowStateStyle           | nvarchar(50)  | Nee          | Een tekstbeschrijving waarmee een hint aan clients in deze status in de gebruikersinterface voor het renderen. Ondersteunde statussen bevatten *succes* en *fout* |
