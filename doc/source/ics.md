# ICS / iCal

Add support for generic ICS / iCal files which are downloaded from a fix location. The waste type will be taken from the `summary` attribute.

This source has been successfully tested with the following service providers:

- [Abfall Landkreis Stade](https://abfall.landkreis-stade.de/)
- [AVL Ludwigsburg](https://www.avl-ludwigsburg.de/)
- [AWB Esslingen](https://www.awb-es.de/)
- [Awista Starnberg](https://www.awista-starnberg.de/)
- [AWM München](https://www.awm-muenchen.de)
- [EDG Entsorgung Dortmund](https://www.edg.de/)
- [Stadtreinigung Leipzig](https://www.stadtreinigung-leipzig.de/)
- [Entsorgungsgesellschaft Görlitz-Löbau-Zittau](https://www.abfall-eglz.de/abfallkalender.0.html) (Remove the year from the generated URL to always get the current year.)
- [Abfallwirtschaft Kreis Böblingen](https://www.lrabb.de/start/Service+_+Verwaltung/Abfuhrtermine.html) (API from AbfallPlus / Abfall.IO)
- [ASR Chemnitz](https://www.asr-chemnitz.de/kundenportal/entsorgungskalender/) (Service von https://asc.hausmuell.info/ics/ics.php)

## Configuration via configuration.yaml

```yaml
waste_collection_schedule:
  sources:
    - name: ics
      args:
        url: URL
        file: FILE
        offset: OFFSET
        method: METHOD
        params: PARAMS
        year_field: YEAR_FIELD
        split_at: SPLIT_AT
```

### Configuration Variables

**url**<br>
*(string) (optional)*

URL to ICS / iCal file. File will be downloaded using a HTTP GET request.

If the original url contains the current year (4 digits including century), this can be replaced by the wildcard `{%Y}` (see example below).

You have to specify either `url` or `file`!

**file**<br>
*(string) (optional)*

Local ICS / iCal file name. Can be used instead of `url` for local files.

You have to specify either `url` or `file`!

**offset**<br>
*(int) (optional, default: `0`)*

Offset in days which will be added to every start time. Can be used if the start time of the events in the ICS file are ahead of the actual date.

**method**<br>
*(string) (optional, default: `GET`)*

Method to send the URL `params`.

Need to be `GET` or `POST`.

**params**<br>
*(dict) (optional, default: None)*

Dictionary, list of tuples or bytes to send in the query string for the HTTP GET request. Only used if `url` is specified, not used for `file`.

**year_field**<br>
*(string) (optional, default: None)*

Field in params dictionary to be replaced with current year (4 digits including century).

**split_at**<br>
*(string) (optional, default: None)*

Delimiter to split event summary into individual collection types. If your service puts multiple collections types which occur at the same day into a single event, this option can be used to separate the collection types again.

## Examples

```yaml
waste_collection_schedule:
  sources:
    - name: ics
      args:
        url: "https://www.avl-ludwigsburg.de/fileadmin/Files/Abfallkalender/ICS/Privat/Privat_{%Y}_Ossweil.ics"
        offset: 1
```

```yaml
waste_collection_schedule:
  sources:
    - name: ics
      args:
        file: "test.ics"
```

```yaml
waste_collection_schedule:
  sources:
    - name: ics
      args:
        url: "https://www.abfallkalender-zak.de",
        params:
            city: 2,3,4
            street: 3
            types[]:
              - restmuell
                gelbersack
                papiertonne
                biomuell
                gruenabfall
                schadstoffsammlung
                altpapiersammlung
                schrottsammlung
                weihnachtsbaeume
                elektrosammlung
            go_ics: Download
        },
        year_field: year
```

```yaml
waste_collection_schedule:
  sources:
    - name: ics
      args:
        url: "https://www.eaw-rheingau-taunus.de/abfallkalender/calendar.ics?streetid=1429"
        split_at: ","
```
