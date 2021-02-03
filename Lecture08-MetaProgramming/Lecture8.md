1. ### Build Systems ###
   1. A project "build process" with sequence of operations for inputs -> outputs; steps and branches, generate plot of results using tools.
   2. The "build systems" (many options) depends on task, preferred language, size of project but they all depends on 
      1. number of dependencies
      2. number of targets
      3. rules for going from one to another
   ie rules to produce intermediate targets, transitive dependencies of the target to eventual final target
   3. ``` make```  is a common build systems for simple-to-moderate project. It consults a ```Makefile``` in the current directory which contains all the targets, their dependencies and rules. 
   4. RHS -> LHS. RHS = dependencies and LHS = targets. The indented block is a sequence of programs to produce the target from those dependencies. ```%``` in a rule is a “pattern”, and will match the same string on the left and on the right. For example, if the target ```plot-foo.png``` is requested, make will look for the dependencies ```foo.dat``` and ```plot.py```.
    ```
    paper.pdf: paper.tex plot-data.png
	        pdflatex paper.tex

    plot-%.png: %.dat plot.py
	        ./plot.py -i $*.dat -o $@
    ```
    5. Create files
        ```
        $ cat paper.tex
        \documentclass{article}
        \usepackage{graphicx}
        \begin{document}
        \includegraphics[scale=0.65]{plot-data.png}
        \end{document}

        $ cat plot.py
        #!/usr/bin/env python
        import matplotlib
        import matplotlib.pyplot as plt
        import numpy as np
        import argparse

        parser = argparse.ArgumentParser()
        parser.add_argument('-i', type=argparse.FileType('r'))
        parser.add_argument('-o')
        args = parser.parse_args()

        data = np.loadtxt(args.i)
        plt.plot(data[:, 0], data[:, 1])
        plt.savefig(args.o)
        $ cat data.dat
        1 1
        2 2
        3 3
        4 4
        5 8
        ```
    6.  run ```make``` once. A second run ```make``` is not needed as it checked that all previously-built target is up to date.
        ```
        $ make
        ./plot.py -i data.dat -o plot-data.png
        pdflatex paper.tex
        ... lots of output ...
        ```
        ```
        $ make
        make: 'paper.pdf' is up to date.
        ```
2.  ### Dependency Management ###        
    1. Some projects have dependencies that are projects themselves although these days repository centralise all of these dependencies so that we can install conveniently. e.g Ubuntu package can be installed easily with ```apt```  
    2. Versioning xx.xx.xxxx tells users which library/package version they are using. Every function update / rename justify a new version. 
    3. Semantic Versioning. With semantic versioning, every version number is of the form: **major.minor.patch**. The rules are:
        * If a new release does not change the API, increase the patch version.
        * If you add to your API in a backwards-compatible way, increase the minor version.
        * If you change the API in a non-backwards-compatible way, increase the major version.
    4. So if my project depends on your project, , it should be safe to use the latest release with the same major version as the one I built against when I developed it, as long as its minor version is at least what it was back then. In other words, if I depend on your library at version 1.3.7, then it should be fine to build it with 1.3.8, 1.6.1, or even 1.3.0. Version 2.2.4 would probably not be okay.  Python 2 and Python 3 code do not mix very well, which is why that was a major version bump. Similarly, code written for Python 3.5 might run fine on Python 3.7, but possibly not on 3.4.
    5.  A lock file is simply a file that lists the exact version you are currently depending on of each dependency. Usually, you need to explicitly run an update program to upgrade to newer versions of your dependencies. 
    6.  Vendoring, where you copy all the code of your dependencies into your own project, gives you total control over any changes to it, and lets you introduce your own changes but have to explicitly pull in any updates from the upstream maintainers over time.
3. ### Continuous integration systems ###
   1. Project development includes upload a new version of the documentation, upload a compiled version somewhere, release the code to pypi, run your test suite etc.
   2. Continuous integration, or CI, is an umbrella term for “stuff that runs whenever your code changes”. Free ones are Travis CI, Azure Pipelines, and GitHub Actions. They perform action according to a file in your repository that tells them the expected sequence of events. MOst common is “when someone pushes code, run the test suite”, produces the results, maybe with additional trigger when test suite stops passing.
4. ### Terminology ###
   * Test suite: a collective term for all the tests
   * Unit test: a “micro-test” that tests a specific feature in isolation
   * Integration test: a “macro-test” that runs a larger part of the system to check that different feature or components work together.
   * Regression test: a test that implements a particular pattern that previously caused a bug to ensure that the bug does not resurface.
   * Mocking: to replace a function, module, or type with a fake implementation to avoid testing unrelated functionality. For example, you might “mock the network” or “mock the disk”.