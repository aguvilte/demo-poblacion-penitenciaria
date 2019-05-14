Para la Argentina la proyección oficial es la Gauss-Kruger, y esta difiere bastante de la proyección Mercator que es normalmente utilizada en mapas digitales. Si queremos desplegar el territorio argentino de manera de lograr una representación armoniosa que conserve las formas y áreas, lo recomendable es usa Gauss-Kruger.

![Transverse Mercator vs Mercator](https://gist.github.com/jkutianski/6532516/raw/2dff3f80e07b3722e65251ff389ce9124407139a/tmercvsmerc.png)

En D3.js no esta soportada esta proyeccion pero se puede optar por la proyección [Mercator transversa](http://en.wikipedia.org/wiki/Transverse_Mercator) `d3.geo.transverseMercator()` que es la base para la definición de la referencia cartográfica Gauss-Kruger. Esta proyección está definida dentro del plugin `d3.geo.projection.js` por lo que deberá ser cargado junto con `d3.js`.

Para poder calcular los parámetros necesarios para aplicar esta proyección es necesario calcular el centro geográfico de la Argentina continental, que está comprendida entre los paralelos de 22° y 55° de latitud sur y los meridianos de 53° y 74° de longitud oeste.

Para esto utilizaremos la siguiente formula:

 ![lat = ((-55°) - (-22°)) / 2 + (-22°) = -38.5°](https://gist.github.com/jkutianski/6532516/raw/18cdb453f1454c9f62ed7f655b1a89e623ca6fdb/lat.png)

 ![long = -(((-74) - (-53)) /2 + (-53)) = 63.5](https://gist.github.com/jkutianski/6532516/raw/cb3bfbd0297d3d8d2550b7991b67fdcc67dc2104/long.png)

La Argentina esta dividida en 7 fajas de 3° de ancho. La faja 3 que corresponde al meridiano de 66° es la utilizada en el caso de querer representar todo el territorio Argentino.

![Fajas Gauss Kruger Argentina](https://gist.github.com/jkutianski/6532516/raw/193b010204f4192fb3e6b028e4f67298cee3a4ef/zones.png)

Para poder centrar el mapa es necesario moverlo 2.5° que es la diferencia entre el meridiano 66° y la longitud del centro geografico.

Dados estos valores podremos definir la ubicación del centro geográfico en la proyección con `.center([2.5, -38.5])` y `.rotate([66, 0])`. 

Para la escala he ajustado empíricamente la siguiente formula:

 ![56.5 * map.height / ((-55) - (-22))](https://gist.github.com/jkutianski/6532516/raw/5966443b64ea695d0c886c99f768de62e8ccbbe7/scale.png)

la cual se aplica en D3.js con el siguiente comando `.scale((56.5 * map.height) / 33)`.

![Universal Transverse Mercator](http://upload.wikimedia.org/wikipedia/commons/b/b9/Usgs_map_traverse_mercator.PNG)