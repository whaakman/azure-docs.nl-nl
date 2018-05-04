---
title: Azure Policy gebruiken om beleidsregels te maken en beheren voor het afdwingen van organisatorische naleving | Microsoft Docs
description: Azure Policy gebruiken om standaarden af te dwingen, te voldoen aan wettelijke vereisten voor naleving en audits, kosten te beheren, consistente beveiliging en prestaties te houden en bedrijfsbrede ontwerpprincipes op te leggen.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/19/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: e50c2330c03e5fc36e608b672fe8390a1f19e99b
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Beleidsregels voor het afdwingen van naleving maken en beheren

Als u aan uw bedrijfsnormen en serviceovereenkomsten wilt blijven voldoen, is het belangrijk dat u begrijpt hoe u beleidsregels kunt maken en beheren in Azure. In deze zelfstudie leert u hoe u Azure Policy gebruikt om enkele algemene taken voor het maken, toewijzen en beheren van beleid in uw organisatie uit te voeren, zoals:

> [!div class="checklist"]
> * Toewijzen van een beleid om een voorwaarde voor bronnen af te dwingen die u in de toekomst maakt
> * Maken en toewijzen van de definitie van een initiatief om naleving bij te houden voor meerdere bronnen
> * Problemen met een bron die niet voldoet of is geweigerd oplossen
> * Een nieuw beleid binnen een organisatie implementeren

Als u een beleid wilt toewijzen voor het identificeren van de huidige nalevingsstatus van uw bestaande bronnen, raadpleegt u de snelstart-artikelen over dit ontwerp. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="assign-a-policy"></a>Een beleid toewijzen

De eerste stap bij het afdwingen van naleving met Azure Policy bestaat uit het toewijzen van een beleidsdefinitie. Een beleidsdefinitie definieert onder welke voorwaarde een beleid wordt afgedwongen en welke actie moet worden uitgevoerd. In dit voorbeeld wijzen we een ingebouwde beleidsdefinitie toe met de naam *SQL Server-versie 12.0 vereisen* om de voorwaarde af te dwingen dat alle SQL Server-databases v12.0 moeten zijn om aan het beleid te voldoen.

1. Start de Azure Policy-service in Azure Portal door **Beleid** op te zoeken en te selecteren in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/search-policy.png)

2. Selecteer **Toewijzingen** in het linkerdeelvenster van de Azure Policy-pagina. Een toewijzing is een beleid dat is toegewezen om te worden toegepast binnen een bepaald bereik.
3. Selecteer **Beleid toewijzen** boven in het deelvenster **Toewijzingen**.

   ![Een beleidsdefinitie toewijzen](media/create-manage-policy/select-assign-policy.png)

4. Klik op de pagina **Beleid toewijzen** op de knop ![Beleidsdefinitie](media/assign-policy-definition/definitions-button.png) naast het veld **Beleid** om de lijst met beschikbare definities te openen. U kunt het **Type** beleidsdefinitie filteren op *BuiltIn* om alles te bekijken en de beschrijvingen te lezen.

   ![Beschikbare beleidsdefinities openen](media/create-manage-policy/open-policy-definitions.png)

5. Selecteer **SQL Server-versie 12.0 vereisen**. Als u dit niet meteen kunt vinden, typt u **SQL Server-versie 12.0 vereisen** in het zoekvak en drukt u op Enter.

   ![Een beleid zoeken](media/create-manage-policy/select-available-definition.png)

6. De weergegeven **Naam** wordt automatisch ingevuld, maar u kunt deze wijzigen. In dit voorbeeld gebruiken we *SQL Server-versie 12.0 vereisen*. U kunt ook een optionele **Beschrijving** opgeven. De beschrijving bevat details over hoe de toewijzing van dit beleid ervoor zorgt dat alle SQL-servers die in deze omgeving worden gemaakt van versie 12.0 zijn.

