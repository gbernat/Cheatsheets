# Python cheatsheet

---
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
- Subprocess  
  - How to execute multiple commands in a single subprocess call:
```python
res = subprocess.run(['ssh', '-i', '/Users/EC2.pem', 'ec2-user@1.1.1.1;', 'ls', '-hal', '/tmp/;', 'pwd'], shell=False, stdout=subprocess.PIPE)

res.stdout
```
```
b'total 0\ndrwxrwxrwt  9 root     root     261 Jan 15 10:20 .\ndr-xr-xr-x 18 root     root     257 Jan  6 23:15 ..\ndrwxrwxrwt  2 root     root       6 Jan  6 23:15 .font-unix\ndrwxrwxrwt  2 root     root       6 Jan  6 23:15 .ICE-unix\n-rw-r--r--  1 ec2-user ec2-user   0 Jan 15 03:52 pepe\ndrwx------  3 root     root      17 Jan 15 10:20 systemd-private.service-lcjkJ5\ndrwx------  3 root     root      17 Jan 15 10:20 systemd-private-httpd.service-qFuTJf\ndrwxrwxrwt  2 root     root       6 Jan  6 23:15 .Test-unix\n/home/ec2-user\n'
```
```
res
```
```
CompletedProcess(args=['ssh', '-i', '/Users//EC2.pem', 'ec2-user@52.67.253.187', 'ls', '-hal', '/tmp/;', 'pwd'], returncode=0, stdout=b'total 0\ndrwxrwxrwt  9 root     root     261 Jan 15 10:20 .\ndr-xr-xr-x 18 root     root     257 Jan  6 23:15 ..\ndrwxrwxrwt  2 root     root       6 Jan  6 23:15 .font-unix\ndrwxrwxrwt  2 root     root       6 Jan  6 23:15 .ICE-unix\n-rw-r--r--  1 ec2-user ec2-user   0 Jan 15 03:52 pepe\ndrwx------  3 root     root      17 Jan 15 10:20 systemd-private.service-lcjkJ5\ndrwx------  3 root     root      17 Jan 15 10:20 systemd-private-httpd.service-qFuTJf\ndrwxrwxrwt  2 root     root       6 Jan  6 23:15 .Test-unix\n/home/ec2-user\n')
```

Subprocess returns stdout in binary. To convert to String:  
```res.stdout.decode('utf-8')```  
  
  
  - Optionally (however, using subprocess.run is prefered):
```
encoding = 'latin1'  
p = subprocess.Popen('cmd.exe', stdin=subprocess.PIPE,
             stdout=subprocess.PIPE, stderr=subprocess.PIPE)
for cmd in cmds:
    p.stdin.write(cmd + "\n")
p.stdin.close()
print p.stdout.read()
```
  Ref: https://stackoverflow.com/questions/39721924/how-to-run-multiple-commands-synchronously-from-one-subprocess-popen-command/39722695

- **Prefered method to execute commands and getting output:**  
```python
res = subprocess.run([cmd, arg1, arg2], capture_output=True)  
stdout_in_string = res.stdout.decode('utf-8')
```  
> capture_output=True is equal to stdout=subprocess.PIPE, stderr=subprocess.PIPE (since Python 3.5)  
Since Python 3.7 instead of using .decode('utf-8') to get it in string, use run param: ```text=True``` 






---

## Installation package

- Crear source distribution paquete .tar.gz  
Esto se ejecuta en el path donde esta setup.py, y crea el .tar.gz en /dist
```
# python setup.py sdist  
```

- Instalar / updatear desde tar.gz:  
```
# pip install -–user <package_name>-x.x.x.tar.gz
o
(usar este) # pip install --upgrade <package_name>-x.x.x.tar.gz
```

- Install package in specified dir, instead of default Pyhton location:  
Example in current dir:  
```
# pip3 install paramiko -t .
```
- Listar paquetes instalados:  
```
pip list o pip freeze
```

