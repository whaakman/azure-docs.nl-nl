---
title: Databaserollen en gebruikers in Azure Analysis Services beheren | Microsoft Docs
description: Informatie over het beheren van functies van de database en gebruikers op een Analysis Services-server in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 53e946bba6bbd882e78b51ee8d222ab0d3ec056a
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="manage-database-roles-and-users"></a>Databaserollen en gebruikers beheren

Alle gebruikers moeten op het niveau van de database model behoren tot een rol. Gebruikers met bepaalde machtigingen voor de modeldatabase definiëren rollen Elke gebruiker of beveiligingsgroep toegevoegd aan een rol moet een account in een Azure AD-tenant in hetzelfde abonnement als de server hebben.

Hoe het definiëren van rollen is verschillend, afhankelijk van het hulpprogramma dat u gebruikt, maar het effect is hetzelfde.

Rolmachtigingen zijn onder andere:
*  **Beheerder** -gebruikers volledige machtigingen hebben voor de database. Databaserollen met Administrator-machtigingen zijn anders dan serverbeheerders.
*  **Proces** -gebruikers kunnen verbinding maken met proces bewerkingen uitvoeren op de database en analyseren van gegevens van de database model.
*  **Lees** -kunnen gebruikers een clienttoepassing gebruiken om verbinding maken met en analyseren van gegevens van de database model.

Wanneer een model in tabelvorm-project maakt, kunt u rollen maken en toevoegen van gebruikers of groepen aan deze rollen met rolbeheer in SSDT. Wanneer geïmplementeerd naar een server, gebruikt u SSMS, [Analysis Services-PowerShell-cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), of [Tabellaire Model scripttaal](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) toevoegen of verwijderen van rollen en leden van de gebruiker.

## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a>Toevoegen of beheren van rollen en gebruikers in SSDT  
  
1.  In SSDT > **Tabellaire Model Explorer**, met de rechtermuisknop op **rollen**.  
  
2.  Klik in **Role Manager** op **New**.  
  
3.  Typ een naam voor de rol.  
  
     Standaard wordt de naam van de standaardfunctie incrementeel genummerd voor elke nieuwe rol. Het raadzaam typt u een naam die het lidtype, bijvoorbeeld Financiën Managers of Human Resources specialisten duidelijk aangeeft.  
  
4.  Selecteer een van de volgende machtigingen:  
  
    |Machtiging|Beschrijving|  
    |----------------|-----------------|  
    |**Geen**|Leden kunnen het modelschema niet wijzigen en kunnen geen gegevens opvragen.|  
    |**Lezen**|Leden kunnen een query over gegevens (op basis van rijfilters), maar kunnen het modelschema niet wijzigen.|  
    |**Lees- en**|Leden kunnen vragen gegevens (op basis van rijniveau filters) en voer proces en proces alle bewerkingen, maar kunnen het modelschema niet wijzigen.|  
    |**Proces**|Leden kunnen proces en proces alle bewerkingen uitvoeren. Kan het modelschema niet wijzigen en kunnen geen gegevens opvragen.|  
    |**Beheerder**|Leden kunnen het modelschema wijzigen en alle gegevens opvragen.|   
  
5.  Als de rol zijn maken heeft lees- of de machtigingen lezen en proces, kunt u rijfilters toevoegen met behulp van een DAX-formule. Klik op de **rijfilters** tabblad en vervolgens een tabel selecteren en klik vervolgens op de **DAX-Filter** veld en typ vervolgens een DAX-formule.
  
6.  Klik op **leden** > **extern toevoegen**.  
  
