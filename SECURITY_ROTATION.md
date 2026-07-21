# Rotación de credencial expuesta

## Incidente confirmado

El archivo `local.properties` fue publicado en el commit `db519a40732ab572e9c7f5841317a423f59ff9bd`. Ese archivo contenía una credencial de Gemini y una ruta local del Android SDK.

La credencial no se reproduce en este documento.

## Acciones inmediatas obligatorias

1. Revocar o eliminar la clave expuesta desde el proyecto de Google AI Studio o Google Cloud al que pertenece.
2. Crear una clave nueva con el alcance y las restricciones mínimas necesarias.
3. Revisar métricas, cuotas y registros por uso no reconocido desde la fecha del commit expuesto.
4. No reutilizar la clave anterior aunque este pull request sea fusionado.
5. Guardar la nueva clave únicamente en configuración local no versionada o en secretos de CI.

## Corrección aplicada en esta rama

- eliminación de `local.properties` del estado actual del repositorio;
- incorporación de `local.properties`, archivos de firma y archivos de entorno a `.gitignore`;
- documentación del commit de origen sin publicar el valor de la clave.

## Eliminación del historial

Fusionar esta rama evita nuevas publicaciones, pero la clave seguirá visible en el historial antiguo. Después de revocarla, un administrador puede reescribir el historial con `git filter-repo`:

```bash
git clone --mirror https://github.com/MedinaParra/FotoIA.git
cd FotoIA.git
git filter-repo --path local.properties --invert-paths --force
git push --force --mirror
```

Antes del push forzado se deben avisar los colaboradores, cerrar o actualizar ramas antiguas y conservar una copia privada solo cuando sea necesaria para auditoría. Todos los clones existentes deberán volver a clonarse o limpiar sus referencias para no reintroducir el archivo.

## Uso seguro local

El desarrollador puede recrear un `local.properties` exclusivamente en su equipo:

```properties
sdk.dir=/ruta/local/al/Android/Sdk
gemini.api.key=REEMPLAZAR_POR_CLAVE_NUEVA
```

Ese archivo no debe adjuntarse a issues, artefactos ni paquetes de diagnóstico.
