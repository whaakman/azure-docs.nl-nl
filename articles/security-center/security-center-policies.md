---
title: Beveiligingsbeleid instellen in Azure Security Center | Microsoft Docs
description: In dit document leest u hoe u beveiligingsbeleid configureert in Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: yurid
ms.openlocfilehash: 67564e930310433bf4d51f7642bdd7ebf7e8e600
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Beveiligingsbeleid instellen in Azure Security Center
Dit document helpt u om beveiligingsbeleid in Security Center te configureren door u te begeleiden bij de stappen die nodig zijn om deze taak uit te voeren.


## <a name="how-security-policies-work"></a>Hoe werkt beveiligingsbeleid?
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. U kunt het beleid bewerken in Security Center of [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) gebruiken voor het maken van nieuwe definities, het definiëren van extra beleidsregels en het toewijzen van beleidsregels binnen beheergroepen (die de hele organisatie, een bedrijfseenheid, enzovoort kan vertegenwoordigen), en naleving van deze beleidsregels bewaken binnen deze bereiken.

> [!NOTE]
> Azure Policy is alleen beschikbaar als beperkte preview-versie. Klik [hier](https://aka.ms/getpolicy) om de preview te gebruiken. Lees [Beleid maken en beheren om naleving af te dwingen](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy) voor meer informatie over Azure-beleid.

## <a name="how-to-change-security-policies-in-security-center"></a>Beveiligingsbeleid wijzigen in Security Center
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen bewerken in Security Center. U kunt een beveiligingsbeleid alleen wijzigen als u een eigenaar, bijdrager of beveiligingsbeheerder van dat abonnement bent of van de bovenliggende beheergroep. Meld u aan bij Azure Portal en voer de volgende stappen uit om beveiligingsbeleid te bekijken in Security Center:

1. Ga op het dashboard van **Security Center** naar **Algemeen** en klik op **Beveiligingsbeleid**.
2. Selecteer het abonnement waarvoor u het beveiligingsbeleid wilt inschakelen.

    ![Beleidsbeheer](./media/security-center-policies/security-center-policies-fig10.png)

3. Klik in het gedeelte **BELEIDSONDERDELEN** op **Beveiligingsbeleid**.

    ![Beleidsonderdelen](./media/security-center-policies/security-center-policies-fig12.png)

4. Dit is het standaardbeleid dat via Azure Policy is toegewezen aan Security Center. U kunt items verwijderen die onder **BELEIDSREGELS EN PARAMETERS** staan, of u kunt andere beleidsdefinities toevoegen die onder **BESCHIKBARE OPTIES** staan. Klik hiervoor op het plusteken (+) naast de naam van de definitie.

    ![Beleidsdefinities](./media/security-center-policies/security-center-policies-fig11.png)

5. Als u meer gedetailleerde informatie over het beleid wilt, klikt u erop om een pagina met details te openen, evenals de JSON-code met de structuur van de [beleidsdefinitie(https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy#policy-definition-structure):

    ![Json](./media/security-center-policies/security-center-policies-fig13.png)

6. Klik op **Opslaan** als u klaar bent.

## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid configureert in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md). Leer de ontwerpoverwegingen kennen en leer deze in te plannen als u de overstap naar Azure Security Center wilt maken.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md). Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.
