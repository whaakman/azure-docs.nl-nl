---
title: Licenties toewijzen aan een groep in Azure Active Directory | Microsoft Docs
description: Licenties toewijzen aan gebruikers met behulp van Azure Active Directory-groep licentieverlening
services: active-directory
keywords: Azure AD-licentieverlening
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 250e84550fdf3253f28d682e85fe4bf855e40a4d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Licenties toewijzen aan gebruikers door het lidmaatschap in Azure Active Directory

Dit artikel begeleidt u bij het productlicenties toewijzen aan een groep gebruikers in Azure Active Directory (Azure AD) en vervolgens verifiëren dat ze correct zijn gelicentieerd.

In dit voorbeeld bevat de tenant een beveiligingsgroep met de naam **HR-afdeling**. Deze groep bevat alle leden van de afdeling human resources (ongeveer 1000 gebruikers). U Office 365 Enterprise E3-licenties toewijzen aan de hele afdeling. De Enterprise Yammer-service die opgenomen in het product moet tijdelijk worden uitgeschakeld totdat de afdeling is gereed om te gaan gebruiken. U wilt er ook voor het implementeren van Enterprise Mobility + Security licenties tot dezelfde groep gebruikers.

> [!NOTE]
> Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat u een licentie kan worden toegewezen aan een gebruiker, wordt de beheerder heeft de eigenschap Usage locatie op de gebruiker opgeven.

> Voor de licentietoewijzing van de groep, worden alle gebruikers zonder een gebruikslocatie opgegeven overgenomen door de locatie van de map. Als u gebruikers op meerdere locaties hebt, raden wij aan dat u altijd gebruikslocatie als onderdeel van de stroom van de gebruiker maken in Azure AD (bijvoorbeeld via AAD Connect-configuratie) - het resultaat van de licentietoewijzing zo dat altijd juist is en gebruikers ontvangen geen services op locaties die niet zijn toegestaan.

## <a name="step-1-assign-the-required-licenses"></a>Stap 1: De vereiste licenties toewijzen

