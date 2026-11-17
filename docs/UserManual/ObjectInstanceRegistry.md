# Object Instance Registry

The **Object Instance Registry** lists the object instances a federate registers when it runs, and tells the code generator how and when to register them. It is the first tab of the [Federate Behavior Editor](FederateBehaviorEditor.md) of a **federate application**: instances are declared by the federate that registers them, so the declarations belong to the application's behavior and are bound to the classes of its SOM. A FOM module has no registry.

For the rest of the behavior editor see [FBE — Federate Behavior Editor](FederateBehaviorEditor.md). For the object model see [OME — Object Model Editor](OME.md). For what the generator makes of the declarations, see [Code Generator](CodeGenerator.md).

![The Object Instance Registry tab of the Federate Behavior Editor: the detail of the selected declaration on the left, the list of declarations on the right](images/fbe-instance-registry.png)

*The registry tab of the Federate Behavior Editor. The selected declaration is edited on the left; the list of all declarations is on the right.*

---

## Where to find it

Open the **Federate Behavior Editor** of the application (see [Opening the editor](FederateBehaviorEditor.md#opening-the-editor)); the registry is its **Object Instance Registry** tab. It lists every declaration of the application. A change marks the project as modified.

Two applications that use the same SOM each have their own registry: editing one does not change the other. An application without a SOM shows a message in place of the registry until a SOM is assigned to it in [FAME](FAME.md).

> The registry used to be a tab of the OME and of the Object Class editor. Those tabs are gone; declarations of an older project are moved to the applications that use the SOM when the project is opened (see [Saving](#saving)).

---

## Layout

Like the other split views, the **detail** (properties) is on the left and the **master** list is on the right. Each side has its own toolbar:

| Toolbar | Where | What it does |
|---|---|---|
| **Detail toolbar** | above the detail | **Add**, **Duplicate** and **Delete** change the declarations. |
| **Master toolbar** | above the list | One row: **Copy** copies the listed declarations to the clipboard as a Markdown table (Instance Name, Class, Count, Strategy, Trigger, Release Name On Delete, Issue), the **grouping** box groups the list, and the filters narrow it by object class, naming strategy and trigger, and a text search on name or class. The copied table contains only the rows the list shows. |

Under the detail toolbar, **New declarations** shows the naming strategy a newly added declaration starts with, and the **HLA prefix guard** (see [Validation](#validation)).

**Grouping** offers *No grouping*, *Group by class*, *Group by strategy* and *Group by trigger*. Each group has a round chevron header, like the sections of the Dashboard, that shows its name and how many declarations it holds, and can be collapsed; a row moves to its new group when you edit the field it is grouped by. Filters and grouping combine.

Pressing **Delete** with a row selected in the list also deletes it. The list header shows how many declarations the view has and how many instances they register.

---

## A declaration

Each row is one **declaration**: a class, a way of getting the instance name, and a moment to register.

### Identity

- **Target Class** — the object class the instances are registered for. The federate has to publish the class.
- **Instance Name** — shown for the Guaranteed strategy only.

### Registration & Reservation

**Strategy** decides where the name comes from:

| Strategy | Names | Count |
|---|---|---|
| **Autonomous (RTI-Assigned)** — the default | The RTI assigns the name when the instance is registered. The list shows *(RTI-assigned)*. | The **Count** field: several instances of the class in one row. |
| **Guaranteed Reservation (Pre-Reserved)** | One name that you give; it is reserved first and registered as given. | Always 1. |
| **Pattern-Based (Indexed Sequence)** | A pattern such as `UAV_{Index:D2}`, a **Start Index** and a **Count**; the card previews the first and last name (`UAV_01 … UAV_04`). | The **Count** field. |

To tell instances apart by name, give them a name: the strategy then is Guaranteed or Pattern-Based. The registry has no separate label.

**Trigger** decides when the generated federate registers the instances:

| Trigger | Registered |
|---|---|
| **Startup Immediate** | As soon as the federation is ready. |
| **Advisory-Driven (Start Registration)** | When the RTI advises the federate to start registering instances of the class — that is, when another federate of the federation subscribes to a published attribute of it. Needs the **Object Class Relevance Advisory** switch of the SOM to be enabled. |
| **On-Demand (Manual)** | When your code asks for it. |

**Lifecycle Policy** (shown for reserved names only) decides what happens to the name when its instance is deleted: **Release name when instance is deleted** gives it back to the RTI, **Keep name reserved** holds it until the federate resigns.

Names are reserved while the federate starts, whatever the trigger, so the instance of an advisory-driven declaration is registered as soon as the advisory arrives.

### Initial Attribute Values

The grid lists every attribute of the target class, the ones it inherits included, with its data type and an **Initial Value** you can type. Every instance of the declaration gets the same values. An empty value (or **(none)**) gives none from the registry: that attribute is left to the application (see [Values only the application knows](#values-only-the-application-knows)).

**Sending** decides when the values go out. *Send at registration* (the default) submits them to the generated update policy after registration. With Attribute Relevance enabled, Off/Unknown values wait until On; available relevant values are sent, grouped by tag/hint. *Send only when requested* only sets the state of the instance; the values are sent when another federate asks for them. Either way the instance keeps the values as its state and the generated federate answers a request for them from that state, so a federate that joins later receives them too, with whatever the application has updated since.

What you type is checked against the attribute's data type as you type; a wrong value gets a red icon with the reason, and blocks code generation:

| Data type | Accepted |
|---|---|
| Boolean (`HLAboolean`) | Chosen from **(none)**, `true` and `false` |
| Integers | A whole number inside the range of the type (`HLAinteger16BE`: −32768 … 32767, unsigned types: no negative numbers) |
| Floating point | A decimal number with a point (`1.5`, `-2e3`); not `NaN` or infinity |
| Character | One character, or its code as `X`; `HLAASCIIchar` takes ASCII only |
| String | Any text; `HLAASCIIstring` takes ASCII only |
| Enumeration | Chosen from a list of the enumerators, led by **(none)** |
| Opaque data and other byte arrays | Hexadecimal pairs, for example `0A 1B`; a fixed-size array needs exactly that many bytes |

Records, variant records, references and arrays of other element types cannot be given a value yet; their rows are greyed and cannot be edited. Choosing another class for the declaration starts without values, and an attribute that is deleted from the class disappears from the grid together with its value. Renaming an attribute keeps its value.

### Region / DDM Scope

When the object class has dimensions (its own and the ones it inherits), the **Region / DDM Scope** section has a **Region** box. It offers *none (default region)* and the regions of the [DDM design](DdmDesign.md) the class can use, that is, the regions made of dimensions the class has. Choose one and the generated federate registers every instance of the declaration with that region (IEEE 1516.1-2025 §9, Register Object Instance With Regions): the region is created and committed once, every attribute the instances publish is associated with it, and it is deleted again before the federate resigns. The ranges of the region are listed under the box; edit them in the DDM tab. A federate that subscribes with regions receives the instances only where its regions overlap theirs.

Declarations that name the same region share it, and so do the classes that subscribe with it: the generated code creates one region for all of them.

With no region, or for a class that has no dimensions, the instances are registered in the default region (§9.1.3.2): every subscriber, with or without regions, can receive them. The generator reports this for a class that has dimensions (`CG5012`, information only). Choosing another class keeps the region when the new class can use it and drops it otherwise. If the region is deleted from the design, the declaration keeps its name and shows an error until you choose another region or none.

> Projects saved before the DDM design gave each declaration its own ranges. When one is opened, the ranges become a named region (`VesselRegion`, named after the class) that the declaration refers to, and the project asks to be saved.

---

## Validation

Every row shows a status icon: a green check, an amber warning, or a red error whose tooltip says why. The card repeats the message under the field.

| Check | Level |
|---|---|
| No class selected, or the class no longer exists in the module | Error |
| Guaranteed: the name is empty | Error |
| Pattern: no `{Index}` placeholder, or an invalid format | Error |
| A name begins with **"HLA"** — IEEE 1516.1-2025 §6.2.3 (d) and §6.5.3 (d) do not allow reserving such a name. The check is case-sensitive, so `hlaLead` is accepted. | Error |
| Another Guaranteed declaration reserves the same name | Error |
| An initial value does not fit its attribute's data type | Error |
| The region of the declaration no longer exists in the DDM design, or covers a dimension the class does not have | Error |
| A range of the region is not a valid range of its dimension (one bound only, not whole numbers, lower not below upper, upper above the dimension's upper bound) | Error |
| The class is not published by the federate, so the RTI would refuse the registration | Warning |

The **HLA prefix guard** can be switched off; the generator then only warns. Turn it off only when you know the RTI you use accepts such names.

If you delete a class that has declarations, the declarations are kept and shown with an error, so you can choose another class or delete them. Renaming or moving a class needs no maintenance: the declarations follow it.

---

## Saving

Declarations are saved with the behavior of the application, in `Behavior\‹application›.sfbm` (see [FBE — Saving](FederateBehaviorEditor.md#saving)). They are not written to the SOM module's files, whose `.xml` content stays a standard IEEE 1516.2 object model, and they do not appear in exported FDD or FED files. An application that declares nothing writes no instance data.

A project saved before behavior models kept the declarations in the `.sfom` metadata of the SOM module. Opening it moves them to the behavior of every application that uses the module, and the project asks to be saved.

---

### Values only the application knows

Some values exist only at run time, such as the nickname of the user who started the program. Leave such an attribute empty. For every declaration with at least one empty attribute the generated manager has a hook, `protected virtual <Declaration>InitialValues()`, which returns a data object of the class. The manager calls it when it registers each instance and sends what it returns, together with the declared values, in the same first update:

```csharp
protected override ClientSession ClientSessionInitialValues()
  => new() { NickName = NickName, StartedAt = DateTime.Now.ToOADate() };
```

A value the registry declares is sent as declared; a value the hook sets for such an attribute is ignored. The generated manager also gives the registered instances as entities: `<Declaration>Instances` (an `IReadOnlyList` of the class's entity).

## Code generation

For an application with declarations the generated Fora federate:

1. checks the declarations before writing any file; an error stops generation (codes `CG5001`–`CG5009`, see [Code Generator](CodeGenerator.md));
2. reserves the declared names while the federate starts and waits for the RTI's answer (IEEE 1516.1-2025 §6.2 – §6.8);
3. registers each instance with its name only after the reservation succeeded; if the RTI refuses a name, the run fails with `InstanceNameReservationException` and nothing is registered under that name;
4. registers startup declarations when the federation is ready, advisory-driven ones after the Start Registration advisory, and on-demand ones when `Register<Class>InstancesAsync()` is called;
5. submits the declaration's initial attribute values to the managed publication policy, with the values the application's `<Declaration>InitialValues()` hook returns for the attributes left empty, after registration; relevance and ownership determine the actual updates (or keeps the declared ones as state, if the declaration sends them only on request), and answers a later request for them from the instance's state;
6. releases a name when its instance is deleted, if the declaration asks for it.

`SimulationManagerBase.DeclaredInstances` reports, for every declaration, whether its names are reserved, whether it is registered, and the instances registered so far. The generated `README.generated.md` lists the declarations.

Declarations of a class the federate does not publish are skipped with a warning. A model with no declarations generates the same code as before.

---

## Tips

- Give an instance its starting state in **Initial Attribute Values** instead of updating it by hand after registering, and override `<Declaration>InitialValues()` for the values only the program knows.
- Use Autonomous for populations whose members need no name; use Pattern-Based for a numbered fleet.
- Prefer **Advisory-Driven** for instances nobody needs until a federate subscribes; enable the **Object Class Relevance Advisory** switch in the module's [Switches table](OME.md#13-switches-table) first.
- Two applications can share one SOM and declare different instances; the generator reads the declarations of the application it generates for.
- Two federates cannot reserve the same name in one federation execution. Give each federate's declarations distinct names, or use Autonomous.

---

**Previous:** [FBE — Federate Behavior Editor](FederateBehaviorEditor.md) · **Next:** [DDM Design](DdmDesign.md)

---
Updated October 9, 2026


Runtime switch note: disabling Object Class Relevance releases the registration wait of an AdvisoryDriven declaration on the heartbeat. It registers only once; Stop Registration never deletes an existing instance. See [Advisory-aware sends and updates](CodeGenerator.md#9-advisory-aware-sends-and-updates) for initial-value buffering, explicit requests and failure behavior.

### Publication policy customization

Declared initial values use the same generated defaults as later updates. Override `ShouldSendAttribute`, `ShouldBufferAttribute` and `ShouldAutomaticallyFlush` in manual SimulationManager to change publication behavior; these hooks do not change declaration activation or create additional instances. Non-buffered initial values blocked by advice are discarded, so supply fresh values when the application wants to send. See [manual publication examples](CodeGenerator.md#91-overriding-publication-policies-in-manual-simulationmanager).

## Manual lifecycle policy overrides

Model triggers are defaults. Generated managers with declarations expose `ShouldRegisterDeclaredInstance`, `ShouldDeleteDeclaredInstances`, typed `<Key>Instances`, runtime `<Key>InitialValues`, registration/removal hooks and explicit register/delete commands. Default Stop Registration retains existing objects; automatic deletion is an application opt-in. Slot bookkeeping, released-name reservation and cleanup are generated. Partial retries fill only missing slots. See [lifecycle examples and threading rules](CodeGenerator.md#10-customizing-declared-object-lifecycle).
