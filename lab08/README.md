# EventFlow

API REST de logística de eventos con Django REST Framework y cliente web
nativo. La app `series` preexistente se conserva; el dominio EventFlow vive en
la app `logistica`.

## Entorno local

Usa Python 3.13 y el entorno virtual `venv` del proyecto. Instala dependencias
con:

```powershell
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

La configuración real está en `.env` (no se sube al repositorio). Usa
`.env.example` como referencia. Ejecuta las migraciones y crea una cuenta para
acceder a las operaciones de escritura:

```powershell
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

El panel de administración está en `http://127.0.0.1:8000/admin/`. Desde ahí
se pueden cargar espacios y recursos técnicos.

## API

- `POST /api/login/`: recibe `username` y `password` como JSON y devuelve un
  token.
- `/api/espacios/`: listar, crear y filtrar por `estado` o `search`.
- `/api/espacios/{id}/`: consultar, actualizar o eliminar un espacio.
- `/api/recursos/`: listar, crear, buscar y filtrar por `espacio`, `costo_min`
  y `costo_max`.
- `/api/recursos/{id}/`: consultar, actualizar o eliminar un recurso.

Las lecturas son públicas. Para crear, actualizar o eliminar, envía
`Authorization: Token <token>`. Los espacios con recursos asignados no se
pueden eliminar. El servidor devuelve `400` si los filtros numéricos no son
válidos y `404` para identificadores inexistentes.

## Cliente web

Con el servidor Django activo, abre otra terminal:

```powershell
cd frontend
python -m http.server 5500
```

Abre `http://127.0.0.1:5500/`; no abras el HTML mediante `file://`. Inicia
sesión con la cuenta creada, registra espacios y recursos, y usa los filtros
del listado. CORS está habilitado para `localhost:5500` y `127.0.0.1:5500`.

## Verificación

```powershell
python manage.py check
python manage.py test
```
