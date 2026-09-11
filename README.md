**Background**: This block was originally created to import different formats of V(D)J data, including MIXCR, ImmunoSeq, Cell Ranger, AIRR, and custom CSV/TSV files. 
This block aims to normalize the data into standard clonotype datasets that downstream Platforma blocks can use. 

**Goal**: This block addresses two gaps in importing full length paired sequences into a usable format for downstream analysis.
      1. Importing full length sequences and pairing heavy/light chains by cell barcode/name. This fix will enable users to input a list
      of full length amino acid sequences, along with cell barcode/name and chain information, and reconstruct correct pairing. 
      2. Importing pre-paired full length amino acid sequences. This fix will support an alternate workflow where the data has already
      been paired by an upstream tool, and import the data as is. 

**Key Changes Made:**
