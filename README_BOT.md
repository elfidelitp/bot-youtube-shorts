# 🤖 Bot Autónomo de YouTube Shorts 24/7

Genera y publica Shorts de terror estilo *"reglas de pueblos de Roblox"* sin intervención humana: produce lotes de videos verticales 9:16 con voz IA, subtítulos dinámicos estilo Hormozi, y los sube a la API oficial de YouTube respetando una pausa estricta de 4-6 horas entre publicaciones.

## 📁 Estructura creada por el bot

| Carpeta/Archivo | Descripción |
|---|---|
| `main.py` | Script completo del bot |
| `requirements.txt` | Dependencias |
| `client_secret.json` | Credenciales OAuth (las pones tú) |
| `token.pickle` | Sesión guardada (auto) — no pedir login cada ciclo |
| `bot_activity.log` | Log completo de actividad (auto) |
| `estado_bot.json` | Pausas e intentos persistentes (auto) |
| `cola_videos/` | Videos renderizados esperando subida |
| `subidos/` | Archivo de videos ya publicados |
| `errores/` | Videos que agotaron sus intentos |
| `temp/` | Archivos temporales de producción |

## 🚀 Puesta en marcha (primera vez, con ventana visible)

```powershell
cd C:\Users\Fidelito\Downloads\proyectos
pip install -r requirements.txt
python main.py
```

1. La primera vez se abre el navegador → inicia sesión con la cuenta del canal → autoriza → cierra pestaña. Queda guardado en `token.pickle` y no se volverá a pedir.
2. (Opcional) Genera un video de muestra sin subir nada: `python main.py --prueba`
3. El bot arranca: si `cola_videos/` está vacía fabrica un lote de 5 videos (tarda: la 1.ª vez descarga el modelo Whisper y el ffmpeg automático), luego sube 1, espera 4-6 h, sube el siguiente…

## 🌙 Dejarlo corriendo en segundo plano (2 días)

**Opción A — Doble clic:** ejecuta `iniciar_bot.bat` (ventana minimizada).

**Opción B — PowerShell oculto:**
```powershell
Start-Process pythonw -ArgumentList "main.py" -WorkingDirectory "C:\Users\Fidelito\Downloads\proyectos" -WindowStyle Hidden
```

**Opción C — Programador de tareas (recomendada para 2 días):**
```powershell
schtasks /Create /TN "BotYT24" /TR "\"C:\Users\Fidelito\Downloads\proyectos\iniciar_bot.bat\"" /SC ONLOGON /F
schtasks /Run /TN "BotYT24"
```

**Requisitos físicos de los 2 días:** el PC encendido, sin suspensión:
```powershell
powercfg /change standby-timeout-ac 0
```

## 📡 Monitoreo en vivo

```powershell
Get-Content .\bot_activity.log -Wait -Tail 20
```

## ⏹ Detener el bot

- Si corre en consola: `Ctrl + C`
- Si corre oculto: `taskkill /IM pythonw.exe /F`

Al reiniciarse, el bot **respeta la pausa pendiente** (gracias a `estado_bot.json`), no re-sub videos ya publicados (se movieron a `subidos/`) y reintenta los fallidos hasta 5 veces.

## ⚙️ Personalización (constantes al inicio de `main.py`)

- `VIDEOS_POR_LOTE`, `N_REGLAS`, `VOZ`, `MODELO_WHISPER`
- `ANCHO/ALTO` → `1080, 1920` si tu PC renderiza bien (más calidad, más lento)
- Plantillas de historias, títulos, hashtags → bancos de texto editables

## ⚠️ Notas de buen uso

- Cuota diaria de la API: 10.000 unidades ≈ 6 subidas/día; con pausas de 4-6 h quedas dentro del límite.
- La descripción ya declara que es ficción con voz de IA (recomendado por YouTube).
- YouTube puede limitar la monetización de contenido repetitivo generado masivamente; publica con tu propio criterio.