8.  In **extern lid toevoegen**, gebruikers of groepen opgeven in uw Azure AD-tenant op e-mailadres. Nadat u op OK en rolbeheer sluit, rollen en leden van een rol worden weergegeven in de Tabellaire Model Explorer. 
 
     ![Rollen en gebruikers in de Tabellaire Model Explorer](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Implementeren in uw Azure Analysis Services-server.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>Toevoegen of beheren van rollen en gebruikers in SSMS
Voor rollen en gebruikers toevoegen aan een geïmplementeerde modeldatabase, moet u verbonden zijn met de server als serverbeheerder of wordt al een databaserol met administrator-machtigingen.

1. Met de rechtermuisknop in Verkenner-Object, **rollen** > **nieuwe rol**.

2. In **rol maken**, een role-naam en beschrijving opgeven.

3. Selecteer een machtiging.
   |Machtiging|Beschrijving|  
   |----------------|-----------------|  
   |**Volledig beheer (beheerder)**|Leden kunnen wijzigen van het modelschema verwerken en alle gegevens kunt opvragen.| 
   |**Proces-database**|Leden kunnen proces en proces alle bewerkingen uitvoeren. Kan het modelschema niet wijzigen en kunnen geen gegevens opvragen.|  
   |**Lezen**|Leden kunnen een query over gegevens (op basis van rijfilters), maar kunnen het modelschema niet wijzigen.|  
  
4. Klik op **lidmaatschap**, voert u een gebruiker of groep in uw Azure AD-tenant op e-mailadres.

     ![Gebruiker toevoegen](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Als de rol die u maakt, de machtiging lezen heeft, kunt u rijfilters toevoegen met behulp van een DAX-formule. Klik op **rijfilters**, selecteer een tabel en typ vervolgens een DAX-formule in de **DAX-Filter** veld. 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>Rollen en gebruikers toevoegen met behulp van een script TMSL
U kunt een TMSL-script uitvoeren in het venster XMLA in SSMS of met behulp van PowerShell. Gebruik de [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) opdracht en de [rollen](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.

**Voorbeeldscript TMSL**

In dit voorbeeld worden een externe gebruiker B2B en een groep toegevoegd aan de analist rol met leesmachtigingen voor de database SalesBI. De externe gebruiker en de groep moet in dezelfde Azure AD-tenant.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="to-add-roles-and-users-by-using-powershell"></a>Rollen en gebruikers toevoegen met behulp van PowerShell
De [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) module biedt taakspecifieke database-cmdlets voor beheer en de algemene Invoke ASCmd cmdlet die een Tabellair Model Scripting Language (TMSL) query of script accepteert. De volgende cmdlets worden gebruikt voor het beheren van databaserollen en gebruikers.
  
|Cmdlet|Beschrijving|
|------------|-----------------| 
|[Voeg RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Een lid toevoegen aan een databaserol.| 
|[Verwijder RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Een lid verwijderen uit een databaserol.|   
|[Aanroepen ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Een script TMSL uitvoeren.|

## <a name="row-filters"></a>Rijfilters  
Rijfilters definiëren welke rijen in een tabel door leden van een bepaalde rol kunnen worden opgevraagd. Rijfilters zijn gedefinieerd voor elke tabel in een model met DAX formules.  
  
Rijfilters alleen voor rollen met lees- en kunnen worden gedefinieerd en proces machtigingen. Standaard, als een rijfilter is niet gedefinieerd voor een bepaalde tabel kunnen leden opvragen alle rijen in de tabel tenzij cross-filtering is van toepassing van een andere tabel.
  
 Rijfilters vereisen een DAX-formule in een TRUE/FALSE-waarde resulteren moet, voor het definiëren van de rijen die door leden van die bepaalde rol kunnen worden opgevraagd. Rijen niet opgenomen in de DAX-formule, kunnen niet worden opgevraagd. Bijvoorbeeld, de tabel Klanten met de volgende rij expressie, filtert *= klanten [Country] = "VS"*, leden van de verkoop-rol kunnen alleen zien voor klanten in de Verenigde Staten.  
  
Rijfilters toepassen op de opgegeven rijen en de gerelateerde rijen. Wanneer een tabel meerdere relaties heeft, toepassing filters beveiliging voor de relatie die actief is. Rijfilters beginnen en eindigen met een andere rij filers gedefinieerd voor de gerelateerde tabellen, bijvoorbeeld:  
  
|Tabel|DAX-expressie|  
|-----------|--------------------|  
|Regio|= Regio [Country] = "VS"|  
|ProductCategory|= ProductCategory [Name] = "Fietsen"|  
|Transacties|= Transacties [jaar] = 2016|  
  
 Het uiteindelijke resultaat is leden gegevensrijen waar de klant is in de Verenigde Staten, de productcategorie voor fietsen en het jaar voor 2016 kunnen opvragen. Gebruikers kunnen geen transacties buiten de Verenigde Staten, transacties die niet fietsen of transacties niet 2016 tenzij ze deel uitmaken van een andere functie die deze machtigingen verleent opvragen.
  
 U kunt het filter *=FALSE()*, voor het weigeren van toegang tot alle rijen voor een hele tabel.

## <a name="next-steps"></a>Volgende stappen
  [Serverbeheerders beheren](analysis-services-server-admins.md)   
  [Azure analyseservices met PowerShell beheren](analysis-services-powershell.md)  
  [Scripting Language (TMSL) Reference Model in tabelvorm](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

