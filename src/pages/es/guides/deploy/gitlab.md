---
title: Despliega tu proyecto de Astro en GitLab Pages
description: Cómo desplegar tu proyecto de Astro usando GitLab Pages.
layout: ~/layouts/DeployGuideLayout.astro
i18nReady: true
---

Puedes usar [GitLab Pages](https://pages.gitlab.io/) para alojar un proyecto de Astro para tu proyecto, grupo, o cuenta de usuario en [GitLab](https://about.gitlab.com/).

:::tip[¿Buscas un ejemplo?]
¡Revisa [el proyecto de ejemplo de Astro en GitLab Pages](https://gitlab.com/pages/astro)!
:::

## Cómo desplegar

1. Establece el `site` correcto en `astro.config.mjs`.
2. Establece `outDir:public` en `astro.config.mjs`. Este ajuste le indica a Astro que coloque la salida de archivos estáticos al compilar en una carpeta llamada `public`, la cual es requerida por GitLab Pages para los archivos expuestos.

Si estás usando el [directorio `public/`](/es/core-concepts/project-structure/#public) como fuente de archivos estáticos en tu proyecto de Astro, renombralo y usa ese nuevo nombre del directorio en `astro.config.mjs` para el valor de `publicDir`.

Por ejemplo, estos son los ajustes correctos de `astro.config.mjs` cuando el directorio `public/` es renombrado a `static/`:

   ```js
   import { defineConfig } from 'astro/config';

   export default defineConfig({
     sitemap: true,
     site: 'https://astro.build/',
     outDir: 'public',
     publicDir: 'static',
   });
   ```

3. Crea un archivo llamado `.gitlab-ci.yml` en la raíz de tu proyecto con el siguiente contenido. Esto compilará y desplegará tu proyecto cada vez que realices cambios en el contenido:

   ```yaml
   # La imagén de Docker que será usada para compilar tu app.
   image: node:14

   pages:
     cache:
       paths:
         - node_modules/
     script:
       # Especifica los pasos involucrados para compilar tu app.
       - npm install
       - npm run build

     artifacts:
       paths:
         # La carpeta que contiene los archivos compilados para publicarse.
         # Éste debe llamarse "public".
         - public

     only:
       # Acciona una nueva compilación y despliegala solo cuando hay cambios en
       # las rama(s) siguientes
       - main
   ```
