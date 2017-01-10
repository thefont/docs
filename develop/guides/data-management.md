# Data Management Best Practices

**Sections:**

* [Thread Ownership of Nodes](#thread-ownership-of-nodes)
* [Task to Render Thread Rendezvous](#task-to-render-thread-rendezvous)
* [Data Modeling](#data-modeling)
* [Marshalling Data](#marshalling-data)
* [Use of m.global](#use-of-mglobal)
* [Referencing Subsections of m.global](#referencing-subsections-of-mglobal)
* [Threading and Observer Callbacks](#threading-and-observer-callbacks)
* [Task Initialization](#task-initialization)

---

## Thread Ownership of Nodes

* Each node is owned by a particular thread. That is usually the render thread.

* In particular, instances of RenderableNode (Group) and every component extended from it are owned by the render thread no matter which thread calls `createObject()`. The global node is owned by the render thread. When a node is set as a child or field of a node owned by the render thread, the referenced node becomes owned by the render thread. Any [Task](https://sdkdocs.roku.com/display/sdkdoc/Task) node is owned by the render thread, not the thread spawned by the Task node. Nodes do not transition ownership away from the render thread.

* The only case of non-render thread instance ownership is that of a plain [Node](https://sdkdocs.roku.com/display/sdkdoc/Node) or [ContentNode](https://sdkdocs.roku.com/display/sdkdoc/ContentNode) or a component extending one of those that is created by a Task thread. A node instance in those circumstances will be owned by the Task thread as long as it is not added as a field or a child of a node owned by the render thread.

* This transfer of ownership is recursively performed on all nodes and field referenced by a transferred node, and it is even performed on nodes referenced in AA or array fields.

* The consequence of these rules is that neither the render thread nor any other Task thread has a way to reference a Task-thread-owned node. This is key to optimizing performance by allowing the owning thread to access the node directly without a rendezvous as described below.

## Task to Render Thread Rendezvous

* When a Task thread operates on nodes it owns, it does so directly.

* When a Task thread operates on a render-thread-owned node, it must do so through a rendezvous. A Task thread has no way to reference a node owned by another Task thread, so that is not an issue.

* A rendezvous is a synchronization between the Task thread and the render thread. Under the covers, the Task thread queues a requested operation on the render thread's queue and blocks waiting for completion of the operation. The render thread eventually pulls the requested operation off of its queue and executes it, returning results into references from which the Task thread may access them. The Task thread sees the results of the operation as soon as it unblocks from the rendezvous, thus it appears as a synchronous call to the Task thread. From the Task thread's point of view, both the syntax and the semantics of the call are the same as if the Task thread owned the node itself.

* While rendezvous are designed to be invisible to syntax and semantics, performance is another matter. **A rendezvous is at least an order of magnitude more expensive than a direct access.** For this reason, rendezvous should be used somewhat sparingly, and both the design and the details of channel scripts should reflect this.

* A distinct rendezvous is used on each separate operation invoked by a Task thread on a render-thread-owned node. In particular, consider an expression like x.y.z where x is a node and y and z are node fields. This is evaluated as a succession of getField() calls. If x is owned by the render thread and this expression is evaluated in a Task thread, each dot represents a distinct rendezvous. Simply counting dots in naively organized Task thread scripts can indicate the number of rendezvous and the performance penalties incurred. In well designed Task thread scripts, most dots deliberately avoid rendezvous, and the few rendezvous that are incurred are unavoidable.

## Data Modeling

* In general, data passed to and from fields is passed by copy. This is necessary to support a multi-threaded model that avoids explicit locking.

* In particular, containers like associative arrays and plain arrays get passed to and from fields by deep copy.  This is a complete recursive copy of all of the data in the container, including copies of all the containers in the container.

* Unlike traditional BrightScript, passing large webs of AAs or arrays through SceneGraph Node fields is not efficient and should be avoided.

* On the other hand, using AAs to model small data structures is a reasonable way to consolidate related fields that are usually set or read as a whole. If the AA is being passed between the render thread and a Task thread, this will incur a single rendezvous. Contrast this with a rendezvous for each field access if the separate data elements are modeled as separate fields.

* The exception to the copy rule through fields is each instance of a SceneGraph node. **SceneGraph nodes get passed to and from fields by reference**.  Using a node tree to model complex content means that a single field or child can accept a large change in the data model with one reference change.

* The result of the above considerations is that data that needs to be accessible via nodes and fields falls into two categories:

 * The first category represents small, shallow data structures where each structure instance is usually treated as a single cohesive item. These are reasonable and efficient to model as **AA fields**.
 * The second represents large, deep data webs where copying would be prohibitive. These are reasonable to model as **node trees**.

## Marshalling Data

* Loading large amounts of server data should be done in the background using a Task node. The Task thread should try to keep ownership of the node while updating its fields, and should only pass the node into a field or as a child of a node owned by the render thread after performing all the operations on the node that it must.

* Rendering the content in screen in the channel on the render thread is done by setting the ContentNode of the appropriate SceneGraph UI component.  By the above guidance, this ContentNode should only be set into the UI node's content field once all of the data model is loaded by the Task thread into the ContentNode.  Since each dot operation on a node requires a rendezvous once the node is owned by the render thread, a Task thread should use one dot reference at the end to set the entire ContentNode and its subtree rather than transferring the node and then operating on it with many subsequent dot operations.

* Reading its own node's fields (`m.top.*`) from a Task thread will entail a rendezvous for each reference since the Task node is owned by the render thread.  To get data structures (more than one scalar value for example) into a Task node at a time, it is best to create a single interface field on the Task node with type `assocarray`.  The render thread can then set that interface field with the data it wants to pass. Only one rendezvous is required for the whole copy, versus multiple if there are individual interface fields for each value.

* As a general rule, small amounts of data that form input or status for Task threads can be grouped in AA fields, and large data webs should be returned from the Task thread to the render thread as node trees.

## Use of `m.global`

* Since the global node (`m.global` in a Component) is owned by the SceneGraph render thread, any operations on `m.global` from a Task thread are accomplished via rendezvous.

* If a Task thread creates a node and adds it to `m.global` via a field or as a child (or grandchild, etc.), it is best to have the Task thread perform operations on the node before it is added so that it may do so without rendezvous.

## Referencing Subsections of `m.global`

Given the rendezvous penalties, don't repeatedly reference the same fields in `m.global` to get data subsections. Use temporaries to hold references to successive parts of the tree. For example, assume that you have a large set of channel configuration data stored in `m.global.config`.  This data is a large web with elements (AAs or node trees) for settings, analytics, etc.:

```brightscript
m.global
{
        config
        {
                settings
                {
                }
                analytics
                {
                }
        }
}
```

Getting some or all of this data into a Task node can be done as follows:

```xml
<component name="DataTask" extends="Task">
<interface>
    <field id="settings" ... />
    <field id="analytics" ... />
</interface>
...

function init() as void
    m.config = m.global.config
    m.settings = m.config.settings
    m.analytics= m.config.analytics
end function

...

</component>
```

The Task makes a local copy of the config global data which it then references via that local variable, avoiding a rendezvous and a copy for subsequent references.  Generally speaking a Task should locally copy only what it needs from `m.global`, so there is a design trade-off in grouping data in subtrees versus spreading it all out at the top level.

## Threading and Observer Callbacks

* The observer callback function for setting a particular field is executed in the same thread in which the setting itself occurs, that of the node's owning thread.

* Since the render thread owns any Task node, any callbacks set up on the Task node's fields will be executed by the render thread. If a Task thread wishes to respond to a setting of one of its Task node's fields, it should use the port form of the [`observeField()`](https://sdkdocs.roku.com/display/sdkdoc/ifSGNodeField) call and wait for an [roSGNodeEvent](https://sdkdocs.roku.com/display/sdkdoc/roSGNodeEvent) on that port. Conversely, callback functions set on fields of a node owned by the Task thread are called back on that Task thread as a direct consequence of that Task thread setting the field.

## Task Initialization

* The initialization of a Task does a bit more work and has more nuances than for other node types. The differences are designed to be mostly invisible, much like a rendezvous operation versus a normal operation, but they nevertheless can affect have design impact in some scenarios. The primary point is that the render thread and the Task thread do not share an `m` for this component, even though they both access `m`. This is forced by concurrency consistency.

* First, the `init()` is executed by the Task node's owning thread, the render thread, during creation of the Task node.

* The Component `m` that is created before and modified during `init()` belongs to the render thread at this point. Function callbacks of Task node fields setup during `init()` will execute in the render thread and access this `m`.

* When the Task's control field is set to `RUN`, the render thread's `m` is cloned (deep copied), the render thread gets the clone, and the Task thread gets the original `m`. This little dance ensures that a port created in `init()` is available to the Task thread, as it is the thread that will need to actually access the port.

* In `init()`, the newly created port can be immediately used to observe fields, and fields so observed are guaranteed to generate events that will be received by the Task thread once it is started. Waiting to create a port and observe fields in the Task thread itself admits a race condition where settings of fields from the render thread may occur before the Task thread has had a chance to setup observation.

* If the same instance of a Task node stops and is started again, it will do the dance again: the render thread's `m` will be cloned, the render thread will get the clone, and the Task thread will get the original (now a 1st generation clone). If there is setup that must be done for `m` (like creating a new port), it must be in a field callback of the Task node, executed by the render thread, that can modify `m` appropriately and then set the Task node's control to `RUN`.
