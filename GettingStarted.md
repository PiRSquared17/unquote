# Getting Started #

## Installation ##

Unquote has builds targeting both .NET 4.0 and Silverlight 4.

To use Unquote within an F# unit testing project, you must obtain and add a reference to Unquote.dll. There are two ways to obtain Unquote:

  * Install the latest version of the [Unquote NuGet package](http://www.nuget.org/List/Packages/Unquote) to your unit testing project, this will automatically add the correct reference to Unquote.dll according to your project type (.NET 4.0 or Silverlight 4).
  * Download and unzip the featured release of Unquote from the [Downloads](https://code.google.com/p/unquote/wiki/Downloads?tm=2) page and then locate and add a reference to Unquote.dll via the Visual Studio Add Reference dialog. The .NET 4.0 builds are located in the net40 folder, the Silverlight 4 builds are located in the sl4 folder.

You must also obtain and install your unit testing framework of choice, NUnit and xUnit having been given special support.

To use Unquote within FSI, locate and add a reference to Unquote.dll using the `#r` directive (use the .NET 4.0 build).

_To use the .NET 4.0 build within a Visual Studio 2012 library or project, you must add a binding redirect from FSharp.Core 4.0.0.0 to 4.3.0.0. See [Issue 78](https://code.google.com/p/unquote/issues/detail?id=78) for more information._

## Mono Support ##

Unquote has not been fully tested on Mono, however, it does not use any native Win32 calls and has been seen working on Mono without any issues. Specifically, the .NET 4.0 build has been seen running on _Mono 2.10.8.1 (Debian 2.10.8.1-5ubuntu1)_ inside a version of the F# 3.0 Interactive built from the github sources.

## User Guide ##

The `Swensen.Unquote` namespace contains three `AutoOpen` modules: `Assertions`, `Operators`, and `Extensions`. Therefore you may choose to bring all of Unquote's features into top-level scope simply by opening `Swensen.Unquote` or you may choose to open individual modules or even alias individual modules for a finer level of control.

### Assertions ###

The `Swensen.Unquote.Assertions` module contains all functions used for performing unit testing assertions. These include `test`, `raises`, and the `xx?` series of binary infix operators. All of these operators can be used within a unit test enabled project or FSI.

Unquote chooses its output source as follows
  * if loaded in FSI then print to the console
  * else if xUnit or NUnit loaded in currently executing assembly, then use appropriate test failed methods
  * else throw a `Swensen.Unquote.Assertions.AssertionFailureException` with a message

The following is a reference of the functions available and some examples:

  * `val inline test : Quotations.Expr<bool> -> unit`
```
> test <@ (1+2)/3 = 1 @>;;
val it : unit = ()
> test <@ (1+2)/3 = 2 @>;;

Test failed:
	
(1 + 2) / 3 = 2
3 / 3 = 2
1 = 2
false

val it : unit = ()
```

  * `val inline ( =? ) : 'a -> 'a -> unit when 'a : equality`
```
> [1;2;3;4] =? [4;3;2;1];;

Test failed:

[1; 2; 3; 4] = [4; 3; 2; 1]
false

val it : unit = ()
```
  * `val inline ( <? ) : 'a -> 'a -> unit when 'a : comparison`
  * `val inline ( >? ) : 'a -> 'a -> unit when 'a : comparison`
  * `val inline ( <=? ) : 'a -> 'a -> unit when 'a : comparison`
  * `val inline ( >=? ) : 'a -> 'a -> unit when 'a : comparison`
  * `val inline ( <>? ) : 'a -> 'a -> unit when 'a : equality`
  * `val inline raises<'a when 'a :> exn> : Quotations.Expr -> unit`
```
> raises<exn> <@ (null:string).Length @>;;
val it : unit = ()
> raises<NullReferenceException> <@ (null:string).Length @>;;
val it : unit = ()
> raises<System.ArgumentException> <@ (null:string).Length @>;;

Test failed:

Expected exception of type 'ArgumentException', but 'NullReferenceException' was raised instead

null.Length
System.NullReferenceException: Object reference not set to an instance of an object.
   at Swensen.Unquote.Evaluation.evalInstance@275(FSharpList`1 env, FSharpOption`1 expr)
   at Swensen.Unquote.Evaluation.eval@133(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.reduce(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.loop@132(FSharpList`1 env, FSharpExpr expr, FSharpList`1 acc)

val it : unit = ()
> raises<exn> <@ 3 @>;;

Test failed:

Expected exception of type 'Exception', but no exception was raised

3

val it : unit = ()
```
  * `val inline raisesWith : Expr -> (#exn -> Expr<bool>) -> unit`
```
> raisesWith<System.NullReferenceException> <@ (null:string).Length @> (fun e -> <@ e.ToString() = null @>);;

Test failed:

The expected exception was raised, but the exception assertion failed:

Exception Assertion:

System.NullReferenceException: Object reference not set to an instance of an object.
   at Swensen.Unquote.Evaluation.evalInstance@275(FSharpList`1 env, FSharpOption`1 expr)
   at Swensen.Unquote.Evaluation.eval@133(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.reduce(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.loop@132(FSharpList`1 env, FSharpExpr expr, FSharpList`1 acc).ToString() = null
"System.NullReferenceException: Object reference not set to an instance of an object.
   at Swensen.Unquote.Evaluation.evalInstance@275(FSharpList`1 env, FSharpOption`1 expr)
   at Swensen.Unquote.Evaluation.eval@133(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.reduce(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.loop@132(FSharpList`1 env, FSharpExpr expr, FSharpList`1 acc)" = null
false

Test Expression:

null.Length
System.NullReferenceException: Object reference not set to an instance of an object.
   at Swensen.Unquote.Evaluation.evalInstance@275(FSharpList`1 env, FSharpOption`1 expr)
   at Swensen.Unquote.Evaluation.eval@133(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Evaluation.eval(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.reduce(FSharpList`1 env, FSharpExpr expr)
   at Swensen.Unquote.Reduction.loop@132(FSharpList`1 env, FSharpExpr expr, FSharpList`1 acc)

val it : unit = ()
```
### Operators ###

The `Swensen.Unquote.Operators` module contains additional functions for decompiling, evaluating, and reducing quotation expressions. The following is a reference of the functions available and some examples:

  * `val inline decompile : Quotations.Expr -> string`
```
> decompile <@ (1+2)/3 @>;;
val it : string = "(1 + 2) / 3"
```

  * `val inline eval : Quotations.Expr<'a> -> 'a`
```
> eval <@ "Hello World".Length + 20 @>;;
val it : int = 31
```

  * `val inline evalRaw : Quotations.Expr -> 'a`
```
> evalRaw<int> <@@ "Hello World".Length + 20 @@>;;
val it : int = 31
```

  * `val inline reduce : Quotations.Expr -> Quotations.Expr`
```
> <@ (1+2)/3 @> |> reduce |> decompile;;
val it : string = "3 / 3"
```

  * `val inline reduceFully : Quotations.Expr -> Quotations.Expr list`
```
> <@ (1+2)/3 @> |> reduceFully |> List.map decompile;;
val it : string list = ["(1 + 2) / 3"; "3 / 3"; "1"]
```

  * `val inline isReduced : Quotations.Expr -> bool`
```
> <@ (1+2)/3 @> |> isReduced;;
val it : bool = false
> <@ 1 @> |> isReduced;;
val it : bool = true
```

  * `val inline unquote : Quotations.Expr -> unit`
```
> unquote <@ (1+2)/3 @>;;

(1 + 2) / 3
3 / 3
1

val it : unit = ()
```

The following are functions which accept a variable environment for processing synthetic expressions with unbound variables (N.B. this API may evolve and should be considered "experimental"):

  * `val inline evalWith : Map<string,obj> -> Quotations.Expr<'a> -> 'a`
  * `val inline evalRawWith : Map<string,obj> -> Expr -> 'a`
  * `val inline reduceWith : Map<string,obj> -> Quotations.Expr<'a> -> Quotations.Expr`
  * `val inline reduceFullyWith : Map<string,obj> -> Quotations.Expr<'a> -> Quotations.Expr list`

### Extensions ###

The `Swensen.Unquote.Extensions` module duplicates functions in the `Swensen.Unquote.Operators` as instance type extensions on `Quotations.Expr` and `Quotations.Expr<'a>`. But it also includes an instance property extension on `System.Type`, `FSharpName`, which returns the F#-style signature of a type (N.B. this feature is out-of-place in Unquote and should be considered "experimental"):

```
> typeof<int>.FSharpName;;
val it : string = "int"
> typeof<int[]>.FSharpName;;
val it : string = "int[]"
> typeof<int[,,,]>.FSharpName;;
val it : string = "int[,,,]"
> typeof<System.Collections.Generic.Dictionary<string, float>>.FSharpName;;
val it : string = "Dictionary<string, float>"
> typeof<unit -> int -> string>.FSharpName;;
val it : string = "unit -> int -> string"
> typeof<unit -> (float -> int) -> string>.FSharpName;;
val it : string = "unit -> (float -> int) -> string"
> typeof<int * float * string>.FSharpName;;
val it : string = "int * float * string"
> typeof<int * (bool * float) * string>.FSharpName;;
val it : string = "int * (bool * float) * string"
> typeof<int -> (list<(int * (int -> string))[]> * string[,,])>.FSharpName;;
val it : string = "int -> list<(int * (int -> string))[]> * string[,,]"
> typedefof<int -> int>.FSharpName;;
val it : string = "'T -> 'TResult"
```