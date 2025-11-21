POS VERDULERÍA – README COMPLETO PARA INSTALACIÓN Y CONFIGURACIÓN
=================================================================

Este documento permite que cualquier integrante del equipo pueda instalar, configurar y ejecutar el proyecto POS Verdulería desde cero usando Supabase local con Supabase CLI.

-------------------------------------------------------------------------------
1. REQUISITOS PREVIOS
-------------------------------------------------------------------------------

Instalar lo siguiente:

1. Node.js LTS
   https://nodejs.org

2. Git
   https://git-scm.com/

3. Docker Desktop  
   Supabase CLI usa Docker para correr la base de datos local.  
   Descarga: https://www.docker.com/products/docker-desktop  
   Debe estar ENCENDIDO siempre.

4. Supabase CLI  
   Instalar con:
      npm install -g supabase

   Verificar:
      supabase --version

-------------------------------------------------------------------------------
2. CÓMO CLONAR EL PROYECTO
-------------------------------------------------------------------------------

Desde la terminal:

   git clone <URL_DEL_REPO>
   cd pos-verduleria

-------------------------------------------------------------------------------
3. INSTALAR DEPENDENCIAS DEL PROYECTO
-------------------------------------------------------------------------------

   npm install

-------------------------------------------------------------------------------
4. CONFIGURAR SUPABASE LOCAL
-------------------------------------------------------------------------------

Si el proyecto NO tiene carpeta “/supabase”, ejecutar:

   supabase init

Levantar Supabase local:

   supabase start

Esto levanta:
- API: http://127.0.0.1:54321
- Studio: http://127.0.0.1:54323
- Base de datos: postgresql://postgres:postgres@127.0.0.1:54322/postgres

-------------------------------------------------------------------------------
5. CREAR VARIABLES DE ENTORNO
-------------------------------------------------------------------------------

Crear archivo:

   .env
   o
   .env.local
   o
   .vite-env.local

Dependiendo del proyecto.

Contenido:

   VITE_SUPABASE_URL=http://127.0.0.1:54321
   VITE_SUPABASE_ANON_KEY=sb_publishable_xxxxxx

**Para obtener tu ANON KEY:**

   supabase status

Copiar el valor de “Publishable key”.

-------------------------------------------------------------------------------
6. RESTAURAR LA BASE DE DATOS EXACTA DEL PROYECTO
-------------------------------------------------------------------------------

Abrir Supabase Studio:  
   http://127.0.0.1:54323  
Entrar a SQL Editor.

-----------------------------------
PASO 1 – Limpiar Base
-----------------------------------

   TRUNCATE TABLE public.products RESTART IDENTITY CASCADE;
   TRUNCATE TABLE public.categories RESTART IDENTITY CASCADE;

-----------------------------------
PASO 2 – Crear Categorías
-----------------------------------

   INSERT INTO public.categories (name) VALUES
     ('Frutas'),
     ('Verduras'),
     ('Tubérculos'),
     ('Hierbas'),
     ('Otros');

-----------------------------------
PASO 3 – Insertar Productos Finales Correctos
-----------------------------------

INSERT INTO public.products (name, price, unit, category_id, image_url, is_active)
VALUES

-- FRUTAS
('Manzana Roja', 4.50, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Frutas' LIMIT 1),
 'https://images.pexels.com/photos/39803/pexels-photo-39803.jpeg',
 true),

('Naranja', 2.80, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Frutas' LIMIT 1),
 'https://images.pexels.com/photos/42059/background-beverage-citrus-juice-42059.jpeg',
 true),

('Fresa', 8.00, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Frutas' LIMIT 1),
 'https://images.pexels.com/photos/968275/pexels-photo-968275.jpeg',
 true),

-- VERDURAS
('Brócoli', 4.20, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Verduras' LIMIT 1),
 'https://images.pexels.com/photos/8390/food-wood-tomatoes.jpg',
 true),

('Cebolla Roja', 1.50, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Verduras' LIMIT 1),
 'https://images.pexels.com/photos/143133/pexels-photo-143133.jpeg',
 true),

('Lechuga', 2.00, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Verduras' LIMIT 1),
 'https://images.pexels.com/photos/1435907/pexels-photo-1435907.jpeg',
 true),

('Zanahoria', 1.00, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Verduras' LIMIT 1),
 'https://images.pexels.com/photos/65174/pexels-photo-65174.jpeg',
 true),

('Culantro', 1.50, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Hierbas' LIMIT 1),
 'https://images.pexels.com/photos/1437267/pexels-photo-1437267.jpeg',
 true),

-- TUBÉRCULOS
('Papa Canchan', 1.80, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Tubérculos' LIMIT 1),
 'https://images.pexels.com/photos/4109940/pexels-photo-4109940.jpeg',
 true),

('Camote', 2.00, 'kg',
 (SELECT id FROM public.categories WHERE name = 'Tubérculos' LIMIT 1),
 'https://images.pexels.com/photos/5946086/pexels-photo-5946086.jpeg',
 true),

-- OTROS
('Ensalada mixta', 12.00, 'plato',
 (SELECT id FROM public.categories WHERE name = 'Otros' LIMIT 1),
 'https://images.pexels.com/photos/1640777/pexels-photo-1640777.jpeg',
 true),

('Pasta al pesto', 15.00, 'plato',
 (SELECT id FROM public.categories WHERE name = 'Otros' LIMIT 1),
 'https://images.pexels.com/photos/1279330/pexels-photo-1279330.jpeg',
 true);

-------------------------------------------------------------------------------
7. EJECUTAR EL FRONTEND
-------------------------------------------------------------------------------

Con Supabase local encendido:

   supabase start

Luego:

   npm run dev

Abrir:

   http://localhost:3000

-------------------------------------------------------------------------------
8. ARQUITECTURA DEL PROYECTO
-------------------------------------------------------------------------------

src/
├── components/   → Componentes UI
├── pages/        → Pantallas
├── store/        → Manejo de carrito y venta
├── context/      → Estado global
├── hooks/        → Lógica reutilizable
├── lib/supabase.ts → Cliente Supabase

supabase/
├── migrations/
├── config
└── policies/

-------------------------------------------------------------------------------
9. ERRORES COMUNES Y SOLUCIONES
-------------------------------------------------------------------------------

ERROR: Docker is not running  
→ Abrir Docker Desktop y repetir: supabase start

ERROR: column does not exist  
→ Ejecutar PASOS 1, 2 y 3 del SQL de este README

ERROR: foreign key constraint  
→ Insertar categorías ANTES de los productos

ERROR: Publishable key incorrecta  
→ Revisar supabase status y copiar la correcta

-------------------------------------------------------------------------------
10. CRÉDITOS
-------------------------------------------------------------------------------

Proyecto POS Verdulería - Equipo 2025  
Configuración técnica optimizada por Alex Martel + ChatGPT


FIN DEL README TXT
