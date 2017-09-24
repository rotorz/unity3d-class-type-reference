# unity3d-class-type-reference

An example npm package that contains some example assets to demonstrate how to create a
package for use in games made using the Unity game engine.


```sh
$ npm install --save rotorz/unity3d-class-type-reference
```

This package is compatible with [unity3d-package-syncer](https://github.com/rotorz/unity3d-package-syncer).

![screenshot](screenshot.png)


## Warning

Whilst we have not encountered any platform specific issues yet, the source code in this
repository *might* not necessarily work for all of Unity's platforms or build
configurations. It would be greatly appreciated if people would report issues using the
issue tracker.


## Usage Examples

Type references can be made using the inspector simply by using `ClassTypeReference`:

```csharp
using Rotorz.Games.Reflection;
using UnityEngine;

public class ExampleBehaviour : MonoBehaviour
{
    public ClassTypeReference greetingLoggerType;
}
```

A default value can be specified in the normal way:

```csharp
public ClassTypeReference greetingLoggerType = typeof(DefaultGreetingLogger);
```

You can apply one of two attributes to drastically reduce the number of types presented
when using the drop-down field.

```csharp
using Rotorz.Games.Reflection;
using UnityEngine;

public class ExampleBehaviour : MonoBehaviour
{
    // Allow selection of classes that implement an interface.
    [ClassImplements(typeof(IGreetingLogger))]
    public ClassTypeReference greetingLoggerType;

    // Allow selection of classes that extend a specific class.
    [ClassExtends(typeof(MonoBehaviour))]
    public ClassTypeReference someBehaviourType;
}
```

To create an instance at runtime you can use the `System.Activator` class from the .NET /
Mono library:

```csharp
using Rotorz.Games.Reflection;
using System;
using UnityEngine;

public class ExampleBehaviour : MonoBehaviour
{
    [ClassImplements(typeof(IGreetingLogger))]
    public ClassTypeReference greetingLoggerType = typeof(DefaultGreetingLogger);


    private void Start()
    {
        if (this.greetingLoggerType.Type == null) {
            Debug.LogWarning("No type of greeting logger was specified.");
        }
        else {
            var greetingLogger = Activator.CreateInstance(this.greetingLoggerType) as IGreetingLogger;
            greetingLogger.LogGreeting();
        }
    }
}
```

Presentation of drop-down list can be customized by supplying a `ClassGrouping` value to
either of the attributes `ClassImplements` or `ClassExtends`.

- **ClassGrouping.None** - No grouping, just show type names in a list; for instance,
  "Some.Nested.Namespace.SpecialClass".

- **ClassGrouping.ByNamespace** - Group classes by namespace and show foldout menus for
  nested namespaces; for instance, "Some > Nested > Namespace > SpecialClass".

- **ClassGrouping.ByNamespaceFlat** (default) - Group classes by namespace; for instance,
  "Some.Nested.Namespace > SpecialClass".

- **ClassGrouping.ByAddComponentMenu** - Group classes in the same way as Unity does for
  its component menu. This grouping method must only be used for `MonoBehaviour` types.

For instance,

```csharp
using Rotorz.Games.Reflection;
using UnityEngine;

public class ExampleBehaviour : MonoBehaviour
{
    [ClassImplements(typeof(IGreetingLogger), Grouping = ClassGrouping.ByAddComponentMenu)]
    public ClassTypeReference greetingLoggerType;
}
```


## Contribution Agreement

This project is licensed under the MIT license (see LICENSE). To be in the best
position to enforce these licenses the copyright status of this project needs to
be as simple as possible. To achieve this the following terms and conditions
must be met:

- All contributed content (including but not limited to source code, text,
  image, videos, bug reports, suggestions, ideas, etc.) must be the
  contributors own work.

- The contributor disclaims all copyright and accepts that their contributed
  content will be released to the public domain.

- The act of submitting a contribution indicates that the contributor agrees
  with this agreement. This includes (but is not limited to) pull requests, issues,
  tickets, e-mails, newsgroups, blogs, forums, etc.
