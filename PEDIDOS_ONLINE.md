# 📦 Sistema de Gestión de Pedidos Online

## Descripción General

El nuevo tab de **Pedidos Online** permite gestionar todos los pedidos que llegan desde la tienda virtual (tienda_virtual) en un único lugar centralizado. Este sistema facilita la confirmación de pagos y el despacho de productos de forma rápida y eficiente.

## Acceso al Sistema

1. Ir a **pos.html** (Punto de Ventas)
2. En la barra de navegación, hacer clic en el tab **"📦 Pedidos Online"**
3. Se abrirá un modal con la lista de todos los pedidos pendientes

## Estados de los Pedidos

### 1. ⏳ Pendiente de Pago
- **Descripción:** Pedido registrado pero sin confirmar el pago
- **Método de Pago:** Puede ser Sinpe, Transferencia u otro
- **Acción:** 
  - Para TARJETA: Se confirma automáticamente
  - Para SINPE/TRANSFERENCIA: Se debe marcar como "Marcar Pagado"
- **Botones disponibles:**
  - ✅ Confirmar Pago / Marcar Pagado
  - ❌ Cancelar

### 2. ✅ Confirmado
- **Descripción:** Pago confirmado, listo para despacho
- **Próximo paso:** Completar el despacho
- **Botones disponibles:**
  - 📦 Completar Despacho
  - ❌ Cancelar

### 3. 📦 Despachado
- **Descripción:** Pedido completamente procesado y enviado
- **Estado final:** Pedido completado
- **Sin acciones disponibles**

## Flujo de Trabajo

### Pedidos Pagados con Tarjeta (Automático)
```
1. Pedido llega en estado "Pendiente de Pago"
2. Sistema detecta método de pago = "tarjeta"
3. Se muestra botón "✅ Confirmar Pago" (para confirmación manual si es necesario)
4. Una vez confirmado → Estado "✅ Confirmado"
5. Hacer clic en "📦 Completar Despacho"
6. Estado final → "📦 Despachado"
```

### Pedidos Pagados con Sinpe/Transferencia (Manual)
```
1. Pedido llega en estado "Pendiente de Pago"
2. Esperar confirmación del banco/Sinpe
3. Hacer clic en "✅ Marcar Pagado"
4. Estado cambia a "✅ Confirmado"
5. Hacer clic en "📦 Completar Despacho"
6. Estado final → "📦 Despachado"
```

## Filtros Disponibles

### Estado
- Todos los estados
- ⏳ Pendiente de pago
- ✅ Confirmado
- 📦 Despachado

### Cliente
- Búsqueda por email del cliente
- Búsqueda parcial soportada

### Método de Pago
- Todos los métodos
- 💳 Tarjeta
- 📱 Sinpe
- 🏦 Transferencia

## Información Mostrada por Pedido

### Encabezado del Pedido
- **ID Pedido:** Identificador único (ej: ORD-12345)
- **Cliente:** Email del cliente
- **Estado:** Badge con color según estado actual
- **Método de Pago:** Método de pago seleccionado

### Resumen Financiero
- **Total:** Monto total del pedido
- **Cantidad Artículos:** Número de productos
- **Envío:** Tipo de envío (Retiro o Envío)
- **Fecha:** Fecha de creación del pedido

### Detalle de Artículos
- Expandible para ver lista completa de productos
- Muestra nombre, cantidad y precio de cada artículo

## Limpieza Automática

El sistema automáticamente:
- **Elimina pedidos pendientes** que hayan estado sin confirmar por más de **7 días**
- Se ejecuta al abrir el tab de pedidos online
- Notifica en la consola cuándo se elimina un pedido

## Almacenamiento de Datos

Los pedidos se almacenan en `localStorage` bajo la clave `onlineOrders`:
- Formato JSON
- Estructura: Array de objetos pedido
- Persiste mientras no se limpie el navegador

## Campos de Datos de un Pedido

```javascript
{
  id: "ORD-1234567890",                    // ID único del pedido
  date: "2026-01-18T14:30:45",             // Fecha de creación
  customerEmail: "cliente@email.com",      // Email del cliente
  paymentMethod: "tarjeta",                // Método de pago
  total: 45000,                             // Monto total
  status: "pendiente",                      // Estado actual
  items: [                                  // Productos
    {
      id: 1,
      title: "Producto",
      price: 10000,
      qty: 2
    }
  ],
  shippingMethod: "envio",                 // "envio" o "pickup"
  confirmedDate: "2026-01-18T15:00:00",   // Cuando se confirmó (si aplica)
  dispatchedDate: "2026-01-18T16:00:00"   // Cuando se despachó (si aplica)
}
```

## Confirmación Automática para Tarjeta

Cuando un pedido se paga con tarjeta:
1. El cliente realiza el pago
2. El pedido se registra en estado "Pendiente de Pago"
3. Al abrir Pedidos Online, el botón muestra "✅ Confirmar Pago"
4. **Se debe confirmar manualmente para registrar que se recibió**

## Notas Importantes

- ✅ Todos los usuarios tienen acceso (sin validación de admin)
- 📝 Los datos se guardan localmente en el navegador
- 🔄 No hay sincronización con base de datos externa
- ⚠️ Si se limpian los datos del navegador, se pierden los pedidos
- 📱 Completamente responsivo para mobile y desktop

## Próximas Mejoras Sugeridas

1. Integración con email para notificar al cliente
2. Cargar automáticamente pedidos desde tienda virtual
3. Integración con pasarela de pagos real
4. Base de datos para persistencia a largo plazo
5. Reporte de pedidos despachados
6. Tracking de envíos
7. Comprobantes de despacho PDF
8. Notificaciones push

## Troubleshooting

### "No hay pedidos online registrados"
- Es normal si no hay datos en `localStorage`
- Los pedidos se crean desde la tienda virtual (checkout.html)

### Pedidos desaparecidos
- Pueden haber sido eliminados por límite de 7 días
- Se pierden al limpiar caché del navegador

### Cambios no se guardan
- Verificar que localStorage esté habilitado
- Revisar que no esté en modo privado/incógnito
