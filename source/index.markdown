---
layout: page
# title: "index"
date: 2015-04-02 15:34
comments: true
sharing: true
footer: true
---


Overview
=============
STP is a constraint solver that can solve many kinds of problems including those by program analysis tools, theorem provers, automated bug finders, cryptographic algorithms, intelligent fuzzers and model checkers.  STP is <a href="https://github.com/stp/stp">actively developed</a> and used by many, including large companies and research institutes.

<!--The input to STP are formulas over the theory of bit-vectors and arrays. This theory captures most expressions from languages like C,C++,Java, Verilog etc. STP can tell if the input formula is satisfiable or not and if is, then it can also generate a variable assignment to satisfy the input formula.-->

Install instructions
=============

For Debian-like platforms first install the prerequisites: `apt-get install cmake g++ zlib1g-dev libboost-all-dev flex bison
git clone https://github.com/stp/minisat.git`

Then install minisat:
```
cd minisat && mkdir build && cd build
cmake ..
make
sudo make install
```

Then install STP:
```
git clone https://github.com/stp/stp.git
mkdir build && cd build
cmake ..
make
sudo make install
```


For windows-like environments: you will need to first install the zlib library then install minisat and stp exactly like above, using cmake to create Visual Studio project files, e.g. `cmake .. -G "Visual Studio 10 Win64"` and then building with Visual Studio.

Languages parsed
=============
The file based input formats that STP reads are the: CVC, SMT-LIB1, and SMT-LIB2 formats. The SMT-LIB2 format is the recommended file format, because it is parsed by all modern bitvector solvers. STP implements a subset of the SMT-LIB2 language; not all SMT-LIB2 features are implemented.


Python usage
=============

```
import stp
s = stp.Solver()
a = s.bitvec('a', 32)
b = s.bitvec('b', 32)
c = s.bitvec('c', 32)
s.add(a == 5)
s.add(b == 6)
s.add(a + b == c)
s.check()
-> True
s.model()
-> {'a': 5L, 'b': 6L, 'c': 11L}
```

SMT-LIBv2 Usage
=============
Signed division of -1/-2 =  0, should be satisfiable.

```
(set-logic QF_BV)
(assert (= (bvsdiv (_ bv3 2) (_ bv2 2)) (_ bv0 2)))
(check-sat)
(exit)
```


C library usage
=============

When STP is built it generates a ``lib/libstp.a`` static library which can be used with one of two header files, depending on the preferred language:

- ``include/stp/c_interface.h`` for a C interface to STP
- ``include/stp/cpp_interface.h``for a C++ interface to STP

An example C header usage can be as simple as:

```
#include "stp/c_interface.h"
#include <assert.h>

int main(int argc, char **argv) {
  VC vc = vc_createValidityChecker();

  // 32-bit variable 'c'
  Expr c = vc_varExpr(vc, "c", vc_bvType(vc, 32));

  // 32 bit constant value 5
  Expr a = vc_bvConstExprFromInt(vc, 32, 5);

  // 32 bit constant value 6
  Expr b = vc_bvConstExprFromInt(vc, 32, 6);

  // a+b!=c
  Expr xp1 = vc_bvPlusExpr(vc, 32, a, b);
  Expr eq = vc_eqExpr(vc, xp1, c);
  Expr eq2 = vc_notExpr(vc, eq);

  //Is a+b!=c always correct?
  int ret = vc_query(vc, eq2);

  //No, c=a+b is a counterexample
  assert(ret == false);

  //print c = 11 counterexample
  vc_printCounterExample(vc);

  //Delete validity checker
  vc_Destroy(vc);

  return 0;
}
```

