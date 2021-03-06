---
# Please do not delete --- above :)

# ========================================================================
# Automatically edited by GitHub actions, please do not modify     | START
# ==============================================================
report_url: https://github.com/htwangtw/2021_08_Wang_load-confounds

last_changed: 2021-08-26 18:15 UTC

auth_created: htwangtw
# ===============================================================  | END

# Name of the event.
# -----------------
event: 'Brainhack Global 2020 Montreal'

# The title of your report.
# -------------------------
title:  'Load fMRIprep confounds in Python'

# Please SET this url to your GitHub project repo!
# -------------------------------------------------
url: https://github.com/SIMEXP/load_confounds

# Please edit affiliation details. The affiliation
# ids defined here will be used in the author list.
# -------------------------------------------------
affiliations:
- id: criugm
  orgname: 'CRIUGM'
  street: '4565 chemin Queen Mary'
  postcode: 'H3W 1W4'
  city: Montreal
  state: Quebec
  country: Canada
- id: bsms
  orgname: 'Brighton & Sussex Medical School'
  street: 'Trafford Centre'
  postcode: 'BN1 9RX'
  city: Brighton
  state: 'East Sussex'
  country: UK
- id: hu
  orgname: 'Harvard University'
  street: '86 Brattle Street'
  postcode: 02139
  city: Cambridge
  state: MA
  country: USA
- id: mit
  orgname: 'Massachusetts Institute of Technology'
  street: '46-4033 Vassar Street'
  postcode: 02139
  city: Cambridge
  state: MA
  country: USA

# Please list the authors.
# If an author has multiple affiliations, please
# add corref field and indicate the primary affiliation.
# -----------------------------------------------------
author:
- initials: PB
  surname: Bellec
  firstname: Pierre
  email: pierre.bellec@criugm.qc.ca
  affiliation: criugm
- initials: HTW
  surname: Wang
  firstname: Hao-Ting
  email: wang.hao-ting@criugm.qc.ca
  affiliation: criugm, bsms
  corref: criugm
- initials: SLM
  surname: Meisler
  firstname: Steven Lee
  email: smeisler@g.harvard.edu
  affiliation: hu, mit
  corref: hu

# Please write a brief summary of your project.
# This abstract will (only) appear on the webpage.
# -------------------------------------------------
summary: load_confounds is a tool for loading a sensible subset of the fMRI confounds generated with fMRIprep in python (Esteban et al., 2019). The outputs can be directly passes to nilearn NifitMasker for denoising. The aim at Brainhack MTL 2020 is to implement new strategies as well as imporving the existing functions and documentations for a potential Beta release.

# Please add 1 to 3 tags
tags:
  - fmriprep
  - confounds
  - nilearn

# Please comment out the following 5 lines if you have no supplemental material.
# supplemental:
#   - name: Material 1
#     url: https://osf.io
#   - name: Material 2
#     url: https://zenodo.org

coi: None

acknow: The authors would like to thank the organizers and attendees of Brainhack Global Montreal 2020. Especially, we would like to thank Elizabeth DuPre and Chris Markiewicz for discussions on the choice of test data.

contrib: HTW, SLM wrote the software, HTW, SLM wrote the report

# Please comment out the following 4 lines if no reviewer has been assigned to you yet.
# reviewers:
#   - name: Agah
#     surname: Karakuzu
#     gh_handle: agahkarakuzu

# Show/hide the BinderHub (mybinder.org) badge
# Accepted values: true/false (case sensitive)
# -------------------------------------------
binder: false

# Enable/disable hypothes.is
# Accepted values: true/false (case sensitive)
hypothesis: false

# Please do not delete --- below :)
---

# Introduction

fMRIprep \cite{fmriprep:2019} is a popular minimal preprocessing software for functional MRI data.
'Minimal preprocessing' refers to motion correction, field unwarping, normalization, bias field correction, and brain extraction.
Confound regression and smoothing are excluded from the workflow.
Instead, fMRIprep provides users with a large set of potential confound regressors that covers many denoising strategies.
The users have to select the confound regressors for denoising in subsequent analyses.
Loading a sensible subset of confounds is difficult and error-prone for many strategies, such as ICA-AROMA \cite{icaaroma:2015} and CompCor \cite{compcor:2007}.
\code{load\_confounds} can extract confound variables and implement preset strategies for confound selections.
The loaded format is compatible with \code{nilearn} analysis functions such as \code{NiftiMasker} and the GLM modules.
Our aim was to provide a easy and foolproof API for users to perform subsequent denoising of \code{fMRIprep} outputs.

