
## Crear entorno virtual python 

Nos metemos en la carpeta donde queramos crear el entorno virtual y simplemente escribimos

```
virtualenv "nombre del entorno"
cd "nombre del entorno"
.\Scripts\Activate.ps1 #para activarlo
```

**Nota: Ejecutar powershell con admin. Si falla la activación escribir:**

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUse
```

Un error que me ha salido cuando intentaba instalar airflow es que no era compatible con mi version de python, asi que la podemos modificar así:

```
virtualenv -p "C:\Users\ruben\AppData\Local\Microsoft\WindowsApps\python3.11.exe" airflow-env
```

# GIT
Automatización apuntes github.

Primero iniciamos bash de git en la carpeta de mis apuntes.

```
git init
git remote add origin "link repositorio"
git branch -M main
git push -u origin main
git add .
git commit -m "titulo commit"
git push origin main
```




