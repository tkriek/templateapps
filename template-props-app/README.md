# ################## #
# Template props app #
# ################## #
# ######################################### #
# Kleine app met wat opties en voorbeelden. #
#                                           #
#                                           #
# Author : SMT                              #
# versie : 0.1                              #
#                                           #
# ######################################### #

Props word veelal gebruikt door een indexer om bepaalde parsing opties mee te geven.
Props word ook door een search head gebruikt voor search time parsing

deze app is specifiek voor index parsing.

Props.conf is uit het volgende opgebouwd:
Zoals alle config files binnen splunk is deze ook opgebouwd uit stanza -- settings

[<propsnaam>]
optie1 = waarde
optie2 = waarde

* deze stanza doet settings enablen voor bepaalde settings.
* 1 props.conf kan meerdere stanza's bevatten.
* als je een bepaalde optie niet zet zal de standaard gebruikt worden.

<propsnaam> kan zijn:
1. <sourcetype>, de sourcetype van het event.
2. host::<host>, dit gebruikt het host veld hier kan je een patroon neerzetten of een letterlijke host
3. source::<source>, dit gebruikt het source veld hier kan je een patroon neerzetten of een letterlijke source

Splunk auto parsing werkt heel goed echter als wij deze een beetje helpen spelen we heel wat resources vrij voor andere zaken.
Dit beinvloed ook de search performance omdat de indexer meer resources heeft om de zoekopdrachten af te ronden.
Met de onderstaande settings help je splunk

LINE_BREAKER = <regex> voorbeeld: ([\n\r]+)\d\d\w\w #belangrijk hier is dat deze regex altijd begint met ([\n\r]+)
TRUNCATE = <0 - 999999> hier geef je aan wanneer splunk stopt met 1 event indexeren en door gaar met een andere (dit is in bytes)
MAX_TIMESTAMP_LOOKAHEAD = <0 - 9999> geef hier aan tot hoe ver splunk moet kijken in het event voor timestamping. let op: dit is tot het laatste karakter van de timestamp
TIME_FORMAT = <strftime format> voorbeeld: %Y-%m-%d %H:%M:%S.%L refereerd naar: 2018-10-18 10:10:47.234
TIME_PREFIX = <regex> geef hier je regex tot het punt waar de timestamp begint voorbeeld als deze aan het begin begint: ^
TZ = <timezone> voorbeeld: UTC
SHOULD_LINEMERGE = <boolean> zorg dat deze op false staat.

### belangrijke resources ###
http://strftime.net/    #hier kan je met klikken het strftime formaat maken
https://regex101.com/   #hier kan je je regex testen op test data.
http://docs.splunk.com/Documentation/Splunk/7.2.0/Admin/Propsconf

### instalatie handleiding ###

-   ga naar de SRVSCMPR001 via de command line
-   log in met de juiste credentials
-   ga naar /opt/splunk/etc/master-apps
    - hier staan alle apps in die naar de indexers gedeployed worden.
-   edit de props.conf van een van de volgende:
    - rwg_indexer_all_n4_props of;
    - rwg_indexer_all_teams_props
    - of als dit niet voldoet maak een nieuwe map aan met dezelfde naming convention en maak in die map een map genaamd default
-   save
-   ga naar de gui van dezelfde machine en klik op settings --> indexer clustering
-   klik op het tabje edit --> configuration bundle actions
-   klik op Validate and Check Restart (hiermee controleer je of de aanpassing/toevoeging correct is qua syntaxen.)
-   als alles goed is klik je op push


## let op als bovenstaand mis gaat kan je de rollback knop gebruiken deze doet de laatste wijziging ongedaan  maken.


