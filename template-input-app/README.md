# ################## #
# Template input app #
# ################## #
# ######################################### #
# Kleine app met wat opties en voorbeelden. #
#                                           #
#                                           #
# Author : SMT                              #
# versie : 0.1                              #
#                                           #
# ######################################### #


in deze app staan een aantal voorbeelden om je data naar splunk te krijgen.

in de map default vind je een config file <inputs.conf>
in deze file staan een aantal voorbeelden hoe je een inputs.conf zou kunnen inrichten.

Let op deze niet 1 op 1 overnemen maar alle velden invullen.

een config item binnen in een config file begint altijd met het benoemen van een zogenoemde "stanza" deze ziet er als volgt uit:
[<stanzanaam>]
setting1 = waarde
setting2 = waarde
etc..

voor inputs zijn een paar settings belangrijk:

in de stanza naam geef je aan wat je wilt doen.
[monitor://] dit is een prefix om een file of pad te monitoren.
[monitor:///path/to/file/] voledig uitgeschreven

Belangrijk als je wildcards gaat gebruiken:
* probeer dit ten alle tijden te voorkomen en zo specifiek mogelijk te zijn.
    - zaken zoals logrotate kunnen hierdoor mee genomen worden en daardoor dubbele events ontstaan.
* Je kan in een stanza ook een wildcard gebruiken. gebruik
  "..." om alle onderliggende folders te matchen en "*" om alles binnen een folder te matchen.
* "..." gaat door folders heen, als je volgend gebruikt: /foo/.../bar zullen de volgende folders gematched worden:
  foo/1/bar, foo/1/2/bar, etc.
* je kan meerdere "..." gebruiken in 1 input stanza. voorbeeld: /foo/.../bar/...
* * matched alles in een folder
  voorbeeld: /foo/*/bar matched de files
  /foo/1/bar, /foo/2/bar, etc. maar niet :
  /foo/bar of /foo/1/2/bar.
  2e voorbeeld: /foo/m*r/bar matched /foo/mr/bar, /foo/mir/bar,
  /foo/moor/bar, etc.
* je kan de beide wildcards ook combineren, voorbeeld:  foo/.../bar/* matched elke file in een bar directory dus: foo/1/bar/file of foo/2/bar/file



host = hier kan je veranderen welke host je wilt mee geven(is een optionele optie)
source = hier kan je veranderen welke source je wilt mee geven (is een optionele optie)
index = hier benoem je in welke index je data heen gaat
sourcetype = hier benoem je welk sourcetype je data krijgt (Let op! dit word gebruikt op de indexing laag om je data te parsen.)
disabled = <boolean> zet de input uit of aan 1 of true is uit 0 of false is aan.
whitelist = <regex> alles wat hier in staat word mee genomen 
blacklist = <regex> alles wat hier in staat word niet mee genomen.


### belangrijke resources ###
http://docs.splunk.com/Documentation/Splunk/7.1.3/Admin/Inputsconf
http://docs.splunk.com/Documentation/SplunkCloud/7.0.5/Data/Monitorfilesanddirectorieswithinputs.conf
https://docs.splunk.com/Documentation/Splunk/7.1.3/Data/MonitorWindowseventlogdata

### instalatie handleiding ###

-   ga naar de SRVDSPR001 via de command line
-   log in met de juiste credentials
-   ga naar /opt/splunk/etc/deployment-apps
    - hier staan alle apps in die naar de forwarders gedeployed worden.
-   maak een nieuwe folder aan met naming convention rwg_<n4/teams>_<m1/prod>_<typelog>_forwarder_inputs
    - voorbeeld: rwg_n4_prod_navis_cluster_bento_forwarder_inputs
-   maak in deze app een folder aan die heet default
-   maak in de map default een file die heet inputs.conf
-   in inputs.conf zet al je inputs voor deze specifieke use case
-   save
-   ga naar de gui van dezelfde machine en klik op settings --> forwarder management
-   klik op het tabje serverclasses --> nieuw
-   geef de serverclass dezelfde naam en koppel de app met de serverclass
-   voeg clients toe door het whitelist boxje te vullen.