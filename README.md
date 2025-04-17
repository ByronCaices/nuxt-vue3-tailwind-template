# Nuxt Minimal Starter

Look at the [Nuxt documentation](https://nuxt.com/docs/getting-started/introduction) to learn more.

## Setup

Make sure to install dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install

# bun
bun install
```

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm dev

# yarn
yarn dev

# bun
bun run dev
```

## Production

Build the application for production:

```bash
# npm
npm run build

# pnpm
pnpm build

# yarn
yarn build

# bun
bun run build
```

Locally preview production build:

```bash
# npm
npm run preview

# pnpm
pnpm preview

# yarn
yarn preview

# bun
bun run preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.

## Faker data 
# models 
- student
- carrer
- departament
- facultie

# endpoints 
[GET]
- ```/api/students ```
- ```/api/students/{id} ```
- ```/api/carrers ```
- ```/api/departaments ```
- ```/api/faculties ```

## Project Structure

```
pages/
 └─ commitments/
     ├─ manual-entry/
     │   └─ index.vue              // 1. Manual Entry
     ├─ payment/
     │   └─ index.vue             // 2. Payment
     ├─ early-payment/
     │   └─ index.vue             // 3. Early Payment
     ├─ documentation/
     │   └─ index.vue             // 4. Commitment Documentation
     ├─ update-paid/
     │   └─ index.vue             // 5. Update Paid Commitments
     ├─ delete-no-movements/
     │   └─ index.vue             // 6. Delete Without Movements
     ├─ school-pass/
     │   └─ index.vue             // 7. Load School Pass
     ├─ basic-fee/
     │   └─ index.vue             // 8. Basic Fee
     ├─ payment-voucher/
     │   └─ index.vue             // 9. Payment Coupon Issue
     ├─ payment-transfer/
     │   └─ index.vue             // 10. Payment Transfer
     ├─ payment-discount/
     │   └─ index.vue             // 11. Installment Payment with Payroll Discount
     ├─ bank-deposit/
     │   └─ index.vue             // 12. Payment with Bank Deposit (PAC - PAT)
     ├─ legal-retention/
     │   └─ index.vue             // 13. Load Legal Retention for Resignation
     ├─ special-payments/
     │   └─ index.vue             // 14. Enter Special Payments
     ├─ cancel-bank-deposit/
     │   └─ index.vue             // 15. Cancel Bank Deposit (PAC - PAT)
     ├─ competency-exam/
     │   └─ index.vue             // 16. Load Proficiency Test
     ├─ unloaded-payments/
     │   └─ index.vue             // 17. Unregistered Web Payments
     └─ reverse-web-payments/
         └─ index.vue             // 18. Web Payment Reservations
```

# API Endpoints Documentation

## Facultades (Faculties)

### Obtener todas las facultades
```
GET /api/faculties
```
**Query Parameters:**
- `includeDepartments` (boolean): Include departments list for each faculty
  ```typescript
  // Example response with includeDepartments=true
  {
    facultad_id: number,
    nombre: string,
    departamentos: Department[]
  }
  ```

### Obtener una facultad específica
```
GET /api/faculties/:id
```
- Retorna la facultad con sus departamentos por defecto
- No requiere parámetros adicionales

### Obtener departamentos de una facultad
```
GET /api/faculties/:facultyId/departments
```
**Query Parameters:**
- `includeCareers` (boolean): Include careers list for each department
  ```typescript
  // Example response with includeCareers=true
  {
    departamento_id: number,
    nombre: string,
    facultad_id: number,
    carreras: Career[]
  }
  ```

### Obtener todas las carreras de una facultad
```
GET /api/faculties/:facultyId/all-careers
```
- Retorna todas las carreras asociadas a todos los departamentos de la facultad
- No requiere parámetros adicionales
```typescript
// Response structure
{
  facultyId: number,
  departments: Department[],
  careers: Career[]
}
```

## Departamentos (Departments)

### Obtener todos los departamentos
```
GET /api/departments
```
**Query Parameters:**
- `includeCareers` (boolean): Include departments list for each faculty
  ```typescript
  // Example response with includeCareers=true
  {
    departamento_id: number,
    nombre: string,
    facultad_id: number,
    careeras: Career[]
  }
  ```

### Obtener un departamento específico
```
GET /api/departments/:id
```
**Query Parameters:**
- `includeCareers` (boolean): Include associated careers
- `includeFaculty` (boolean): Include faculty information
```typescript
// Example response with both parameters true
{
  departamento_id: number,
  nombre: string,
  facultad_id: number,
  facultad: Faculty,
  carreras: Career[]
}
```

### Obtener carreras de un departamento
```
GET /api/departments/:departmentId/careers
```
- Retorna la lista de carreras asociadas al departamento
- No requiere parámetros adicionales

## Carreras (Careers)

### Obtener todas las carreras
```
GET /api/careers
```
**Query Parameters:**
- `includeDepartment` (boolean): Include department and faculty information
  ```typescript
  // Example response with includeDepartment=true
  {
    codigo_carrera: string,
    departamento_id: number,
    grado_id: number,
    antecedente_id: number,
    es_acreditada: boolean,
    nombre_carrera: string,
    departamento: Department
    facultad: Faculty
  }
  ```

### Obtener una carrera específica
```
GET /api/careers/:id
```
**Query Parameters:**
- `includeDepartment` (boolean): Include department and faculty information
```typescript
// Example response with includeDepartment=true
{
  codigo_carrera: string,
  departamento_id: number,
  grado_id: number,
  antecedente_id: number,
  es_acreditada: boolean,
  nombre_carrera: string,
  departamento: Department
  facultad: Faculty
}
```

## Estructuras de Datos

### Faculty
```typescript
{
  facultad_id: number
  nombre: string
  departamentos?: Department[]
}
```

### Department
```typescript
{
  departamento_id: number
  nombre: string
  facultad_id: number
  facultad?: Faculty
  carreras?: Career[]
}
```

### Career
```typescript
{
  codigo_carrera: string
  departamento_id: number
  grado_id: number
  antecedente_id: number
  es_acreditada: boolean
  nombre_carrera: string
  departamento?: Department
  facultad?: Faculty
}
```

## Ejemplos de Uso

### Obtener una facultad con todos sus datos relacionados
```typescript
const { data: faculty } = await useFetch('/api/faculties/1', {
  query: {
    includeDepartments: true
  }
})
```

### Obtener departamentos de una facultad con sus carreras
```typescript
const { data: departments } = await useFetch(`/api/faculties/${facultyId}/departments`, {
  query: {
    includeCareers: true
  }
})
```

### Obtener una carrera con información de su departamento
```typescript
const { data: career } = await useFetch(`/api/careers/${id}`, {
  query: {
    includeDepartment: true
  }
})
```

### Obtener todas las carreras de una facultad
```typescript
const { data } = await useFetch(`/api/faculties/${facultyId}/all-careers`)
```