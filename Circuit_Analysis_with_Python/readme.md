# Circuit Analysis with Python
Last update: 26 Aug 2026

Preliminary files uploaded for testing.

This folder contains the source material for the paper titled, _Circuit Analysis with Python_. The JypyterLab notebook, _Circuit_analysis_w_python.ipynb_, is the source material for the technical paper. Once the notebook has been completed, much of the dialog and results will be copied to [Typst](https://typst.app/) for conversion to a PDF. One column of text per page seems to work best for code, figures and equations. 

The subfolder LTSpice contains *.asc and *.png files for the schematics used in the paper. The subfolder Typst contains the the Typst files and the generated pdf. 

The abstract for the paper is copied here:

**_Abstract_**  
This paper presents a procedure to analyze electric circuits which may contain resistors, capacitors, inductors, Op Amps, dependent and independent sources using the Python programming language. The procedure presented in this paper will use Modified Nodal Analysis to generate the network equations. It is shown that the SymPy and NumPy libraries can be used to generate symbolic network equations from a circuit's netlist and solve those equations with almost no effort. The procedure is efficient and less error prone compared to manual calculations.  
