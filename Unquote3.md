# Introduction #

It's been over two years since the last release of Unquote. Partly because it is version 2.2.2 is very stable, partly because breaking changes in vs2012 and F# 3.0 delayed value in upgrading from vs2010 and F# 2.0. However, things have stabilized around vs2013 and F# 3.1, so we are due for some new bits!

# Planned Features #

The following are the main features planned for Unquote 3.0.0.
  * Drop support for Silverlight4.
  * Upgrade main solution and projects to 2013 and add support for net45 and portable profile 259
  * Continue support for net40 with a secondary vs2010 solution file and projects
  * Deprecate xx? operators (they conflict with F# 3.1 nullable operators) and replace with xx! operators
  * Add support for xunit 2.x
  * Possibly add FSharp.Core dependencies in NuGet package?

# TODO #
  * Add test cases for net40
  * Add assembly info for net40
  * Add net40 to build.bat
  * Add verify project for xunit2
  * Possibly do better automating testing, building, and packing with fake