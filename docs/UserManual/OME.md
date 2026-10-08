# OME (Object Model Editor)

The Object Model Editor (OME) is the main workspace for editing a FOM or SOM module in tabular form. It combines hierarchy-oriented table views, flat property tables, and item editors for detailed OMT work.

For accepted names, duplicate-name scopes, parent and inheritance rules, required fields, and disabled confirmation messages, see [Editing Rules and FOM Validation](Validation.md#names-while-editing).

Every OME editor dialog shares an orange Error/Warning status bar with message navigation and a Copy button. See [Editor validation status bar](Validation.md#editor-validation-status-bar) for colors, Save behavior, and copying all messages.

![The OME table editor showing an OMT table with its rows and the editing toolbar](images/table-editor.png)

*The OME table editor, here showing a module's **Attribute** table as editable rows (name, object class, data type, update type/condition, P/S, transportation, order, …). The toolbar's Add/edit controls adapt to what the active table supports, and double-clicking a row opens its dedicated item editor. The module workspace also carries tabs along the bottom — **Dashboard**, **Table Editor**, **FED Viewer**, **FDD Viewer (2010)**, **FDD Viewer (2025)**, and **Diagram Editor** — covered in their own chapters. A SOM module adds a last tab, the [Object Instance Registry](ObjectInstanceRegistry.md).*

---

## Workspace Scope

OME is module-scoped. Each open OME tab works on one active module and can expose:

- identification metadata
- object classes
- interaction classes
- directed interactions
- attributes
- parameters
- dimensions
- time representations
- tags
- synchronizations
- transportations
- update rates
- switches
- datatypes
- notes
- services

### MOM integration

The MOM checkbox in the identification area adds the standard Management Object Model to the open module so you can see and reference its classes. What you switch on is a **view over your module, not part of it**: the module file is always written without MOM, and the setting is remembered in the module metadata, so reopening the project restores it. Switching it off removes every MOM element again, leaving your own content untouched. Standard modules (MIM/MOM) cannot be integrated into themselves.

Changes made in OME mark the active module as modified and set the project save state to `Unsaved changes` until the next successful save.

Sample references in this document use the installed Chat sample under `C:\ProgramData\SimGe\Samples\Chat\Fom`, primarily `ChatSom.xml`.

---

## Dashboard: semantic diagnosis

For the complete dashboard guide, including calibration controls and analysis scope, see [FOM Dashboard](Dashboard.md#semantic-diagnosis).

The **Semantics** tab shows a diagnosis for each Object and Interaction domain in the selected **Composed** or **Module Only** scope. The existing **SSI_n** card also displays the propensity score **Ψ**, its colored region badge, the driving component, and the concrete class population **P_c**. Use its information button to expand the explanation or copy it.

| Region | Propensity |
| --- | --- |
| Balanced | Ψ < 0.30 |
| Transitional | 0.30 ≤ Ψ < 0.60 |
| High | 0.60 ≤ Ψ < 0.95 |
| Critical | Ψ ≥ 0.95 |

For objects, the larger of the **anemia** and **saturation** components determines Ψ. Interactions use saturation only; lightweight interactions do not receive an anemia penalty. Region assignment uses the unrounded score; hover over the score or badge for details.

**P_c** counts concrete classes with active sharing semantics. Below 10, the card shows **Gated** and suppresses the diagnosis; the raw SSI_n remains informational. An empty population or undefined SSI_n shows **Unavailable**. Neither state means Balanced.

![Dashboard cards showing High anemia, Balanced saturation-only, Gated, Unavailable, High concentration, and a neutral descriptive card](images/semantic-diagnosis-cards.png)

*The shared dashboard card template with representative inputs, including the Restaurant object-domain example: SSI_n ≈ 2.83, Ψ = 0.813, High anemia, P_c = 39.*

Raw **SSI_n** remains at the top of the same card; the companion **CV_p** card shows the dispersion band, the largest class weight, and P_c. The two are assessed independently. The dashboard guide explains the card colors, the joint saturation × dispersion view, calibration sensitivity, payload drill-down, and the structure × payload lens: see [FOM Dashboard](Dashboard.md#semantic-diagnosis).

### Propensity curves

The previous color band is replaced by two curves: **Anemia (OC only)** and **Saturation (OC / IC)**. The blue circle marks the OC score on its larger component; the orange triangle marks the IC saturation score. A low-SSI interaction therefore remains in the Balanced region, even when anemia is high for objects at the same SSI.

The vertical axis is Ψ, with the four region bands. The horizontal axis uses a log₂ scale so small and large SSI values remain readable; zero has a separate lane. Reference anchors 4 and 256 correspond to Ψ = 0.5; critical points 2 and 512 correspond to Ψ = 0.95 on the respective curves. The axis expands to include values above 512. Hollow markers indicate gated populations: their positions are informational, not assigned diagnoses. Undefined indices have no marker.

### Payload concentration

Each domain shows the five classes with the largest W_i, followed by **Others** when more classes exist. Bars show individual shares of the domain's total semantic weight; the connecting line and **Σ** labels show cumulative shares. The summary identifies the largest contributor and the combined top-five share. Hover over a row for its full name and values.

The denominator includes the entire concrete class population, not just the visible five. Inherited attributes/parameters and the reference update-volatility weights are included consistently with CV_p. These are **design-time semantic-weight shares**, not observed runtime traffic or workload. Empty or zero-total populations have no percentage chart.

![Reference propensity curves and an illustrative payload Pareto chart](images/semantic-curves-pareto.png)

*Actual WPF rendering. Curve inputs illustrate OC SSI_n ≈ 2.83 and IC SSI_n = 0.18. Pareto weights are illustrative test inputs, not measured Restaurant results.*

### Resolution health

The module card shows one green badge when the composed analysis resolves cleanly, or one badge per issue: lenient composition, unresolved datatype references, missing dependencies, or dependencies outside the scope. **Dependencies and composition** lists the modules in composition order, direct and transitive, with missing ones marked. See [Resolution health and analysis scope](Dashboard.md#resolution-health-and-analysis-scope).

**Lenient fallback does not establish strict composition conformance.** Missing dependencies and unresolved types can affect semantic weights; a zero unresolved-reference count alone does not prove a complete dependency closure. The check covers declared dependencies only. In Module Only scope, the strip states that dependency closure was not applied and lists available dependencies outside the selected scope.

---

## 1. Identification Table

The `Identification` table is used for module-level descriptive metadata.

Typical `Information` copy-table shape:

| Field | Value |
| --- | --- |
| Name | module name |
| Type | `FOM` / `SOM` |
| Version | semantic or project version |
| Modification Date | last model date |
| Security Classification | classification text |
| Copyright | copyright text |
| Application Domain | domain label |
| Use Limitation | limitation text |
| Purpose | purpose text |
| Description | description text |
| Other | free text |

### Typical Use

Use this table when you are documenting the module itself rather than editing class structure.

### Chat Sample Example

`ChatSom.xml` example:

| Field | Value |
| --- | --- |
| Name | `ChatSom` |
| Type | `SOM` |
| Version | `0.4.4` |
| Modification Date | `2026-04-24` |
| Security Classification | `Unclassified` |
| Application Domain | `HLA General` |
| Purpose | `Chat Federation is a sample project for SimGe.` |
| Description | `Chat Federation is a sample project for SimGe.` |
| Use Limitation | `NA` |

The same area also includes one keyword row, one POC row, and one reference row in their own sub-tables.

### Add Behavior

The generic OME `Add` button is meaningful here only for the `POC` sub-view.

---

## 2. Objects Table

The `Objects` table is the hierarchy view for `Object Classes (OC)`.

Typical copy-table shape:

| Name | Sharing | Semantics |
| --- | --- | --- |
| object class name | publish/subscribe mode | class semantics |

### Object Class Editor

Opening an object class from the `Objects` table shows the `Object Class` editor.

The editor covers parent, sharing, declared dimensions, directed interactions, semantics, notes, and the embedded `Attributes` list for attributes declared on that class. In a SOM it also has an `Instances` tab that declares the object instances the federate registers for the class; see [Object Instance Registry](ObjectInstanceRegistry.md).

### Recommended Use

Use the `Objects` table and `Object Class` editor when working on one object class in context.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Sharing | Semantics |
| --- | --- | --- |
| `HLAobjectRoot` | `Neither` | root object class |
| `User` | `PublishSubscribe` | user entity for chat participation |
| `Poll` | `PublishSubscribe` | poll state object |
| `ChatGroup` | `PublishSubscribe` | chat room / group state object |

Open `ChatGroup` when you want to review one concrete object class with several declared attributes and a different transportation policy than the default `User` / `Poll` branch.

---

## 3. Interactions Table

The `Interactions` table is the hierarchy view for `Interaction Classes (IC)`.

Typical copy-table shape:

| Name | Transportation | Order | Sharing | Semantics |
| --- | --- | --- | --- | --- |
| interaction class name | transportation | order type | publish/subscribe mode | semantics |

### Interaction Class Editor

Opening an interaction class from the `Interactions` table shows the `Interaction Class` editor.

The editor covers parent, transportation, order, sharing, declared dimensions, semantics, notes, and the embedded `Parameters` list for parameters declared on that class.

### Recommended Use

Use the `Interactions` table and `Interaction Class` editor when working on one interaction class in context.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Transportation | Order | Sharing | Notes |
| --- | --- | --- | --- | --- |
| `HLAinteractionRoot` | `HLAreliable` | `TimeStamp` | `Neither` | root interaction class |
| `CastVote` | `HLAreliable` | `Receive` | `PublishSubscribe` | poll vote payload |
| `ChatMessage` | `HLAreliable` | `Receive` | `PublishSubscribe` | uses dimension `Group` |
| `GroupManagement` | `HLAbestEffort` | `Receive` | `Neither` | parent for group control interactions |
| `GroupManagement.JoinGroup` | `HLAbestEffort` | `Receive` | `PublishSubscribe` | join request |
| `GroupManagement.LeaveGroup` | `HLAbestEffort` | `Receive` | `PublishSubscribe` | leave request |

`ChatMessage` is a good example for an interaction class that also uses a declared dimension.

---

## 4. Directed Interactions Table

The `Directed Interactions` table shows directed interaction relationships defined on object classes.

Typical use:

- inspect which interaction classes are associated with which object classes
- review directed-interaction coverage while designing object behavior

Typical review shape:

| Object Class | Directed Interaction |
| --- | --- |
| class name | referenced interaction class |

### Chat Sample Example

`ChatSom.xml` example:

| Object Class | Directed Interaction |
| --- | --- |
| `User` | `HLAinteractionRoot.ChatMessage` |

---

## 5. Attributes Table

The `Attributes` table is the flat property table for object-class attributes across the active module.

Typical copy-table shape:

| Name           | Data Type | Update Type             | Ownership      | Transportation | Semantics |
| -------------- | --------- | ----------------------- | -------------- | -------------- | --------- |
| attribute name | datatype  | static/conditional/etc. | ownership mode | transportation | semantics |

### Attribute Editor

When an attribute editor is opened directly from the `Attributes` table:

- the current parent object class is shown
- parent reassignment is allowed
- the change is applied when the editor is confirmed with `OK`

### Recommended Use

Use the `Attributes` table when the task is cross-cutting:

- cleanup across many classes
- compare similar attributes
- reparent an attribute

### Chat Sample Example

`ChatSom.xml` example:

| Name | Parent | Data Type | Update Type | Ownership | Sharing | Transportation | Order |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `Status` | `User` | `StatusTypes` | `Static` | `NoTransfer` | `PublishSubscribe` | `HLAreliable` | `Receive` |
| `NickName` | `User` | `HLAASCIIstring` | `Static` | `NoTransfer` | `Neither` | `HLAreliable` | `Receive` |
| `PollId` | `Poll` | `HLAunicodeString` | `Static` | `NoTransfer` | `PublishSubscribe` | `HLAreliable` | `Receive` |
| `Question` | `Poll` | `HLAunicodeString` | `Static` | `NoTransfer` | `PublishSubscribe` | `HLAreliable` | `Receive` |
| `Options` | `Poll` | `HLAunicodeString` | `Static` | `NoTransfer` | `PublishSubscribe` | `HLAreliable` | `Receive` |
| `Status` | `Poll` | `PollStatus` | `Conditional` | `NoTransfer` | `PublishSubscribe` | `HLAreliable` | `Receive` |
| `Results` | `Poll` | `HLAunicodeString` | `Conditional` | `NoTransfer` | `PublishSubscribe` | `HLAreliable` | `Receive` |
| `GroupIndex` | `ChatGroup` | `integer32BE` | `Static` | `NoTransfer` | `PublishSubscribe` | `HLAbestEffort` | `Receive` |
| `GroupName` | `ChatGroup` | `HLAunicodeString` | `Static` | `NoTransfer` | `PublishSubscribe` | `HLAbestEffort` | `Receive` |
| `AdminNickName` | `ChatGroup` | `HLAunicodeString` | `Conditional` | `DivestAcquire` | `PublishSubscribe` | `HLAbestEffort` | `Receive` |
| `MemberCount` | `ChatGroup` | `integer32BE` | `Conditional` | `NoTransfer` | `PublishSubscribe` | `HLAbestEffort` | `Receive` |

This table is a good place to compare datatype and transportation choices across `User`, `Poll`, and `ChatGroup`.

For modular FOMs, the **Data Type** column also shows dependency-defined type names. For example,
`NETN-CBRN.ProcessingTime` displays `TimeSecondInteger32` even though that datatype is declared in
`RPR-Base`. OME keeps the active module independent and displays its preserved XML reference; the
live datatype object is resolved when the complete module dependency closure is composed.

#### Data-Type Selection Dropdown (ComboBox UX)

When selecting a data type inside OME editors (such as attribute, parameter, array, fixed-record field, variant discriminant/alternative, or dimension editors), use the searchable dropdown:

- **Filter as you type**: Type part of a name to filter local and resolved dependency-owned types allowed by that editor. Matches are ranked exact name first, then prefix, then substring; matching text is highlighted.
- **Select a result**: Pick an entry or press **Enter** to accept the top-ranked match. The search text is not automatically replaced while you type, so `LocationStruct` can be selected without being expanded to `LocationStructArray`.
- **No results**: **No matching data type** means that no permitted type in the current catalog matches the search. Check the spelling and whether the defining dependency module is loaded.
- **Categorical Grouping**: Data types are automatically grouped by their kind (e.g., Simple, Enumerated, FixedRecord, VariantRecord, Array) with clear visual headers separating each group.
- **Visual Badges & Color Indicators**: Each type is prefixed with a colored indicator dot corresponding to its category, along with a category label badge.
- **Module Attribution Badge**: If the data type is dependency-owned (e.g., imported from a parent module like `NETN-BASE`), a distinct module name badge is displayed next to the name to help differentiate local types from dependency types.

### Case Study: Group Administration Ownership Transfer

For a concrete scenario utilizing attribute ownership transfer, consider the **Group Administration** coordination pattern where the administration role is managed dynamically across chat participants:

- **Object Model Elements**:
  - **Object Class**: `HLAobjectRoot.ChatGroup`
  - **Ownership Attribute**: `AdminNickName` with ownership mode set to **DivestAcquire** (Conditional)

- **Ownership Transfer Workflow**:
  1. **Initial Ownership**: The creator of a group registers the `ChatGroup` object and becomes the initial owner of the `AdminNickName` attribute, gaining administrative privileges (e.g. muting users, deleting messages).
  2. **Negotiated Divestiture**: When the current admin decides to hand over administrative duties to another member, they initiate a divestiture request via `NegotiatedAttributeOwnershipDivestiture`.
  3. **Acquisition Request**: The target user requests acquisition of the attribute via `AttributeOwnershipAcquisition`.
  4. **Handover & Notification**: The RTI coordinates the handover. Once confirmed, the old admin loses ownership, and the new admin is notified via the `OwnershipAcquisitionNotification` callback. The application UI dynamically enables administrative tools only for the new owner.

---

## 6. Parameters Table

The `Parameters` table is the flat property table for interaction-class parameters across the active module.

Typical copy-table shape:

| Name | Data Type | Semantics |
| --- | --- | --- |
| parameter name | datatype | semantics |

### Parameter Editor

When a parameter editor is opened directly from the `Parameters` table:

- the current parent interaction class is shown
- parent reassignment is allowed
- the change is applied when the editor is confirmed with `OK`

### Recommended Use

Use the `Parameters` table when the task spans multiple interaction classes or requires parameter reparenting.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Parent | Data Type |
| --- | --- | --- |
| `PollId` | `CastVote` | `HLAunicodeString` |
| `OptionIndex` | `CastVote` | `HLAunicodeString` |
| `VoterNickName` | `CastVote` | `HLAunicodeString` |
| `Sender` | `ChatMessage` | `HLAASCIIstring` |
| `Content` | `ChatMessage` | `HLAASCIIstring` |
| `TimeStamp` | `ChatMessage` | `DateTime` |
| `NickName` | `GroupManagement` | `HLAunicodeString` |
| `GroupIndex` | `GroupManagement` | `integer32BE` |

Use the flat `Parameters` table when you want to compare how `ChatMessage` and `CastVote` carry different kinds of payload.

As with attributes, a parameter whose datatype is declared by another FOM module displays the
preserved datatype name instead of `NA` or an empty value.

---

## 7. Dimensions Table

The `Dimensions` table is used for dimension definitions referenced by classes and properties.

Typical copy-table shape:

| Name | Data Type | Value | Semantics |
| --- | --- | --- | --- |
| dimension name | datatype | policy/value | semantics |

Use this table when maintaining routing or dimensional metadata shared across the model.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Data Type | Value | Notes |
| --- | --- | --- | --- |
| `Group` | `integer32BE` | `Excluded` | upper bound `1024`, used by `ChatMessage` for group isolation |

---

## 8. Time Representations Table

The `Time Representations` table shows the module's time representation entries.

Typical copy-table shape:

| Name | Data Type | Semantics |
| --- | --- | --- |
| logical time row | datatype | semantics |

### Add Behavior

The generic OME `Add` button is intentionally disabled in this table.

This is expected behavior. New entries are not created from the generic top-level add path in this view, and the toolbar button should appear visibly disabled while this table is active.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Data Type | Semantics |
| --- | --- | --- |
| `LogicalTime` | `HLAfloat64Time` | standardized float HLA time type |
| `LogicalTimeInterval` | `HLAfloat64Time` | standardized float HLA time interval |

---

## 9. Tags Table

The `Tags` table shows user-supplied tag entries.

Typical copy-table shape:

| Name | Data Type | Semantics |
| --- | --- | --- |
| tag name | datatype | semantics |

### Add Behavior

The generic OME `Add` button is intentionally disabled in this table.

This is expected behavior. The table supports inspection and editing, but not generic top-level creation from the main toolbar, and the toolbar button should appear visibly disabled while this table is active.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Data Type | Notes |
| --- | --- | --- |
| `sendReceiveTag` | `QoS` | user-supplied runtime metadata for `SendInteraction` / `ReceiveInteraction` |

---

## 10. Synchronizations Table

The `Synchronizations` table is used for synchronization point definitions.

Typical copy-table shape:

| Name | Capability | Semantics |
| --- | --- | --- |
| sync label | capability | semantics |

### Synchronization Editor

The synchronization editor is used for:

- label
- tag datatype
- capability
- semantics
- notes

#### Label Naming Rules

Synchronization point labels use SimGe's common element-name checks (see [Naming Rules](Validation.md#names-while-editing)):
- **Case-Sensitive Uniqueness**: Sibling labels must be unique within the module.
- **Valid Characters**: Labels must start with a letter (`A-Z`, `a-z`) or underscore (`_`), followed by letters, digits (`0-9`), underscores, or hyphens (`-`).
- **Reserved Prefix**: User-defined labels must not start with the reserved prefix `"HLA"` (case-insensitive).
- **Reserved Value**: Labels must not be exactly the sentinel value `"NA"` (case-insensitive).

Use this table when defining federation-level synchronization coordination data.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Data Type | Capability | Notes |
| --- | --- | --- | --- |
| `poll` | `HLAunicodeString` | `RegisterAchieve` | label format `poll-{pollId}` |

Its semantics describe a poll lifecycle label format such as `poll-{pollId}`.

### Case Study: Distributed Polling Coordination

For a concrete scenario using synchronization points, consider the **Distributed Polling** coordination pattern where a participant initiates a poll and synchronizes votes across the federation:

- **Object Model Elements**:
  - **Object Class**: `HLAobjectRoot.Poll` (attributes: `PollId`, `Question`, `Options`, `Status`, `Results`)
  - **Interaction Class**: `HLAinteractionRoot.CastVote` (parameters: `PollId`, `OptionIndex`, `VoterNickName`)
  - **Synchronization Point**: A dynamic synchronization point labeled `poll-{pollId}`

- **Coordination Workflow**:
  1. **Start Poll**: The initiator registers a `Poll` object instance and updates its attributes (`Status = Open`).
  2. **Register Sync Point**: The initiator registers a federation synchronization point with the label `poll-{pollId}` targeting the set of active users.
  3. **Vote & Achieve**: Participants discover the `Poll` and receive the synchronization point announcement. They submit their vote via `CastVote` interactions, and immediately call `SynchronizationPointAchieved` for the label `poll-{pollId}` to signal completion.
  4. **Federation Synchronized**: Once all members have voted/achieved, the RTI triggers the `FederationSynchronized` callback on the initiator.
  5. **Freeze & Clean up**: The initiator closes the poll (`Status = Closed`), updates final results, and deletes the `Poll` object instance, resetting the state for subsequent polls.

---

## 11. Transportations Table

The `Transportations` table manages transportation definitions used by interaction classes and attributes.

Typical copy-table shape:

| Name | Reliability | Semantics |
| --- | --- | --- |
| transportation name | yes/no | semantics |

Use this table when maintaining the transport definitions referenced elsewhere in the model.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Reliability | Notes |
| --- | --- | --- |
| `HLAreliable` | `Yes` | `Provide reliable delivery of data in the sense that TCP/IP delivers its data reliably` |
| `HLAbestEffort` | `No` | `Make an effort to deliver data in the sense that UDP provides best-effort delivery` |

You can cross-check here why `User` and `Poll` attributes use `HLAreliable`, while `ChatGroup` attributes and `GroupManagement` interactions use `HLAbestEffort`.

---

## 12. Update Rates Table

The `Update Rates` table manages update-rate definitions.

Typical copy-table shape:

| Name | Rate | Semantics |
| --- | --- | --- |
| update rate name | numeric/rate text | semantics |

Use this table when the model requires explicit reusable update-rate definitions.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Rate | Semantics |
| --- | --- | --- |
| no rows | no rows | valid empty table |

---

## 13. Switches Table

The `Switches` table shows switch definitions.

Typical review shape:

| Switch | State |
| --- | --- |
| switch name | enabled/disabled |

### Add Behavior

The generic OME `Add` button is intentionally disabled in this table.

The toolbar button should appear visibly disabled while this table is active.

### Chat Sample Example

`ChatSom.xml` example:

| Switch | State |
| --- | --- |
| `attributeScopeAdvisory` | enabled |
| `attributeRelevanceAdvisory` | enabled |
| `objectClassRelevanceAdvisory` | enabled |
| `interactionRelevanceAdvisory` | enabled |

---

## 14. Datatypes Table

The `Datatypes` table groups multiple datatype categories under one area.

Covered categories:

| Category                   | Typical copy-table columns                                               |
| -------------------------- | ------------------------------------------------------------------------ |
| basic data representations | `Name`, `Size`, `Interpretation`, `Endian`, `Encoding`                   |
| simple datatypes           | `Name`, `Representation`, `Units`, `Resolution`, `Accuracy`, `Semantics` |
| enumerated datatypes       | `Name`, `Representation`, `Semantics`                                    |
| array datatypes            | `Name`, `Element Type`, `Cardinality`, `Encoding`, `Semantics`           |
| fixed-record datatypes     | `Name`, `Encoding`, `Semantics`                                          |
| variant-record datatypes   | `Name`, `Discriminant`, `Discriminant Type`, `Encoding`, `Semantics`     |
| reference datatypes        | model-dependent reference rows                                           |

Typical use:

- define reusable data foundations before assigning them to attributes, parameters, tags, or synchronization points
- inspect cross-category datatype dependencies

This is one of the most important tables in day-to-day model authoring.

### Editing enumerators and record fields

Open an Enumerated or Fixed Record datatype, then use **Add** or **Edit** in its member list. Double-click and **F2** also open the selected member; **Remove**, **Up**, and **Down** manage membership and order. Enumerators have Name, Value(s), and note selections; record fields have Name, Data Type, Semantics, and notes. Invalid member names, duplicate names, and values shared by different enumerators are reported before confirmation. Array cardinality is also checked for its count/list/range/Dynamic form.

Confirm the item, then confirm the enclosing datatype to apply the changes. Cancelling the enclosing editor discards its staged member and note changes. See [Datatype editing rules](Validation.md#datatypes-and-record-contents) for examples and the scope of these checks.

### Variant records: discriminants and alternatives

A **variant record** represents a value whose payload has different possible forms. It is a discriminated (tagged) union: a selector value identifies which alternative applies. Unlike a fixed record, it does not carry every declared alternative together. The same datatype can be used in attributes, parameters, array elements, or fields of other records.

![[Pasted image 20260924140033.png]]

| Editor term                | Meaning                                                     | Restaurant FOM example |
| -------------------------- | ----------------------------------------------------------- | ---------------------- |
| **Name**                   | Name of the reusable variant datatype                       | `ServerValue`          |
| **Discriminant**           | Name of the selector inside each record value               | `Experience`           |
| **Discriminant Type**      | Enumerated datatype defining the selector's possible values | `ExperienceLevel`      |
| Alternative **Enumerator** | Discriminant value or values selecting this alternative     | `Trainee`              |
| Alternative **Name**       | Name of the payload member selected by that value           | `CoursePassed`         |
| Alternative **Data Type**  | Datatype of that payload member                             | `HLAboolean`           |
| **Encoding**               | Rule for representing the record in exchanged bytes         | `HLAvariantRecord`     |

The discriminant's **name**, **type**, and **value** are different things: `Experience` is the name, `ExperienceLevel` is its type, and `Temporary` is one possible value. The alternative name is separate again: `Temporary` selects the member named `TempAgency`.

#### Restaurant example: ServerValue

The IEEE Restaurant example declares `ExperienceLevel` as an enumerated datatype represented by `HLAinteger32BE`:
![[Pasted image 20260924140132.png]]

| Enumerator | Numeric value | Selected alternative | Alternative datatype |
| --- | ---: | --- | --- |
| `Trainee` | 0 | `CoursePassed` | `HLAboolean` |
| `Apprentice` | 1 | `Rating` | `RateScale` |
| `Journeyman` | 2 | `Rating` | `RateScale` |
| `Senior` | 3 | `Rating` | `RateScale` |
| `Temporary` | 4 | `TempAgency` | `HLAunicodeString` |
| `Master` | 5 | `Rating` | `RateScale` |

The variant itself needs only three alternative rows:

| Enumerator | Name | Data Type |
| --- | --- | --- |
| `Trainee` | `CoursePassed` | `HLAboolean` |
| `Temporary` | `TempAgency` | `HLAunicodeString` |
| `HLAother` | `Rating` | `RateScale` |

`HLAother` covers the discriminant enumerators not explicitly assigned to the other alternatives. It is a special mapping entry, not an additional numeric value in `ExperienceLevel`. In this example it covers `Apprentice`, `Journeyman`, `Senior`, and `Master`. It is allowed at most once with `HLAvariantRecord`; it is not allowed with `HLAextendableVariantRecord`.

`RateScale` is a simple datatype represented by `HLAinteger32BE`. An alternative may also use a composite datatype, such as a fixed record: selecting that alternative then selects the whole nested record, not just one of its fields.

These are illustrative values, not additional datatype declarations:

```text
Experience = Trainee    -> CoursePassed = true
Experience = Temporary  -> TempAgency = "Example Agency"
Experience = Senior     -> Rating = 2
```

In the second value, the discriminant selects `TempAgency`; `CoursePassed` and `Rating` are not part of that value's encoded payload. `HLAvariantRecord` encodes the discriminant and its selected alternative, with alignment padding where required. This is an interchange encoding, not the in-memory layout of a C `union`.

#### Inspecting the example in OME

1. Open the Restaurant FOM module and select **Datatypes**.
2. Open `ExperienceLevel` to inspect its enumerators and numeric values.
3. Open `ServerValue` in the variant-record category. Its **Discriminant** is `Experience`, **Discriminant Type** is `ExperienceLevel`, and **Encoding** is `HLAvariantRecord`.
4. Inspect the three **Alternatives** rows above. Use **Add** or **Edit** to define the enumerator-to-alternative mapping; these rows describe permitted values, not the current state of a running simulation.
5. Confirm the alternative editor, then the enclosing datatype editor with **OK** to apply changes.

In the Restaurant example, the `Server` class uses `ServerValue` for both `Efficiency` and `Cheerfulness`. Each value contains its own discriminant. Both attributes declare **Update Type = Conditional** and **Update Condition = Performance review**. Those settings describe when the attribute is updated; they do not select a variant alternative. The application supplies the discriminant and matching payload. The FOM defines the mapping but does not prescribe how frequently each alternative is used.

Source: IEEE Std 1516.2-2025, Sections 4.5.2, 4.14.8 (Tables 39–40), and 4.14.10.2; `RestaurantFOMmodule-2025.xml` supplied with the standard.

For the corresponding Fora C# types, application updates, and encoder behavior, see [Restaurant FOM: ServerValue variant record](CodeGenerator.md#restaurant-fom-servervalue-variant-record).

### Datatype change impact (delete / rename)

Because a datatype is usually referenced from many places, changing one can affect far more than the type itself. When you delete a datatype, SimGe analyzes the whole model and, if anything still references it, the delete-confirmation dialog reports:

- how many references, across how many classes, will break, and
- a sample of the affected elements (for example `Aircraft.Position (attribute)`, `TrackStruct.location (record field)`, `LocationArray.element (array element)`).

Those referrers do not disappear — they fall back to an unresolved type name, which blocks code generation until you repoint them or restore the type. The warning is informational: you can still confirm the deletion, or cancel and repoint the references first. When nothing references the type, the dialog says it is safe to remove.

Renaming a referenced datatype shows a similar confirmation first. In-module references follow the rename automatically, but references to the old name from *other* (dependent) modules and any already-generated code do not — so you can proceed or cancel with that in view.

You can also inspect this at any time without changing anything: right-click a datatype in the [Project Explorer](ProjectExplorer.md) and choose **Show Impact / Usages** for the same report — the elements that reference it, what a removal/rename would break, and what a representation/encoding change would force to be regenerated.

> The same reference graph powers the **Data-Type Impact** section of the analysis report, which ranks every datatype by how far its change would reach — see [Model Metrics & Reports](MetricsReports.md).

### Chat Sample Example

`ChatSom.xml` reuse map:

| Datatype | Category | Example Use |
| --- | --- | --- |
| `DateTime` | simple | `ChatMessage.TimeStamp` |
| `integer32BE` | simple | dimension `Group`, parameter `GroupIndex` |
| `PollStatus` | enumerated | `Poll.Status` |
| `StatusTypes` | enumerated | `User.Status` |
| `QoS` | enumerated | `sendReceiveTag` |
| `HLAASCIIstring` | array | `User.NickName`, `ChatMessage.Sender`, `ChatMessage.Content` |
| `HLAunicodeString` | array | sync `poll`, `Poll.*`, `GroupManagement.NickName` |
| `SynchPointFederate` | fixed record | synchronization-related support type |

---

## 15. Notes Table

The `Notes` table is the central place for project/module notes stored in the active model.

Typical copy-table shape:

| Name | Semantics |
| --- | --- |
| note name | note text |

Use this table when maintaining textual documentation assets across the model.

### Chat Sample Example

`ChatSom.xml` example:

| Name | Semantics |
| --- | --- |
| `MOM1` | `The value of the Dimension Upper Bound entry for the Federate dimension is RTI implementation dependent.` |

---

## 16. Services Table

The `Services` table is used for service usage declarations.

Typical review shape:

| Section | Service | Used | Callback |
| --- | --- | --- | --- |
| service chapter | service name | yes/no | yes/no |

### Add Behavior

The generic OME `Add` button is intentionally disabled in this table.

The toolbar button should appear visibly disabled while this table is active.

This table is mainly for validation and standards-alignment review.

### Chat Sample Example

`ChatSom.xml` example subset:

| Section | Service | Used | Callback |
| --- | --- | --- | --- |
| `4. Federation Management` | `createFederationExecution` | yes | no |
| `4. Federation Management` | `joinFederationExecution` | yes | no |
| `5. Declaration Management` | `publishInteractionClass` | yes | no |
| `5. Declaration Management` | `subscribeObjectClassAttributes` | yes | no |

---

## Table View vs Editor

Use the two surfaces for different tasks:

- **Table View**
  Best for scanning many rows, sorting, and making small field edits quickly.
- **Editor Dialog**
  Best for structural changes or when several related fields must be reviewed together before commit.

### Typical End-User Pattern

1. browse in `Objects` or `Interactions`
2. open the class editor for structural work
3. use `Attributes` or `Parameters` tables for cross-cutting cleanup
4. use `Datatypes`, `Dimensions`, `Transportations`, or `Update Rates` as supporting definition tables
5. save once the module reaches a stable state

If you are working on one class in isolation, prefer the class editor. If you are comparing many properties across multiple classes, prefer the flat table views.

For the Chat sample, a typical walkthrough is:

1. open `Objects` and inspect `User`, `Poll`, and `ChatGroup`
2. open `Interactions` and review `ChatMessage` and `GroupManagement`
3. switch to `Attributes` to compare `Status`, `NickName`, `MemberCount`, and `Results`
4. switch to `Parameters` to compare `ChatMessage.Content` and `CastVote.OptionIndex`
5. use `Dimensions`, `Tags`, `Synchronizations`, and `Datatypes` as supporting definition tables

---

## Embedded Editors in Class Editors

The embedded `Attributes` list inside the `Object Class` editor and the embedded `Parameters` list inside the `Interaction Class` editor behave more conservatively than the top-level flat tables.

### Parent Reassignment Rule

Those embedded lists are part of the outer class editor's draft state. If nested attribute or parameter editors were allowed to change parent immediately, the nested dialog could mutate the live model even when the outer class editor is later cancelled.

To preserve correct `OK / Cancel` behavior:

- opening an attribute editor from `Object Class -> Attributes` shows the current parent but does not allow changing it
- opening a parameter editor from `Interaction Class -> Parameters` shows the current parent but does not allow changing it

If you need to move an attribute or parameter to another parent class, use the dedicated top-level `Attributes` or `Parameters` table instead of the embedded list.

---

## Save and Cancel Semantics

The commit boundary depends on where the editor is opened:

- **Top-level table item editor**
  `OK` commits that item edit immediately.
- **Object Class / Interaction Class editor**
  `OK` commits the class draft and its embedded local edits together.
- **Cancel**
  Cancels the active editor scope and leaves the model unchanged for that scope.

In practice:

- cancelling an `Object Class` editor discards its class-level draft changes
- cancelling an `Interaction Class` editor discards its class-level draft changes
- cancelling a nested attribute or parameter dialog does not commit changes from that nested dialog

---

## Object Instance Registry

In a SOM module, the last tab of the workspace — after **Diagram Editor** — is the **Object Instance Registry**. It declares which object instances the federate registers, how their names are obtained, and when they are registered, and the code generator turns the declarations into reservation and registration code. FOM modules have no registry. See [Object Instance Registry](ObjectInstanceRegistry.md).

---

## Practical Guidance

- Use `Identification` for module documentation.
- Use `Objects` and `Interactions` for class-structure design.
- Use `Attributes` and `Parameters` for cross-cutting property maintenance.
- Use `Datatypes` before assigning complex properties.
- Use `Synchronizations`, `Transportations`, and `Update Rates` for reusable infrastructure definitions.
- Save the project after a set of structural edits so the shell state returns to `All changes saved`.

---

**Next:** [Diagram Editor](Diagrams.md)

---
Updated October 2, 2026
