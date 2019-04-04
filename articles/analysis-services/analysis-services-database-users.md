---
title: Databaserollen en gebruikers in Azure Analysis Services beheren | Microsoft Docs
description: Informatie over het beheren van databaserollen en gebruikers op een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 462625ce61f4538aa0769667648e07cc6307cbb3
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891681"
---
# <a name="manage-database-roles-and-users"></a>Databaserollen en gebruikers beheren

Op het niveau van de database model moeten alle gebruikers behoren tot een rol. Rollen bepalen welke gebruikers met bepaalde machtigingen voor de modeldatabase. Een gebruiker of beveiligingsgroep toegevoegd aan een rol moet een account hebben in een Azure AD-tenant in hetzelfde abonnement als de server. 

Hoe u rollen definiëren verschilt, afhankelijk van het hulpprogramma die u gebruikt, maar het effect is hetzelfde.

Rolmachtigingen zijn onder andere:
*  **Beheerder** -gebruikers hebben volledige machtigingen voor de database. Databaserollen met beheerdersmachtigingen verschillen van server-beheerders.
*  **Proces** -gebruikers kunnen verbinding maken met en proces bewerkingen op de database uitvoeren en analyseren van gegevens van de model-database.
*  **Lezen** -gebruikers kunnen een clienttoepassing gebruiken verbinding maken met en analyseren van gegevens van de model-database.

Wanneer u een project voor tabellair model maakt, kunt u rollen maken en gebruikers of groepen toevoegen aan deze rollen met behulp van Role Manager in SSDT. Als de geïmplementeerd op een server, u SSMS, [Analysis Services PowerShell-cmdlets](/sql/analysis-services/powershell/analysis-services-powershell-reference), of [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) toevoegen of verwijderen van rollen en gebruikers die lid zijn.

> [!NOTE]
> Beveiligingsgroepen moeten hebben de `MailEnabled` eigenschap ingesteld op `True`.

## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a>Toevoegen of beheren van rollen en gebruikers in SSDT  
  
1.  In SSDT > **Tabular Model Explorer**, met de rechtermuisknop op **rollen**.  
  
2.  Klik in **Role Manager** op **New**.  
  
3.  Typ een naam voor de rol.  
  
     Standaard wordt de naam van de rol die standaard incrementeel genummerd voor elke nieuwe rol. Het verdient aanbeveling typt u een naam die het lidtype, bijvoorbeeld, financiën Managers of Human Resources-specialisten duidelijk aangeeft.  
  
4.  Selecteer een van de volgende machtigingen:  
  
    |Machtiging|Beschrijving|  
    |----------------|-----------------|  
    |**Geen**|Leden kunnen het modelschema niet wijzigen en kunnen geen gegevens op te vragen.|  
    |**Lezen**|Leden kunnen gegevens (op basis van rijfilters) op te vragen, maar kunnen het modelschema niet wijzigen.|  
    |**Lezen en verwerken**|Leden kunnen opvragen uitgevoerd proces en proces alle bewerkingen en gegevens (op basis van beveiliging op rijniveau filters), maar kunnen het modelschema niet wijzigen.|  
    |**Verwerken**|Leden kunnen verwerken en proces alle bewerkingen uitvoeren. Kan het modelschema niet wijzigen en kan geen gegevens op te vragen.|  
    |**Beheerder**|Leden kunnen het modelschema wijzigen en alle gegevens op te vragen.|   
  
5.  Als de rol die u niet maken is gelezen of de machtigingen lezen en verwerken, kunt u rijfilters toevoegen met behulp van een DAX-formule. Klik op de **Row Filters** tabblad, en vervolgens selecteert u een tabel en klik vervolgens op de **DAX Filter** veld en typ vervolgens een DAX-formule.
  
6.  Klik op **leden** > **extern toevoegen**.  
  