# Progress

At Brianhack Global Montreal 2020, our goal was to prepare the package for a potential Beta release.
To prepare for the release, related issues involved implementing several preset strategies and improving the user experience with better examples and error messages.
Several issues had been identified before Brainhack, and the full discussion can be found under the \code{load\_confounds} \href{https://github.com/SIMEXP/load_confounds/issues?q=is%3Aissue+label%3AbrainhackMTL2020+is%3Aclosed}{GitHub issue page}.
Participants at Brainhack were encouraged to pick up existing issues from the list.
The following issues have been discussed and/or resolved:

## Strategies

We worked on three strategies:
\begin{itemize}
  \item Added ICA-AROMA \cite{icaaroma:2015} (contributed by Hao-Ting Wang)
  \item Added Scrubbing \cite{power:2012}(contributed by Steven Meisler)
  \item Improved the anatomical mask selection for anatomical CompCor \cite{compcor:2007} (contributed by Steven Meisler)
\end{itemize}

### ICA-AROMA

ICA-AROMA is only applicable to \code{fMRIprep} outputs generated with \code{\-\-use\-aroma}.
\code{fMRIprep} produces files with suffix \code{desc\-smoothAROMAnonaggr\_bold} and outputs ICA components in the confounds file.
Using the \code{desc\-smoothAROMAnonaggr\_bold} output is the recommanded way of applying ICA-AROMA and implemented in \code{load\_confounds} as a preset strategy.
When passing regular fMRIprep outputs suffixed with \code{desc\-prepro\_bold}, \code{load\_confounds} retreives the noise independent compoenents for aggressive denoising.

### Scrubbing

\code{load\_confounds} provides both a basic and full approach for scrubbing.
The basic scrubbing approach flags and censors time frames with excessive motion, using thresholds on framewise displacement and standardized DVARS.
For full scrubbing, described in Power et al \cite{power:2012}, after censoring volumes as in the basic approach, the full approach further remove continuous segments containing fewer than 5 volumes.

### Anatomical CompCor

\code{fMRIprep} uses three kinds of anatomical masks to compute CompCor components, WM, CSF and the combination of the two.
Metadata in fMRIPrep's confounds .json file specify the anatomical compartment associated with each component.
The previous iteration of \code{load\_confounds} did not consider the metadata, hence the difference of masks were not taken into account.
In the revised aCompCor approach, \code{load\_confounds} provides selections of anaotomical maps (WM, CSF, and combined) to avoid regressing relevant signal twice, which may introduce noise.

## Demo

An executable demo using the nilearn developmental fMRI dataset (\href{https://openneuro.org/datasets/ds000228}{OpenNeuro ds000228}) was added (contributed by Michael W. Weiss).
The demo was adapted from an exisitng nilearn example on denoising \href{https://nilearn.github.io/auto_examples/03_connectivity/plot_signal_extraction.html#sphx-glr-auto-examples-03-connectivity-plot-signal-extraction-py}{'Extracting signals from a brain parcellation'}.
This example notebook shows how to extract signals from a brain parcellation and compute a correlation matrix, using different denoising strategies from the package.

## Error message

A new class \code{NotFoundConfoundException} was added for collecting all the missing parameters needed for the given noise component(s).
A final error would be raised with the list of all parameters missing, rather than just the first encountered missing parameter. (Contributed by Michael W. Weiss and Fran??ois Paugam)

## Identify test dataset

\href{https://openneuro.org/datasets/ds000003}{OpenNeuro ds000003} was adopted as the new test data, including all ICA-AROMA related components.
However, non-steady-state volume and motion estmation mertic RMSD \cite{jenkinson:2002} are still missing.
We plan on preprocessing the nilearn demo dataset (\href{https://openneuro.org/datasets/ds000228}{OpenNeuro ds000228}) with the latest stable fMRIprep LTS release for collecting all possible confounds in the future.
(Discussions amongs Pierre Bellec, Hao-Ting Wang, Elizabeth DuPre, and Chris Markiewicz)

## All contributor bot

\code{allcontributors} bot is added to track community contributions (Contributed by Pierre Bellec).

## Add \code{load\_confounds} to \code{nixtract}

In addition to the main package, there was a collaborative project with the developers of \href{https://github.com/danjgale/nixtract}{\code{nixtract}}.
\code{nixtract} is a tool that extract and process timeseries data from neuroimaging files.
Annabelle Harvey and Dan Gale added \code{load\_confounds} as a dependency of \code{nixtract} for reading fMRIPrep confound variables.

# Results

\code{load\_confounds} has now covered most of the strategies tested in Ciric et al. \cite{ciric:2017}.
The following noise components and a set of paramaters for dedicated approaches are supported.
\begin{itemize}
  \item \code{motion}: motion parameters, including 6 translation/rotation (\code{basic}), and optionally derivatives, squares, and squared derivatives (\code{full}).
  \item \code{high\_pass}: basis of discrete cosines covering slow time drift frequency band.
  \item \code{wm\_csf} the average signal of white matter and cerebrospinal fluid masks (\code{basic}), and optionally derivatives, squares, and squared derivatives (\code{full}).
  \item \code{global}: the global signal (\code{basic}), and optionally derivatives, squares, and squared derivatives (\code{full}).
  \item \code{compcor} \cite{compcor:2007}: the results of a PCA applied on a mask based on either anatomy (\code{anat}) or temporal variance (\code{temp}). For aCompCor, one can choose either separate or combined WM and CSF compartments, as well as choosing to extract components contributing 50\% of variance or a set number of components (typically 5).
  \item \code{ica\_aroma} \cite{icaaroma:2015}: the results of an idependent component analysis (ICA) followed by identification of noise components. This can be implementing by incorporating ICA regressors (\code{basic}) or directly loading a denoised file generated by fMRIprep (\code{full}).
  \item \code{scrub} \cite{power:2012}: regressors coding for time frames with excessive motion, using thresholds on framewise displacement and standardized DVARS (\code{basic}) and suppressing short time windows using the (Power et al., 2014) appreach (\code{full}).
\end{itemize}

\code{load\_confounds} can produce the following strategies from Ciric et al. \cite{ciric:2017}. The following table highlights the relevant options:

\begin{tabular}{ | l | c | c | c |  c |  c |  c |  c |  }
  \hline
  Strategy             & \code{high\_pass} & \code{motion} & \code{wm\_csf} & \code{global} & \code{compcor} & \code{ica\_aroma} & \code{scrub} \\
  \hline
  \code{Params2}       & x                &               & \code{basic}  &               &                &                  &              \\
  \code{Params6}       & x                & \code{basic}  &               &               &                &                  &              \\
  \code{Params9}       & x                & \code{basic}  & \code{basic}  & \code{basic}  &                &                  & \code{full}  \\
  \code{Params9scrub}  & x                & \code{basic}  & \code{basic}  &               &                &                  &              \\
  \code{Params24}      & x                & \code{full}   &               &               &                &                  &              \\
  \code{Params36}      & x                & \code{full}   & \code{full}   & \code{full}   &                &                  & \code{full}  \\
  \code{Params36scrub} & x                & \code{full}   & \code{full}   &               &                &                  &              \\
  \code{AnatCompCor}   & x                & \code{full}   &               &               & \code{anat}    &                  &              \\
  \code{TempCompCor}   & x                &               &               &               & \code{temp}    &                  &              \\
  \code{ICAAROMA}      & x                &               & \code{basic}  &               &                & \code{full}      &              \\
  \code{AROMAGSR}      & x                &               & \code{basic}  & \code{basic}  &                & \code{full}      &              \\
  \code{AggrICAAROMA}  & x                &               & \code{basic}  & \code{basic}  &                & \code{basic}     &              \\
  \hline
\end{tabular}

\smallskip
The future direction is to integrate \code{load\_confounds} in \code{nilearn} for better reach to a wider range of users who may benifit from the package.
To facilitate the nilearn inegration, we will add \code{sample\_mask} to support volume-sensoring based scrubbing and improve the \code{nilearn} \code{NiftiMasker} related feature.
