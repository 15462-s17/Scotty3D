---
layout: page
title: simple site
tagline: Easy websites with GitHub Pages
---
![Scotty3DLogo](http://15462.courses.cs.cmu.edu/fall2016content/Scotty3D/images/Scotty3DLogo.svg)

### Overview 

Welcome to your first day of work at Scotty Industries!  Over the next few months you will implement core features in Scotty Industries' flagship product _Scotty3D_, which is a modern package for 3D modeling, rendering, and animation.  In terms of basic structure, this package doesn't look much different from "real" 3D tools like [Maya](http://www.autodesk.com/products/maya/overview-dts?s_tnt=69290:1:0), [Blender](https://www.blender.org), [modo](https://www.thefoundry.co.uk/products/modo/), or [Houdini](https://www.sidefx.com).  Your overarching goal is to use the _Developer Manual_ to implement a package that works as described in the _User Guide_, much as you would at a real software company (more details below).

Your current assignment focuses on the MeshEdit component of Scotty3D, which performs 3D modeling, subdivision, and mesh processing.  When you are done, you will have a tool that allows you to transform a simple cube model into beautiful, organic 3D surfaces described by high-quality polygon meshes.  This tool can import and export industry-standard COLLADA files, allowing Scotty3D to interact with the broader ecosystem of computer graphics software.

### Important Information

**Due Date:** Oct 13th, 2016 11:59pm

You will need three things to complete this assignment:

1. The [skeleton code for Scotty3D](https://github.com/15462-f16/Scotty3D)---your implementation will modify the files `meshEdit.cpp` and `halfEdgeMesh.cpp`.

2. The [User Guide](http://15462.courses.cs.cmu.edu/fall2016/article/14) explains how to _use_ the software to an end-user.

3. The [Developer Manual](http://15462.courses.cs.cmu.edu/fall2016/article/15) explains low-level details and internals of the code to a software developer.

Note that the User Guide is **not** the Assignment Writeup. The User Guide contains only instructions on how to use the software, and serves as a high-level specification of _what the software should do_. The Developer Guide contains information about the internals of the code, i.e., _how the software works_.  This division is quite common in software development: there is a __design specification__ or "design spec", and an __implementation__ that implements that spec.  Also, as in the real world, the design spec does _not_ necessarily specify every tiny detail of how the software should behave!  Some behaviors may be undefined, and some of these details are left up to the party who implements the specification.  A good example you have already seen is OpenGL, which defines some important rules about how rasterization should behave, but is not a "pixel-exact" specification.  In other words, two different OpenGL implementations from two different vendors (Intel and NVIDIA, say) may produce images that differ by a number of pixels.  Likewise, in this assignment, your implementation may differ from the implementation of your classmates in terms of the exact values it produces, or the particular collection of corner-cases it handles.  However, as a developer you should strive to provide a solution that meets a few fundamental criteria:

* [Failing gracefully](https://en.wikipedia.org/wiki/Fault_tolerance) is preferable to failing utterly---for instance, if a rare corner case is difficult to handle, it is far better to simply refuse to perform the operation than to let the program crash!
* Your implementation should follow the [principle of least surprise](https://en.wikipedia.org/wiki/Principle_of_least_astonishment).  A user should be able to expect that things behave more or less as they are described in the User Guide.
* You should not use an algorithm whose performance is [asymptotically worse](https://en.wikipedia.org/wiki/Asymptotic_computational_complexity) just because it makes your code easier to write (for instance, using [bubble sort](https://en.wikipedia.org/wiki/Bubble_sort) rather than [merge sort](https://en.wikipedia.org/wiki/Merge_sort) on large data sets).
* That being said, when it comes to performance, [premature optimization is the root of all evil!](https://en.wikipedia.org/wiki/Program_optimization#When_to_optimize)  The only way to know whether an optimization matters is to [measure performance](https://en.wikipedia.org/wiki/Profiling_(computer_programming)), and understand [bottlenecks](https://en.wikipedia.org/wiki/Program_optimization#Bottlenecks).
* Finally, you should take [pride in your craft](http://thenextweb.com/apple/2011/10/24/steve-jobs-obsession-with-the-quality-of-the-things-unseen/#gref).  Beautiful things just tend to work better.

Just to reiterate the main point above:

**As in real-world software development, we will not specify every little detail about how methods in this assignment should work!**

If you encounter a tough corner case (e.g., "how should edge flip behave for a tetrahedron"), we want you to _think about what a good **design choice** might be_, and implement it to the best of your ability.  This activity is part of becoming a world-class developer.  However, we are more than happy to discuss good design choices with you, and you should also feel free to discuss these choices with your classmates.  Practically speaking, it is ok for routines to simply exit if they encounter a rare and difficult corner case---as long as it does not interfere with successful operation of the program (i.e., if it does not crash or yield bizarre behavior).  Your main goal here above all else should be to develop _effective tool for modeling, rendering, and animation_.

### Getting started

The basic code skeleton will be distrbuted via [Git](https://en.wikipedia.org/wiki/Git).  This skeleton includes basic functionality (e.g., file loading/saving, a graphical user interface, etc.), but the core software features have been removed.  You can find the repository for this assignment at <https://github.com/15462-f16/Scotty3D>. If you are unfamiliar with git, here is what you need to do to get the starter code:

```
$ git clone https://github.com/15462-f16/Scotty3D.git
```

This will create a <i class="icon-folder"> </i> *Scotty3D* folder with all the source files. 

### Build Instructions
In order to ease the process of running on different platforms, we will be using [CMake](http://www.cmake.org "CMake Homepage") for our assignments. You will need a CMake installation of version 2.8+ to build the code for this assignment. The GHC 5xxx cluster machines have all the packages required to build the project. It should also be relatively easy to build the assignment and work locally on your OSX or Linux. Building on Windows is in beta support. 

If you are working on OS X and do not have CMake installed, we recommend installing it through [Macports](https://www.macports.org/):

```
sudo port install cmake
```

Or [Homebrew](http://brew.sh/):

```
brew install cmake
```

To build your code for this assignment:

Create a directory to build your code:

```
$ cd Scotty3D && mkdir build && cd build
``` 

Run CMake to generate makefile:

```
$ cmake ..
```

Build your code:

```
$ make
```

Install the executable:

```
$ make install
```

#### Windows Build Instructions
We have a beta build support for Windows systems. You need to install the latest version of [CMake](http://www.cmake.org "CMake Homepage") and install Visual Studio 2015 from [CMU Dreamspark web store](https://www.cmu.edu/computing/software/all/dreamspark/). After installing these programs, you can run `runcmake_win.bat` by double-clicking on it. This should create a `build` directory with a Visual Studio solution file in it named `Scotty3D.sln`. You can double-click this file to open the solution in Visual Studio.

If you plan on using Visual Studio to debug your program, you can change `scotty3D` project in the Solution Explorer as the startup project by right-clicking on it and selecting `Set as StartUp Project`. You can also set the commandline arguments to the project by right-clicking `scotty3D` project again, selecting `Properties`, going into the `Debugging` tab, and setting the value in `Command Arguments`. If you want to run the program with the default cube file, you can set this command argument to `../../cube.dae`. After setting all these, you can hit F5 to build your program and run it with the debugger.

If you feel that your program is running slowly, you can also change the build mode to `Release` from `Debug` by clicking the Solution Configurations drop down menu on the top menu bar. Note that you will have to set `Command Arguments` again if you change the build mode.

### Using the Scotty3D App

When you have successfully built your code, you will get an executable named **scotty3d**. The **scotty3d** executable takes exactly one argument from the command line. You may load a single COLLADA file by specifying its path.  Initially we have provided only a simple cube mesh <i class="icon-file"> </i>*cube.dae*, but as you develop the software you will be able to create (and save) more and more interesting surfaces.  You can load the cube from the build directory by running


```
./scotty3d ../cube.dae
```

See more in the User Guide on [Github, Scotty3d UserGuide](https://github.com/15462-f16/Scotty3D/blob/master/Scotty3D_UserGuide.md).

Note that since most of the features of Scotty3D have not yet been implemented, you may encounter occasional unexpected behavior (even crashes!).  Your goal is to replace these blank routines with stable implementations.

### Handin Instructions


Your handin directory is on AFS under:

```
/afs/cs/academic/class/15462-f16-users/ANDREWID/asst2/
```

Note that you may need to create the `asst2` directory yourself. All your files should be placed there. Please make sure you have a directory and are able to write to it well before the deadline; we are not responsible if you wait until 10 minutes before the deadline and run into trouble. Also, you may need to run `aklog cs.cmu.edu` after you login in order to read from/write to your submission directory.

You should submit all files needed to build your project, this includes:

* The `src` folder with all your source files

Please do not include:

* The `build` folder
* Executables
* Any additional binary or intermediate files generated in the build process.

You should include a `README` file (plaintext, MarkDown, or PDF) indicating which features you chose to implement, including any features implemented for extra credit.  You should also briefly state any other issues the grader should be aware of.

Do not add levels of indirection when submitting. And please use the same arrangement as the handout. We will enter your handin directory, and run:

```
mkdir build && cd build && cmake .. && make
```

and your code should build correctly. The code must compile and run on the GHC 5xxx cluster machines. Be sure to check to make sure you submit all files and that your code builds correctly.

### Evaluation

For this assignment, you will implement methods in `meshEdit.cpp`.

The User Guide describes a large number of features that are in principle available in MeshEdit mode.  **You do not have to implement all of these features to receive full credit on the assignment!**  However, you do have to succesfully implement a _subset_ of the features.  If you choose to work in a pair, the two of you must implement more features than someone who is working alone.  (However, you will likely learn more---and more quickly discover errors, etc.---by teaming up!) Implementing additional features beyond the required subset will earn you extra credit points.

The particular requirements, and the percentage of the grade they correspond to, are:

* If you are working **alone**, you have to implement
   * **Four** of the local operations (listed below), including `FaceBevel` (10 pts each)
   * `Triangulation`, `LinearSubdivision`, and `CatmullClarkSubdivision` (10 pts each)
   * **One** of: `LoopSubdivision`, `IsotropicRemeshing`, or `Simplification` (30 pts each)
   * Create **one** beautiful 3D model using Scotty3D (20 pts)
   * (Total score percentage is number of points out of 120)

* If you are working **in a pair**, you have to implement
   * **Eight** of the local operations (listed below), including `FaceBevel` (10 pts each)
   * `Triangulation`, `LinearSubdivision`, and `CatmullClarkSubdivision` (10 pts each)
   * **Two** of: `LoopSubdivision`, `IsotropicRemeshing`, or `Simplification` (30 pts each)
   * Create **two** beautiful 3D models using Scotty 3D (15 pts each)
   * (Total score percentage is number of points out of 200)

In other words, _everyone_ has to implement FaceBevel, triangulation, linear subdivision, and Catmull-Clark.  These features are the bare minimum needed to model interesting subdivision surfaces; triangulation is necessary in order to do the global remeshing task(s).  *Note that some of the global tasks will require that you implement specific local operations!*  For instance, if you plan on implementing Simplification, you should also choose to implement EdgeCollapse.  The local operations are as follows (these operations are described in the User Guide):

* `VertexBevel`
* `EdgeBevel`
* `FaceBevel` - everyone must implement
* `EraseVertex`
* `EraseEdge`
* `EdgeCollapse`
* `FaceCollapse`
* `EdgeFlip`
* `EdgeSplit`

The global operations, and their dependency on local operations, are as follows:

* `Triangulation` - everyone must implement
* `LinearSubdivision` - everyone must implement
* `CatmullClarkSubdivision` - everyone must implement
* `LoopSubdivision` - depends on `EdgeSplit` and `EdgeFlip`
* `IsotropicRemeshing` - depends on `EdgeSplit`, `EdgeFlip`, and `EdgeCollapse`
* `Simplification` - depends on `EdgeCollapse`

You are free to change the subset of features you choose to implement at any point during the assignment, but you should __clearly indicate which features you chose__ (including those implemented for extra credit) by putting this information in your README file.

**Extra credit:** each additional local operation beyond the requirements will be worth 2% of the final grade; each additional global operation will be worth 4% of the final grade.

### America's Next Top 3D Model

Every student is required to submit a 3D model created using their implementation of Scotty3D, which will be automatically entered into a class-wide 3D modeling competition.  Models will be critiqued and evaluated based on both technical sophistication and aesthetic beauty.  The top $n$ models (for $n \geq 1$) will be printed in 3D, showcased to the class, and given to the winners.  **Note:** Use of any other 3D package (e.g., free or commercial 3D modelers like _Maya_ or _Blender_) is _strictly_ prohibited!
