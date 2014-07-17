TLC Library
===========

Author: Arthur Chargu�raud

********************************************************************************
NOTE: This is a modified version of Arthur's library that is compatible with
      SSReflect syntax. Mainly, \notin, \in, and & were changed to \notinLN,
      \inLN, and +&+.
      This was done by running the following commands on Arthurs code:
        find ./ -name "*.v" -type f -exec sed -i -e 's/\\notin/\\notinLN/g' {} \;
        find ./ -name "*.v" -type f -exec sed -i -e 's/\\in/\\inLN/g' {} \;
        sed -i.bak 's/\([^&]\)&\([^&]\)/\1\+\&\+\2/g' LibLN.v LibEnv.v
        rm LibLN.v.bak LibEnv.v.bak

********************************************************************************
How to include TLC files in your project

You can obtain the files from the svn.

To get a stable version (e.g. v3.1), run:
   svn checkout svn://scm.gforge.inria.fr/svn/tlc/branches/v3.1 tlc

To get the trunk version, run:
   svn checkout svn://scm.gforge.inria.fr/svn/tlc/trunk tlc

If your project is already managed by SVN, you may use the very
convenient "svn-external" feature. Run the following command
in the folder from your project that should contain tlc as a subfolder, 
after replacing "202" by the head revision number of tlc.

   svn propset svn:externals 'tlc -r 202 svn://scm.gforge.inria.fr/svn/tlc/branches/v3.1' .
   cd tlc
   svn up

To get the head version number, use
  "svn info svn://scm.gforge.inria.fr/svn/tlc/branches/v3"

Note: forcing the revision number ensures allows you to control when
updates on TLC should be reflected in your project.


********************************************************************************
How to set up the Makefile for TLC

If you do not plan on modifying TLC files, simply use the Makefile
distributed with TLC. In particular, you can call "make -C tlc" from
your own makefile in order to compile TLC.

However, if you want to modify TLC or to be able to compile only
the files from TLC that you actually need, then you need to extend
your own Makefile. In particular, you will need:

*) A rule to compile coq files:

.v.vo : .depend
	$(COQC) -I $(TLC) $<

*) A listing of the source files from TLC

TLC_SRC=$(wildcard $(TLC)/*.v)

*) A computation of the dependencies; extend the rules with:

DEPS=$(TLC_SRC)
.depend : $(DEPS) 
	$(COQDEP) $(DEPS) > .depend

*) a additional command for make clean:

clean:
   bash -c "rm -f $(TLC)/*.{vo,deps,dot,glob}" 


********************************************************************************
How to import TLC library in your Coq files


To import the tactics, use:

  Require Import LibTactics.

To import all the most important files from TLC, use:

  Require Import LibCore.     

You may also import files individually.


********************************************************************************
Content

- Lib*.v      => the files from the library
- Lib*Demos.v => some demos showing how tactics work
- Makefile    => used to compile the project
- LICENSE.txt => explains that the files are covered by LGPL
- README.txt  => the current file


********************************************************************************
Compilation of TLC

For faster compilation on multicore processors, 
use "make -j 2" instead of "make" (change 2 by the
number of cores available)

- make         => compiles everything
- make lib     => compiles everything but the demos
- make demo    => compiles only the demos
- make tactics => compiles only LibTactics and its demos
 

********************************************************************************
Running the demos

Run the following commands in the TLC folder.

- make tactics
- coqide LibDemoTactics.v &


********************************************************************************
Auxiliary tools

*) The location of the Coq binaries to use can be specified in the 
   file "settings.sh", for example by writing:

COQBIN=/var/tmp/charguer/v8.4/bin/

*) To open a file in Coqide using the specified binaries for Coq:

./open.sh myfile.v  









