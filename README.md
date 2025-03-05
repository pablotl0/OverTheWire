# OverTheWire: Bandit Solutions

Este repositorio contiene las soluciones paso a paso para los niveles del wargame **Bandit** de OverTheWire.

## 📌 Descripción
**Bandit** es un juego diseñado para enseñar los fundamentos de Linux y la seguridad informática. Cada nivel presenta un desafío que requiere el uso de comandos y herramientas del sistema para encontrar la contraseña del siguiente nivel.

## 🚀 Requisitos
Para jugar a **Bandit**, necesitas:
- Acceso a una terminal (Linux/MacOS o Git Bash en Windows).
- Cliente SSH instalado.
- Conocimientos básicos de comandos de Linux.

## 📜 Soluciones
Cada archivo de nivel incluye:
- **Objetivo** 🎯: Qué se necesita hacer.
- **Comando** 💻: Los comandos necesarios.
- **Explicación** 📖: Cómo funciona la solución.

Ejemplo para **Bandit Level 1 → Level 2**:
```md
### Nivel 1 → Nivel 2

**Objetivo:** Leer el archivo oculto en el directorio home.

**Comando:**
```bash
ls -la
cat ./-
```

**Solución:**
Usamos `ls -la` para listar los archivos ocultos y `cat ./-` para leer el contenido del archivo `-`, que contiene la contraseña del siguiente nivel.
```

## 🎯 Niveles disponibles
✅ **Niveles 0 a 33 resueltos.**

## 📜 Recursos adicionales
- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)
- [Comandos básicos de Linux](https://www.gnu.org/software/bash/manual/bash.html)



