# Proyecto Next.js 15+ (App Router) - Arquitectura Hexagonal

## Descripción del Proyecto
Este proyecto es una plantilla para aplicaciones modernas basadas en **Next.js 15+** (App Router), **TypeScript** y **Tailwind CSS**. La estructura interna del proyecto está diseñada en base a los principios de **Arquitectura Hexagonal (Ports & Adapters)**, promoviendo una clara separación de responsabilidades y facilitando el mantenimiento y la escalabilidad del sistema. 

Además, se incluye una configuración completa para un entorno de desarrollo reproducible y aislado utilizando **Dev Containers**.

---

## Guía de Inicio Rápido (DevContainer)
Para aprovechar al máximo este entorno, asegúrate de tener instalado [Docker](https://www.docker.com/) y [Visual Studio Code](https://code.visualstudio.com/).

1. Clona el repositorio y abre la carpeta del proyecto en **Visual Studio Code**.
2. Instala la extensión **[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)**.
3. Al abrir el proyecto, VS Code te sugerirá reabrirlo en un contenedor. Haz clic en **"Reopen in Container"** (o presiona `F1` y busca este comando).
4. El contenedor se construirá automáticamente utilizando la imagen oficial de Node.js LTS, se instalarán las dependencias esenciales de VS Code (ESLint, Prettier, Tailwind CSS, Prisma) y se ejecutará `npm install`.
5. Una vez finalizado, puedes iniciar el servidor de desarrollo ejecutando el siguiente comando en la terminal integrada:
   ```bash
   npm run dev
   ```
6. La aplicación estará disponible localmente a través del puerto configurado, por ejemplo: `http://localhost:3000`.

---

## Arquitectura y Estructura de Carpetas

La aplicación sigue el modelo de **Arquitectura Hexagonal**, organizando el código dentro de la carpeta `src` de la siguiente manera:

```text
src/
├── app/                  # (Capa de Presentación Externa) Next.js App Router, pages, layouts y endpoints.
└── modules/              # (Capa de Dominio y Lógica Interna)
    ├── shared/           # Código común, utilidades, componentes genéricos e interfaces base.
    └── [domain_name]/    # Módulo de negocio específico (ej. "users", "products").
        ├── domain/       # Entidades, Value Objects e Interfaces de Repositorio (Puertos). NO depende de nada externo.
        ├── application/  # Casos de uso (Interactors). Implementa la lógica de negocio y consume las interfaces del dominio.
        └── infrastructure/ # Adaptadores técnicos (Repositorios concretos, Prisma/Drizzle, APIs externas). Implementa los puertos del dominio.
```

### Lógica Hexagonal en Next.js
* **El Núcleo (Domain & Application)**: Define *qué* hace el sistema y las reglas de negocio, totalmente agnóstico al framework.
* **Los Puertos (Interfaces)**: Son contratos dentro de `domain` que establecen cómo el núcleo espera comunicarse con el mundo exterior.
* **Los Adaptadores (Infrastructure)**: Implementaciones concretas de los puertos, como un repositorio que se conecta a la base de datos o a servicios externos.
* **El Framework (App Router)**: Actúa como el canal de entrada principal (Controladores) invocando los casos de uso (Application).

---

## Scripts Disponibles

El proyecto incluye los siguientes scripts básicos definidos en el `package.json` de Next.js:

* `npm run dev`: Inicia el servidor de desarrollo con Hot Module Replacement en el puerto 3000.
* `npm run build`: Compila la aplicación para producción generando la carpeta `.next`.
* `npm run start`: Inicia la aplicación en modo producción tras realizar el build.
* `npm run lint`: Ejecuta ESLint en el código del proyecto para asegurar la calidad y consistencia del código.
