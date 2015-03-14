# Silverlight Support #

Development for Silverlight 4 support is complete as of Unquote 2.1.0.

Due to certain issues and limitations of the reduced Silverlight runtime, certain features and performance are lost.
  * [System.Numerics.BigInteger confusion](http://stackoverflow.com/questions/7048578/missing-biginteger-features-under-f-silverlight) put us in the position to remove certain static implementations of bigint operators in both Silverlight and non-Silverlight versions. This will come with a performance cost since then the implementation must fall back on reflection.
  * Can't use Compiled regex
  * Can't detect NUnit or Xunit assemblies using current methods, fall back on generic AssertionFailureException to convey failed tests.

Due to reflection security restrictions, certain features are lost. Note that Unquote's custom evaluation engine is Reflection based while PowerPack's evaluation engine is Reflection Emit based, but they are both similarly constrained by security restrictions (see http://msdn.microsoft.com/en-us/library/stfy7tfc(v=vs.95).aspx and http://msdn.microsoft.com/en-us/library/9syytdak(v=vs.95).aspx). But also note that we are blessed by Unquote's reflection based evaluation engine, since there are no Silverlight builds available for FSharp.PowerPack.Linq, which contains PowerPack's quotation evaluation engine (despite the fact the the source code build file for PowerPack indicates that there might be).
  * When TargetInvocationExceptions are stripped stacktrace information must be lost since we can't set the private field `_`remoteStackTraceString of the exception.
  * Can't evaluate nested quotations due to an [issue](http://stackoverflow.com/questions/6567225/is-it-possible-to-differentiate-between-typed-and-untyped-nested-quotations) which requires us to access some nonpublic members to convert an raw quotation to a typed quotation.