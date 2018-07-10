# SimbelEcommerce API Shop #
Manual de uso



## Objetivo ##

Este documento describe los distintos endpoints de la API Simbel Ecommerce.

Se presupone conocimientos básicos sobre cómo implementar y consumir una API REST.


## Requisitos ##

Antes de comenzar, vas a necesitar un token que normalmente es suministrado vía email.

Este token tendrá que ser enviado dentro de los headers en todas las peticiones que se hagan.

Por ejemplo:

```json

curl -X GET \
  http://dev.api.simbel.com.ar/v1/products \
  -H 'cache-control: no-cache' \
  -H 'x-api-key: 5XXX738A83B40XXXX44D46E0032EXXXX'
```

La URL base de pruebas a utilizar es la siguiente:

[http://dev.api.simbel.com.ar/v1/](http://dev.api.simbel.com.ar/v1/)


## Respuestas ##

El formato de respuestas utilizado es JSON y tiene los siguientes campos:


```json

{
    "success": true,
    "data": [],
    "error":{
         "code": 1234,
         "message": “Error message”
    }
}

```



## Endpoints ##

La API ofrece los siguientes endpoints:

###  Productos ###

Pueden obtenerse todos los productos disponibles en el inventario de la tienda.


GET /products

Devuelve un listado de todos los productos, limitado a 10 productos por página.



**Parámetros:**

| **Campo** | **Tipo** | **Descripción** |
| --- | --- | --- |
| search | String | Permite hacer búsquedas. Por ejemplo: &#39;silla&#39; |
| categoryId | Integer | Filtra por la categoría del producto. |
| brandId | Integer | Devuelve sólo los produtos de cierta marca. |
| orderBy | String | Ordena los resultados. Los valores posibles son: price, name. |
| orderDirection | String | El sentido de ordenamiento. Los valores posibles son: desc, asc. |
| page | Integer | La página actual. Por defecto es 1. |
| limit | Integer | El límite de productos por página. Por defecto es 10. |

Ejemplo:


```json

{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Juego 4 Sillas",
            "code": "SKU_TEST",
            "price": 5000,
            "description": "<p>Juego de 4 sillas + 2 sillones</p>",
            "slug": "juego-4-sillas-2-sillones",
            "images": [
                {
                    "id": 1894,
                    "url": "http://www.tienda.com.ar/public/images/productos/producto_519_1.jpg",
                    "type": "product"
                }
            ],
            "category": {
                "id": 1,
                "name": "Diseño"
            },
            "brand": {
                "id": 3,
                "name": "Marca"
            },
            "active": true,
            "attributes": [
                {
                    "id": 22,
                    "name": "Color",
                    "value": "Verde"
                }
            ],
            "tags": [
                "sillon",
                "sillones cromo",
                "cromo blanco y negro",
                "sillon cromo",
                "eames verde"
            ]
        }
   ]
}

```




**GET /products/{id}**

Devuelve un producto específico, en el formato mencionado en el ejemplo anterior.

Si no existe el producto, se devuelve success en falso y el objeto &#39;error&#39; tendrá la descripción del error.



### Categorías ###

Este endpoint devuelve un listado con las categorías que agrupan ciertos productos.

**GET /categories**

Devuelve un listado de categorías principales (aquellas que no tengan categoría padre), limitado a 30 categorías por página.

Parámetros:

| **Campo** | **Tipo** | **Descripción** |
| --- | --- | --- |
| page | Integer | La página actual. Por defecto es 1. |
| limit | Integer | El límite de categorías por página. Por defecto es 10. |



Ejemplo:


```json


{
    "success": true,
    "data": [
        {
            "id": 53,
            "name": "SALE",
            "parent": null,
            "description": "",
            "slug": "sale",
            "tags": [
                "promociones",
                "hot sale",
                "hotsale",
                "promociones hotsale",
                "ofertas hotsale",
                "oferta",
                "promo hot sale",
                "promo hot",
                "mega ofertas",
                "megaofertas",
                "promos",
                "megapromos",
                "promo"
            ],
            "temporal": true,
            "images": [],
            "active": true,
            "children": []
        }
   ]
}

```




**GET /categories/{id}**

Devuelve una categoría y sus hijas.




### Ordenes ###

Este endpoint devuelve un listado de los pedidos efectuados por un determinado cliente.

**GET /orders**

Devuelve un listado de pedidos, limitado a 10 por página.



Parámetros:

| **Campo** | **Tipo** | **Descripción** |
| --- | --- | --- |
| page | Integer | La página actual. Por defecto es 1. |
| limit | Integer | El límite de pedidos por página. Por defecto es 10. |
| external\_id | String | El ID de usuario con el que originalmente se concretó la compra. El campo es **obligatorio**. |





Ejemplo:


```json


{
    "success": true,
    "data": [
        {
            "id": 1,
            "external_id": "123",
            "amount": 6560,
            "installments": 1,
            "payment_method": {
                "id": 4,
                "name": "Mercado Pago"
            },
            "order_status": {
                "id": 4,
                "name": "Finalizada"
            },
            "type": "pickup",
            "pickup_info": {
                "id": 174,
                "code": "",
                "name": "San Telmo - Buenos Aires",
                "address": "Carlos Calvo 600",
                "phone": "",
                "zip": 0,
                "province_id": 1,
                "is_own": false,
                "hours": "",
                "cost": 500,
                "enabled": true
            },
            "products": [
                {
                    "id": 196,
                    "amount": 1,
                    ...
                    // Los mismos valores que se mencionan en el endpoint de productos.
                }
            ],
            "created_at": "2018-06-18 19:21:11",
            "delivered_at": null
        }

   ]
}

```


** GET /orders/{id}**

Devuelve un pedido. También es obligatorio el uso del campo external\_id.





### Marcas ###

Este endpoint devuelve las marcas activas que hayan sido cargadas en la tienda.

**GET /utils/brands**

Devuelve un listado de marcas, limitado a 10 por página.



Parámetros:

| **Campo** | **Tipo** | **Descripción** |
| --- | --- | --- |
| page | Integer | La página actual. Por defecto es 1. |
| limit | Integer | El límite de marcas por página. Por defecto es 10. |



Ejemplo:


```json

{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "Gardenlife",
            "logo": "http://gardenlife.simbel.com.ar/public/images/marcas/marca_3.jpg",
            "active": true
        }
   ]
}

```




**GET /utils/brands/{id}**

Devuelve una marca.




### Tienda ###

Este endpoint devuelve información general de la tienda.

**GET /store/info**

Devuelve un objeto JSON conteniendo información de la tienda.







Ejemplo:


```json

{
    "success": true,
    "data": {
            "id": 123,
              "name": "GardenLife",
             "address": "Av. Brasil 1921, Parque Industrial Tortuguitas",
             "phone": "+542320331800",
             "blog": null,
             "email": "contacto@gardenlife.com.ar",
             "description": "Home & Garden design",
             "domain": "gardenlife.simbel.com.ar",
             "currency": "ARS",
             "logo": "http://gardenlife.simbel.com.ar/public/images/static/logo.svg",
             "social": {
                 "facebook": "gardenlifearg",
                 "gplus": null,
                 "instagram": "gardenlifearg",
                 "twitter": null,
                 "pinterest": null
    }

}

```