---
## Logging among classes
To get logging either from main program as inner classes:

In main script:  
```python
import logging

# Global logging config
log_file = '/tmp/log_file.log'

file_handler = logging.FileHandler(filename=log_file)
console_handler = logging.StreamHandler()

file_handler.setLevel(logging.WARNING)    # specific for file, if needed
console_handler.setLevel(logging.INFO)    # specific for console, if needed

formater = logging.Formatter('%(asctime)s - %(name)s - [%(levelname)s] - %(message)s')
file_handler.setFormatter(formatter)
console_handler.setFormatter(formatter)

# root logger, no __name__ as in submodules further down the hierarchy
##global logger
logger = logging.getLogger()
logger.setLevel(logging.DEBUG)  # general

logger.addHander(file_handler)
logger.addHander(console_handler)

```


In inner classes:  
```python
import logging

# Global
logger = logging.getLogger(__name__)
```

---
## Error handling behavior
Showing by examples the behavior of error handling on inner funtions, parents, and raising

### Example code with error variants:
```python
def error_not_catched():
    1/0
    print('[fn] I have just made a fatal error. Never executed...')

def error_catched():
    try:
        1/0
    except Exception as e:
        print('[fn] I made an error, but I captured it. Error: {}'.format(e))

def catched_and_raised():
    try:
        1/0
    except Exception as e:
        print('[fn] I made an error, I captured it, and I am raising it. Error: {}'.format(e))
        raise 
        print('[fn] After raise in except. Never executed...')
    print('[fn] After raise in outside try/except block.')

def no_catched_raise_something():
    raise Exception('This is the exception expected to handle. Derives from Exception, but can be anyother more accurate.')
    print('[fn] After raise. Never executed...')


def parent_function():
    try:
        print('[main] Calling func that fails without error capture:')
        error_not_catched()
        print('[main] Result: program cancel if not captured in parent. Never executed...')
    except Exception as e:
        print('[main_except] Result: Captured in parent. Error: {}\n'.format(e))
        
    print('[main] Calling func that fails and error is captured inside:')
    error_catched()
    print("[main_except] Result: parent func doesn't notice the inner error. Program continues.\n")
    
    try:
        print('[main] Calling func that captures error and raises in except:')
        catched_and_raised()
        print('After raise. Never executed...')
    except Exception as e:
        print('[main_except] Result: parent func recives the error and program cancel if not handled in parent.')
        print('[main_except] Captured in parent. Inner error raised: {}\n'.format(e))
      
    print('[main] Calling func that manually raises something:')
    try:
        no_catched_raise_something()
        print('After raise. Never executed...')
    except Exception as e:
        print('[main_excpet] Result: parent func recives the error and program cancel if not handled in parent. Inner funtion cancels execution after raise.')
        print('[main_except] Captured in parent. Inner eror raised: {}\n'.format(e))


parent_function()

```

### Output:
```
[main] Calling func that fails without error capture:
[main_except] Result: Captured in parent. Error: division by zero

[main] Calling func that fails and error is captured inside:
[fn] I made an error, but I captured it. Error: division by zero
[main_except] Result: parent func doesn't notice the inner error. Program continues.

[main] Calling func that captures error and raises in except:
[fn] I made an error, I captured it, and I am raising it. Error: division by zero
[main_except] Result: parent func recives the error and program cancel if not handled in parent.
[main_except] Captured in parent. Inner error raised: division by zero

[main] Calling func that manually raises something:
[main_excpet] Result: parent func recives the error and program cancel if not handled in parent. Inner funtion cancels execution after raise.
[main_except] Captured in parent. Inner eror raised: This is the exception expected to handle. Derives from Exception, but can be anyother more accurate.
```

> References:
https://stackoverflow.com/questions/2052390/manually-raising-throwing-an-exception-in-python  
https://docs.python.org/3/library/exceptions.html#exception-hierarchy  
https://docs.python.org/3/tutorial/errors.html


---


