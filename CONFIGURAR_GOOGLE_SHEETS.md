# 📊 Cómo conectar el juego a Google Sheets

Con esto todos los participantes quedarán guardados automáticamente
en una hoja de cálculo que puedes ver desde cualquier dispositivo.

---

## PASO 1 — Crear la hoja de Google Sheets

1. Ve a https://sheets.google.com
2. Crea una hoja nueva
3. En la fila 1 escribe estos títulos (uno por columna):
   NOMBRE | CEDULA | FICHA | FRASE

---

## PASO 2 — Crear el Apps Script

1. En la hoja, ve al menú: **Extensiones → Apps Script**
2. Borra todo el código que aparece
3. Pega este código exactamente:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    sheet.appendRow([
      data.nombre  || '',
      data.documento || '',
      data.ficha   || '',
      data.frase   || ''
    ]);
    return ContentService.createTextOutput(JSON.stringify({ok:true}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService.createTextOutput(JSON.stringify({error:err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Haz clic en **Guardar** (ícono del disquete)
5. Ponle nombre al proyecto: `BodhiLiterario`

---

## PASO 3 — Publicar como Web App

1. Haz clic en **Implementar → Nueva implementación**
2. Tipo: **Aplicación web**
3. Configuración:
   - Descripción: `Bodhi Literario`
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier usuario**
4. Haz clic en **Implementar**
5. Autoriza los permisos cuando te lo pida
6. **COPIA la URL** que aparece (algo como `https://script.google.com/macros/s/AKfy.../exec`)

---

## PASO 4 — Pegar la URL en el juego

1. Abre `index.html` con un editor de texto (Notepad, VS Code, etc.)
2. Busca esta línea:
   ```
   var APPS_SCRIPT_URL = '';
   ```
3. Pega tu URL entre las comillas:
   ```
   var APPS_SCRIPT_URL = 'https://script.google.com/macros/s/TU_URL.../exec';
   ```
4. Guarda el archivo

---

## ✅ Listo

Desde ahora cada participante que complete el juego quedará
registrado automáticamente en tu Google Sheet con:
- NOMBRE
- CÉDULA  
- FICHA
- FRASE

Los datos también se guardan en localStorage como respaldo.

---

## 💡 Ver los datos

- Abre tu Google Sheet en cualquier momento
- O usa el Panel Admin del juego (⚙️) → Exportar a Excel
