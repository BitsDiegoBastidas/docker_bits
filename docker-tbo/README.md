# Recomendaciones para usuarios windows
Antes de clonar el proyecto TBO ejecutar el siguiente comando dentro de la carpeta o disco donde se almacenará los archivos fuentes de TBO:

git config --global core.autocrlf input


# Configuración de VSCODE para habilitación de debugger 


{
    // Use IntelliSense para saber los atributos posibles.
    // Mantenga el puntero para ver las descripciones de los existentes atributos.
    // Para más información, visite: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        
        {
            "name": "Listen for Xdebug on Docker",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                "/usr/share/nginx/html": "${workspaceFolder}/PATH_TO_REPO_TBO_LOCAL"
            },
            "xdebugSettings": {
               "max_data": -1,
            }
        }
    ]
}

# AYUDAS
    - Solución al error "You are not allowed to use docker you must be in the docker-users group"
        - https://www.youtube.com/watch?v=9Hc5gePoDt8