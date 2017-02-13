Open-XML-SDK
============

Release Version 2.6.0

The Open XML SDK provides open-source libraries for working with Open XML
Documents (DOCX, XLSX, and PPTX).  It supports scenarios such as:
- High-performance generation of word-processing documents, spreadsheets, and presentations.
- Populating content in DOCX files from an XML data source.
- Splitting up (shredding) a DOCX or PPTX file into multiple files, and combining
  multiple DOCX / PPTX files into a single file.
- High fidelity conversion of DOCX => HTML/CSS and HTML/CSS => DOCX.
- Extraction of data from XLSX.
- Searching and replacing content in DOCX / PPTX using regular expressions.
- Updating cached data and embedded spreadsheets for charts in DOCX / PPTX.
- Document modification, such as removing tracked revisions or removing
  unacceptable content from documents.

[Open-Xml-PowerTools](https://github.com/OfficeDev/Open-Xml-PowerTools) provides example code and guidance for implementing many of the above important Open XML scenarios.  Be sure to check it out
before re-inventing your own solutions.

The Open-Xml-Sdk relies on the System.IO.Packaging namespace.  There is an implementation of System.IO.Packaging in the WindowsBase assembly,
and there is a new, open source implementation of System.IO.Packaging that is included with version 2.6 of the Open-Xml-Sdk.  Because of this, there
are six projects in the Visual Studio solution.  [This screen-cast](https://www.youtube.com/watch?v=B13IYHKhcc8) describes the six projects, and explains
the purpose behind each.

Copyright (c) Microsoft Open Technologies, Inc.  All rights reserved.
Licensed under the Apache License, Version 2.0.
See License.txt in the project root for license information.

News
====
We are happy to announce the release of the Open XML SDK Version 2.6.  The
big feature in 2.6 is the inclusion of a replacement for System.IO.Packaging, which
has a serious bug that causes it (albeit rarely and in specific circumstances)
to throw exceptions (ObjectDisposedException and NullReferenceException).  You can
find out more about this bug and whether it affects you in [the blog post that announces the release of the new System.IO.Packaging](http://openxmldeveloper.org/blog/b/openxmldeveloper/archive/2015/06/29/announcing-the-release-of-a-new-system-io-packaging-implementation.aspx).

Going forward, the Open XML SDK will be based on this new implementation of System.IO.Packaging.

We are also happy to announce the release of Open-Xml-PowerTools on GitHub.  Open-Xml-PowerTools provides
example code and guidance for implementing a wide range of Open XML scenarios.  You can find PowerTools for Open XML, which previously lived at http://powertools.codeplex.com,
at https://github.com/OfficeDev/Open-Xml-PowerTools.

Documentation
=============

The functionality of the specific classes in Open XML SDK Version 2.6.0 is the same as for version 2.5, therefore
the documentation available on MSDN is still accurate.

[Open XML SDK 2.5 for Office](http://msdn.microsoft.com/en-us/library/office/bb448854.aspx)

In addition to open sourcing of the SDK, Microsoft has opened up the
conceptual documentation for public review / contributions.  A copy of
the documentation is [now in GitHub](https://github.com/OfficeDev/office-content) for you to edit and review.

Build Instructions
==================

Note: for this first release, you must have some version of Visual Studio
installed.  Visual Studio 2015 Community Edition will work just fine:
https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs

To build the Open XML SDK:
- Clone the repo at https://github.com/OfficeDev/Open-XML-SDK
- Open the solution using Visual Studio 2013 or 2015.  Community Edition of VS2015 works just fine.
- Build the solution (using either Debug or Release configuration)
- Run the Xunit tests to verify the installation

If you want to use a command line approach:
- Start a Visual Studio command prompt, and change into the directory that contains the repo
- Use MSBUILD to build the SDK  (C:> MSBUILD Open-Xml-Sdk.sln)
- You can also use MSBUILD to build the individual projects.

To use the SDK:
- In your program that uses the Open XML SDK, add references to the newly built libraries in DocumentFormat.OpenXml/bin/Debug (or in DocumentFormat.OpenXml/bin/Release)

Previously, we were using PowerShell to generate a new version number for each build.  This is no longer required, and it is more convenient to build the SDK using MSBUILD, therefore I have removed the instructions for building using the PowerShell script.

Building with Mono
=================
- First, ensure you have mono installed in a manner appropriate to your linux/unix distribution.
- `make -f Makefile-Linux-Mono build`
- Find libraries in the folder: ./build/OpenXmlSdkLib
- [Screen-Cast: Using the Open XML SDK on Linux using Mono](http://openxmldeveloper.org/blog/b/openxmldeveloper/archive/2014/07/03/screen-cast-using-open-xml-sdk-on-linux-using-mono.aspx)


Description of the Projects
===========================

Copied from http://openxmldeveloper.org/discussions/development_tools/f/38/p/8480/164607.aspx

You can build the Open-Xml-Sdk using the WB solution and projects.  This builds the open source Open-Xml-Sdk with the old System.IO.Packaging in the WindowsBase assembly.  This is more or less OK so long as you are not building a web front end, a SharePoint app, or a high-performance threaded app.

All of the XUnit tests are run with both this library, and the library built with the new open source System.IO.Packaging.

In the Open-XML-SDK.sln file, there are references to four projects:

    DocumentFormat.OpenXml
    DocumentFormat.OpenXml.Tests
    DocumentFormat.OpenXml.WB
    DocumentFormat.OpenXml.WB.Tests
    System.IO.Packaging
    System.IO.Packaging.Tests

The System.IO.Packaging project contains the new System.IO.Packaging implementation, which works with .NET 4.5 and above only, because of a dependency on System.IO.Compression, certain classes of which are available only in 4.5 and above.

System.IO.Packaging.Tests are the XUnit tests for System.IO.Packaging, of course.

DocumentFormat.OpenXml.WB builds against the System.IO.Packaging in the WindowsBase assembly.  The DicumentFormat.OpenXml.WB.Tests are linked source files to DocumentFormat.OpenXml.Tests, and test out the basic functionality of the Open-Xml-Sdk when built in that fashion.  You can build with .NET 3.5 and above when building with the WindowsBase assembly.

Cheers, Eric

