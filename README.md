# Manejo de eventos con Tkinter

## Tema

Semana 16: Manejo de eventos aplicado a la gestion de usuarios en una aplicacion de restaurante con Tkinter.

## Objetivo

Evolucionar el proyecto en referencia a la Semana 15 sin reconstruirlo desde cero. La aplicacion conserva el login, la arquitectura por capas, la persistencia JSON, el CRUD de productos, el registro de ventas, el menu lateral y la identidad visual, se genero la funcionalidad de manejo por roles es decir que de acuerdo al rol cumplido tiene acceso a cierta información relevante, ejemplo el Administrador tiene más atributos que el empleado y cliente.

En esta semana se transforma la seccion `Usuarios` en una gestion sencilla e interactiva para observar el flujo entre una accion del usuario, un evento de Tkinter, `bind()`, un callback con `event`, el servicio, la persistencia y la respuesta visual.

## Continuidad desde Semana 15

La interfaz mantiene los mismos colores, estilos, iconos, menu lateral, barra de estado y organizacion general. La seccion `Ventas` permanece como ejemplo de `command=` y callbacks. La seccion `Usuarios` se convierte en el ejemplo principal de manejo de eventos.

## Nueva funcionalidad de Semana 16

La gestion de usuarios permite:

- registrar usuarios;
- consultar usuarios;
- seleccionar un usuario desde un `ttk.Treeview`;
- cargar automaticamente sus datos en el formulario;
- actualizar usuarios;
- eliminar usuarios con confirmacion;
- limpiar o cancelar la seleccion;
- trabajar con roles simples.

Roles disponibles en el modelo:

- `Administrador`
- `Empleado`
- `Cliente`

Desde la pantalla de gestion solo se pueden registrar usuarios con rol `Empleado` o `Cliente`. La cuenta administradora existente se conserva como cuenta administrativa del sistema.

Flujo educativo:

```text
Interaccion del usuario -> evento -> bind() -> callback(event) -> servicio -> JSON -> interfaz actualizada
```

## Eventos implementados

La seccion `Usuarios` muestra la diferencia entre botones con `command=` y eventos con `bind()`.

Botones con `command=`:

- `Registrar`
- `Actualizar`
- `Eliminar`
- `Limpiar`

Eventos con `bind()`:

- `<<TreeviewSelect>>`: al seleccionar una fila, carga el usuario en el formulario.
- `<Return>`: registra un usuario desde el formulario.
- `<Escape>`: limpia el formulario y cancela la seleccion actual.
- `<<ComboboxSelected>>`: detecta el cambio de rol en el `ttk.Combobox`.

Flujo principal del `Treeview`:

```text
Treeview -> identificador -> buscar_usuario_por_identificador() -> objeto Usuario -> formulario
```

## Estructura del proyecto

```text
restaurante_app/
├── assets/
│   ├── icons/
│   │   ├── home.png
│   │   ├── users.png
│   │   ├── books.png
│   │   ├── sales.png        # opcional para la seccion Ventas
│   │   ├── logout.png
│   │   ├── add.png
│   │   ├── edit.png
│   │   ├── delete.png
│   │   ├── search.png
│   │   └── clean.png
│   └── logo/
│       ├── logo.png
│       └── icono.png
├── datos/
│   ├── productos.json
│   ├── usuarios.json
│   └── ventas.json
├── modelos/
│   ├── usuario.py
│   ├── producto.py
│   └── venta.py
├── servicios/
│   ├── archivo_servicio.py
│   └── restaurante_servicio.py
├── ui/
│   ├── login_view.py
│   └── main_view.py
└── main.py
```

## Capas

`modelos/`: define las clases `Usuario`, `Producto` y `Venta`, con validaciones basicas para evitar campos vacios. `Usuario` incluye el atributo `rol`.

`servicios/`: contiene la logica de consulta, registro, actualizacion, eliminacion y persistencia. `RestauranteServicio` registra ventas y administra el CRUD de usuarios.

`datos/`: guarda la informacion persistente en archivos JSON.

`ui/`: contiene las vistas creadas con Tkinter.

`assets/icons/`: contiene iconos PNG usados por los botones. Si falta un icono, la aplicacion sigue funcionando con texto.

## Pantallas principales

`LoginView`: pantalla de inicio de sesion.

