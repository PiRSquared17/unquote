## Visual Studio and F# ##

Development is currently done in VS 2010 with the F# 2.0 compiler. Any tier of VS will do, including the free Shell (the solution is 100% F#). Note that we cannot use VS 2012 at this time, since F# 3.0 + VS 2012 does not support Silverlight 4.

## Silverlight 4 Tools ##

Silverlight 4 development requires installation of the Silverlight 4 Tools for Visual Studio 2010. In fact, you can't avoid this even if you don't plan on touching the Unquote Silverlight project since Visual Studio will give you tons of errors upon startup if it is not properly installed.

Installing the tools can be tricky, due to a bug in the installer which doesn't install F# support. However, the following steps should work.

  * [Download Microsoft Silverlight 4 Tools for Visual Studio 2010](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=18149) and unzip it (although it has a .exe extension, it is actually a zip. running the .exe will result in an incomplete installation which doesn't include the F# tools)
  * Run FSharpRuntimeSL4.msi
  * Run silverlight\_developer.exe
  * Run silverlight\_sdk.msi.

In some instances, Visual Studio errors regarding Silverlight can be due to a corruption in Visual Studio which may not manifest in other ways. In such a case, try repairing / reinstalling Visual Studio through appwiz.cpl, starting with the Service Packs.

## Binary Dependencies ##

Most 3rd-party binary dependencies are resolved through NuGet solution integration. However, those which are not available as NuGet packages are placed in the `\lib` folder within the repository.

At the time of this writing, the only 3rd-party binaries required are within the test projects (Unquote itself has no such dependencies).

## Continuous Integration ##

TeamCity continuous integration is hosted at http://teamcity.codebetter.com/project.html?projectId=project223&tab=projectOverview. Work needs to be done to make this truly functional. For now, all we have is build-ci.bat which compiles the code and packages archives. Our end goal is to have the build server build, run all tests (including silverlight tests), package archives, and possibly deploy archives.