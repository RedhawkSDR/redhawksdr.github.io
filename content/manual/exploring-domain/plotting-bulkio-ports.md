---
title: "Plotting BulkIO Ports"
weight: 40
---

The REDHAWK IDE contains the ability to [plot]({{< relref "manual/ide/editors-and-views/redhawk-plot-view.md" >}}) using the NeXtMidas plotting framework. If the output port uses the BulkIO interface, it can take advantage of this feature and plot a line graph or a falling raster.

To bring up a plot within the IDE:

1.  Make sure that the component is currently in the started state.

2.  Right-click the desired port to plot.

3.  Select either **Plot Port Data** or **Plot Port FFT**.

    A new view is created, and it contains the plot of the port’s output data.

{{% notice tip %}}
The new view has the same name as the source port. To view additional source information, hover the mouse over the title.
{{% /notice %}}
