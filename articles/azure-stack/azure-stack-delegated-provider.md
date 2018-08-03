---
title: Aanbiedingen in Azure Stack delegeren | Microsoft Docs
description: Informatie over het plaatsen van andere personen die verantwoordelijk is voor het maken van aanbiedingen en ondertekening van gebruikers voor u.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: 161a17f2360767dacc4f418a078c695d7cb8daa7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449748"
---
# <a name="delegate-offers-in-azure-stack"></a>Aanbiedingen in Azure Stack delegeren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als de Azure Stack-operators wilt u meestal om andere personen die verantwoordelijk is voor het maken van aanbiedingen en aanmelden van gebruikers. Bijvoorbeeld, als u een serviceprovider bent, kunt u wederverkopers aan klanten aanmelden en deze beheren namens. Of, als u deel uitmaakt van een centrale IT-groep in een onderneming bent, kunt u om te delegeren aanmelding door een gebruiker tot andere IT-personeel.

Delegering wordt het gemakkelijker te bereiken en te beheren van meer gebruikers die u zelf kunt uitvoeren. De volgende afbeelding ziet u één niveau van de delegatie van, maar ondersteunt meer dan één niveau van het Azure Stack. Gedelegeerde providers (DPs) kunnen delegeren aan andere providers, maximaal vijf niveaus.

![Niveaus van overdracht](media/azure-stack-delegated-provider/image1.png)

## <a name="understand-delegation-roles-and-steps"></a>Functies van de delegatie en stappen begrijpen

### <a name="delegation-roles"></a>De delegatie-rollen

De volgende rollen zijn onderdeel van de delegatie van:

* De *Azure Stack-operators* beheert de Azure Stack-infrastructuur en maakt u een sjabloon van de aanbieding. De operator voor netwerkapparaten delegeert anderen voor aanbiedingen voor hun gebruikers.

* De gedelegeerde Azure Stack-operators worden genoemd *gedelegeerde providers*. Ze kunnen deel uitmaken van andere organisaties, zoals andere gebruikers van Azure Active Directory (Azure AD).

* *Gebruikers* zich registreren voor de aanbiedingen en ze gebruiken voor het beheren van hun workloads, het maken van virtuele machines, opslag van gegevens, enzovoort.

### <a name="delegation-steps"></a>Stappen voor overdracht

Er zijn twee eenvoudige stappen voor het instellen van overdracht:

1. *Maken van een gedelegeerde provider* Abonneer u een gebruiker op een aanbieding op basis van een plan dat alleen de service van de abonnementen heeft. Gebruikers die zijn geabonneerd op deze aanbieding kunnen vervolgens aanbiedingen uitbreiden en registreren van gebruikers voor de aanbiedingen.

1. *Aanbieding delegeren aan de gedelegeerde provider*. Deze aanbieding is een sjabloon voor wat de gedelegeerde provider kan bieden. De gedelegeerde provider kan nu nemen van de aanbieding en aanbieden aan andere gebruikers.

De volgende afbeelding ziet u de stappen voor het instellen van overdracht.

![De gedelegeerde provider maken en ze te registreren van gebruikers](media/azure-stack-delegated-provider/image2.png)

**Vereisten voor gedelegeerde providers**

Om te fungeren als een gedelegeerde provider, moet een gebruiker een relatie tot stand brengen met de belangrijkste provider door een abonnement te maken. Dit abonnement geeft de gedelegeerde provider als bestaande uit het recht op het huidige aanbiedingen namens de belangrijkste provider.

Nadat u deze relatie tot stand is gebracht, kunt de Azure Stack-operator een aanbieding delegeren aan de gedelegeerde-provider. De gedelegeerde provider kan duren voordat de aanbieding, wijzig de naam (maar niet de inhoud te wijzigen), en deze te bieden aan klanten.

## <a name="delegation-walkthrough"></a>Overzicht van delegeren

De volgende secties bevatten een praktische scenario voor het instellen van een gedelegeerde provider en verifiëren van gebruikers kunnen zich aanmelden voor de gedelegeerde aanbieding een aanbieding wordt gedelegeerd.

### <a name="set-up-roles"></a>Rollen instellen

Als u wilt gebruiken in dit scenario, moet u twee Azure AD-accounts naast uw Azure Stack-operator-account. Als u deze twee accounts hebt, moet u om ze te maken. De accounts kunnen deel uitmaken van een Azure AD-gebruiker en worden aangeduid als de gedelegeerd provider en de gebruiker.

| **Rol** | **Organisatie-rechten** |
| --- | --- |
| Gedelegeerde provider |Gebruiker |
| Gebruiker |Gebruiker |

### <a name="identify-the-delegated-provider"></a>De gedelegeerde provider identificeren

1. Meld u aan de administrator-portal als Azure Stack-operators.