`Inicio`: panel de resumen con usuarios, productos y ventas.

`Usuarios`: pantalla de gestion de usuarios, disponible para el rol `Administrador`.

`Productos`: pantalla de gestion con formulario, botones y tabla para el CRUD basico.

`Ventas`: pantalla para seleccionar un usuario, seleccionar un producto y registrar una venta simple.

## Componentes Tkinter utilizados

`Label`: textos y titulos.

`Entry`: campos de entrada para login, formulario de productos y formulario de usuarios.

`ttk.Combobox`: selectores de usuario y producto en ventas; selector de rol en usuarios.

`ttk.Button`: botones de navegacion y acciones con `command=`.

`ttk.Treeview`: tablas de usuarios, productos y ventas.

`ttk.Scrollbar`: barra de desplazamiento para las tablas.

`messagebox`: mensajes simples de confirmacion o error.

## Persistencia

Los usuarios, productos y ventas se cargan desde JSON al iniciar la aplicacion.

```text
restaurante_app/datos/usuarios.json
restaurante_app/datos/productos.json
restaurante_app/datos/ventas.json
```

Al registrar, actualizar o eliminar usuarios, el servicio actualiza la coleccion en memoria y escribe `usuarios.json`.

Ejemplo de usuario:

```json
{
    "identificador": "U001",
    "nombre": "Administrador",
    "usuario": "admin",
    "contraseña": "1234",
    "rol": "Administrador"
}
```

## Control de acceso

La aplicacion usa el objeto `usuario_actual` recibido por `MainView`.

Regla sencilla:

- `Administrador`: puede ver y usar la seccion `Usuarios`.
- `Empleado` y `Cliente`: no ven la opcion `Usuarios` en el menu lateral.

No se implementa un sistema avanzado de permisos. El objetivo es mantener el ejemplo claro para clase.

## Validaciones principales

La gestion de usuarios valida:

- campos obligatorios;
- rol valido;
- identificador duplicado;
- nombre de usuario duplicado;
- usuario inexistente al actualizar o eliminar;
- ausencia de seleccion;
- intento de eliminar el usuario actualmente autenticado;
- intento de registrar nuevos administradores desde el formulario;
- intento de cambiar el rol del administrador actual.

## Como ejecutar

Desde la carpeta del proyecto:

```powershell
cd "C:\Users\Usuario\OneDrive\Documentos\Restaurante_Semana_16\restaurante_app"
py main.py
```

Si `python` esta disponible:

```powershell
python main.py
```

## Credenciales de demostracion

Usuario: `admin`

Contrasena: `1234`

Tambien puede usarse:

Usuario: `docente`

Contrasena: `abcd`

Usuario cliente de prueba:

Usuario: `ana`

Contrasena: `cliente123`

## Como probar la gestion de usuarios

1. Iniciar la aplicacion.
2. Acceder con `admin / 1234`.
3. Verificar que aparece la opcion `Usuarios` en el menu lateral.
4. Entrar a `Usuarios`.
5. Registrar un usuario con rol `Cliente`.
6. Registrar un usuario con rol `Empleado`.
7. Seleccionar una fila del `Treeview` y verificar que los datos se cargan automaticamente en el formulario.
8. Modificar nombre, usuario, contraseña o rol y pulsar `Actualizar`.
9. Seleccionar un usuario y pulsar `Eliminar`; confirmar la eliminacion.
10. Usar `Escape` para limpiar el formulario y quitar la seleccion.
11. Usar `Enter` desde el selector de rol para registrar.
12. Cerrar sesion e iniciar con `docente / abcd`.
13. Verificar que el usuario empleado no dispone de la opcion `Usuarios`.
14. Revisar que `Productos` y `Ventas` continuan funcionando.

## Nota educativa

El proyecto mantiene una implementacion sencilla para que el estudiante pueda seguir el crecimiento progresivo de la aplicacion:

```text
Semana 14: componentes y contenedores
Semana 15: accion -> command= -> callback -> servicio -> persistencia -> respuesta visual
Semana 16: interaccion -> evento -> bind() -> callback(event) -> servicio -> respuesta visual
```

La gestion de usuarios no representa un sistema de seguridad avanzado. Solo muestra una aplicacion clara y controlada de eventos reales en Tkinter dentro del proyecto de restaurante.
