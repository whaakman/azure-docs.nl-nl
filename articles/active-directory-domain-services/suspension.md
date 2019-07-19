---
title: 'Azure Active Directory Domain Services: Onderbroken domeinen | Microsoft Docs'
description: Beheerde domein opschorting en verwijdering
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 781a81589032c290cef7342e7210ee36f388b22a
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68233993"
---
# <a name="understand-the-suspended-states-for-an-azure-active-directory-domain-services-managed-domain"></a>Meer informatie over de onderbroken statussen voor een beheerde Azure Active Directory Domain Services domein

Als Azure Active Directory Domain Services (Azure AD DS) een beheerd domein gedurende een lange periode niet kan onderhouden, wordt het beheerde domein in een onderbroken status geplaatst. In dit artikel wordt uitgelegd waarom beheerde domeinen worden onderbroken en hoe een opgeschort domein kan worden hersteld.


## <a name="states-your-managed-domain-can-be-in"></a>Statussen van uw beheerde domein kunnen zijn

![Tijd lijn van onderbroken domein](media/active-directory-domain-services-suspension/suspension-timeline.PNG)

In de voor gaande afbeelding ziet u een overzicht van de mogelijke statussen van een beheerd domein van Azure AD DS.

### <a name="running-state"></a>Status wordt uitgevoerd
Een beheerd domein dat op de juiste wijze is geconfigureerd en regel matig wordt uitgevoerd, heeft de status **bezig met uitvoeren** .

**Wat u kunt verwachten**
* Micro soft kan de status van uw beheerde domein regel matig controleren.
* Domein controllers voor uw beheerde domein worden regel matig patches en bijgewerkt.
* Wijzigingen van Azure Active Directory worden regel matig gesynchroniseerd met uw beheerde domein.
* Reguliere back-ups worden gemaakt voor uw beheerde domein.


### <a name="needs-attention-state"></a>Status ' aandacht vereist '
Een beheerd domein bevindt zich in de attentie status **vereist** als een of meer problemen vereisen dat een beheerder actie onderneemt. Op de pagina status van uw beheerde domein worden een of meer waarschuwingen in deze status weer gegeven.

Als u bijvoorbeeld een beperkend NSG voor uw virtuele netwerk hebt geconfigureerd, kan micro soft uw beheerde domein mogelijk niet bijwerken en bewaken. Deze ongeldige configuratie activeert een waarschuwing die uw beheerde domein in de status ' aandacht vereist ' plaatst.

Elke waarschuwing heeft een reeks oplossings stappen. Sommige waarschuwingen zijn tijdelijk en worden automatisch opgelost door de service. U kunt een aantal andere waarschuwingen oplossen door de instructies in de bijbehorende oplossings stappen voor die waarschuwing te volgen. Voor een aantal kritieke waarschuwingen moet u contact opnemen met micro soft ondersteuning om een oplossing te verkrijgen.

Zie [problemen met waarschuwingen in een beheerd domein oplossen](troubleshoot-alerts.md)voor meer informatie.

**Wat u kunt verwachten**

In sommige gevallen (bijvoorbeeld als u een ongeldige netwerk configuratie hebt), zijn de domein controllers voor uw beheerde domein mogelijk onbereikbaar. Als uw beheerde domein zich in de status ' attentie ' bevindt, kan micro soft niet garanderen dat het wordt bewaakt, patcht, bijgewerkt of een back-up maakt.

* Uw beheerde domein bevindt zich in een slechte staat en de status controle wordt mogelijk gestopt totdat de waarschuwing is opgelost.
* Er kunnen geen patches of updates worden uitgevoerd voor domein controllers voor uw beheerde domein.
* Wijzigingen van Azure Active Directory mogelijk niet gesynchroniseerd met uw beheerde domein.
* Er kunnen, indien mogelijk, back-ups voor uw beheerde domein worden gemaakt.
* Als u waarschuwingen oplost die van invloed zijn op uw beheerde domein, kunt u deze mogelijk herstellen naar de status ' wordt uitgevoerd '.
* Kritieke waarschuwingen worden geactiveerd voor configuratie problemen waarbij micro soft uw domein controllers niet kan bereiken. Als dergelijke waarschuwingen niet binnen 15 dagen worden opgelost, wordt uw beheerde domein in de status ' opgeschort ' geplaatst.


