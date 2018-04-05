<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. Emacs as Java IDE</a>
<ul>
<li><a href="#sec-1-1">1.1. Outline</a></li>
<li><a href="#sec-1-2">1.2. Basic Environment</a>
<ul>
<li><a href="#sec-1-2-1">1.2.1. About Eclim</a></li>
<li><a href="#sec-1-2-2">1.2.2. Install</a></li>
<li><a href="#sec-1-2-3">1.2.3. Running the server</a></li>
<li><a href="#sec-1-2-4">1.2.4. Projects</a></li>
</ul>
</li>
<li><a href="#sec-1-3">1.3. Compiling</a>
<ul>
<li><a href="#sec-1-3-1">1.3.1. Setting up Gradle</a></li>
<li><a href="#sec-1-3-2">1.3.2. Using Gradle</a></li>
<li><a href="#sec-1-3-3">1.3.3. Example</a></li>
</ul>
</li>
<li><a href="#sec-1-4">1.4. Autocomplete</a></li>
<li><a href="#sec-1-5">1.5. Syntax-checking</a></li>
<li><a href="#sec-1-6">1.6. Refactoring</a></li>
</ul>
</li>
</ul>
</div>
</div>

# Emacs as Java IDE<a id="sec-1" name="sec-1"></a>

referenced from [Setting up Emacs for Java Development](http://www.goldsborough.me/emacs,/java/2016/02/24/22-54-16-setting_up_emacs_for_java_development/).

## Outline<a id="sec-1-1" name="sec-1-1"></a>

1.  Basic Environment: how to install and use *eclime*
2.  Compiling: using Gradle to compile and run your code.
3.  Autocomplete: configure sweek completion features with *company*.
4.  Syntax-checking: Knowing what you're messing up at write-time.
5.  Refactoring: Lightweight features that make life more pleasant.

## Basic Environment<a id="sec-1-2" name="sec-1-2"></a>

### About [Eclim](http://eclim.org)<a id="sec-1-2-1" name="sec-1-2-1"></a>

### Install<a id="sec-1-2-2" name="sec-1-2-2"></a>

1.  Eclipse

        brew install caskroom/cask/brew-cask 2> /dev/null
        
        # install 
        brew install caskroom/cask/eclipse-java
        brew install caskroom/cask/eclipse-ide
        
        # for uninstall
        brew cask zap eclipse-java
        brew cask zap eclipse-ide
    
    Or, install eclipse using GUI installer.

2.  eclim

    1.  Download [eclim<sub>xx</sub>.bin](https://github.com/ervandew/eclim/releases/download/2.7.2/eclim_2.7.2.bin)
    2.  execute the installer:
        
            cd ~/Downloads
            chmod +x eclim_2.7.2.bin
            ./eclim_2.7.2.bin
    
    Log:
    
    ---
    
    The eclim install completed successfully.
    You can now start the eclimd server by executing the script:
      /Applications/Eclipse.app/Contents/Eclipse/eclimd
    
    The most important thing  is know where you are putting the `eclim` and `eclimd` executables.
    
    `eclim` is what you could use from the command-line to interface with Eclipse. It sends all the commands to the server and handles the communication. The emacs mode introduced below wraps `eclim`.
    
    `eclimd` is the eclim `daemon`, which starts up and manages the server.
    
    At, last: start that daemon:
    
        /Applications/Eclipse.app/Contents/Eclipse/eclimd

3.  emacs-eclim

        (use-package eclim
          :ensure t
          :config
          (add-hook 'java-mode-hook 'eclim-mode))

### Running the server<a id="sec-1-2-3" name="sec-1-2-3"></a>

Setup Eclipse server using eclimd
`M-x start-eclimd`
It asks to set the workplace's directory: such as `\~/workspace`.

If get error, check the variable settings by:
`M-x customize-variable RET eclimd-executable`

### Projects<a id="sec-1-2-4" name="sec-1-2-4"></a>

Create project directory by:
`M-x eclim-project-create`

Open a project by:
`M-x eclim-project-open`

At this point you should be fully setup with emacs-eclim. Such as, use
`M-x eclim-java-refactor-rename-symbol-at-point` to rename a symbol in your java code.

## Compiling<a id="sec-1-3" name="sec-1-3"></a>

The *simplest* way to compile and run Java code is with `M-x compile`, where
you can type in your standard `javac Foo.java Bar.java` following by another `M-x compile` with `java Fool`. 

But that is not intuitive. So use Gradle.

### Setting up Gradle<a id="sec-1-3-1" name="sec-1-3-1"></a>

-   install Gradle

    brew install gradle
    brew upgrade gradle

-   install gradle-mode from MELPA

    (use-package gradle-mode
      :ensure t
      :config
      (progn
        (gradle-mode 1)
        (add-hook 'java-mode-hook '(lambda () (gradle-mode 1)))))

    apply plugin: 'java'
    apply plugin: 'application'
    
    mainClassName = "Test"
    
    applicationDefaultJvmArgs = ["-ea"]

1.  setting up the build file for Java by applying the java plugin
2.  The application plugin we will use to run our code, as opposed to just building it or creating a jar (which is probably the easiest thing to do with Gradle).
3.  To run our code using the application plugin, we need to set a main class (we’ll define a Test.java file later).
4.  Lastly I enabled assertions, just as an example.

### Using Gradle<a id="sec-1-3-2" name="sec-1-3-2"></a>

Two most important commands from gradle-mode are `gradle-build` and `gradle-execute`.

The gradle expects a standard directory structure under project directory:
`src/main/java` contains production code
`src/test/java` contains test source code
`src/main/resources` contains resources

### Example<a id="sec-1-3-3" name="sec-1-3-3"></a>

project folder is `/Users/zw/workspace/emacs-java`:

    zwpdbhs-MBP:emacs-java zw$ tree .
    .
    ├── bin
    ├── build.gradle
    ├── build.gradle~
    └── src
        └── main
            └── java
                └── Test.java

Code in Test.java:

    public class Test {
        public static void main(String... args) {
            for (int i = 0; i < 10; ++i) {
                System.out.println(i);
            }
        }
    }

`M-x gradle-build` build the project when the current buffer is the opened .java

`gradle-execute`, will prompt to ask what tasks you want to execut, type:
`build run`, which build and run the program.

Make life easier by build `M-x gradle-execute build run` to a key-combination in our .emacs file:

    (defun build-and-run ()
      (interactive)
      (gradle-run "build run"))
    
    (define-key gradle-mode-map (kbd "C-c C-r") 'build-and-run)

## Autocomplete<a id="sec-1-4" name="sec-1-4"></a>

Choose autocomplete or company

## Syntax-checking<a id="sec-1-5" name="sec-1-5"></a>

This feature actually requires no extra modes, but is entirely integrated into eclim. When you go over an error with your cursor, you can then use `M-x eclim-problems-correct` to select some possible corrections:

## Refactoring<a id="sec-1-6" name="sec-1-6"></a>

1.  Renaming symbols. When over a symbol, hit `M-x eclim-java-refactor-rename-symbol-at-point` (probably going to want to bind that to something).
2.  Moving classes between files. Use `M-x eclim-java-refactor-move-class` for this.