If you use CMake as the build system for your project it is easy to use STP as an external project. An example can be found in the sources under [``examples/simple``](https://github.com/stp/stp/tree/master/examples/simple).

Awards
=============
* STP placed <a href="http://www.msoos.org/2014/06/smt-competition14-and-stp/">2nd in the bitvector category</a> in the SMTCOMP 2014, just after the proprietary <a href="http://fmv.jku.at/boolector/">Boolector</a> system
* STP placed 2nd in the bitvector category in the SMTCOMP 2011
* STP won the bit-vector category at <a href="http://www.smtcomp.org/2010/">SMTCOMP 2010</a>
* STP won the <a href="https://www.cs.upc.edu/~oliveras/espai/papers/JAR-smtcomp.pdf">SMTCOMP 2006</a> competition (Bit-vector category) in 2006

Use cases
=============


* <a href="http://klee.github.io/">KLEE symbolic fuzzer</a> is using STP at its core (Professor Cristian Cadar's group at Imperial College, London, and Professor Dawson Engler's group at Stanford University)
* <a href="https://github.com/google/souper">Souper project</a> at the University of Utah and Google
* <a href="http://s2e.epfl.ch/">S2E</a> at EPFL
* Mayhem fuzzer, which <a href="http://lwn.net/Articles/557055/">found over 1000 bugs</a> in mainline Debian is using KLEE and hence STP
* <a href="http://bap.ece.cmu.edu/">Binary Analysis Platform (BAP)</a> is using STP for analysis, by the CMU
* <a href="http://people.csail.mit.edu/vganesh/STP_files/exe.pdf">EXE</a> is a symbolic-execution based bug-finding tool that reads your C program and tries to automatically crash it (Stanford University)
* <a href="http://users.ece.cmu.edu/~dawnsong/">MINESWEEPER</a>  is a tool that automatically analyzes certain malicious behavior in unix utilities and malware.  (Carnegie Mellon University)
* <a href="http://sourceforge.net/projects/catchconv/">CATCHCONV</a> is a bug finding tool that tries to find bugs due to type mismatch in C programs. (University of California, Berkeley)
* Backward path-sensitive analysis of C programs to find bugs by Tim Leek from MIT Lincoln Labs
* Bug finding in Verilog code (a major microprocessor company)
* <a href="http://ase.arc.nasa.gov/people/pcorina/papers/jpfseTACAS07.pdf">JPF-SE</a> is a symbolic execution extension to the Java PathFinder model checker . (NASA Ames Research Center)
* <a href="http://code.google.com/p/avalanche/">Avalanche</a> bug-finding tool (Institute of Systems Programming, Moscow, Russia)
* <a href="http://llbmc.org/">Low-level Bounded Model Checker - LLBMC</a> (Karlsruhe Institute of Technology (KIT), Germany)
* <a href="http://esec-lab.sogeti.com/pages/Fuzzgrind">FuzzGrind</a>  (ESEC Lab)
* In conjunction with <a href="http://www.cs.utexas.edu/users/moore/acl2/">ACL2</a> to formally verify implementation of encryption algorithms in Java (Stanford University)
* <a href="http://people.csail.mit.edu/akiezun/hampi/">Hampi</a> : A solver for string constraints used to automatically construct SQL injection and XSS exploits (MIT)
* Automatic configuration: Tvl2STP (University of Namur in Belgium)


Architecture
=============

STP is an efficient decision procedure for the validity (or satisfiability) of formulas from a quantifier-free many-sorted theory of fixed-width bitvectors and (non-extensional) one-dimensional arrays. The functions in STP's input language include concatenation, extraction, left/right shift, sign-extension, unary minus, addition, multiplication, (signed) modulo/division, bitwise Boolean operations, if-then-else terms, and array reads and writes. The predicates in the language include equality and (signed) comparators between bitvector terms.

The basic architecture of STP essentially follows the idea of word-level preprocessing followed by translation to SAT (We use MINISAT and CRYPTOMINISAT). In particular, we introduce several new heuristics for the preprocessing step, including abstraction-refinement in the context of arrays, a new bitvector linear arithmetic equation solver, and some interesting simplifications. These heuristics help us achieve several magnitudes of order performance over other tools, and also over straight-forward translation to SAT. STP has been heavily tested on thousands of examples sourced from various real-world applications such as program analysis and bug-finding tools like EXE, and equivalence checking tools and theorem-provers.

History and authors
=============

The initial versions of STP were written primarily by Vijay Ganesh as part of his PhD thesis, and the project was later maintained by Trevor Hansen. The current primary maintainers are Mate Soos, Dan Liew, and Ryan Govostes who have improved it in many ways. STP is based on the following papers:

* <a href="https://ece.uwaterloo.ca/~vganesh/Publications_files/vg2007-STP-CAV.pdf">A Decision Procedure for Bit-vectors and Arrays</a> by Vijay Ganesh and David L. Dill. In Proceedings of the International Conference in Computer Aided Verification (CAV 2007), Berlin, Germany, July 2007
* <a href="https://ece.uwaterloo.ca/~vganesh/Publications_files/vg2006-EXE-CCS.pdf">EXE: Automatically Generating Inputs of Death</a> by Cristian Cadar, Vijay Ganesh, Peter Pawlowski, Dawson Engler, David Dill. In Proceedings of ACM Conference on Computer and Communications Security 2006 (CCS 2006), Alexandria, Virginia, October, 2006

Past contributors: Khoo Yit Phang, Ed Schwartz, Mike Katelman (PhD Student, University of Illinois, Urbana-Champaign, IL, USA), Philip Guo (Student, Stanford University, Stanford, CA, USA), David L. Dill (Professor, Stanford University, Stanford, CA, USA), Tim King (Student, Stanford University and NYU). Please note that everyone working on the project is doing so out of hobby or as a way to help them in their work/study projects.

