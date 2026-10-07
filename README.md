# epg-dazn

Guía XMLTV de los canales DAZN internacionales (DE, IT, PT, ES) y afines, generada a diario con
el grabber de [iptv-org/epg](https://github.com/iptv-org/epg).

**URL de la guía** (es lo que va en el `url-tvg` de la lista):

```
https://raw.githubusercontent.com/exdelins/epg-dazn/guide/dazn.xml
```

Se publica en la rama `guide` con un único commit, que se reescribe en cada ejecución: así el
repositorio no crece aunque el fichero sea de 1,6 MB al día.

## Qué hay aquí

- `dazn.channels.xml` — los 25 canales con el sitio y el `site_id` de cada uno. El `xmltv_id` es
  propio (`DAZN1DE`, `DAZN1IT`, `DAZN1PT`…) y **no** el de iptv-org: con los suyos, las tres
  señales de "DAZN 1" comparten id y la guía se cruza entre países.
- `.github/workflows/epg.yml` — cron diario a las 04:00 UTC, y ejecución manual.

## Cobertura

25 canales, unos 3.000 programas, 8 días de parrilla (de ayer a +6).

Sin fuente en ninguna parrilla, y por tanto fuera: DAZN F1, DAZN 1 (España), LALIGA TV
HYPERMOTION, DAZN RISE DE, DFB.TV DE y Radio TV Serie A IT.

DAZN FAST+ DE sale en la guía, pero `tv.blue.ch` solo devuelve relleno con el título "DAZN".

## Si un canal se queda vacío

Les pasa a los scrapers cuando la web de origen cambia. Comprobado el 7 oct 2026: Eurosport 1 y 2
IT daban 0 programas con `sky.com` **y también con `guida.tv`**; funcionan con `tv.blue.ch`.
Unbeaten daba 0 con `i.mjh.nz` y funciona con `plex.tv`.

Para cambiar de sitio, busca alternativas en la API de iptv-org y edita `dazn.channels.xml`:

```sh
curl -s https://iptv-org.github.io/api/guides.json | jq '.[] | select(.channel=="Eurosport1.fr")'
```

El paso «Comprobar que sirve para algo» del workflow falla si el total baja de 500 programas, para
que una guía inservible no se publique encima de la buena.
