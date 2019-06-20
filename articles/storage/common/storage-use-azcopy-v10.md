---
title: Gegevens kopiëren of verplaatsen naar Azure Storage met behulp van AzCopy v10 | Microsoft Docs
description: AzCopy is een opdrachtregelprogramma die u gebruiken kunt om gegevens te kopiëren naar, uit of tussen opslagaccounts. Dit artikel helpt u AzCopy downloaden, verbinding maken met uw storage-account en vervolgens bestanden over te dragen.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 722097f1a61a10cd45c0c330e998021cd1abf0c8
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147962"
---
# <a name="get-started-with-azcopy"></a>Aan de slag met AzCopy

AzCopy is een opdrachtregelprogramma waarmee u kunt blobs of bestanden kopiëren naar of van een storage-account. Dit artikel helpt u AzCopy downloaden, verbinding maken met uw storage-account en vervolgens bestanden over te dragen.

> [!NOTE]
> AzCopy **V10** is de momenteel ondersteunde versie van AzCopy.
>
> Als u wilt gebruiken van AzCopy **v8.1**, Zie de [gebruikt u de vorige versie van AzCopy](#previous-version) sectie van dit artikel.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>AzCopy downloaden

Download eerst het uitvoerbare bestand van AzCopy V10 naar een map op uw computer. 

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Als u wilt kopiëren van gegevens naar en uit uw [Azure Table storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) -service en vervolgens installeren [AzCopy versie 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>AzCopy uitvoert

Voor het gemak, houd rekening met de Active directory-locatie van het uitvoerbare bestand van AzCopy toe te voegen aan het systeempad staat voor gebruiksgemak. Op die manier kunt u typen `azcopy` uit een map op uw systeem.

Als u niet de AzCopy-map toevoegen aan uw path, hebt u mappen wijzigen naar de locatie van het uitvoerbare bestand van AzCopy en hetzelfde type `azcopy` of `.\azcopy` in Windows PowerShell-opdracht wordt gevraagd.

Een lijst met opdrachten wilt bekijken, typt u `azcopy -h` en druk vervolgens op de ENTER-toets.

Voor meer informatie over een specifieke opdracht, de naam van de opdracht NET bevatten (bijvoorbeeld: `azcopy list -h`).

![Inline-hulp](media/storage-use-azcopy-v10/azcopy-inline-help.png)

> [!NOTE] 
> Als een eigenaar van uw Azure Storage-account, worden niet u automatisch machtigingen voor toegang tot gegevens toegewezen. Voordat u iets zinvolle met AzCopy doen kunt, moet u bepalen hoe u autorisatiereferenties om de storage-service te bieden. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Kies hoe u autorisatiereferenties bieden

U kunt autorisatiereferenties opgeven met behulp van Azure Active Directory (AD) of met behulp van een Shared Access Signature (SAS)-token.

Deze tabel als richtlijn gebruiken:

| Opslagtype | Momenteel ondersteunde methode van autorisatie |
|--|--|
|**Blob Storage** | Azure AD & SAS |
|**BLOB-opslag (hiërarchische naamruimte)** | Alleen Azure AD |
|**Bestandsopslag** | Alleen SAS |

### <a name="option-1-use-azure-ad"></a>Optie 1: Gebruik Azure AD

Het niveau van de machtiging die u nodig hebt, is gebaseerd op of u wilt bestanden uploaden of ze alleen te downloaden.

Als u alleen wilt om bestanden te downloaden, klikt u vervolgens controleren of de [gegevenslezer voor Opslagblob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) is toegewezen aan uw identiteit.

Als u uploaden van bestanden wilt, controleert u of dat een van deze rollen is toegewezen aan uw identiteit:

- [Gegevensbijdrager voor Blob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [De eigenaar van een opslag-Blob-gegevens](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Deze rollen kunnen worden toegewezen aan uw identiteit in een van deze bereiken:

- Container (bestandssysteem)
- Storage-account
- Resourcegroep
- Abonnement

Zie voor meer informatie over het controleren en toewijzen van rollen, [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

U hoeft niet beschikken over een van deze rollen toegewezen aan uw identiteit als je identiteit wordt toegevoegd aan de toegangsbeheerlijst (ACL) van de doelcontainer of map. In de ACL moet je identiteit schrijfmachtiging hebben voor de doelmap en voert u de machtigingen op de container en de bovenliggende map.

Zie voor meer informatie, [toegangsbeheer in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-your-identity"></a>Uw identiteit verifiëren

Nadat u hebt gecontroleerd dat de identiteit is gegeven het benodigde autorisatieniveau, open een opdrachtprompt, typ de volgende opdracht en druk vervolgens op de ENTER-toets.

```azcopy
azcopy login
```

Als u werkzaam bent bij meer dan één organisatie, opgenomen in de tenant-ID van de organisatie waarvan het opslagaccount dat deel uitmaakt.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Vervang de `<tenant-id>` tijdelijke aanduiding door de tenant-ID van de organisatie waarvan het opslagaccount dat deel uitmaakt. Als u wilt zoeken in de tenant-ID, selecteer **Azure Active Directory > Eigenschappen > map-ID** in Azure portal.

Met deze opdracht retourneert een verificatiecode op te geven en de URL van een website. Open de website, de op te geven en kies vervolgens de **volgende** knop.

![Een container maken](media/storage-use-azcopy-v10/azcopy-login.png)

Er wordt een venster Aanmelden weergegeven. In dit venster, meld u aan bij uw Azure-account met behulp van de referenties van uw Azure-account. Nadat u hebt aangemeld, kunt u het browservenster sluiten en beginnen met behulp van AzCopy.

### <a name="option-2-use-a-sas-token"></a>Optie 2: Een SAS-token gebruiken

U kunt een SAS-token toevoegen aan elke bron of doel-URL die gebruikmaken van in de AzCopy-opdrachten.

In dit voorbeeld van de opdracht recursief kopieert gegevens van een lokale map naar een blob-container. Een fictieve SAS-token is toegevoegd aan het einde van de van de container-URL.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Zie voor meer informatie over SAS-tokens en het verkrijgen van een [Using shared access signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Overdracht van bestanden

Nadat u hebt uw identiteit wordt geverifieerd of een SAS-token verkregen, kunt u beginnen met het overdragen van bestanden.

Als u Voorbeeldopdrachten, bezoekt u een van deze artikelen.

- [Gegevens overdragen met AzCopy en blob-opslag](storage-use-azcopy-blobs.md)

- [Gegevens overdragen met AzCopy en bestandsopslag](storage-use-azcopy-files.md)

- [Gegevens overdragen met AzCopy en Amazon S3 buckets](storage-use-azcopy-s3.md)

## <a name="use-azcopy-in-a-script"></a>AzCopy gebruiken in een script

Na verloop van tijd, de AzCopy [downloadkoppeling](#download-and-install-azcopy) verwijst naar de nieuwe versies van AzCopy. Als uw script AzCopy downloadt, kan het script mogelijk niet meer werkt als een nieuwere versie van AzCopy functies die afhankelijk van uw script wijzigt. 

Om te voorkomen dat deze problemen, verkrijgen van een statische (niet wijzigen) koppelen aan de huidige versie van AzCopy. Op die manier uw script wordt gedownload exact dezelfde versie van AzCopy elke keer dat deze wordt uitgevoerd.

Als u de koppeling, kunt u deze opdracht uitvoeren:

| Besturingssysteem  | Opdracht |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Voor Linux, `--strip-components=1` op de `tar` opdracht verwijdert u de op het hoogste niveau map met de naam van de en haalt u in plaats daarvan het binaire bestand rechtstreeks in de huidige map. Hiermee wordt het script dat moet worden bijgewerkt met een nieuwe versie van `azcopy` door bij te werken alleen de `wget` URL.

De URL wordt weergegeven in de uitvoer van deze opdracht. Het script kunt vervolgens AzCopy downloaden met behulp van deze URL.

| Besturingssysteem  | Opdracht |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

## <a name="use-azcopy-in-storage-explorer"></a>AzCopy gebruiken in Storage Explorer

Als u wilt en biedt daardoor de prestatievoordelen van AzCopy, maar u liever met Storage Explorer in plaats van de opdrachtregel om te communiceren met uw bestanden, schakelt u vervolgens AzCopy in Storage Explorer. 

Kies in Storage Explorer **Preview**->**AzCopy gebruiken voor verbeterde Blob uploaden en downloaden**.

![AzCopy inschakelen als een engine voor overdracht in Azure Storage Explorer](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> U hebt geen Schakel deze instelling als u een hiërarchische naamruimte hebt ingeschakeld op uw storage-account. Dat komt doordat Opslagverkenner maakt automatisch gebruik van AzCopy op storage-accounts waarvoor een hiërarchische naamruimte.  

Storage Explorer uw accountsleutel gebruikt voor het uitvoeren van bewerkingen, dus nadat u zich bij de Storage Explorer aanmeldt, hoeft u niet meer autorisatiereferenties op te geven.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Gebruik de vorige versie van AzCopy

Als u wilt gebruiken van de vorige versie van AzCopy (AzCopy v8.1), ziet u een van de volgende koppelingen:

- [AzCopy in Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [AzCopy in Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configureer, optimaliseren en oplossen van AzCopy

Zie [configureren, te optimaliseren en oplossen van AzCopy](storage-use-azcopy-configure.md)

## <a name="next-steps"></a>Volgende stappen

Als u vragen of problemen met algemene feedback te sturen, dient u ze [op GitHub](https://github.com/Azure/azure-storage-azcopy) pagina.
