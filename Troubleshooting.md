# Troubleshooting Common Issues #

**If your issue is not listed or further information required email gasv@cs.brown.edu**

(1) ERROR: Lmin has been increased beyond >= Lmax

> This error Lmin>=Lmax, indicates an inconsistency in the data and means that our GASV program did not run to completion.

> Most often the "Lmin>=Lmax" error arrises when a BAM file contains data from read libraries (with sufficiently different fragment lengths or read lengths) which were processed as a single library because :

> (i) they were labeled in the BAM file as being the same library

> (ii) BAMToGASV was run with the "-LIBRARY SEPARATED all'' option.


> _**How the Issue Arrises**_

> - The estimates for Lmin/Lmax (the minimum/maximum length) for each read library are computed by BAMToGASV.

> - When GASV clusters discordant fragments we require the minimum fragment length (Lmin) to be at least as large as 2 times the Readlength.  So, if GASV encounters a fragment with 2\*Readlength > Lmin, GASV assumes the estimates provided were wrong and attempts to correct the problem by increasing Lmin to 2\*Readlength.

> -  Of course, we also need Lmin < Lmax, so if GASV increases Lmin beyond Lmax execution halts.

> _**How to Fix the Issue**_

> First, be sure BAMToGASV was not run with the "-LIBRARY SEPARATED all'' option. The default behavior of BAMToGASV should correctly separate libraries. If the error persists, this indicates a discrepancy in the read library in the BAM file itself.

> The simplest way to address this issue is to increase Lmax beyond 2 times the maximum Readlength in the problematic library. This will ensure GASV will run to completion, but is likely to effect the set of SV predictions produced by GASV.

> The best way to address the issue is to figure out the cause of the discrepancy in the data. We suggest separating the read data from this library according to Readlength and then empirically re-estimating Lmin/Lmax.  Discordant fragments from these additional "sub-libraries"  can be clustered with GASV by modifying the gasv.in file. (See GASV Manual for more information.)

(2) BAM File Lacking Pairing Information

> Receiving no output from BAMToGASV, or receiving a warning about not having paired reads in your BAM files, is caused by two primary reasons:

> First, be sure that the reads in your BAM file are genuinely paired. You can do so by examining the FLAG field and running a utility like SAMTools Fixmate. However, if you have generated your BAM file with current mapping software like BWA, this is not likely to be the problem.

> The second, and more common reason you may not appear to have valid paired reads in your BAM file has to do with the naming of reference sequences/chromosomes.

> BAMToGASV assumes that chromosomes have "standard" namings such as chr1, Chr1, chrX, ChrY. If your BAM file has non-standard chromosome naming, then you'll need to supply a "Chromosome Naming" file as specified in the GASV documentation:
> http://gasv.googlecode.com/svn/trunk/doc/GASV_UserGuide.pdf