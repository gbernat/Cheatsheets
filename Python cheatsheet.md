# Python cheatsheet

## Environments
### 1) Legacy:

- Create a folder which will house the virtual environments:
```
# mkdir ~/.venvs && cd ~/.venvs
```
This is optional if you like to have your envs separatelly from code folder. Otherwise, create venv in the code path without mkdir. Add it to .gitignore

- Make the virtual environment:
```
# python3 -m venv my-first-environment
```
This command will create a directory for de virt env, and put the virt env files inside it.  
As a good practice, create venv in code path, and name it 'venv': ```python3 -m venv venv```. 

Running this command should generate a folder with an internal file structure that looks something like this:  
```
├── bin  
│   ├── activate  
│   ├── activate.csh  
│   ├── activate.fish  
│   ├── easy_install  
│   ├── easy_install-3.6  
│   ├── pip  
│   ├── pip3  
│   ├── pip3.6  
│   ├── python -> python3.6  
│   ├── python3 -> python3.6  
├── include  
├── lib  
│   └── python3.6  
│       └── site-packages  
└── pyvenv.cfg  
```
- Use the virtual environment:
```
# source ~/.venvs/my-first-environment/bin/activate
```
source can be expressed as just .

Now any packages installed with pip should be contained within this environment, and inaccessible outside of it.  
... work ...  
Save all the dependencies and packages installed to be reused:
```
# pip freeze > requirements.txt
```

- Exiting the virtual environment:
```
# deactivate
```

- Using the requirements.txt list to setup a new environment:
```
# python3 -m venv my-second-environment
# . my-second-environment/bin/activate
# pip install -r requirements.txt
```

ON WINDOWS:
- Make the virtual environment: (Igual que Linux)
```
  python3 -m venv venv 
```
- Use the virtual environment: (Cambia)
```
  venv\Scripts\activate.bat  o  venv\Scripts\Activate.ps1
```
- Exiting the virtual environment: (Igual que Linux)
```
  deactivate
````

***Para usarlo en VSCode***: 

Con la carpeta del proyecto abierta (en el path anterior a la carpeta venv):
View Command Palette -> "Interpreter" -> Seleccionar el interprete de Python dentro de venv.
Listo. Cuando abra un terminal en VSCode, va a aparecer (venv)  
Refs:  
https://www.youtube.com/watch?v=4jt9JPoIDpY  
https://www.youtube.com/watch?v=-nh9rCzPJ20&t=1951s

OBS! Para activar el venv en VSCode, como ejecuta automaticamente el activate.ps1  
Tuve que cambia las execution policies de powershell:  
Get-ExecutionPolicy -List  
Lo pase a:  
```Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Bypass```


### 2) pipenv:
```
# pip install pipenv
```
Create a folder which will house the virtual environments:
```
# mkdir ~/dev/projects/my-new-project && cd ~/dev/projects/my-new-project
```

Instantiate pipenv and create a new virtual environment for the project:
```
# pipenv install
````
This will create a Pipfile file that will be automatically updated whenever you install a new package.

Adding new packages to your project:
```
# pipenv install flask
```
To show which packages you have installed (and the dependencies of those dependencies):
```
# pipenv graph
```
Activate the virtual environment:
```
# pipenv shell
```
Alternatively, you can run scripts without activating the virtual environment first by running commands like so:
```
# pipenv run python my-script.py
```
Exiting the virtual environment:
```
# exit
```

---

## Tips

- Commandline JSON Pretty Print everywhere
```
# echo '{"test1": 1, "test2": "win"}' | python -m json.tool
will be returned as:
{
    "test1": 1, 
    "test2": "win"
}
```

- Ver arbol de dependencias de paquetes instalados (necesito pipdeptree)
```
(venv) Giskard:tstdependencies guido$ pipdeptree
paramiko==2.7.2
  - bcrypt [required: >=3.1.3, installed: 3.2.0]
    - cffi [required: >=1.1, installed: 1.14.4]
      - pycparser [required: Any, installed: 2.20]
    - six [required: >=1.4.1, installed: 1.15.0]
  - cryptography [required: >=2.5, installed: 3.3.1]
    - cffi [required: >=1.12, installed: 1.14.4]
      - pycparser [required: Any, installed: 2.20]
    - six [required: >=1.4.1, installed: 1.15.0]
  - pynacl [required: >=1.0.1, installed: 1.4.0]
    - cffi [required: >=1.4.1, installed: 1.14.4]
      - pycparser [required: Any, installed: 2.20]
    - six [required: Any, installed: 1.15.0]
pipdeptree==2.0.0
  - pip [required: >=6.0.0, installed: 19.0.3]
setuptools==40.8.0
```

Pipdeptree salida en json:
```
$ pipdeptree -p paramiko -j
```

#### Otra forma de ver dependencias, sin instalar pipdeptree. No hace recursivo:  
```
$ pip3 show paramiko
Name: paramiko
Version: 2.7.2
Summary: SSH2 protocol library
Home-page: https://github.com/paramiko/paramiko/
Author: Jeff Forcier
Author-email: jeff@bitprophet.org
License: LGPL
Location: /Users/guido/Documents/ASAPP/tstdependencies/venv/lib/python3.7/site-packages
**Requires: pynacl, cryptography, bcrypt**
Required-by: 
Files:
  paramiko-2.7.2.dist-info/DESCRIPTION.rst
  paramiko-2.7.2.dist-info/INSTALLER
  paramiko-2.7.2.dist-info/LICENSE.txt
  paramiko-2.7.2.dist-info/METADATA
  paramiko-2.7.2.dist-info/RECORD
  paramiko-2.7.2.dist-info/WHEEL
  paramiko-2.7.2.dist-info/metadata.jso
  ...  
```

Para ver recursivo, hay que hacerlo a mano hasta que no hay mas Requires:  
```
(venv) Giskard:tstdependencies guido$ pip3 show paramiko |grep -i requires
Requires: bcrypt, pynacl, cryptography
(venv) Giskard:tstdependencies guido$ pip3 show cryptography |grep -i requires
Requires: cffi, six
(venv) Giskard:tstdependencies guido$ pip3 show cffi |grep -i requires
Requires: pycparser
(venv) Giskard:tstdependencies guido$ pip3 show pycparser |grep -i requires
Requires: 
```



---

## Installation package

- Crear source distribution paquete .tar.gz  
Esto se ejecuta en el path donde esta setup.py, y crea el .tar.gz en /dist
```
# python setup.py sdist  
```

- Instalar / updatear desde tar.gz:  
# pip install -–user <package_name>-x.x.x.tar.gz
o
(usar este) # pip install --upgrade <package_name>-x.x.x.tar.gz

- Listar paquetes instalados:  
pip list o pip freeze

