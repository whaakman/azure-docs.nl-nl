---
title: Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving | Microsoft Docs
description: Dit artikel helpt u met de stappen voor het maken van een beleidsdefinitie voor het identificeren van niet-compatibele resources.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: b28e442a075e38a4fbe7b0d9d46f2c9d23e7c6fb
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving
Om naleving in Azure te begrijpen, moet u eerst weten hoe het er voorstaat met uw eigen huidige resources. In deze quickstart gaat u een beleidstoewijzing maken voor het identificeren van virtuele machines die geen beheerde schijven gebruiken.

Als u het proces helemaal hebt doorlopen, hebt u virtuele machines geïdentificeerd die geen beheerde schijven gebruiken, en daarom *niet compatibel* zijn.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze quickstart maken we een beleidstoewijzing en wijzen we de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren* toe.

1. Selecteer **Toewijzingen** in het linkerdeelvenster van de Azure Policy-pagina.
2. Selecteer **Beleid toewijzen** boven in het deelvenster **Toewijzingen**.

   ![Een beleidsdefinitie toewijzen](media/assign-policy-definition/select-assign-policy.png)

3. Klik op de pagina **Beleid toewijzen** op de knop ![Beleidsdefinitie](media/assign-policy-definition/definitions-button.png) naast het veld **Beleid** om de lijst met beschikbare definities te openen.

   ![Beschikbare beleidsdefinities openen](media/assign-policy-definition/open-policy-definitions.png)

   Azure Policy wordt geleverd met ingebouwde beleidsdefinities die u kunt gebruiken. U ziet ingebouwde beleidsdefinities als:

   - Tag en waarde afdwingen
   - Tag en waarde toepassen
   - SQL Server-versie 12.0 vereisen

4. Doorzoek uw beleidsdefinities en zoek de definitie *Virtuele machines die geen beheerde schijven gebruiken controleren* op. Klik op dat beleid en klik op **Toewijzen**.

   ![De juiste beleidsdefinitie vinden](media/assign-policy-definition/select-available-definition.png)

5. Geef een weergave**naam** op voor de beleidstoewijzing. In dit geval gebruiken we *Virtuele machines die geen beheerde schijven gebruiken controleren*. U kunt ook een optionele **Beschrijving** opgeven. De omschrijving biedt details over de manier waarop deze beleidstoewijzing alle virtuele machines identificeert die in deze omgeving zijn gemaakt en geen beheerde schijven gebruiken.
6. Wijzig de prijscategorie in**Standard** om ervoor te zorgen dat het beleid wordt toegepast op bestaande resources.

   Er zijn twee prijscategorieën in Azure Policy: *Gratis* en *Standard*. Met de prijscategorie Gratis kunt alleen beleid afdwingen op toekomstige resources, terwijl u met Standard ook beleid kunt afdwingen op bestaande resources om een beter inzicht te krijgen in uw nalevingsstatus. Omdat we met een beperkte preview werken, hebben we nog geen prijsmodel uitgebracht en ontvangt u geen factuur als u *Standard* selecteert. Zie [Prijzen voor Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy/) voor meer informatie over prijzen.

7. Selecteer het **Bereik** waarop u het beleid wilt toepassen.  Het bereik bepaalt op welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen.
8. Selecteer het eerder geregistreerde abonnement (of de eerder geregistreerde resourcegroep). In dit voorbeeld gebruiken we dit abonnement: **Azure Analytics Capacity Dev**. Uw opties zullen echter anders zijn.

   ![De juiste beleidsdefinitie vinden](media/assign-policy-definition/assign-policy.png)

9. Selecteer **Toewijzen**.

U bent er nu klaar voor om niet-compatibele resources te identificeren om inzicht te krijgen in de nalevingsstatus van uw omgeving.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

Selecteer **Naleving** in het linkerdeelvenster en zoek de beleidstoewijzing op die u hebt gemaakt.

![Beleidsnaleving](media/assign-policy-definition/policy-compliance.png)

Als er bestaande resources zijn die niet compatibel zijn met deze nieuwe toewijzing, worden deze weergegeven onder het tabblad **Niet-compatibele resources**.

Als een voorwaarde wordt geëvalueerd ten opzichte van uw bestaande resources en deze waar blijkt te zijn voor enkele ervan, worden deze resources gemarkeerd als niet-compatibel met het beleid. Hier volgt een tabel met informatie over hoe de acties die nu beschikbaar zijn samenwerken met het resultaat van het evalueren van de voorwaarde en de nalevingsstatus van uw resources.

|Resource  |Als voorwaarde in het beleid wordt geëvalueerd als  |Actie in het beleid   |Nalevingsstatus  |
|-----------|---------|---------|---------|
|Bestaat     |True     |Weigeren     |Niet-compatibel |
|Bestaat     |False    |Weigeren     |Compatibel     |
|Bestaat     |True     |Toevoegen   |Niet-compatibel |
|Bestaat     |False    |Toevoegen   |Compatibel     |
|Bestaat     |True     |Controleren    |Niet-compatibel |
|Bestaat     |False    |Controleren    |Niet-compatibel |

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling zijn op deze quickstart gebaseerd. Als u door wilt gaan met andere zelfstudies, verwijdert u de resources die u in deze quickstart hebt gemaakt, niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.
1. Selecteer **Toewijzingen** in het linkerdeelvenster.
2. Zoek naar de toewijzing die u zojuist hebt gemaakt.

   ![Een toewijzing verwijderen](media/assign-policy-definition/delete-assignment.png)

3.  Selecteer **Toewijzing verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een beleidsdefinitie toegewezen aan een bereik om er voor te zorgen dat alle resources in dat bereik compatibel zijn en de resources die dat niet zijn te identificeren.

Voor meer informatie over het toewijzen van beleid om er voor te zorgen dat **toekomstige** resources die worden gemaakt compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./create-manage-policy.md)
