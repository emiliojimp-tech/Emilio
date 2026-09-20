# Ubicaciones y contacto — Valemi Café

> Fuente: Google Maps (Composio, toolkit `google_maps`, búsquedas `GOOGLE_MAPS_TEXT_SEARCH`) + Apify Google Search Results Scraper (`apify/google-search-scraper`), consultados el 2026-09-20. Lista de sedes confirmada y ajustada por el usuario el 2026-09-20. Ver también `directives/catalogo_valemi_cafe_2025.md` para productos y precios.
>
> **Regla de uso:** esta es la única fuente válida de ubicaciones y contacto de Valemi Café. No inventar sedes ni datos de contacto que no aparezcan aquí.

## Instrucción de proceso: respuesta sobre ubicaciones

Cada vez que el chatbot mencione "locations"/ubicaciones, dé ejemplos de dónde queda Valemi Café, o un cliente pregunte dónde están ubicados, debe mencionar **siempre las 5 sedes juntas, completas**, nunca solo una — con la dirección de la tabla de abajo. Nunca ubicaciones genéricas o inventadas.

## Sedes de Valemi Café

| Sede | Dirección | Teléfono | Coordenadas | Google Maps |
|---|---|---|---|---|
| La Candelaria | Cra. 53 #61a-72, La Candelaria, Medellín, Antioquia | 301 7356179 | 6.2608403, -75.5678229 | [Ver](https://maps.google.com/?cid=15495968816233969647) |
| Hospital San Vicente Fundación Rionegro | Vía Aeropuerto - Llanogrande, Vereda La Convención km 2.3, Rionegro, Antioquia | — | 6.1520117, -75.4347689 | [Ver](https://www.google.com/maps/place/?q=place_id:ChIJk8LROwidRo4R4USppYAO0zM) |
| Hospital Pablo Tobón Uribe | Cl. 78B #69-240, Robledo, Medellín, Antioquia | — | 6.2768442, -75.5797040 | [Ver](https://www.google.com/maps/place/?q=place_id:ChIJhYLWICQpRI4R-kVDWQats1U) |
| Clínica Campestre — Medellín | Cl. 17 Sur #44-06, El Poblado, Medellín, Antioquia | — | 6.1878396, -75.5785487 | [Ver](https://maps.google.com/?cid=15251360592069204613) |
| Clínica Campestre — Rionegro | Vereda Guayabito, Don Diego-Llanogrande km 3, Rionegro, Antioquia | — | 6.1048479, -75.4598809 | [Ver](https://www.google.com/maps/place/?q=place_id:ChIJWQl7BACbRo4R9dQlXMkeNvU) |

Nota: "Clínica Campestre — Medellín" es la misma dirección que antes teníamos registrada como "El Poblado" — Valemi Café opera ahí dentro/junto a la Clínica del Campestre, no es una tienda de calle aparte. En la ficha de Google Maps de La Candelaria el negocio figura como **"Valemi"**, no "Valemi Café" — así está registrado en Google Business Profile; corregirlo requiere editar la ficha desde ahí, no desde este repo.

## Instrucción de proceso: distancia y sede recomendada

Cuando un cliente diga en qué barrio/zona vive o dé una dirección/punto de referencia, usar la conexión de Google Maps (Composio, toolkit `google_maps`) para calcular la distancia y el tiempo en carro desde ese punto hasta cada una de las 5 sedes de la tabla de arriba, y recomendar la sede más cercana:

1. Ubicar el punto del cliente con `GOOGLE_MAPS_GEOCODE_ADDRESS_WITH_QUERY` (barrio/zona + "Medellín, Colombia" o "Rionegro, Antioquia" según aplique).
2. Calcular distancia/tiempo a las 5 sedes con `GOOGLE_MAPS_COMPUTE_ROUTE_MATRIX` (`travelMode: DRIVE`, `units: METRIC`), usando las coordenadas de la tabla de arriba como destinos.
3. Responder con la sede más cercana (distancia en km y tiempo en carro), y opcionalmente las demás distancias si el cliente pregunta por todas.

**Ejemplo ya verificado (2026-09-20):** desde Puente Madero (El Poblado, Medellín) la sede más cercana es **Clínica Campestre — Medellín**, a ~3.5 km / ~9 min en carro. Las demás desde ese punto: La Candelaria ~9.2 km / ~16 min, Hospital Pablo Tobón Uribe ~10.9 km / ~23 min, Hospital San Vicente Fundación Rionegro ~23.2 km / ~38 min, Clínica Campestre — Rionegro ~31.2 km / ~47 min.

## Contacto oficial (Apify Google Search Scraper)

- Sitio web: https://valemicafe.com/ (no se pudo verificar contenido — Apify no logró cargarlo el 2026-09-20; el sitio puede estar caído o bloqueando el crawler)
- Teléfono: +57 4 2317424
- Email: ventas@valemicafe.com
- Facebook: https://www.facebook.com/valemicafe/
- Instagram: https://www.instagram.com/valemicafe/ (bio: "Repostería, panadería y postres artesanales. Desde 2010")
- Entrega a domicilio: 301 6365253 (dato ya usado en `catalogo_valemi_cafe_2025.md`)

## Notas

- Las direcciones de los 4 puntos en hospitales/clínicas fueron indicadas por el usuario (2026-09-20) y verificadas contra la ficha del hospital/clínica en Google Maps (no tienen ficha propia de Valemi Café en Maps, a diferencia de La Candelaria y Clínica Campestre Medellín).