8.  In **extern lid toevoegen**, gebruikers of groepen in uw Azure AD-tenant invoeren door e-mailadres. Nadat u op OK en Role Manager sluit, functies en leden van een rol in Tabular Model Explorer worden weergegeven. 
 
     ![Rollen en gebruikers in Tabular Model Explorer](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Implementeren naar uw Azure Analysis Services-server.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>Toevoegen of beheren van rollen en gebruikers in SSMS

Rollen en gebruikers toevoegen aan een geïmplementeerd model-database, moet u worden verbonden met de server als serverbeheerder of heeft al een databaserol met beheerdersmachtigingen.

1. Met de rechtermuisknop in Verkenner-Object, **rollen** > **nieuwe rol**.

2. In **rol maken**, voer een rolnaam en de beschrijving.

3. Selecteer een machtiging.

   |Machtiging|Beschrijving|  
   |----------------|-----------------|  
   |**Volledige bevoegdheid (beheerder)**|Leden kunnen wijzigen van het modelschema verwerken, en kunnen alle gegevens op te vragen.| 
   |**Proces-database**|Leden kunnen verwerken en proces alle bewerkingen uitvoeren. Kan het modelschema niet wijzigen en kan geen gegevens op te vragen.|  
   |**Lezen**|Leden kunnen gegevens (op basis van rijfilters) op te vragen, maar kunnen het modelschema niet wijzigen.|  
  
4. Klik op **lidmaatschap**, voert u een gebruiker of groep in uw Azure AD-tenant door e-mailadres.

     ![Gebruiker toevoegen](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Als de rol die u maakt, de machtiging lezen heeft, kunt u rijfilters toevoegen met behulp van een DAX-formule. Klik op **Row Filters**, selecteert u een tabel en typ vervolgens een DAX-formule in de **DAX Filter** veld. 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>Rollen en gebruikers toevoegen met behulp van een TMSL-scripts

U kunt een TMSL-scripts uitvoeren in het XMLA-venster in SSMS of met behulp van PowerShell. Gebruik de [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) opdracht en de [rollen](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.

**Voorbeeld van een script TMSL**

In dit voorbeeld wordt worden een externe B2B-gebruiker en een groep toegevoegd aan de analist-rol met leesmachtigingen voor de database SalesBI. De externe gebruiker en de groep moeten zich in dezelfde Azure AD-tenant.

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

De [SqlServer](/sql/analysis-services/powershell/analysis-services-powershell-reference) module biedt taakspecifieke database management-cmdlets en de algemene Invoke-ASCmd cmdlet die een TMSL Tabular Model Scripting Language ()-query of het script accepteert. De volgende cmdlets worden gebruikt voor het beheren van databaserollen en gebruikers.
  
|Cmdlet|Description|
|------------|-----------------| 
|[Voeg RoleMember](/sql/analysis-services/powershell/analysis-services-powershell-reference)|Lid toevoegen aan een databaserol.| 
|[Remove-RoleMember](/sql/analysis-services/powershell/analysis-services-powershell-reference)|Een lid verwijderen uit een databaserol.|   
|[Invoke-ASCmd](/sql/analysis-services/powershell/analysis-services-powershell-reference)|Voer een TMSL-script.|

## <a name="row-filters"></a>Rijfilters  

Rijfilters definiëren welke rijen in een tabel door leden van een bepaalde rol kunnen worden opgevraagd. Rijfilters zijn gedefinieerd voor elke tabel in een model met behulp van DAX-formules.  
  
Rijfilters kunnen worden gedefinieerd alleen voor rollen met lees- en lezen en machtigingen voor proces. Standaard als een rijfilter is niet gedefinieerd voor een bepaalde tabel, leden kunnen een query alle rijen in de tabel, tenzij kruislings filteren is van toepassing van een andere tabel.
  
 Rijfilters vereisen een DAX-formule in een TRUE/FALSE-waarde resulteren moet, voor het definiëren van de rijen die door leden van die bepaalde rol kunnen worden doorzocht. Rijen die niet zijn opgenomen in de DAX-formule kunnen niet worden opgevraagd. Bijvoorbeeld, de tabel Klanten inschakelen met de volgende rij expressie filters *= klanten [Country] = "VS"*, leden van de rol Sales ziet alleen de klanten in de Verenigde Staten.  
  
Rijfilters van toepassing op de opgegeven rijen en de gerelateerde rijen. Wanneer een tabel meerdere relaties heeft, filters beveiliging toe te passen voor de relatie die actief is. Rijfilters beginnen en eindigen met een andere rij-filter gedefinieerd voor gerelateerde tabellen, bijvoorbeeld:  
  
|Tabel|DAX-expressie|  
|-----------|--------------------|  
|Regio|= Regio [Country] = "VS"|  
|ProductCategory|= ProductCategory [Name] = "Fietsen"|  
|Transacties|=Transactions[Year]=2016|  
  
 Het uiteindelijke resultaat is leden kunnen rijen met gegevens waar de klant zich in de Verenigde Staten, de productcategorie fietsen is en het jaar 2016 wordt een query. Gebruikers kunnen geen query uitvoeren op transacties buiten de Verenigde Staten, transacties die niet fietsen of transacties niet in 2016 tenzij ze lid zijn van een andere rol die machtigingen verleent.
  
 U kunt het filter, *=FALSE()*, voor het weigeren van toegang tot alle rijen voor een hele tabel.

## <a name="next-steps"></a>Volgende stappen

  [Serverbeheerders beheren](analysis-services-server-admins.md)   
  [Azure analyseservices beheren met PowerShell](analysis-services-powershell.md)  
  [Tabellair Model Scripting Language (TMSL) Reference](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

