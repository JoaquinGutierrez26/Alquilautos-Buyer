# AlquilAutos — Buyer App

Aplicación web para descubrir y alquilar vehículos publicados por particulares. Esta es la aplicación orientada al **alquilador** dentro del ecosistema AlquilAutos: permite explorar vehículos, gestionar reservas, coordinar entregas, consultar pagos y dejar reseñas.

> Proyecto desarrollado como una microapp independiente, preparada para integrarse con los servicios de vehículos, entregas, pagos y feedback mediante APIs REST.

## Demo

**[Abrir la aplicación en producción](https://proyecto-c-buyer-alquilautos.vercel.app)**

### Usuarios de demostración

Las siguientes cuentas están disponibles únicamente para probar la demo desplegada:

| Rol | Email | Contraseña |
| --- | --- | --- |
| Alquilador | `buyer+clerk_test@iaw.com` | `iawuser#` |
| Administrador | `adminbuyer+clerk_test@iaw.com` | `iawuser#` |

## Funcionalidades

### Para alquiladores

- Explorar el catálogo público de vehículos.
- Filtrar vehículos por modelo y rango de precio.
- Consultar el detalle de cada vehículo, su propietario y las reseñas recibidas.
- Solicitar reservas y consultar su estado durante todo el flujo.
- Cancelar reservas cuando el estado lo permite.
- Coordinar la entrega del vehículo.
- Acceder al enlace de pago de una reserva.
- Guardar y quitar vehículos de favoritos.
- Editar los datos personales del perfil.
- Consultar las reseñas y el resumen generado sobre el perfil del alquilador.
- Dejar reseñas relacionadas con una reserva finalizada.

### Para administradores

- Acceder a un panel protegido por roles.
- Listar usuarios registrados.
- Consultar y editar la información de un usuario.
- Eliminar usuarios desde el panel de administración.

## Flujo de una reserva

La aplicación contempla el ciclo completo de una reserva:

```text
Pendiente → Aceptada → Coordinada → Pagada → Entregada → Finalizada
```

También contempla los estados alternativos `Cancelada` y `Rechazada`. La demo incluye datos de ejemplo en cada estado para poder recorrer y revisar las distintas acciones disponibles.

## Arquitectura

AlquilAutos está organizado como un ecosistema de microapps:

- **Buyer App:** experiencia del usuario que alquila el vehículo.
- **Seller App:** publicación y gestión de vehículos.
- **Shipping App:** coordinación y seguimiento de entregas.
- **Payments App:** gestión de pagos, actualmente pensada para integrarse con Mercado Pago.
- **Feedback App:** reseñas, calificaciones y resúmenes de usuarios, vehículos y propietarios.

La Buyer App expone y consume endpoints REST para comunicarse con estas aplicaciones. En el estado actual del proyecto, las integraciones externas todavía están pendientes, por lo que los vehículos, reservas, propietarios, pagos y reseñas de la demo utilizan datos mockeados.

## Stack tecnológico

- [Next.js 16](https://nextjs.org/) con App Router.
- [React 19](https://react.dev/).
- [TypeScript](https://www.typescriptlang.org/).
- [Tailwind CSS](https://tailwindcss.com/).
- [Clerk](https://clerk.com/) para autenticación y gestión de usuarios.
- [Prisma](https://www.prisma.io/) como ORM.
- [PostgreSQL en Neon](https://neon.tech/) como base de datos.
- [Vercel](https://vercel.com/) para el despliegue de producción.

## Ejecución local

### Requisitos

- Node.js 20 o superior.
- pnpm 11.
- Una instancia de PostgreSQL.
- Una aplicación configurada en Clerk.

### Instalación

La aplicación se encuentra dentro de la carpeta `alquilautos-buyer`:

```bash
cd alquilautos-buyer
pnpm install
```

1. Crea un archivo `.env.local` a partir de `.env.example`.
2. Completa las credenciales de Clerk y la variable `DATABASE_URL`.
3. Genera el cliente de Prisma y aplica las migraciones:

```bash
pnpm prisma generate
pnpm prisma migrate deploy
```

4. Inicia el servidor de desarrollo:

```bash
pnpm dev
```

La aplicación estará disponible en [http://localhost:3000](http://localhost:3000).

### Scripts disponibles

| Comando | Descripción |
| --- | --- |
| `pnpm dev` | Inicia el servidor de desarrollo. |
| `pnpm build` | Genera la build de producción. |
| `pnpm start` | Inicia la aplicación en modo producción. |
| `pnpm lint` | Ejecuta ESLint. |

## Decisiones y reglas principales

- El onboarding es obligatorio después del registro. Hasta completarlo, el middleware redirige al usuario para solicitar nombre, apellido, fecha de nacimiento, documento, licencia de conducir y dirección.
- El documento y la licencia de conducir son únicos por usuario y se validan tanto durante el onboarding como al actualizar el perfil.
- Un vehículo marcado como indisponible no puede reservarse desde la interfaz.
- El acceso al panel de administración requiere el rol `adminBuyer`, configurado mediante `publicMetadata` en Clerk.
- Las cuentas y los datos incluidos en la demo son de prueba y no representan información real.

## Estado del proyecto

La experiencia principal de la Buyer App está implementada y desplegada. El siguiente paso de integración es reemplazar los datos mockeados por las respuestas de las Seller, Shipping, Payments y Feedback Apps a través de sus APIs REST.
