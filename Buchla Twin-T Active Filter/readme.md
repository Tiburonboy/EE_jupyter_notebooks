***Abstract***: The JupyterLab notebooks in this folder are an analysis of a band pass filter using Symbolic Modified Nodal Analysis implemented with Python in a JupyterLab Notebook then rendered into a PDF. The report builds on the work, *A Bandpass Twin-T Active Filter Used in the Buchla 200 Electric Music Box Synthesizer*, by Aaron D. Lanterman. The report continues with related analysis, thoughts and observations about the filter topology. The use of SymPy makes generating the node equations and obtainig analalytic solutions for the node voltages almost efferortless. 

The main notebok is: `Twin-T_Active_Bandpass_Filter.ipynb`

![Schematic for Bandpass Twin-T Active Filter with componets for the 1000 Hz band pass filter.](Bandpass_Twin-T_Active_Filter.png){width=600}

# INTRODUCTION
This report walks through some of the analysis found in [1], which, describes a third order bandpass filter (BPF) employed in the Buchla Model 295 10 Channel Comb Filter, a synthesizer module developed as part of the Buchla 200 Electric Music Box. The BPF described has a unique arrangement of elements not found in the typical Twin-T configuration, which makes this BPF a interesting candiditate to study. I sometimes watch YouTube videos made by [Lantertronics - Aaron Lanterman](https://www.youtube.com/@Lantertronics), the author of [1]. His video, [Analysis of the Buchla 295 10 Channel Comb Filter - a Weird Twin-T Topology](https://www.youtube.com/watch?v=6DCNOUWSGxc&t=3s), describes this filter and some of its characteristics. 

A proceddure implemented in the [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) programming language called [modified nodal analysis](https://en.wikipedia.org/wiki/Modified_nodal_analysis) (MNA) [2] is used to generate the network equations used in this analysis. The MNA procedure provides an algorithmic method for generating a system of independent equations for linear circuit analysis. Once the scematic is drawn and the net list is generated, the network equations and solutions for the node voltages are eaily obtained as shown in this report. The code in the Jupyter notebook can be used as a template to analyize almost any linear circuit. The schematic for the filter, Figure 1, was drawn using [LTSpice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html) and the netlist was exported for the analysis.

This report was written using a [JupyterLab](https://jupyter.org/) notebook and rendered to a PDF document using [Quarto](https://quarto.org/) which is an open-source scientific and technical publishing system. The source code for this paper is available [here](https://github.com/Tiburonboy/EE_jupyter_notebooks/tree/main/Buchla%20Twin-T%20Active%20Filter) and related material is located [here](https://github.com/Tiburonboy/Symbolic-modified-nodal-analysis).

My motivation for writing this report is: a) my own educational purposes, b) to explore the ability of Python to solve circuit analysis problems, and c) to document the procedure for rendering a JupyterLab notebook into a PDF document.

## Buchla History
Buchla Electronic Musical Instruments (BEMI) was founded by Don Buchla in 1963 under the name Buchla & Associates. The company manufactured synthesizers and MIDI controllers. Buchla's modular synthesizers, like the Buchla 100 and 200 series, were innovative in their design and sound creation approach, differing from Moog synthesizers by employing complex oscillators and waveshaping. 

## Related Work
The Python code presented in this report is somewhat unique since Python is open source, free and runs on a variety of platforms. The Python MNA code, [x], is made available under a public domain license and archived in a github [repository](https://github.com/Tiburonboy/Symbolic-modified-nodal-analysis).

There are other symbolic circuit analysis codes available and some of these are described here. Some of these codes are based on commercial software such as MATLAB, [TINA](https://www.tina.com/) and [Maple](https://www.maplesoft.com/).

[SLiCAP](https://analog-electronics.tudelft.nl/SLiCAP.html) is a symbolic linear analysis tool. SLiCAP is now a Python program, but originally it was written in MATLAB.

TINA is an acronym of Toolkit for Interactive Network Analysis. The TINA design suite is a circuit simulator and PCB design software package for analyzing, designing, and real time testing of analog, digital, HDL, MCU, and mixed electronic circuits and their PCB layouts. TINA has some [symbolic analysis capability](https://www.tina.com/symbolic-analysis).

Maple is a mathematical package and there is an application [note](https://www.maplesoft.com/applications/view.aspx?SID=1427) available describing its use in symbolic circuit analysis. The application note presents a method for evaluating, solving and designing a common, but not so simple pulse-mode high-gain transimpedance amplifier or TIA circuit.

[Symbolic Circuit Analysis](https://rodanski.net/ben/work/symbolic/index.htm) is a web page devoted to symbolic circuit analysis.

[SAPWIN](http://www.ewh.ieee.org/soc/es/May2001/12/Begin.htm) is a windows program package for symbolic and numerical simulation of analog circuits.

[Lcapy](https://github.com/mph-/lcapy) is an experimental Python package for teaching linear circuit analysis. It uses SymPy for symbolic mathematics. In [3] there is an overview of Lcapy as well as a survey of symbolic circuit analysis packages.

## Circuit Description
The schematic for the 1000 Hz section of the comb filter is shown in Figure 1. The schematic was drawn using LTSpice and the netlist was exported for this analysis. The schematic has component values for the 1000 Hz section of the comb filter. The input node is labeled node 1 and the output of the Op Amp is labeled node 2. The other nodes are sequencially labeled from 3 to 5. The filter circuit has three resisters, three capacitors and one Op Amp. The component values shown in the schematic, Figure 1, where chosen from [1, Table I], which is the 1000 Hz filter. In [4], the Op Amp is listed as N5556V, which is an operational amplifier manufactured by Signetics. The Op Amp used in the analysis is an ideal Op Amp, which is defined in the Python MNA code as a circuit element of type ```O``` in the circuit netlist.  

The Twin-T filter topology is made up of two "T" shaped networks connected together, which is why it's called "Twin-T". The arrangment of the resistors and capacitors in Figure 1 is a bit different than the more common notch (band stop) Twin-T filter which shows up in on-line when you search for "Twin-T".  

The filter has three capacitors which sugests that the circuit has a thrird order characteristic polynominal. 

## Analysis
The circuit analysis is performed using a JupyterLab notebook running Python code. This report is directly rendered from a JupyterLab notebook and the code cells with results are displayed. The code cells are highlighted in gray and most of the code output are displayed by converting the equations to LaTex and then markdown to make the fonts used by the PDF rendering engine consistent. Some of the code cells have supressed in the PDF output by placing mark down command, ```#| echo: false```, in the first line of the code cell. This was done to improve readability since some of the code repeats. The ```ipynb``` notebook file in the GitHub repository can be examined to view all the code cells, even those hidden by using HTML comment tags.

LTSpice was used to draw the schematic of the filter and generate the netlist. The component values were manually edited to used scientif notation and the Op Amp entry was fixed. The analysis begins with generating the newtork equations from the circuits netlist by calling:

```report, network_df, df2, A, X, Z = SymMNA.smna(example_net_list)```

SymPy is capabled of working through the algebra to solved the system of equations and even find the roots of the numerator and denominator polimominals.

The analysis continues with a reduced complexity version of the filter, as described in [1]. A sensitivity analysis for the filter is performed along with a Monte Carlo and worst case tolerance analysis.

New sections

- Combinations of substitutions
- Investigate range of allowable initial guesses
- design plots

New section. Results, Discussion and Conclusion
