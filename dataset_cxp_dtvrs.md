# Dataset BD_cxp_dtvrs
- Referencias para construir el dataset:
- Existen 9 proveedores, que radican en 2 ciudades
- Cada proveedor puede emitir varias facturas de venta de productos en el mes,  con diferente fecha de emisión de la factura a 30, 60, 90, 120 o incluso más de 120 días, éstas últimas serán las mínimas en el dataset (5%)
- Los plazos de las facturas, se agrupan en las siguientes edades: 1. facturas de 0 a 30 días, el 36% de la cartera, 2. facturas de 31 a 60 días, el 24% de la cartera, 3. facturas de 61 a 90 días el 19% de la cartera, 4. facturas de 91 a 120 días, el 16% de la cartera 5. facturas mayores a 121 días el 5% de la cartera
- El dataset debe tener los saldos en moneda nacional (bs) y en dolares estadounidenses ($). Las facturas de los proveedores se emiten en bolivianos, por tanto en base a la fecha de la factura se calcula la reexpresion en dólares
- El dataset debe contener 500 facturas de proveedores para preparar el artefacto
- El dataset debe tener la información completa de todos los campos requeridos

> Nota de ubicación:
> Se cuenta con una base de datos con el registro del tipo de cambio histórico, para que tomes de ese archivo el tipo de cambio. La ubicación es: `\02_dtvrs\04_operaciones_projs_dtvrs\0406_webapp_cxp_dtvrs` El nombre del archivo es: `bd_tc`

**ciudad:**
10 SCZ
30 LPZ

**base de proveedores**
cod_proveedor
nombre_comercial
razon_social
nit
telefono
direccion
cod_ciudad
ciudad
web
mail
persona_contacto
posicion
fecha_creacion

**proveedor**
cod_proveedor
nombre_comercial
razon_social

**base_cuentas_por_pagar**
cod_proveedor
nombre_comercial
fecha_factura
no_factura
fecha_vencimiento_fact
importe_original_bs
pago_a_cuenta_bs
saldo_vencido_bs
0 - 30 [ Bs ]
31 - 60 [ Bs ]
61 - 90 [ Bs ]
91 - 120 [ Bs ]
'> 121 [ Bs ]
tco
importe_original_$
pago_a_cuenta_$
saldo_vencido_$
0 - 30 [ $ ]
31 - 60 [ $ ]
61 - 90 [ $ ]
91 - 120 [ $ ]
'> 121 [ $ ]

**edad_saldos_por_pagar_bs**
0 - 30 [ Bs ]
31 - 60 [ Bs ]
61 - 90 [ Bs ]
91 - 120 [ Bs ]
'> 121 [ Bs ]

**edad_saldos_por_pagar_$bs$**
0 - 30 [ $ ]
31 - 60 [ $ ]
61 - 90 [ $ ]
91 - 120 [ $ ]
'> 121 [ $ ]
