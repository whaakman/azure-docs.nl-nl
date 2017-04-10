U gebruikt mogelijk een Azure-abonnement waarvoor u niet de beheerder of de eigenaar bent, zoals een abonnement dat eigendom is van het bedrijf. Als dit het geval is, moet u controleren of de volgende items zijn verkregen om de stappen in dit artikel te kunnen volgen:

* Toegang voor inzenders. Voor aanmelding bij Azure moet u ten minste toegangsrechten op Inzender-niveau hebben voor de Azure-resourcegroep. Deze resourcegroep wordt gebruikt om een HDInsight-cluster en andere Azure-resources te maken.
* Registratie van provider. Iemand met minimaal Inzender-toegang tot het Azure-abonnement moet de provider van de resource die u gebruikt, al hebben geregistreerd. Registratie van een provider vindt plaats wanneer een gebruiker met toegang tot het abonnement op het niveau van Inzender, voor het eerst een resource maakt onder het abonnement. Dit kan ook worden bereikt zonder dat er een resource wordt gemaakt door [een provider te registreren met behulp van REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Raadpleeg de volgende artikelen voor meer informatie over werken met toegangsbeheer:

* [Aan de slag met toegangsbeheer in Azure Portal](../articles/active-directory/role-based-access-control-what-is.md)
* [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../articles/active-directory/role-based-access-control-configure.md)
