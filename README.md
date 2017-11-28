# vscode-gradle-language
An extension to provide Gradle language support for Visual Studio Code, including advanced functionalities like __Syntax Highlighting__, __Keyword Auto-completion Proposals__ and __Duplication Validation__.

## Stucture
The extension observes all `.gradle` documents and uses the server to provide validation and auto-completion proposals (IntelliSense).

The code for the extension is in the 'client' folder, which uses the 'vscode-languageclient' node module to launch the language server.

The language server is located in the 'server' folder. 

## Features

### Syntax Highlighting

* The extension converts the [sublime-gradle](https://github.com/kingofmalkier/sublime-gradle)'s TextMate grammar configuration for Gradle language.

![feature A-1](images/feature-A-1.png)

### Keyword Auto-completion Proposals (IntelliSense)

The extension automatically detects the Java and Android plugin used in the Gradle scripts, then propose keywords smartly in different situations depending on the position of the cursor:
    
* Inside a line with existing code:

    * At the start of a word:
        
        * Particularly, after the `apply` method, propose the parameters for plugin application.
        
        ![feature B-1-1-1](images/feature-B-1-1-1.png)

        * In a Task Container's constructor:

            * At the start of a parameter, propose Task Container's constructor parameters.
            
            ![feature B-1-1-2-1](images/feature-B-1-1-2-1.png)

            * Particularly, after the `type` parameter, propose Task's default types.

            ![feature B-1-1-2-2](images/feature-B-1-1-2-2.png)

        * Elsewhere, propose the Delegate Object together with its properties and methods.
        
        ![feature B-1-1-3](images/feature-B-1-1-3.png)

    * After an entity's dot inside a line with existing code, propose the properties and methods of this entity.
    
    ![feature B-1-2](images/feature-B-1-2.png)


* At the start of a new line:

    * On the root level of the build script, propose the properties and methods of the current script's Delegate Object.
    
    ![feature B-2-1](images/feature-B-2-1.png)

    * Particularly, in a `TaskContainer`, analyze the task's type and propose the properties and methods according to this type.
    
    ![feature B-2-2](images/feature-B-2-2.png)

    * In a script block of `NamedDomainObjectContainer`, such as `BuildTypes`, `ProductFlavors`, `SigningConfigs` and `AndroidSourceSets`, __DO NOT__ override default keywords and let the user decide the name for each item.
    
    ![feature B-2-3](images/feature-B-2-3.png)

    * In a configure closure of an item within a `NamedDomainObjectContainer`, propose the properties and methods of this container's element type.
    
    ![feature B-2-4](images/feature-B-2-4.png)

    * In a script block of a general property or method, propose the properties and methods of this script block.

    ![feature B-2-5](images/feature-B-2-5.png)
    
### Duplication Validation

* The extension automatically validates the script under editing and warns the user about duplicated script blocks.

![feature C-1](images/feature-C-1.png)

## Requirements

* Download and install [Visual Studio Code](https://code.visualstudio.com/download).
* Download and install [Node.js](https://nodejs.org/zh-cn/download/).


## How to run locally
* Pull the repo and `cd` to the root directory.
* Run `npm install` to initialize the extension and the server
* Run `npm run compile` to compile the extension and the server
* Open the root folder in Visual Studio Code, then set the `Preferences -> Color Theme` to `Dark+ (default dark)`.
* In the Debug viewlet, run `Launch Client` from drop-down menu (or press `F5`) to launch the extension and attach to the extension.
* Create and open a file `build.gradle` and type `'app'` into it. You should see keywords proposed including `'apply'`.
* To debug the server, use the `Attach to Server` launch config and set breakpoints in the client or the server.


## Extension Settings

Include if your extension adds any VS Code settings through the `contributes.configuration` extension point.

This extension contributes the following settings:

* `myExtension.enable`: enable/disable this extension
* `myExtension.thing`: set to `blah` to do something

## Known Issues

* Syntax highlighting cannot mark task constructor with parameters in parentheses, i.e. `task foo(type: Bar) {...}` 

## Release Notes

### 1.0.0

Initial release.

