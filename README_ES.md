### 💻 Actividades

- ¡Las acciones de GitHub son fáciles con GitHub Copilot!
- Despliega en GitHub Pages con GitHub Actions y Copilot.
- Crea un flujo de trabajo reutilizable.

![Preview de la APP](./image.png)

### 🎯Objetivo General:

- Familiarizarse con el uso de GitHub Copilot para crear flujos de trabajo automatizados en GitHub Actions:
- Automatizar el ciclo de desarrollo y despliegue de una aplicación Next.js usando GitHub Actions

### Objetivos Específicos:

- Configurar el entorno de Node.js usando GitHub Actions
- Ejecutar pruebas automatizadas y construir la aplicación
- Integrar acciones de seguridad en el flujo de trabajo
- Desplegar la aplicación Next.js en GitHub Pages
- Optimizar y refinar el flujo de trabajo con la asistencia de GitHub Copilot

### Resultados Esperados:

Al final de la sesión práctica, deberías tener una canalización CI/CD completamente funcional que maneje todo, desde la instalación de dependencias hasta el despliegue de la aplicación en GitHub Pages.
Además, habrás aprendido cómo aprovechar GitHub Copilot para acelerar la configuración de flujos de trabajo en GitHub Actions y cómo integrar herramientas de seguridad en el proceso.

### Paso 1: Clonar el Repositorio 🚀

### Paso 2: Crear un Nuevo Repositorio

Crea un nuevo repositorio con el nombre: gh-action-copilot-handson

Asegurarse de tener permisos de lectura y escritura en la configuración del flujo de trabajo

Configuración

Configuración -> Acciones -> General -> Permisos del Flujo de Trabajo -> Leer y Escribir

### Paso 3: Añadir un Nuevo Remoto al Repositorio Clonado

Usa la CLI de copilot para añadir un nuevo remoto

```
gh copilot suggest "cómo añadir un nuevo remoto al repositorio de GitHub"
```

### Paso 4: Crear una plantilla de flujo de trabajo básica usando GitHub Copilot

Pregunta a copilot Chat

👤 Prompt:
```
@workspace Quiero crear un flujo de trabajo básico de GitHub llamado handson que se active en cada push a la rama main. Solo quiero el paso de activación
```

🤖 Respuesta de Copilot:
```
name: handson

on:
  push:
    branches:
      - main
```

### Paso 5: Definir un trabajo en el flujo de trabajo

Pregunta a copilot para definir un trabajo básico

👤 Prompt:
```
#file:handson.yml define un job ci sin pasos, usa ubuntu-latest como runner
```

🤖 Respuesta de Copilot:
```
jobs:
  ci:
    runs-on: ubuntu-latest
    steps: []
```

### Paso 6: Checkout del Código y Configuración de Node

👤 Prompt:
```
Añadir pasos para hacer checkout del código y configurar node, usar la versión 20 de node
```

🤖 Respuesta de Copilot:
```
steps:
  - name: Checkout del código
    uses: actions/checkout@v3

  - name: Configurar Node.js
    uses: actions/setup-node@v3
    with:
      node-version: 20
```

### Paso 7: Añadir paso para instalar dependencias

**Desarrollo impulsado por comentarios**:

👤 Prompt:
```
Añadir paso para instalar dependencias
```

🤖 Respuesta de Copilot:
```
- name: Instalar dependencias
  run: npm install
```

### Paso 8: Implementar CodeQl para análisis de código y encontrar vulnerabilidades

👤 Prompt:
```
@workspace Quiero implementar un paso para análisis de código y encontrar vulnerabilidades, usar CodeQl y Javascript/TypeScript como lenguajes objetivo y explícame los nuevos pasos
```

🤖 Respuesta de Copilot:
```
- name: Inicializar CodeQL
  uses: github/codeql-action/init@v2
  with:
    languages: javascript,typescript

- name: Realizar Análisis de CodeQL
  uses: github/codeql-action/analyze@v2
  with:
    category: "/language:javascript"

```

Referencia:

- https://codeql.github.com/
- https://github.com/github/codeql-action/blob/v2/analyze/action.yml

### Paso 9: Implementar TruffleHog para Encontrar Credenciales Filtradas

👤 Prompt:
```
@workspace Ahora, quiero implementar un paso para encontrar posibles credenciales filtradas, usar trufflesecurity/trufflehog con la versión main, only-verified como extra_args, head con el nombre de la referencia actual
```

En caso de que tengas algunos errores con la versión usa:

```
name: Escanear en busca de credenciales filtradas
uses: trufflesecurity/trufflehog@main
with:
base: ""
head: ${{ github.ref_name }}
extra_args: --only-verified
```

