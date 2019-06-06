---
title: Configureren en goedkeuren van just-in-time-toegang voor Azure Managed Applications
description: Hierin wordt beschreven hoe gebruikers van Azure Managed Applications aanvragen voor just-in-time-toegang tot een beheerde toepassing goedkeuren.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 55fbf7292ab894cad3de3de9e96ddc96fe0b79b3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481712"
---
# <a name="configure-and-approve-just-in-time-access-for-azure-managed-applications"></a>Configureren en goedkeuren van just-in-time-toegang voor Azure Managed Applications

Als een gebruiker van een beheerde toepassing, kunt u mogelijk niet vertrouwd geven van de uitgever van de permanente toegang tot de beheerde resourcegroep. Als u wilt geven u meer controle over het verlenen van toegang tot beheerde resources, voorziet Azure Managed Applications in een functie met de naam van just-in-time (JIT) toegang, dat zich momenteel in preview. Dit kunt u goed te keuren wanneer en hoe lang de uitgever heeft toegang tot de resourcegroep. De uitgever van de vereiste updates gedurende die tijd kunt maken, maar als deze tijd voltooid is, de toegang van de uitgever is verlopen.

De werkstroom voor het verlenen van toegang is:

1. De uitgever wordt toegevoegd een beheerde toepassing in de marketplace en geeft aan dat de toegang tot JIT beschikbaar is.

1. Tijdens de implementatie, moet u JIT-toegang voor uw exemplaar van de beheerde toepassing inschakelen.

1. U kunt de instellingen voor toegang tot JIT wijzigen na de implementatie.

1. De publisher stuurt een aanvraag voor toegang.

1. U goedkeuren de aanvraag.

In dit artikel richt zich op de acties die gebruikers toegang tot JIT inschakelen en goedkeuren van aanvragen uitvoeren. Zie voor meer informatie over het publiceren van een beheerde toepassing met toegang tot JIT, [just-in-time-toegang in Azure Managed Applications aanvragen](request-just-in-time-access.md).

> [!NOTE]
> Voor het gebruik van just-in-time-toegang, hebt u een [Azure Active Directory P2-licentie](../active-directory/privileged-identity-management/subscription-requirements.md).

## <a name="enable-during-deployment"></a>Tijdens de implementatie inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Een marketplace-vermelding voor een beheerde toepassing vinden met JIT is ingeschakeld. Selecteer **Maken**.

1. Bij het verstrekken van waarden voor de nieuwe beheerde toepassing, de **JIT-configuratie** stap kunt u JIT-toegang voor de beheerde toepassing uit te schakelen. Selecteer **Ja** voor **JIT-toegang inschakelen**. Deze optie is standaard geselecteerd voor beheerde toepassingen die worden gedefinieerd met behulp van JIT is ingeschakeld in de marketplace.

   ![Toegang configureren](./media/approve-just-in-time-access/configure-jit-access.png)

   U kunt alleen toegang tot JIT inschakelen tijdens de implementatie. Als u selecteert **Nee**, de uitgever opgehaald permanente toegang tot de beheerde resourcegroep. U kunt toegang tot JIT later niet inschakelen.

1. Als u wilt wijzigen van de standaardinstellingen van de goedkeuring, selecteer **JIT-configuratie aanpassen**.

   ![Toegang aanpassen](./media/approve-just-in-time-access/customize-jit-access.png)

   Een beheerde toepassing met JIT ingeschakeld heeft standaard de volgende instellingen:

   * Goedkeuringsmodus: automatische
   * Maximale duur – 8 uur
   * Fiatteurs: geen

   Wanneer de modus ' goedkeuren ' is ingesteld op **automatische**, de goedkeurders ontvangen een melding voor elke aanvraag, maar de aanvraag is automatisch goedgekeurd. Als de waarde **handmatige**, de goedkeurders een melding voor elke aanvraag ontvangen en een van deze moet worden goedgekeurd.

   De maximale activeringsduur Hiermee geeft u de maximale hoeveelheid tijd die een uitgever kan aanvragen voor toegang tot de beheerde resourcegroep.

   De lijst met goedkeurders is de Azure Active Directory-gebruikers die een van JIT-aanvragen kunnen goedkeuren. Als u wilt een goedkeurder kan toevoegen, selecteert u **goedkeurder toevoegen** en zoek naar de gebruiker.

   Na het bijwerken van de instelling, selecteer **opslaan**.

## <a name="update-after-deployment"></a>Na de implementatie bijwerken

U kunt de waarden voor hoe aanvragen worden goedgekeurd. Echter, als u geen toegang tot JIT tijdens de implementatie inschakelt, u niet inschakelen dit later opnieuw.

De instellingen voor een geïmplementeerde beheerde toepassing wijzigen:

1. Selecteer in de portal, de toepassing beheren.

1. Selecteer **JIT-configuratie** en de instellingen desgewenst wijzigen.

   ![Instellingen voor toegang wijzigen](./media/approve-just-in-time-access/change-settings.png)

1. Selecteer **Opslaan** wanneer u klaar bent.

## <a name="approve-requests"></a>Aanvragen goedkeuren

Als de uitgever toegang aanvraagt, krijgt u een melding van de aanvraag. U kunt goedkeuren JIT-aanvragen rechtstreeks via de beheerde toepassing of voor alle beheerde toepassingen via de Azure AD Privileged Identity Management-service. Voor het gebruik van just-in-time-toegang, hebt u een [Azure Active Directory P2-licentie](../active-directory/privileged-identity-management/subscription-requirements.md).

Om goed te keuren aanvragen via de beheerde toepassing:

1. Selecteer **toegang tot JIT** voor de beheerde toepassing en selecteer **aanvragen goedkeuren**.

   ![Aanvragen goedkeuren](./media/approve-just-in-time-access/approve-requests.png)
 
1. Selecteer de aanvraag goed te keuren.

   ![Selecteer aanvraag](./media/approve-just-in-time-access/select-request.png)

1. Geef de reden voor de goedkeuring en selecteer in het formulier **goedkeuren**.

Om goed te keuren aanvragen via Azure AD Privileged Identity Management:

1. Selecteer **alle services** en beginnen met zoeken naar **Azure AD Privileged Identity Management**. Selecteer in de beschikbare opties.

   ![Zoek naar service](./media/approve-just-in-time-access/search.png)

1. Selecteer **aanvragen goedkeuren**.

   ![Selecteer goedkeuren aanvragen](./media/approve-just-in-time-access/select-approve-requests.png)

1. Selecteer **Azure beheerde toepassingen**, en selecteert u de aanvraag goed te keuren.

   ![Selecteer aanvragen](./media/approve-just-in-time-access/view-requests.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het publiceren van een beheerde toepassing met toegang tot JIT, [just-in-time-toegang in Azure Managed Applications aanvragen](request-just-in-time-access.md).
