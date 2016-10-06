---
author: todo
layout: master
title: Load dll file on dynamically from diffrent path
categories: dotnet
---

Load dll file on dynamically from diffrent path

### Register event

``` csharp
AppDomain currentDomain = AppDomain.CurrentDomain;
currentDomain.AssemblyResolve += new ResolveEventHandler(currentDomain_AssemblyResolve);

static Assembly currentDomain_AssemblyResolve(object sender, ResolveEventArgs args)
{
    //This handler is called only when the common language runtime tries to bind to the assembly and fails.

    //Retrieve the list of referenced assemblies in an array of AssemblyName.
    Assembly MyAssembly, objExecutingAssemblies;
    string strTempAssmbPath = "";

    objExecutingAssemblies = Assembly.GetExecutingAssembly();
    AssemblyName[] arrReferencedAssmbNames = objExecutingAssemblies.GetReferencedAssemblies();

    //Loop through the array of referenced assembly names.
    foreach (AssemblyName strAssmbName in arrReferencedAssmbNames)
    {
        //Check for the assembly names that have raised the "AssemblyResolve" event.
        if (strAssmbName.FullName.Substring(0, strAssmbName.FullName.IndexOf(",")) == args.Name.Substring(0, args.Name.IndexOf(",")))
        {
            //Build the path of the assembly from where it has to be loaded.
            //The following line is probably the only line of code in this method you may need to modify:
            strTempAssmbPath += System.IO.Directory.GetCurrentDirectory() + @"\test.dll";
            break;
        }

    }

    var bytes = System.IO.File.ReadAllBytes(strTempAssmbPath);
    //Load the assembly from the specified path.
    //MyAssembly = Assembly.LoadFrom(strTempAssmbPath);
    MyAssembly = Assembly.Load(bytes);

    //Return the loaded assembly.
    return MyAssembly;
}
```