7. Wijzig de prijscategorie in **Standaard** om ervoor te zorgen dat het beleid wordt toegepast op bestaande resources.

   Er zijn twee prijscategorieën in Azure Policy: *Gratis* en *Standaard*. Met de prijscategorie Gratis kunt alleen beleid afdwingen op toekomstige resources, terwijl u met Standaard ook beleid kunt afdwingen op bestaande resources om een beter inzicht te krijgen in uw nalevingsstatus. Omdat Azure Policy in preview is, is er nog geen prijsmodel uitgebracht en ontvangt u geen factuur als u *Standaard* selecteert. Zie [Prijzen voor Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy) voor meer informatie over prijzen.

8. Selecteer het **Bereik** - het eerder geregistreerde abonnement (of de eerder geregistreerde resourcegroep). Het bereik bepaalt op welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen.

   In dit voorbeeld wordt het abonnement **Azure Analytics capaciteit Dev** gebruikt. U hebt waarschijnlijk een ander abonnement.

10. Selecteer **Toewijzen**.

## <a name="implement-a-new-custom-policy"></a>Een nieuw aangepast beleid implementeren

Nu u een ingebouwde beleidsdefinitie hebt toegewezen, kunt u meer kunt doen met Azure Policy. Maak nu een nieuw aangepast beleid om kosten te besparen door ervoor te zorgen dat virtuele machines die in uw omgeving worden gemaakt, niet in de G-serie kunnen zijn. Hiermee wordt telkens wanneer een gebruiker in uw organisatie de virtuele machine in de G-serie wil maken, de aanvraag geweigerd.

1. Selecteer **Definitie** onder **Ontwerpen** in het linkerdeelvenster.

   ![Definitie onder Ontwerpen](media/create-manage-policy/definition-under-authoring.png)

2. Selecteer **+ Beleidsdefinitie**.
3. Voer het volgende in:

   - De naam van de beleidsdefinitie - *Vereisen dat VM-SKU's kleiner zijn dan de G-serie*
   - De beschrijving van het doel van de beleidsdefinitie. Deze beleidsdefinitie dwingt af dat alle virtuele machines die zijn gemaakt in dit bereik, SKU's hebben die kleiner zijn dan de G-serie om kosten te beperken.
   - Het abonnement waarin de beleidsdefinitie zich bevindt. In dit geval bevindt de beleidsdefinitie zich in **Advisor Analytics capaciteit Dev**. U hebt waarschijnlijk een andere abonnementslijst.
   - Kies een van de bestaande opties of maak een nieuwe categorie voor deze beleidsdefinitie.
   - Kopieer de volgende json-code en werk deze vervolgens naar wens bij:
      - De beleidsparameters.
      - De beleidsregels/-voorwaarden, in dit geval – VM SKU-grootte gelijk aan G-serie
      - Het effect van dit beleid, in dit geval: **Weigeren**.

    Zo moet de json eruitzien. Plak uw gewijzigde code in Azure Portal.

    ```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "like": "Standard_G*"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
}
    ```

    De waarde van de *veldeigenschap* in de beleidsregel moet een van de volgende waarden zijn: Naam, Type, Locatie, Tags of een alias. Bijvoorbeeld `"Microsoft.Compute/VirtualMachines/Size"`.

    Lees het artikel [Sjablonen voor Azure Policy](json-samples.md) als u meer voorbeelden van json-code wilt bekijken.

4. Selecteer **Opslaan**.

## <a name="create-a-policy-definition-with-rest-api"></a>Een beleidsdefinitie maken met de REST API

U kunt een beleid maken met de REST API voor beleidsdefinities. Met de REST API kunt u beleidsdefinities maken en verwijderen en informatie over bestaande definities ophalen.
Gebruik het volgende voorbeeld om een beleidsdefinitie te maken:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
Voeg aanvraagtekst toe die soortgelijk is aan het volgende voorbeeld:

