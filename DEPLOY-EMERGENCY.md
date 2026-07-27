# 🚨 DEPLOY DE EMERGENCIA - OpenWA en Railway

El deploy automático desde GitHub está roto ("upstream GitHub issues").
Estas son las 3 opciones para forzar el deploy, de más fácil a más invasiva.

---

## OPCIÓN 1 (RECOMENDADA): Re-conectar GitHub en Railway

Esto resincroniza el webhook Railway↔GitHub.

1. Abrí https://railway.app/dashboard
2. Click en tu proyecto "OpenWA"
3. Arriba a la derecha → **"Settings"**
4. Sección **"Source"** → click en **"Disconnect"**
5. Esperá 5 segundos
6. Click en **"Connect Repo"** → seleccioná `JeanCardozo/OpenWA` → branch `main`
7. Railway hace deploy inmediato

**Si esto funciona, ya está. El deploy aparece en 1-2 minutos con el commit `231cd29`.**

---

## OPCIÓN 2: Forzar redeploy desde el dashboard

1. Railway Dashboard → tu servicio → tab **"Deployments"**
2. Buscá el deploy "Queued" del commit `231cd29`
3. Click en los 3 puntos (⋯) → **"Cancel"** (si está disponible)
4. Click en **"Deploy"** (arriba a la derecha) → **"Redeploy"**
5. Si te deja elegir commit, seleccioná `231cd29`
6. Esperá 1-2 minutos

---

## OPCIÓN 3 (BALA DE PLATA): Deploy por CLI

Si las opciones 1 y 2 no funcionan, este método deploya el código local sin depender de GitHub.

### Paso 1: Obtener tu token de Railway

1. Abrí https://railway.app/account/tokens
2. Click en **"Create Token"**
3. Copialo (solo se muestra una vez)

### Paso 2: Login desde tu terminal

```powershell
cd "C:\Users\Jean Cardozo\Documents\MicroSaas\OpenWA"
railway login --browserless
# Pegá el token cuando te lo pida
```

### Paso 3: Linkear al proyecto

```powershell
railway link
# Te muestra una lista de proyectos → elegí "OpenWA"
```

### Paso 4: Deployar

```powershell
railway up
# Esto sube tu código local y redeploya
# Tarda 1-2 minutos
```

Si `railway up` da error de "no project linked", probá:

```powershell
railway up --service openwa-production-52ce
```

---

## Una vez deployado

Independientemente de la opción, una vez que el deploy esté activo:

1. **Verificá** que la versión sea 0.10.9 con el fix `forcePn`:
   ```
   curl https://openwa-production-52ce.up.railway.app/api/health
   ```

2. **Andá al dashboard** → Sessions → escaneá el QR (la sesión se cayó)

3. **Una vez `ready`:** Sessions → `cuentasflash` → **Restart** (limpia cache LID)

4. **Configurá el webhook** con esta URL exacta:
   ```
   https://cuentasflash.online/api/webhooks/whatsapp?api_key=owa_k1_7c94cea054398a3e65abeae451cb2389c3b915c6c9361b652b569e1cbff53a38
   ```

5. **Probalo** con un número que antes fallaba → debería llegar
