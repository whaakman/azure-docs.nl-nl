<properties
    pageTitle="Azure AD Domain Services: een virtueel netwerk maken of selecteren | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>


# Een virtueel netwerk voor Azure AD Domain Services maken of selecteren

## Richtlijnen voor het selecteren van een virtueel netwerk van Azure
> [AZURE.NOTE] **Voordat u begint**: raadpleeg [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md) (Overwegingen voor netwerken voor Azure AD Domain Services).


## Taak 2: een virtueel netwerk van Azure maken
De volgende configuratietaak bestaat uit het maken van een virtueel Azure-netwerk waarin u Azure AD Domain Services wilt inschakelen. Als u al een bestaand virtueel netwerk hebt dat u wilt gebruiken, kunt u deze stap overslaan.

> [AZURE.NOTE] Controleer of het virtuele netwerk van Azure dat u maakt of wilt gebruiken met Azure AD Domain Services, deel uitmaakt van een Azure-regio die wordt ondersteund door Azure AD Domain Services. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.

Noteer de naam van het virtuele netwerk, zodat u het juiste virtuele netwerk selecteert wanneer u Azure AD Domain Services inschakelt in een volgende configuratiestap.

Voer de volgende configuratiestappen uit om een virtueel Azure-netwerk te maken waarin u Azure AD Domain Services wilt inschakelen.

1. Navigeer naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **Netwerken** in het linkerdeelvenster.

3. Klik op **Nieuw** in het taakvenster onder aan de pagina.

    ![Knooppunt Virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Selecteer bij het knooppunt **Netwerkservices** de optie **Virtueel netwerk**.

5. Klik op **Snel maken** om een virtueel netwerk te maken.

    ![Virtueel netwerk - Snel maken](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Geef een **Naam** op voor het virtuele netwerk. U kunt ook de **Adresruimte** of het **Maximum aantal VM's** opgeven voor dit netwerk. U kunt de DNS-serverinstelling voorlopig op Geen laten staan. Deze instelling wordt bijgewerkt nadat u Azure AD Domain Services hebt ingeschakeld.

7. Zorg ervoor dat u een ondersteunde Azure-regio selecteert in de vervolgkeuzelijst **Locatie**. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.

8. Klik op de knop **Een virtueel netwerk maken** om het virtuele netwerk te maken.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)

<br>

## Taak 3: Azure AD Domain Services inschakelen
Bij de volgende configuratietaak gaat u [Azure AD Domain Services inschakelen](active-directory-ds-getting-started-enableaadds.md).



<!--HONumber=Sep16_HO3-->