1. Maken van een aanbieding waarmee een gebruiker om te worden van een gedelegeerde provider:

   a.  [Maak een plan](azure-stack-create-plan.md).
       Dit plan moet alleen de service abonnementen bevatten. In dit artikel wordt een abonnement met de naam **PlanForDelegation** als voorbeeld.

   b.  [Maak een aanbieding](azure-stack-create-offer.md) op basis van dit plan. In dit artikel wordt gebruikgemaakt van een aanbieding met de naam **OfferToDP** als voorbeeld.

   c.  De gedelegeerde provider toevoegen als een abonnee op deze aanbieding door te selecteren **abonnementen** > **toevoegen** > **nieuwe Tenantabonnement**.

   ![De gedelegeerde provider toevoegen als een abonnee](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Als met alle Azure Stack-aanbiedingen, hebt u de optie van het maken van de aanbieding voor openbare en zodat gebruikers zich aanmelden voor, of het privé houden en laat het beheren van de aanmelding bij Azure Stack-operators. Gedelegeerde providers zijn meestal een kleine groep. U wilt om te bepalen wie is toegelaten, zodat het privé houden van deze aanbieding zinvol in de meeste gevallen.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure Stack-operators maakt de gedelegeerde aanbieding

De volgende stap is het maken van het plan en aanbieding die u wilt overdragen, en dat uw gebruikers wordt gebruikt. Er is een goed idee om te definiëren van deze aanbieding, precies zoals u wilt dat gebruikers zien omdat de gedelegeerde provider de abonnementen en quota's die het bevat niet wijzigen.

1. Als Azure Stack-operators, [maken van een plan](azure-stack-create-plan.md) en [een aanbieding](azure-stack-create-offer.md) op basis van het abonnement. In dit artikel wordt gebruikgemaakt van een aanbieding met de naam **DelegatedOffer** als voorbeeld.

   > [!NOTE]
   > Deze aanbieding hoeft te zijn openbaar, maar u kunt u deze openbaar maken als u wilt. In de meeste gevallen wilt u echter alleen gedelegeerde providers toegang hebben tot de aanbieding. Nadat u een persoonlijke aanbieding delegeren zoals beschreven in de volgende stappen, heeft de provider van gedelegeerde toegang toe.

1. De aanbieding delegeren. Ga naar **DelegatedOffer**. Onder **instellingen**, selecteer **gedelegeerde Providers** > **toevoegen**.

1. Selecteer het abonnement voor de gedelegeerde provider uit de vervolgkeuzelijst en selecteer vervolgens **gemachtigde**.

   ![Een gedelegeerde provider toevoegen](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Gedelegeerde provider past de aanbieding

Meld u aan bij de gebruikersportal aanmeldt als gedelegeerde-provider en maak vervolgens een nieuwe aanbieding met behulp van de gedelegeerde aanbieding als sjabloon.

1. Selecteer **nieuwe** > **Tenant aanbiedingen + plannen** > **bieden**.

    ![Een nieuwe aanbieding maken](media/azure-stack-delegated-provider/image5.png)

1. Een naam toewijzen aan de aanbieding. In dit artikel wordt gebruikgemaakt van **ResellerOffer** als voorbeeld. Selecteer de gedelegeerde aanbieding waarop u wilt baseren, en selecteer vervolgens **maken**.

   ![Een naam toewijzen](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >Het is belangrijk om te begrijpen dat in tegenstelling tot de Azure Stack-operators, een gedelegeerde provider niet van een aanbieding van basisplannen en aanvullende plannen maken. Gedelegeerde providers alleen aanbiedingen die zijn overgedragen voor hen kunnen kiezen, ze geen wijzigingen aanbrengen in aanbiedingen.

1. De aanbieding openbaar maken door te selecteren **Bladeren**, en vervolgens **biedt**. Selecteer de aanbieding en selecteer vervolgens **status wijzigen**.

1. Deze aanbiedingen via hun eigen portal wordt aangegeven dat de gedelegeerde provider URL. Deze aanbiedingen zijn alleen via de portal voor gedelegeerde zichtbaar. Om te zoeken en wijzig deze URL:

    a.  Selecteer **Bladeren** > **meer services** > **abonnementen**. Selecteer vervolgens de gedelegeerde-providerabonnement. Bijvoorbeeld, **DPSubscription** > **eigenschappen**.

    b.  Kopiëren van de portal URL naar een afzonderlijke locatie, zoals Kladblok.

    ![De gedelegeerde-providerabonnement selecteren](media/azure-stack-delegated-provider/dpportaluri.png)  

   U klaar bent met het maken van een gedelegeerde aanbieding als een gedelegeerde-provider. Meld u af als gedelegeerde-provider op en sluit het browservenster dat u gebruikt.

### <a name="sign-up-for-the-offer"></a>Aanmelden voor de aanbieding

1. In een nieuw browservenster geopend, gaat u naar de gedelegeerde portal-URL die u in de vorige stap hebt opgeslagen. Meld u aan de portal aan als een gebruiker.

   >[!NOTE]
   >De gedelegeerde aanbiedingen zijn niet zichtbaar, tenzij u de gedelegeerde-portal.

1. Selecteer in het dashboard, **Neem een abonnement**. U ziet dat alleen de gedelegeerde aanbiedingen die zijn gemaakt door de gedelegeerde provider worden weergegeven voor de gebruiker.

   ![Weergeven en selecteren van aanbiedingen](media/azure-stack-delegated-provider/image8.png)

Het proces van het overdragen van een aanbieding is voltooid. Nu kan een gebruiker zich registreren voor deze aanbieding met het ophalen van een abonnement voor deze.

## <a name="multiple-tier-delegation"></a>Overdracht van meerdere lagen

Overdracht van meerdere lagen kunt een gedelegeerde provider om te delegeren van de aanbieding voor een andere entiteiten. Bijvoorbeeld, kanalen voor het maken van diepere reseller waar:

* Een aanbieding met een distributeur voor netwerkapparaten delegeert de provider die wordt beheerd door Azure Stack.
* De distributor gemachtigden sturen naar een wederverkoper.

Voor het maken van meerdere lagen van de aanbieding delegatie, delegeert de gedelegeerde provider de aanbieding met de volgende provider. Het proces is hetzelfde voor de provider van gedelegeerde als bij de Azure Stack-operators. Zie voor meer informatie, [Azure Stack-operators maakt de gedelegeerde aanbieding](#cloud-operator-creates-the-delegated-offer).

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)