Referencia:

- https://github.com/trufflesecurity/trufflehog

### Paso 10: Añadir paso para ejecutar pruebas

Pregunta a copilot chat o usa desarrollo impulsado por comentarios para añadir otro paso para ejecutar pruebas


**Copilot chat**:

👤 Prompt:
```
@workspace Añadir otro paso para ejecutar pruebas con npm run
```

**Desarrollo impulsado por comentarios**:

```
Añadir otro paso para ejecutar pruebas con npm run
```
🤖 Respuesta de Copilot:
```
- name: Ejecutar pruebas
  run: npm run test
```

### Paso 11: Añadir paso para construir

**Copilot chat**:

👤 Prompt:
```
Añadir otro paso para ejecutar la construcción con npm run
```

**Desarrollo impulsado por comentarios**:

```
Añadir otro paso para ejecutar la construcción con npm run
```

🤖 Respuesta de Copilot:
```
- name: Construir
  run: npm run build
```

### Paso 12: Probar el flujo de trabajo

Vamos a probar nuestro flujo de trabajo, por favor haz commit y push a la rama main. Luego:

- Ve a tu repositorio
- Navega a la pestaña "Actions"

Verás tu flujo de trabajo ejecutándose

## Desplegar en GitHub Pages con GitHub Actions y Copilot

Antes de continuar con los siguientes pasos, por favor abre el archivo 'next.config.mjs' y edita los campos assetPrefix y base path, cambia los valores de estos campos al nombre del repositorio (gh-action-copilot-handson)

### Paso 1: Añadir paso para subir el directorio out con la acción de subir artefactos

Pregunta a copilot chat para añadir un nuevo paso en el trabajo actual para subir el directorio out usando la acción de subir artefactos

👤 Prompt:
```
@workspace Ahora, quiero usar la acción de subir artefactos para subir el directorio out
```

🤖 Respuesta de Copilot:
```
- name: Subir artefacto
  uses: actions/upload-artifact@v3
  with:
    name: out-directory
    path: out
```

### Paso 2: Crear un nuevo trabajo para desplegar nuestra aplicación

Pregunta a copilot chat para crear otro trabajo llamado deploy y añadir un paso para descargar el artefacto subido previamente

👤 Prompt:
```
@workspace Cómo crear otro trabajo llamado deploy con solo un paso para descargar un artefacto y la salida debería ser 'out'
```

### Paso 3: Añadir paso para desplegar en GitHub Pages

Pregunta a copilot cómo desplegar en GitHub Pages

👤 Prompt:
```
@workspace Cómo desplegar el directorio out en GitHub Pages?
```

🤖 Respuesta de Copilot:
```
- name: Desplegar en GitHub Pages
  uses: peaceiris/actions-gh-pages@v3
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    publish_dir: ${{ inputs.publish_dir }}
```
Una vez hecho push a la rama main y se haya terminado de ejecutar el flujo de trabajo, debes ir a la pestaña de configuracion -> pages y seleccionar Deploy from a branch,seleccionar la rama `gh-pages`, la carpeta `/root` y dar click en save. Luego de unos minutos, tu aplicación debería estar disponible junto con la URL de la página.

![alt text](./assets/image-1.png)

## Crear un flujo de trabajo reutilizable.

### Paso 1: añadir un flujo de trabajo reutilizable

Pregunta a copilot para crear un flujo de trabajo reutilizable para el paso de desplegar en GitHub Pages

👤 Prompt:
```
@workspace cómo puedo crear un flujo de trabajo reutilizable para el paso de Desplegar en GitHub Pages, el flujo de trabajo reutilizable debe requerir el directorio de publicación como entrada. Solo dame el flujo de trabajo reutilizable
```

🤖 Respuesta de Copilot:
```
name: Desplegar en GitHub Pages

on:
  workflow_call:
    inputs:
      publish_dir:
        description: 'El directorio para publicar en GitHub Pages'
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout del código
        uses: actions/checkout@v3

      - name: Desplegar en GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ${{ inputs.publish_dir }}
```

### Paso 2: Usar el flujo de trabajo reutilizable

Pregunta a copilot para renombrar el trabajo de despliegue y crear otro trabajo para usar el flujo de trabajo reutilizable

👤 Prompt:
```
@workspace Renombrar el trabajo de despliegue a descargar artefacto. Luego crear otro trabajo después del trabajo de descargar artefacto y usar el flujo de trabajo reutilizable #file:deploy.yml
```

Debería darnos una respuesta como esta:

🤖 Respuesta de Copilot:
```
  deploy:
    needs: download-artifact
    uses: ./.github/workflows/deploy.yml
    with:
      publish_dir: ./out
```