### <a name="the-suspended-state"></a>De status ' opgeschort '
Een beheerd domein wordt in de **onderbroken** status geplaatst om de volgende redenen:

* Een of meer kritieke waarschuwingen zijn in 15 dagen niet opgelost. Kritieke waarschuwingen kunnen worden veroorzaakt door een onjuiste configuratie waarmee de toegang wordt geblokkeerd tot resources die nodig zijn voor Azure AD DS.
    * Bijvoorbeeld, de waarschuwing [AADDS104: De netwerk](alert-nsg.md) fout is langer dan 15 dagen in het beheerde domein niet meer geldig.
* Er is een probleem met de facturering van uw Azure-abonnement of uw Azure-abonnement is verlopen.

Beheerde domeinen worden onderbroken wanneer micro soft het domein doorlopend niet kan beheren, bewaken, patchen of een back-up maakt.

**Wat u kunt verwachten**
* Domein controllers voor uw beheerde domein worden onbeschikbaar gemaakt en zijn niet bereikbaar binnen het virtuele netwerk.
* Secure LDAP toegang tot het beheerde domein via internet (als deze is ingeschakeld) niet meer werkt.
* U ziet fouten in de verificatie bij het beheerde domein, aanmelden bij virtuele machines die lid zijn van een domein of verbinding maken via LDAP/LDAPS.
* Back-ups voor uw beheerde domein worden niet langer ondernomen.
* Synchronisatie met Azure AD stopt.

Nadat u de waarschuwing hebt opgelost, wordt het beheerde domein de status ' Suspended '. Vervolgens moet u contact opnemen met de ondersteuning.
Ondersteuning kan uw beheerde domein herstellen, maar alleen als er een back-up van minder dan 30 dagen bestaat.

Het beheerde domein blijft gedurende 15 dagen alleen in een onderbroken status. Als u uw beheerde domein wilt herstellen, raadt micro soft u aan om de kritieke waarschuwingen onmiddellijk op te lossen.


### <a name="deleted-state"></a>Status verwijderd
Een beheerd domein dat gedurende 15 dagen in de status ' opgeschort ' blijft, wordt **verwijderd**.

**Wat u kunt verwachten**
* Alle bronnen en back-ups voor het beheerde domein worden verwijderd.
* U kunt het beheerde domein niet herstellen en moet een nieuw beheerd domein maken om Azure AD DS te gebruiken.
* Nadat de app is verwijderd, worden er geen kosten in rekening gebracht voor het beheerde domein.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Hoe weet u of uw beheerde domein is onderbroken?
U ziet een [waarschuwing](troubleshoot-alerts.md) op de pagina Azure AD DS Health in de Azure portal die declareert dat het domein is onderbroken. De status van het domein bevat ook ' Suspended '.


## <a name="restore-a-suspended-domain"></a>Een opgeschort domein herstellen
Voer de volgende stappen uit om een domein met de status ' opgeschort ' te herstellen:

1. Ga naar de [pagina Azure Active Directory Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in het Azure Portal.
2. Selecteer het beheerde domein.
3. Selecteer in het linkerdeel venster **status**.
4. Selecteer de waarschuwing. De waarschuwings-ID is ofwel AADDS503 of AADDS504, afhankelijk van de oorzaak van de onderbreking.
5. Selecteer de oplossings koppeling die in de waarschuwing wordt gegeven. Volg vervolgens de stappen om de waarschuwing op te lossen.

Uw beheerde domein kan alleen worden hersteld op de datum van de laatste back-up. De datum van de laatste back-up wordt weer gegeven op de pagina status van uw beheerde domein. Wijzigingen die zijn opgetreden na de laatste back-up, worden niet hersteld. Back-ups voor een beheerd domein worden Maxi maal 30 dagen opgeslagen. Back-ups die ouder zijn dan 30 dagen, worden verwijderd.


## <a name="next-steps"></a>Volgende stappen
- [Waarschuwingen voor uw beheerde domein oplossen](troubleshoot-alerts.md)
- [Meer informatie over Azure Active Directory Domain Services](overview.md)
- [Neem contact op met het product team](contact-us.md)

## <a name="contact-us"></a>Contact opnemen
Neem contact op met het product team van Azure Active Directory Domain Services om [feedback te delen of voor ondersteuning](contact-us.md).
