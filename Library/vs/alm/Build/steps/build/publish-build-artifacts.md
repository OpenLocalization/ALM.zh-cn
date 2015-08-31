Title: Publish build artifacts
Description: Publish build artifacts
ms.TocTitle: Publish build artifacts
ms.ContentId: 1BB6BB93-5749-438F-B0BD-B8DB61141D9A

#Publish build artifacts

Copy binaries and other files you need from your build process into an artifact on your server or file share so your team can download them.

##Demands

None

##Arguments

<table>
  <thead>
    <tr>
      <td caps_internal_Id="2af9b9bf-c5fd-49e9-9b2b-03dad3f3c731">Argument</td>
      <td caps_internal_Id="93f9bd00-4e55-47f6-b919-b6f3d05e9e71">Description</td>
    </tr>
  </thead>
  <tr>
    <td caps_internal_Id="a951e8d7-302d-4980-95b2-2219b5c3475b">Copy Root</td>
    <td>
      <p caps_internal_Id="0326529c-734c-4ffc-a422-e32a67eb012d">Specify the root folder that contains the files you want to copy.</p>
      <p caps_internal_Id="efeca3c3-6d75-4eca-a908-36c0d5ea0599">Leave it empty to copy from the root folder of the repo. For example ```C:\a\```&lt;randomid&gt;```\OurRepo```</p>
      <p caps_internal_Id="c1fe003a-e783-4020-b1f1-51f162d5373b">Specify $(Agent.BuildDirectory) if your build produces artifacts outside of the sources directory. This will copy files from the build agent working directory. For example ```C:\a\```&lt;randomid&gt;</p>
    </td>
  </tr>
  <tr>
    <td caps_internal_Id="ffcf77db-3de9-4d94-9ff9-5de1965485c8">Contents</td>
    <td>
      <p caps_internal_Id="e6080b55-e08f-4e2a-946a-d43d502c025d">Specify minimatch pattern filters (one on each line) that you want to apply to the list of files to be copied. For example, specify ```**\bin``` to copy files in any sub-folder named bin.</p>
    </td>
  </tr>
  <tr>
    <td caps_internal_Id="1074cb07-5b5f-44eb-a2f5-a53fb13ff509">Artifact Name</td>
    <td caps_internal_Id="9adf5b3c-2426-407a-b0f0-837abe04df91">Specify the name of the artifact.</td>
  </tr>
  <tr>
    <td caps_internal_Id="1a821727-72af-497e-98dc-aada8b06f497">Artifact Type</td>
    <td>
      <p caps_internal_Id="433fb35f-e738-406e-a8c5-c85f5dc5d23c">Choose server to store the artifact on your Team Foundation Server. This is the best and simplest option in most cases.</p>
      <p caps_internal_Id="fae806af-c0a6-48cf-abd9-906c7058ed8c">Choose file share to copy the artifact to a file share. Some common reasons to do this:</p>
      <ul>
        <li caps_internal_Id="fb2b43db-e128-40bb-9037-0875080579be">The size of your drop is large and consumes too much time and bandwidth to copy.</li>
        <li caps_internal_Id="82dcb909-ef0f-48fd-b1e2-7dd8c5dce9a8">You need to run some custom scripts or other tools against the artifact.</li>
      </ul>
      <p caps_internal_Id="6b654c81-6273-4d89-b42b-ea5c00c7cd3b">
If you use a file share, specify the UNC file path to the folder. You can control how the folder is created for each build using variables. For example ```\\my\share\$(Build.DefinitionName)\$(Build.BuildNumber)```. 
</p>
      <p caps_internal_Id="1d60d795-abdc-47fe-b5eb-b5348ff8ac1a">**Note:** You cannot use a file share if you are using the [hosted pool](https://www.visualstudio.com/get-started/build/hosted-agent-pool).</p>
    </td>
  </tr>
</table>

##Examples

###Copy executables and readme file

####Goal

You want to copy just the read me and the files needed to run this C# console app:

*   Solution 'ConsoleApplication1' (3 projects)
    
    *   C# ClassLibrary1
    *   C# ClassLibrary2
    *   C# ConsoleApplication1   

####Arguments

*   Copy Root: $(Build.SourcesDirectory)
*   Contents example 1: 


   ```
   ConsoleApplication1\bin\**\*.exe
   ConsoleApplication1\bin\**\*.dll
   readme.txt

   ```


*   Content example 2:


   ```
   ConsoleApplication1\bin\**\?(*.exe|*.dll)
   readme.txt

   ```


*   Artifact Name: drop

####Results

The *drop* artifact is published and contains these files:

*   ConsoleApplication1
    
    *   bin
    *   Release
        
        *   ClassLibrary1.dll
        *   ClassLibrary2.dll
        *   ConsoleApplication1.exe
*   readme.txt

##Q & A


###Q: I'm having problems. How can I troubleshoot them?

A: Try this:

1.  On the variables tab, add `system.debug` and set it to `true`.
    Select to allow at queue time.
2.  In the explorer tab, view your completed build and click the publish artifact step to view the output.

###Q: What's a minimatch pattern? How does it work?

See https://github.com/isaacs/minimatch and https://realguess.net/tags/minimatch/.

###Q: What variables can I use?

A: [Predefined variables](/library/vs/alm/build/scripts/variables.md)



