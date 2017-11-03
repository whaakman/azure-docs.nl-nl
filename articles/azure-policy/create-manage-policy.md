---
title: Azure-beleid gebruiken om te maken en beheren van beleidsregels voor het afdwingen van compatibiliteit organisatie | Microsoft Docs
description: Azure-beleid gebruiken om te standaarden afdwingen, voldoen aan wettelijke vereisten voor naleving en audit beheerkosten, beveiliging en prestaties consistentie en enterprise wide principes opleggen.
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 10/06/2017
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 55e5a60294fc5ccb2a55b1e572af2fd27c68f462
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Beleidsregels voor het afdwingen van compatibiliteit maken en beheren

Informatie over het maken en beheren van beleid in Azure is belangrijk voor het voldoen aan uw bedrijfsnormen en serviceovereenkomsten bijwerkt. In deze zelfstudie leert u Azure beleid gebruiken voor het uitvoeren van de algemene taken met betrekking tot het maken, toewijzen en beheren van beleid in uw organisatie, zoals:

> [!div class="checklist"]
> * Toewijzen van een beleid voor het afdwingen van een voorwaarde voor bronnen die u in de toekomst zult maken
> * Maken en toewijzen van de definitie van een initiatief om bij te houden van naleving voor meerdere resources
> * Een niet-compatibele of geweigerde resource oplossen
> * Een nieuw beleid binnen een organisatie implementeren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="opt-in-to-azure-policy"></a>U meldt zich aan Azure-beleid

Beleid voor Azure is nu beschikbaar in de beperkte Preview, dus u hoeft te registreren bij aanvragen voor toegang.

1. Ga naar de Azure-beleid op https://aka.ms/getpolicy en selecteer **aanmelden** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/sign-up.png)

2. Aanmelden voor Azure-beleid door het selecteren van de abonnementen in de **abonnement** u werken wilt met lijst. Selecteer vervolgens **registreren**.

   De lijst met abonnementen omvat alle uw Azure-abonnementen.

   ![Aanmelden voor Azure-beleid gebruiken](media/assign-policy-definition/preview-opt-in.png)

   Afhankelijk van de vraag, kan deze duren een paar dagen voor ons uw registratieaanvraag accepteren. Wanneer uw aanvraag wordt geaccepteerd, u ontvangt een melding via e-mail dat u kunt beginnen met de service.

## <a name="assign-a-policy"></a>Geen beleid toewijzen

De eerste stap bij het afdwingen van compatibiliteit met beleid voor Azure is een beleidsdefinitie toewijzen. Een beleidsdefinitie definieert onder welke voorwaarde een beleid wordt afgedwongen en welke actie moet worden uitgevoerd. In dit voorbeeld wordt een ingebouwde beleidsdefinitie aangeroepen toewijzen *vereisen SQL Server-versie 12.0*, af te dwingen de voorwaarde dat alle SQL Server-databases v12.0 moet te voldoen.

1. Starten van de service Azure-beleid in de Azure-portal door te zoeken en te selecteren **beleid** in het linkerdeelvenster.

   ![Zoeken naar beleid](media/assign-policy-definition/search-policy.png)

2. Selecteer **toewijzingen** in het linkerdeelvenster van de pagina Azure-beleid. Een toewijzing is een beleid dat is toegewezen aan plaatsvinden binnen een bepaald bereik.
3. Selecteer **beleid toewijzen** vanaf de bovenkant van de **toewijzingen** deelvenster.

   ![Een beleidsdefinitie toewijzen](media/create-manage-policy/select-assign-policy.png)

4. Op de **beleid toewijzen** pagina, klikt u op ![beleid definitie knop](media/assign-policy-definition/definitions-button.png) naast **beleid** veld om de lijst van beschikbare definities te openen.

   ![Definities beschikbaar beleid openen](media/create-manage-policy/open-policy-definitions.png)

5. Selecteer **is SQL Server-versie 12.0**.
   
   ![Zoek een beleid](media/create-manage-policy/select-available-definition.png)

