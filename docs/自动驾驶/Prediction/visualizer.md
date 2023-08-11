# The Design of A Visualization Tool for Autonomous Vehicle Developers(TODO)

![demo](images/visualizer/demo.gif)

This is a tool to visualize map and agents for self-driving developers. It's based on two libraries:

- [Dear ImGui](https://github.com/ocornut/imgui)
- [ImPlot](https://github.com/epezent/implot)

It works with `Apollo Cyber`, and support:

- Show hdmap(apollo) lane boundaries and reference line with ids;
- Show agents(ego vehicle and obstacles, extract from apollo channel) with history/shape/futures(planning or prediction trajectory);
- Show real time plot of vlaues(extracted from apollo channel)

## Architecture

The `.png` picture is created by [Draw.io](https://app.diagrams.net/) you can re-edit the pic with that.

![architecture](images/visualizer/arch.png)
