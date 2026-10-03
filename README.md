# Restaurante App — Semana 15

## Propósito
Tercera iteración del proyecto **restaurante_app**, evolucionando la interfaz gráfica construida con **Tkinter**.
Esta semana se enfoca en los **conceptos fundamentales de manejo de eventos**, incorporando una nueva sección de **Ventas** que relaciona un usuario con un producto, utilizando el flujo: acción del usuario → `command=` → callback → servicio → persistencia → respuesta visual.

## Novedades — Semana 15

### Fundamentos de manejo de eventos
- Vinculación de botones con callbacks mediante `command=` (pasando la referencia de la función, sin paréntesis).
- El callback coordina la interacción: obtiene datos de la interfaz, delega la lógica al servicio, actualiza la vista y comunica el resultado al usuario.
- Separación estricta de responsabilidades: la interfaz no manipula directamente los archivos JSON ni contiene reglas de negocio.

### Nueva sección: Gestión de Ventas
- **Selección de usuario**: mediante `ttk.Combobox` (solo lectura) que muestra identificación y nombre.
- **Selección de producto**: mediante `ttk.Combobox` (solo lectura) que muestra código y nombre.
- **Registro de venta**: botón "Registrar venta" que ejecuta el callback, valida la existencia de los elementos y delega a `RestauranteServicio`.
- **Visualización**: tabla `ttk.Treeview` que muestra las ventas registradas (ID, Usuario, Producto, Fecha).
- **Persistencia**: almacenamiento automático en `datos/ventas.json`.

### Recursos visuales obligatorios
- Carpeta `assets/logo/` con el logotipo del sistema (`logo.png`), cargado como ícono de la ventana principal.
- Carpeta `assets/icons/` con 6 íconos (`icon_inicio`, `icon_usuarios`, `icon_productos`, `icon_ventas`, `icon_agregar`, `icon_salir`) integrados en los botones del menú lateral, en "Cerrar sesión" y en "Registrar venta".

## Estructura del proyecto

```text
restaurante_app_semana15/
├── restaurante_app/
│   ├── assets/
│   │   ├── logo/
│   │   │   └── logo.png
│   │   └── icons/
│   │       ├── icon_inicio.png
│   │       ├── icon_usuarios.png
│   │       ├── icon_productos.png
│   │       ├── icon_ventas.png
│   │       ├── icon_agregar.png
│   │       └── icon_salir.png
│   ├── datos/
│   │   ├── productos.json
│   │   ├── usuarios.json
│   │   └── ventas.json
│   ├── modelos/
│   │   ├── __init__.py
│   │   ├── producto.py
│   │   ├── usuario.py
│   │   └── venta.py
│   ├── servicios/
│   │   ├── __init__.py
│   │   ├── archivo_servicio.py
│   │   └── restaurante_servicio.py
│   ├── ui/
│   │   ├── __init__.py
│   │   ├── login_view.py
│   │   └── main_view.py
│   └── main.py
└── README.md
```

## Responsabilidades

- **`modelos/venta.py`**: representa la entidad `Venta`, relacionando un `usuario_id`, un `producto_codigo` y una `fecha`, con validación de campos obligatorios.
- **`servicios/archivo_servicio.py`**: lee y escribe los archivos JSON (productos, usuarios y ventas), sin lógica de negocio.
- **`servicios/restaurante_servicio.py`**: concentra las reglas de negocio, valida la existencia de usuarios y productos antes de una venta, genera el identificador de venta (`V001`, `V002`...) y gestiona la persistencia.
- **`ui/main_view.py`**: construye las vistas con Tkinter, utiliza `command=` para asociar botones a callbacks y actualiza la interfaz tras cada operación, sin tocar los archivos JSON directamente. Carga los íconos de `assets/` y los conserva como referencia para que Tkinter no los descarte.
- **`main.py`**: crea la ventana principal `Tk()`, configura el ícono de la aplicación desde `assets/logo/logo.png` y controla el cambio entre `LoginView` y `MainView`.

## Flujo de la aplicación y manejo de eventos

```text
Inicio de la aplicación
        |
main.py prepara Tkinter, carga el icono y los servicios
        |
   LoginView (validación de acceso)
        |
    MainView (menú lateral)
        |
Navegación: Inicio | Usuarios (consulta) | Productos (CRUD) | Ventas (NUEVO)
        |
--- FLUJO DE EVENTO EN VENTAS ---
1. Usuario selecciona usuario y producto en los Combobox
2. Usuario hace clic en el botón "Registrar venta"
3. Componente Button activa el callback vía command=self.registrar_venta
4. Callback obtiene selecciones y llama a RestauranteServicio.registrar_venta()
5. Servicio valida datos, crea objeto Venta y guarda en ventas.json
6. Callback refresca el Treeview y muestra messagebox de confirmación
---------------------------------
        |
   Cerrar sesión
        |
   LoginView
```

## Cómo ejecutar

Ubicarse dentro de la carpeta `restaurante_app`:

```bash
cd restaurante_app
```

Ejecutar el punto de entrada:

```bash
python main.py
```

Iniciar sesión con el usuario de prueba cargado en `datos/usuarios.json`:
- Usuario: `jperez`
- Contraseña: `1234`

Navegar al menú lateral "Ventas", seleccionar un usuario y un producto, y probar el registro de una nueva venta.
Cerrar y volver a abrir la aplicación para verificar que las ventas se recuperan correctamente desde `ventas.json`.

## Requisitos técnicos
- Python 3.8 o superior
- Tkinter (incluido con la instalación estándar de Python)
- Pillow, solo si se desea regenerar los íconos PNG (no es necesario para ejecutar la app)

## Referencias
Estructura y flujo adaptados y evolucionados de los proyectos docentes Biblioteca App:
- Semana 13: https://github.com/kevin10lascano-sketch/Clase-Semana-13-POO.git
- Semana 14: https://github.com/kevin10lascano-sketch/Clase-Semana-14-POO.git
- Semana 15: https://github.com/kevin10lascano-sketch/Clase-Semana-15-POO.git

## Autor
Dennis Leonardo Pacheco Álvarez — Proyecto académico de Programación Orientada a Objetos.
