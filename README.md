designssquare-lib-feature-assets
================================

import/export assets referenced by variable via Feature Module


There may be a times when variable is referencing the FID of the file. When that is the case and you are exporting 
your module, you probably like to also transfer the assset(file) referenced by variable to be exported. This module 
hooks into FEATURES module to provide a way via FEATURE UI select variables and their refernced assets to be exported

REQUIREMENT:
The file referenced by the variable needs to have entry in the file_usage table which 'type' is the name for the variable
