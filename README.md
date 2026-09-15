**Background**: This block was originally created to import different formats of V(D)J data, including MIXCR, ImmunoSeq, Cell Ranger, AIRR, and custom CSV/TSV files. 
This block aims to normalize the data into standard clonotype datasets that downstream Platforma blocks can use. 

**Goal**: This block addresses two gaps in importing full length paired sequences into a usable format for downstream analysis.

      1. Importing full length sequences and pairing heavy/light chains by cell barcode/name. This fix will enable users to input a list
      of full length amino acid sequences, along with cell barcode/name and chain information, and reconstruct correct pairing. 
      2. Importing pre-paired full length amino acid sequences. This fix will support an alternate workflow where the data has already
      been paired by an upstream tool, and import the data as is. 

**Key Changes Made:**
**1. Simple Paired Format**
      
      1. Created new file called infer-columns-simple-paired.lib.tengo. This file defines the column spec definitions, 
      including mapping user columns to canonical columns Platforma is able to recognize, creating _heavy and _light specs.
      This file also defines synthetic abundance columns such as read_count. 
      2. Created new file called import-simple-paired.tpl.tengo which contains the bulk of the import and join logic. 
      This file renames the user defined columns to canonical names, splits the input data into two data frames heavyDf(IGH) 
      and lightDf (IGL/IGK), and adds suffixes to the remaining column names to distinguish them for heavy and light data. 
      It then performs an inner join on the name column, assuming a 1:1 ratio of unique heavy to light sequences. This file 
      also contains logic to create a synthetic read_count and read_fraction to enable use of the process-bulk.tpl.tengo file. 
      3. Edited the process-bulk-tpl.tengo file to register the simple-paired import template, made abundance output optional, and 
      runs the import and clonotype files. 
            
**2. Pre-Paired Format**
      1. Created a new file called infer-columns-pre-paired.lib.tengo which defines column specs, including columns for heavy
            and light sequences which have pairedRole defined in domain. This file also maps user columns to canonical columns that 
            Platforma is able to recognize. Also creates synthetic read counts which are hidden from resulting table. 
            2. Created new file called import-pre-paired.tpl.tengo which renames columns to canonical IDs, creates a pairedDf using the name
            column as the clonotypeKey. This file also contains logic to create synthetic read_count and read_fraction data. 
            3. Edited the process-bulk-tpl.tengo file to register the pre-paired import template and run the import and clonotype files. 
