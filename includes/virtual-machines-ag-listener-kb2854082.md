Vervolgens als alle servers op het cluster werkt met Windows Server 2008 R2 of Windows Server 2012, moet u controleren of de hotfix [KB2854082](http://support.microsoft.com/kb/2854082) is geïnstalleerd op elk van de lokale servers of virtuele Azure-machines die deel van het cluster uitmaken. Een server of de virtuele machine die in het cluster, maar niet in de beschikbaarheidsgroep, moet u ook deze hotfix is geïnstalleerd hebben.

In de extern bureaublad-sessiehost voor elk van de clusterknooppunten, downloaden [KB2854082](http://support.microsoft.com/kb/2854082) naar een lokale map. Vervolgens installeert u de hotfix op elk clusterknooppunt sequentieel worden verwerkt. Als de clusterservice wordt momenteel uitgevoerd op het clusterknooppunt, wordt de server opnieuw opgestart aan het einde van de hotfixinstallatie.

> [!WARNING]
> De clusterservice wordt gestopt of opnieuw starten van de server heeft gevolgen voor de status van het quorum van uw cluster en de beschikbaarheidsgroep en kan ertoe leiden dat uw cluster offline te gaan. Als u wilt de hoge beschikbaarheid van uw cluster behouden tijdens de installatie, zorg ervoor dat:
> 
> * Het cluster heeft quorum optimale health. 
> * Voordat u de hotfix op een willekeurig knooppunt installeert, worden alle clusterknooppunten online zijn.
> * Voordat u de hotfix op een ander knooppunt in het cluster installeert, kunt u de installatie van de hotfix in voor uitvoering op één knooppunt, met inbegrip van de server volledig opnieuw te starten.
> 
> 

