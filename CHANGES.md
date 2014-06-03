=========
Changelog
=========

Version 2.0.0 - 2014-..-..
==========================
Version 2.0.0 of AdapterRemoval is a near complete rewrite, with the goal of
improved safety, increased speed, fixing a number of minor issues with
previous versions of AdapterRemoval, and adding a few new features:

Major changes:
  * Strict validation of input FASTQ records, to ensure that records are well
    formed, that quality scores fall within the expected range given the
    specified format/offset, and more
  * Limited support for Solexa quality scores; these are converted to and
    saved as Phred+33 or Phred+64 encoded scores.
  * Improved handling of asymetric read-pairs, in which the length of the
    mate1 read differs from the length of the mate2 read.
  * Significant improvements in performance, resulting in a ~5x increase in the
    rate of adapter trimming in basic version, and a ~20x increase in the rate
    of adapter trimming in the SSE enabled version (the default).
  * Support for multiple adapter sequences as well as multiple barcode
    sequences; AdapterRemoval will favor the highest scoring alignment,
    favoring longer alignments over shorter alignments with the same score,
    and favoring alignments with the fewest ambigous bases (N) involved if
    the score and length is identical.

Other improvements / bug-fixes:
  * Barcodes may now contain Ns.
  * Fixed underestimation of error-probabilities during sequence collapse.
  * Fixed (futher) underestimation of error-probabilities of bases during
    collapsing, for conflicting base-calls with the same Phred score.
  * Fixed the maximum number of mismatches for alignments in the range of
    6 .. 9 bases always being 1, even if --mm was set to 0.
  * Fixed the maximum number of mismatches for alignments being calculated
    based on the length of the alignment including ambiguous bases (N),
    thereby inflating the number of mismatches allowed for poor alignments.
  * Replaced use of lower bits of rand() calls with random(), as the former
    generates low entropy bits in that range on some (non-Linux) platforms.
  * Fixed well-aligned reads being discarded as due to the minimum-length
    requirement after trimming not being counted as well-aligned, resulting
    in the total number of alignments not matching the total number of reads.
  * Fixed bug in shifts for PE reads, which was causing some alignments to be
    missed for adapter-only sequences.
  * Improved input validation and sanity checks for command-line parameters.

Features removed from v1.x:
  * The use of --file1 and (optionally) --file2 is now required; reads will not
    be read from STDIN, nor written to STDOUT by default. To approximate the
    previous behavior, the following command may be used:
    $ AdapterRemoval --file1 /dev/stdin --output1 /dev/stdout
  * Per-read statistics of adapter / low-quality base trimming using --stats is
    no longer supported.