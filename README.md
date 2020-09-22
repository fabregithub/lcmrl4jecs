
<!-- README.md is generated from README.Rmd. Please edit that file -->

lcmrl4jecs
==========

This package is to implement the LCMRL code developed by Dr Steven C.
Wendelken at US EPA into a package. This is not intended for
distribution.

<!-- badges: start -->
<!-- badges: end -->

Installation
------------

You can install the development version of lcmrl4jecs from
[GitHub](https://github.com/) with:

    # install.packages("devtools")
    devtools::install_github("fabregithub/lcmrl4jecs")

Instructions for Calculating LCMRLs using RStudio
=================================================

Introduction
------------

This document describes the procedure for calculating the Lowest
Concentration Minimum Reporting Level (LCMRL) using the free software R
and RStudio.

Definitions
===========

Working Directory
-----------------

The directory where the LCMRL code, LCMRL input files, and LCMRL output
files reside.

Input File for LCMRL Data
-------------------------

The input file is a Microsoft Excel comma separated values (.csv) file
containing the results of the LRB and LFB samples for a partial or
completed LCMRL procedure. These data must be arranged in a specific
format within the .csv file that is compatible with the LCMRL programme.

Laboratory Fortified Blank (LFB)
--------------------------------

The laboratory fortified blank (LFB) is an aliquot of reagent water,
containing method preservatives, to which known quantities of the method
analytes are added. The concentration of the analytes in an LFB is
called a “spiking level” in the terminology of the LCMRL procedure.

Laboratory Reagent Blank (LRB)
------------------------------

The laboratory reagent blank (LRB) is an aliquot of reagent water
containing the method preservatives. LRBs are used to collect data for
the required “zero spiking level” in the LCMRL procedure.

Lowest Concentration Minimum Reporting Level (LCMRL)
----------------------------------------------------

The single-laboratory LCMRL is the lowest spiking concentration such
that the probability of spike recovery in the 50% to 150% range is at
least 99%.1,2

Test Data
---------

There two test datasets included in this package. ‘pfas’ is the dataset
created by the package maintainer. ‘test’ dataset is the one provided by
the US EPA. Dataset can be loaded by (data(pfas)) or (data(test)).

Creating the RStudio Computing Environment
==========================================

Create the Working Directory
----------------------------

You can create a working directory wherever you want as far as you have
a writing privilege or administrative privilege.

From the Session drop down menu in RStudio, select Set Working Directory
&gt; Chose Directory. Browse to the location of the new working
directory, click on the ‘you\_working\_directory’ folder, then click
Open.

Verify Operation
----------------

Follow the steps in Section 4 to calculate LCMRLs for the five analytes
in the test data input file. After processing is complete, open the
working directory, and find the file named “LCMRL.values.R LCMRL
Test.csv. Verify that the LCMRLs in Column B and the messages in Column
G are identical to those listed in Table 1. Ignore the information in
the other columns. Two of the analytes, Analyte 2 and Analyte 4, should
return error messages stating that an additional spiking level is needed
to determine a valid LCMRL. These represent the two possible error
messages for an incomplete LCMRL determination.

For Analyte 2, an estimated LCMRL of 1.3 ng/L is reported with the
message, “Lower spiking level needed to bracket LCMRL”. See Section
5.2.3 for more information on this circumstance. For Analyte 4, the
LCMRL is reported as “0” with the message, “LCMRL is above highest
spiking level”. See Section 5.2.4 for more information on this
circumstance.

Follow the steps in Section 4.6.1 to generate graphs for the test data.
Open the working directory, and find the file named “R LCMRL
Test.LCMRL.Graphs.pdf”. Two graphs for each analyte should appear: the
QC Interval Coverage Plot and the LCMRL Plot. Both graphs for Analyte 2,
should display this error message: “LCMRL is Below Lowest Non-Zero SL”.
For Analyte 4, no graphs will appear in the PDF output file.

Creating the Input File for LCMRL Data
--------------------------------------

The input files are expected to be Microsoft Excel .csv files with a
header in the first row. If the file is composed of multi-analyte data,
then all analytes should be analyzed using the same method: either
including non-negative values or not. The following rules apply:

Header First Row: “Analyte”, “Lab”, “Spike”, “Result”, “Dilution
Factor”, “Units”

| Column | Field Description                                                                   |
|:-------|:------------------------------------------------------------------------------------|
| A      | Analyte name (alphanumeric; no comas should appear in the name)                     |
| B      | Lab name (alphanumeric; no comas should appear in the name)                         |
| C      | Spiking level (numeric); enter “0” for laboratory reagent blanks (LRBs)             |
| D      | Measurement (numeric)                                                               |
| E      | Dilution factor (numeric). Program at this time only expects dilution factors of 1. |
| F      | Units of measurement (alphanumeric with no comas)                                   |

The method analytes and concentration levels can be inserted in any
order.

Running LCMRL Scripts
=====================

For LCMRL data consisting of only positive values, three command lines
are required to calculate LCMRLs and generate the graphs, each followed
by a carriage return:

    fh.labdata <- "datafilename.csv"
    LCMRL.Values(fh.labdata, rnnr = 1)
    LCMRL.Graphs(fh.labdata, rnnr = 1)

For LCMRL data sets that include negative values, three command lines
are required to calculate LCMRLs and generate the graphs, each followed
by a carriage return:

    fh.labdata <- "datafilename.csv"
    LCMRL.Values(fh.labdata, rnnr = 0)
    LCMRL.Graphs(fh.labdata, rnnr = 0)

Procedure for Collecting LCMRL Data
===================================

The LCMRL Procedure requires a minimum of four LFBs at each of seven
concentrations, or “spiking levels”. These LFBs, plus a minimum of four
LRBs, or “zero spiking level”, are processed through the entire method
procedure. All method specified steps, such as sample extraction and
sample preservation, must be included.

The following subsections provide detailed instruction for conducting
the LCMRL study. An overview is provided here.

Calibrate the analytical instrument. Suggested calibration ranges for
the method analytes and concentrations for the internal standards and
surrogates can be found in the EPA method.

Start by selecting five spiking levels that will be used to estimate the
LCMRL for each analyte. Section 5.2 including subsections discuss
criteria useful for determining appropriate concentrations. Make sure
each LFB concentration you select is bracketed by calibration standards
(Sect. 5.1.2). After you complete five of the seven LFB levels,
calculate LCMRLs for the method analytes.

Select at least two more LFB levels. See Section 5.4 for guidelines on
appropriate LFB concentrations. As you complete these additional levels,
update the LCMRL input file with the additional data and calculate
revised LCMRLs. For each analyte, the LCMRL program will display a
message in the output file indicating whether a valid LCMRL has been
determined or if additional spiking levels are required. Follow the
recommendations in Section 5.2.3 (lower level LFB needed) and Section
5.2.4 (higher level LFB needed) to add additional spiking levels until
valid LCMRLs are determined for each analyte.

LCMRL Calibration Range
-----------------------

### Use a Typical Range

The concentration of the highest calibration standard should be
consistent with routine analysis. The instrument calibration range
should be typical of expected routine sample analysis. The use of an
instrument calibration that has an unreasonably narrow range may yield a
lower LCMRL, but the LCMRL will not reliably reflect performance during
routine analysis. The calibration range must not exceed the response
that begins to saturate the detector.

### Range Must Encompass LFB Spiking Levels

The concentration of the LFBs must lie within the range of the
calibration. Spiking levels are invalid if the concentration exceeds the
calibration range at the low or high end.

LFB Concentrations Must Bracket LCMRL
-------------------------------------

The LCMRL must be bracketed by at least one LFB concentration level that
is at, or below, the LCMRL and by at least one LFB level that is at, or
above, the LCMRL. If this is not the case, then at least one more level
of LFBs must be processed to include the LCMRL within the concentration
range of the LFBs. An estimate for a low-level spiking concentration
might be where the mean of replicates is close to the accuracy extremes
of 50 and 150% or where reproducibility begins to break down. When
interferences are present, one might use a low-level estimate of twice
the equivalent concentration of the analyte peak as found in the method
blank. The accuracy and precision for data lower than the LCMRL may be
of poor quality, but it is necessary to find at what concentration data
quality begins to break down.

### Range of LFB Concentrations

It is recommended that the range of LFB concentrations be within about a
factor of twenty of the LCMRL. Spiking levels that far exceed the LCMRL
lose significance when estimating an LCMRL. More than seven levels of
LFBs may be needed to observe this recommendation for multi-analyte
methods.

### Counting LFB Levels

For each analyte, three of the four analyses in each spiking level must
be reported as non-zero results or that level does not count toward the
required seven LFB levels. LRBs do not count as one of the required
seven spiking levels.

### When LCMRL Estimate is Less than the Lowest LFB Concentration

When the calculated LCMRL is lower than the lowest LFB spiking level, an
estimated LCMRL result will appear in the output file with a message:
“Lower spiking level needed to bracket the LCMRL.” This means that the
LCMRL program needs a lower LFB level to calculate a valid LCMRL.
Fortify this additional LFB below the estimated LCMRL while retaining as
much signal to noise as possible. Practically speaking, at least one
spiking level must fail LCMRL QC limits to determine an LCMRL.

If the lower spiking level is below the current calibration range, a
calibration standard must be added at, or below, the concentration of
the added LFB. Run a full calibration with the new standard included.
The additional low-level calibration standard may fail the method QC
limits for calibration, but this calibration level should be allowed so
that the LCMRL calculator can determine an LCMRL. Completed LFB levels
do not have to be re-analyzed.

### When LCMRL Estimate is Greater than the Highest LFB Concentration

When the calculator determines that an LCMRL estimate is higher than the
highest LFB spiking level, an LCMRL result will not appear in the output
file. Instead, this message will appear: “LCMRL is above highest spiking
level”. This means that when the calculator processed the LFB results,
the LCMRL QC probability limits were greater than 50 to 150%, even at
the highest level. If an LCMRL is to be determined, a higher LFB level
must be processed. This assumes that accuracy and precision will improve
at higher spiking levels. ’For example, if the method cannot extract at
least a recovery of 50% at any concentration, an LCMRL defined as
“between 50 and 150% recovery” cannot be determined. At least one
spiking level needs to pass LCMRL QC probability limits to determine an
acceptable LCMRL.

If the higher spiking level exceeds the current calibration range, a
calibration standard must be added at, or above, the concentration of
the added LFB. Run a full calibration with the new standard included.
Completed LFB levels do not have to be re-analyzed.

### Running Out of Response

If the calculated LCMRL is below the lowest LFB concentration and data
cannot be obtained below the LCMRL, then the laboratory should set the
LCMRL the lowest LFB concentration.

Include Laboratory Reagent Blanks in Data Set
---------------------------------------------

At least four LRBs must be included in the dataset. If the LRB does not
have a response, report “0”.

Estimate LCMRL after Five Spiking Levels
----------------------------------------

It is recommended to calculate the LCMRL after the first five spiking
levels have been processed so that the next two LFB concentrations can
be selected based on the estimated LCMRL. A lower or higher
concentration may be needed to ensure the LCMRL is bracketed by LFBs. If
the estimated LCMRL is already bracketed, select additional
concentrations within the existing range, or expand the range at either
end keeping the LFB concentrations as close as possible to the estimated
LCMRL.

Outliers
--------

Extreme data observations, or outliers, can occur. An outlier may
represent the actual lack of reproducibility of the method at that
concentration. Or, an outlier may result from changed conditions that
represent a different population of data. Or, an outlier may due to
analyst error. If the reason for an outlier is known, such as a double
spike, that data should not be used. Otherwise, the data should be used.
Extreme outliers are down-weighted by the LCMRL calculator.

Formatting Conventions for Data
-------------------------------

### Numerical Results

At least three significant figures for each non-zero result should be
reported for LFB and LRB samples and entered into the LCMRL input file.
Otherwise, the variance may be censored, resulting in an artificially
low LCMRL. Because data systems may truncate the number of digits at the
low end of the calibration curve, this requirement may need to be
addressed by selecting a lower set of units for the LCMRL analyses. Note
that numerical results ending in zero are truncated in .csv files. For
example, a result of 4.00 ng/L would appear in the .csv input file
truncated as “4”. This is normal and does not affect the calculation.

### Report No Response as Zero

Instrument software may not report a result when there is not a peak to
integrate. Report such an analysis as “0”. In addition, when a software
system reports “below calibration,” enter “0”.

### Negative Values

At low-level concentration, some instrument software systems will report
negative numbers. Enter negative values into the input file and
calculate LCMRLs using the script for data sets that include negative
numbers (Sect. 4.5).

No Time Requirement
-------------------

The LCMRL procedure does not have a time requirement for LFBs to be
processed and analyzed. The LFBs can be processed all at once or over
time.

Confidence Level and Data Quality Interval
------------------------------------------

A reporting level is defined in terms of level of confidence and data
quality. The 99% confidence level that is used for the prediction
interval is considered conservative, but for drinking water surveys the
quality of the data is important. The data quality interval that was
chosen for the LCMRL, 50 to 150%, was based upon the judgement of
experienced analysts at OGWDW.