```
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Een beleidsdefinitie maken met PowerShell

Controleer voordat u doorgaat met het PowerShell-voorbeeld of u de nieuwste versie van Azure PowerShell hebt geïnstalleerd. Beleidsparameters zijn toegevoegd in versie 3.6.0. Als u een eerdere versie hebt, retourneren de voorbeelden een fout met de mededeling dat de parameter niet is gevonden.

U kunt een beleidsdefinitie maken met de `New-AzureRmPolicyDefinition`-cmdlet.

Als u een beleidsdefinitie wilt maken op basis van een bestand, geeft u het pad naar het bestand op. Gebruik het volgende voorbeeld voor een extern bestand:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Gebruik het volgende voorbeeld voor een lokaal bestand:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Gebruik het volgende voorbeeld om een beleidsdefinitie met een inline regel te maken:

```
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

De uitvoer wordt opgeslagen in een `$definition`-object, dat wordt gebruikt tijdens de toewijzing van configuratiebeleid.
In het volgende voorbeeld wordt een beleidsdefinitie met parameters gemaakt:

```
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters
```

## <a name="view-policy-definitions"></a>Beleidsdefinities weergeven

Als u alle beleidsdefinities in uw abonnement wilt weergeven, gebruikt u de volgende opdracht:

```
Get-AzureRmPolicyDefinition
```

Hiermee worden alle beschikbare beleidsdefinities geretourneerd, inclusief ingebouwd beleid. Elk beleid wordt geretourneerd in de volgende indeling:

```
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Een beleidsdefinitie maken met Azure CLI

U kunt een beleidsdefinitie maken met Azure CLI met behulp van de beleidsdefinitieopdracht.
Gebruik het volgende voorbeeld om een beleidsdefinitie met een inline regel te maken:

```
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

## <a name="view-policy-definitions"></a>Beleidsdefinities weergeven

Als u alle beleidsdefinities in uw abonnement wilt weergeven, gebruikt u de volgende opdracht:

```
az policy definition list
```

Hiermee worden alle beschikbare beleidsdefinities geretourneerd, inclusief ingebouwd beleid. Elk beleid wordt geretourneerd in de volgende indeling:

