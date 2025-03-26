#Tarea 1
**Descripción del sistema**
El sistema ficticio a analizar corresponde al aplicativo de un sistema de dispensado de combustible en una gasolinera. El usuario, de ahora en adelante llamado como pistero cuenta con un dispositivo (datáfono) que utiliza para manejar todas las acciones necesarias para la venta de combustible.
El flujo de una venta de combustible corresponde a:

1.El pistero debe registrarse con su numero de usuario en la aplicació	
2.Selecciona la opción de venta
3.Selecciona la máquina donde va a dispensar combustible, toma en cuenta si la máquina está siendo utilizada por otro pistero
4.Selecciona el producto a dispensar, ya sea super, regular o diesel
5.Programa la máquina, ya sea por litros o por colones, el sistema conoce cuánto es la conversion de colones por litro en ese momento.
6.Confirma la selección con el botón de dispensar, en este momento la máquina se habilita para dispensar combustible con la bomba correspondiente.


Una vez termina de dispensar el combustible, el pisterio tiene la opción de generar la factura electrónica o sólo emitir el tiquete, para la facturación electrónica necesita solicitar los datos del cliente, ya sea cedula física, jurídica y esto busca en la base de datos previamente cargada esta información, el aplicativo regresa la búsqueda con la información del cliente, nombre, correo electrónico, número de teléfono, y tiene la opción de agregar información como kilometraje, etc, una vez se ingresa toda la información requería se procede a generar la factura electrónica y a emitir el tiquete impreso.

Finalmente, existen otras opciones como generar reportes de ventas, donde el sistema puede buscar información de la última venta realizada o las ventas realizadas en el turno y mostrar la información al usuario en forma de pdf. Información como el producto, cantidad de litros, colones y la máquina utilizada.

##Descripción de los contextos delimitados
###Facturación electrónica
**Responsabilidad:** Envío de información de compra al cliente 
**Entidades:** Código, Tipo de documento, Número de documento, kilometraje, combustible, litros, total de compra, Usuario
**Reglas del negocio:**
- El flujo del estado de facturación es: Solicitud de factura electrónica→ Pendiente dispensar → Vehículo dispensado→Solicitud de información→Pago Recibido→ Envío de información 
- No se factura hasta terminar de dispensar el combustible

**Interacciones con otros contextos:**
- Se comunica con autenticación para conocer la identidad del pistero
- Se comunica con Pagos para confirmar venta.

###Solicitud de información de ventas por usuario
**Responsabilidad:** Consultas de información de ventas de un usuario en especifico a bases de datos
**Entidades:** Numero de máquina, Usuario,Producto,Litros,Colones,Precios combustible
**Reglas del negocio:**
- El flujo de solicitud de información es: login del usuario→ Solicitud de información de ventas → Generación de pdf→Despliegue de información 
- Se puede generar la información de la ultima venta
-Se puede generar la información de lo que lleva del turno

**Interacciones con otros contextos:**
- Se comunica con Autenticación para conocer la identidad del pistero

###Autenticación
**Responsabilidad:** Consulta de usuario válido para dispensar
**Entidades:** Número de usuario, Usuario, Validez
**Reglas del negocio:**
- El flujo de autenticación es: Ingreso al aplicativo→ Solicitud de código de usuario → Consulta de validez→Acepta/Rechaza usuario 
- No puede ingresar al sistema sin autenticarse primero

**Interacciones con otros contextos:**
- Se comunica con fracturación para identificar al pistero que dispensa el combustible
- Se comunica con Información de ventas por usuario para filtrar y extraer información del pistero identificado

##Justificación

El modulo de facturación electrónica es un componente opcional que el usuario tiene la opción o no de elegirlo, se utiliza en varios módulos de venta como el de pago a contado, a crédito, etc, por lo que es un buen candidato a extraer del monolito como un microservicio.

El módulo de solicitud de información de ventas del usuario le resta complejidad al aplicativo móvil, puede que por recursos del datáfono convenga tener esta capacidad técnica en un servidor externo que se conecte a la base de datos, filtre la infomación, procese la información y retorne un pdf con la información requerida por el usuario.

La autenticación para este proyecto debe funcionar por separado, ya que no tiene sentido ejecutar otras acciones del aplicativo si la persona no está previamente autorizada para hacerlo. 