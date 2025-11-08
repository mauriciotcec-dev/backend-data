# backend-data

## Resolviendo Problemas de Permisos de GitHub

Si encuentras el error: **"You don't have permissions to push to 'mauriciotcec-dev/backend-data' on GitHub"**, aquí están las soluciones:

### Solución 1: Hacer un Fork del Repositorio

1. Ve a https://github.com/mauriciotcec-dev/backend-data
2. Haz clic en el botón "Fork" en la esquina superior derecha
3. Clona tu fork:
   ```bash
   git clone https://github.com/TU_USUARIO/backend-data.git
   cd backend-data
   ```
4. Agrega el repositorio original como upstream:
   ```bash
   git remote add upstream https://github.com/mauriciotcec-dev/backend-data.git
   ```
5. Haz tus cambios y push a tu fork:
   ```bash
   git add .
   git commit -m "Tus cambios"
   git push origin tu-branch
   ```
6. Crea un Pull Request desde tu fork al repositorio original

### Solución 2: Configurar Autenticación con Personal Access Token (PAT)

1. Ve a GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Genera un nuevo token con permisos `repo`
3. Configura Git para usar el token:
   ```bash
   git remote set-url origin https://TU_TOKEN@github.com/mauriciotcec-dev/backend-data.git
   ```
   O usa:
   ```bash
   git remote set-url origin https://TU_USUARIO:TU_TOKEN@github.com/mauriciotcec-dev/backend-data.git
   ```

### Solución 3: Configurar SSH Keys

1. Genera una clave SSH (si no tienes una):
   ```bash
   ssh-keygen -t ed25519 -C "tu_email@example.com"
   ```
2. Agrega la clave SSH a tu cuenta de GitHub:
   - Copia la clave pública: `cat ~/.ssh/id_ed25519.pub`
   - Ve a GitHub Settings → SSH and GPG keys → New SSH key
   - Pega la clave y guarda
3. Cambia la URL del remote a SSH:
   ```bash
   git remote set-url origin git@github.com:mauriciotcec-dev/backend-data.git
   ```

### Solución 4: Solicitar Acceso al Repositorio

Si necesitas acceso directo al repositorio, contacta al propietario (mauriciotcec-dev) para que te agregue como colaborador.

## Troubleshooting

### Verificar tu configuración actual:
```bash
git remote -v
git config --list | grep user
```

### Limpiar credenciales almacenadas:
```bash
# En macOS
git credential-osxkeychain erase
host=github.com
protocol=https
[presiona Enter dos veces]

# En Windows
git credential-manager erase https://github.com

# En Linux
git config --global --unset credential.helper
```

Para más información, consulta [CONTRIBUTING.md](CONTRIBUTING.md).