```
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Een initiatiefdefinitie maken en toewijzen

Met een initiatiefdefinitie kunt u verschillende beleidsdefinities groeperen om één overkoepelend doel te bereiken. U maakt een initiatiefdefinitie om ervoor te zorgen dat bronnen binnen het bereik van de definitie voldoen aan de beleidsdefinities die gezamenlijk die initiatiefdefinitie vormen.  Zie het [Overzicht van Azure Policy](./azure-policy-introduction.md) voor meer informatie over initiatiefdefinities.

### <a name="create-an-initiative-definition"></a>Een initiatiefdefinitie maken

1. Selecteer **Definities** onder **Ontwerpen** in het linkerdeelvenster.

   ![Definities selecteren](media/create-manage-policy/select-definitions.png)

2. Selecteer **Initiatiefdefinitie** boven aan de pagina. U gaat dan naar het formulier **Initiatiefdefinitie**.
3. Voer de naam en beschrijving van het initiatief in.

   In dit voorbeeld moet ervoor worden gezorgd dat bronnen voldoen aan beleidsdefinities over beveiliging. De naam van het initiatief wordt dus **Beveiligen**, en de beschrijving is: **dit initiatief is gemaakt voor het afhandelen van alle beleidsregels die zijn gekoppeld aan het beveiligen van bronnen**.

   ![Initiatiefdefinitie](media/create-manage-policy/initiative-definition.png)

4. Blader door de lijst met **beschikbare definities** en selecteer de beleidsdefinities die u aan dit initiatief wilt toevoegen. Voor ons initiatief **Beveiligen** voegt u de volgende ingebouwde beleidsdefinities toe:
   - SQL Server-versie 12.0 vereisen
   - Niet-beveiligde webtoepassingen in Security Center bewaken.
   - Toestaan van machtigingen door netwerk in Security Center bewaken.
   - Mogelijke opname apps op whitelist in Security Center bewaken.
   - Niet-versleutelde VM-schijven in Security Center bewaken.

   ![Initiatiefdefinities](media/create-manage-policy/initiative-definition-2.png)

   Na het selecteren van de beleidsdefinities in de lijst ziet u deze onder **Beleid en parameters**, zoals wordt weergegeven op de vorige afbeelding.

5. Gebruik **Definitie locatie** om een abonnement voor het opslaan van de definitie te selecteren. Selecteer **Opslaan**.

### <a name="assign-an-initiative-definition"></a>Een initiatiefdefinitie maken

1. Ga naar het tabblad **Definities** onder **Ontwerp**.
2. Zoek naar de initiatiefdefinitie **Beveiligen** die u hebt gemaakt.
3. Selecteer de initiatiefdefinitie en selecteer vervolgens **Toewijzen**.

   ![Een definitie toewijzen](media/create-manage-policy/assign-definition.png)

4. Vul het formulier **Toewijzing** in door de volgende voorbeeldinformatie in te voeren. U kunt uw eigen gegevens gebruiken.
   - Naam: toewijzing van Beveiliging
   - Beschrijving: deze initiatieftoewijzing heeft als doel deze groep beleidsdefinities af te dwingen in het **Azure Advisor capaciteit Dev**-abonnement.
   - Prijscategorie: Standaard
   - Bereik waarop u deze toewijzing wilt toepassen: **Azure Advisor capaciteit Dev**. U kunt uw eigen abonnement en brongroep kiezen.

5. Selecteer **Toewijzen**.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Een bron die niet voldoet of wordt geweigerd uitsluiten met behulp van Uitsluiten

Volgens het bovenstaande voorbeeld wordt na het toewijzen van de beleidsdefinitie die SQL server-versie 12.0 vereist, een SQL-server die is gemaakt met een andere versie dan 12.0 geweigerd. In deze sectie doorloopt u het oplossen van een geweigerde poging om een SQL-server te maken door een uitsluiting aan te vragen voor specifieke resources. De uitsluiting voorkomt dat het beleid wordt afgedwongen. In het volgende voorbeeld wordt een willekeurige versie van SQL Server toegestaan. Een uitsluiting kan worden toegepast op een brongroep of u kunt de uitsluiting beperken tot afzonderlijke bronnen.

1. Selecteer **Toewijzingen** in het linkerdeelvenster.
2. Blader door alle beleidstoewijzingen en open de toewijzing *SQL Server-versie 12.0 vereisen*.
3. **Selecteer** een uitzondering voor bronnen in de brongroepen waarin u de SQL-server probeert te maken. In dit voorbeeld sluiten we Microsoft.Sql/servers/databases: *azuremetrictest/testdb* en *azuremetrictest/testdb2* uit.

   ![Uitsluiting aanvragen](media/create-manage-policy/request-exclusion.png)

   Een andere manier waarop u een probleem met een geweigerde bron kunt oplossen, is contact opnemen met de contactpersoon van het beleid, als u een gegronde reden hebt voor de noodzaak tot het maken van de SQL Server. U kunt het beleid dan rechtstreeks bewerken als u toegang hebt.

4. Klik op **Toewijzen**.

In deze sectie hebt u de weigering van uw poging om een SQL Server te maken, opgelost door een uitsluiting voor de resources aan te vragen.

## <a name="clean-up-resources"></a>Resources opschonen

Als u door wilt gaan met andere zelfstudies, verwijdert u de bronnen die u in deze handleiding hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle bovenstaande toewijzingen of definities te verwijderen:

1. Selecteer **Definities** (of **Toewijzingen** als u een toewijzing probeert te verwijderen) in het linkerdeelvenster.
2. Zoek naar de nieuwe initiatief- of beleidsdefinitie die u zojuist hebt gemaakt.
3. Selecteer het weglatingsteken aan het einde van de definitie of de toewijzing en selecteer **Definitie verwijderen** (of **Toewijzing verwijderen**).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Een beleid toegewezen voor het afdwingen van een voorwaarde voor bronnen die u in de toekomst maakt
> * Een initiatiefdefinitie gemaakt en toegewezen om naleving bij te houden voor meerdere bronnen
> * Problemen met een bron die niet voldoet of is geweigerd, opgelost
> * Een nieuw beleid binnen een organisatie geïmplementeerd

Lees het volgende artikel voor meer informatie over de structuur van beleidsdefinities:

> [!div class="nextstepaction"]
> [Structuur van Azure Policy-definities](policy-definition.md)
