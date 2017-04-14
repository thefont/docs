# BrightScript Profiler

![](../../images/visualization-tool.png)

### Overview

The BrightScript Profiler gathers important metrics such as CPU usage, "wall-clock" time (the real world time for functions to complete), and the number of times functions are called during the execution of your channel. This tool can help you analyze where performance improvements and efficiencies can be made in your channel.

**Sections:**

* [Required manifest entries](#required-manifest-entries)
* [Collecting the data](#collecting-the-data)
* [Processing the data](#processing-the-data)
* [Understanding the data](#understanding-the-data)
* [Using this data](#using-this-data)

---

## Required manifest entries

Two new manifest entries are required to use the profiler:

| Attribute | Type   | Description | Sample manifest entry |
| --------- | ------ | ----------- | --------------------- |
| `bs_prof_enabled` | boolean | enable BrightScript profiling | `bs_prof_enabled=true`
| `bs_prof_sample_ratio` | float | the ratio at which profiling samples are taken | `bs_prof_sample_ratio=1.0`

The `bs_prof_sample_ratio` can be adjusted from `0.001` to `1.0`. A sample ratio of `1.0` is the default and will measure every BrightScript statement. A sample ratio of `1.0` will have some performance impact, but in most cases it won’t affect the usability of your channel and will provide the most accurate data. However,  if your channel is overly sluggish with a ratio of `1.0`, you can reduce the ratio to reduce the profiler’s overhead. A lower sample ratio will provide less accurate data, so it’s recommended that you use the highest ratio that still allows your channel to be usable.

## Collecting the data

Launch the channel as you normally would and run through your test cases. Once you exit the channel, open the page to your Roku device's [Developer Settings](/develop/developer-tools/developer-settings.md) and click on [Utilities](/develop/developer-tools/developer-settings.md#utilities).

![](../../images/profiling-data-utility.png)

After you've run through your channel's test cases, click on `Profiling Data` to generate a `.bsprof` file and a link to download the data from your Roku device.

![](../../images/profiling-data-ready.png)

The `.bsprof` format is unique to Roku to ensure the format is as efficient and small as possible and easy to generate even on low end Roku devices.

## Processing the data

After you've downloaded the `.bsprof` file, the data can be viewed using the BrightScript Profiler Visualization Tool: https://devtools.web.roku.com/profiler/viewer/

![](../../images/visualization-tool.png)

## Understanding the data

The profiling data is divided into 4 main sections: The function (and associated call path which can be expanded), CPU time, wall-clock time, and function call counts. The CPU time and wall-clock time sections are further divided into separate sections for `self`, `callees`, and `total`. `self` refers to the CPU/wall-clock time the function itself consumed whereas `callees` is the amount of time consumed by any functions called by the original function. `total` is the amount of time consumed by the original function (`self`) and any `callee` functions.

**Function call paths**

This section of the profiling data contains the function calls in each thread. For SceneGraph applications, each thread corresponds to either the main BrightScript thread or a single instance of a `<component>`. For example, if you have a Task node that is instantiated multiple times, each instance will appear as a separate thread. The results will be the same for any custom `<component>` in your channel that is instantiated multiple times. The main BrightScript thread (`Thread main`) is also represented as a single thread even though it has no `<component>`.

**CPU time**

The first 3 columns of the visualization tool lists the time consumed by the CPU process (`CPU Self`), any other functions that are called (`CPU Callees`), and the total amount of time consumed (`CPU Self` + `CPU Callees`). **CPU time refers to the number of operations each function takes to complete and this number should be equal on low end and high end Roku devices.**

**Wall-clock time**

The next 3 columns lists the amount of "wall-clock" time for the function, its callees, and the total. **Wall-clock time refers to the real world time that a function takes to complete. This value can vary across different Roku devices.** For example, a function may take an equal number of operations to complete across different Roku devices but low end Roku devices can take more real world time to complete one operation than a high end Roku device.

**Function call counts**

As the name implies, this column lists the number of times they were called when the channel ran with profiling enabled.

## Using this data

Here are a few key points on how to use this data to improve channel performance:

1. **High wall-clock time but low CPU time:** This pattern shows a function is consistenly waiting, whether it be for input or a response from an external source. These functions are best suited for Task nodes so that it doesn't block the main thread.

2. **Complex functions:** Try to simplify functions as much as possible. If a function handles multiple tasks, consider breaking it out into several functions to further isolate how much CPU or wall-clock time is consumed by each task.

3. **Functions that consume a large amount of CPU or wall-clock time:** Try to reduce the number of calls to these functions as much as possible.
