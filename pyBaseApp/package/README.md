# package module

## Description
The package module makes use of Pyinstaller to build an app package ready to go.
It is a wrapper to ease the use of Pyinstaller.
It defines the build parameters in a settings file (yml file) and also add the ability to add external documents (documentation, config files, ...)and a leucher (.bat or .sh file)

Note that Pyinstaller builds executable files according to the OS on chich the program is exectuted :
- if executed on Windows, a windows executable file is generated
- if executed on Linux, a linux executable file is generated

## Example
Here is a basic example of use:

    from pyBaseApp.package import Package, Options
    from pyBaseApp.applauncher import Configuration
     
    settings = Configuration().settings('settings.yml')
    try:
        options = Options(settings)
        Package(options, data)
    except ValueError:
        print('package value is missing in settings')

Where *settings.yml* could be:

    name: myApp
    package: C:/scripts/my_app.py
    distpath: C:/dist/myApp
    data: 
    - src: C:/scripts/resources/myapp.yml
        root_level: true
    - src: C:/scripts/resources/README.md
        root_level: true
    - src: C:\data\github\work\scripts\doc\easyPresentation\images
        dst: images
        root_level: true
    sh:
        path: mypath
        options:
            l: myloggerpath
            s: mysettingspath

See [pyinstaller documentation](https://pyinstaller.readthedocs.io/en/stable/) for more info

def _createsh(self, path, name, parameters):
        file = os.path.join(path, name+'.sh')
        target = '{0}/{1}/{1} '.format(parameters['path'], name)
        with open(file, 'w') as f:
            f.write(target)            
            for key, value in parameters['options'].items():
                f.write('-{} {} '.format(key, value))