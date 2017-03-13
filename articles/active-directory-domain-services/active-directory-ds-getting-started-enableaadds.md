---
title: 'Azure AD Domain Services: Azure AD Domain Services inschakelen | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 64bb5f7e78a6e48faf3487da780597b25891a2fa
ms.lasthandoff: 12/07/2016


---
# <a name="enable-azure-ad-domain-services"></a>Azure AD Domain Services inschakelen
## <a name="task-3-enable-azure-ad-domain-services"></a>Taak 3: Azure AD Domain Services inschakelen
In deze taak schakelt u Azure AD Domain Services in voor uw directory. Voer de volgende configuratiestappen uit om Azure AD Domain Services in te schakelen voor uw directory.

1. Navigeer naar de **klassieke Azure Portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.
3. Selecteer de Azure AD-tenant (directory) waarvoor u Azure AD Domain Services wilt inschakelen.

    ![Azure AD-directory selecteren](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klik op het tabblad **Configureren**.

    ![Tabblad Configureren van directory](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Blader omlaag naar de sectie **Domeinservices**.

    ![Configuratiesectie Domeinservices](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)
6. Stel de optie **Domeinservices inschakelen voor deze directory** in op **Ja**. Er verschijnen nu nog een paar configuratieopties voor Azure AD Domain Services op de pagina.

    ![Domain Services inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Wanneer u Azure AD Domain Services inschakelt voor uw tenant, worden de referentie-hashes van Kerberos en NTLM die nodig zijn voor de verificatie van gebruikers, gegenereerd en opgeslagen door Azure AD.
   >
   >
7. Geef de **DNS-domeinnaam van domeinservices** op.

   * De standaarddomeinnaam van de directory (die eindigt met het domeinachtervoegsel **. onmicrosoft.com**) wordt standaard geselecteerd.
   * De lijst bevat alle domeinen die zijn geconfigureerd voor uw Azure AD-directory, inclusief de geverifieerde en niet-geverifieerde domeinen die u configureert op het tabblad Domeinen.
   * U kunt daarnaast ook een aangepaste domeinnaam invoeren. In dit voorbeeld is de aangepaste domeinnaam contoso100.com gebruikt.

     > [!WARNING]
     > Controleer of het domeinvoorvoegsel van de domeinnaam die u opgeeft (bijvoorbeeld 'contoso100' in de domeinnaam 'contoso100.com') minder dan 15 tekens lang is. U kunt geen Azure AD Domain Services-domein maken met een domeinvoorvoegsel dat langer is dan 15 tekens.
     >
     >
8. Controleer of de DNS-domeinnaam die u hebt gekozen voor het beheerde domein, nog niet bestaat in het virtuele netwerk. Controleer met name het volgende:

   * Of het virtuele netwerk al een domein met dezelfde DNS-domeinnaam bevat.
   * Of het virtuele netwerk dat u hebt geselecteerd, een VPN-verbinding heeft met uw on-premises netwerk en u een domein met dezelfde DNS-domeinnaam hebt op uw on-premises netwerk.
   * Of het virtuele netwerk een bestaande cloudservice met deze naam bevat.
9. De volgende stap bestaat uit het selecteren van een virtueel netwerk waarin Azure AD Domain Services beschikbaar moet zijn. Selecteer het virtuele netwerk en toegewezen subnet die u hebt gemaakt in de vervolgkeuzelijst **Domeinservices verbinden met dit virtuele netwerk**.

   * Controleer of het virtuele netwerk dat u hebt opgegeven, deel uitmaakt van een Azure-regio die wordt ondersteund door Azure AD Domain Services. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te zien in welke Azure-regio's Azure AD Domain Services beschikbaar is.
   * Virtuele netwerken die bij een regio horen waarin Azure AD Domain Services niet wordt ondersteund, worden niet weergegeven in de vervolgkeuzelijst.
   * Gebruik een toegewezen subnet in het virtuele netwerk voor Azure AD Domain Services. Zorg dat u niet de gatewaysubnet selecteert. Zie [Aandachtspunten voor netwerken](active-directory-ds-networking.md).
   * Virtuele netwerken die zijn gemaakt met Azure Resource Manager, worden ook niet weergegeven in de vervolgkeuzelijst. Virtuele netwerken op basis van Resource Manager worden momenteel niet ondersteund door Azure AD Domain Services.
10. Klik op **Opslaan** in het taakvenster onder aan de pagina om Azure AD Domain Services in te schakelen.
11. Op de pagina wordt 'In behandeling...' weergegeven, terwijl Azure AD Domain Services wordt ingeschakeld voor uw directory.

    ![Domain Services inschakelen - status In behandeling](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [!NOTE]
    > Azure AD Domain Services zorgt voor maximale beschikbaarheid voor uw beheerde domein. Nadat u Azure AD Domain Services hebt ingeschakeld, ziet u dat de IP-adressen waarop Domain Services beschikbaar zijn, een voor een worden weergegeven in het virtuele netwerk. Het tweede IP-adres wordt kort weergegeven zodra maximale beschikbaarheid is ingeschakeld voor uw domein. Wanneer maximale beschikbaarheid is geconfigureerd en actief is voor uw domein, ziet u twee IP-adressen in de sectie **Domeinservices** van het tabblad **Configureren**.
    >
    >
12. Na ongeveer 20-30 minuten ziet u het eerste IP-adres waarop Domain Services beschikbaar is in het virtuele netwerk in het veld **IP-adres** op de pagina **Configureren**.

    ![Domain Services ingeschakeld - eerste IP ingericht](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
13. Wanneer maximale beschikbaarheid is ingeschakeld voor uw domein, ziet u twee IP-adressen op de pagina. Het beheerde domein is op deze twee IP-adressen beschikbaar in het geselecteerde virtuele netwerk. Noteer de IP-adressen zodat u de DNS-instellingen voor het virtuele netwerk kunt bijwerken. In deze stap kunnen virtuele machines in het virtuele netwerk verbinding maken met het domein voor bewerkingen, zoals het koppelen van domeinen.

    ![Domain Services ingeschakeld - beide IP's ingericht](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Afhankelijk van de grootte van de Azure AD-tenant (aantal gebruikers, groepen enzovoort) kan de synchronisatie met het beheerde domein enige tijd duren. Dit synchronisatieproces wordt op de achtergrond uitgevoerd. Voor grote tenants met tienduizenden objecten kan het een dag of twee duren voordat alle gebruikers, groepslidmaatschappen en referenties zijn gesynchroniseerd.
>
>

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk
De volgende configuratietaak bestaat uit het [bijwerken van de DNS-instellingen voor het virtuele netwerk van Azure](active-directory-ds-getting-started-dns.md).

