---
marp: true
theme: bizzsummit
paginate: true
header: '![image](./images/FXE.svg)'
footer: '![image](./images/DynamicsValueWide.svg) ![image](./images/jordi2.png)'
---

<style>
@import 'https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css';
</style>

# Quick Guide

### Migrating FakeXrmEasy 1.x to the latest versions

If you had a project that was using FakeXrmEasy (`FXE`) v1.x, this guide will help you migrate your projects to the latest versions in a few steps.

`FXE` allows you to **build, run, test and debug your applications 100%** `In-Memory`, which is extremely `fast`, and more `carbon friendly`.

---

## Benefits


- Drive development of `.net` applications that connect to Dataverse through **automated testing** `at scale`. 

![imageAtScale](./migrating-from-1x/tests-at-scale.png)

- Increased `developer efficiency`: prevent and find `bugs` instantly.
- <i class="fa fa-long-arrow-down"></i> CPU cycles than with integration testing > <i class="fa fa-long-arrow-down"></i> energy > `carbon friendly`.


---

## Benefits (II)


- `Mitigate` issues `locally` due to how `FXE` mimics Dataverse functionality.

- `Shortens` the developer feedback loop, cause you don't have to deploy in-between changes.

- `Open Source`: source code is public and **fully auditable**.

---

## Background

You could use `FXE v1.x` to dev & test both server-side and client-side applications in either `net452` or `net462` before.

With the latest version of the `DataverseClient` , `1.0.1`, that targets `netcoreapp3.1`, we can use now `netcore` to build **client-side applications**... 

...but we must still use the previous `Microsoft.CrmSdk.CoreAssemblies` package for **server-side development**.


---

## Server-side applications

Examples of server side applications are:

- **Plugins**
- **Custom Actions / Apis**
- **Custom Connectors**
- ...

These run inside the Dataverse's app pool / sandbox service once deployed.

---

## Client-side applications

Examples of client-side applications are:

- **Azure Functions**
- **Bespoke Web Api implementations**
- **MVC web applications**
- **Blazor applications**
- **Console apps**
- **Messaging apps**


---

## Choosing your FXE version

### Use `v2.1.2` for Server-Side...

... applications that use the `Microsoft.CrmSdk.CoreAssemblies` package.

### Use `v3.1.2` for Client-Side...

... applications that use the new `Microsoft.PowerPlatform.Dataverse.Client` `v1.0.1` package.

---


## Step 1a: Refactor to a base class


- Move all references to `XrmFakedContext` and `IOrganizationService` to a base test class
- This will make using the new middleware configuration a whole lot easier.
- Plus, it makes unit tests `smaller`, `clean`, and easier to `read`.

---
## Step 1b: Refactor to a base class


``` csharp
public class FakeXrmEasyTestBase
{
    protected readonly IOrganizationService _service;
    protected readonly XrmFakedContext _context;

    public FakeXrmEasyTestBase()
    {
        _context = new XrmFakedContext();
        _service = _context.GetOrganizationService();
    }
}
```
---

## Step 2: Uninstall FakeXrmEasy v1.x

![image](./migrating-from-1x/FakeXrmEasy_v1_uninstall.png)

---

## Step 3: Install FakeXrmEasy v2.x/v3.x

![image](./migrating-from-1x/FakeXrmEasy_v2_install.png)

---

## Step 4 (Optional):  Update namespaces

You might get build errors because some pre-existing methods have been rewritten as extension methods into dedicated namespaces.

For example, if you're unit testing plugins, include these namespaces: 

``` csharp
using FakeXrmEasy.Abstractions.Plugins;
using FakeXrmEasy.Plugins;
```

---

## Step 5: Setup middleware config

Now, we'll update the base test class that we refactored in Step 1 to use the brand new, fully configurable, `middleware`.

- Add the necessary usings:

``` csharp
using FakeXrmEasy.Abstractions;
using FakeXrmEasy.Abstractions.Enums;
using FakeXrmEasy.FakeMessageExecutors;
using FakeXrmEasy.Middleware;
using FakeXrmEasy.Middleware.Crud;
using FakeXrmEasy.Middleware.Messages;
```

---

## Step 5: Setup middleware config (II)

Replace the `XrmFakedContext` reference by a new interface `IXrmFakedContext`:


``` csharp
protected readonly IXrmFakedContext _context;
```

In the next slide we'll define the new middleware and we'll choose a relevant license:

---

## Step 5: Setup middleware config (III)

``` csharp
public class FakeXrmEasyTestBase
{
    protected readonly IOrganizationService _service;
    protected readonly IXrmFakedContext _context;

    public FakeXrmEasyTestBase()
    {
        _context = MiddlewareBuilder
                    .New()

                    .AddCrud()
                    .AddFakeMessageExecutors(Assembly.GetAssembly(typeof(AddListMembersListRequestExecutor)))

                    .UseCrud()
                    .UseMessages()
                    .SetLicense(FakeXrmEasyLicense.RPL_1_5)
                    .Build();

        _service = _context.GetOrganizationService();
    }
}
```

---

## Step 6: Re-run existing unit tests

After doing all the previous steps above now we're in a position to run again all the preexisting tests which should still `pass`.

This is because the `IOrganizationService` interface and most of the `FXE 1.x` API is forward compatible with `v2.x` and `v3.x` versions and the latest versions of the `ServiceClient` in the `Microsoft.PowerPlatform.Dataverse.Client` package, which also implements the IOrganizationService interface.


---

## Congrats!

You should have successfully migrated across the latest version!

`Remember`: `FXE` version 2 or later uses a `sustainable OSS` licensing model which is free of charge for lots of scenarios but requires a commercial license for `propietary code and commercial use`.

You're still free to use it for `evaluation purposes` in a commercial context (i.e. PoC). 

More info about [pricing here.](https://dynamicsvalue.com/pricing)


---


## Thank You!

<h4>If you liked this material please give us a <i class="fa fa-thumbs-up"></i> and share it with your peers!</h4>


- https://dynamicsvalue.github.io/fake-xrm-easy-docs/quickstart/migrating-from-1x/

- https://dynamicsvalue.com

Let's connect if you have any questions / doubts: info@dynamicsvalue.com


