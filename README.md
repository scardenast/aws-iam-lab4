# 🧪 Laboratorio 4 – IAM Avanzado con Roles y Políticas por Grupo

## 🎯 Objetivo del laboratorio
Aplicar el **principio de mínimo privilegio** en AWS utilizando:
- Grupos de IAM
- Políticas personalizadas
- Asignación de roles mediante herencia grupal
- Simulación de acceso con usuarios restringidos

## 🛠 Herramientas usadas
- AWS IAM
- AWS CLI
- AWS STS
- Perfiles de usuario configurados en `~/.aws/credentials`

---

## 🧭 Pasos realizados

### 1. Crear carpeta de trabajo
📸 `01-aws-iam-roles-grupos.png`

### 2. Crear usuarios IAM
```bash
aws iam create-user --user-name usuario-analista
aws iam create-user --user-name usuario-dev
```
📸 `02-crear-usuarios.png`

### 3. Verificar usuarios creados
```bash
aws iam list-users
```
📸 `03-lista-usuarios.png`

### 4. Crear políticas personalizadas
- `PoliticaAnalistas` con permisos sobre S3 y CloudTrail
- `PoliticaDevs` con permisos para describir instancias EC2

📸 `05-politica-devs.png`, `06-politica-analistas.png`

### 5. Crear grupos IAM
```bash
aws iam create-group --group-name grupo-analistas
aws iam create-group --group-name grupo-devs
```
📸 `07-crear-grupos.png`

### 6. Crear políticas en AWS
📸 `08-aws-create-policy-analistas.png`, `09-aws-create-policy-devs.png`

### 7. Asociar políticas a grupos
```bash
aws iam attach-group-policy --group-name grupo-analistas --policy-arn arn:...
aws iam attach-group-policy --group-name grupo-devs --policy-arn arn:...
```
📸 `10-asociar-politicas.png`, `11-asociar-politica-analistas.png`, `12-asociar-politica-devs.png`

### 8. Crear credenciales de acceso
📸 `13-credencial-analista.png`, `14-credencial-dev.png`

### 9. Configurar perfiles locales
```bash
aws configure --profile analista
aws configure --profile dev
```
📸 `15-configurar-perfiles.png`

### 10. Asociar usuarios a grupos
```bash
aws iam add-user-to-group --user-name usuario-analista --group-name grupo-analistas
aws iam add-user-to-group --user-name usuario-dev --group-name grupo-devs
```
📸 `16-user-analista-asociado-a-grupo.png`, `18-user-devs-asociado-a-grupo.png`

### 11. Probar permisos del usuario analista
```bash
aws s3 ls --profile analista
```
📸 `17-listar-buckets.png`

### 12. Probar permisos del usuario dev
```bash
aws ec2 describe-instances --profile dev
aws s3 ls --profile dev  # debe fallar
```
📸 `19-dev-describe-instancias.png`, `20-dev-acceso-denegado-s3.png`

---

## ✅ Resultado esperado
- `usuario-analista` puede listar buckets S3 y consultar CloudTrail
- `usuario-dev` puede ver instancias EC2, pero NO acceder a S3
- Cualquier acción fuera de lo definido es denegada

---

## ⚠️ Errores comunes resueltos
- `Unable to locate credentials`: faltaba configurar perfil con `aws configure`
- `AccessDenied`: usuario no estaba asociado al grupo o política no aplicada
- Región no especificada: se corrigió con `--region` o `aws configure`

---

## 🧠 Lección clave
Este laboratorio demuestra la aplicación **real del principio de mínimo privilegio**, utilizando buenas prácticas de IAM como:
- Políticas personalizadas
- Separación de responsabilidades
- Herencia a través de grupos
- Simulación de usuarios con perfiles locales