1. Aanmelden bij de [ **Azure-portal** ](https://portal.azure.com) met een administratoraccount. Het account moet een globale beheerder rol of de gebruiker de accountbeheerder voor het beheren van licenties.

2. Selecteer **meer services** in het navigatiedeelvenster links en selecteer vervolgens **Azure Active Directory**. U kunt deze blade toevoegen aan Favorieten of vastmaken aan het portaldashboard.

3. Op de **Azure Active Directory** blade Selecteer **licenties**. Hiermee opent u een blade kunt u zien en beheren van alle licensable producten in de tenant.

4. Onder **alle producten**, Office 365 Enterprise E3 en de Enterprise Mobility + Security selecteren door de productnamen te selecteren. Selecteer voor het starten van de toewijzing **toewijzen** boven aan de blade.

   ![Alle producten toewijzen licentie](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. Op de **licentie toewijzen** blade, klikt u op **gebruikers en groepen** openen de **gebruikers en groepen** blade. Zoeken naar de groepsnaam *HR-afdeling*, selecteer de groep en vervolgens moet u ervoor zorgen door te klikken op **Selecteer** onderaan de blade.

   ![Selecteer een groep](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. Op de **licentie toewijzen** blade, klikt u op **toewijzingsopties (optioneel)**, weergeven met alle serviceniveaus opgenomen in de twee producten dat we eerder hebt geselecteerd. Vinden **Enterprise Yammer** en schakel het **uit** die service van de productlicentie uitschakelen. Controleren door te klikken op **OK** onderaan **toewijzingsopties**.

   ![Opties voor toewijzing](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. Voltooid de toewijzing voor de **licentie toewijzen** blade, klikt u op **toewijzen** onderaan de blade.

8. Een melding wordt weergegeven in de rechterbovenhoek waarin de status en het resultaat van het proces. Als de toewijzing aan de groep kan niet worden voltooid (bijvoorbeeld vanwege bestaande licenties in de groep), klikt u op de melding om informatie over het probleem weer te geven.

We hebt nu een licentiesjabloon voor de groep van de afdeling Personeelszaken opgegeven. Een achtergrondproces in Azure AD is gestart voor het verwerken van alle bestaande leden van die groep. Deze eerste bewerking kan even duren, afhankelijk van de huidige grootte van de groep. We beschrijven hoe u kunt controleren of het proces is voltooid en bepalen als verdere actie vereist voor het oplossen van problemen in de volgende stap.

> [!NOTE]
> U kunt dezelfde toewijzing starten vanaf een alternatieve locatie: **gebruikers en groepen** in Azure AD. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle groepen**. Gaat u naar de groep, selecteert u deze en Ga naar de **licenties** tabblad. De **toewijzen** knop boven op de blade opent u de blade licentie.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Stap 2: Controleer of de eerste toewijzing heeft voltooid

1. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle groepen**. Zoek de **HR-afdeling** die licenties zijn toegewezen aan.

2. Op de **HR-afdeling** Selecteer groep blade **licenties**. Hiermee kunt u snel bevestigen als licenties volledig zijn toegewezen aan gebruikers en als er geen fouten bevat die u nodig hebt om te zoeken in. De volgende informatie is beschikbaar:

   - Lijst van de productlicenties die momenteel aan de groep toegewezen zijn. Selecteer een item om weer te geven van de specifieke services die zijn ingeschakeld en wijzigingen aanbrengen.

   - Status van de meest recente licentie wijzigingen die zijn aangebracht aan de groep (als de wijzigingen worden verwerkt of als verwerking is voltooid voor alle leden van de gebruiker).

   - Informatie over gebruikers die zich in een foutstatus omdat licenties kunnen niet worden toegewezen naar hen.

   ![Opties voor toewijzing](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Meer gedetailleerde informatie over het verwerken van onder licentie **Azure Active Directory** > **gebruikers en groepen** > *groepsnaam* > **controlelogboeken**. Houd rekening met de volgende activiteiten:

   - Activiteit: **Start groep gebaseerd licentie wordt toegepast op gebruikers**. Dit wordt geregistreerd wanneer het systeem de licentietoewijzing wijziging in de groep opgehaald en toegepast op alle leden van de gebruiker begint. Bevat informatie over de wijziging die is gemaakt.

   - Activiteit: **voltooien groep gebaseerd licentie wordt toegepast op gebruikers**. Dit wordt geregistreerd wanneer het systeem klaar is met verwerking van alle gebruikers in de groep. Het bevat een overzicht van hoeveel gebruikers zijn verwerkt en hoeveel gebruikers groep licenties kunnen niet worden toegewezen.

   [Lees dit gedeelte](./active-directory-licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) voor meer informatie over hoe controlelogboeken kunnen worden gebruikt voor het analyseren van wijzigingen uit op basis van een groep licenties.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Stap 3: Controleren of er licentieproblemen en deze kunt oplossen

1. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle groepen**, en zoek de **HR-afdeling** die licenties zijn toegewezen aan.
2. Op de **HR-afdeling** Selecteer groep blade **licenties**. De melding boven op de blade geeft aan dat er 10 gebruikers die licenties kunnen niet worden toegewezen aan. Erop te klikken, opent u een lijst van alle gebruikers in een status-licentieverlening fout voor deze groep.
3. De **toewijzingen mislukt** kolom vertellen dat beide productlicenties kunnen niet worden toegewezen aan de gebruikers. De **belangrijkste reden voor fout** kolom bevat de oorzaak van de fout. In dit geval heeft **serviceplannen conflicterende**.

   ![Mislukte toewijzingen](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Selecteer een gebruiker opent de **licenties** blade. Deze blade ziet u alle licenties die momenteel aan de gebruiker toegewezen zijn. In dit voorbeeld wordt de gebruiker heeft de licentie voor Office 365 Enterprise E1 die is overgenomen van de **kioskmodus gebruikers** groep. Dit veroorzaakt een conflict met de E3-licentie die het systeem heeft geprobeerd om te passen vanaf de **HR-afdeling** groep. Als gevolg hiervan is geen van de licenties van die groep toegewezen aan de gebruiker.

   ![Licenties voor een gebruiker weergeven](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. U lost dit conflict op door de gebruiker uit verwijderen de **kioskmodus gebruikers** groep. Nadat u Azure AD de wijziging, verwerkt de **HR-afdeling** licenties correct zijn toegewezen.

   ![Correct toegewezen licentie](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functie voor het Licentiebeheer van de via groepen instellen, de volgende artikelen:

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](active-directory-licensing-whatis-azure-portal.md)
* [Opsporen en oplossen van licentieproblemen met een voor een groep in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Het migreren van afzonderlijke gebruikers met een licentie aan op basis van een groep licentieverlening in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory-licentieverlening voor aanvullende scenario's op basis van groep](active-directory-licensing-group-advanced.md)