6. Geef een weergave **naam** voor de beleidstoewijzing. In dit geval gaan we gebruiken *vereisen SQL Server versie 12.0*. U kunt ook toevoegen een optionele **beschrijving**. De beschrijving bevat details over hoe de toewijzing van dit beleid zorgt ervoor alle SQL-servers gemaakt in deze omgeving dat versie 12.0 zijn.
7. Wijzig de prijscategorie te **standaard** om ervoor te zorgen dat het beleid wordt toegepast op bestaande bronnen.

   Er zijn twee prijscategorieën in Azure beleid – *vrije* en *standaard*. Met de gratis laag, kunt u alleen beleid afdwingen op toekomstige resources met de standaard, kunt u ook afdwingen ze op bestaande resources beter inzicht in uw compatibiliteitsstatus. Omdat we in de beperkte Preview, we nog niet is vrijgegeven prijsmodel gebruikt, zodat u ontvangt geen een factuur voor het selecteren van *standaard*. Voor meer informatie over prijzen, bekijk: [prijzen van Azure beleid](https://acom-milestone-ignite.azurewebsites.net/pricing/details/azure-policy/).

8. Selecteer de **bereik** -abonnement (of de resourcegroep) u eerder hebt geregistreerd wanneer u mee aan het beleid van Azure. Een bereik bepaalt welke resources of groeperen van resources de toewijzing van beleid wordt afgedwongen op. Dit kan variëren van een abonnement aan resourcegroepen.

   In dit voorbeeld gebruiken we dit abonnement - **Azure Analytics capaciteit Dev**. Uw abonnement zijn.

10. Selecteer **toewijzen**.

## <a name="implement-a-new-custom-policy"></a>Een nieuw aangepast beleid implementeren

Nu dat we de beleidsdefinitie hebt toegewezen, gaan we een nieuw beleid maken om kosten door ervoor te zorgen dat virtuele machines die zijn gemaakt, inlezen in uw omgeving kunnen niet in de reeks G opslaan. Op deze manier telkens wanneer een gebruiker in uw organisatie wil de virtuele machine maken in de reeks G de aanvraag wordt ophalen geweigerd.

1. Selecteer **definitie** onder **ontwerpen** in het linkerdeelvenster.

   ![Definitie onder ontwerpen](media/create-manage-policy/definition-under-authoring.png)

2. Selecteer **+ beleidsdefinitie**.
3. Voer het volgende:

   - De naam van de beleidsdefinitie - *vereisen VM-SKU's kleiner is dan de G-serie*
   - De beschrijving van de beleidsdefinitie is bedoeld om u te doen: de beleidsdefinitie voor dit wordt afgedwongen dat alle virtuele machines die zijn gemaakt in dit bereik hebben SKU's kleiner is dan de reeks G om kosten te beperken.
   - Het abonnement waarin de beleidsdefinitie wordt bevinden zich in: In dit geval onze beleidsdefinitie leven **Advisor Analytics capaciteit Dev**. De lijst met abonnementen zijn.
   - De json-code met schrijven:
      - De beleidsparameters.
      - De regels/beleidsvoorwaarden, in dit geval – VM SKU-grootte die gelijk is aan G serie
      - Het beleid effect in dit geval – **weigeren**.
   
   Dit is wat de json moet eruitzien als

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

   Voor voorbeelden van json-code weergeven, kijkt u naar dit artikel - [overzicht van de Resource-beleid](../azure-resource-manager/resource-manager-policy.md)
   
4. Selecteer **Opslaan**.

## <a name="create-and-assign-an-initiative-definition"></a>Maken en toewijzen van de definitie van een initiatief

U kunt verschillende beleidsdefinities om een overkoepelende doel te bereiken groeperen met een initiatief definitie. U maakt een definitie van een initiatief om ervoor te zorgen dat resources binnen het bereik van de definitie voldoen aan de beleidsdefinities die gezamenlijk de initiatief definitie blijven.  Zie de [overzicht Azure](./azure-policy-introduction.md) voor meer informatie over het initiatief definities.

### <a name="create-an-initiative-definition"></a>De definitie van een initiatief maken

1. Selecteer **definities** onder **ontwerpen** in het linkerdeelvenster.

   ![Definities selecteren](media/create-manage-policy/select-definitions.png)

2. Selecteer **initiatief definitie** aan de bovenkant van de pagina deze selectie gaat u naar de **initiatief definitie** formulier.
3. Voer de naam en beschrijving van het initiatief.

   In dit voorbeeld willen we om ervoor te zorgen dat resources zijn in overeenstemming met beleidsdefinities over het ophalen van beveiligde, de naam van het initiatief **beveiligde ophalen**, en de beschrijving is: **dit initiatief is gemaakt voor het verwerken van alle beleidsregels die zijn gekoppeld aan het beveiligen van bronnen**.

   ![Initiatief definitie](media/create-manage-policy/initiative-definition.png)

4. Blader door de lijst met **beschikbare definities** en selecteert u het beleid (s) die u wilt toevoegen aan dit initiatief. Voor onze **beveiligen van uw** -initiatief, voeg de volgende ingebouwde beleidsdefinities:
   - SQL Server versie 12.0 vereisen
   - Niet-beveiligde webtoepassingen in het Beveiligingscentrum bewaken.
   - Monitor strikte netwerk via in Security Center.
   - Controleren op mogelijke app Whitelisting in Security Center.
   - Niet-versleutelde VM-schijven in Security Center bewaken.

   ![Initiatief definities](media/create-manage-policy/initiative-definition-2.png)

   Na het selecteren van de beleidsdefinities in de lijst ziet u dit onder **beleidsregels en parameters**, zoals hierboven.

5. Selecteer **Maken**.

### <a name="assign-an-initiative-definition"></a>De definitie van een initiatief toewijzen

1. Ga naar de **definities** tabblad onder **ontwerpen**.
2. Zoeken naar de **beveiligen van uw** initiatief definitie die u hebt gemaakt.
3. Het initiatief definitie selecteren en selecteer vervolgens **toewijzen**.

   ![Een definitie toewijzen](media/create-manage-policy/assign-definition.png)

4. Vul de **toewijzing** formulier door te voeren:
   - naam: beveiligde toewijzing ophalen
   - Beschrijving: deze initiatief toewijzing is aangepast voor het afdwingen van deze groep van voor beleidsdefinities van de **Azure Advisor capaciteit Dev** abonnement
   - prijscategorie: standaard
   - bereik dat u deze toewijzing toegepast dat wilt op: **Azure Advisor capaciteit Dev**

5. Selecteer **toewijzen**. 

## <a name="resolve-a-non-compliant-or-denied-resource"></a>Een niet-compatibele of geweigerde resource oplossen

Na het bovenstaande voorbeeld, na het toewijzen van de beleidsdefinitie vereisen SQL server-versie 12.0, zou een SQL-server die is gemaakt met een andere versie ophalen geweigerd. In deze sectie lopen we bij het oplossen van een geweigerde poging een SQL-server met een andere versie gemaakt.

1. Selecteer **toewijzingen** in het linkerdeelvenster.
2. Blader door alle beleidstoewijzingen en start de *vereisen SQL Server versie 12.0* toewijzing.
3. Een uitzondering voor de resourcegroepen in die u probeert te maken van de SQL-server aanvragen. In dit geval we Microsoft.Sql/servers/databases zijn exclusief: *baconandbeer/Cheetos* en *baconandbeer/Chorizo*.

   ![Uitsluiting van aanvraag](media/create-manage-policy/request-exclusion.png)

   Andere manieren een geweigerde bron kan worden omgezet, bijvoorbeeld: uit naar de contactpersoon die zijn gekoppeld aan het beleid, hebt u een sterk reden voor dat de SQL-server gemaakt en het beleid rechtstreeks te bewerken als u toegang hebt tot is bereikt.

4. Selecteer **Opslaan**.

In deze sectie kunt u de denial-of hebt geprobeerd te maken van een SQL-server met versie 12.0, door het aanvragen van een uitsluiting aan de resources opgelost.

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om door te gaan werken met de volgende zelfstudies, geen clean up maakt van de resources in deze handleiding hebt gemaakt. Als u niet van plan bent om door te gaan, gebruikt u de volgende stappen uit voor het verwijderen van de toewijzingen of de definities die eerder is gemaakt:

1. Selecteer **definities** (of **toewijzingen** als u probeert te verwijderen van een toewijzing) in het linkerdeelvenster.
2. Zoeken naar het nieuwe beleid of initiatief definitie (of toewijzing) u zojuist hebt gemaakt.
3. Selecteer het beletselteken aan het einde van de definitie of de toewijzing en selecteer **definitie verwijderen** (of **toewijzing verwijderen**).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt doen u met succes het volgende:

> [!div class="checklist"]
> * Een beleid voor het afdwingen van een voorwaarde voor bronnen die u in de toekomst maakt toegewezen
> * Gemaakt en de definitie van een initiatief om bij te houden van naleving voor meerdere resources toe te wijzen
> * Een resource niet-compatibele of niet wordt opgelost
> * Een nieuw beleid geïmplementeerd in een organisatie

Bekijk dit artikel voor meer informatie over de structuur van de beleidsdefinities:

> [!div class="nextstepaction"]
> [Definitie beleidsstructuur](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure)
