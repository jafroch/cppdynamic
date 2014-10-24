# cppdynamic

The [biicode block](http://www.biicode.com/jafroch/jafroch/cppdynamic/master) has been automatically published from the [forked github repo](https://github.com/toeb/cppdynamic) .It includes slight modifications added to the original repository: [toeb/cppdynamic](https://github.com/toeb/cppdynamic) in order to work properly with biicode.

To use it in biicode include:

      #include "jafroch/jafroch/cppdynamic/include/any.h"

Check out an example on how to use [cppdynamic](http://www.biicode.com/examples/examples/). Or a more detailed guide on [biicode docs] (http://docs.biicode.com/c++/examples/)


# cppdynamic.


Dynamic object are useful, especially in rapid prototyping, non-performance critical situations, and situations in which data/functions -bags are needed (non schema specific data).
Also when serializing and deserializing dynamic objects can be a very valuable asset.

Note that at the moment it only works with gcc and using c++11 or c++14.

Dynamic programming languages inherently support this. e.g.

    var obj = {};
    obj.a = 'asd';
    obj.b = {};
    obj.b.c = 3;
    obj.d = function(i,j){ return i+j;}
    

C# Also supports dynamic objects utilizing the `dynamic` keyword:

    dynamic obj = new ExpandoObject();
    obj.a = "asd";
    obj.b = new ExpandObject();
    obj.b.c = 3;
    obj.d = (int i, int j)=>i+j;
    

I tried as to stay close to c#'s dynamic objects when trying to implement them for c++. Of course I could not alter the language so I have to use the indexer operator `operator[](const std::string &)`  


My syntax is as follows:

    DynamicObject obj;
    obj["a"] = "asd";
    obj["b"] = DynamicObject();
    obj["b"]["c"] = 3;
    obj["d"] = [](int i, int j){return i+j;};
    

Here is a working example of what you can do (it works only with gcc and c++11):

    #include <core.dynamic.h>
    #include <assert.h>
    
    int main(){
        {
          // dynamic object can be a fundamental type
          dynamic::DynamicObject uut = 33;
          assert(33 == (int)uut);
    
        }
      {
        // dynamic object can be a complex type
        dynamic::DynamicObject uut = std::string("hello");
        assert("hello" == (std::string)uut);
      }
      {
        // dynamic object can be reassigned
        dynamic::DynamicObject uut = 33;
        assert(33 == (int)uut);
        uut = std::string("hello");
        assert("hello" == (std::string)uut);
      }
      {
        //dynamic object can be a functor
        dynamic::DynamicObject uut = [](int i, int j){return i + j; };
        int result = uut(3, 4);
        assert(result == 7);
      }
    
      {
        // dynamic object can be an expando object
        dynamic::DynamicObject uut;
        uut["prop1"] = 33;
        uut["prop2"] = std::string("hello");
        uut["prop3"]["prop31"] = 5;
        uut["prop4"] = [](int i, int j){return i + j; };
        int prop1 = uut["prop1"];
        std::string prop2 = uut["prop2"];
        int prop31 = uut["prop3"]["prop31"];
        int result = uut["prop4"](5, 6);
        std::function<int(int, int)> prop4 = uut["prop4"];
        auto result2 = prop4(3, 4);
    
    
        assert(prop1 == 33);
        assert(prop2 == "hello");
        assert(prop31 == 5);
        assert(result == 11);
        assert(result2 == 7);
      }
    
      {
        // you can create a custom dynamic object implementation:
        class MyImp : public dynamic::DynamicObjectImplementationBase{
        protected:
          virtual bool tryMemberGet(const get_member_context & context, result_type & result)override
          {
            if (context.name == "prop1"){
              result = 44;
              return true;
            }
            if (context.name == "prop2"){
              result = std::string("asd");
              return true;
            }
    
            return false;
          }
        };
        // initialization is still a bit complex 
        dynamic::DynamicObject uut = 
          std::dynamic_pointer_cast<dynamic::IDynamicObjectImplementation>(std::make_shared<MyImp>());
        int a = uut["prop1"];
        std::string b = uut["prop2"];
    
        assert(a == 44);
        assert(b == "asd");
    
      }
    
    
    
    }

Caveats:

*   Functors with multiple overloads of `operator()` cannot be used as members or values (the is_callable<> and func_traits<> type traits would need to be specialized for the corresonding type.) This is especially sad, because no compiler independent version for std::bind return functors are usable.
*   Method modifiers except nonconst and const are not implemented.
 

Since c++ is statically typed working around it can be hard at times, however it is possible thanks to operator overloading. Especially `operator()`, `operator[]`, `operator T()` are usefull to allow dynamic objects.

# Future work.
See the [issues](https://github.com/toeb/cppdynamic)
on GitHub for work to be done.


travis:
Command line:
"travis encrypt =jafroch --add"


   ________                                 __
  /        |                               /  |
  ########/ ______    ______    __     __  ##/    _______
     ## |  /      \  /      \  /  \   /  | /  |  /       |
     ## | /######  | ######  | ##  \ /##/  ## | /#######/
     ## | ## |  ##/  /    ## |  ##  /##/   ## | ##      \
     ## | ## |      /####### |   ## ##/    ## |  ######  |
     ## | ## |      ##    ## |    ###/     ## | /     ##/
     ##/  ##/        #######/      #/      ##/  #######/

     TRajectory Analyzer and VISualizer  -  Open-source freeware under GNU GPL v3

     Copyright (c) Martin Brehm      (2009-2014)
                   Martin Thomas     (2012-2014)
                   Barbara Kirchner  (2009-2014)
                   University of Leipzig / University of Bonn.

     http://www.travis-analyzer.de

     Please cite:
     M. Brehm and B. Kirchner, J. Chem. Inf. Model. 2011, 51 (8), pp 2007-2023.

     There is absolutely no warranty on any results obtained from TRAVIS.

  #  Running on jose-VirtualBox at Wed Oct 22 11:40:13 2014 (PID 6753).
  #  Running in /home/jose/dyn/blocks/jafroch/cppdynamic
  #  Source code version: Jan 17 2014.
  #  Compiled at Jan 19 2014 05:12:11.
  #  Compiler version: 4.8.2
  #  Target platform: Linux
  #  Compile flags: DEBUG_ARRAYS 
  #  Machine: int=4b, long=8b, addr=8b, 0xA0B0C0D0=D0,C0,B0,A0.
  #  User home: /home/jose
  #  Exe path: /usr/bin/travis
  #  Input from terminal, Output to terminal

 >>> Please use a color scheme with dark background or specify "-nocolor"! <<<

    Loading configuration from /home/jose/.travis.conf ...

Unknown parameter: "encrypt".

    List of supported command line options:

      -p <file>       Loads position data from the specified trajectory file.
                      The file format may be *.xyz, *.pdb, *.lmp (Lammps) or HISTORY (DLPOLY).
      -i <file>       Reads input from the specified text file.

      -config <file>  Load the specified configuration file.
      -stream         Treats input trajectory as a stream (e.g. named pipe): No fseek, etc.
      -showconf       Shows a tree structure of the configuration file.
      -writeconf      Writes the default configuration file, including all defines values.

      -verbose        Show detailed information about what's going on.
      -nocolor        Executes TRAVIS in monochrome mode.
      -dimcolor       Uses dim instead of bright colors.

      -credits        Display a list of persons which contributed to TRAVIS.
      -help, -?       Shows this help.

    If only one argument is specified, it is assumed to be the name of a trajectory file.
    If argument is specified at all, TRAVIS asks for the trajectory file to open.


    Note: To show a list of all persons which contributed to TRAVIS,
          please add "-credits" to your command line arguments, or set the
          variable "SHOWCREDITS" to "TRUE" in your travis.conf file.

    Source code from other projects used in TRAVIS:
      - lmfit     from Joachim Wuttke
      - kiss_fft  from Mark Borgerding
      - voro++    from Chris Rycroft

    http://www.travis-analyzer.de

    Please cite:

  * "TRAVIS - A Free Analyzer and Visualizer for Monte Carlo and Molecular Dynamics Trajectories",
    M. Brehm, B. Kirchner; J. Chem. Inf. Model. 2011, 51 (8), pp 2007-2023.

*** The End ***

