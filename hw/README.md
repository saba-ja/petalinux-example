# PETALINUX Example

From Vivaod TCL console change the directory to the hw folder where the synth.tcl file is and run the following:

```
source ./synth.tcl
```

This should generate the project, synthesize, implement and export the bitstream.

You need to export the XSA using the Vivado menu.

Open the Block design, then from the File > Export > Export Hardware export the XSA and make sure to include the bitstream in the exported file. Save the file in the ../export folder.



